# TTTpulse – Advanced Workflows

Detailed decision logic, scoring systems, and step-by-step workflows used by TTTpulse.  
All calculations are fully based on real Bankr API data + available market data.

---

## 1. Sentiment Score System (0–100)

### Weighted Components

| Component                  | Weight | Calculation Method                                      | Score Range |
|---------------------------|--------|---------------------------------------------------------|-------------|
| Fee Momentum              | 30%    | Trend slope of dailyEarnings (last 7–14 days)           | 0–100      |
| Volume Quality & Strength | 25%    | 24h volume vs 7d average + organic behavior             | 0–100      |
| Holder / On-chain Health  | 20%    | Holder growth or stability + concentration signals      | 0–100      |
| Price-Fee Alignment       | 15%    | Correlation between price action and fee generation     | 0–100      |
| Liquidity Stability       | 10%    | Liquidity / Market Cap ratio + recent changes           | 0–100      |

**Final Sentiment Score** = weighted average

### Score Labels
- **75–100** → Strongly Bullish
- **60–74**  → Bullish
- **45–59**  → Neutral
- **30–44**  → Bearish
- **0–29**   → Strongly Bearish

---

## 2. Narrative Strength Classification

| Level       | Conditions                                                                 |
|-------------|----------------------------------------------------------------------------|
| **Strong**  | Rising fee momentum + strong volume quality + positive price-fee alignment + healthy liquidity |
| **Moderate**| Mixed signals (some positive, some neutral/weak)                           |
| **Weak**    | Declining fees + weak/low quality volume + poor alignment or thin liquidity |

---

## 3. Core Analysis Workflows

### A. Full Single-Token Pulse (Default)
1. Resolve token name → address
2. Fetch Bankr fees endpoint (`days=30` or requested)
3. Fetch market data (price, volume, liquidity, holders)
4. Calculate each component score
5. Compute final Sentiment Score
6. Determine Narrative Strength
7. Generate Key Pulse Drivers
8. Output complete Pulse Report + actionable insight

### B. Fee Momentum Deep Dive
- Analyze `dailyEarnings` array
- Calculate short-term slope (last 3–7 days vs previous period)
- Detect acceleration, deceleration, or collapse
- Label as Rising / Stable / Declining / Volatile

### C. Volume Quality Assessment
- Compare 24h volume vs 7-day average
- Check for sudden spikes vs sustained volume
- Evaluate if volume supports price and fee growth
- Label: High / Medium / Low quality

### D. Portfolio Pulse Mode
- Trigger: “my portfolio pulse”, “sentiment of my tokens”
- Use creator-fees endpoint
- Calculate weighted average Sentiment Score across tokens
- Highlight strongest and weakest pulse tokens
- Show concentration of positive/negative sentiment

### E. Comparison Mode
- Support 2–3 tokens side-by-side
- Compare Sentiment Score, Narrative Strength, Fee Momentum, Volume Quality
- Declare clear pulse leader

---

## 4. Confidence Level Logic

- **High**: All major data points available + clear trends
- **Medium**: Some data missing or mixed signals
- **Low**: Limited data (new token, sparse dailyEarnings, or missing market metrics)

Always display Confidence together with the Sentiment Score.

---

## 5. Cross-Skill Priority

When other TTT skills are available in the conversation:
1. TTTpulse provides the “mood / narrative” layer
2. Combine with TTTsignal for direction
3. Combine with TTTrisk for safety
4. Feed into TTTstrat for executable strategy

TTTpulse never overrides high risk warnings from TTTrisk.

---

## 6. Decision Flow (Simplified)

```
Start
├── Fetch Bankr fees + market data
├── Calculate 5 component scores
├── Compute Sentiment Score (0–100)
├── Determine Narrative Strength
├── Assess Confidence
├── Generate drivers + insight
└── Output full Pulse Report + Quick Actions
```

---

This file is the core intelligence engine of TTTpulse.  
Keep it updated when better on-chain or social data sources become available.
```

---
