---
title: "Regime Gate vs Bias Filter — design pattern"
status: growing
created: 2026-05-05
updated: 2026-05-21
tags: [design-pattern, mechanism-meta, bias-filter, regime-gate, signal-design, day-swing, empirical-evidence, scaffold-as-mechanism]
---

# Regime Gate vs Bias Filter — design pattern

> **TL;DR**: For crypto-perp day/swing strategies, an **entry-embedded directional bias filter** outperforms a **separate post-hoc regime gate** on the same mechanism class. Empirical evidence: N=4 supporting cases from the 2026-05-05 batch (`ema_bias_breakout_4h_v2` wins vs `htf_aligned_breakout_4h`; `bollinger_squeeze_4h_short_regime` operates only on degenerate parameter subset; `funding_flip_recovery_4h_short_regime` hurts vs short-only control; direction restriction wins on both `bollinger_squeeze_4h_short_only` and `funding_flip_recovery_4h_short_only`). The pattern: bias filter is *intrinsically aligned* with entry signal; regime gate is *additive and decoupled* — that decoupling introduces noise, and at narrow gate settings the gate eats most signal volume creating selection bias. **Generalization (2026-05-06)**: a fifth-and-load-bearing finding from the 2026-05-05 5-strategies batch — four strategies sharing the HTF-EMA-bias scaffold cluster at +0.59 to +0.71 trade-timing correlation regardless of secondary axis. Generalizes to **scaffold-determines-timing**: secondary axes are signal expressions of one mechanism, not distinct mechanisms. **Companion to** [[foundation/market-wisdom/bias-filter-vs-entry-signal|bias-filter-vs-entry-signal taxonomy]] (signal *types*) and [[trading/mechanisms/scaffold-as-mechanism|scaffold-as-mechanism]] (mechanism-class-level generalization).

## Hypothesis (Structured Format)

```
Hypothesis:    For crypto-perp day/swing strategies, an entry-embedded
               directional bias filter outperforms a separate post-hoc
               regime gate on the same mechanism class.

Mechanism:     The bias filter is *intrinsically aligned* with the entry
               signal — every entry IS bias-aligned by construction.
               The regime gate is *additive and decoupled* — it can fire
               or not-fire independently of the entry signal. That
               decoupling introduces noise. The bias filter also doubles
               as direction picker: long-bias = entry_long only;
               bearish = entry_short only — which is the
               direction-asymmetric edge captured in different language.

               The regime gate's typical implementation (slow EMAs at
               longer periods than entry signal) introduces *timing
               decoupling*: the gate's regime classification lags the
               entry signal's faster-moving features. When regime
               transitions, gate misclassifies — the strategy enters
               counter-regime trades or skips with-regime trades.

               Additionally, narrow gate settings (slow EMA crossover
               with strict criteria) eliminate trade volume across most
               of the parameter grid, leaving only a degenerate subset
               where the gate happens to leave enough trades. The
               apparent edge on this subset partly reflects selection
               bias rather than genuine mechanism.

Observable:    In paired-grid backtests on the same mechanism class +
               same window, strategy with entry-embedded bias filter
               > strategy with separate regime gate AND > strategy
               without either, on the same parameter space breadth.

Expected:      ≥0.20 Sharpe gap in favor of bias-filter version vs
               regime-gated version on matched parameter ranges. Larger
               gap (>0.50) when regime gate's slow EMAs lag the entry
               signal's fast EMAs (timing-decoupling penalty).

Falsifier:     If a regime-gated mutation outperforms its bias-filter
               sibling on the same strategy class by ≥0.10 Sharpe across
               parameter-space-comparable runs (eligibility ≥10% of
               grid), this hypothesis weakens. Counter-evidence requires
               N≥3 across mechanism classes to overturn. Single
               degenerate-parameter-subset wins do NOT count as
               counter-evidence (they are themselves the failure mode).
```

## Two architectures defined

### Architecture A: Entry-embedded bias filter

