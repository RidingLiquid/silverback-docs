# Network Overview

Silverback DEX operates on two networks, each with unique advantages.

***

## Base Network

Base is an Ethereum Layer 2 built by Coinbase, offering low fees and full EVM compatibility.

### Features on Base

* **Classic Pools** — Simple 50/50 liquidity pools, great for beginners
* **Concentrated Pools** — Advanced pools with custom price ranges for higher capital efficiency
* **OpenOcean Aggregation** — Automatically finds the best rates across multiple DEXs
* **Deep Liquidity** — Access to popular trading pairs

### Best For

* Trading popular ERC-20 tokens
* Users already in the Ethereum ecosystem
* Leveraging deep liquidity on established pairs

### Requirements

* ETH on Base for gas fees
* Any Web3 wallet (MetaMask, Coinbase Wallet, etc.)

***

## Keeta Network

Keeta is a high-speed DAG-based blockchain designed for fast, low-cost transactions.

### Features on Keeta

* **AMM Pools** — Standard automated market maker pools
* **FX Anchor Trading** — Access official Keeta network anchors for stable trading
* **User Anchor Pools** — Create your own liquidity pools with custom fees (0.01% - 10%)
* **400ms Settlement** — Near-instant transaction finality
* **Lower Fees** — Minimal transaction costs

### Best For

* High-frequency trading
* Creating custom liquidity pools
* Users who want to earn fees as pool operators
* Fast settlement requirements

### Requirements

* KTA tokens for transaction fees
* Keythings wallet browser extension

***

## Comparison

| Feature          | Base        | Keeta        |
| ---------------- | ----------- | ------------ |
| Settlement Time  | \~2 seconds | \~400ms      |
| Gas Token        | ETH         | KTA          |
| Pool Types       | V2, V3      | AMM, Anchors |
| Custom Fee Pools | ❌           | ✅            |
| Aggregation      | OpenOcean   | FX Anchors   |
| Wallet           | Any Web3    | Keythings    |

***

## Switching Networks

Use the network dropdown in the top navigation bar to switch between Base and Keeta. Your wallet connection is maintained, but each network has separate:

* Token balances
* Liquidity positions
* Transaction history

***

## Which Should I Use?

**Choose Base if:**

* You're new to DeFi
* You want to trade established tokens
* You already have ETH on Base

**Choose Keeta if:**

* You want faster transactions
* You want to create your own liquidity pools
* You want to earn custom fees as a pool operator
