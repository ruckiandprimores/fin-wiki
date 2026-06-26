---
title: "Deployment-Edge Threshold — Net-of-Friction Floor for Live Strategies"
status: growing
created: 2026-05-05
updated: 2026-05-15
tags: [foundation, methodology, deployment-edge, slippage, economic-significance, friction, net-edge, day-swing, position-sizing]
---

# Deployment-Edge Threshold — Net-of-Friction Floor for Live Strategies

> **TL;DR**: A strategy that passes statistical-significance gates (PSR, DSR, PBO; see [[foundation/methodology/strategy-validation-discipline|sister page]]) is **research-grade**. To be **deployment-grade** it must additionally clear the **net-of-friction edge floor**: gross edge per trade must exceed slippage + market impact + funding-rate friction by a margin large enough that the strategy is profitable at realistic capital + position scale. The 2026-05-05 batch surfaced the gap concretely — three strategies labeled "productionize candidates" on falsifier-pass grounds had gross median ATP $0.20–$1.50 on $1000 capital; realistic ~1bp roundtrip slippage on $5K positions eats most or all of that edge. This page articulates the discipline. **Companion to** [[foundation/methodology/strategy-validation-discipline|strategy-validation-discipline]] (statistical significance); together they form the complete validation gate.

## 1. The Research-vs-Deployment Edge Gap

Statistical significance and economic significance are **independent** failure modes:

| Pass statistical gate | Pass deployment gate | Verdict |
|:---:|:---:|---|
| ✅ | ✅ | **Deployment-grade** — worth live capital |
| ✅ | ❌ | **Research-grade only** — useful for mechanism mapping, partial-truth extraction, methodology validation; not for live capital |
| ❌ | ✅ | **Lucky window** — the high gross edge is likely a fluke; statistical gate exists precisely to catch this |
| ❌ | ❌ | **Reject** — graveyard |

The 2026-05-05 batch showed that **passing the statistical gate is *not* sufficient** for deployment. Three strategies passed pre-registered falsifiers ([[trading/rejected/ema-bias-breakout-4h-v2-2026-05|ema_bias_breakout_4h_v2]], [[trading/backtest-results/direction-restriction-short-only-2026-05|short-only mutation controls]]) with gross median ATP $0.20–$1.50 on $1000 capital. Realistic slippage at any productionize scale eats most or all of that edge.

> **Honest finding**: The research process marked these "productionize candidates" because they passed pre-registered Sharpe / %PF>1 / %val+ thresholds. Those thresholds were **statistical-significance thresholds**, not deployment-grade thresholds. The methodology gap was real and is now wiki-encoded.

## 2. Slippage Components

In live trading, the gap between backtest fill and live fill comes from five distinct sources. Most current backtests model only the first.

| Component | What it is | In the backtesting lab engine? |
|-----------|-----------|:---:|
| **Exchange fee** | Maker/taker fee charged by the venue | ✅ (0.04% in current grid) |
| **Bid-ask spread half-cross** | Difference between mid and execution price for marketable orders | ⚠️ Not modeled |
| **Market impact** | Price moves *because* of your order (size relative to depth) | ❌ Not modeled |
| **Latency cost** | Adverse selection from slow fills (price moves before order arrives at book) | ❌ Not modeled |
| **Funding-rate slippage** | Paying next 8h funding cycle when held across settlement (perp-specific) | ⚠️ Partial — strategies that close before settlement avoid this |