The bias filter is part of the entry signal logic. Every signal evaluation checks the bias condition; entries fire only when bias is aligned. Direction is implicit: bullish bias → entry_long; bearish bias → entry_short.

**Example** (`ema_bias_breakout_4h_v2`):
```
if EMA(20) > EMA(50):    # bullish bias
    if price breaks above N-bar high:   entry_long
elif EMA(20) < EMA(50):  # bearish bias
    if price breaks below N-bar low:    entry_short
# else: no entry
```

The bias filter and entry signal are **co-evaluated**. Filter periods (EMA 20, 50) are typically *similar timescale* to entry signal (breakout lookback 10).

### Architecture B: Separate post-hoc regime gate

The regime gate is a separate condition layered on top of an existing strategy. The base strategy's entry logic is unchanged; the gate filters which entries are allowed.

**Example** (`funding_flip_recovery_4h_short_regime`):
```
# Base strategy: funding-flip-recovery
funding_flip_event = (funding_t-1 > 0) and (funding_t < 0)
if funding_flip_event:
    if base_short_signal:   candidate_entry_short
    if base_long_signal:    candidate_entry_long

# Gate (separate, longer-timescale):
if EMA(50) < EMA(200):  # bearish regime gate
    allow candidate_entry_short
    block candidate_entry_long
```

The gate operates at **longer timescale** than the entry signal (EMA 50/200 gate vs funding-flip event). The gate's classification lags entry signal moves.

## Supporting evidence (2026-05-05 batch)

| Strategy class | Bias-filter version | Regime-gate version | Outcome |
|----------------|---------------------|---------------------|---------|
| EMA-aligned breakout | [[trading/rejected/ema-bias-breakout-4h-v2-2026-05\|ema_bias_breakout_4h_v2]]: medSh **+0.89**, medL$ +$35.20 | `htf_aligned_breakout_4h` (Apr): medSh −0.04 | **Bias filter wins** by 0.93 Sharpe |
| Bollinger squeeze | (no bias-filter version tested) | `bollinger_squeeze_4h_short_regime`: medSh +2.24 BUT n=63/10368 (selection bias; squeeze params collapsed to single combination — `squeeze_window=3` AND `squeeze_pct=0.25`) | **Inconclusive** — regime gate operates only at degenerate parameter subset |
| Funding-flip recovery | (no bias-filter version tested) | [[trading/rejected/funding-flip-recovery-regime-gate-2026-05\|funding_flip_recovery_4h_short_regime]]: medSh +0.34 vs short-only control's +0.50 | **Regime gate hurts** by 0.16 Sharpe |
| Direction restriction (degenerate bias filter) | [[trading/backtest-results/direction-restriction-short-only-2026-05\|short_only controls]]: bollinger +1.02; funding-flip +0.50 | Symmetric parents: −1.01, −0.11 | **Direction restriction wins** by +2.03, +0.61 |

