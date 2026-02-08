# Paid API (x402)

Access Silverback's DeFi intelligence through micropayments.

---

## What is x402?

[x402](https://www.x402.org) is a micropayment protocol built on HTTP. When you request a paid endpoint, the server responds with **HTTP 402 Payment Required** along with accepted payment options. Your client signs a payment authorization, re-sends the request, and the payment settles on-chain automatically.

You only pay if you get a response. No subscriptions, no API keys, no accounts.

---

## How It Works

1. **Request** any paid endpoint
2. **Receive** a 402 response with payment options
3. **Sign** a payment authorization (tokens don't leave your wallet yet)
4. **Re-send** the request with the signed payment attached
5. **Receive** the data — payment settles on-chain in the background

The [x402 SDK](https://www.x402.org) handles steps 2-4 automatically, so you can use it just like a normal `fetch` call.

---

## Accepted Tokens

Pay with any of these tokens on **Base** (Chain ID 8453):

| Token | Type | Settlement Fee | Notes |
|-------|------|---------------|-------|
| **$BACK** | Silverback Native | **0% (fee-exempt)** | **20% discount** on all endpoints |
| USDC | Stablecoin | 0.1% | Primary payment token |
| USDT | Stablecoin | 0.1% | |
| DAI | Stablecoin | 0.1% | |
| VIRTUAL | Ecosystem | 0.1% | Virtuals Protocol token |
| WETH | Blue-chip | 0.25% | Wrapped Ether |
| cbBTC | Blue-chip | 0.25% | Coinbase Wrapped BTC |

USDC payments are also accepted via the **Keeta** payment network.

Prices for volatile tokens (BACK, VIRTUAL, WETH, cbBTC) are calculated using live market rates, refreshed every 5 minutes.

---

## $BACK Payment Benefits

Paying with $BACK gives you two advantages over any other token:

- **Zero settlement fees** — all other tokens incur 0.1% to 0.25%
- **20% discount** on every endpoint — a $0.02 endpoint costs only $0.016 in BACK

This is the cheapest way to access Silverback's intelligence services.

---

## Available Endpoints

### Market Data ($0.001)

| Endpoint | Description |
|----------|-------------|
| Gas Price | Current Base network gas prices and estimates |
| Trending Tokens | Trending tokens on Base |
| Top Coins | Top coins by market cap with Silverback intelligence signals |
| Top Pools | Top liquidity pools ranked by APR |
| Top Protocols | Top DeFi protocols by TVL |
| Token Metadata | Detailed token information, links, and stats |

### Agent Intelligence ($0.001 - $0.002)

| Endpoint | Price | Description |
|----------|-------|-------------|
| Agent Reputation | $0.001 | On-chain agent reputation scores (ERC-8004) |
| Agent Discover | $0.002 | Discover registered AI agents on Base |

### Trading Tools ($0.002 - $0.05)

| Endpoint | Price | Description |
|----------|-------|-------------|
| Swap Quote | $0.002 | DEX swap quotes via 0x Protocol aggregation |
| Pool Analysis | $0.005 | Pool reserves, TVL, fees, and risk analysis |
| Correlation Matrix | $0.005 | Multi-token price correlations (30-day) |
| Token Audit | $0.01 | Smart contract security audit (honeypot, taxes, ownership) |
| Whale Moves | $0.01 | Large holder movement tracking |
| Technical Analysis | $0.02 | RSI, EMA, Bollinger Bands, and chart patterns |
| DeFi Yield | $0.02 | DeFi yield opportunities across Base protocols |
| Arbitrage Scanner | $0.02 | Cross-DEX price spread detection |
| Swap Execution | $0.05 | Execute a DEX swap |

### Advanced ($0.10)

| Endpoint | Price | Description |
|----------|-------|-------------|
| Backtest | $0.10 | Historical trading strategy backtesting |

---

## Quick Start

### Using the x402 SDK (Recommended)

```javascript
import { wrapFetchWithPaymentFromConfig } from '@x402/fetch';
import { ExactEvmScheme } from '@x402/evm/exact/client';

const fetchWithPayment = wrapFetchWithPaymentFromConfig(fetch, {
  schemes: [{
    x402Version: 2,
    network: 'eip155:8453',
    client: new ExactEvmScheme(yourWalletSigner)
  }]
});

// Use it like normal fetch — payments are automatic
const response = await fetchWithPayment(
  'https://x402.silverbackdefi.app/api/v1/gas-price'
);
const data = await response.json();
```

### Requirements

- A wallet with any accepted token on Base
- **USDC:** Works out of the box (uses ERC-3009, no approvals needed)
- **All other tokens:** One-time ERC-20 approval to Permit2 (`0x000000000022D473030F116dDEE9F6B43aC78BA3`)

---

## Free Endpoints

These endpoints are available without payment:

| Endpoint | Description |
|----------|-------------|
| `/health` | Server health check |
| `/api/v1/pricing` | View all endpoint prices |
| `/api/v1/endpoints` | List available endpoints |

---

## Silverback Facilitator

Silverback operates its own **x402 payment facilitator** — the service that verifies, settles, and confirms payments on-chain. This means Silverback controls the full payment stack, with no reliance on third-party facilitators.

**Facilitator URL:** `https://facilitator.silverbackdefi.app`

### What the Facilitator Does

When you send a signed payment with your API request:

1. **Verify** — The facilitator checks that your signature is valid, the token is whitelisted, and the amount covers the endpoint price
2. **Settle** — It submits the payment transaction on-chain (Base), transferring tokens from your wallet to the fee splitter
3. **Confirm** — Once the transaction is confirmed (~2 seconds), it returns a receipt with the transaction hash

You never interact with the facilitator directly — the x402 SDK and Silverback's API server handle this automatically.

### Payment Protocols

The facilitator supports two settlement protocols:

| Protocol | Used For | How It Works |
|----------|----------|-------------|
| **ERC-3009** | USDC | `transferWithAuthorization` — no approvals needed, sign-and-pay in one step |
| **Permit2** | All other tokens | `permitWitnessTransferFrom` — requires a one-time ERC-20 approval to Permit2 |

When you pay with USDC, the x402 SDK uses ERC-3009 for the simplest experience. For any other token (BACK, USDT, DAI, VIRTUAL, WETH, cbBTC), it uses Permit2.

### Fee Splitter

All payments route through Silverback's on-chain **Fee Splitter** contract on Base:

```
Your Wallet → Fee Splitter Contract → splits into:
  ├── Net payment → Silverback Treasury (for buybacks & operations)
  └── Settlement fee → Protocol fee recipient
```

- **$BACK payments:** No fee is taken — 100% goes to treasury
- **Stablecoin payments (USDC, USDT, DAI):** 0.1% fee
- **Blue-chip payments (WETH, cbBTC):** 0.25% fee
- **Ecosystem tokens (VIRTUAL):** 0.1% fee

The fee split happens atomically on-chain — there's no manual step or delay.

### Token Whitelisting

The facilitator only accepts **curated, whitelisted tokens**. This protects against scam tokens and ensures reliable settlement. The current whitelist includes 8 tokens across stablecoins, ecosystem tokens, and blue-chip assets.

New tokens can apply for listing on a case-by-case basis.

### Facilitator Public Endpoints

The facilitator exposes several informational endpoints (no payment required):

| Endpoint | Description |
|----------|-------------|
| [`/health`](https://facilitator.silverbackdefi.app/health) | Facilitator status, fee model, and configuration |
| [`/supported`](https://facilitator.silverbackdefi.app/supported) | Supported networks, tokens, and x402 version |
| [`/supported/tokens`](https://facilitator.silverbackdefi.app/supported/tokens) | All accepted tokens with live USD prices |
| [`/supported/convert?usd=0.02&token=BACK`](https://facilitator.silverbackdefi.app/supported/convert?usd=0.02&token=BACK) | Convert a USD amount to any token |
| [`/settle/stats`](https://facilitator.silverbackdefi.app/settle/stats) | Total settlements, volume, and success rate |
| [`/discovery/resources`](https://facilitator.silverbackdefi.app/discovery/resources) | Browse all available paid resources |

### Settlement Details

| Property | Value |
|----------|-------|
| Network | Base (Chain ID 8453) |
| Confirmation | ~2 seconds (1 block) |
| x402 Version | 2 |
| Scheme | Exact |
| Facilitator Address | `0x48380bCf1c09773C9E96901F89A7A6B75E2BBeCC` |
| Fee Splitter | `0x6FDf52EA446B54508f50C35612210c69Ef1C1d9a` |
| Permit2 | `0x000000000022D473030F116dDEE9F6B43aC78BA3` |
| Treasury | `0xD34411a70EffbDd000c529bbF572082ffDcF1794` |

After a successful payment, the transaction hash is returned in the `payment-response` header so you can verify settlement on [BaseScan](https://basescan.org).

---

## FAQ

**Do I need an API key?**
No. x402 uses wallet-based authentication. Your payment signature is your access.

**What if the request fails after I pay?**
The x402 protocol is atomic — payment only settles if the server returns a successful response.

**Can I pay from any wallet?**
Yes, any wallet with supported tokens on Base. Hardware wallets, browser wallets, and programmatic wallets all work.

**How do I approve tokens for Permit2?**
For non-USDC tokens, you need a one-time ERC-20 approval to the Permit2 contract. Most wallet interfaces can do this, or you can call `approve()` directly on the token contract with spender `0x000000000022D473030F116dDEE9F6B43aC78BA3`.

**Where do payments go?**
Payments route through Silverback's fee splitter contract. The settlement fee (if any) goes to protocol operations, and the net amount goes to the Silverback treasury — contributing to $BACK buybacks and staker rewards.
