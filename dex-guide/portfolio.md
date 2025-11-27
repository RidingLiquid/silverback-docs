# Portfolio & Positions

Track and manage all your liquidity positions on Silverback DEX.

***

## Base Network Portfolio

Navigate to the **Portfolio** page to view all your Base positions.

### Classic Positions&#x20;

Your V2 liquidity positions display:

| Field          | Description                     |
| -------------- | ------------------------------- |
| Pool           | Token pair (e.g., ETH/USDC)     |
| Your Liquidity | Dollar value of your position   |
| Pool Share     | Your percentage of total pool   |
| Token Amounts  | How many of each token you have |
| Unclaimed Fees | Fees earned (auto-compounded)   |

**Actions:**

* **Add** — Deposit more liquidity
* **Remove** — Withdraw some or all

### Concentrated Positions&#x20;

Positions show additional information:

| Field          | Description                      |
| -------------- | -------------------------------- |
| Price Range    | Min/Max prices for your position |
| Status         | In Range ✅ or Out of Range ⚠️    |
| Liquidity      | Your deposited value             |
| Unclaimed Fees | Fees ready to claim              |
| Token Amounts  | Current composition              |

**Actions:**

* **Collect Fees** — Claim earned fees to wallet
* **Add Liquidity** — Increase position size
* **Remove** — Withdraw partially or fully
* **Close** — Remove 100% and close position

### Position Health&#x20;

Your concentrated position only earns fees when the current price is within your range:

| Status              | Meaning               | Action                   |
| ------------------- | --------------------- | ------------------------ |
| ✅ In Range          | Earning fees normally | None needed              |
| ⚠️ Out of Range     | Not earning fees      | Consider adjusting range |
| 🔴 Far Out of Range | Idle liquidity        | Reposition or withdraw   |

***

## Keeta Network Portfolio

### Viewing Positions

Navigate to **Keeta → My Anchors** to view your Keeta positions.

Each anchor pool shows:

* Pool pair and reserves
* 24h volume and swap count
* Fees collected
* Pool status (Active/Paused)

### Managing Positions

From your portfolio you can:

* Update pool fees
* Pause/resume pools
* Add or remove liquidity
* View detailed analytics

***

## Understanding Your Returns

### Fee Earnings

Your earnings depend on:

1. **Pool volume** — More swaps = more fees
2. **Your share** — Larger share = more of each fee
3. **Fee rate** — Higher rate = more per swap (but may reduce volume)

**Example Calculation:**

* Pool does $10,000 daily volume
* Fee rate is 0.3%
* Daily fees: $30
* Your share: 10%
* Your daily earnings: $3

### Total Return

Your total return = Fee Earnings - Impermanent Loss

Even with impermanent loss, you can be profitable if fees exceed IL.

***

## Tracking Performance

### What to Monitor

| Metric      | Why It Matters                        |
| ----------- | ------------------------------------- |
| Pool TVL    | Larger pools = more stable            |
| Volume      | Higher volume = more fees             |
| Your Share  | Tracks if being diluted               |
| Fee APR     | Annualized fee return                 |
| IL Exposure | Potential downside from price changes |

### Performance Tips

1. **Check weekly** — Don't obsess daily, but stay informed
2. **Compare to holding** — Would holding tokens outperform?
3. **Track fees vs IL** — Are fees covering impermanent loss?
4. **Rebalance when needed** — Adjust positions that underperform

***

## Withdrawing Liquidity

### When to Withdraw

Consider withdrawing when:

* You need the capital elsewhere
* IL exceeds fee earnings significantly
* Pool volume has dried up
* You want to reposition into better opportunities

### How to Withdraw

**Base Classic :**

1. Go to Portfolio
2. Find position → Click "Remove"
3. Select percentage (25/50/75/100%)
4. Confirm transaction
5. Receive both tokens + fees

**Base Concentrated :**

1. Go to Portfolio
2. Find position → Click "Collect" for fees only
3. Or "Remove" for partial/full withdrawal
4. Confirm transaction

**Keeta Pools:**

1. Go to My Anchors
2. Select pool → Remove Liquidity
3. Enter amount
4. Confirm transactions
5. Receive tokens proportionally

### What You Receive

When withdrawing, you receive:

* Proportional amounts of both tokens (based on current ratio)
* Any accumulated fees

> **Note:** You may not receive the same ratio you deposited. The pool ratio changes as people trade.

***

## FAQ

**Why did my token amounts change?**

As people trade, the pool ratio shifts. You always own a percentage of the _pool_, not fixed token amounts.

**Where are my fees?**

* Classic/AMM: Auto-compounded into position (received on withdrawal)
* Concentrated: Accumulate separately (claim anytime)
* Anchors: Compound into pool reserves

**Can I withdraw anytime?**

Yes! Liquidity is never locked. Withdraw whenever you want.

**Why is my Concentrated LP position out of range?**

The market price moved outside your set range. You're not earning fees until price returns to range, or you reposition.