**N=4 cases. Direction in favor of bias-filter / direction-restriction architecture: 4/4** (including the inconclusive bollinger squeeze case where regime gate's apparent win dissolves under selection-bias scrutiny).

## Generalization (2026-05-06): scaffold determines timing

The 2026-05-05 5-strategies batch surfaced a structural finding that **generalizes the bias-filter > regime-gate result**: across four strategies sharing the HTF-EMA-bias scaffold but differing in their secondary axes, **trade-timing correlation clusters at +0.59 to +0.71** — independently of which secondary axis is used.

| Strategy pair (all share HTF-EMA-bias scaffold) | Per-bar Q5 P&L correlation |
|---|---:|
| `confluence_trend_4h` × `ema_bias_breakout_4h_v2` | **+0.711** |
| `taker_buy_asymmetric_4h` × `ema_bias_breakout_4h_v2` | **+0.704** |
| `volume_z_asymmetric_4h` × `ema_bias_breakout_4h_v2` | **+0.586** |
| `confluence_trend_4h` × `taker_buy_asymmetric_4h` | **+0.677** |
| `confluence_trend_4h` × `volume_z_asymmetric_4h` | **+0.650** |
| `taker_buy_asymmetric_4h` × `volume_z_asymmetric_4h` | **+0.686** |

Source: 2026-05-05 batch stocktake (the backtesting lab §"Cross-strategy correlation matrix"). All four strategies share the bias gate (`EMA(20) vs EMA(50)`) and the bar-direction-aligned trigger; they differ in secondary axes (vol-expansion confluence, taker-imbalance, volume-Z, breakout-direction).

For comparison, **different-scaffold strategies are far less correlated** with this cluster:

| Strategy (different scaffold) | Max correlation with HTF-EMA-bias cluster |
|---|---:|
| `range_fade_4h` (vol-state, compressed) | −0.09 |
| `bollinger_squeeze_4h_short_only` (vol-state, expansion) | +0.26 |
| `funding_flip_recovery_4h_short_only` (funding-regime) | +0.03 |

All <+0.30 with the trend cluster. **The scaffold determines timing; secondary axes are expressions of one mechanism, not distinct mechanisms.**

### What this generalizes

The original finding (N=4 bias-filter vs regime-gate cases above) was **architecture-level**: how to combine a directional axis with an entry signal in *one* strategy. The scaffold-cluster finding is **mechanism-class-level**: how multiple strategies relate when they share that architecture.

Combined statement:
- **Pick the scaffold deliberately** — it determines timing of the family of strategies built on it.
- **Embed the scaffold's directional axis in the entry signal** (this page's original finding) — once chosen, the architecture should be embedded, not layered.
- **Don't treat same-scaffold variations as portfolio diversification** — secondary-axis variation is signal-expression variation, not mechanism diversification.

### Implication: "novel signal axis" claims need pre-registered timing-correlation tests

Both `taker_buy_asymmetric_4h` and `volume_z_asymmetric_4h` were spec'd with claims about distinct signal axes (informed-flow microstructure, volume-clustering). Both novelty claims **collapsed at the timing level**: the secondary axes selected the same bars the HTF-bias trend scaffold did.

Going forward, strategy specs that claim a novel signal axis should pre-register a falsifiable timing-correlation prediction with named comparators on the existing scaffold. Falsifier: if observed correlation is ≥+0.6 with any same-scaffold comparator, the novelty claim is falsified — the strategy is a same-mechanism mutation, not a new mechanism class.

The full framework lives at companion page [[trading/mechanisms/scaffold-as-mechanism]] (definition + threshold + scaffold catalog + caveats).

### Scaffold catalog (current lab inventory)

| Scaffold | Strategies on it |
|---|---|
| **HTF-EMA-bias** | `ema_bias_breakout_4h_v2`, `confluence_trend_4h`, `taker_buy_asymmetric_4h`, `volume_z_asymmetric_4h` (4) |
| **Vol-state (expansion)** | `bollinger_squeeze_4h` family (incl. `_short_only`, `_short_regime`) (3) |
| **Vol-state (compression)** | `range_fade_4h` (1) |
| **Funding-regime (flip)** | `funding_flip_recovery_4h` family (3) |
| **Funding-regime (extreme persistent)** | `funding_extreme_exhaustion_4h` (gate compound never fired) |
| **Session-boundary** | `asia_range_sweep_4h` (1; mechanism failure flagged) |
| **Liquidation-cascade** | `wyckoff_spring_4h` (capability-gap-blocked) |

The HTF-EMA-bias scaffold dominates the active inventory (4 of 13 testable strategies; 4 of 5 in 2026-05-05 batch). **This is the lab's biggest diversification gap.**

## Mechanism-specific corollaries

