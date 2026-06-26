---
title: "Chart Patterns — Reversal and Continuation"
status: growing
created: 2026-04-29
updated: 2026-04-29
tags: [mechanism, trading, chart-patterns, murphy, day-swing-applicable]
---

# Chart Patterns — Reversal and Continuation

> **TL;DR**: Murphy's Chs 5–6: the canonical taxonomy of chart formations that signal trend reversals (head-and-shoulders, double tops/bottoms, triple tops/bottoms, V-tops/spikes, saucers) versus continuation pauses (triangles, flags, pennants, wedges, rectangles). Each pattern has a measurable price target derived from the pattern's height. Honest caveat: empirically, pattern completion rates are above random but not by huge margins — patterns are *suggestive*, not deterministic.

## Two Categories

| Reversal patterns | Continuation patterns |
|-------------------|------------------------|
| Signal end of existing trend | Signal pause within existing trend |
| Form at trend exhaustion | Form during trend |
| Larger and longer-lasting | Smaller and shorter |
| Volume usually declines into the pattern | Volume usually declines into the pattern |
| Examples: H&S, doubles, triples, V-tops, saucers | Examples: triangles, flags, pennants, wedges, rectangles |

Both categories share two structural features that come from [[trading/mechanisms/support-resistance]]:
- A horizontal level (neckline, top, base) acting as the breakdown trigger
- A measurable height that sets the price target after breakout

## Reversal Patterns

### Head and Shoulders (H&S) — The Most-Famous Reversal

Structure (top variant):
- **Left shoulder** — peak followed by pullback
- **Head** — higher peak followed by pullback
- **Right shoulder** — lower peak (lower than head, often near left shoulder)
- **Neckline** — line connecting the two pullback troughs

Signal: when price breaks below the neckline on volume, the pattern is confirmed.

> "This major reversal pattern, like all of the others, is just a further refinement of the concepts of trend covered in Chapter 4. Picture a situation in a major uptrend, where a series of ascending peaks and troughs gradually begin to lose momentum. The uptrend then levels off for awhile. During this time the forces of supply and demand are in relative balance. Once this distribution phase has been completed, support levels along the bottom of the horizontal trading range are broken and a new downtrend has been established."
> — Murphy, Ch 5

Price target rule: measure the height from the head to the neckline; project that distance below the breakout point. The mechanism: the pattern represents a finite quantity of distribution; once exhausted, the prior accumulation gets unwound at roughly the same magnitude.

**Inverse H&S** (bottom variant): same structure inverted, signaling end of downtrend.

**Caveat from Murphy himself**: H&S can occasionally act as a *consolidation* rather than reversal pattern. About 80% of completed H&S formations reverse the trend; ~20% fail. Always wait for the neckline break — the unconfirmed pattern is just three peaks.

### Double Tops / Double Bottoms

Structure (double top):
- First peak after extended uptrend, pullback
- Second peak at approximately the same level, pullback
- Pullback breaks the prior trough → confirmation

Volume usually lower on the second peak than the first (a divergence with price — see [[trading/mechanisms/oscillators-divergence]]).

Price target: distance from peak to intervening trough, projected below the trough.

**Triple tops / triple bottoms**: same logic with three peaks/troughs at similar levels. Less common but stronger when they form (more rejection of the level = more selling/buying pressure ready to act).

### V-Tops and Spikes

Sharp reversal without a distribution phase. Common at:
- News-driven climaxes
- Short-squeeze tops
- Capitulation bottoms

Hardest to trade because there's no formation period to confirm — by the time you recognize a V-top, you're already mid-reversal. Murphy treats these as more cautionary than tradable.

### Saucer Bottoms / Rounded Bottoms

Slow, gradual change of trend over many bars. Often seen in low-volatility stocks at bear market lows. Volume mirrors the price (low at the bottom, increasing as the saucer turns up).

Less common in crypto due to higher volatility — V-shapes and double-bottoms dominate.

## Continuation Patterns

