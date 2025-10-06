
# Synthetic African Currency Exchange (SACE)

**Ticker:** SACE  
**Token Type:** BEP-20 Upgradeable  
**Total Supply:** 10,000,000,000 SACE  
**Creator:** Simon Kapenda  
**Version:** 1.7  
**Date:** October 6, 2025  

---

## About

The **Synthetic African Currency Exchange (SACE)** is a BEP-20 token representing a **weighted basket of Africa’s top 21 currencies**. Inspired by benchmarks such as the **U.S. Dollar Index (DXY)**, SACE offers a unified, tradable instrument for tracking and trading Africa’s collective currency strength.  

SACE consolidates Africa’s top 21 currencies into a single tradable instrument, creating a transparent benchmark for the continent’s currency performance. It enhances market efficiency, improves liquidity, enables effective hedging against currency volatility, and facilitates cross-border trade — positioning SACE as a foundational economic tool for Africa’s growing financial ecosystem.  

SACE is a development brand of **Abba Platforms Inc.**, created by **Simon Kapenda**, the creator of **CillarCoin (CILLAR)** — a utility token — and **NADD**, Namibia's first blockchain-native stablecoin pegged 1:1 to the Namibian Dollar.

---

## Purpose

SACE was designed to:  
- Create a **synthetic index** representing Africa’s currency performance.  
- Enable **cross-border trade and exposure** to African forex.  
- Serve as a **transparent benchmark** for African currency movements.  
- Provide traders and investors with a **single tradable instrument**.  

---

## Currency Basket

As of September 25, 2025, SACE includes the following currencies:  

| #  | Currency                                      |
|----|-----------------------------------------------|
| 1  | Tunisian Dinar (TND)                         |
| 2  | Libyan Dinar (LYD)                           |
| 3  | Moroccan Dirham (MAD)                        |
| 4  | Ghanaian Cedi (GHS)                          |
| 5  | Botswana Pula (BWP)                          |
| 6  | Seychelles Rupee (SCR)                       |
| 7  | Eritrean Nakfa (ERN)                         |
| 8  | Namibian Dollar (NAD)                        |
| 9  | Swazi Lilangeni (SZL)                        |
| 10 | South African Rand (ZAR)                     |
| 11 | Lesotho Loti (LSL)                           |
| 12 | São Tomé and Príncipe Dobra (STN)            |
| 13 | Zambian Kwacha (ZMW)                         |
| 14 | Mauritanian Ouguiya (MRU)                    |
| 15 | Egyptian Pound (EGP)                         |
| 16 | Algerian Dinar (DZD)                         |
| 17 | Mauritian Rupee (MUR)                        |
| 18 | West African CFA Franc (XOF)                 |
| 19 | Central African CFA Franc (XAF)              |
| 20 | Kenyan Shilling (KES)                        |
| 21 | Rwandan Franc (RWF)                          |

---

## Technical Overview

SACE uses **OpenZeppelin’s Upgradeable BEP-20 implementation** with the UUPS upgrade pattern and integrates **Chainlink Oracles** for real-time currency rate feeds.  

Key features:  
- **Currency Basket Management** — dynamic add/remove currency logic.  
- **Weighted Index Calculation** — based on currency weights and rates.  
- **Deviation Protection** — rejects outlier price feeds.  
- **Oracle Heartbeat** — ensures feeds are up-to-date.  
- **Upgradeable Contract** — governance controlled updates.  
- **Transparent Events** — logs all updates and changes.  

---

## Economic Analysis

The **Synthetic African Currency Exchange (SACE)** creates a unique economic instrument that aggregates the strength of Africa’s top currencies into a single tradable token. By doing so, SACE addresses key economic challenges:  

- **Market Efficiency:** Simplifies exposure to African currencies, reducing the complexity of multi-currency forex trading.  
- **Liquidity Creation:** Consolidates liquidity for African currencies, making them more accessible to global investors.  
- **Hedging Tool:** Provides a standardized way for investors, businesses, and governments to hedge against currency volatility.  
- **Transparency:** Offers a clear benchmark for Africa’s collective currency performance, promoting informed economic decisions.  
- **Trade Facilitation:** Supports cross-border trade by reducing currency conversion friction and providing a stable reference currency.  

In effect, SACE functions as Africa’s **currency performance index**, much like the DXY for the U.S. dollar, and has the potential to become a core instrument in the continent’s financial ecosystem.  

---

## Getting Started

### Prerequisites
- [Node.js](https://nodejs.org/) v18+
- [Hardhat](https://hardhat.org/)  
- [Yarn](https://yarnpkg.com/) or npm  
- [MetaMask](https://metamask.io/) or similar wallet  
- Binance Smart Chain (BSC) testnet/mainnet access  

### Installation
```bash
git clone https://github.com/yourusername/SACE.git
cd SACE
yarn install
```

### Compile Contract
```bash
npx hardhat compile
```

### Deploy Contract (Testnet Example)
```bash
npx hardhat run scripts/deploy.js --network bsctestnet
```

### Run Tests
```bash
npx hardhat test
```

---

## Whitepaper

For the full technical and economic details, see the [SACE Whitepaper](./SACE_Whitepaper.md).  

---

## License

This repository is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.  
