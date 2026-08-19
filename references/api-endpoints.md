# TTTpulse – API Endpoints & Data Sources

Official and preferred data sources used by TTTpulse.  
Always follow this priority order. Never invent or estimate missing values.

---

## 1. Bankr Official Endpoints (Highest Priority)

### Token Fees & Historical Earnings (Most Important for Fee Momentum)
```
GET https://api.bankr.bot/token-launches/{tokenAddress}/fees?days={1-90}
```
- Default: `days=30`
- Critical field: `dailyEarnings` array (used for fee momentum & sentiment scoring)
- Legacy (deprecated but still works):  
  `GET https://api.bankr.bot/public/doppler/token-fees/{tokenAddress}?days=30`

### Creator / Wallet Portfolio Fees
```
GET https://api.bankr.bot/public/doppler/creator-fees/{walletAddress}?days=30
```
- Use for “my portfolio pulse” requests

### Quick Claimable Check
```
GET https://api.bankr.bot/public/doppler/claimable-fees/{tokenAddress}?beneficiary={walletAddress}
```

### Recent Token Launches
```
GET https://api.bankr.bot/token-launches
```

### Agent Profiles
```
GET https://api.bankr.bot/agent-profiles?sort=marketCap&limit=20
GET https://api.bankr.bot/agent-profiles/{slug-or-address}
GET https://api.bankr.bot/agent-profiles/{id}/llm-usage?days=30
```

**Notes from Bankr:**
- Public fee endpoints require no authentication
- Responses are cached server-side for approximately 2 minutes
- `days` parameter accepts values from 1 to 90
- Always convert WETH → approximate USD using current ETH price
- Response includes `chain` field (`base` or `robinhood`)

---

## 2. Market & On-chain Data Sources

Preferred order:
1. GeckoTerminal
2. DexScreener
3. Birdeye
4. Zerion / Alchemy
5. CoinGecko (fallback)

Required metrics for pulse analysis:
- Current price
- 24h and 7d price change
- 24h volume
- Liquidity (USD)
- Market Cap
- Holders count (when available)
- Liquidity / Market Cap ratio (calculated)
- Volume trend vs recent average

---

## 3. Cross-Skill Data (When Available)

TTTpulse should reuse latest results from:
- **TTTsignal** → direction and technical momentum
- **TTTrisk** → risk environment and liquidity health
- **TTTracker** → detailed fee history and claimable status
- **TTTstrat** → current strategy context

If these skills have already produced output in the same conversation, prefer reusing that data.

---

## 4. Data Handling Rules

- Never hallucinate sentiment scores, holder numbers, or volume figures
- If critical data is missing → clearly state “Limited data” and reduce Confidence
- Always show both WETH and approximate USD values
- Detect and display the correct chain (Base or Robinhood Chain)
- Cache fetched data for 1–2 minutes inside the same conversation
- For fee momentum: prioritize the `dailyEarnings` array

---

## 5. Recommended Fetch Sequence for Full Pulse

1. Resolve token name → contract address (if needed)
2. Fetch Bankr fees endpoint (`days=30` or user-requested period)
3. Fetch market data (price, volume, liquidity, holders)
4. Pull latest TTTsignal / TTTrisk results if available
5. Calculate component scores and final Sentiment Score
6. Generate full Pulse Report

---

This file must stay synchronized with the latest Bankr public API documentation.  
Update immediately when new useful endpoints become available.
```
