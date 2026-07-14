---
title: "Support, Resistance, and Trends"
status: growing
created: 2026-04-29
updated: 2026-04-29
tags: [mechanism, trading, support-resistance, trend, murphy, day-swing-applicable]
---

# Support, Resistance, and Trends

> **TL;DR**: The most foundational tradable mechanism in technical analysis. Trends are sequences of higher highs + higher lows (uptrend) or lower highs + lower lows (downtrend). Support is a price level where buying pressure has historically halted declines; resistance is the inverse. The mechanism works because of memory — past large transactions at a price level create asymmetric supply/demand at that level when price returns. Direct, well-tested, and the cleanest crypto application of any Murphy mechanism.

## Hypothesis (Structured Format)

```
Hypothesis:    Price levels at which significant transaction volume previously
               occurred show asymmetric reactions on subsequent visits, due to
               position-related memory of market participants.

Mechanism:     Participants who bought at a level (now-trapped longs after a
               drop) sell to break-even on the level's retest, creating
               resistance. Participants who sold or watched a level bounce now
               see it as a buy zone, creating support. The memory is real — it
               persists in trader behavior, position management, and stop
               placement.

Observable:    Multiple price reactions (rejection, consolidation) at the same
               or near-same price level. Volume profile shows transaction
               clusters at these levels.

Expected:      First retest of a level produces ~60-70% mean-reversion within
               1-3 bars (timeframe-dependent). Each subsequent retest weakens
               the level (memory decays).

Falsifier:     If retests produce random outcomes (50% pass-through vs.
               rejection), the mechanism is not present in the asset/regime.
```

## The Trend Definition

> "An uptrend is a series of successively higher peaks and higher troughs. A downtrend is just the opposite — a series of declining peaks and troughs."
> — Murphy, Ch 4

This is the **load-bearing definition** of all trend-based work. It's also the operational rule for "is the trend still in effect?":

| Pattern | Trend status |
|---------|-------------|
| Higher high + higher low | Uptrend continuing |
| Higher high + lower low | Indeterminate / consolidating |
| Lower high + higher low | Indeterminate / consolidating |
| **Lower high + lower low** | **Uptrend broken; downtrend may be starting** |
| Higher high after a downtrend | First higher high — signal, not confirmation |
| Higher low after the first higher high | Confirmation of trend reversal |

The mechanism converts a vague claim ("the trend reversed") into a measurable one ("the price made a lower low"). Tradable.

### Three trend timeframes

Per Dow's tenet 2 — trends nest:
- **Major trend** — months to years (BTC 4-year cycle, ETH cycle)
- **Intermediate trend** — weeks to months (alt season, distribution phase)
- **Minor trend** — days to weeks (the day/swing horizon!)

For the trader's day/swing trading horizon (≤1 week), the relevant trend is **minor within an intermediate**: identify the prevailing intermediate trend (BTC dominance regime, alt rotation phase) and trade minor reversals/continuations within it.

## Support and Resistance Mechanics

> "Support is a level or area on the chart under the market where buying interest is sufficiently strong to overcome selling pressure. As a result, a decline is halted and prices turn back up again. Usually a support level is identified beforehand by a previous reaction low. Resistance is the opposite of support."
> — Murphy, Ch 4

### Why levels work — the memory mechanism

When price reaches a previous support level after a decline:

- **Original buyers at that level** (the ones who bought the bounce) now see a confirmed pattern — they buy more
- **Original buyers above that level** (now underwater longs) sell into the rally to break-even — supply
- **Sellers who watched the previous bounce** wait for it to bounce again — they buy

The asymmetry: if a level is *strong* support, the buying outweighs the trapped-long selling. If it's *weak* support (or memory has decayed), the trapped-long selling dominates and the level breaks.

For resistance, the inverse logic applies.

### Role reversal

> "When prices break out of a previous trading range — that is, the range itself — that range will frequently be retested... The previous resistance will become the new support."
> — Murphy

When price breaks above resistance and pulls back, the former resistance often acts as new support. The mechanism: traders who didn't buy on the breakout get a "second chance" at what they perceive as the (formerly resistant) level.

This is one of the most-actionable patterns: **buy the retest of a broken resistance, with a stop just below it.** Risk:reward is naturally favorable.

### Strength factors (per Murphy)

