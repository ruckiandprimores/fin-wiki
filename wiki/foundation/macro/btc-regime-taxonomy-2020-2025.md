---
title: "BTC Regime Taxonomy 2020–2025 — Operational Reference"
status: growing
created: 2026-05-04
updated: 2026-06-12
tags: [foundation, macro, btc, regime, taxonomy, calibration-reference, regime-conditioning, day-swing-applicable, 2026-h1-bear]
---

# BTC Regime Taxonomy 2020–2025 — Operational Reference

> **TL;DR**: The single named reference for BTC market regimes from 2020 Q1 to 2026 Q2. Each regime has characteristic markers (realized vol, EMA trend state, funding profile, OI dynamics, stablecoin supply trajectory, dominant strategies that worked). **Operational purpose**: regime-conditioned within-window slicing — when a strategy's backtest produces results, the question "what regime did this work in?" needs to be answerable by lookup, not re-litigation. Cross-references all other wiki pages that classify regimes (Kindleberger 5-stage, Wyckoff 4-phase, Lynch cyclical-timing).

## Why This Page Exists

Backtest interpretation is regime-dependent. A strategy that produced positive returns 2020 Q4 - 2021 Q1 isn't necessarily edge — it may be regime — *trend-following in a vertical bull worked because vertical bulls reward trend-following*. The honest question for any backtest is: **which regimes did this work in? Which did it fail in? And do those regimes have anything in common?**

Without a named regime reference, every backtest discussion re-litigates "what counts as bull vs bear" with whoever's reviewing. This page eliminates that.

The taxonomy is operationally valuable for:
- **Regime-conditioned backtest slicing** — split full-period backtest into per-regime subsets
- **Strategy survivability check** — does the strategy work across multiple regimes, or only one?
- **Regime-aligned strategy selection** — match strategy family ([[trading/mechanisms/funding-cycle-dynamics|funding-cycle]], [[trading/mechanisms/accumulation-distribution|spring/upthrust]], [[trading/mechanisms/group-leader-divergence|group-leader divergence]]) to current regime
- **Live regime classification** — given today's markers, which historical regime is most analogous?
- **Walk-forward alternative** — per the wiki architecture push-back on multi-year walk-forward, regime-conditioned slicing is the deploy-window-relevant test methodology

## How To Use This Page

For any backtest, walk through:

1. **Identify which regime windows fall within the backtest period** (using the table below as the regime reference)
2. **Slice the backtest by regime** — compute per-regime returns, drawdowns, Sharpe
3. **Identify regime-dependence**: did the strategy work in trending regimes only? Did it work in chop only? Did it survive cascade events?
4. **Match current regime against historical**: identify the most-analogous prior regime; calibrate expectations from that regime's results
5. **Position size accordingly**: aggressive in confirmed favorable regimes; defensive in unfavorable

## The Marker Set (Carrier Reference)

Each regime is characterized by combinations of these markers. Full carrier definitions at [[foundation/market-structure/crypto-carrier-features]].

| Marker | What it measures | Regime signal |
|--------|------------------|---------------|
| **Price vs 200-day MA** | Long-term trend regime | Above = bull bias; below = bear bias; MA slope indicates trend strength |
| **Realized vol (30-day)** | Volatility regime | Low (<50%): compressed; Normal (50-80%); High (80-120%); Extreme (>120%) |
| **Funding rate (avg 30-day)** | Crowded positioning | Negative: short-crowded; Neutral (0-5bp/8h); Moderate (5-25bp/8h); Extreme (>25bp/8h) |
| **Open interest trajectory** | Position-building direction | Rising on price up = healthy bull; Rising on flat/down = positioning crowding |
| **Stablecoin supply** | Macro liquidity into crypto | Expanding: capital inflow; Contracting: capital exit |
| **BTC dominance** | Internal-class regime | Rising: BTC-dominance phase; Falling: alt-season phase |
| **LTH cost basis** | Cohort positioning | Diverging from spot reveals accumulation/distribution |

## The Regime Taxonomy

### Regime 1 — COVID Crash + V-Recovery (Mar–May 2020)

| Field | Value |
|-------|-------|
| **Dates** | March 12, 2020 → May 11, 2020 (halving) |
| **Price** | $4,000 (March 12 low) → $9,500 (May halving) |
| **Defining event** | March 12, 2020 ("Black Thursday") — BTC crashed from $7,900 to $3,800 in 24 hours; cascade exhausted same week |
| **Macro context** | COVID lockdowns; Fed cut to 0%; unlimited QE announced March 23 |
| **Realized vol** | **Extreme** (~150-200% during crash; normalized to ~80% by May) |
| **Trend state** | V-bottom recovery; below 200-day MA initially; reclaimed by April |
| **Funding** | Negative during crash; turning neutral by May |
| **OI** | Collapsed during crash; rebuilt slowly |
| **Stablecoin supply** | Modest growth; ~$5B USDT total |
| **BTC.D** | Rising during crash, then stabilizing |
| **Kindleberger stage** | End of prior cycle's Panic; transition to early Boom of new cycle |
| **Wyckoff CO phase** | Late accumulation following selling climax |
| **Strategies that worked** | Long-side capitulation entries (Wyckoff spring); volatility-expansion plays during crash; pre-halving accumulation |
| **Strategies that failed** | Trend-following (whipsaw); mean-reversion shorts during V-recovery |
| **Notable** | Bitcoin itself rose +160% from March low to May halving — fastest BTC recovery on record |

### Regime 2 — Halving Accumulation (May–Sept 2020)

| Field | Value |
|-------|-------|
| **Dates** | May 11, 2020 → Sept 1, 2020 |
| **Price** | $9,500 → $11,800 (modest range) |
| **Defining event** | Halving + DeFi Summer (Compound COMP launch June 15) |
| **Macro context** | Stimulus checks distributing; M2 expanding |
| **Realized vol** | **Compressed** (~40-50%) |
| **Trend state** | Above 200-day MA; flat slope |
| **Funding** | Neutral to slightly positive (~5-10bp/8h) |
| **OI** | Rebuilding gradually |
| **Stablecoin supply** | Beginning rapid expansion (USDT ~$5B → $15B) |
| **BTC.D** | Falling (DeFi tokens outperforming BTC) |
| **Kindleberger stage** | Early Boom |
| **Wyckoff CO phase** | Late accumulation transitioning to markup |
| **Strategies that worked** | DeFi yield farming; ETH outperformance vs BTC; range-trading the BTC consolidation |
| **Strategies that failed** | Aggressive directional BTC trend-following (chop); BTC-only-focus strategies (alt-season was happening) |
| **Notable** | DeFi Summer was a sub-bubble within the larger accumulation — a worked example of micro-mania within macro-accumulation |

### Regime 3 — Vertical Bull #1 (Oct 2020 – Apr 2021)

| Field | Value |
|-------|-------|
| **Dates** | Oct 7, 2020 → Apr 14, 2021 |
| **Price** | $10,500 → $64,805 (Apr 14 first ATH) |
| **Defining event** | MicroStrategy first BTC purchase (Aug 2020 in prior regime, but inflection point); Tesla announced BTC purchase Feb 8, 2021 ($1.5B) |
| **Macro context** | Continued stimulus; vaccine rollout; "everything rally" |
| **Realized vol** | **Normal-to-elevated** (~60-90%) |
| **Trend state** | Strong uptrend; price well above 200-day MA; MA slope steeply rising |
| **Funding** | Moderate-to-extreme positive (~20-50bp/8h sustained) |
| **OI** | Rising on price up — healthy bull buildup |
| **Stablecoin supply** | Rapid expansion (USDT $20B → $50B; USDC $3B → $13B) |
| **BTC.D** | Falling then stabilizing (alts outperformed BTC late in regime) |
| **Kindleberger stage** | Late Boom transitioning to Euphoria |
| **Wyckoff CO phase** | Markup (CO bidding up to attract public) |
| **Strategies that worked** | Trend-following + breakout entries; long-side carry trades on perp basis; alt-season rotations; pyramid scaling on rising scale |
| **Strategies that failed** | Counter-trend shorts (got squeezed); mean-reversion (trend overrode); fading retail FOMO (the FOMO was the trade) |
| **Notable** | Most-rapid retail entry in BTC history. Coinbase IPO April 14, 2021 marked the peak — classic late-stage signal. |

### Regime 4 — May 2021 Cascade (Apr 14 – Jul 21, 2021)