- **Bias filter works when entry signal IS directional** — breakout, momentum, trend-pullback. The filter's directional output is consumable by the entry signal directly.
- **Direction restriction (a degenerate bias filter — "always long" or "always short") works when grid shows asymmetric long/short P&L distribution.** This is the Part I §9 direction-restriction operator's positive case.
- **Separate regime gate on top of mean-reversion mechanism (funding-flip) is decoration or worse.** The mechanism's own signal already conditions on regime indirectly via the funding-rate carrier feature. The slow-EMA gate adds noise, not information.
- **Separate regime gate on top of vol-cycle mechanism (bollinger squeeze) creates degenerate operating range.** Gate eats most of the signal volume, leaving only a narrow parameter subset where the strategy can run. The apparent edge on that subset is contaminated by selection bias.
- **SOL is the canonical asset-class-level worked example of regime-conditional bias** (2026-05-15). SOL's bull-regime long-bias on BOTH breakout directions (up-breaks continue 87.5% / down-breaks reverse 71%) is bull-regime-amplified by structural asymmetric institutional flows (ETF absorption + DAT buying + no basis-trade shorts on nascent CME SOL futures). Any SOL strategy that doesn't condition on BTC regime is implicitly betting the backtest window's regime persists — regime gate is **mandatory not optional** on SOL. See [[../../foundation/asset-classes/sol-market-structure#regime-amplifier|sol-market-structure §regime-amplifier]] for full structural explanation.

## Selection-bias diagnostic — bollinger_squeeze_4h_short_regime worked example

The bollinger_squeeze_4h_short_regime case is a **worked example of selection-bias-via-min-trade-filter** that this page documents because it is structurally important:

| Diagnostic | Value |
|------------|---:|
| Total parameter combinations | 10,368 |
| Combinations producing ≥10 train trades | 63 (0.6%) |
| Shared parameter values across all 63 | `squeeze_window=3` AND `squeeze_pct=0.25` (most permissive squeeze settings) |
| Median Sharpe on the 63 | +2.24 |

**Reading the diagnostic:** the +2.24 Sharpe is computed on a degenerate subset. Apparent regime-gate edge is **partly real** (those 63 runs do well — there's something real about narrow-squeeze + bearish-regime confluence) but **partly selection** (broader parameter space wasn't testable because gate eliminated trade volume).

This is the failure mode [[foundation/methodology/strategy-validation-discipline|strategy validation discipline]] §7.1 (asset-class universes) warns about, applied here at the **parameter-class scale**: a strategy that works only on a narrow subset of parameters/instruments should be suspected of overfitting unless the narrowness is *theoretically* required (e.g., the mechanism logically only fires in that subset). The bollinger result is *not* theoretically constrained to those 63 runs — broader squeeze settings should mechanically also fire entries; they don't because gate volume requirement clipped them.

**Operational implication**: when a regime gate produces a high-Sharpe result on <5% of parameter grid, treat the result as **mostly selection bias** by default. The cleaner mutation is direction restriction (works across full parameter breadth) or embedded bias filter (works across full parameter breadth).

## Why bias filter beats separate regime gate (mechanism-level)

Three reasons converge:

1. **Timing alignment.** The bias filter's timescale is matched to entry signal. Direction reversals are caught at the same speed as entries fire. Separate regime gate at longer timescale lags — when regime flips, the strategy enters wrong-side trades for a window before the gate updates.

2. **Trade volume preservation.** Embedded bias filter restricts *direction* per bar; entry signal still fires on every eligible bar. Separate regime gate restricts *eligibility* per bar; entry signal fires only when gate AND signal align. The conjunction narrows trade count, often driving parameter-grid coverage to degenerate subsets.

3. **Decoupling penalty.** Bias filter and entry signal are co-evaluated against the same data window. Regime gate evaluates a separate (longer) window. The two windows can disagree — gate says "bearish" while entry signal sees "current move bullish reversal" — and the gate's vote dominates by veto, not by signal-strength.

## Counter-evidence sought (open questions)

- **Are there mechanism classes where separate regime gate genuinely outperforms?** Worth scanning the wiki + literature for known cases. Candidates:
  - HTF context filters in classical TA (Murphy intermarket analysis): different timescale by *design* — the HTF context is the mechanism, not a gate. May not be analogous.
  - Macro regime gates on equity strategies (recession/expansion gates): institutional research suggests these add value on long-horizon strategies. Day/swing crypto may differ.
  - Volatility-regime gates (only trade in vol regime X): conceptually different from directional regime gate; unrelated mechanism. May or may not generalize.