How meaningful a level is depends on:
1. **Time spent at the level** — more time = more participants with memory
2. **Volume traded at the level** — more volume = more locked-in positions
3. **How recent the level is** — recent levels have stronger memory
4. **How many times the level has been tested** — first test is strongest; each successive test weakens the level (memory decays as trapped participants exit)
5. **Round numbers** (in equities; even more in crypto) — psychological anchors

## Trendlines

> "A trendline is a line drawn that connects either a series of highs or lows. The trendline can represent either support or resistance, depending on its position. Trendlines are very simple but among the most useful tools at the chartist's disposal."

Construction rules:
- Need **at least three touches** for a valid trendline
- Up-trendline: connects ascending lows; serves as support
- Down-trendline: connects descending highs; serves as resistance
- The angle / slope correlates with trend strength (steeper = stronger but more brittle)

### Channel trading

Two parallel trendlines bracket a trending market in a channel. Trading the channel: buy at the lower line, sell at the upper line, or trade breakouts on either side.

This is the canonical day/swing setup: identifiable, mechanical, with clear stops.

## The Line of Least Resistance (Wyckoff 1910, Livermore 1923)

Murphy's chart-pattern mechanics describe *the structure* of support and resistance. The pre-regulation tradition named *the operational principle* that determines when to use it: **the line of least resistance**. The concept is articulated in stereo by [[foundation/sources/wyckoff-method|Wyckoff]] and [[foundation/sources/lefevre-reminiscences|Lefèvre/Livermore]] — both observed the same NYSE phenomena and converged on the same metaphor.

**Wyckoff (1910, the earliest published articulation):**

> "When a stream breaks through a dam it goes into new territory. Likewise the breaking through of a stock is significant, for it means that the resistance has been overcome. The stronger the resistance the less likelihood of finding further obstacles in the immediate vicinity—dams are not usually built one behind the other." (Studies Ch V)

> "Prices follow the line of least resistance." (Studies Ch V)

**Livermore (1923, the better-known popularization):**

> "When the price line of least resistance is established I follow it, not because I am manipulating that particular stock at that particular moment but because I am a stock operator at all times." (Reminiscences Ch XXI)

> "Prices, like everything else, move along the line of least resistance." (Reminiscences Ch X)

Wyckoff's 1910 prose predates Livermore's published statements by 13 years; the concept may be Wyckoff's contribution that Livermore absorbed, or both may have derived it from the floor culture they shared.

The principle: when a stock has been ranging between two levels (resistance R, support S), one side will eventually overwhelm the other — accumulation at S exceeds distribution at R, or vice versa. The breakout direction *names* the line of least resistance. It encodes the cumulative result of all participants' beliefs and forced flows. Wyckoff's hydraulic metaphor (stream + dams) is the mechanistic version: dams aren't built one behind another, so once price breaks through a level of resistance, the path is comparatively clear until the next major level.

### Why this is more than chart-pattern restatement

Murphy's chart-pattern framework says "buy the breakout of resistance." Livermore's line-of-least-resistance principle says something deeper: **wait at the sidelines through the get-nowhere range; commit only when the line is decisively established**.

> "I don't buy long stock on a scale down, I buy on a scale up." (Ch VII)

The discipline is *not the breakout entry* (Murphy covers that). The discipline is *the patience to wait through the ranging market without being drawn into the noise*. Livermore: the Wall Street fool trades the range; the speculator waits for the line to be revealed and then trades the swing.

> "Disregarding the big swing and trying to jump in and out was fatal to me. Nobody can catch all the fluctuations. In a bull market your game is to buy and hold until you believe that the bull market is near its end." (Ch V)

### Operational implications

For the trader's day/swing horizon:

1. **In ranging markets, sit out.** No edge in random-walk reactions; commission and slippage are negative-EV.
2. **When the range is decisively pierced (with volume + macro alignment), commit.** This is the line of least resistance moment.
3. **Pyramid on confirmation** (per Livermore Ch VII; see also [[foundation/market-wisdom/sit-tight-discipline]]): commit 1/5 of planned line on the break, add more as the line continues.
4. **The line of least resistance is multi-timeframe.** A daily-chart line of least resistance can be up while a 4h is sideways. Pick your timeframe and commit to it.

### Connection to other mechanisms