| Field | Value |
|-------|-------|
| **Dates** | April 14, 2021 → July 21, 2021 (mid-cycle bottom) |
| **Price** | $64,805 → $29,300 (July 21 mid-cycle low; -55%) |
| **Defining event** | China mining ban (May 21, 2021); Elon Musk Tesla reversal (May 12: Tesla suspended BTC payments citing energy concerns) |
| **Macro context** | Inflation concerns rising; Fed taper anticipation |
| **Realized vol** | **Extreme during cascade** (~120%); normalizing to high (~80%) by July |
| **Trend state** | Broke 200-day MA briefly; reclaimed by mid-July |
| **Funding** | Flipped extreme negative briefly (cascade); back to neutral |
| **OI** | Collapsed during cascade; rebuilt gradually |
| **Stablecoin supply** | Continued growth despite price decline (USDT to $62B by July) |
| **BTC.D** | Rose during cascade (flight to BTC from alts); peak May 21 at ~46% |
| **Kindleberger stage** | Mid-cycle Distress (NOT full Panic — recovery within 2-3 months) |
| **Wyckoff CO phase** | Mid-cycle distribution (failed); CO accumulating again at lower prices |
| **Strategies that worked** | Long-side capitulation entries late in regime; spring setups at $30K (multi-month base) |
| **Strategies that failed** | Long-side directional during cascade; trend-following (whipsaw) |
| **Notable** | This was a *mid-cycle correction*, not a full bear market. Distinguishing this from a true bear was real-time-difficult. The framework's "is this Distress or full Panic?" question matters here. |

### Regime 5 — Vertical Bull #2 + Euphoria Peak (Jul 21 – Nov 10, 2021)

| Field | Value |
|-------|-------|
| **Dates** | July 21, 2021 → November 10, 2021 |
| **Price** | $29,300 → $69,044 (Nov 10 ATH) |
| **Defining event** | First BTC futures ETF (BITO) Oct 19, 2021; FTX naming-rights sponsorships ramping |
| **Macro context** | Pre-Fed-pivot; persistent inflation; "transitory" debate |
| **Realized vol** | **Elevated** (~70-90%) |
| **Trend state** | Strong uptrend; MA reclaim then steady rise |
| **Funding** | Extreme positive sustained (~30-80bp/8h annualized 100-250%) |
| **OI** | Aggregate ~$80B+; BTC perp ~$24B |
| **Stablecoin supply** | USDT ~$74B, USDC ~$45B at peak |
| **BTC.D** | ~42% at peak (alts outperforming during latter Boom) |
| **Kindleberger stage** | Euphoria (peak Nov 10) |
| **Wyckoff CO phase** | Late markup transitioning to distribution |
| **Strategies that worked** | Trend-following continued; alt-season trades; pyramid extensions; long-side carry continued (despite extreme funding) |
| **Strategies that failed** | Shorts (still got squeezed); fading parabolas (parabolas extended further) |
| **Notable** | Twin peaks structure (Apr + Nov 2021) is unusual — prior cycles had single peaks. May 2021 cascade reset positioning enabling the second peak. |

### Regime 6 — Distress (Nov 10, 2021 – May 9, 2022)

| Field | Value |
|-------|-------|
| **Dates** | November 10, 2021 → May 9, 2022 |
| **Price** | $69,044 → $32,000 (May 9 — pre-Terra-collapse) |
| **Defining event** | Fed signals tightening Dec 2021; first hike March 16, 2022 (25bp) |
| **Macro context** | Inflation surge; Russia/Ukraine war Feb 2022; aggressive Fed pivot |
| **Realized vol** | **Elevated** (~80-100%) |
| **Trend state** | Below 200-day MA from Q1 2022; trend reversal confirmed |
| **Funding** | Volatile; flipping between modest positive and modest negative |
| **OI** | Falling on weakness — distress signature |
| **Stablecoin supply** | Plateau then beginning to contract |
| **BTC.D** | Rising (flight from alts to BTC) |
| **Kindleberger stage** | Distress |
| **Wyckoff CO phase** | Distribution → early markdown |
| **Strategies that worked** | Group-leader-divergence shorts (alts vs BTC); reaction-on-rally fades; volatility-expansion long-vol plays |
| **Strategies that failed** | Long-side breakout chases (false breakouts); buy-the-dip on prior support (support failed) |
| **Notable** | NFT volumes fell sharply Q1 2022 *while floor prices held* — classic denial-phase signature |

### Regime 7 — Panic / Bottom (May 9, 2022 – Nov 21, 2022)

| Field | Value |
|-------|-------|
| **Dates** | May 9, 2022 → November 21, 2022 |
| **Price** | $32,000 → $15,476 (Nov 21 cycle low; -77.6% from peak) |
| **Defining events** | Terra/UST collapse May 7-12 ($40B wiped); 3AC insolvent June 15-16; Celsius halt June 12; Voyager Chapter 11 July 5; FTX collapse Nov 8-11 |
| **Macro context** | Continued Fed hiking; recession fears; persistent inflation |
| **Realized vol** | **High** (~80-120%); spikes during cascade events |
| **Trend state** | Confirmed downtrend below 200-day MA |
| **Funding** | Deeply negative during cascades; neutral between |
| **OI** | Multiple collapses (cascades) followed by rebuilds |
| **Stablecoin supply** | USDC fell from $43B to $32B on redemptions Nov 2022 |
| **BTC.D** | Rising sharply on each cascade event |
| **Kindleberger stage** | Panic (multi-event sequence) |
| **Wyckoff CO phase** | Markdown → late accumulation |
| **Strategies that worked** | Long-side capitulation entries on multi-channel confluence; basis-trade compression plays; selective short-side after FTX (premature) |
| **Strategies that failed** | Long-side trend-following; "buy the dip" without confluence; counterparty-exposed strategies (FTX users lost everything) |
| **Notable** | **No LOLR** — bottom came via price exhaustion + multi-cascade exhaustion, not central-bank rescue. Drawdown depth -77% consistent with no-LOLR architecture (vs. 2008 -56% S&P with Fed action). See [[foundation/macro/crypto-2020-2022-cycle]] for full Kindleberger framework application. |

### Regime 8 — Bottoming + Initial Recovery (Nov 22, 2022 – Mar 31, 2023)

| Field | Value |
|-------|-------|
| **Dates** | November 22, 2022 → March 31, 2023 |
| **Price** | $15,476 → $28,500 (end Q1 2023; +84% from low) |
| **Defining event** | Silicon Valley Bank collapse March 10, 2023; USDC briefly de-pegged; FDIC rescue → BTC rallied as banking-stress hedge narrative |
| **Macro context** | Fed continuing to hike but slowing; banking sector stress; "dollar debasement" narrative |
| **Realized vol** | **Normalizing** (~60-80%); SVB-spike briefly ~120% |
| **Trend state** | Below 200-day MA initially; reclaimed late Q1 |
| **Funding** | Neutral, occasional moderate positive |
| **OI** | Rebuilding from cycle lows |
| **Stablecoin supply** | USDT growing; USDC contracting (post-SVB regulatory caution) |
| **BTC.D** | Stable around 42-45% |
| **Kindleberger stage** | End of Panic; transition to early Recovery |
| **Wyckoff CO phase** | Late accumulation; spring confirmed |
| **Strategies that worked** | Long-side spring entries; Wyckoff phase-D markup positioning; SVB-stress crisis-hedge long |
| **Strategies that failed** | Short-side continuation (cycle bottom held); mean-reversion shorts during SVB rally |
| **Notable** | The Nov 21 low held through SVB stress test = high-conviction confirmation that bottom was structural |

### Regime 9 — Mid-2023 Chop (Apr 2023 – Sept 2023)

| Field | Value |
|-------|-------|
| **Dates** | April 2023 → September 2023 |
| **Price** | $25,000 - $31,500 range (~$26-28K most of period) |
| **Defining event** | Quiet period; SEC vs Coinbase / Binance enforcement actions June 2023; BlackRock spot ETF filing June 15 (catalyst for next regime) |
| **Macro context** | Fed continuing hikes through July; pause begins; banking stress contained |
| **Realized vol** | **Compressed** (~30-50%) — historically low for BTC |
| **Trend state** | Sideways; oscillating around 200-day MA |
| **Funding** | Neutral with occasional moderate spikes |
| **OI** | Range-bound |
| **Stablecoin supply** | Slow contraction continuing |
| **BTC.D** | Rising slowly toward 50% |
| **Kindleberger stage** | Early Recovery (post-Panic accumulation) |
| **Wyckoff CO phase** | Accumulation in defined range |
| **Strategies that worked** | Range-trading the $25-31K bounds; mean-reversion strategies; volatility-selling (compressed regime); funding-cycle trades during occasional spikes |
| **Strategies that failed** | Trend-following (chopped); breakout-buying (false breakouts); aggressive directional bets |
| **Notable** | This was the *quietest period* for BTC in years — vol compression to 30% level was a multi-year low. Range-trading + vol-selling were the optimal regime strategies. |

