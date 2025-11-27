# FAQ

Frequently asked questions about Silverback DEX.

***

## General

### What is Silverback DEX?

Silverback is a dual-network decentralized exchange supporting Base (Ethereum L2) and Keeta (high-speed DAG blockchain). It features AMM pools, FX anchor trading, and is powered by an autonomous AI trading agent.

### What makes Silverback different?

Unlike other AI agents that just provide commentary, Silverback _operates_ actual trading infrastructure:

* Runs a live DEX
* Trades with real capital
* Shares profits with token holders
* Proves expertise through results, not claims

### Which network should I use?

**Base** is best for:

* Trading popular ERC-20 tokens
* Accessing deep liquidity
* Users already in the Ethereum ecosystem

**Keeta** is best for:

* Fast transactions (400ms settlement)
* Lower fees
* Creating custom liquidity pools with your own fee rates

### Is Silverback safe to use?

Silverback is non-custodial — you maintain full control of your funds. The smart contracts follow battle-tested Uniswap V2 standards. However, as with all DeFi:

* Never invest more than you can afford to lose
* Understand risks like impermanent loss
* Always verify contract addresses

### Does Silverback custody my funds?

No. This is a fully non-custodial DEX. Your tokens remain in your wallet until you execute a swap, and liquidity can be withdrawn anytime.

***

## Trading & Fees

### What are the trading fees?

| Type         | Fee         | Goes To             |
| ------------ | ----------- | ------------------- |
| AMM Swaps    | 0.3%        | Liquidity Providers |
| Anchor Swaps | 0.01% - 10% | Pool Creator        |
| Gas          | Variable    | Network validators  |

### What is slippage tolerance?

The maximum price change you'll accept between submitting a transaction and it executing. If price moves more than your tolerance, the transaction fails, protecting you from bad fills.

Recommended settings:

* Stable pairs: 0.1%
* Normal pairs: 0.5%
* Volatile tokens: 1-3%

### Why do I need to approve tokens?

ERC-20 tokens require you to grant permission before a smart contract can spend them. This is a security feature. You only need to approve once per token per contract.

***

## Liquidity

### What are LP tokens?

LP (Liquidity Provider) tokens represent your share of a liquidity pool. They:

* Prove your deposit
* Entitle you to accumulated fees
* Can be redeemed to withdraw liquidity
* Should be kept safe like any crypto asset

### How do I earn fees as a liquidity provider?

When traders swap through a pool, a 0.3% fee is collected. This fee is automatically added to the pool. When you withdraw, you receive your proportional share of all accumulated fees.

### What is impermanent loss?

A temporary loss that occurs when token prices change from when you deposited. If you withdraw when prices have diverged significantly, you may have less value than if you just held the tokens.

Key points:

* Called "impermanent" because it reverses if prices return to original ratio
* Trading fees can offset impermanent loss
* More volatile pairs have higher IL risk

### Can I remove liquidity anytime?

Yes! Liquidity is never locked. Withdraw whenever you want and receive tokens proportional to your pool share.

***

## Anchor Pools (Keeta)

### What's the difference between AMM pools and Anchor pools?

| Feature     | AMM Pools    | Anchor Pools        |
| ----------- | ------------ | ------------------- |
| Fee         | Fixed 0.3%   | Custom 0.01%-10%    |
| Creator     | Anyone       | Anyone              |
| Competition | Within pool  | Against all anchors |
| Network     | Base & Keeta | Keeta only          |

### How do I make money with anchor pools?

Earn fees when swaps are routed through your pool. The Anchor page automatically selects the best rate, so competitive fees and good liquidity attract more volume.

### What fee should I set?

Check existing pools for your token pair. Common strategies:

* 10-20 bps — Aggressive volume play
* 30 bps — Standard, competitive
* 50-100 bps — Higher margin, less volume

### Can I have multiple anchor pools?

Yes! Create one pool per token pair. You cannot have two pools for the same pair from the same wallet.

### Can others provide liquidity to my pool?

Not currently. Each user creates and manages their own anchor pools independently.

***

## $BACK Token

### What is $BACK?

$BACK is the native token of the Silverback ecosystem, launched through Virtuals Protocol. It provides:

* Revenue sharing from DEX operations and trading
* Staking rewards
* Governance participation

### How do I earn with $BACK?

Stake $BACK to receive a share of protocol revenue. Revenue from DEX fees and AI trading profits is used to buy back $BACK, which fills the staking reward pool.

### Is there token inflation?

No. $BACK doesn't mint new tokens for rewards. Instead, real revenue funds buybacks that distribute existing tokens. This creates sustainable yield without dilution.

### Where can I buy $BACK?

$BACK launches on Virtuals Protocol. After launch, it will be tradeable on Silverback DEX and potentially other platforms.

***

## Technical

### What wallets are supported?

**Base Network:**

* MetaMask
* Coinbase Wallet
* WalletConnect (300+ wallets)
* Rainbow
* And more

**Keeta Network:**

* Keythings (required)

### What are the contract addresses?

See [Contract Addresses](../resources/contract-addresses.md) for all verified contracts.

## Security

### Has Silverback been audited?

We conducted comprehensive pre-mainnet testing on Base Sepolia with 100+ automated tests, including edge cases like tax tokens and reentrancy attacks. All contracts are verified on Basescan and include built-in protections like reentrancy guards, deadline checks, and slippage protection. While we haven't undergone an independent security audit, our foundation is the most proven AMM design in DeFi history. Use at your own risk,\
as with all smart contract protocols.

### What if I lose my LP tokens?

LP tokens are like any crypto asset. Losing them means losing access to your liquidity. Always:

* Back up your wallet recovery phrase
* Don't share private keys
* Keep LP tokens secure

### How do I verify I'm on the real site?

* Official domain: silverbackdefi.app
* Always bookmark official links
* Verify contract addresses before interacting
* Never click links from unknown sources

***

## Need More Help?

* **Discord**: Coming soon
* **Twitter**: [@SilverbackDefi](https://x.com/SilverbackDefi)
* **GitHub**: Open soon
