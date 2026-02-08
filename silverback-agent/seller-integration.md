# Seller Integration Guide

How to accept multi-token x402 payments on your own endpoints using the Silverback Facilitator.

---

## Overview

If you run an API and want to charge per-request using x402, the Silverback Facilitator handles all the payment complexity for you. You just need to:

1. Return the right **402 response** when a client hasn't paid
2. Point to the **Silverback Facilitator** as your payment processor
3. Choose which **tokens** you want to accept

The facilitator handles signature verification, on-chain settlement, fee splitting, and transaction confirmation. You focus on your API.

---

## What You Need

- A wallet address on Base to receive payments
- An Express.js server (or any HTTP server)
- The `@x402/express` package (or build your own 402 responses)

---

## Step 1: Install Dependencies

```bash
npm install @x402/express
```

---

## Step 2: Configure Your Server

Set these environment variables:

```bash
# Required
X402_WALLET_ADDRESS=0xYourWalletAddress      # Where you receive payments

# Facilitator
SILVERBACK_FACILITATOR_URL=https://facilitator.silverbackdefi.app
```

---

## Step 3: Set Up Payment Middleware

### Option A: Using the x402 Express Package

```javascript
import express from 'express';
import { paymentMiddleware } from '@x402/express';

const app = express();

// Configure the Silverback facilitator
const facilitatorUrl = 'https://facilitator.silverbackdefi.app';

// Define your paid routes with prices
const paidRoutes = {
  '/api/v1/my-endpoint': {
    price: '$0.01',
    network: 'eip155:8453',
    config: {
      description: 'My paid endpoint'
    }
  }
};

// Add payment middleware
app.use(paymentMiddleware(
  facilitatorUrl,
  paidRoutes,
  { walletAddress: process.env.X402_WALLET_ADDRESS }
));

// Your endpoint logic (only runs after payment is verified)
app.get('/api/v1/my-endpoint', (req, res) => {
  res.json({ success: true, data: 'Your response here' });
});

app.listen(3000);
```

### Option B: Build Your Own 402 Response

If you're not using Express or want full control, return an HTTP 402 response with the `payment-required` header:

```javascript
app.get('/api/v1/my-endpoint', (req, res) => {
  // Check if request has a valid payment
  const paymentHeader = req.headers['payment-signature'];

  if (!paymentHeader) {
    // Return 402 with payment options
    const paymentRequired = {
      x402Version: 2,
      error: 'Payment required',
      accepts: buildAcceptsArray('$0.01'),
      facilitatorUrl: 'https://facilitator.silverbackdefi.app'
    };

    const encoded = Buffer.from(JSON.stringify(paymentRequired)).toString('base64');

    return res.status(402)
      .set('payment-required', encoded)
      .set('Cache-Control', 'no-store, no-cache, must-revalidate, private')
      .json({ error: 'Payment required' });
  }

  // Payment exists — verify with facilitator, then serve response
  // ...
});
```

---

## Step 4: Build Your Payment Options

The `accepts` array in your 402 response tells clients which tokens you accept. Each entry is one payment option.

### Accept Only USDC (Simplest)

```javascript
function buildAcceptsArray(usdPrice) {
  const priceNum = parseFloat(usdPrice.replace('$', ''));
  const usdcAmount = Math.floor(priceNum * 1e6); // USDC has 6 decimals

  return [{
    scheme: 'exact',
    network: 'eip155:8453',
    amount: usdcAmount.toString(),
    asset: '0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913',  // USDC on Base
    payTo: process.env.X402_WALLET_ADDRESS,
    maxTimeoutSeconds: 300,
    extra: {
      name: 'USD Coin',
      version: '2'
    }
  }];
}
```

### Accept Multiple Tokens

To accept more tokens, add additional entries to the `accepts` array. Each token needs its amount converted from USD at the current market rate.

