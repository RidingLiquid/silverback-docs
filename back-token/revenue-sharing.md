# Revenue Sharing

How Silverback protocol revenue flows back to $BACK holders.

---

## Revenue Model

Silverback generates revenue from multiple sources, all contributing to holder benefits:

```
┌─────────────────────────────────────────────┐
│              REVENUE SOURCES                │
├─────────────────────────────────────────────┤
│  DEX Fees (Base & Keeta)                    │
│  + Treasury Trading Profits                 │
│  + x402 API Settlement Fees                 │
│  + Cross-Chain Arbitrage                    │
│  + Future Revenue Streams                   │
└─────────────────────────────────────────────┘
                    ↓
            Protocol Treasury
                    ↓
         $BACK Market Buybacks
                    ↓
          Staking Reward Pool
                    ↓
         Distributed to Stakers
```

---

## Revenue Sources

### 1. DEX Protocol Fees

Every swap on Silverback DEX generates fees:

| Network | Fee | LP Share | Protocol Share |
|---------|-----|----------|----------------|
| Base | 0.3% | Majority | Portion to treasury |
| Keeta AMM | 0.3% | Majority | Portion to treasury |
| Keeta Anchors | Variable | Pool creator | Small protocol cut |

The protocol's share of fees contributes to buybacks.

### 2. Treasury Trading Profits

The Silverback AI agent actively trades with treasury capital:

- Systematic, data-driven strategies
- Strict risk management (max 2% per trade)
- Multiple strategies: arbitrage, perpetuals, liquidity provision
- Profits contribute to buyback pool

### 3. x402 API Fees

Silverback's [paid intelligence API](../silverback-agent/x402-payments.md) generates revenue from every request:

- 16+ paid endpoints covering market data, trading tools, and DeFi analytics
- Settlement fees of 0.1% (stablecoins) to 0.25% (blue-chip assets) on every payment
- $BACK payments are fee-exempt — encouraging $BACK adoption while other tokens generate protocol revenue
- All fees route through the on-chain fee splitter to the treasury

### 4. Cross-Chain Arbitrage

Operating across Base and Keeta enables arbitrage opportunities:

- Price discrepancies between networks
- Atomic execution reduces risk
- AI agent identifies and executes opportunities
- Profits flow to treasury

### 5. Future Revenue Streams

As Silverback expands:
- Additional network deployments
- Fiat on-ramp fees
- Premium features
- Partnership revenue

---

## Buyback Mechanism

### How It Works

1. **Revenue Accumulates** — From all sources above
2. **Buybacks Execute** — Treasury purchases $BACK from market
3. **Pool Fills** — Bought tokens enter staking reward pool
4. **Stakers Earn** — Rewards distributed to stakers

### Why Buybacks?

| Approach | Effect |
|----------|--------|
| Mint new tokens | Dilutes holders, inflation |
| Distribute revenue | Taxable event, fragmented |
| **Buybacks → Staking** | Buy pressure + sustainable rewards |

Buybacks create natural demand while providing real yield to stakers.

---

## Transparency

### On-Chain Verification

All revenue and buybacks are verifiable on-chain:

- Treasury wallet publicly viewable
- Buyback transactions traceable
- Staking pool balances visible
- Distribution history available

### Reporting

Silverback provides regular updates on:

- Trading performance (without revealing strategies)
- DEX volume and fees generated
- Buyback amounts and timing
- Staking pool growth

---

## Sustainable Economics

### The Problem with Most DeFi

Many protocols:
- Promise high APY through inflation
- Mint tokens faster than they create value
- Result: Token price declines, real returns negative
- Unsustainable "ponzinomics"

### Silverback's Approach

- **Revenue-backed rewards** — Only distribute what's earned
- **No inflation** — Fixed supply, buybacks only
- **Real yield** — Returns from actual protocol usage
- **Aligned incentives** — Team succeeds when holders succeed

---

## Estimating Returns

Your share of revenue depends on:

| Factor | Impact |
|--------|--------|
| Protocol Revenue | More revenue = larger buybacks |
| Your Stake % | Larger stake = larger share |
| Total Stakers | More stakers = more division |
| Buyback Timing | Market conditions affect quantities |

**Note:** Returns will vary. Higher protocol adoption and trading volume generally leads to better staking returns.

---

## FAQ

**How often do buybacks happen?**
Schedule TBD — likely based on accumulated revenue thresholds or time intervals.

**Can I see buyback transactions?**
Yes, all transactions will be visible on-chain and reported.

**What if trading has losses?**
The AI agent uses strict risk management. Losses reduce buyback amounts but don't create obligations for holders.

**Is this sustainable long-term?**
Yes — rewards scale with actual revenue. No promises of fixed APY, just fair share of real profits.

**Do I need to stake to benefit?**
Buybacks benefit all holders through buy pressure. Staking lets you also earn direct rewards from the pool.
