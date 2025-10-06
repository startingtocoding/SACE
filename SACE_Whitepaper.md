
# Synthetic African Currency Exchange (SACE)

**Ticker:** SACE
**Creator:** Simon Kapenda
**Version:** 1.7
**Date:** October 6, 2025

## 1. Abstract

The **Synthetic African Currency Exchange (SACE)** is a blockchain-native BEP-20 token designed to represent a **weighted basket of top African currencies** — creating a synthetic currency index for Africa. Inspired by benchmarks such as the **U.S. Dollar Index (DXY)**, SACE enables a new form of cross-border value representation and a single tradable instrument reflecting Africa's collective currency strength.

SACE will serve as:
- **A tradable synthetic index** paired against USD or other basket currencies.
- **A transparent liquidity instrument** for African currencies.
- **A governance-driven financial infrastructure** to track Africa’s economic strength.

## 2. Project Vision

Africa is home to **54 countries with over 40 active currencies**, each with unique macroeconomic drivers. SACE aggregates the **top 21 most valuable currencies**, creating a stable and transparent synthetic index to:
- Increase intra-African trade efficiency.
- Provide a unified reference point for African forex.
- Empower financial products and derivatives.
- Enable investors to hedge or gain exposure to African currency movements.

## 3. SACE Currency Basket

As of **September 25, 2025**, SACE’s initial basket includes:

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

## 4. Tokenomics

- **Token Name:** Smart African Currency Exchange
- **Ticker:** SACE
- **Total Supply:** 10,000,000,000 SACE
- **Decimals:** 18
- **Token Type:** BEP-20 Upgradeable
- **Initial Mint:** All tokens minted to Treasury Wallet upon deployment.

### Treasury Wallet:
A single wallet address provided at deployment will hold the entire supply. The treasury manages distribution, liquidity, and governance.

## 5. Technical Design

SACE uses **OpenZeppelin’s Upgradeable BEP-20 implementation**, UUPS upgrade pattern, and integrates **Chainlink Oracles** for real-time currency rate feeds.

### Key Features:
1. **Currency Basket Management** — Dynamic add/remove currency logic.
2. **Weighted Index Calculation** — Based on currency weights and rates.
3. **Deviation Protection** — Ensures feed prices do not deviate beyond a configurable threshold.
4. **Oracle Heartbeat** — Ensures price feeds are current.
5. **Upgradeable Contract** — Governance controlled updates.
6. **Transparent Events** — Logs all changes and updates.

## 6. Oracle Integration

SACE integrates Chainlink price feeds for all currencies.

Key features:
- **Heartbeat Check:** Rejects stale feeds older than configurable time (default: 1 hour).
- **Deviation Check:** Rejects sudden feed deviations beyond **5%** by default.
- **Last Valid Price Storage:** Maintains resilience against faulty or malicious feeds.
- **Feed Validation:** Ensures valid feed addresses before updates.

## 7. Governance

SACE is **governance-controlled** via an ownership model with recommendations for:
- **Timelock mechanisms** for sensitive operations.
- **Multi-sig wallets** for treasury and upgrade control.
- **Basket rebalancing governance** to adjust weights and currencies.
- **Deviation parameter control** for Oracle protection.

## 8. Economic Impact

SACE creates a unified currency index for Africa with significant incentives:
- **Market Efficiency:** Single tradable instrument for African forex exposure.
- **Hedging:** Enables investors and institutions to hedge against African currency volatility.
- **Liquidity:** Increases cross-border liquidity for African currencies.
- **Transparency:** Provides clear tracking of African currency movements.

## 9. Conclusion

SACE is positioned to redefine African forex by introducing a **single tradable synthetic currency** representing Africa’s collective currency strength. It provides a powerful tool for traders, investors, and governments alike to measure, hedge, and trade African currencies in a standardized way.

---

**Author:** Simon Kapenda