```javascript
// Token registry with addresses and decimals
const TOKENS = {
  USDC:    { address: '0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913', decimals: 6,  stable: true },
  USDT:    { address: '0xfde4C96c8593536E31F229EA8f37b2ADa2699bb2', decimals: 6,  stable: true },
  DAI:     { address: '0x50c5725949A6F0c72E6C4a641F24049A917DB0Cb', decimals: 18, stable: true },
  VIRTUAL: { address: '0x0b3e328455c4059EEb9e3f84b5543F74E24e7E1b', decimals: 18, stable: false },
  WETH:    { address: '0x4200000000000000000000000000000000000006', decimals: 18, stable: false },
  cbBTC:   { address: '0xcbB7C0000aB88B473b1f5aFd9ef808440eed33Bf', decimals: 8,  stable: false },
  BACK:    { address: '0x558881c4959e9cf961a7E1815FCD6586906babd2', decimals: 18, stable: false, discount: 0.20 },
};

function buildAcceptsArray(usdPrice) {
  const priceNum = parseFloat(usdPrice.replace('$', ''));
  const accepts = [];

  for (const [symbol, token] of Object.entries(TOKENS)) {
    // Get token price (stablecoins = $1, others fetched from API)
    const tokenPrice = token.stable ? 1.0 : getTokenPrice(symbol);
    if (!tokenPrice) continue;

    // Apply discount for BACK
    const effectivePrice = token.discount
      ? priceNum * (1 - token.discount)
      : priceNum;

    // Convert USD to token base units
    const tokenAmount = effectivePrice / tokenPrice;
    const baseUnits = BigInt(Math.floor(tokenAmount * (10 ** token.decimals)));

    accepts.push({
      scheme: 'exact',
      network: 'eip155:8453',
      amount: baseUnits.toString(),
      asset: token.address,
      payTo: process.env.X402_WALLET_ADDRESS,
      maxTimeoutSeconds: 300,
      extra: {
        name: symbol,
        version: '2',
        ...(token.discount ? { discount: `${token.discount * 100}%` } : {})
      }
    });
  }

  return accepts;
}
```

### Use the Facilitator's Price API

Instead of fetching prices yourself, use the facilitator's built-in converter:

```bash
# Convert $0.02 to BACK token amount
curl https://facilitator.silverbackdefi.app/supported/convert?usd=0.02&token=BACK

# Response:
# { "usdAmount": 0.02, "tokenAmount": "206172839506173", "token": "BACK", "decimals": 18 }
```

```bash
# Get all token prices at once
curl https://facilitator.silverbackdefi.app/supported/tokens

# Response includes priceUsd for each token
```

---

## Step 5: Using the Fee Splitter (Optional)

If you want Silverback to handle fee splitting automatically, point `payTo` to the Fee Splitter contract instead of your own wallet. The fee splitter takes a small percentage and forwards the rest to your wallet.

```javascript
const FEE_SPLITTER = '0x6FDf52EA446B54508f50C35612210c69Ef1C1d9a';

// In your accepts array:
{
  scheme: 'exact',
  network: 'eip155:8453',
  amount: '2000',
  asset: '0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913',
  payTo: FEE_SPLITTER,                    // Route through fee splitter
  maxTimeoutSeconds: 300,
  extra: {
    name: 'USD Coin',
    version: '2',
    actualRecipient: '0xYourWalletAddress'  // You receive the net amount
  }
}
```

**Fee rates:**
- $BACK: 0% (fee-exempt)
- Stablecoins (USDC, USDT, DAI): 0.1%
- Ecosystem tokens (VIRTUAL): 0.1%
- Blue-chip assets (WETH, cbBTC): 0.25%

---

## Step 6: Register in the Discovery Bazaar (Optional)

Make your endpoints discoverable by other agents and clients through the Silverback Bazaar.

The facilitator's discovery service at `/discovery/resources` lets clients browse available x402 endpoints. To register your endpoints, publish a resource description that includes:

```json
{
  "resource": "https://your-server.com/api/v1/your-endpoint",
  "x402Version": 2,
  "description": "What your endpoint does",
  "category": "defi",
  "tags": ["swap", "quote", "base"],
  "accepts": [
    { "scheme": "exact", "network": "eip155:8453", "amount": "2000", "asset": "0x833589..." }
  ],
  "metadata": {
    "provider": "Your Project Name",
    "method": "POST",
    "inputSchema": {
      "type": "object",
      "properties": {
        "token": { "type": "string", "description": "Token address or symbol" }
      },
      "required": ["token"]
    }
  }
}
```

