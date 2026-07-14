---
title: "Volume and Open Interest Analysis"
status: growing
created: 2026-04-29
updated: 2026-04-29
tags: [mechanism, trading, volume, open-interest, murphy, wyckoff-precursor]
---

# Volume and Open Interest Analysis

> **TL;DR**: Volume confirms or denies price moves. A breakout on light volume is suspect; a breakout on heavy volume is real. Open interest (futures-specific) tells you whether new positions are being opened or existing positions closed. Murphy's Ch 7 is the most direct precursor in the wiki to the still-unfilled [[foundation/sources/lineage|Wyckoff source]] — accumulation/distribution analysis is fundamentally about volume + price patterns. Strong crypto translation: volume-confirmed breakouts work; futures open interest is uniquely visible in crypto perps.

## Volume — The Core Confirmation Signal

> "Volume should generally increase in the direction of the market trend and is an important confirming factor in the completion of all price patterns. The completion of each pattern should be accompanied by a noticeable increase in volume."
> — Murphy, Ch 7

The mechanism: real demand (or supply) shows up as volume. A price move on declining volume is suspect because participation isn't there. A price move on expanding volume is structurally real.

### The asymmetry: volume more important on the upside

> "Volume is more important on the upside. Markets have a way of 'falling of their own weight' once a bear move gets underway. Chartists like to see an increase in trading activity as prices drop, but it is not critical. At bottoms, however, the volume pickup is absolutely essential."
> — Murphy, Ch 7

Why: declining markets can persist on apathy (no buyers) without dramatic volume. Rising markets require *new* buyers — volume is needed to confirm real demand.

For crypto: bottoms are often accompanied by volume spikes (capitulation); slow grinds higher *without* volume often roll over.

## The Six Volume / Price Combinations

Murphy's framework — every price-volume combination has a meaning:

| Price | Volume | Reading |
|-------|--------|---------|
| Up | Up | **Healthy uptrend** — confirmed by participation |
| Up | Down | **Suspect uptrend** — moving without participation |
| Down | Up | **Healthy downtrend** — selling pressure confirmed |
| Down | Down | **Suspect downtrend** — selling exhausting |
| Sideways | Up | Accumulation or distribution depending on phase |
| Sideways | Down | Apathy / consolidation |

The most-actionable readings:
- **Up + Up at breakout** → take it as real
- **Up + Down at breakout** → wait for confirmation; common false breakout
- **Down + Down (volume contracting on declines)** → potential bottom forming; "falling without conviction"
- **Sideways + Up after extended downtrend** → potential accumulation phase (Wyckoff/Dow Tenet 3)

## Volume At Pattern Completion

Per [[trading/mechanisms/chart-patterns|chart patterns]], volume should:
- **Decline through pattern formation** (consolidation = decreased participation)
- **Expand on breakout** (real participation enters)
- **Confirm direction** (continuation breakout has expanding volume in trend direction; reversal breakout has expanding volume against the prior trend)

A pattern completing without volume confirmation has a much higher failure rate.

## Open Interest — Futures-Specific Information

Open interest = total number of outstanding futures contracts at a point in time.

This is *additional* information beyond volume because:
- **Volume** measures activity (trades happening)
- **Open interest** measures positioning (positions held)

### The four price / OI combinations

| Price | Open Interest | Reading |
|-------|---------------|---------|
| Up | Up | **New money on long side** — bullish; new buyers driving rally |
| Up | Down | **Short covering** — rally without new buyers; less sustainable |
| Down | Up | **New money on short side** — bearish; new sellers driving decline |
| Down | Down | **Long liquidation** — decline without new sellers; less sustainable |

The mechanism: rising OI = new positions being opened (someone has new conviction); falling OI = positions being closed (existing players exiting).

The most-actionable readings:
- **Up + OI Up** = sustainable uptrend; new buyers committing capital
- **Up + OI Down** = short-squeeze rally; less likely to sustain once shorts are covered
- **Down + OI Up** = sustainable downtrend; new shorts committing capital
- **Down + OI Down** = long capitulation; potential bottom forming

## Volume Indicators

### On-Balance Volume (OBV)

Joseph Granville, 1963.

```
If close > previous close: OBV = previous OBV + today's volume
If close < previous close: OBV = previous OBV − today's volume
If close = previous close: OBV unchanged
```

Cumulative running total. The signal: OBV divergence with price (price up, OBV down → smart money distributing) precedes price reversal.

### Volume Price Trend (VPT)

Refinement of OBV — weights volume by % price change rather than fixed ±1.

### Accumulation/Distribution Line (Marc Chaikin)

Weights each bar's volume by where the close fell within the day's range:
```
Money Flow Multiplier = ((Close − Low) − (High − Close)) / (High − Low)
A/D Line = Σ (Volume × Money Flow Multiplier)
```