- **N=4 is small.** Pattern needs cross-window validation before being treated as universal. The 2025-26 BTC bearish-dominance is the regime context for this batch; bullish or chop regimes may produce different patterns.

- **Multi-instrument validation absent.** All 4 cases are BTCUSDT perp.

- **Bias-filter siblings for funding-flip and bollinger NOT TESTED.** The current evidence has bias-filter and regime-gate variants on different strategies. Cleaner head-to-head comparison would require building bias-filter versions of funding-flip and bollinger and testing same window.

## What this page does NOT claim

- Does not claim regime gates are *universally* worse than bias filters across all asset classes / horizons / mechanisms. The claim is empirical, scoped to crypto-perp day/swing strategies, with N=4 supporting cases from one window.
- Does not claim direction restriction is always better than retaining symmetric framing. Direction restriction is regime-conditional — same logic in different regime would favor opposite restriction or symmetric.
- Does not invalidate the [[foundation/macro/btc-regime-taxonomy-2020-2025|regime taxonomy]] — regime *classification* is valuable for context and for strategy selection. The claim is about *implementing* regime conditioning at the strategy-instance level: prefer embedded bias filter, deprioritize separate post-hoc gate.

## Operationalization

**For new strategies**: prefer embedding bias filter at design time. The classic structure:
```
if bias_long_condition:
    fire long entries only
elif bias_short_condition:
    fire short entries only
else:
    no entries
```

**For existing strategies showing asymmetric long/short P&L**: apply direction restriction first (Part I §9 mutation operator), test single-direction sleeve. Do not stack regime gate on top by default.

**For existing strategies needing regime conditioning**: rebuild the entry signal with embedded bias filter rather than adding separate regime gate. Cost is one strategy file rewrite; gain is full-parameter-space coverage.

**For cases where regime gate seems to win**: check parameter-space coverage (eligibility ≥10% of grid). If regime-gate variant operates on <5% of grid, treat as selection-bias suspect.

## Operationalization status

This is wiki-encoded design guidance. **Not pipeline-implemented as automated check.** Strategy authors apply the discipline manually at design time. Future engineering work could include a parameter-coverage diagnostic that flags gated strategies operating on <5% of grid as selection-bias suspects.

## Regime Applicability

This page is itself ABOUT regime conditioning, so the regime-applicability framing is reflexive — but worth making explicit:

| Regime (per [[foundation/macro/btc-regime-taxonomy-2020-2025\|BTC taxonomy]]) | Pattern stability | Evidence |
|---|---|---|
| Cycle peak rejection / bearish (e.g. 2025-Q4) | **Pattern derived from this regime** — N=4 supporting cases all from 2026-05-05 batch in this window | Source data |
| Bearish continuation (e.g. 2026-Q1) | Pattern likely-stable — same regime dynamics | Per [[foundation/macro/regime-conditional-edge-2025-26]] |
| Bullish trending | **Pattern likely INVERTS** — bias filter would favor long; same architecture but direction-flipped | Theoretical; would need bullish-regime backtest |
| Chop / transitional | **Pattern uncertain** — neither bias filter nor regime gate may be reliable; entry signal mechanisms may work better | Theoretical |
| Cascade aftermath | Pattern likely-stable but bias direction flips rapidly during cascades | Theoretical |

**Notes:**
- The "embedded bias filter > separate regime gate" finding is **regime-derived empirical evidence**, not regime-universal law
- The architectural pattern (embedded > separate) likely transfers across regimes; what changes is the *direction* of the bias filter, not whether to use one
- N=4 is small; cross-window validation across regimes is the load-bearing missing test
- The **selection-bias diagnostic** (regime gate operating on <5% of parameter grid → suspect) is regime-agnostic; applies regardless of which regime is active

