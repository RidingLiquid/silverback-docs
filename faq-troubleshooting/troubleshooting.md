# Troubleshooting

Solutions to common issues on Silverback DEX.

***

## Transaction Failed

### Symptoms

* Transaction reverts
* Error message in wallet
* Swap doesn't complete

### Common Causes & Solutions

**Insufficient Gas**

* Ensure you have enough ETH (Base) or KTA (Keeta) for fees
* Base transactions typically need \~$0.01-0.10 in ETH

**Slippage Too Low**

* Click the gear icon ⚙️
* Increase slippage tolerance (try 1-3%)
* For volatile tokens, you may need 5%+

**Token Approval Needed**

* First time trading a token requires approval
* Look for "Approve" button before "Swap"
* This is a separate transaction

**Price Moved**

* The price changed between quote and execution
* Increase slippage tolerance
* Try again — prices update in real-time

**Insufficient Balance**

* Verify you have enough of the input token
* Remember to keep some ETH/KTA for gas

***

## Swap Issues

### "Insufficient Liquidity"

**Problem:** Not enough tokens in the pool for your trade.

**Solutions:**

* Reduce your swap amount
* Try a different trading pair
* On Keeta: Check Anchor page for alternative pools
* Split into multiple smaller trades

### "Price Impact Too High"

**Problem:** Your trade is large relative to pool liquidity.

| Price Impact | Recommendation             |
| ------------ | -------------------------- |
| 1-3%         | Acceptable for most trades |
| 3-5%         | Consider reducing size     |
| 5-10%        | Split into multiple trades |
| >10%         | Find more liquid pair      |

**Solutions:**

* Reduce trade size
* Route through intermediate tokens
* Wait for more liquidity
* Use a different pool

### "Rate Expired"

**Problem:** Quote is no longer valid.

**Solution:** Click to refresh the quote and try again.

***

## Wallet Issues

### Wallet Not Connecting

**MetaMask / Web3 Wallets:**

1. Refresh the page
2. Make sure wallet is unlocked
3. Check you're on the correct network
4. Try disconnecting and reconnecting
5. Clear browser cache if issues persist

**Keythings (Keeta):**

1. Ensure extension is installed
2. Access DEX at `localhost:3000` (required)
3. Check extension is enabled in browser
4. Refresh the page
5. Verify wallet is unlocked

### Wrong Network

**Symptoms:** Tokens not showing, transactions fail

**Solution:**

1. Check network dropdown in top nav
2. Switch to correct network (Base or Keeta)
3. Ensure wallet is on same network
4. For Base: Add network if needed (Chain ID: 8453)

### Connection Keeps Dropping

**Solutions:**

* Update your wallet extension
* Disable other browser extensions temporarily
* Try a different browser
* Clear wallet cache/activity data

***

## Liquidity Issues

### Can't Add Liquidity

**"Insufficient Balance"**

* Verify you have both tokens
* You need equal _value_ of each token

**"Approve Token"**

* Approve each token before adding liquidity
* Two approval transactions needed (one per token)

### Can't Remove Liquidity

**"No Position Found"**

* Make sure you're on the correct network
* Check Portfolio page for your positions
* Verify wallet is connected

**LP Tokens Missing**

* LP tokens may be in a different wallet
* Check you didn't transfer them
* Contact support if tokens are genuinely missing

***

## Anchor Pool Issues (Keeta)

### Pool Creation Failed

**Common Causes:**

* Pool already exists for this pair
* Insufficient token balance
* Not enough KTA for fees (need 3-5 KTA)

**Solutions:**

* Check "My Anchors" for existing pools
* Verify both token balances
* Ensure adequate KTA for transactions

### LP Tokens Not Received

**Problem:** Pool created but LP tokens didn't arrive.

**Solutions:**

1. Wait a moment — may take time to process
2. Refresh the page
3. Check your wallet for the LP token
4. If still missing, contact support with transaction hash

### Pool Not Showing in Aggregator

**Possible Causes:**

* Pool is paused
* Indexing delay (wait a few minutes)
* Pool has zero liquidity

**Solutions:**

* Check pool status in "My Anchors"
* Ensure pool is active (not paused)
* Verify liquidity is deposited

***

## Performance Issues

### Slow Loading

* Check your internet connection
* Try refreshing the page
* Clear browser cache
* Disable unnecessary browser extensions

### Charts Not Loading

* Refresh the page
* Wait a moment — data may be loading
* Try a different browser

### Stuck Transactions

**On Base:**

1. Check BaseScan for transaction status
2. If pending long, may need to speed up or cancel in wallet
3. MetaMask: Settings → Advanced → Reset Account (clears pending)

**On Keeta:**

* Keeta transactions are fast (\~400ms)
* If stuck, refresh and check transaction history
* Contact support if genuinely stuck

***

## Still Need Help?

If these solutions don't resolve your issue:

1. **Search our FAQ** — [FAQ Page](faq.md)
2. **Ask on Discord** — Community support (coming soon)
3. **Tweet at us** — [@AgentSilverback](https://twitter.com/AgentSilverback)
4. **Open GitHub Issue** — Open Soon

**When reporting issues, include:**

* Wallet address (public is fine)
* Transaction hash
* Network (Base/Keeta)
* What you were trying to do
* Error message received
* Screenshots if helpful