Per [[foundation/market-structure/bid-ask-spread-decomposition|Harris's spread decomposition]]: the spread is not just a transaction cost — it has a permanent (adverse-selection) component that compensates dealers for losses to informed traders. **Retail traders pay both components on every fill, regardless of whether they use limit or market orders.** This is the deepest reason most retail strategies systematically lose despite "good" backtest metrics.

Per [[foundation/market-structure/liquidity|Harris's four dimensions of liquidity]]: width (spread cost) and depth (size you can trade without moving price) jointly determine slippage. Crypto majors (BTCUSDT, ETHUSDT on top venues) have tight width but variable depth; alts have wider width AND thinner depth. Slippage estimates must be venue + asset + size + regime specific.

## 3. Position Sizing Math

The slippage cost depends on **position notional**, not capital. Position notional follows from risk-per-trade and stop distance:

```
position_notional = (capital × risk_pct) / stop_distance_pct
```

Worked at typical risk_pct = 1% and stop_distance_pct = 2.5% (≈ 1× ATR with 2.5× multiplier):

| Capital | Risk per trade | Stop distance | Position notional |
|---------|---:|---:|---:|
| $1,000 | $10 | 2.5% | **$400** |
| $10,000 | $100 | 2.5% | **$4,000** |
| $100,000 | $1,000 | 2.5% | **$40,000** |
| $1,000,000 | $10,000 | 2.5% | **$400,000** |

For strategies with tighter stops (e.g., 0.5× ATR = ~1% stop), position notional is ~2.5× larger at the same risk_pct.

**Implication**: a $1K-capital backtest with median ATP $0.60 has *implicit* position size around $400 at typical params. Friction on $400 is small in absolute terms but large *relative* to the $0.60 ATP. As capital scales up, position notional scales up linearly — so does friction. Edge has to scale up too.

## 4. Net Edge Formula

```
net_edge_per_trade = gross_ATP − friction_per_trade
friction_per_trade ≈ (fee_bps + spread_half_bps + impact_bps) × 2 × position_notional / 10000
                   + funding_per_trade  (if held across funding settlement)
```

Backtest engine already includes `fee_bps × 2` (8bp roundtrip at 0.04%/side). The methodology adds:

- **Spread half-cross**: ~0.5–2bp per leg on BTCUSDT majors (varies with vol regime + time of day); 1–4bp roundtrip
- **Market impact**: depth-dependent; on $400 notional in BTCUSDT essentially zero; on $400K notional possibly 1–5bp; scales nonlinearly with size
- **Funding slippage**: 8h cycle at typical 0.01% (1bp) per cycle = 1bp added to roundtrip if held one settlement; 3bp if held across three settlements

**Threshold derivation**: net edge must exceed friction noise with **margin**. Because slippage realizations are noisy (vol-regime dependent, time-of-day dependent), a 5× safety factor over base slippage is the working rule.

## 4.5. Estimation Methodology — Upstream (Predictive) vs Descriptive

**Critical distinction added 2026-05-05 evening** (per [[foundation/methodology/strategy-validation-discipline|Lopez de Prado Second Law]] §2.1: *"Do not research under the influence of a backtest."*):

The deployment-grade gate is the **upstream / predictive** estimate, NOT the post-test descriptive number. Same-data estimates are **overfit by construction** — measuring the same sample twice does not validate anything.

### Two estimate types — sharply different epistemic status

| Estimate type | Source | When computed | Validates? |
|---------------|--------|---------------|:---:|
| **Predictive** (deployment-grade gate) | Independent of test window | **Pre-test** (at hypothesis stage) | ✓ |
| **Descriptive** | Test window itself | Post-test | ❌ (post-hoc only; useful for description, NOT validation) |

A predictive estimate that the test confirms is *signal*. A descriptive number from the test window is *measurement of the test window* — circular if used as its own validation.

### Acceptable sources for predictive estimates

The `expected_net_edge` field in Part I §4 structured hypothesis MUST come from one of:

1. **Prior literature** — academic papers (with cited methodology), practitioner research (with documented backtest period), public datasets (with disclosed window). Source citation required.
2. **Different time window** — if backtesting 2025-26, estimate from 2024 or earlier. The estimation window must NOT overlap the test window.
3. **First-principles / Fermi estimate** — assumed return distribution × trade frequency × position size, derived theoretically from mechanism characteristics. No data fitting. Example: "if mechanism produces 5bp/trade on average and fires 50 times/year, expected gross is $25 on $1K with $5K position; friction $50/yr; net −$25/yr — research-grade-only at this scale."
4. **Strictly held-out OOS chunk** — 70/30 split with the 30% held out from ALL prior research touching this strategy. No peeking; no parameter tuning informed by held-out chunk.

### Worked failure case (same-data round-trip)

```
Researcher A:
  1. Scans 2025-26 BTC perp data for setups
  2. Notices "candles with body-to-range > 70% predict continuation 60% of time"
  3. Computes: at 50 trades/yr × 5bp gross × $5K position = $125/yr
  4. States expected_net_edge = $125/yr
  5. Backtests on 2025-26 data
  6. Result: $125/yr (matches estimate)
  7. Concludes: "validated"

Verdict: NOT VALIDATED. Steps 1-2 looked at 2025-26; step 3 estimated from
the same scan; step 5 measured the same window. The sample was used to
generate the hypothesis AND validate it. Per LdP Second Law, this is
"researching under the influence of a backtest" via implicit data peeking.
Expected and observed match because they're the same number.
```

### Worked clean case (independent-source predictive)

```
Researcher B:
  1. Reads Bailey & López de Prado 2014 paper on factor-zoo overfit risk
  2. Reads Han/Kang/Ryu 2024 on momentum in BTC perps with 2017-2023 data
  3. From cited literature: "trend-aligned breakout with EMA bias filter
     produced ~+0.3 Sharpe on similar setups in BTC 2017-2023 data."
  4. Computes Fermi: at 0.3 Sharpe, vol ~1%/day, expected return ~10%/yr
  5. States expected_net_edge = ~10%/yr at $1K capital ≈ $100/yr
     - source: Han/Kang/Ryu 2024 + Bailey/LdP 2014 methodology
     - value: $100/yr at $1K capital
     - independence: estimated window is 2017-2023; test window is 2025-26
  6. Backtests on 2025-26 data
  7. Result: $40/yr observed
  8. Reads result HONESTLY: "literature predicted $100; observed $40 in
     a different window; under-performance ratio 0.4x. Possibilities:
     (a) regime changed (2025-26 different from 2017-2023);
     (b) literature optimistic;
     (c) noise.
     Triangulating: regime-conditional-edge finding (Q4 2025 + Q1 2026)
     shows regime concentration; this strategy may underperform across
     full year vs literature."

Verdict: This IS validation. Estimation source is independent of test
window. Literature's $100 vs observed $40 is *informative gap* — pointing
toward regime non-stationarity, not validating the strategy at face value.
```

### Default to research-grade-only when independence is unclear

If the `expected_net_edge` field's `independence` sub-component is missing, ambiguous, or test-window-derived, the strategy defaults to **research-grade-only** verdict regardless of backtest result. The discipline is symmetric with falsifier discipline: a hypothesis with no falsifier is unfalsifiable; a hypothesis with no independent edge estimate is unvalidatable.

### Implications for existing artifacts (2026-05-05 retrospective)

- **2026-05-05 inventory strategies (13 tested in PM batch + earlier)**: their implicit `expected_net_edge` numbers are 2025-26-derived. Per [[trading/rejected/ema-bias-breakout-4h-v2-2026-05|the verdict-reversal worked case]], the deployment-grade reckoning was applied retrospectively. **Future strategies follow the new discipline from start; retroactive cleanup not required.**
- **Pattern catalogs derived from same-window data scans**: their implicit edge estimates are 2025-26-circular. When translated to strategies, falsifier backtest must use a different window (2024 or earlier, OR strictly held-out 30%) to avoid circularity. Pattern catalog entries are operational queue items in `tasks/research process/` per memory rule; sharpening notes flagged at research process level.
- **Pattern catalogs derived from external literature** (e.g. options-gamma, ETF-flow, halving-cycle from W1 research): clean provenance — estimates ARE independent. No retroactive change needed.

### What this discipline does NOT require

- Does NOT require *out-of-sample edge persistence* validation — that's a separate downstream gate (regime non-stationarity, walk-forward). This discipline addresses *upstream estimation provenance*.
- Does NOT require literature for every strategy — Fermi estimates from first principles are acceptable; theoretical derivation from mechanism characteristics is acceptable.
- Does NOT require multi-source triangulation — single independent source is sufficient (though triangulation strengthens confidence).

### Operationalization status

| Element | Wiki encoded | Pipeline implemented |
|---------|:---:|:---:|
| Predictive vs descriptive distinction (this section) | ✅ | n/a (concept) |
| Source citation requirement in hypothesis schema (per Part I §4) | ✅ | ⚠️ partial — pre-registration discipline; not enforced by tooling |
| Held-out OOS chunk discipline | ✅ | ❌ — pipeline uses single 70/30 split per [[foundation/methodology/strategy-validation-discipline]] §11; held-out integrity not gated |
| Same-data round-trip detection | ✅ flag | ❌ — no automated check |

**Net**: discipline is wiki-encoded; enforcement is pre-registration-protocol-dependent (Section 11 of this page). Tooling support for held-out integrity is engineering work owner-flagged.

## 5. Threshold Recommendations (BTC Perp Day/Swing on Binance/Bybit-Class Venues)

These are working numbers, not certified — out-of-sample slippage calibration via live data is owner-flagged engineering work:

| Concept | Working number | Notes |
|---------|---:|---|
| Roundtrip slippage estimate (additional to fees) | **0.5–2bp** | Varies with size, time of day, vol regime |
| 5× safety factor | **2.5–10bp** | Safety margin over base slippage |
| **Required gross edge per trade (in addition to fees)** | **≥10bp** | At typical position notional |
| Required gross ATP at $1K capital, $400–$5K position | **≥$5** | Working floor for "borderline-deployable" |
| Required gross ATP at $100K capital, $40K–$500K position | **≥$500** | Working floor scales linearly with capital |

**These thresholds are venue + asset + regime specific.** Binance/Bybit BTCUSDT day/swing is one regime; thinner alts have higher slippage and need higher gross edge floors; OTC blocks have lower slippage but different constraints. The discipline: **compute friction explicitly from current venue + asset + position-size assumptions; don't rely on cached numbers.**

## 5A. Opportunity-Cost Gate — the displacement bar (vs already-deployed strategies) {#opportunity-cost-gate}

§5's threshold is an **absolute** gate (net-of-friction edge above floor). When the deployment decision is whether to add a new strategy to a portfolio that already runs strategies, an **opportunity-cost gate** sits above the absolute floor: the new strategy must clear the risk-adjusted return of what would have to be reduced to fund it.

**A strategy that clears §5 still doesn't deploy if it underperforms an already-deployed strategy on the same capital.** §5 says "don't lose money to friction"; §5A says "don't reallocate to a worse risk-adjusted bet."

### Operational benchmark — an already-deployed strategy as the hurdle bar

When a research operation already runs a live strategy, that strategy's **risk-adjusted return is the hurdle bar** for any new candidate competing for the same capital. The benchmark is whatever the deployed alternative actually earns (its live Sharpe / Sortino / Calmar / max-drawdown) — a portfolio-specific number that lives in the operator's private substrate, not on the wiki.

The principle is general: a new strategy with a Sharpe well below the deployed alternative doesn't enter that capital bucket; a clearly higher-Sharpe strategy does (if the other gates also pass). The relevant comparison is **Sharpe-against-Sharpe** as a first cut, with explicit caveats about live-vs-backtest comparability (next sub-section).

### Worked example — `meta_post_cascade_btc_prod_v1` (2026-05-13)

A lead deployment candidate (per [[trading/mechanisms/liquidation-cascade-aftermath]]) sits at backtest Sharpe **2.54** — meaningful absolute edge, clears §5 friction floor, clears Gate 4 path-quality with mid-band caveat, but **sits BELOW the deployed funding-carry strategy's live hurdle bar**.

Deployment-decision implication: the candidate doesn't displace the deployed capital. Options:

1. Pre-deployment work to lift Sharpe above the hurdle (architecture changes, regime-conditioning, sub-strategy tuning per the per-sub PASS evidence)
2. Allocate to research-grade-pilot capital outside the deployed bucket (smaller stake; treat as live-validation, not deployment)
3. Hold pending further architecture results — if a later variant lifts the candidate above the hurdle bar AND clears regime-suite-preference, full deployment is on the table

The decision framework is **not** "Sharpe 2.54 is below an absolute deployment floor" — it's "Sharpe 2.54 is below the opportunity-cost of capital that's already earning a higher live Sharpe." Different gate; different implication; different pre-deployment work.

### Live-vs-backtest comparability caveats

Direct Sharpe-against-Sharpe comparison between a live deployed strategy and a backtest candidate has known asymmetries:

| Dimension | Live deployed strategy includes | Backtest candidate includes |
|---|---|---|
| Slippage | Yes — venue-specific microstructure | Engine assumption (typically 0.04% fee model) |
| Funding | Yes — paid/received on each settlement | Modeled at engine assumption |
| Venue/connectivity drag | Yes — API latency, fills, partial fills | No |
| Regime exposure | Multi-cycle live experience | Window-specific (2y multi-cycle for prod cand.) |
| Capital scale | At deployed-capital scale | At $1K reference; capacity unknown at scale |

**Sharpe-against-Sharpe is a fair first cut.** Live Sharpe is friction-true; backtest Sharpe is gross-of-real-friction. The honest move is to **discount the backtest Sharpe** by an implementation-shortfall factor before comparing, or to test the backtest strategy in live-pilot mode at small capital to surface implementation drag empirically. A backtest-Sharpe-2.54 strategy might be live-Sharpe-2.0–2.3 once deployment friction lands.

This caveat sharpens — not weakens — the opportunity-cost gate. If anything, the live-Sharpe-vs-live-Sharpe comparison is *stricter* than the headline backtest-vs-live shows.

### When the opportunity-cost gate is binding vs not

The §5A gate binds when:

- The portfolio has a **finite capital base** (any reallocation comes from somewhere)
- There's **already-deployed strategies** with measurable Sharpe / Calmar
- The new strategy candidate isn't strictly orthogonal (some of its capital would come from existing buckets)

The gate is **less binding** (effectively dormant) when:

- Net-new capital is being added (no reallocation; absolute floor §5 is the binding gate)
- The new strategy is in a structurally distinct mechanism family with capacity outside existing buckets (e.g., a multi-symbol portfolio when the existing bucket is BTC-only)
- The strategy is sized as research-grade-pilot (small enough that the opportunity-cost is rounding-error relative to the validation value)

### Connection to the 5-gate deployment-decision stack

§5A is **not** a sixth gate — it's a subordinate test under §5 (economic significance). The 5-gate stack from [[foundation/methodology/path-quality-diagnostics#connection-to-deployment-decision-gate-stack|path-quality-diagnostics §4]] stays:

| Gate | Page | Type |
|---|---|---|
| 0. Mechanism articulation | [[ARCHITECTURE|wiki schema]] §4 | Schema |
| 1. Statistical significance | [[foundation/methodology/strategy-validation-discipline]] | Absolute |
| 2. Translation-loss | [[foundation/methodology/forecast-vs-tradeable-gap]] | Absolute |
| 3. Economic significance | this page §5 (floor) + §5A (opportunity-cost) | **Absolute + relative** |
| 4. Path-quality | [[foundation/methodology/path-quality-diagnostics]] | Absolute |

Gate 3 is the only gate with both an absolute (§5) and relative (§5A) component, because economic significance is the only gate where comparison to alternatives is structurally meaningful (statistical significance, translation-loss, mechanism, and path-quality are all intrinsic-to-the-strategy properties).

### Honest caveats

- **N=1 worked example as of 2026-05-13** (a funding-carry strategy's live Sharpe vs meta_post_cascade backtest Sharpe 2.54). The opportunity-cost gate is wiki-encoded as discipline; the specific hurdle-bar number is portfolio-specific and changes as the deployed-strategy mix changes — it lives in the operator's private substrate, not here.
- **Sharpe-against-Sharpe isn't the only fair comparison.** Calmar (return / max_dd) may be more appropriate for capital with strict drawdown limits; Sortino for asymmetric-loss tolerance. The candidate's Calmar comparison would use the deployed strategy's live Calmar as benchmark — meta_post_cascade Calmar would compute to (engine_ann_return / 11.4% dd) per the [[foundation/methodology/forecast-vs-tradeable-gap#engine-output-gotcha-annualized_return_pct|engine-output-gotcha]] caveat (use simple/compound annualization manually, not engine field).
- **The gate is portfolio-specific, not universal.** A different operator with different deployed-strategy Sharpe levels has a different hurdle bar. The wiki encodes the *discipline*; the *number* lives in the private substrate.
- **Small-N interaction with the hurdle bar** (added 2026-05-15) — beating the live hurdle Sharpe at small N is harder than it looks. Per [[foundation/methodology/small-sample-validation-discipline#psr-operational-thresholds|small-sample-validation-discipline §PSR operational thresholds]]: at `N = 11`, an observed Sharpe of 1.65 has `PSR(SR > 0) ≈ 0.70`; to be confident the true Sharpe exceeds the hurdle requires both elevated point-Sharpe AND elevated PSR. A small-N candidate beating the hurdle at point estimate but failing the corresponding `PSR(SR > hurdle) > 0.7` is **research-grade, not deployment-grade displacement** — the opportunity-cost gate interacts multiplicatively with the small-sample discipline.

## 6. Worked Examples — The 2026-05-05 Deployment-Grade Stocktake (Canonical)

This is the **load-bearing recalibration** of the entire 2026-05-05 batch. Per the [[trading/rejected/ema-bias-breakout-4h-symmetric-2026-05|deployment-grade stocktake]], **0 of 13 strategies cleared the 12%/yr ($120/yr at $1K capital) net deployment-grade threshold.** Per-strategy table:

| Strategy | Gross ATP | Trades/yr | Gross/yr ($1K) | Friction/yr | **Net/yr** | Verdict |
|----------|---:|---:|---:|---:|---:|---|
| [[trading/rejected/wyckoff-spring-4h-2026-05\|wyckoff_spring_4h]] | strongly neg | ~30 | strongly neg | $30 | **−$153** | REJECT (worst result; capability-gap-blocked) |
| [[trading/rejected/asia-range-sweep-4h-2026-05\|asia_range_sweep_4h]] | weak/neg | ~15 | neg | $15 | **−$142** | REJECT (mechanism failure in current regime) |
| [[trading/rejected/funding-flip-recovery-4h-symmetric-2026-05\|funding_flip_recovery_4h]] (symmetric) | neg | ~50 | neg | $50 | **−$66** | REJECT (asymmetry confirmed; symmetric framing bug) |
| [[trading/rejected/bollinger-squeeze-4h-symmetric-2026-05\|bollinger_squeeze_4h]] (symmetric) | neg | ~30 | neg | $30 | **−$58** | REJECT (asymmetry confirmed; symmetric framing bug) |
| [[trading/rejected/ema-bias-breakout-4h-symmetric-2026-05\|ema_bias_breakout_4h]] (symmetric) | $0.30 | 71 | $21 | $71 | **−$50** | REJECT (mechanism real; deployment-grade fail) |
| [[trading/rejected/funding-flip-recovery-4h-short-only-2026-05\|funding_flip_recovery_4h_short_only]] | $0.20 | 50 | $11 | $50 | **−$39** | REJECT (frequent-but-tiny) |
| [[trading/rejected/htf-aligned-breakout-4h-2026-05\|htf_aligned_breakout_4h]] | neg | ~33 | neg | $33 | **−$38** | REJECT (HTF gate destroyed edge) |
| [[trading/rejected/ema-bias-breakout-4h-v2-2026-05\|ema_bias_breakout_4h_v2]] | $0.60 | 62 | $38 | $62 | **−$24** | REJECT (verdict reversal under deployment-grade) |
| [[trading/backtest-results/direction-restriction-short-only-2026-05\|bollinger_squeeze_4h_short_only]] | $1.50 | 16 | $24 | $16 | **+$8** | **research-grade-only** (closest-to-passing) |
| [[trading/rejected/bollinger-squeeze-4h-short-regime-2026-05\|bollinger_squeeze_4h_short_regime]] | apparent +$60 face value | 0.6% grid eligible | — | — | INCONCLUSIVE | (selection bias on 63 of 10,368 grid runs) |
| [[trading/rejected/funding-flip-recovery-regime-gate-2026-05\|funding_flip_recovery_4h_short_regime]] | — | — | — | — | apparent +$10 | REJECT (regime gate hurts vs control) |
| [[trading/rejected/turtle-soup-4h-2026-05\|turtle_soup_4h]] | neg | ~25 | neg | $25 | **negative** | REJECT (3/3 falsifier) |
| `ema_bias_breakout_4h_counter` | — | — | — | — | — | ARCHIVE (control mirror; never a strategy) |

Friction calc: 1bp roundtrip × $5K position × N trades. (1bp on $5K = $0.50 per leg, $1 roundtrip.)

**Reading the table:**
- **0 of 13 cleared deployment-grade threshold** ($120/yr at $1K = 12% return)
- **1 borderline-positive** (bollinger short_only +$8/yr) — research-grade-only; closest-to-passing; flagged for stacking-mutation Task 5 follow-up
- **8 outright rejected** — gross-negative or net-negative even before strict friction
- **1 inconclusive** (bollinger short_regime — selection bias on degenerate parameter subset)
- **1 archive** (counter-bias mirror — control mirror, not a strategy)
- **2 already rejected in earlier 2026-05-05 batch** (turtle_soup, funding_flip_short_regime)

**Sensitivity**: at higher capital with proportionally larger position (the realistic deployment scale), gross ATP scales linearly with position size and so does friction — the *ratio* doesn't change, so verdicts stay the same. At larger capital, *additional* impact costs may worsen the verdict (impact is non-linear in size).

**Sensitivity to slippage assumption**: at 0.5bp roundtrip (best-case major-perp daytime BTCUSDT), bollinger_squeeze's net improves to +$16 (still small); ema_bias variants improve marginally; **none cross threshold**. Even best-case assumptions don't rescue these strategies into clearly deployment-grade territory.

**Honest finding**: this is not a failure. The 2026-05-05 batch was the *first* batch tested against the deployment-grade gate (added later 2026-05-05 PM-2). The batch's gross-edge magnitudes were optimized for statistical-significance only — the deployment-grade question wasn't asked upstream. Per Part I §4 sharpening (also 2026-05-05 PM-2), future strategies must include `expected_net_edge` upstream of backtest. The PM-3 stocktake formalizes the methodology gap as wiki content.

## 7. What Strategies Pass the Threshold (Honest Finding)

**At the current backtesting-lab inventory: mostly none.** This is not a failure of the wiki — it is a *useful research finding* that the wiki should not hide.

The 2026-05-05 batch tested ≥13 strategies; the research process initially marked 3 as productionize candidates. Recalibrating against this methodology:

- **0 of 3 are clearly deployment-grade**
- **1 of 3 (bollinger short_only) is borderline** at best-case slippage assumptions
- **2 of 3 (ema_bias v2, funding-flip short_only) are research-grade only** under any reasonable slippage assumption

Reading the finding charitably: the strategies have *real research value* (mechanism mapping, partial-truth extraction, methodology validation). They are not deployment-grade *as currently parameterized*.

**The discipline is to be honest about this upstream**, not to discover it after live capital is allocated.

## 8. Mechanism Implications — Which Families Are MORE Likely to Clear the Threshold

Not all mechanism families are equally friction-sensitive. The deployment-edge threshold favors:

### Lower turnover (less friction × trades)

Strategies holding multi-day to multi-week have lower friction-per-year-per-strategy because friction scales with trade count. A 10-trade/yr strategy with $5 ATP has $50 gross/yr at $1K and $10 friction/yr → **+$40 net** (much cleaner than 50-trade/yr with same ATP).

Mechanism families more likely:
- [[trading/mechanisms/calendar-effects|Calendar effects]] — token unlocks, halving (rare events)
- [[trading/mechanisms/accumulation-distribution|Wyckoff accumulation/distribution]] — multi-day cycles
- Long-horizon investment thesis (different branch — see [[foundation/methodology/investment-thesis-protocol]])

Mechanism families less likely:
- High-frequency mean reversion (friction dominates)
- Tight-stop scalp setups (high turnover with small ATP)

### Larger absolute edge per trade

Event-driven mechanisms produce concentrated edge — a halving event or ETF approval moves price 5–20%, with corresponding $-edge per trade much larger than incremental technical-pattern setups.

Mechanism families more likely:
- Halving-cycle plays
- ETF flow-driven moves
- Options expiry pinning / unpinning
- Macro events (FOMC, CPI surprise)

Mechanism families less likely:
- Generic technical breakouts (small edge per trade, frequent)
- RSI mean-reversion setups (small edge per trade, frequent)

### Less retail-accessible mechanisms

Mechanisms requiring specialized data, infrastructure, or capital are less arbitraged → larger remaining edge.

Mechanism families more likely:
- Derivatives-structure (perp-spot basis, cross-exchange funding) — see a funding-carry strategy in [[trading/mechanisms/funding-cycle-dynamics]]
- On-chain flow (whale wallet tracking, exchange flows)
- Dealer economics (market-making, AMM LP) — see [[foundation/market-structure/dealer-economics]]
- Cross-exchange microstructure (basis trades, latency arbitrage)

Mechanism families less likely:
- Public technical patterns visible on TradingView (everyone sees them; arbitraged faster)

### Asset selection matters

Thinner alts have higher edge per move but worse slippage and depth; majors (BTC/ETH) have tight slippage but compressed edge from heavy competition. Optimal asset depends on:

- Strategy's edge magnitude vs friction profile
- Capital deployable without market impact
- Available data infrastructure (alt-specific depth, liquidations, on-chain)

A strategy with $5 gross ATP on BTC perps may have $50 gross ATP on a thinner alt — but if alt slippage is 5× higher, net is similar. **Asset selection is part of the deployment-edge calculation, not separate from it.**

## 9. Operationalization Status

| Element | Wiki encoded | Pipeline implemented |
|---------|:---:|:---:|
| Net-of-friction summary statistic | ✅ this page | ❌ not in summary outputs |
| Slippage modeling in engine | ⚠️ flagged here | ❌ engine includes fees only |
| Position-sizing-aware backtest | ⚠️ partial | ⚠️ risk_pct only; no impact model |
| Spread half-cross modeling | ✅ formula | ❌ not in engine |
| Market impact modeling | ✅ concept | ❌ not in engine |
| Funding-rate slippage | ✅ formula | ⚠️ partial (only if strategies time around settlement) |
| Live-vs-backtest decay calibration | ❌ no data harness | ❌ — would benefit from a funding-carry strategy / P2P / Grid Bot live data cross-reference per Performance section |
| Net-edge threshold gate | ✅ this page | ❌ no automatic gate |
| Per-asset slippage estimates | ⚠️ working numbers | ❌ no calibrated per-asset table |

**Net**: every concept on this page is **wiki-encoded but mostly not pipeline-implemented**. This page is therefore **aspirational discipline** — the gap between what we should compute and what the engine currently computes. Closing the gap is engineering work, owner candidates per Apr 28 division of labor.

## 10. Companion Disciplines (Cross-Links)

This page is one of four validation gates that must compose:

| Gate | Discipline | Page |
|------|-----------|------|
| **Mechanism articulation** | WHO / WHY / WHEN | Part I §4 + [[foundation/idea-sourcing-methodology]] |
| **Statistical significance** | PSR / DSR / PBO; non-IID label correction; multiple testing | [[foundation/methodology/strategy-validation-discipline]] |
| **Translation-loss (forecast → tradeable)** | L1 effect magnitude / stop / horizon compatibility | [[foundation/methodology/forecast-vs-tradeable-gap]] |
| **Economic significance (deployment-grade)** | Net-of-friction edge floor | **This page** |

A strategy must pass all four. Mechanism without the others is *storytelling*. Statistical significance without the others is a *lucky window*. **A clean L1 forecasting effect can fail the L2 translation gate even before friction is evaluated** — see [[foundation/methodology/forecast-vs-tradeable-gap]] for the diagnostic checklist (effect-magnitude / stop ratio ≥ 3, horizon match ≤ 1.5×, mechanics-compatibility). Once L1 → L2 succeeds, deployment edge applies the friction floor. The discipline is to identify upstream which gate is most binding for a given strategy class.

### Cross-references

- [[foundation/methodology/strategy-validation-discipline]] — sister page; statistical significance side
- [[foundation/methodology/forecast-vs-tradeable-gap]] — sister page; translation-loss side (L1 effect → L2 strategy)
- [[foundation/sources/lopez-de-prado-advances-fin-ml]] §7.1 — multi-instrument testing reduces overfit AND provides slippage diversification
- [[foundation/market-structure/liquidity]] — Harris's four dimensions; slippage is sub-component of width + depth
- [[foundation/market-structure/bid-ask-spread-decomposition]] — Harris's most important single concept; explains adverse-selection component of friction
- [[foundation/market-structure/dealer-economics]] — dealer-side economics; a funding-carry strategy is a dealer-side strategy that extracts edge from this side of the spread

## 11. Pre-Deployment Protocol (Operational Discipline)

Adopt **before** any live capital allocation. Symmetric to [[foundation/methodology/strategy-validation-discipline|validation discipline §10 pre-registration]] but on the deployment-grade side:

For any strategy moving from `trading/backtest-results/` to `trading/active/`:

| Step | Output |
|------|--------|
| **(a) State assumed slippage** | Working number per leg; bp; reason for choice (venue / asset / size / time) |
| **(b) Compute net edge per trade** | `gross_ATP − friction_per_trade` at proposed live position size |
| **(c) Compute net edge per year** | `(net_edge_per_trade × trades_per_year)` at proposed capital scale |
| **(d) State the safety factor** | Net-edge / friction ratio; 5× over base slippage is working floor |
| **(e) Identify the most binding constraint** | Slippage? Impact? Funding? Latency? Adverse selection? Forces honest scaling assumption |
| **(f) State live-deployment hypothesis** | What live ATP would refute the backtest's gross edge? Pre-register this before running live |
| **(g) Initial capital cap** | Small allocation justified by the expected drift between backtest and live ATP |

If any step is skipped, the strategy is not deployment-ready. Discipline is the load-bearing thing; tooling is optional.

## 12. What This Methodology Does NOT Replace

- **Statistical-significance discipline** — see [[foundation/methodology/strategy-validation-discipline|sister page]]. Both gates must pass.
- **Mechanism articulation** — Part I §4. A strategy with good net edge but no articulated mechanism is fragile (regime change kills it; you have no thesis for *why* it works).
- **Position sizing methodology** — [[investment/allocation/equity-management-rules]]. Net-edge math depends on position notional, which depends on sizing rules; the two compose.
- **Out-of-window validation** — net edge in 2025-26 may not generalize to 2027 regime. This page does not certify regime-stationarity.
- **Live execution detail** — venue selection, order routing, smart order placement, latency mitigation are engineering work downstream of net-edge clearing.

## 13. Crypto-Specific Concerns

Crypto-perp friction has features TradFi backtest discipline doesn't always cover:

1. **Funding rate as a hidden cost**. Holding a long perp through a positive-funding settlement transfers value to shorts. Strategies must either time entries to avoid settlements or include funding cost in friction calc. This is *not optional* for any strategy with hold periods >8h.
2. **Cross-venue basis** — same instrument trades different prices on different exchanges; live execution may need to consolidate; spread *across venues* is sometimes wider than spread *within venue*.
3. **Liquidation cascade adverse selection** — during cascades, even tight stops slip materially because the order book is thin and one-sided. Backtests with constant slippage assumptions underestimate friction during these events. This is a regime-conditional friction component.
4. **Maker-vs-taker fee asymmetry** — Binance: maker -0.005% (rebate), taker 0.04%. Strategies that *cross* the book (taker) pay much more than strategies that *post* limits (maker rebate). DSL strategies that send "market" orders are effectively pay-taker; backtest at 0.04% all-in.
5. **Stablecoin venue risk** — USDT depeg events (FTX, Tether scares) introduce friction beyond standard slippage. Multi-stablecoin position management is real engineering work.
6. **Regulatory friction** — withdrawal limits, KYC delays, geographic restrictions — affect *deployable* capital, separate from market-side friction.

These are wiki-side homework on top of the universal slippage math.

## 14. Honest Caveats

- **Slippage estimates are working numbers, not measured.** 0.5–2bp roundtrip on BTCUSDT majors is industry-typical for retail-size positions on top venues. Calibration via live execution data is owner-flagged engineering work; until then, treat thresholds as conservative-aspirational.
- **The 5× safety factor is heuristic.** Could be 3×, could be 10× — depends on slippage variability per asset/regime. The discipline is to *use a margin*, not the specific number.
- **The methodology does not capture all friction sources.** Funding spikes during high-vol events, weekend gaps on TradFi-correlated instruments, exchange downtime during cascades all add friction not modeled here.
- **Net-edge math is per-trade × trades-per-year.** It does not capture path-dependence: a strategy that loses $50/trade × 50 trades is worse than one that loses $2500 once if drawdown timing matters for capital management. Sizing rules and drawdown limits are companion disciplines.
- **Crypto regime non-stationarity.** A strategy with adequate net edge in 2025-26 may have insufficient edge in 2027 if friction regime changes (more competition, deeper books, narrower spreads) or edge regime changes (mechanism crowding). The methodology is a *current-snapshot* discipline; revalidation is required across regimes.

## Key Takeaways

- **Statistical significance ≠ deployment-grade.** The 2026-05-05 batch demonstrated this concretely — 3 productionize candidates, 0 clearly deployable, 1 borderline.
- **Friction is always present** — fees + spread + impact + latency + funding. Backtest engines model fees only; the rest must be added.
- **Position notional, not capital, drives friction.** Friction scales with notional × number-of-trades. Edge must scale faster.
- **Working floor for BTC perp day/swing**: gross edge ≥10bp per trade at typical position notional. Equivalent to gross ATP ≥$5 at $1K capital with $5K position.
- **Three mechanism characteristics increase deployment-grade likelihood**: lower turnover, larger absolute edge per trade, less retail-accessible mechanism.
- **Most strategies in current inventory are research-grade only.** This is honest finding, not failure — research-grade work has value (mechanism mapping, partial-truth extraction). Don't promote to deployment without clearing this gate.
- **The discipline is to compute net edge upstream, not discover it after live capital is at risk.** Symmetric to falsifier discipline on the validation side.
- **Operationalization gap**: every concept here is wiki-encoded; few are pipeline-implemented. Engineering work owner-flagged.

## Related

- [[foundation/methodology/strategy-validation-discipline]] — sister page; statistical-significance side of validation
- [[foundation/sources/lopez-de-prado-advances-fin-ml]] — multiple-testing correction, walk-forward critique; complementary statistical discipline
- [[foundation/market-structure/liquidity]] — Harris's four dimensions; slippage is width + depth dependent
- [[foundation/market-structure/bid-ask-spread-decomposition]] — adverse-selection component of friction
- [[foundation/market-structure/dealer-economics]] — dealer side of the friction the strategy pays
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — regime-conditional friction (cascades, vol regime)
- [[foundation/macro/regime-conditional-edge-2025-26]] — empirical W2 finding: concentrated 5-6 month edge × full-year friction is the deployment-grade penalty this finding makes concrete
- [[trading/mechanisms/funding-cycle-dynamics]] — funding-rate slippage (perp-specific component)
- [[trading/rejected/ema-bias-breakout-4h-v2-2026-05]] — worked example: apparent pass that fails deployment-grade gate (Net −$25/yr)
- [[trading/backtest-results/direction-restriction-short-only-2026-05]] — worked example: 1 borderline (bollinger Net +$8/yr) + 1 fail (funding-flip Net −$40/yr)
- Part I §4 — Structured Hypothesis Format (sharpened 2026-05-05 to require expected_net_edge sub-component of Expected field)
- [[investment/allocation/equity-management-rules]] — position sizing rules; the position notional input to friction calc

## Sources

- 2026-05-05 backtest batch: the backtesting lab{ema_bias_breakout_4h_v2, bollinger_squeeze_4h_short_only, funding_flip_recovery_4h_short_only}/`
- the backtesting lab — heat-map analysis surfacing the gross-edge magnitudes
- Harris, *Trading and Exchanges* (2003) — microstructure foundation for friction decomposition (see source page [[foundation/sources/harris-trading-exchanges]])
- Working slippage numbers from major-venue order-book observations (Binance/Bybit BTCUSDT) — calibrated via working knowledge, not formally measured. Live data calibration is owner-flagged engineering work.