**Discovery endpoints on the facilitator:**
- `GET /discovery/resources` — Browse all registered endpoints
- `GET /discovery/categories` — List categories (defi, trading, analytics, etc.)
- `GET /discovery/tags` — List all tags
- `GET /discovery/info` — Service metadata

---

## Important Rules

### Amount Conversion

Amounts in the `accepts` array must be in **base units** (smallest denomination), not human-readable amounts:

| Token | Decimals | $0.01 in base units |
|-------|----------|-------------------|
| USDC | 6 | `10000` |
| USDT | 6 | `10000` |
| DAI | 18 | `10000000000000000` |
| WETH (at $2,100) | 18 | `4761904761904` |
| cbBTC (at $70,000) | 8 | `14` |
| BACK (at $0.00008) | 18 | `100000000000000000000` |

Getting decimals wrong is the most common integration mistake.

### Cache Control

Always set `Cache-Control: no-store, no-cache, must-revalidate, private` on 402 responses. Without this, CDNs may cache the payment options and serve stale prices.

### Timeout

Set `maxTimeoutSeconds: 300` (5 minutes) in each payment option. This gives clients enough time to sign and submit payment without the authorization expiring.

### x402 Version

Always set `x402Version: 2` in your 402 response. Version 2 supports both ERC-3009 (USDC) and Permit2 (all other tokens).

---

## How the Client Pays

You don't need to implement this — the x402 SDK handles it automatically on the client side. But for reference:

1. Client receives your 402 response
2. SDK picks a token the client has sufficient balance for
3. **USDC** — SDK signs an ERC-3009 `transferWithAuthorization` (no prior approval needed)
4. **Any other token** — SDK signs a Permit2 `permitWitnessTransferFrom` (requires one-time Permit2 approval)
5. SDK sends the signed payment to the facilitator for verification
6. Facilitator confirms the payment is valid
7. SDK re-sends the original request with the payment proof in the `PAYMENT-SIGNATURE` header
8. Your server receives the request, facilitator settles on-chain, and you serve the response

---

## What You Handle vs What the Facilitator Handles

| You (the seller) | Silverback Facilitator |
|-------------------|----------------------|
| Return 402 with `accepts` array | Verify payment signatures |
| Set prices in USD | Convert USD to token amounts |
| Choose which tokens to accept | Settle payments on-chain |
| Serve your API response | Split fees automatically |
| Keep token prices updated | Manage Permit2 and ERC-3009 |
| | Provide discovery/bazaar |
| | Handle transaction confirmations |

---

## Testing

### On Testnet (Base Sepolia)

Use `network: 'eip155:84532'` in your `accepts` array and test with Sepolia tokens.

### On Mainnet

Start with low-value endpoints ($0.001) and test with the [x402 SDK](https://www.x402.org):

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

const res = await fetchWithPayment('https://your-server.com/api/v1/my-endpoint');
console.log(await res.json());
```

### Verify Facilitator Connectivity

```bash
# Check the facilitator is reachable
curl https://facilitator.silverbackdefi.app/health

# Check supported tokens and current prices
curl https://facilitator.silverbackdefi.app/supported/tokens

# Test price conversion
curl "https://facilitator.silverbackdefi.app/supported/convert?usd=0.01&token=BACK"
```

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Client gets 402 but can't pay | Check `accepts` array format — amounts must be strings in base units |
| Payment fails with "unsupported token" | Token must be on the facilitator's whitelist (check `/supported/tokens`) |
| Wrong amount charged | Verify decimal conversion: `amount = usdPrice * 10^decimals` |
| Stale prices for volatile tokens | Refresh prices every 5 minutes, or use the facilitator's `/supported/convert` endpoint |
| 402 response cached by CDN | Add `Cache-Control: no-store` header to 402 responses |
| Settlement timeout | Check that `maxTimeoutSeconds` is at least 300 |

---

## Need Help?

- **Facilitator status:** [facilitator.silverbackdefi.app/health](https://facilitator.silverbackdefi.app/health)
- **Supported tokens:** [facilitator.silverbackdefi.app/supported/tokens](https://facilitator.silverbackdefi.app/supported/tokens)
- **x402 protocol docs:** [x402.org](https://www.x402.org)
- **Community:** [Silverback Discord](https://discord.gg/silverback)