Closes near the high produce positive multipliers (accumulation); closes near the low produce negative (distribution).

### Volume Profile (Volume At Price)

Plots volume horizontally against price (rather than against time). Shows which price levels have seen the most activity. The high-volume nodes are likely [[trading/mechanisms/support-resistance|support/resistance]] levels.

This is the most-actionable volume tool for crypto — and connects directly to on-chain UTXO Realized Price Distribution (URPD) for Bitcoin.

## Crypto Application

Volume analysis works in crypto, with crypto-specific layers:

### Spot vs. perps vs. on-chain volume

Three different "volumes" in crypto, each with different signals:

| Type | What it captures | Signal |
|------|-------------------|--------|
| **Spot exchange volume** | Trades on Binance/Coinbase/etc. | Closest to traditional equity volume; can be wash-traded on lower-tier exchanges |
| **Perp / derivatives volume** | Futures contract activity | Reflects leveraged speculation; high correlation with retail activity |
| **On-chain volume** | Native blockchain transactions | True economic settlement; highest signal-quality but excludes off-chain trading |

A breakout confirmed by **all three** is much higher conviction than one confirmed by perps alone.

### Open interest is uniquely visible in crypto

Crypto perpetual futures publish open interest data in real-time per exchange. Coinglass, Glassnode, Skew aggregate this. The signal quality is excellent compared to traditional futures (where OI is disclosed daily).

OI dynamics commonly tracked:
- **Rising OI + rising price**: new longs piling in (sustainable but vulnerable to liquidation cascade)
- **Falling OI + rising price**: short squeeze (often quick to reverse)
- **Rising OI + falling price**: new shorts piling in
- **Falling OI + falling price**: long unwind / capitulation

### On-chain accumulation indicators (Wyckoff translation)

The Dow tenet 3 / Wyckoff accumulation framework gets specific in crypto:

- **Long-term holder cohort behavior** (Glassnode metric): when LTH supply growing during price weakness → accumulation
- **Exchange outflows**: BTC moving from exchanges to private wallets → accumulation (off-market supply)
- **Stablecoin supply growth**: stablecoin issuance rising → buying power building
- **Realized cap vs market cap**: divergence shows accumulation by long-term holders at different prices

## Effort vs. Result — The Wyckoff Diagnostic (added 2026-05-03 from Wyckoff ingest)

[[foundation/sources/wyckoff-method|Wyckoff (1910/1931)]] articulates the deepest volume-analysis principle in the literature: **volume is effort; price change is result**. When effort doesn't translate into commensurate result, someone large is on the other side absorbing.

> "It required 8400 shares to be taken in order to advance the stock 1¼ points. This is not bullish because of the way they did it; therefore, bearish." (Method TR Sec 5)

> "Notwithstanding the predominance of apparent demand, the resistance offered (whether legitimate or artificial) became too great for the stock to overcome, and it fell back from 144⅛… On the way up these volumes alone suggested a purchase, but the tape showed abnormal transactions accompanied by poor response from the rest of the list. This suggested manipulation and warned the operator to be cautious on the bull side." (Studies Ch V)

This is the principle Murphy's six volume/price combinations (above) operationalize — but Wyckoff stated it 89 years earlier and more precisely.

### The full diagnostic table (effort/result)

| Effort (volume) | Result (price change) | Diagnosis |
|-----------------|----------------------|-----------|
| **High volume, small price advance** | Mismatch | Hidden seller absorbing demand → bearish |
| **High volume, small price decline** | Mismatch | Hidden buyer absorbing supply → bullish |
| **High volume, large price move with the trend** | Match | Genuine breakout/breakdown — trend continuation |
| **Low volume, large price move** | Mismatch (different way) | Lack of commitment; suspect; prone to reversal |
| **Low volume, small decline against trend** | Match | No real selling pressure; bullish (lack-of-supply confirmation) |
| **Low volume, small advance against trend** | Match | No real buying pressure; bearish (lack-of-demand confirmation) |

### The Reading 1909 worked example

Wyckoff's canonical illustration (Studies Ch V): 50,000 shares change hands at 144 in a ¾-point band over ~1 hour. Apparent demand keeps price holding at 144. But:

- Effort: 50K shares (massive)
- Result: ¾-point range (tiny)
- Diagnosis: hidden distribution; the CO is unloading into apparent demand
- Confirmation: lack of follow-through in the rest of the list (group breadth)

Wyckoff sells short; rides break to lower prices.

### Crypto operationalization

The effort-vs-result diagnostic is the load-bearing principle for crypto-perp volume work:

