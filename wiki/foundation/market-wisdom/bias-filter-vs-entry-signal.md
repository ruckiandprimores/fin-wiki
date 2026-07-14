---
title: "Bias Filter vs Entry Signal — The Two-Type Signal Taxonomy"
status: growing
created: 2026-05-04
updated: 2026-05-04
tags: [foundation, market-wisdom, signal-design, methodology, bias-filter, entry-signal, day-swing-applicable]
---

# Bias Filter vs Entry Signal — The Two-Type Signal Taxonomy

> **TL;DR**: All trading signals fall into one of two categories: **bias filters** that gate *direction* without timing, and **entry signals** that time *entry* without gating direction. Most strategies that "work" combine both; most strategies that "fail" confuse the two and treat one as if it were the other. **Operational consequence**: every signal in a strategy must be classified as one or the other; if you can't say which, the signal is doing both jobs poorly. The counter-bias mirror test (running the strategy against its bias direction) is the empirical check that bias-filter and entry-signal components are properly decoupled.

## The Two Types

### Bias Filter

**What it does**: tells you which *direction* to trade in. Gates entries — only longs in bullish bias, only shorts in bearish bias.

**What it does NOT do**: tell you *when* to enter. A bias filter is a regime classifier that may stay in the same state for weeks or months. It doesn't fire entries; it gates them.

**Defining property**: a pure bias filter has no time information. It says "long bias" today, but doesn't say "enter long today." Entry timing comes from a separate signal applied within the bias regime.

**Examples in the wiki**:
- **Price vs 200-day MA** (per [[trading/mechanisms/moving-averages]]) — pure bias filter; tells you bull vs bear regime; doesn't tell you when to enter
- **BTC dominance regime** (rising vs falling per [[trading/mechanisms/group-leader-divergence]]) — bias for BTC vs alt
- **Bubble-cycle stage** (per [[foundation/market-wisdom/bubble-cycle-stages]]) — bias for which strategy *family* to use; doesn't trigger entries
- **LTH/STH cost basis regime** (per [[foundation/market-structure/crypto-carrier-features|carrier features]]) — bull vs bear bias from cohort positioning
- **Macro liquidity regime** — Fed easing vs tightening biases risk assets

### Entry Signal

**What it does**: tells you *when* to enter — a specific point in time at which a setup is active.

**What it does NOT do**: tell you which direction. Most entry signals are direction-blind — they fire at the same setup regardless of whether you should be long or short.

**Defining property**: an entry signal has explicit time information. It identifies a *moment* when conditions favor a trade. The trade direction must be supplied by the bias filter.

**Examples in the wiki**:
- **Wyckoff spring** (per [[trading/mechanisms/accumulation-distribution]]) — entry signal that fires when price wicks below support and recovers; tells you *when*; the long-bias is supplied by the accumulation-phase classification (which is itself a bias filter)
- **Failed-breakout reversal** (per [[trading/mechanisms/failed-breakout-reversal]]) — entry signal at the wick + reclaim; works long or short depending on which direction failed
- **RSI overbought/oversold** (per [[trading/mechanisms/oscillators-divergence]]) — entry signal at extremes; works as fade-the-extreme but the direction depends on bias context
- **Pivotal-point breakout** (per [[trading/mechanisms/pivotal-points]]) — entry signal at level cross; direction supplied by which level (resistance break = long; support break = short)
- **Volume + price expansion** — entry signal when volume confirms a move; direction independent of the signal

### The Hybrid Case

Some signals do both. **Trend breakouts** (per [[glossary/breakout]]) often function as both: the breakout direction supplies the bias (price exited resistance to the upside → bull bias), and the breakout event supplies the entry timing.

The honest framing: hybrid signals can work, but they entangle two jobs. When the strategy fails, you can't isolate which component (bias or timing) was wrong.

## Why The Distinction Matters

### The signal-confusion failure mode

Consider a strategy: "Buy when RSI < 30, sell when RSI > 70."

What is RSI doing here?
- It's being used as an *entry signal* (RSI < 30 fires the entry at a specific time).
- It's also being used as a *direction gate* (RSI > 70 means short / exit).

The strategy treats one indicator as both bias filter and entry signal. **This works in some regimes and fails catastrophically in others.**

