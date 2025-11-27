# Managing Liquidity

Provide liquidity to earn trading fees on Silverback DEX.

***

## How Liquidity Works

When you provide liquidity, you:

1. Deposit a pair of tokens into a pool
2. Receive LP (Liquidity Provider) tokens representing your share
3. Earn a portion of all swap fees in that pool
4. Can withdraw your tokens + earned fees anytime

**Example:** You deposit ETH + USDC. Every time someone swaps ETH↔USDC, you earn a share of the 0.3% fee.

***

## Base Network Liquidity

### Pool Types

**Classic  Pools**

* Simple 50/50 token ratio
* Great for beginners
* Liquidity spread across all prices
* Earn fees on any trade in the pool

**Concentrated  Pools**

* Advanced — set custom price ranges
* Higher capital efficiency
* Earn more fees when price is in your range
* Requires more active management

***

### Adding Liquidity (Classic )

1. Navigate to **Pool** page
2. Select **Base** network
3. Click **"Add Liquidity"**
4. Select two tokens for your pair
5. Enter amount for the first token
   * Second token auto-calculates to maintain 50/50 ratio
6. Review:
   * Pool share you'll receive
   * LP tokens you'll get
7. Click **"Add Liquidity"**
8. **Approve both tokens** (if first time)
9. Confirm the transaction
10. Receive LP tokens in your wallet

> **Note:** You must add both tokens in equal value. If you enter 1 ETH worth $2,000, you'll need $2,000 worth of the other token.

***

### Adding Liquidity (Concentrated )

1. Navigate to **Pool** page
2. Select **Base** network
3. Click **"New Position"**
4. Select token pair
5. Choose fee tier (0.05%, 0.3%, or 1%)
6. Set your price range:
   * **Min Price**: Lower bound
   * **Max Price**: Upper bound
7. Enter deposit amounts
8. Review position details
9. Confirm transaction

**Price Range Tips:**

* Narrower range = More fees when in range, but risk going out of range
* Wider range = Less fees, but more consistent earnings
* Current price should be within your range to start earning

***

### Removing Liquidity (Base)

1. Go to **Portfolio** page
2. Find your position under "Classic Positions" or "Concentrated Positions"
3. Click **"Remove"** or **"Withdraw"**
4. Choose how much to remove:
   * 25% / 50% / 75% / 100%
   * Or enter custom amount
5. Review what you'll receive:
   * Original tokens
   * Accumulated fees
6. Confirm the transaction
7. Tokens return to your wallet

***

## Keeta Network Liquidity

### Creating a New Pool

On Keeta, you can create entirely new pools:

1. Navigate to **Keeta → Pool** page
2. Click **"Create New Pool"**
3. Select two tokens
4. Set initial amounts for both tokens
   * This determines the starting price ratio
   * Example: 100 KTA + 1000 USDC = 1 KTA costs 10 USDC
5. Click **"Create Pool"**
6. Confirm 3 transactions:
   * Create pool structure
   * Send token A
   * Send token B
7. Receive LP tokens

### Adding to Existing Pools

1. Navigate to **Keeta → Pool** page
2. Find the pool you want to join
3. Click **"Add Liquidity"**
4. Enter amounts (maintains current price ratio)
5. Confirm transactions
6. Receive LP tokens proportional to your deposit

### Removing Liquidity (Keeta)

1. View your positions on the Pool page
2. Select the pool
3. Enter amount to remove
4. Confirm transaction
5. Receive tokens proportionally based on current pool ratio

***

## Understanding LP Tokens

### What Are LP Tokens?

LP tokens are proof of your deposit. They represent:

* Your share of the pool's total liquidity
* Your claim on accumulated fees
* Your right to withdraw

### Important Facts

* **Keep them safe** — Losing LP tokens = losing your liquidity
* **They're transferable** — Can be sent to other wallets
* **Value changes** — Based on pool performance and token prices
* **Fees compound** — Automatically added to the pool (no claiming needed)

***

## Earning Fees

### How Fees Work

| Network | Pool Type       | Fee Rate             |
| ------- | --------------- | -------------------- |
| Base    | Classic V2      | 0.3% per swap        |
| Base    | Concentrated V3 | 0.05% / 0.3% / 1%    |
| Keeta   | AMM             | 0.3% per swap        |
| Keeta   | Anchor          | Custom (0.01% - 10%) |

### Fee Distribution

Fees are distributed proportionally based on your share of the pool.

**Example:**

* Pool has $100,000 total liquidity
* You provided $10,000 (10% share)
* Pool earns $100 in fees
* You earn $10 (10% of fees)

### When You Receive Fees

* **Classic/AMM Pools**: Fees automatically compound into the pool. You receive them when you withdraw.
* **Concentrated Positions**: Fees accumulate separately and can be claimed anytime.
* **Anchor Pools**: Fees compound into your pool reserves.

***

## Impermanent Loss

### What Is It?

Impermanent loss occurs when the price ratio of your deposited tokens changes. You may end up with less value than if you had simply held the tokens.

### Example

You deposit:

* 1 ETH ($2,000)
* 2,000 USDC
* Total: $4,000

ETH price doubles to $4,000:

* If you had just held: 1 ETH ($4,000) + 2,000 USDC = $6,000
* In the pool: \~0.71 ETH ($2,828) + 2,828 USDC = $5,656
* Impermanent loss: \~$344 (5.7%)

### Key Points

* Called "impermanent" because it reverses if prices return to original ratio
* Becomes "permanent" when you withdraw at different price ratios
* **Trading fees can offset impermanent loss** — If you earn more in fees than you lose to IL, you're still profitable

### Minimizing Impermanent Loss

* Provide liquidity to **stable pairs** (USDC/USDT) — minimal price divergence
* Choose **correlated assets** — tokens that move together
* Use **concentrated positions** strategically
* **Monitor your positions** — withdraw if IL exceeds fee earnings

***

## Best Practices

1. **Start small** — Test with small amounts first
2. **Understand the tokens** — Know what you're providing liquidity for
3. **Monitor regularly** — Check your positions periodically
4. **Consider IL** — Factor impermanent loss into your strategy
5. **Compare APRs** — Higher fees might come with higher risk
6. **Diversify** — Don't put all liquidity in one pool
