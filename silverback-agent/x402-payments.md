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

## Settlement

- **Network:** Base (Chain ID 8453)
- **Confirmation:** ~2 seconds (1 block)
- **Facilitator:** Silverback operates its own payment facilitator
- **Fee Splitter:** Payments route through an on-chain fee splitter that handles fee separation automatically
- **Transaction hash** is returned in the response headers after successful payment

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