### Triangles — Three Variants

| Triangle type | Resistance line | Support line | Bias |
|---------------|-----------------|--------------|------|
| **Symmetric** | Descending | Ascending | Neutral; breakout direction usually = prior trend |
| **Ascending** | Horizontal | Ascending | Bullish |
| **Descending** | Descending | Horizontal | Bearish |

Mechanism: the triangle represents converging supply and demand. The flat side (resistance for ascending, support for descending) is the side under attack — when price breaks the flat line, the trend resumes in that direction.

Volume should decline as the triangle forms (consolidation = decreased participation), then expand on breakout.

Price target: project the height of the triangle's *base* (the widest part) from the breakout point.

### Flags and Pennants

Short-term continuation patterns that form after sharp moves. Pause for ~1–3 weeks, then resume the prior trend.

- **Flag**: rectangular pause, sloping against the prior trend
- **Pennant**: small symmetric triangle

Both signal "the trend is resting." Most common in fast-moving markets — including crypto. The bull flag is the most-recognized continuation pattern in crypto trader culture.

Price target: typically the height of the "flagpole" (the prior sharp move) projected from the breakout.

### Wedges

Similar to triangles but both lines slope in the same direction:
- **Rising wedge** — both lines slope up; bearish (despite rising)
- **Falling wedge** — both lines slope down; bullish

Wedges are *narrowing* and *sloping*, distinguishing them from channels. They're frequently confused with triangles — the slope direction is what differentiates.

### Rectangles

Trading range / consolidation. Multiple touches of horizontal support and resistance lines. Price target on breakout: the height of the rectangle.

These overlap with simple support/resistance (a rectangle is just a defined trading range with multiple visits).

## The Empirical Reality (Honest)

Bulkowski's *Encyclopedia of Chart Patterns* gives empirical completion rates from large-sample backtesting:

| Pattern | Approx. completion rate (target reached) |
|---------|------------------------------------------|
| Head and shoulders top | ~60-70% |
| Double bottom | ~65-70% |
| Triangle (symmetric) | ~50-55% |
| Bull flag | ~60-65% |
| Falling wedge | ~70-80% (one of the most reliable) |

Most patterns are above the 50% null rate but not dramatically. The mechanism is real but probabilistic. The error: betting on a single pattern as if it's deterministic.

The right framing: patterns are *prior probability shifts*, not certainties. A confirmed bull flag is maybe 65% likely to reach its target. Position sizing must account for the 35% miss.

## Crypto Application

Chart patterns work in crypto, often *more cleanly* than in equities, because:

1. **Less algorithmic intervention** — equity markets have HFT smoothing that mutes patterns; crypto has less of this on retail-dominated flow
2. **More liquid round-number anchors** — patterns at round numbers in BTC/ETH have particularly strong follow-through
3. **24/7 price discovery** — patterns develop continuously rather than gapping over weekends
4. **Visible structural breaks** — liquidation cascades create cleaner V-tops and spikes than equities exhibit

But also:
- **Higher false-breakout rate on low-cap alts** — manipulation breaks patterns
- **News risk is constant** (24/7 markets) — a pattern can be invalidated by an Asia-session announcement
- **Pattern self-defeat is stronger in crypto** — Crypto Twitter teaches the same patterns to millions; well-known patterns get front-run

### Crypto-specific patterns Murphy doesn't cover