- **CVD divergence**: cumulative volume delta (buy-aggressor minus sell-aggressor volume) diverging from price = effort/result mismatch
- **OI-volume mismatch**: rising OI without rising price = positions opening but not winning
- **High-volume narrow-range bars** at tops/bottoms: the modern equivalent of Wyckoff's Reading-at-144 — distribution/accumulation footprint
- **Liquidation cluster + funding flip + price recovery**: see [[trading/mechanisms/accumulation-distribution|spring setup]]

### Connection to the Three Laws

This is **Law 3 — Effort vs. Result** in [[foundation/market-wisdom/wyckoff-three-laws|Wyckoff's diagnostic framework]]. The named formal "law" is successor synthesis (Evans 1940s+); the operational logic is primary Wyckoff.

## The Wyckoff Wave Chart (added 2026-05-03)

Wyckoff's mechanical device for diagnosing immediate trend (Method TR Sec 2): build a composite of five leading stocks (sum of prices) plotted as buying and selling waves; trend = whichever wave (up or down) is longer in time and distance.

> "All movements in the market are made up of alternating buying and selling waves. We judge the strength or weakness of the market by the distance in points and fractions recorded by these waves; we combine this distance with the length of time each wave takes to run its course." (Method TR Sec 2)

### Crypto translation

The Wave Chart is the proto-version of modern moving-average / oscillator logic, but tied to *discrete buying-pressure vs. selling-pressure events* rather than smoothed prices. Modern crypto analogs:

- **Aggregate CVD across BTC + ETH + top 10 alts** = Wave Chart for crypto majors
- **Total stablecoin-pair volume across major CEXes** = Wave Chart for the speculative pool
- **OI-weighted directional flow across BTC, ETH, SOL perps** = a derivatives-side Wave Chart

The principle: aggregate-of-leaders is more reliable than any single asset; trend identification benefits from this composite view.

These are the modern crypto equivalents of Wyckoff's "smart money footprint" analysis. The wiki's [[foundation/sources/lineage|priority queue]] has Wyckoff at #2 for exactly this reason.

## Hypothesis (Structured Format)

```
Hypothesis:    BTC perp breakouts confirmed by simultaneous spot + perp +
               on-chain volume expansion produce significantly higher
               follow-through rates than perp-only breakouts.

Mechanism:     Three-source volume confirmation indicates the breakout is
               supported by real demand across markets, not just leveraged
               positioning. Single-source breakouts (perp-only) are often
               leverage-driven and revert quickly.

Observable:    At each breakout: simultaneous % volume expansion across
               spot, perp, on-chain (BTC settlement volume).

Expected:      Three-source confirmed breakouts produce 65-70% follow-through
               within 24h vs 50-55% for perp-only.

Falsifier:     If three-source confirmed and perp-only breakouts have
               similar follow-through rates (within 5pp), the additional
               sources are not adding signal.
```

## Test Plan

For any volume-based strategy:

1. Define "breakout" objectively (e.g., price exceeding prior 20-bar high by ≥ 1% on bar close)
2. Define "volume expansion" objectively (e.g., bar volume ≥ 2× 20-bar SMA volume)
3. Define "follow-through" objectively (e.g., price doesn't retest the breakout level within 4h)
4. Backtest across multiple regimes (bull, bear, chop)
5. Falsifier: if confirmed breakouts don't beat random-direction baseline by >5pp on follow-through, signal isn't real

## Regime Applicability

| Regime (per [[foundation/macro/btc-regime-taxonomy-2020-2025\|BTC taxonomy]]) | Mechanism state | Evidence |
|---|---|---|
| Cycle peak rejection / bearish (e.g. 2025-Q4) | Not yet tested as standalone in pipeline | — |
| Bearish continuation (e.g. 2026-Q1) | Not yet tested as standalone in pipeline | — |
| Bullish trending | Active — volume confirms continuations | Inferred from Wyckoff Effort vs Result Law |
| Chop / transitional | Active — volume divergence highlights regime turn | Inferred from Wyckoff Effort vs Result Law |
| Cascade aftermath | **Most-diagnostic** — capitulation volume + diminishing volume on retest = bottoming signal | Per Wyckoff phase analysis |
| Volume-divergent breakouts (any regime) | **Pattern signal**: if breakout fires without volume confirmation, treat as suspect | Per textbook §249 |

**Notes:**
- Mechanism is **regime-agnostic as confirmation signal** but **regime-aware in interpretation** — same volume signature reads differently in trend vs chop
- Required regime markers for productionize: volume baseline (20-bar SMA volume) + clear price-volume relationship + regime classification for interpretation
- Anti-regime conditions: low-liquidity hours (Asian session weekends; holiday periods); regime where volume distortion from large players masks real flow

This mechanism is foundational to several others (chart-patterns confirmation, accumulation-distribution Effort vs Result, breakout validation). Per [[foundation/macro/regime-conditional-edge-2025-26|W2 finding]]: 13/13 strategies in the cohort showed regime-conditional edge concentrated in 2025-Q4 + 2026-Q1; volume confirmation likely inherited but not isolated.

## Key Takeaways

- Volume confirms or denies price moves; volume-divergent moves are suspect
- Volume is more important on the upside (markets can fall on apathy but need participation to rise)
- Six price/volume combinations each have a reading; "Up + Up at breakout" is the high-conviction setup
- Open interest in futures (especially crypto perps) adds *positioning* information beyond volume's *activity* information
- Four price/OI combinations: rising OI = new positions; falling OI = unwinds; the asymmetry between new positions and unwinds is tradable
- Volume profile / Volume At Price links volume analysis directly to support/resistance levels — most actionable tool
- Crypto has three volume sources (spot, perps, on-chain); cross-confirmation across all three is highest-conviction
- This page is the wiki's current best precursor to the still-unfilled [[foundation/sources/lineage|Wyckoff source]] — accumulation/distribution analysis is fundamentally volume-and-price work

## Related
- [[../../foundation/sources/cong-crypto-wash-trading|Crypto Wash Trading (Cong et al. 2023)]] — data-quality gate: reported volume is unreliable on unregulated venues

- [[foundation/market-structure/crypto-carrier-features]] — central catalog of crypto-native carriers; the three crypto volume sources (spot, perp, on-chain) and OI-as-positioning-layer are catalogued there
- [[foundation/sources/wyckoff-method]] — primary articulation of effort-vs-result; the Wave Chart; volume-as-effort framework. Source for the Wyckoff sections above.
- [[foundation/sources/murphy-technical-analysis]] — modernization of volume analysis; six combinations table
- [[foundation/sources/lefevre-reminiscences]] — Livermore's tape reading (1923) is the experiential precursor to formalized volume analysis
- [[foundation/market-wisdom/wyckoff-three-laws]] — the diagnostic framework within which volume analysis operates (Law 3: Effort vs. Result)
- [[foundation/market-wisdom/composite-operator]] — what volume analysis is actually diagnosing — CO action across the four-phase cycle
- [[foundation/market-wisdom/tape-reading]] — the broader discipline; this page operationalizes the absorption / failure-to-react / breakout-volume patterns
- [[trading/mechanisms/accumulation-distribution]] — operational Wyckoff cycle mechanism
- [[foundation/market-wisdom/dow-theory-and-ta-premises]] — Dow tenet 5 (volume confirms trend) is operationalized here
- [[trading/mechanisms/support-resistance]] — volume profile / VAP shows literal volume distribution at price levels
- [[trading/mechanisms/pivotal-points]] — volume confirmation on the breakout bar is integral to pivotal-point validity
- [[trading/mechanisms/chart-patterns]] — patterns require volume confirmation to be valid
- [[trading/mechanisms/oscillators-divergence]] — volume divergence is one of the strongest oscillator-divergence signals
- [[foundation/sources/lineage]] — Wyckoff is priority #2 to fill; this page should be expanded substantially when Wyckoff arrives

## Open Questions

- **What's the empirical follow-through rate for three-source confirmed breakouts in BTC?** Testable; would be a highest-leverage early backtest given the trader's day/swing horizon.
- **Does on-chain volume add signal beyond spot+perp volume, or is it redundant?** Hypothesis: adds signal because on-chain captures non-trading transfers (whale movements, exchange flows).
- **Can OI velocity (rate of change) outperform OI level for predicting trend continuation?** Common claim in crypto; deserves test.

## Sources

- Murphy, John J. *Technical Analysis of the Financial Markets*, Ch 7 ("Volume and Open Interest"), 1999.
- **Wyckoff, Richard D. (as Rollo Tape).** *Studies in Tape Reading*, Ch V (Volumes and Their Significance) — 1910. Source: [[foundation/sources/wyckoff-method]]. The most-precise volume-analysis principle in the literature: "the figures on the tape represent the consensus of opinion, the effect of manipulation and the supply and demand, all combined into concrete units."
- **Wyckoff, Richard D.** *The Richard D. Wyckoff Method of Trading in Stocks*, Method Div 2, TR Secs 2, 5 — 1931. Source for the Wave Chart and the 8400-shares-for-1¼-points effort/result example.
- Lefèvre, Edwin. *Reminiscences of a Stock Operator* (1923) — Chs I, V, X, XVII, XX. Source: [[foundation/sources/lefevre-reminiscences]]. Pre-formal account of the same volume/price patterns Murphy and Wyckoff formalize.