Volatility-compression breakouts (Bollinger band squeeze, Donchian channel breakouts, ATR contraction → expansion) are quantitative restatements of the line-of-least-resistance principle — they identify ranges and signal commitment when the range breaks. The mechanism is the same; the instrumentation is more specific.

Crypto-specific: when funding is balanced, OI is rising, and volatility is compressed, the market is *loaded* for a one-sided unwind. The line of least resistance has structural backing (forced-flow asymmetry).

## Crypto Application — The Cleanest Mechanism

Support/resistance is arguably the *single best* technical mechanism for crypto, because:

1. **Round numbers carry exceptional weight in crypto** — BTC at $10,000, $20,000, $50,000, $100,000. ETH at $1,000, $2,000, $4,000. These levels are publicly debated and become self-fulfilling.

2. **On-chain data shows literal volume profile** — UTXO Realized Price Distribution (URPD) shows exactly how much BTC was bought at each price level. Levels with high URPD density are mechanically real support/resistance, not just chart-pattern guesswork.

3. **Liquidation maps add a second layer** — leveraged perpetual positions cluster at predictable levels (10x of round numbers; major MA levels). These create visible *liquidation clusters* that price often gravitates toward.

4. **Long-term holder cost basis acts as macro support** — LTH realized price (Glassnode metric) has historically been a hard floor in BTC bear markets.

5. **Less institutional smoothing than in equities.** Levels in equities can be muddied by algorithmic execution that minimizes footprint. Crypto markets are noisier but the levels are more visible.

### Concrete crypto support/resistance applications

| Layer | Source |
|-------|--------|
| **Round-number levels** | $10K / $50K / $100K BTC; $1K / $4K ETH |
| **Prior cycle highs/lows** | $69K (2021 BTC top); $4.8K (2021 ETH top); $20K (2017 BTC top, then 2022 bottom) |
| **On-chain URPD clusters** | Glassnode, Coinmetrics — actual transaction-volume distribution |
| **Liquidation clusters** | Coinglass — visible perp liquidation map |
| **MA-based dynamic support** | 200-day MA, 200-week MA — historically reliable bull-bear regime markers for BTC |
| **Long-term-holder cost basis** | Realized price by holder cohort; bear-market floor proxy |

## How To Use This Mechanism

For the day/swing trader:

1. **Identify the major trend** (intermediate timeframe) from higher highs/lows pattern
2. **Mark major support/resistance** on the daily/4h chart from prior swing points + round numbers + on-chain data
3. **Trade in the direction of the major trend** when price hits a counter-trend level (buy support in uptrend; sell resistance in downtrend)
4. **Place stops just beyond the level** — if support breaks, the level is *not* support after all
5. **Take profit at the next major level** in the direction of the trade — natural target, naturally bounded risk

This is the most-mechanical entry framework available. It's also the foundation of every other Murphy mechanism (chart patterns, MA crossovers, breakouts) — they all reference support/resistance levels.

## What Breaks This Mechanism

- **Low-cap alts** — manipulation overrides level memory; whales push price through levels at will
- **Major news events** — fundamentals override technical levels (the level is "respected" until news happens, then becomes irrelevant)
- **Excessive leverage in perps** — liquidation cascades push price through levels temporarily, even if the underlying spot demand is at the level
- **Regime changes** — when the macro regime shifts (Fed pivot, ETF approval, regulatory event), prior levels lose relevance until new ones form

## Test Plan / Operationalization

For backtesting on BTC perps (or any specific crypto market):

1. Define support/resistance levels objectively (e.g., swing highs/lows >5% in magnitude; round numbers; URPD clusters above some density threshold)
2. Identify retests of these levels post-formation
3. Measure: what % of retests produce mean-reversion >X% within Y bars?
4. Falsifier: if mean-reversion rate ≈ 50%, the mechanism doesn't operate in this market/regime
5. Refine: which level types have highest reaction rates? (likely: combined round-number + URPD cluster)

This is a structured-hypothesis exercise per Part I Section 4.

## Regime Applicability