These are crypto-native and worth flagging for future ingest:
- **Liquidation hunt** — sharp wick to liquidation cluster, immediate reversal (Lefèvre's "stop hunting" applies)
- **Funding flip pattern** — sharp move + funding flip from positive to negative (or vice versa) → reversal probability rises
- **Open-interest divergence** — price rising but OI falling = short squeeze ending
- **CEX/DEX volume divergence** — flow regime change

These build on Murphy's framework but aren't in the source itself. They belong in future trading/mechanisms/ pages.

## How To Use This Mechanism

For the day/swing trader:
1. Identify the prevailing trend (per [[trading/mechanisms/support-resistance]])
2. Watch for pattern formations during pauses
3. Prefer continuation patterns (in trend direction) over reversal patterns (against trend) — better risk:reward
4. Wait for the *confirmed* breakout (volume + clear range exit), not the partial pattern
5. Place stop just beyond the pattern's invalidation point
6. Take target at the measured-move objective

The pattern itself is just an *entry framework*. Position sizing, stops, and exits are still determined by [[trading/mechanisms/support-resistance|support/resistance levels]] and risk management.

## Regime Applicability

| Regime (per [[foundation/macro/btc-regime-taxonomy-2020-2025\|BTC taxonomy]]) | Mechanism state | Evidence |
|---|---|---|
| Cycle peak rejection / bearish (e.g. 2025-Q4) | Not yet tested in pipeline | — |
| Bearish continuation (e.g. 2026-Q1) | Not yet tested in pipeline | — |
| Bullish trending | Not yet tested in pipeline | — |
| Chop / transitional | Not yet tested in pipeline | — |
| Cascade aftermath | Not yet tested in pipeline | — |

**Notes:**
- Mechanism is **regime-aware**: reversal patterns (H&S, double tops) work in trending regimes about to turn; continuation patterns (flags, triangles) work in established trends; both fail in chop
- Required regime markers for productionize: clear regime classification (trending vs chop) + pattern-classification confluence
- Anti-regime conditions: whipsaw / chop regimes invalidate most chart patterns; high-vol cascades mask patterns

This mechanism lacks direct backtest evidence in the 2026-05-05 batch. Per [[foundation/macro/regime-conditional-edge-2025-26|W2 finding]]: 13/13 strategies in the cohort showed regime-conditional edge concentrated in 2025-Q4 + 2026-Q1; this page should be revisited when strategies in this mechanism class enter pipeline testing.

## Key Takeaways

- Two categories: reversal (end of trend) and continuation (pause within trend)
- Each pattern has a measurable price target derived from pattern height
- Empirical completion rates are 50-80% — above random but not deterministic
- H&S, double tops, and triangles are the most common; wedges have the highest completion rates
- Volume should decline through the pattern's formation and expand on breakout — failed volume confirmation is a red flag
- Crypto patterns work cleanly but with crypto-specific failure modes (manipulation, liquidation cascades, self-defeating popularity)
- Patterns are *probabilistic*, not deterministic — size accordingly

## Related

- [[foundation/market-structure/crypto-carrier-features]] — central catalog of crypto-native carriers; chart patterns gain crypto-specific reliability when forming around carrier-anchored levels (URPD peaks, liquidation clusters, options strikes) rather than arbitrary price points
- [[foundation/sources/murphy-technical-analysis]] — source
- [[trading/mechanisms/support-resistance]] — patterns form at and reference support/resistance levels
- [[trading/mechanisms/pivotal-points]] — patterns at pivotal levels have stronger structural backing
- [[trading/mechanisms/volume-analysis]] — volume confirmation is essential for pattern validity
- [[trading/mechanisms/oscillators-divergence]] — oscillator divergence within a pattern strengthens the signal
- [[foundation/market-wisdom/dow-theory-and-ta-premises]] — Dow's accumulation/distribution phases are the framework patterns operationalize
- [[glossary/breakout]] — the confirmation event for any pattern

## Open Questions

- **Which Murphy patterns have the highest empirical completion rate in crypto specifically?** Worth backtesting; equities-derived completion rates may not transfer cleanly.
- **Does the well-known-pattern self-defeat effect operate in crypto?** Hypothesis: yes for top-3 patterns (H&S, doubles, bull flags); less for lesser-known patterns (wedges, saucers).

## Sources

- Murphy, John J. *Technical Analysis of the Financial Markets*, Chs 5 ("Major Reversal Patterns") and 6 ("Continuation Patterns"), 1999.
- For empirical rates: Bulkowski, Thomas N. *Encyclopedia of Chart Patterns* (referenced; not in wiki).
