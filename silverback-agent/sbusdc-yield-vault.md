# sbUSDC Yield Vault

Silverback's ERC-4626 tokenized yield vault that automatically generates yield on your USDC deposits through DeFi lending protocols.

## Overview

sbUSDC is a yield-bearing token. When you deposit USDC, you receive sbUSDC shares. As yield accrues from lending markets, your shares become worth more USDC - no action required.

**Key Features:**
- Automatic yield generation (5-10% APY)
- Non-custodial - your funds stay in smart contracts
- Withdraw anytime
- Built on Morpho Blue lending protocol

## How It Works

### User Flow

```
1. DEPOSIT:
   User USDC ──► sbUSDC Vault ──► Strategy ──► Morpho Blue
                    │                              │
                    ▼                              ▼
               Mint sbUSDC              Supply to lending market
               shares to user           (borrowers pay interest)

2. YIELD ACCRUAL (happens automatically):
   Morpho Blue ──► Interest accrues ──► Strategy balance grows
                                              │
                                              ▼
                                    sbUSDC share price increases
                                    (totalAssets grows, supply stays same)

3. WITHDRAW:
   User burns ──► sbUSDC Vault ──► Strategy ──► Morpho Blue
   sbUSDC              │               │              │
                       ▼               ▼              ▼
                  Calculate       Withdraw        Return USDC
                  assets owed     from Morpho    + earned interest
```

### Architecture

```
┌────────────────────────────────────────────────────────────────────────────┐
│                           SILVERBACK CONTRACTS                              │
├────────────────────────────────────────────────────────────────────────────┤
│                                                                            │
│   ┌──────────────┐         ┌──────────────────┐                           │
│   │   sbUSDC     │         │  MorphoStrategy  │                           │
│   │   Vault      │◄───────►│                  │                           │
│   │              │         │  - deposit()     │                           │
│   │ ERC-4626     │         │  - withdraw()    │                           │
│   │ - deposit()  │         │  - balanceOf()   │                           │
│   │ - withdraw() │         │                  │                           │
│   │ - redeem()   │         │  Calls Morpho    │                           │
│   └──────────────┘         └────────┬─────────┘                           │
│                                     │                                      │
└─────────────────────────────────────┼──────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         MORPHO BLUE (External)                              │
│                    0xBBBBBbbBBb9cC5e90e3b3Af64bdAF62C37EEFFCb               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   ┌─────────────────────────────────────────────────────────────────┐      │
│   │                    Lending Markets                               │      │
│   ├─────────────────────────────────────────────────────────────────┤      │
│   │  cbBTC/USDC Market     │  Borrowers deposit cbBTC collateral    │      │
│   │  ─────────────────     │  borrow USDC, pay ~8% interest         │      │
│   │  Our USDC goes here ──►│                                        │      │
│   │                        │  Interest paid ──► grows our balance   │      │
│   ├─────────────────────────────────────────────────────────────────┤      │
│   │  WETH/USDC Market      │  Similar - ETH collateral             │      │
│   │  wstETH/USDC Market    │  Similar - staked ETH collateral      │      │
│   └─────────────────────────────────────────────────────────────────┘      │
│                                                                             │
│   Interest Rate Model:                                                      │
│   - Low utilization  → ~2-3% borrow rate                                   │
│   - High utilization → ~10-15% borrow rate                                 │
│   - Supply APY = Borrow Rate × Utilization                                  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Where Does Yield Come From?

The yield comes from **borrowers paying interest** on Morpho Blue lending markets:

1. **Borrowers** deposit collateral (cbBTC, WETH, etc.) and borrow USDC
2. **Borrowers pay interest** on their loans (5-15% APY depending on utilization)
3. **Interest flows to suppliers** (the sbUSDC vault)
4. **Share price increases** as total assets grow

### Expected Returns

| Market Conditions | Typical APY |
|-------------------|-------------|
| Low utilization   | 3-5%        |
| Normal            | 5-8%        |
| High demand       | 8-12%       |

*APY varies based on market conditions and borrower demand*

## How Share Price Works

The key insight is that **shares stay constant, but their value grows**:

```
Initial State:
- You deposit: 1000 USDC
- You receive: 1000 sbUSDC shares
- Share price: 1.00 USDC per share

After 1 Year (6% APY):
- Your shares: 1000 sbUSDC (unchanged)
- Total assets: 1060 USDC (grew from interest)
- Share price: 1.06 USDC per share
- Your value: 1000 × 1.06 = 1060 USDC
```

### Calculating Your Balance

```
your_usdc_value = your_sbUSDC_shares × share_price

share_price = total_assets / total_supply
```

## Contract Addresses

### Base Sepolia (Testnet)

| Contract | Address |
|----------|---------|
| sbUSDC Vault | `0xADACac4C81D0290Dac215f0B9923140f4AeC4901` |
| MockStrategy | `0x0A74a5366F6ddEd86E26c4237953BFbc7E07A257` |
| USDC | `0x036CbD53842c5426634e7929541eC2318f3dCF7e` |

### Base Mainnet (Production)

| Contract | Address |
|----------|---------|
| sbUSDC Vault | *Coming Soon* |
| MorphoStrategy | *Coming Soon* |
| USDC | `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913` |
| Morpho Blue | `0xBBBBBbbBBb9cC5e90e3b3Af64bdAF62C37EEFFCb` |

## Using sbUSDC

### Deposit USDC

```javascript
// 1. Approve vault to spend USDC
await usdc.approve(VAULT_ADDRESS, amount);

// 2. Deposit USDC, receive sbUSDC
const shares = await vault.deposit(amount, yourAddress);
```

### Check Your Balance

```javascript
const shares = await vault.balanceOf(yourAddress);
const usdcValue = await vault.convertToAssets(shares);
const sharePrice = await vault.sharePrice();

console.log('sbUSDC shares:', shares);
console.log('Worth in USDC:', usdcValue);
console.log('Share price:', sharePrice); // 1e6 = $1.00
```

### Withdraw

```javascript
// Redeem all shares for USDC + yield
const shares = await vault.balanceOf(yourAddress);
const usdcReceived = await vault.redeem(shares, yourAddress, yourAddress);
```

## Security

### Non-Custodial

- All funds are held in smart contracts on-chain
- No centralized party can access your funds
- You can withdraw anytime (subject to liquidity)

### Risks

| Risk | Description |
|------|-------------|
| Smart Contract | Bugs in sbUSDC, Strategy, or Morpho contracts |
| Liquidity | If all USDC is lent out, withdrawals may be delayed |
| Market | APY fluctuates based on market conditions |
| Oracle | Morpho relies on price oracles for collateral valuation |

### Audits

- **Morpho Blue**: Audited by Spearbit, Trail of Bits, Cantina
- **sbUSDC Vault**: Audit pending

## FAQ

### How often does yield accrue?

Yield accrues continuously, every block. The share price updates in real-time.

### Is there a minimum deposit?

No minimum, but gas costs may make very small deposits inefficient.

### Can I lose money?

Principal is generally safe unless there's a smart contract exploit. Yield can fluctuate but principal is protected by over-collateralized loans.

### How do I track my earnings?

Compare your current `convertToAssets(balance)` to your original deposit. The difference is your earned yield.