| Regime (per [[foundation/macro/btc-regime-taxonomy-2020-2025\|BTC taxonomy]]) | Mechanism state | Evidence |
|---|---|---|
| Cycle peak rejection / bearish (e.g. 2025-Q4) | Not yet tested in pipeline | — |
| Bearish continuation (e.g. 2026-Q1) | Not yet tested in pipeline | — |
| Bullish trending | **Trend-following with S/R as continuation tests** | Inferred |
| Chop / transitional | **Most-active** — range-fade at S/R is canonical chop strategy | Inferred from textbook |
| Cascade aftermath | Untested — historical S/R blown through; new levels emerge | Theoretical |

**Notes:**
- Mechanism is **regime-conditional**: support/resistance levels hold in chop; trends break through them. Operationally: fade levels in chop; ride through levels in trend
- Required regime markers for productionize: regime classification (chop vs trend) + level-strength confluence (time + volume + recency + round-number proximity)
- Anti-regime conditions: trending regime where levels are being broken (S/R doesn't hold); cascade aftermath when historical S/R loses memory

This mechanism is foundational to many other mechanisms (pivotal-points, chart-patterns, fibonacci-retracements, accumulation-distribution); regime-applicability of its consumers applies to it transitively. Per [[foundation/macro/regime-conditional-edge-2025-26|W2 finding]]: 13/13 strategies in the cohort showed regime-conditional edge concentrated in 2025-Q4 + 2026-Q1; this page should be revisited when strategies in this mechanism class enter pipeline testing.

## Key Takeaways

- Trend = sequence of higher highs + higher lows (or inverse). Mechanical, observable.
- Support/resistance levels work because of position-related memory, not magic
- Strength factors: time + volume + recency + test count + round-number proximity
- Role reversal: broken resistance becomes support, broken support becomes resistance
- Crypto is the cleanest application of support/resistance work — round numbers, on-chain URPD, liquidation maps, LTH cost basis all add measurable layers
- The mechanism is a *base layer* — chart patterns, MA crossovers, breakouts all reference levels

## Related

- [[foundation/market-structure/crypto-carrier-features]] — central catalog of crypto-native carriers; URPD peaks, liquidation clusters, LTH/STH cost basis, MA-based dynamic levels are the carriers that translate generic S/R into crypto-tradable levels
- [[foundation/sources/murphy-technical-analysis]] — primary source for chart mechanics
- [[foundation/sources/wyckoff-method]] — earliest articulation of line of least resistance (Studies Ch V, 1910); points of resistance terminology; stream-and-dam metaphor
- [[foundation/sources/lefevre-reminiscences]] — Livermore's line-of-least-resistance popularization (Chs X, XXI, 1923) and the discipline of waiting through ranges
- [[trading/mechanisms/accumulation-distribution]] — Wyckoff's accumulation/distribution mechanism uses S/R levels as the boundary structure; spring/upthrust occur at well-defined S/R
- [[foundation/market-wisdom/dow-theory-and-ta-premises]] — Dow tenet 2 (three trend timeframes) is the framing
- [[foundation/market-wisdom/tape-reading]] — line of least resistance is determined by reading the tape's absorption behavior at the range extremes
- [[foundation/market-wisdom/sit-tight-discipline]] — the patience to wait through ranges without forcing trades
- [[trading/mechanisms/pivotal-points]] — round-number breakouts are the most-prominent form of line-of-least-resistance establishment
- [[trading/mechanisms/group-leader-divergence]] — multi-asset version of "which side has the captive flow"
- [[trading/mechanisms/chart-patterns]] — patterns are formed at support/resistance levels
- [[trading/mechanisms/moving-averages]] — MAs act as dynamic support/resistance
- [[trading/mechanisms/cyclical-timing]] — Lynch's cyclical mechanism uses support/resistance at the cycle level
- [[trading/mechanisms/volume-analysis]] — volume confirms or denies level reactions
- [[glossary/breakout]] — the event when price exits a range
- [[glossary/divergence]] — divergence is a leading indicator that often precedes level rejection

## Open Questions

- **What's the empirical reaction rate at each level type for major crypto pairs?** This is testable; would be a great early backtest.
- **Does on-chain URPD-cluster-based support outperform chart-derived support?** Hypothesis: yes, because URPD is less noisy.
- **Do liquidation clusters magnetize price through round-number support, only to bounce after the liquidation?** Common claim in crypto trader culture; deserves rigorous test.

## Sources

- Murphy, John J. *Technical Analysis of the Financial Markets*, Ch 4 ("Basic Concepts of Trends"), 1999.
