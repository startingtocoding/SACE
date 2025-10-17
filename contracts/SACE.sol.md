// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;

/**
 * @title Synthetic African Currency Exchange (SACE)
 * @author Simon Kapenda
 * @notice BEP-20 Upgradeable token representing a weighted basket of Africa's top 21 currencies
 * @dev Version 2.1 — Full fixes applied for production
 */

import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20PermitUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20BurnableUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/proxy/utils/UUPSUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/security/ReentrancyGuardUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/security/PausableUpgradeable.sol";
import "./interfaces/AggregatorV3Interface.sol";

contract SACE is 
    Initializable,
    ERC20Upgradeable,
    ERC20PermitUpgradeable,
    ERC20BurnableUpgradeable,
    UUPSUpgradeable,
    OwnableUpgradeable,
    ReentrancyGuardUpgradeable,
    PausableUpgradeable
{
    uint256 public constant MAX_SUPPLY = 100_000_000_000 * 10 ** 18;

    address public treasuryWallet;
    uint256 public totalWeight;
    uint256 public currentIndexValue;

    uint256 public constant HEARTBEAT = 3600; // 1 hour
    uint256 public constant MAX_DEVIATION_BASIS_POINTS = 500; // 5% deviation allowed

    /**
     * @notice Initialize the SACE contract
     * @param _treasuryWallet Wallet that will hold the initial supply
     */
    function initialize(address _treasuryWallet) public initializer {
        require(_treasuryWallet != address(0), "Invalid treasury wallet");

        __ERC20_init("Synthetic African Currency Exchange", "SACE");
        __ERC20Permit_init("Synthetic African Currency Exchange");
        __ERC20Burnable_init();
        __Ownable_init();
        __UUPSUpgradeable_init();
        __ReentrancyGuard_init();
        __Pausable_init();

        treasuryWallet = _treasuryWallet;
        _mint(_treasuryWallet, MAX_SUPPLY);
    }

    /**
     * @notice Fetch latest price from Chainlink or fallback
     * @param feed Chainlink feed address
     * @param code Currency code
     * @return normalizedPrice Latest normalized price
     */
    function getLatestPrice(address feed, string memory code) public returns (uint256) {
        require(feed != address(0), "Oracle feed address invalid");

        AggregatorV3Interface priceFeed = AggregatorV3Interface(feed);
        (, int price, , uint256 updatedAt, ) = priceFeed.latestRoundData();

        require(price > 0, "Invalid oracle price");
        require(block.timestamp - updatedAt <= HEARTBEAT, "Stale price feed");

        uint256 normalizedPrice = uint256(price) * 1e10; // normalize to 1e18

        uint256 previousPrice = lastValidPrice[code];
        if (previousPrice != 0) {
            uint256 deviation = normalizedPrice > previousPrice
                ? ((normalizedPrice - previousPrice) * 10000) / previousPrice
                : ((previousPrice - normalizedPrice) * 10000) / previousPrice;

            if (deviation > MAX_DEVIATION_BASIS_POINTS) {
                emit PriceDeviationRejected(code, normalizedPrice, previousPrice, deviation);
                return previousPrice;
            }
        }

        lastValidPrice[code] = normalizedPrice;
        return normalizedPrice;
    }

    /**
     * @notice Add or update currency in basket
     */
    function upsertCurrency(
        string memory code,
        uint256 weight,
        bool active,
        address feed
    ) external onlyOwner nonReentrant whenNotPaused {
        require(bytes(code).length > 0, "Invalid code");
        require(weight > 0 && weight <= 10000, "Invalid weight");

        if (basket[code].weight == 0) {
            currencyCodes.push(code);
        }

    /**
     * @notice Remove a currency from basket
     */
    function removeCurrency(string memory code) external onlyOwner nonReentrant whenNotPaused {
        require(basket[code].weight != 0, "Currency does not exist");
        delete basket[code];

        for (uint256 i = 0; i < currencyCodes.length; i++) {
            if (keccak256(bytes(currencyCodes[i])) == keccak256(bytes(code))) {
                currencyCodes[i] = currencyCodes[currencyCodes.length - 1];
                currencyCodes.pop();
                break;
            }
        }

        emit CurrencyRemoved(code);
        recomputeIndex();
    }

    /**
     * @notice Update rate for a currency
     */
    function updateCurrencyRate(string memory code) public onlyOwner nonReentrant whenNotPaused {
        Currency storage currency = basket[code];
        require(currency.active, "Currency not active");
        require(currency.feed != address(0), "Feed not set");

        currency.rateUSD = getLatestPrice(currency.feed, code);
        currency.lastUpdated = block.timestamp;

        emit CurrencyUpdated(code, currency.rateUSD, currency.weight, currency.active, currency.feed, block.timestamp);
        recomputeIndex();
    }

    /**
     * @notice Batch update rates
     */
    function batchUpdateCurrencyRates(string[] memory codes) external onlyOwner nonReentrant whenNotPaused {
        for (uint256 i = 0; i < codes.length; i++) {
            updateCurrencyRate(codes[i]);
        }
    }

    /**
     * @notice Recompute the synthetic index value
     */
    function recomputeIndex() public whenNotPaused {
        uint256 sumWeightedRates = 0;
        uint256 sumWeights = 0;

        for (uint256 i = 0; i < currencyCodes.length; i++) {
            Currency memory c = basket[currencyCodes[i]];
            if (c.active && c.rateUSD > 0) {
                sumWeightedRates += c.rateUSD * c.weight;
                sumWeights += c.weight;
            }
        }

        if (sumWeights > 0) {
            currentIndexValue = sumWeightedRates / sumWeights;
            totalWeight = sumWeights;
            emit IndexRecomputed(currentIndexValue);
            emit BasketWeightsNormalized(sumWeights);
        }
    }

    // --- Admin Functions ---

    function updateTreasuryWallet(address _newTreasuryWallet) external onlyOwner nonReentrant {
        require(_newTreasuryWallet != address(0), "Invalid address");
        address oldWallet = treasuryWallet;
        treasuryWallet = _newTreasuryWallet;
        emit TreasuryWalletUpdated(oldWallet, _newTreasuryWallet);
    }

    function pause() external onlyOwner {
        _pause();
    }

    function unpause() external onlyOwner {
        _unpause();
    }

    function _authorizeUpgrade(address newImplementation) internal override onlyOwner {}

    // --- View Functions ---
    function getCurrencyCount() external view returns (uint256) {
        return currencyCodes.length;
    }

    function getCurrency(string memory code) external view returns (Currency memory) {
        return basket[code];
    }

    uint256[50] private __gap;
}