### Regime 10 — ETF Anticipation Rally (Oct 2023 – Jan 11, 2024)

| Field | Value |
|-------|-------|
| **Dates** | October 2023 → January 11, 2024 (ETF approval) |
| **Price** | $26,500 → $48,500 (ETF approval day) |
| **Defining event** | BlackRock ETF filing momentum; spot ETF anticipation; SEC approval Jan 11, 2024 |
| **Macro context** | Fed pause confirmed; inflation moderating; rate-cut expectations rising |
| **Realized vol** | **Rising** from compressed → normal (~50-70%) |
| **Trend state** | Reclaiming 200-day MA decisively; strong uptrend establishing |
| **Funding** | Moderate positive; rising during the rally |
| **OI** | Rebuilding strongly |
| **Stablecoin supply** | Reversal — USDT growing, total supply turning |
| **BTC.D** | Rising (BTC outperforming alts) — institutional flow |
| **Kindleberger stage** | Boom (new cycle's expansion phase) |
| **Wyckoff CO phase** | Markup |
| **Strategies that worked** | Trend-following on the breakout; pivotal-point breakouts at prior resistance levels; long-side carry returning |
| **Strategies that failed** | Counter-trend shorts; fading the rally on overbought oscillator readings (the rally extended) |
| **Notable** | ETF approval was *anticipated and priced* — the first day of trading saw selling ("buy the rumor sell the news"). But the broader rally continued over months. |

### Regime 11 — ETF Run + Halving + New ATH (Jan 12 – Mar 14, 2024)

| Field | Value |
|-------|-------|
| **Dates** | January 12, 2024 → March 14, 2024 |
| **Price** | $46,000 → $73,750 (March 14 — first ATH break) |
| **Defining event** | ETF inflows begin (BlackRock IBIT in particular); BTC breaks 2021 ATH ($69,044) on March 5, 2024 |
| **Macro context** | Rate-cut expectations; AI tech narrative reinforcing risk-on |
| **Realized vol** | **Elevated** (~70-90%) |
| **Trend state** | Strong uptrend; well above 200-day MA |
| **Funding** | Moderate-to-extreme positive (rising into ATH break) |
| **OI** | Reaching new highs as ETF buying flows in |
| **Stablecoin supply** | Expanding rapidly |
| **BTC.D** | Rising (BTC ETF flow > alt flow); peaked ~58% |
| **Kindleberger stage** | Boom transitioning to early Euphoria |
| **Wyckoff CO phase** | Markup with first signs of distribution at ATH break |
| **Strategies that worked** | Trend-following + pivotal-point breakouts (ATH break was canonical pivotal point); long-side carry maintained |
| **Strategies that failed** | Mean-reversion shorts on overbought; counter-trend trades |
| **Notable** | ETF flows were the *direct* mechanism — BlackRock IBIT alone reached >$10B AUM in months. First time institutional flow was the dominant driver. |

### Regime 12 — Post-Halving Consolidation (Mar 15 – Aug 5, 2024)

| Field | Value |
|-------|-------|
| **Dates** | March 15, 2024 → August 5, 2024 |
| **Price** | $73,750 → $48,000 (Aug 5 — Yen carry unwind) |
| **Defining event** | Halving April 19-20, 2024 (block reward 6.25 → 3.125); Yen carry trade unwind August 5, 2024 |
| **Macro context** | Pre-Fed-cut waiting; AI bubble concerns; Yen carry trade vulnerable |
| **Realized vol** | **Normal** (~60-80%); spike during August |
| **Trend state** | Sideways-to-corrective; tested 200-day MA at August low |
| **Funding** | Modest positive; flipped briefly negative during August unwind |
| **OI** | Range-bound; collapsed during August cascade |
| **Stablecoin supply** | Steady growth |
| **BTC.D** | Rising during corrective period (alts underperformed) |
| **Kindleberger stage** | Mid-cycle Distress (similar to May 2021) |
| **Wyckoff CO phase** | Distribution / re-accumulation |
| **Strategies that worked** | Range-trading; spring setups at the August low (spring confirmed within days); long-side capitulation entries on Aug 5 |
| **Strategies that failed** | Trend-following (chopped); long-side breakout chases at $73K (failed) |
| **Notable** | This was a *mid-cycle correction* analogous to May 2021. Distinguishing from full Distress→Panic was critical — the correction resolved within 5 months back to new highs. |

### Regime 13 — Trump Rally + $100K Crossing (Aug 6 – Dec 2024)

| Field | Value |
|-------|-------|
| **Dates** | August 6, 2024 → December 2024 |
| **Price** | $48,000 → $108,000 (Dec 2024 high) |
| **Defining event** | Trump elected November 5, 2024; pro-crypto policy announcements; SEC chair change anticipated; BTC crosses $100K Dec 5, 2024 |
| **Macro context** | Fed cuts started Sept 2024; risk-on; pro-crypto regulatory environment expected |
| **Realized vol** | **Elevated** (~80-100%) |
| **Trend state** | Strong uptrend; well above 200-day MA |
| **Funding** | Sustained moderate-to-extreme positive |
| **OI** | New all-time highs |
| **Stablecoin supply** | Strong expansion |
| **BTC.D** | Stabilizing around 55-60% (BTC-led, alts beginning to follow) |
| **Kindleberger stage** | Boom → Euphoria |
| **Wyckoff CO phase** | Markup |
| **Strategies that worked** | Trend-following on Trump-rally breakout; pivotal-point trades at $69K reclaim and $100K crossing; long-side carry; alt-rotation late in regime |
| **Strategies that failed** | Counter-trend shorts during Trump rally (squeezed); fading the $100K psychological level (broke through) |
| **Notable** | $100K psychological cross was a textbook Livermore pivotal-point — extended ~10% above the level within days. |

### Regime 14 — 2025 Continued Bull / ATH Run (Jan – Oct 2025)

| Field | Value |
|-------|-------|
| **Dates** | January 2025 → October 2025 |
| **Price** | $94,000 (Jan low) → $126,198 (Oct 6, 2025 ATH) |
| **Defining events** | Continued ETF inflows; sustained institutional adoption; pro-crypto regulatory framework taking shape; multiple narrative cycles within (AI tokens, RWA, restaking, memecoins) |
| **Macro context** | Continued Fed easing; soft landing narrative dominant; AI bubble continuing |
| **Realized vol** | **Elevated** (~70-90%) |
| **Trend state** | Strong uptrend (with corrections); consistently above 200-day MA |
| **Funding** | Moderate-to-extreme positive sustained |
| **OI** | New all-time highs by Q3 2025 |
| **Stablecoin supply** | Continued expansion |
| **BTC.D** | Cyclical (alt season cycles within the broader BTC run); periodic resets |
| **Kindleberger stage** | Late Boom → Euphoria (potentially transitioning) |
| **Wyckoff CO phase** | Markup with intermittent distribution signs at ATH levels |
| **Strategies that worked** | Trend-following + pivotal-point breakouts; alt rotations during BTC-D drops; group-leader divergence on alt-season tops |
| **Strategies that failed** | Sustained shorts (got squeezed repeatedly); fading the cycle (each new ATH extended further) |
| **Notable** | First crypto cycle with significant institutional flow throughout. Question whether the 4-year halving cycle is *still* the dominant clock or whether ETF flow has decoupled it remains live empirically. |

### Regime 15 — Post-ATH Bear (Oct 2025 – present; classified 2026-06-11)

**This entry was a live-tracking placeholder until 2026-06-11; H1 2026 is now a complete, classifiable episode.** It is also a first for this page: every prior regime is *reconstructed* with hindsight; this one was **watched** — the causal-detector trail below is contemporaneous filtering-mode output (per [[foundation/methodology/online-regime-detection]]), not retrospective labelling.

| Field | Value |
|-------|-------|
| **Dates** | October 6, 2025 (ATH $126,198) → present (June 2026) |
| **Price** | $126,198 → ~$63k by 2026-06-11 (**~−50%**) |
| **Defining events** | June 2026 sell-off; Strategy's **first BTC sale in ~4 years** (32 BTC, May 26–31, funding preferred-stock distributions — materially negligible, signature-wise loud); Mt. Gox 10,422 BTC ($739M) administrative transfer Jun 2 ahead of Oct 31, 2026 repayment deadline; record 13-trading-day BTC-ETF net-outflow streak (~May 15 → Jun 3/4, ~$4.33–4.4B / ~59,400 BTC; AUM $104.29B → ~$80.40B; ended Jun 5 with a $3.05M inflow but **did NOT flip to an inflow regime** — see June-episode markers below) |
| **Macro context** | US-Iran conflict oil shock (Hormuz disruption; energy +23.5% YoY); May CPI **4.2%** (highest since Apr 2023, 3rd consecutive acceleration, released Jun 10); rate-cut hopes evaporated, hike-risk debate into **Warsh's first FOMC Jun 16-17**; capital rotating to AI equities while BTC −50% |
| **Realized vol** | Elevated → panic vol-cluster (causal vol-state flagged panic 2026-06-08) |
| **Trend state** | Below 200-day SMA from Apr–May 2026; `macro_bull` gate (price > 200d SMA from below) off the entire 2026-Q2 quarter |
| **Funding** | Negative-leaning through the sell-off (read with the ETF-era carry-distortion calibration flag, dimension 3) |
| **Sentiment** | Fear & Greed **8–12 extreme fear** (Jun 6–11; fell from 52 a week before Jun 6) |
| **Treasury-vehicle mode (dim. 8)** | Forced-selling signature **flickered live, then partially reversed** — periphery DATs forced to liquidate (first confirmed Mode-3 evidence) but core reversed within ~10 days (Strategy dip-buying ~Jun 9, miner re-accumulation since Jun 5). See [[foundation/macro/treasury-vehicle-cascade-watch|cascade-watch status update 2026-06-12]] |
| **Kindleberger stage** | Hypothesis (a) from the placeholder resolved correct: **post-Euphoria Distress → Panic-leaning**. Whether this is a full Panic (Regime-7-analog) or a deep mid-cycle correction (Regime-4/12-analog) is the live question; −50% is already deeper than both mid-cycle precedents (−55% May 2021 was the deepest) |
| **Wyckoff CO phase** | Distribution (Oct 2025 – Apr 2026) → markdown |
| **Strategies that worked** | Short-side / with-the-downtrend mechanisms; de-risk discipline (the human-gate function the state-monitor was built for) |
| **Strategies that failed** | Bull-specialist regime metas (the structural bear weakness measured in `regime_causal_final`: −1.3 Sharpe OOS in bears); buy-the-dip without confluence; breakout longs ([[trading/mechanisms/breakout-direction-asymmetry-by-macro|bear-window gate]] now in force) |
| **Notable** | **Causal detector trail (from the monitor's quarter digest — watched, not reconstructed)**: bull regime through Apr–May *below* the 200d SMA → regime flip bull→bear **2026-05-20** → slow_up ~20d → range_mixed **06-01** → slow_down **06-08** → panic vol-cluster **06-08**; **30 confirmed 4h breaks down / 0 up** over the quarter. The 2024-? cycle's "extended Boom vs cycle top" open question is leaning resolved: Oct 2025 increasingly looks like the cycle high. Re-verify this entry once the episode completes (per the hindsight caveat — contemporaneous classification, subject to revision). |

#### June-episode markers (contemporaneous trail, added 2026-06-12) {#regime-15-june-markers}

The June leg is the first regime episode the wiki accumulates *as it happens* via the causal detector. Markers below are dated + sourced; the bottom-zone stack is **flagged, NOT a bottom call** (the page's hindsight discipline forbids forward calls).

**Detector led the narrative.** The causal detector flipped bull→bear **2026-05-20 03:59** — 5 days after the ETF outflow streak began (May 15) and ~2 weeks before the headline cascade (Jun 3–6) and the mainstream-media narrative caught up. This is the cleanest worked example on the page of filtering-mode detection preceding the story (per [[foundation/methodology/online-regime-detection]]).

**Jun 5 confluence** (selling-exhaustion cluster, logged same-day): monitor's quarter-low **$59,080** + ETF outflow streak ending + public-miner flip to net accumulation — three independent markers on one date.

**Cascade magnitudes:** $1.8B liquidations in one day (largest since Feb 2026; $1.35B longs); a separate $1.1B episode on the $64k break; $1.6B in the Jun 5–6 rout.

**Bottom-zone marker stack (flagged Jun 12, NOT a bottom call):**
- Supply-in-loss (10.5M BTC) > supply-in-profit (9.8M) — crossover that flashed at every prior bear bottom (CoinDesk 2026-06-04)
- First **200-week-MA tag** of the cycle (BeInCrypto)
- LTH-SOPR < 1.0 with high-conviction holders selling at a loss (CNBC 2026-06-03)
- Fear & Greed 10–13 ≈ 2018/2022 bottom prints
- Google search interest below prior bear-cycle baseline (Stocktwits 2026-05-17 — pre-dates the June leg)

**Counter-markers (why "bottom isn't in" is a live read):** zero confirmed up-breaks May 13–Jun 12 (causal digest); funding ~flat (no short-squeeze fuel); 30d ATM IV ~41.4% with deeply negative risk reversals (QCP via NewsBTC 2026-06-08: "bottom isn't in"); small-retail dip-buying *vs* 10–10k-BTC wallet distribution (~21,881 BTC / 9d, Santiment W1-June) — a divergence that historically resolves against retail.

**ETF-flow falsifier state (as of 2026-06-12):** streak of 13 trading days (~$4.33–4.4B, ~59,400 BTC; IBIT ~75% of streak / FBTC −$456M) ended Jun 5 (+$3.05M, effectively flat) — but **NOT an inflow regime**: Jun 8 −$91M (with +$82M into ETH ETFs same day — a BTC→ETH rotation), Jun 9 −$77M IBIT-led; outflows decelerated ~10× from streak pace, bid not returned. YTD spot-BTC-ETF flows flipped negative for the first time since the 2024 launch. AUM $104.29B → ~$80.40B; holdings ~1.277M BTC, −7.2% off the Oct-2025 peak. **Falsifier arithmetic unchanged: max streak 13 trading days vs the 90-day trigger — far from firing.** This is the falsifier guarding the L1 HODL sleeve (paper opening tranche activated this session). Watch: streak restarts. Cross-ref [[foundation/macro/treasury-vehicle-cascade-watch#status-update|cascade-watch §Status Update]] (the miner/Strategy/ETF items overlap).

**Macro overlay:** Iran war / Hormuz closure → May CPI **4.2% y/y** (3-yr high; energy +23.5%) → Warsh's first FOMC **Jun 16–17** (~99% hold priced; the live question is a bias-shift in the dot-plot). BoJ hike to 1.0% expected (carry-trade-unwind watch). The crypto de-rating was **idiosyncratic**: BTC −12% in the week global equities hit records (CoinDesk 2026-06-03) — the BTC↔NDX risk-on correlation that anchored the May bullish snapshots broke.

**Reproducibility:** `monitor.py digest quarter` reproduces the regime trail (bull Apr 11 → slow_up Apr 27 → local peak ~$82k May 14 → bear flip May 20 → low Jun 5).

### Live regime snapshot — May 4, 2026 (from [[foundation/methodology/investopedia-daily-ingest|Investopedia daily-ingest]] May 4 entry)

**Equities (anchoring crypto correlation regime)**:
- S&P 500: 7,230.12 (fresh ATH, May 1 close; **6th consecutive weekly gain**)
- Nasdaq Composite: 25,114.44 (ATH; **6th consecutive weekly gain**; AI-tech-driven)
- Dow Jones: 49,499.27 (lagging — narrow leadership signature)

**Earnings expectations**:
- S&P 500 Q1 2026 expected +14% YoY
- Full-year 2026 revised to +18.7% (up from ~15% at start of year — *earnings revision creep* signature)
- Apple Q2 (May 1, post-close): iPhone revenue +21.7% YoY — **supply-constrained beat** (chip supply capping reported revenue; underlying demand stronger than headline)

**Macro context**:
- Magnificent-7 + AI-tech-driven leadership; broader breadth narrowing (DJI vs SPX divergence)
- Earnings season ongoing; Apple beat May 1 reinforced rally
- **Geopolitical (load-bearing context for 2026)**: US-Iran was in active war earlier in 2026; current regime is *de-escalation rally* via Pakistan-mediated peace talks. Persian Gulf oil-export resumption priced in. Market absorbing geopolitical news without weakness ([[foundation/market-wisdom/tape-reading|tape-reading bullish-underlying-state signal]]).
- **Cross-asset cascade**: oil declining on peace-talk progress → inflation expectations easing → Fed easing more likely → risk-on tailwind across equities + crypto
- Calendar this week: April NFP just released; manufacturing data; major earnings (Palantir, AMD, ARM)

**Stage classification suggestion** (live, contemporaneous):
- **Late Boom or early Euphoria** signatures present: ATH on cap-weighted indices; sustained-streak count (6+ weekly gains); narrowing breadth; earnings-expectation creep; geopolitical news being absorbed without weakness; supply-constrained beats reinforcing demand-durability narrative.
- BTC correlation with NDX during ATH risk-on phases historically high → **BTC bias regime-aligned bullish** at this snapshot.
- **Caveat on de-escalation rally**: this regime emerged from war (earlier 2026) → de-escalation. Such regimes can reverse rapidly if peace talks fail. The current bullish signature is conditional on continued de-escalation progress. Sit-tight discipline applies but with explicit war-resumption invalidation criterion.
- This snapshot is **macro / equity context**, NOT BTC-specific marker reading. For BTC-specific markers (funding, OI, on-chain whale flow, BTC.D), separate observability sources required (Glassnode, Coinglass, CryptoQuant).

**⚠️ Update — Monday May 4 (PM): correction + concrete markers** (from second ingest May 4)

The "de-escalation rally" / "oil exports expected to resume" framing above is **directionally premature**. Current state per Yahoo Finance / Bloomberg / Al Jazeera / CNBC May 4 coverage:

- **Hormuz remains closed** in active war state. Iran rejected Trump's "Project Freedom" escort plan announced Sunday.
- **Brent ~$108/barrel**, flat after initial 2.4% decline on plan announcement. Goldman: 14.5M bpd offline. IEA: "biggest energy disruption in history."
- **Cross-asset cascade as macro tailwind** (May 4 first ingest, principle #4) has a constraint not noted there: works only when energy transmits relief. With oil staying $100+ under active disruption, cascade does NOT transmit; oil-up offsets Fed-easing-bias.

**Concrete current markers (Mon May 4 open)** — supplement to above snapshot:

*Vol & risk*: VIX 17.39 (+2.36%); Gold $4,598 (-1%); SPX/NDX futures grinding higher (+0.17%/+0.36%); DJI futures flat (divergence persisting).

*Crypto-specific* (the missing layer in May 4 first ingest):
- BTC ~$78,000-$78,200 (May 1-2)
- BTC dominance: 58.5%
- Glassnode True Market Mean: ~$79,000 — BTC currently *below* (regime-uncertainty zone)
- BTC short-term holder cost basis: $80,700 — bull-trigger reclaim threshold
- Strategy/MSTR: 818,334 BTC at avg $75,537 (~$61.8B); buying *paused* ahead of May 5 Q1 earnings (treasury-vehicle demand carrier temporarily offline)
- April was strongest crypto-ETF month of 2026

*Fed regime quality*: 8-4 hold at 3.5-3.75% on April 29 — most dissents since 1992 (Miran, Hammack, Kashkari, Logan against easing-bias). Powell remains on Fed board after May 15 chair end (unprecedented since 1948); Warsh confirmed; first FOMC June. **Dissent quality (count vs historical norm) is itself a vol-regime marker** — widens forward-rate distribution; structurally supports vol risk-premium.

*Calendar*: April NFP Friday (forecast 60K vs prior 178K — slowdown signature); MSTR Q1 May 5; LSCC/AMD/ARM/PLTR/PSKY this week.

**Refined stage-classification call** (Mon May 4): regime is **mixed late-Boom signatures with Distress-stage stress overlay**. Cap-weighted ATH + earnings-creep + breadth narrowing all argue late-Boom; Hormuz-closure + Fed-dissent quality + DJI lag + BTC below True Market Mean argue early-Distress stress not yet priced in equity-index level. The signature mismatch (equity ATH + commodity-driven inflation pressure + below-cycle-mean BTC) is itself a regime marker — not pure Boom, not full Distress. **Sit-tight discipline applies but with explicit war-resumption / oil-shock-amplification invalidation criterion.**

**⚠️ Update — Friday May 8 (catch-up, covers May 5-8)** (from third ingest May 8)

The "mixed late-Boom signatures with Distress-stage stress overlay" framing from the May 4 PM update **partially resolved upward** as the week progressed: equity-side signatures strengthened (record SPX/NDX close May 7), Iran fragile-ceasefire produced a 3-day rally before partial unraveling Thursday-Friday, and a **strong April NFP beat** unwound the labor-weakening narrative. Crypto-side stayed mid-range. The signature mismatch (equity strength + commodity volatility + below-cycle-mean BTC) persists but tilted further toward late-Boom.

**Concrete current markers (Fri May 8 mid-session)** — supplement to above snapshots:

*Equities & vol*: S&P 500 7,365.12 (May 7 record close, +1.5%); Nasdaq 25,838.94 (+2%); Dow 49,910.59 (+1.2%, +612pts on the day); VIX 17.38 area (+/- through week, not panicked); DJI/SPX/NDX divergence narrowing this week (DJI participated in Iran-deal rally). Q1 earnings: 84% beat EPS, 81% beat revenue, blended growth 27.1%.

*Macro & oil*: Brent recovered to $101.26 after sub-$100 mid-week dip on ceasefire hopes; WTI $95.64. **April NFP ~178K vs ~62-65K consensus** (release today May 8) — significant hawkish surprise; "first back-to-back monthly increase in nearly a year" framing; unemployment likely 4.3%. 10y Treasury ~4.40% (tested March highs 4.42% earlier in week). Fed funds futures pricing **no cuts** for remainder of 2026. "Sell in May" historical seasonal: avg +2% S&P May-Oct since 1945.

*Geopolitical*: Iran ceasefire officially "in effect" but fragile after May 8 fire exchange (US air strikes hit Bandar Khamir/Sirik/Qeshm Island; Iran said US targeted oil tanker at Fujairah). Trump on ABC: "just a love tap." Three-state framing (active-war / paused-ceasefire / resolved) — currently **paused-ceasefire** with re-escalation risk.

*Crypto*:
- BTC: $79,948-$80,836 range; briefly tagged $81K (first time in 3+ months); ~37.6% below Oct 2025 ATH of $126,198
- BTC dominance: 58.5-61% (tracker divergence)
- ETF flows: 3 consecutive days positive through May 4 ($532M; IBIT $335M / FBTC $185M)
- **April was strongest BTC-ETF month of 2026**
- IBIT holdings: ~812K BTC (~3.8% of total supply); IBIT+FBTC capture >60% of net new flow
- **MSTR strategic pivot (May 5)**: Q1 net loss $12.54B; from "never sell" to "actively manage Bitcoin to maximize per-share value"; floated selling BTC to fund dividend obligations. Holdings: 818,334 BTC at avg $75,537 ($61.81B cost; ~$64.14B mkt value May 1)
- **BNY Mellon (largest US custodian) launches BTC/ETH custody in Abu Dhabi (May 7)** — institutional-on-ramp infrastructure milestone

*Treasury-vehicle regime*: shift from accumulate-mode (pre-May-5) to actively-manage-mode (post-May-5). Demand-carrier role weakened; potential forced-supply windows on dividend dates / earnings blackouts. **N=2 evidence** for treasury-vehicle-as-carrier principle (May 4 buy-pause + May 5 strategic-shift). See [[foundation/methodology/investopedia-daily-ingest|May 8 ledger entry]] for refined two-mode framing.

**Refined stage-classification call (Fri May 8)**: regime is **late-Boom-with-distress-stress-residual**. Equity ATH + 84% earnings-beat + hawkish-NFP-beat-shrugged-off all argue late-Boom continuing; fragile Hormuz ceasefire + Fed-dissent-quality (4 dissenters) + BTC below cycle-mean + MSTR-pivot-to-actively-manage argue residual stress not yet reflected at index level. **Sit-tight discipline applies but with explicit invalidation criteria**: (a) Hormuz war-resumption + sustained oil >$110; (b) MSTR/treasury-vehicle forced-selling cascade (if multiple vehicles enter actively-manage mode synchronously); (c) NFP-style hawkish prints + equity selloff (regime change from "shrug-off" to "respect-the-data"). **Until invalidation triggers, bullish bias persists**.

**⚠️ Superseded (2026-06-11 lint)**: the May-8 invalidation criteria subsequently fired in part — (a) the Iran oil shock fed May CPI 4.2% (energy +23.5% YoY); (b) the forced-selling *signature* went live at N=1-vehicle (Strategy's first sale in ~4 years). The "bullish bias persists" call is closed; current classification is the Regime 15 entry above (Distress → Panic-leaning, −50%). The May 4–9 blocks are preserved as the contemporaneous classification trail — a worked example of live stage-calling under uncertainty, including what it got wrong.

**⚠️ Sharpening — 2026-05-09 (treasury-vehicle marker + funding-rate calibration)**

Two regime-marker dimensions added/refined per [[foundation/macro/treasury-vehicle-cascade-watch|treasury-vehicle cascade-watch]] N=2+ codification:

**(1) New 7th regime dimension: treasury-vehicle-mode.** Public-BTC-treasury-vehicle (MSTR, miners, sovereigns) operating mode is now a regime-relevant marker on par with funding/OI/stablecoin/BTC.D/dollar-liquidity. Three states: **accumulate-mode** (structural demand floor), **actively-manage-mode** (mixed flow / structural supply), **forced-selling-mode** (structural supply shock; not yet observed at scale). Current state Q2 2026: **mixed actively-manage** — MSTR + Riot + miners aggregate confirmed actively-manage; ETF demand absorbing supply-drag; falsifier (3+ top-10 synchronous) not yet live. **Track this dimension on quarterly-earnings cadence** when public companies disclose treasury changes.

**(2) Funding-rate marker calibration flag.** The funding-rate dimension can no longer be read literally in 2024+ regime: live -5% 30d-avg funding can be **institutional cash-and-carry** (long-ETF / short-futures basis trade) rather than bearish positioning. Pre-ETF playbook treated negative funding = bearish-positioning; post-ETF playbook requires **supplementary on-chain confluence** to disambiguate. **Wiki page landed 2026-05-11**: [[trading/mechanisms/etf-era-carry-trade-funding-distortion]] documents the structural distortion mechanic in full + 2026-05-11 `meta_funding_v1` empirical confirmation (fade-extreme-positive-funding 0.4% cohort full-positive in institutional-bid regime). Companion sharpening landed on [[trading/mechanisms/funding-cycle-dynamics#regime-conditioning-etf-era|funding-cycle-dynamics §regime-conditioning-etf-era]] under Positioning-extreme. Reflects the broader 2024-? cycle pattern that pre-ETF cycle markers need recalibration before naive application.

The regime-marker dimensions table now has **8 dimensions** (prior 7 + treasury-vehicle-mode):
1. Price vs 200d MA + slope
2. Realized vol regime
3. Funding profile (with 2024+ ETF-distortion calibration flag)
4. OI dynamics
5. Stablecoin supply trajectory
6. BTC dominance (BTC.D)
7. Dollar-liquidity overlay (per Hayes 2026-05-08 PM-2 ingest)
8. **Treasury-vehicle-mode (per 2026-05-09 N=2+ codification)** — accumulate / actively-manage / forced-selling state of top-10 public BTC treasuries

**Updates expected as daily ingest progresses** — see [[foundation/methodology/investopedia-daily-ingest|the ingest ledger]] for the running marker history.

## Cycle Comparison

| Cycle | Cycle Low | Cycle High | Duration | Drawdown | Macro context |
|-------|-----------|-----------|----------|----------|---------------|
| **2017 cycle** | $0.03 (2010) → $19,500 (Dec 2017) | $19,500 → $3,200 (Dec 2018) | ~5y up; 1y bear | -84% | Pre-institutional retail-driven |
| **2020-2022 cycle** | $4,000 (Mar 2020) → $69,044 (Nov 2021) | $69,044 → $15,500 (Nov 2022) | 20m up; 12m bear | -77% | COVID stimulus + DeFi + retail explosion |
| **2024-? cycle** | $15,500 (Nov 2022) → $126,198 (Oct 6, 2025 — increasingly looks like cycle high) | $126,198 → ~$63k (Jun 2026, ongoing) | 35m up; 8m+ bear so far | **−50% and ongoing** | First ETF cycle; institutional flow dominant on the way up; ETF outflows + treasury-vehicle drag on the way down |

The 2024-? cycle has **diverged from prior 4-year halving cycle pattern**:
- Prior cycles peaked ~18 months post-halving (April 2024 halving → Oct 2025 = 18 months — *consistent* with halving timing but cycle remains in expansion)
- ETF flow may be decoupling cycle from halving (institutional flow doesn't respect 4-year retail cycle)
- Live empirical question: is this an extended Boom phase (ETF-driven structural expansion) or has the cycle topped Oct 2025?

### Halving Diminishing Returns (2024 update — added 2026-05-05)

The 2024 halving (Apr 19, 2024) one-year-later reading shows **diminishing returns** vs prior epochs — empirically supported per Fidelity Digital Assets / ARK Invest / Kaiko analyses 2025-2026:

| Halving epoch | Halving date | 1y-later price | 1y-later return | Pre-halving 1y vol | Post-halving 1y vol |
|---|---|---|---:|---:|---:|
| **2nd (2012)** | Nov 28, 2012 | $13.45 → $1147 (Dec 2013) | (different methodology pre-cycle) | ~200% | ~150% |
| **3rd (2016)** | Jul 9, 2016 | $663 → $2552 (Jul 2017) | **+285%** | ~70% | ~65% |
| **4th (2020)** | May 11, 2020 | $9500 → $63500 (May 2021) | **+567%** | ~60% | ~85% |
| **5th (2024)** | Apr 19, 2024 | $63762 → $83671 (Apr 2025) | **+31%** | ~50% | ~50% |

**Pattern:**
- **Diminishing percentage returns** — as market cap grows, percentage gains compress. Each successive halving produces ~half the percentage return of the prior cycle's same-time-after-halving mark
- **Volatility compression** — drop from ~200% (2012) to ~50% (2024-25); institutional bid + ETF flow + larger market cap all contribute
- **2024 1y-later +31%** is meaningfully smaller than the prior cycles' percentages would have predicted (extrapolating 2012→2016→2020 trend, would have expected +400% → +200% → +100% successive compression; +31% is sharper compression than that trend predicted)

**Open question (already flagged in regime 14 + 15 entries):** is the 4-year halving cycle broken or paused? Live empirical question. The +31% reading is *one data point*; final cycle picture won't resolve until cycle high is in (still possible the 5th-epoch cycle extends and produces more upside). What's *settled*:
- Halving's *causal* importance has diminished (ETF flow is the larger driver in this cycle)
- The 4-year-cycle-as-precise-clock framework has weakened; cycle dynamics are now better described by ETF flow trajectory + macro liquidity than by halving timing

**Implication for cycle-based strategies**: pure halving-cycle plays (long ahead of halving; short ahead of cycle high projected from halving date) are weakening as a framework. Mechanism-grounded strategies that incorporate ETF flow + macro liquidity perform better than pure-halving-clock strategies. Per [[trading/mechanisms/calendar-effects|calendar-effects ETF flow regime indicator subsection]]: ETF flow trajectory is the operational regime classifier; halving date is one input among several rather than the dominant clock.

**Sources**: Fidelity Digital Assets "2024 Bitcoin Halving: One Year Later"; ARK Invest "Bitcoin Cycles, Entering 2025"; Kaiko "Bitcoin's Halving Anniversary: This Time Was Different"; 2026-05-05 W1 research arm output.

## Regime-Strategy Mapping Table

What worked when (operational reference):

| Strategy family | Boom regimes worked in | Distress / Panic regimes worked in | Chop regimes worked in |
|-----------------|------------------------|-------------------------------------|------------------------|
| **Trend-following + breakouts** | All Boom regimes (3, 5, 10, 11, 13, 14) | Failed | Failed |
| **Range-trading / mean-reversion** | Failed | Failed during cascades; worked in late-Distress accumulation | All chop regimes (2, 9, 12) |
| **[[trading/mechanisms/funding-cycle-dynamics|Funding-cycle carry]]** | Worked persistently in regimes 3, 5, 10, 11, 13, 14 | Funding flips during cascades — squeeze opportunities | Marginal returns; modest funding |
| **[[trading/mechanisms/accumulation-distribution|Wyckoff spring/upthrust]]** | Limited (markup phases don't have cascades) | **Highest-leverage regime** — cascades produce springs | Range-bound springs/upthrusts at boundaries |
| **[[trading/mechanisms/group-leader-divergence|Group-leader divergence]]** | Useful late-Boom (alt rotation) | Most-actionable in Distress (BTC.D rotation) | Limited use |
| **Volatility-selling (options)** | Failed (vol expansion) | Failed (vol expansion) | **Optimal regime** (vol compression) |
| **Long-side carry on alts** | Worked in alt-season phases (late 3, late 5, late 14) | Failed | Limited |

### Deployment-Grade Sharpening (2026-05-05 PM-3 stocktake)

The above table describes *which regimes a strategy family operates in statistically*. The 2026-05-05 deployment-grade stocktake adds a sharpening: **statistical regime-fit ≠ deployment-grade readiness.** Per [[foundation/methodology/deployment-edge-threshold|deployment-edge methodology]] applied to existing strategy inventory:

| Strategy class | Statistical regime fit | Deployment-grade verdict at $1K capital |
|----------------|------------------------|----------------------------------------|
| **Trend-following + breakouts** (e.g. ema_bias_breakout family) | Validated mechanism in Boom regimes; bias-filter > separate regime gate | **Research-grade in trends; deployment-grade fail under realistic friction** — see [[trading/rejected/ema-bias-breakout-4h-symmetric-2026-05]] (Net −$50/yr); [[trading/rejected/ema-bias-breakout-4h-v2-2026-05]] (Net −$24/yr verdict reversal) |
| **Mean-reversion (vol-cycle / cascade-aftermath)** (e.g. bollinger_squeeze, wyckoff_spring) | Validated mechanism in chop / cascade regimes | **Mechanism real, edge-below-threshold without portfolio-stacking** — bollinger short_only +$8/yr (closest-to-passing); wyckoff_spring −$153/yr (capability-gap-blocked) |
| **Session-boundary** (e.g. asia_range_sweep) | Should work in chop regimes | **Failed to operate in 2025-26 BTC regime** — per [[trading/rejected/asia-range-sweep-4h-2026-05]] (calibration failure: ETF launch shifted volume to NY; Korean premium compressed) |
| **Funding-cycle carry** (a funding-carry strategy — live system) | Works persistently in carry-positive regimes | **Productionize-grade** — different mechanism class (carry, not flip-recovery); live trading evidence per Performance section |
| **Funding-flip recovery** (mutation experiments, all rejected) | Asymmetric edge real on short side in bearish regime | **All variants below threshold** — symmetric, short_only, regime-gated: all rejected. Mechanism remains research direction needing different framing |

**Core finding from PM-3 stocktake**: of 13 strategies tested, **0 cleared the 12%/yr ($120/yr at $1K) deployment-grade threshold**. 1 borderline (bollinger short_only +$8/yr); 8 outright rejected; 1 inconclusive (selection bias); 1 archive (control mirror); 2 already rejected in earlier batch. The mechanism work is mostly statistical-grade validated but deployment-grade requires different design parameters (lower turnover OR larger absolute edge per trade OR less retail-accessible mechanism). Per [[foundation/methodology/deployment-edge-threshold|methodology §8 mechanism implications]].

This is consistent with the [[foundation/macro/regime-conditional-edge-2025-26|W2 finding]]: 13/13 strategies' edge concentrated in 2025-Q4 + 2026-Q1; 5-6 month edge × full-year friction structurally compresses net edge below threshold even when statistically validated.

## Honest Caveats {#caveats}
- **Cycle vs cycle calibration**: each cycle has cycle-specific features. 2017's Korean premium gone; 2020-2022's NFT/algorithmic-stable mania gone; 2024-?'s ETF flow may reset the 4-year clock. Apply this taxonomy as historical reference; *don't* assume future cycles repeat exactly.
- **Regime boundaries are fuzzy**: dates above are approximate. Real regimes transition over weeks; the boundary date is the most-distinctive event.
- **Single-asset focus**: this is BTC-specific. Alt regimes can diverge (alt season vs BTC-led periods). For alt-specific regimes, separate taxonomy required.
- **Hindsight bias**: this taxonomy is written *retrospectively*. Live regime classification requires committing to current-regime call *before* future data is available. **Regime 15 is the exception** — classified contemporaneously via the causal (filtering-mode) detector trail, the first episode on this page that was watched rather than reconstructed. It remains subject to re-verification once the episode completes.
- **Marker calibration evolves**: "extreme funding" was >50bp/8h in 2021; by 2024 it might be >25bp/8h (compression of historical extremes). Re-calibrate marker thresholds per cycle.
- **AI training corpus dominance**: the 2020-2022 cycle is heavily covered in AI training data. 2023-2025 specific events less so. For live trading decisions, *re-verify all dates and price levels from primary sources* — don't trust this page's specifics without confirmation.
- **Survivorship bias in "what worked"**: the "strategies that worked" column lists strategies that appeared to work *during the regime in retrospect*. Real-time classification required at the time would have been imperfect. The honest framing: "strategies whose regime-aligned use should have been favored" rather than "strategies guaranteed to win."

## How To Use This Page (Reprise)

For backtest interpretation, work through the 5-step protocol from the top:
1. Identify regime windows in backtest period
2. Slice by regime — per-regime returns
3. Identify regime-dependence
4. Match current to historical
5. Position size

For live regime classification, walk through the marker set:
1. Where is BTC vs 200-day MA? (regime direction)
2. What's realized vol percentile? (vol regime)
3. What's funding rate average over 30 days? (positioning regime)
4. What's OI trajectory vs price? (positioning intent)
5. What's stablecoin supply trajectory? (macro liquidity)
6. What's BTC.D direction? (internal-class regime)

Combine into a regime classification matched against the historical taxonomy. **Operationalizes [[foundation/market-wisdom/bubble-cycle-stages|Kindleberger 5-stage classification]] with concrete BTC examples**.

## Key Takeaways

- **15 distinct regimes** identified from 2020 Q1 through 2026 Q2.
- **Markers are the regime classifier**: 200-day MA, realized vol, funding, OI, stablecoin supply, BTC.D, LTH cost basis.
- **Different strategies dominate different regimes** — the regime-strategy mapping table is the operational gate for strategy selection.
- **Mid-cycle corrections** (May 2021, Aug 2024) look like full Distress → Panic but resolve within months. Distinguishing requires cohort-flow + multi-cascade-vs-single-event analysis.
- **2024-? cycle has diverged from prior halving-anchored pattern** — ETF flow as new dominant driver. Empirical question whether 4-year cycle is broken or just paused.
- **Crypto has no LOLR**: cycle bottoms come via price exhaustion + multi-cascade exhaustion, not central-bank rescue. Drawdown depth (-77% 2022) consistent.
- **Hindsight bias warning**: live regime classification requires contemporaneous discipline. Regime 15 (2026-H1 bear) is the first episode classified contemporaneously — via causal detector trail, reproducible with `monitor.py digest quarter` — rather than reconstructed.
- **Re-calibrate thresholds per cycle** — "extreme funding" levels evolve as market structure evolves.

## Hayes contemporary calibration — Dollar liquidity overlay

Per [[foundation/sources/hayes-essays|Hayes Substack ingest 2026-05-08]], **5 of 9 curated essays converge on dollar-liquidity → BTC transmission** as the primary macro driver. The wiki's existing regime taxonomy (vol regime + trend state + funding profile + OI dynamics + stablecoin supply + BTC.D) gains a sixth-and-arguably-load-bearing dimension: **dollar-liquidity regime**.

Operational regime markers from Hayes (composite across Hallelujah / The BBC / Frowny Cloud / Fatty Fatty Boom Boom / Long Live the King!):

| Marker | Reading | Regime implication |
|--------|---------|-------------------|
| **MOVE Index** | >130 | Money-printing trigger threshold likely; bond-vol regime indicating forced-Fed-intervention proximity (per The BBC) |
| **SRF (Standing Repo Facility) usage** | Cumulative growth | Stealth-QE conduit active (per Hallelujah; Cayman RV hedge funds intermediated $1.2T treasury issuance Jan 2022 – Dec 2024) |
| **RRP balance** | Depleted; shifting to T-bills | Fiscal-dominance liquidity injection active (~$2.5T drained from RRP into markets; per Long Live the King!) |
| **Asymmetric QT** | 10y-Treasury preferential purchases | Covert long-yield suppression (per Fatty Fatty Boom Boom) |
| **Asian carry-trade unwind** | TWD/KRW strengthening | Capital repatriation forcing dollar selling (per Fatty Fatty Boom Boom) |

**Multi-currency calibration** (per Long Live the King!): the four-year halving cycle is *regime-conditional* — appears to "fail" in current regime because **both dollar AND yuan systems are easing simultaneously**. 2015-17 cycle was driven by Chinese credit impulse, not Fed; halving-cycle framework should be augmented with a multi-currency credit-impulse view.

These markers belong as live-regime entry tags when next updating the per-quarter regime table above. See [[glossary/dollar-milkshake-thesis]] for the canonical macro framing.

## Related

- [[foundation/macro/btc-state-card]] — **added 2026-06-27**: the living BTC state card — Regime 15's current objective state updated each daily-review iteration (this episode, watched live)
- [[foundation/methodology/online-regime-detection]] — **added 2026-06-07**: this taxonomy is written retrospectively (only the 2026-Q2 entry is contemporaneous — the hindsight caveat below IS the *smoothing* problem). That page is the prospective (filtering) counterpart — how to call a regime *live* from past data only — and catalogues the free callable-now marker feeds (CoinMarketCap Fear & Greed / Altcoin-Season / BTC-Dominance) that supply this page's marker set in real time.
- [[foundation/sources/kindleberger-manias-panics-crashes]] — Kindleberger framework that this taxonomy operationalizes per regime
- [[foundation/market-wisdom/bubble-cycle-stages]] — 5-stage cycle that this taxonomy maps onto
- [[foundation/market-wisdom/minsky-three-finance]] — Ponzi-finance composition shifts across regimes
- [[foundation/market-wisdom/composite-operator]] — Wyckoff CO phase per regime documented in the taxonomy
- [[foundation/macro/crypto-2020-2022-cycle]] — full Kindleberger framework worked example for 2020-2022 cycle (regimes 1-7); this page extends to 2025
- [[foundation/macro/intermarket-analysis]] — Murphy framework; macro overlay informs regime classification
- [[trading/mechanisms/cyclical-timing]] — Lynch's cycle-position framing; regime taxonomy is the operational reference
- [[trading/mechanisms/accumulation-distribution]] — spring/upthrust setups have regime-specific frequencies
- [[trading/mechanisms/group-leader-divergence]] — BTC.D rotation diagnostics per regime
- [[trading/mechanisms/funding-cycle-dynamics]] — funding profile per regime is in the taxonomy
- [[trading/mechanisms/behavioral-positioning-unwind]] — positioning regimes per taxonomy
- [[trading/mechanisms/liquidation-cascade-aftermath]] — cascade events identified per regime
- [[trading/mechanisms/breakout-direction-asymmetry-by-macro]] — **added 2026-05-21**: BTC 4h breakout-class mechanism is regime-conditional (works R:R 1:2 in bull, fails in bear); regime classification at this page's per-quarter granularity gates the mechanism's deployment window
- [[foundation/market-structure/crypto-carrier-features]] — central catalog of carriers used in regime classification

## Sources

- [[foundation/macro/crypto-2020-2022-cycle]] — primary reference for 2020-2022 specifics
- Web research synthesis: BTC price history sources (Capital.com, Stealthex, CCN, Caleb & Brown, CoinDesk, CNBC) — verified 2024-2025 specifics including ETF approval Jan 11, 2024; halving April 19-20, 2024; ATH break March 5, 2024 ($73,750); Yen carry unwind August 5, 2024; Trump election Nov 5, 2024; $100K crossing Dec 5, 2024; 2025 ATH $126,198 on October 6, 2025
- On-chain markers: Glassnode, Coinglass, CryptoQuant, DefiLlama (referenced as observability sources, not directly ingested)
- **Live regime-marker feeds (added 2026-06-07)**: CoinMarketCap publishes callable-now (filtered) readings for several markers in this table — [Bitcoin Dominance](https://coinmarketcap.com/charts/bitcoin-dominance/) (BTC.D marker), [Fear & Greed Index](https://coinmarketcap.com/charts/fear-and-greed-index/) (vol-state + sentiment composite: price momentum, Volmex BTC/ETH implied vol, BTC/ETH options put/call), [Altcoin Season Index](https://coinmarketcap.com/charts/altcoin-season-index/) (internal-class rotation). Available via the CMC `/v1/global-metrics/quotes/latest` API. Honest-scope + cite-only verdict at [[foundation/methodology/online-regime-detection#practitioner-proxies]].
- **Regime 15 figures (verified 2026-06-11 in-session)**: [CoinDesk — ETFs end record outflow streak](https://www.coindesk.com/markets/2026/06/05/bitcoin-and-ether-etfs-end-record-multi-billion-outflow-streak) (13 trading days, $4.37B, AUM $104.29B→$80.40B, $3.05M inflow Jun 5); [CoinDesk — Strategy's bitcoin sale debate](https://www.coindesk.com/markets/2026/06/01/analysts-agree-strategy-s-bitcoin-sale-was-immaterial-differ-on-future-signals) + [Yahoo Finance](https://finance.yahoo.com/markets/crypto/articles/strategy-sells-bitcoin-first-time-130500239.html) (32 BTC, May 26–31, first sale since 2022, preferred-dividend funding); [CoinDesk — Mt. Gox moves 10,422 BTC](https://www.coindesk.com/markets/2026/06/02/mt-gox-moves-10-422-bitcoin-worth-usd739-million-to-a-new-wallet-as-deadline-nears); [CNBC — May CPI 4.2%](https://www.cnbc.com/2026/06/10/cpi-inflation-report-may-2026.html); [Bitcoin.com News — Warsh first FOMC Jun 17](https://news.bitcoin.com/warsh-faces-his-first-test-june-17-as-traders-hunt-for-hidden-signals-in-the-feds-dot-plot/); Fear & Greed 8–12 extreme fear ([Bloomingbit](https://en.bloomingbit.io/feed/news/113762), [Cryptopolitan](https://www.cryptopolitan.com/crypto-fear-and-greed-index-drops-to-12/)). Detector trail reproducible via `monitor.py digest quarter` (cache holds 1m bars from 2024-05).
- **Regime 15 June-episode markers (added 2026-06-12 in-session)**: ETF post-streak state — [TechTimes — ETF outflows hit $4.4B across record streak](https://www.techtimes.com/articles/317833/20260605/bitcoin-etf-outflows-hit-44b-across-record-streak-fidelity-fbtc-among-funds-tested-nfp-looms.htm) (IBIT ~75% / FBTC −$456M; YTD flows negative first time since 2024) + crypto.news + BeInCrypto; Jun 8 −$91M with BTC→ETH rotation (KuCoin / news.bitcoin.com), Jun 9 −$77M IBIT-led. Cascade magnitudes: Investing.com ($1.8B liquidations / $1.35B longs), CryptoBriefing ($1.1B on $64k break), [CoinDesk Jun-6 rout](https://www.coindesk.com/markets/2026/06/06/) ($1.6B Jun 5–6). Bottom-zone stack: [CoinDesk — supply-in-loss > supply-in-profit](https://www.coindesk.com/markets/2026/06/04/) (crossover Jun-4), BeInCrypto (first 200-week-MA tag), [CNBC — LTH-SOPR < 1.0](https://www.cnbc.com/2026/06/03/) (Jun-3), Santiment W1-June (wallet-cohort divergence), QCP via [NewsBTC](https://www.newsbtc.com/) 2026-06-08 ("bottom isn't in"; 30d IV ~41.4%), Stocktwits 2026-05-17 (search interest). Idiosyncratic de-rating: [CoinDesk 2026-06-03](https://www.coindesk.com/markets/2026/06/03/) (BTC −12% as equities hit records).
- **Honest caveat repeated**: AI training corpus dominantly covers 2020-2022; 2023-2025 specifics are reconstructed from secondary sources. For live trading decisions, re-verify all dates and price levels.
- Live regime entry (2026-Q2) is contemporaneous classification — verify current markers against the taxonomy template at trade time