Cross-strategy regime context: per [[foundation/macro/regime-conditional-edge-2025-26|W2 finding]], the cohort generating the N=4 evidence had concentrated edge in Q4 2025 + Q1 2026; pattern derived from this regime concentration.

## Key Takeaways

- **Empirical pattern (N=4): bias filter > separate regime gate** in 2026-05-05 batch.
- **Three reasons**: timing alignment, trade volume preservation, decoupling penalty.
- **Selection-bias risk in regime gates**: when gate eliminates trade volume across most of parameter grid, apparent edge on remaining subset is contaminated.
- **Direction restriction is the simplest bias-filter case** ("always long" or "always short" filter when grid shows asymmetric long/short P&L).
- **Caveats**: small N; 2025-26 bearish-dominant regime; BTCUSDT-only; bias-filter siblings for funding-flip and bollinger not tested.
- **Operationalize**: prefer embedded bias filter at design; apply direction restriction when asymmetry surfaces; do not stack regime gate on top of either.
- **Companion taxonomy**: [[foundation/market-wisdom/bias-filter-vs-entry-signal]] classifies signal *types* (bias filter vs entry signal); this page documents which *combination architecture* wins empirically.

## Related

- [[trading/mechanisms/scaffold-as-mechanism]] — companion generalization (scaffold determines timing across same-scaffold variants); the mechanism-class-level finding
- [[foundation/market-wisdom/bias-filter-vs-entry-signal]] — companion taxonomy (signal types); this page is the empirical-architecture half
- [[trading/mechanisms/moving-averages]] — canonical bias-filter mechanism (EMA20/EMA50 used as bias)
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — regime classification (compatible with this page; regime *classification* is fine, regime *gating-as-implementation* is what this page critiques)
- [[foundation/methodology/strategy-validation-discipline]] — §7.1 selection-bias context (asset-class universes warning, applied at parameter-class scale)
- [[trading/rejected/ema-bias-breakout-4h-v2-2026-05]] — supporting case 1 (canonical bias-filter winner)
- [[trading/backtest-results/direction-restriction-short-only-2026-05]] — supporting case 4 (direction-restriction = degenerate bias filter)
- [[trading/rejected/funding-flip-recovery-regime-gate-2026-05]] — supporting case 3 (regime gate hurts vs short-only control)
- Part I §9 — Mutation Operators (direction-restriction promoted with this page as design-context)
- [[trading/mechanisms/vol-regime-detection]] — vol-regime as embedded bias filter is N=5 candidate case for this design pattern (2026-05-08 ingest); pending DSL primitives + backtest evidence
- [[trading/mechanisms/term-structure-dynamics]] — IVTS-bucket-as-bias-filter is N=6 candidate case (2026-05-08 ingest)
- [[trading/mechanisms/skew-based-positioning-signals]] — skew-sign-as-bias-filter is N=7 candidate case (2026-05-08 ingest)
- [[foundation/methodology/bar-level-validation-overlay]] — bar-level state IS the validated bias-filter layer for any period-level signal (cross-mechanism); period-level claim is the candidate that needs validation
- [[foundation/methodology/team-stable-range-detector-methodology]] — worked example of period-level → bar-level overlay; embedded bias-filter principle applied to the team's stable-range detector
- [[trading/mechanisms/breakout-direction-asymmetry-by-macro]] — regime-conditional mechanism applying bar-level state gating; uses `stable_breaking_up` / `stable_breaking_down` bar-level filters
- [[foundation/sources/sinclair-volatility-trading]] — Ch 5 sticky-strike-vs-sticky-delta is the equity-options analog of bias-filter-vs-entry-signal taxonomy; conceptual back-reference

## Sources

- 2026-05-05 batch: the backtesting lab{ema_bias_breakout_4h_v2, htf_aligned_breakout_4h, bollinger_squeeze_4h_short_regime, bollinger_squeeze_4h_short_only, funding_flip_recovery_4h_short_regime, funding_flip_recovery_4h_short_only}/`
- the backtesting lab — heat-map analysis surfacing asymmetry
- 4 supporting wiki entries linked above