In a *trending bull regime*, RSI < 30 rarely happens (the asset never gets oversold); when it does, it's near a local low and the trade works. RSI > 70 happens constantly; the strategy short-sells repeatedly into a trend that doesn't reverse. Heavy losses on the short side; small wins on the long side.

In a *ranging chop regime*, RSI oscillates between extremes; the strategy works on both sides.

The strategy *appears* to work on backtest data that includes both regimes. It *doesn't* work in pure trending or pure chop in isolation. The reason: RSI is being asked to do two jobs (bias + entry); it does neither well.

The fix: separate the two. Use a *bias filter* (e.g., 200-day MA regime) to decide direction; use *RSI* as an entry signal *within* the bias direction. Only buy oversold RSI in bull regime; only sell overbought RSI in bear regime.

### The clean-design rule

For every signal in a strategy, you must be able to say:
1. **Is this a bias filter?** (Tells me direction; doesn't tell me when.)
2. **Is this an entry signal?** (Tells me when; doesn't tell me which way.)
3. **Is this a hybrid?** (Tells me both — which means the failure modes are entangled.)

If you can't answer, the signal is doing both jobs poorly. Refactor.

### The complete trade design

A complete trade has both layers:

1. **Bias filter** establishes regime (long bias / short bias / no trade)
2. **Entry signal** fires within the bias regime (specific moment)

Plus typically:
3. **Stop placement** (where bias-or-signal is invalidated)
4. **Profit target / trail** (where to exit)
5. **Position size** (how much capital — per [[investment/allocation/equity-management-rules|equity-management rules]])

The first two are the *signal* layer. Mixing them is the most common mistake.

## Cross-Classification of Wiki Mechanisms

For every wiki mechanism, classify what role it plays. **Use this table when designing a new strategy** — match each mechanism to its role.

| Mechanism | Wiki page | Role |
|-----------|-----------|------|
| **Trend (200-day MA regime)** | [[trading/mechanisms/moving-averages]] | **Bias filter** (pure) |
| **MA crossover** | [[trading/mechanisms/moving-averages]] | **Hybrid** — direction from crossover; timing also from crossover. Entangled but trade-able with discipline |
| **RSI overbought/oversold** | [[trading/mechanisms/oscillators-divergence]] | **Entry signal** (pure) — direction must come from bias filter |
| **MACD signal cross** | [[trading/mechanisms/oscillators-divergence]] | **Entry signal** (with mild bias info) |
| **Divergence** (price vs oscillator) | [[trading/mechanisms/oscillators-divergence]] | **Entry signal** with weak bias content (divergence usually precedes mean reversion) |
| **Support / resistance levels** | [[trading/mechanisms/support-resistance]] | **Bias filter** for level-bound regimes; **entry signal** at level test |
| **Range trading** within levels | [[trading/mechanisms/support-resistance]] | **Entry signal** (fade extremes); requires bias filter to confirm range vs trend |
| **Channel breakout** | [[glossary/breakout]] | **Hybrid** — direction from break direction; timing from break event |
| **Pivotal-point breakout** | [[trading/mechanisms/pivotal-points]] | **Hybrid** — same as breakout |
| **Failed-breakout reversal** | [[trading/mechanisms/failed-breakout-reversal]] | **Entry signal** — direction from which side failed |
| **Wyckoff spring** | [[trading/mechanisms/accumulation-distribution]] | **Entry signal** for long-bias; the accumulation-phase classification is the bias filter |
| **Wyckoff upthrust** | [[trading/mechanisms/accumulation-distribution]] | **Entry signal** for short-bias; the distribution-phase classification is the bias filter |
| **Volume + OI confirmation** | [[trading/mechanisms/volume-analysis]] | **Entry signal** confirmer; doesn't gate direction |
| **Group-leader divergence** | [[trading/mechanisms/group-leader-divergence]] | **Bias filter** (regime change for the asset class) |
| **Bubble-cycle stage** | [[foundation/market-wisdom/bubble-cycle-stages]] | **Bias filter** at meta-regime level — gates which *strategy family* to use |
| **Cyclical timing (Lynch P/E inversion)** | [[trading/mechanisms/cyclical-timing]] | **Bias filter** at the cycle level for cyclicals |
| **Effort vs. Result diagnostic** | [[foundation/market-wisdom/wyckoff-three-laws]] | **Entry signal** for absorption setups |
| **Composite Operator phase** | [[foundation/market-wisdom/composite-operator]] | **Bias filter** — markup phase = long bias; markdown = short bias |
| **Funding rate extreme** | [[trading/mechanisms/funding-cycle-dynamics]] | **Hybrid** — extreme funding biases the next move (mean-revert); also signals timing |
| **Liquidation cluster proximity** | [[trading/mechanisms/liquidation-cascade-aftermath]] | **Entry signal** — fires when price approaches cluster |
| **Tape reading absorption** | [[foundation/market-wisdom/tape-reading]] | **Entry signal** — fires at moments of failed-to-react |
| **Sit-tight discipline** | [[foundation/market-wisdom/sit-tight-discipline]] | **Neither** — exit/hold discipline, not signal |
| **Conviction-tiered sizing** | [[investment/allocation/conviction-tiered-sizing]] | **Neither** — sizing meta, not signal |
| **News-response asymmetry** | (Schwager / Lefèvre via tape reading) | **Entry signal** — fires when actual response diverges from expected |
| **Macro liquidity regime** | (Fed easing vs tightening) | **Bias filter** at meta-regime level |
| **BTC dominance** | [[foundation/macro/intermarket-analysis]] | **Bias filter** for asset selection (BTC vs alts) |

## The Counter-Bias Mirror Test

The empirical check that bias-filter and entry-signal components are properly decoupled.

### Setup

Take any strategy. Identify each of its components and classify (bias filter / entry signal / hybrid). Then construct the **counter-bias mirror**: same strategy, but with the *bias filter inverted* (short instead of long, or no bias direction at all).

### Possible outcomes

**Outcome 1**: Mirror produces *worse* results than original (negative expected value, lower hit rate, etc.). 
- *Interpretation*: bias filter is doing real work. The entry signal benefits from being applied within the bias direction.

**Outcome 2**: Mirror produces *similar or better* results than original.
- *Interpretation*: bias filter is *not* doing useful work — the entry signal works regardless of bias. Either (a) the entry signal has standalone edge, or (b) both are noise and any pattern is coincidence.

**Outcome 3**: Mirror produces *opposite-sign* results (original makes 5% per trade; mirror loses 5% per trade).
- *Interpretation*: the bias-direction is the entire edge. The entry signal is just timing convenience; the trade would work without it as long as bias is identified.

**Outcome 4**: Both original and mirror produce noise (no edge in either direction).
- *Interpretation*: the strategy has no edge. Refactor or graveyard.

### Worked example: MA crossover + RSI strategy

Original: long when price > 200-MA AND RSI < 30; short when price < 200-MA AND RSI > 70.

Counter-bias mirror: long when price < 200-MA AND RSI < 30; short when price > 200-MA AND RSI > 70 (RSI signal applied *against* MA bias).

Hypothetical results:
- If mirror underperforms by significant margin: 200-MA bias is doing useful work; RSI is the entry trigger
- If mirror performs similarly: 200-MA isn't useful — RSI extremes have edge regardless of trend regime (rare but possible)
- If mirror outperforms (counter-trend on extremes): the original's edge was specifically counter-trend — RSI is doing the bias work AND timing

The test forces honesty about which component is producing the edge.

## Crypto Application

In crypto specifically, the bias-filter / entry-signal distinction is *especially* useful because:

1. **24/7 markets** — entry signals fire constantly; without bias filter you trade noise
2. **High volatility** — entry signals at extremes are common; without bias filter you fade trends and lose
3. **Strong cycle structure** — bias is identifiable via [[foundation/macro/btc-regime-taxonomy-2020-2025|regime taxonomy]]; pre-committed regime classification feeds bias filter directly
4. **Multi-timeframe interactions** — bias on daily / 4h timeframe; entry signals on lower timeframe; cleaner separation possible than equities

### Crypto-specific bias filters (commonly used)

- **BTC vs 200-day MA** — most-fundamental crypto bias filter
- **BTC vs 200-week MA** — bull/bear cycle filter
- **BTC dominance regime** — BTC vs alt bias
- **Funding regime percentile** — positioning bias (extreme positive = short-bias for mean reversion)
- **LTH cost basis position** — cohort-flow bias
- **Stablecoin supply trajectory** — macro liquidity bias
- **Bubble-cycle stage** ([[foundation/market-wisdom/bubble-cycle-stages]]) — meta-regime bias

### Crypto-specific entry signals (commonly used)

- **Liquidation cluster proximity + reclaim** (Wyckoff spring / failed-breakout reversal)
- **Funding-rate flip** (transition signal)
- **CVD divergence** (effort-vs-result entry)
- **Pivotal-point break** (with directional bias from filter)
- **Volume confirmation** (confirmer, not standalone)

### Sample composite design

A complete day/swing trade design:

1. **Bias filter (regime layer)**: BTC > 200-day MA + bubble-cycle stage = Boom/Euphoria → long bias for BTC perp
2. **Bias filter (positioning layer)**: funding moderate-positive (not extreme) → not crowded → setup viable
3. **Entry signal**: pivotal-point breakout above 90-day high with volume confirmation
4. **Stop**: just below the broken level
5. **Position size**: per equity-management + conviction-tier classification

Each layer has a clearly-named role; failure of any layer can be diagnosed independently.

## Honest Caveats

- **Bias filters can fail too**: a 200-MA bias signal can persist for weeks during a regime change; entry signals taken in the wrong direction during the change-over period lose money. The bias filter is *necessary* but not *sufficient*.
- **Entry signals have asymmetric reliability**: same signal can have different hit rates in different bias regimes. RSI < 30 in a strong bull = high-conviction entry; RSI < 30 in a structural bear = catching-falling-knife. The classification matters but per-regime calibration also matters.
- **Hybrid signals can work**: this page argues for separation, but disciplined hybrid use (e.g., trend-following breakouts) is operationally fine. The discipline is *knowing* it's a hybrid and accepting the entanglement.
- **The classification itself is regime-dependent**: in a trending regime, MA crossover is a hybrid (direction + timing); in a chop regime, the MA crossover might be pure noise. Classification should be made per-regime, not universal.
- **Counter-bias mirror requires sufficient sample**: with only ~20 trades, the mirror result is noise. The test is valid only with N≥100 trades or so per arm.
- **Self-defeating risk**: as more traders apply the bias-filter / entry-signal discipline, simple combinations (200-MA + RSI) get crowded. Edge requires either non-obvious filter or non-obvious entry-signal — or non-obvious combination of both.
- **Not all good strategies use both**: pure trend-following systems (Turtle, Seykota) don't separate — the breakout is both bias and timing. They work because breakout direction *implies* both. The page's discipline doesn't say "always separate"; it says "know which role each component plays."

## Connection to Other Wiki Concepts

### vs. [[foundation/market-wisdom/composite-operator|Composite Operator phases]]

CO phases (accumulation / markup / distribution / markdown) are *bias filters at the cycle level*. They tell you which side the CO is on; they don't fire entries. Entry signals (Wyckoff spring, upthrust, volume confirmation) operate *within* the CO phase.

### vs. [[foundation/market-wisdom/wyckoff-three-laws|Wyckoff Three Laws]]

- **Law 1 (Supply/Demand)** = bias filter (which side has weight)
- **Law 2 (Cause/Effect)** = bias filter (size of expected move)
- **Law 3 (Effort vs. Result)** = entry signal (when the absorption is happening)

The three laws map cleanly onto bias-filter / entry-signal taxonomy.

### vs. [[foundation/market-wisdom/universal-wizards-lessons|Schwager's Universal Lessons]]

- Lesson 2 (Trend Respect) = bias filter discipline
- Lesson 7 (Patience for Setups) = entry-signal discipline
- The two lessons are about different layers; both required for complete trade design.

### vs. [[trading/mechanisms/cyclical-timing|Lynch's Cyclical Timing]]

Lynch's P/E inversion is a *bias filter* at the cycle level for cyclicals. Entry signal is supplied separately (typically by sentiment indicators, dividend-yield levels, or technical setup at the cycle low).

### vs. [[foundation/market-structure/crypto-carrier-features|Carrier Features]]

The carrier-features catalog includes both bias-filter carriers (BTC dominance, halving cycle, MVRV) and entry-signal carriers (liquidation maps, CVD divergence, funding flips). Some carriers serve both depending on how they're used. The bias-filter / entry-signal taxonomy is a *cleaner* cut than carrier categories — it focuses on the role rather than the data source.

## Operational Checklist for Strategy Design

When designing a new strategy, walk through:

1. **List every component** of the strategy (every signal, indicator, rule)
2. **Classify each component**: bias filter, entry signal, hybrid, or other (sizing, exit, etc.)
3. **Audit hybrids**: can any be decomposed into separate bias filter + entry signal? If yes, often cleaner
4. **Confirm completeness**: does the strategy have *both* a bias filter AND an entry signal? Pure-entry strategies are over-traded; pure-bias strategies are under-traded
5. **Counter-bias mirror test**: design the inverted-bias version; backtest both; classify outcome (per Section above)
6. **Document the role classification**: write it down so future-you can review per-component performance

## Key Takeaways

- Two signal types: **bias filter** (gates direction) and **entry signal** (times entry).
- Most failed strategies confuse the two — using one signal as both produces failure modes that depend on regime.
- A complete trade design has both layers: regime / direction filter + entry trigger.
- Cross-classification table maps every wiki mechanism to its role(s).
- **Counter-bias mirror test** is the empirical check — invert the bias filter; mirror should underperform if bias is doing real work.
- Crypto-specific application: 24/7 + high vol + cycle structure makes bias-filter discipline especially valuable.
- Hybrid signals can work; the discipline is *knowing* they're hybrids and accepting the entanglement.
- Self-defeating risk: simple bias + simple entry = crowded.

## Related

- [[foundation/market-wisdom/composite-operator]] — CO phases as cycle-level bias filter
- [[foundation/market-wisdom/wyckoff-three-laws]] — Laws 1+2 = bias; Law 3 = entry signal
- [[foundation/market-wisdom/universal-wizards-lessons]] — Lesson 2 (Trend Respect) + Lesson 7 (Patience) = the two layers
- [[foundation/market-wisdom/bubble-cycle-stages]] — meta-regime bias filter
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — regime reference for bias-filter classification
- [[trading/mechanisms/cyclical-timing]] — cycle-level bias filter
- [[trading/mechanisms/moving-averages]] — MA regime as canonical bias filter
- [[trading/mechanisms/oscillators-divergence]] — RSI/MACD as canonical entry signals
- [[trading/mechanisms/support-resistance]] — levels as bias + entry depending on context
- [[trading/mechanisms/pivotal-points]] — hybrid (direction + timing)
- [[trading/mechanisms/failed-breakout-reversal]] — entry signal (direction supplied by setup)
- [[trading/mechanisms/accumulation-distribution]] — Wyckoff spring/upthrust as entry signals; CO phase classification as bias filter
- [[trading/mechanisms/group-leader-divergence]] — bias filter for asset-class regime change
- [[foundation/market-structure/crypto-carrier-features]] — central catalog; carriers serve as both filter and signal data sources
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — **empirical companion** (added 2026-05-05): which combination architecture wins (embedded bias filter vs separate regime gate); N=4 supporting cases. This page is the *taxonomy* of signal types; that page is the *empirical-architecture* finding.
- Apr 28 wiki-refinement arm — proposed this page as the framework for the counter-bias mirror test

## Sources

- This is a *taxonomic / methodological* page. No single source.
- Implicit antecedents:
  - Trend-following literature (Seykota, Hite, Dennis via [[foundation/sources/schwager-market-wizards|Schwager]]) — implicit bias filter via trend; explicit entry signal via breakout
  - [[foundation/sources/lefevre-reminiscences|Lefèvre]] — line of least resistance is a bias filter; pivotal points are entry signals
  - [[foundation/sources/wyckoff-method|Wyckoff]] — Three Laws map cleanly onto the taxonomy (Laws 1+2 = bias; Law 3 = entry)
  - Trader-psychology literature — tendency to use a single signal for both jobs is named in Tharp, Steenbarger, Mark Douglas (*Trading in the Zone*)
- Counter-bias mirror test methodology — implicit in walk-forward / cross-validation discipline; this page names it explicitly as a strategy-component decoupling check
