---
title: "failed_breakout_phase_fade_eth + short-only ablation — REJECTED for deployment; position-blocking-as-inadvertent-filter discovery (2026-05)"
status: evergreen
created: 2026-05-19
updated: 2026-05-19
tags: [rejected, failed-breakout-reversal, phase-conditioned, position-management-as-filter, direction-restriction-ablation, ablation-vs-subset-methodology, eth, day-swing, research-grade-not-deployment]
---

# failed_breakout_phase_fade_eth + short-only ablation — REJECTED for deployment; position-blocking-as-inadvertent-filter discovery (2026-05)

> **Lead learning**: The parent `failed_breakout_phase_fade_eth` (bidirectional) achieves research-grade aggregate Sharpe 0.60 on N=166, with a clean per-direction split — short side N=77 / +$133 / WR 51% / Sharpe **0.91** carries; long side N=89 / -$17 / WR 48% / Sharpe 0.02 flat. The direction-restriction ablation `failed_breakout_phase_fade_eth_short_only` (drop long leg) was expected to lift aggregate Sharpe by removing the long-leg drag — but actually produced Sharpe 0.49 (single-window) and 2/4 folds walk-forward, BELOW the parent's pure short subset's 0.91 standalone Sharpe. **The discovery: bidirectional position-management acts as an inadvertent quality filter** — 14 NEW shorts that fire when the long leg is dropped contributed -$4.60/trade net-negative. The bidirectional architecture was inadvertently filtering them out via position-occupancy conflict. **Verdict: REJECT short-only ablation as deployment standalone; parent is research-grade-not-deployment via single-window Sharpe.**

## What we learned

1. **Position-management-as-inadvertent-filter is a discoverable mechanism-level phenomenon.** Bidirectional strategies don't fire signals when a position in the opposite direction is open. This blocking has been treated as an architectural detail with no economic content; this batch shows it can have substantial value-add (or value-neutral) impact on per-trade economics depending on the symbol.

2. **Direction-restriction ablation is NOT a clean isolation of the parent's same-direction subset.** The right comparison for "did this mutation improve things?" is ablation-vs-parent's-same-side-subset, NOT ablation-vs-parent-aggregate. On ETH, ablation includes 13 additional shorts that didn't fire in the bidirectional version; those 13 are net-losing.

3. **The selectivity spectrum has 3 distinct levels on this trigger:**

   | Operational form | N | Sharpe | What kind of selectivity |
   |---|---|---|---|
   | Multi-bar + taker_buy_z flow filter (range_tunnel v2) | 32 | +0.18 | Wrong selectivity (filtered out productive trades) |
   | Single-leg ablation, no filter | 90 | +0.49 | No selectivity (admits previously-blocked bad shorts) |
   | Parent SHORT subset (bidirectional position-block filter active) | 77 | **+0.91** | Right selectivity (inadvertent, position-occupancy) |

   The right kind of selectivity dominates by a wide margin.

4. **Symbol-specific filter character.** Symmetric check on SOL (failed_breakout_phase_fade_sol → sol_long_only ablation): position-blocking is NEUTRAL — the 7 additional longs in single-leg are slightly positive (+$1.06/trade). SOL's long-bias regime amplifier means unblocked longs aren't systematically net-losing the way ETH unblocked shorts are. **Position-blocking value depends on symbol structural character.**

5. **The range_tunnel rejection arc's "selectivity removes edge" finding is now sharpened.** range_tunnel's multi-bar + flow filter was tested against the parent's aggregate Sharpe 0.60, looked like "selectivity subtracted edge." The deeper truth: the parent's bidirectional architecture was already DOING selectivity correctly via position-blocking; range_tunnel's multi-bar + flow filter was wrong-class selectivity that competed with and lost to the inadvertent filter.

## Hypothesis (Structured Format)

### Parent strategy hypothesis (validated as research-grade, not deployment-grade)

```
Hypothesis:    During ETH `is_stable=True` phase, intra-bar wick exceeds
               prior-bar period running extreme but closes back inside.
               Stop-runners offside; fade the wick direction. Phase-
               gating + ETH structural fakeout-fade character should
               produce edge.

Mechanism:     Raschke turtle-soup + phase-data-layer structural levels
               + ETH fakeout-fade character.

Observable:    ETHUSDT 4h, 2024-05-20 → 2026-05-08. SL 1.0 ATR / TP 2.5
               ATR / time stop 12h / risk_pct(0.01).

Expected:      Aggregate Sharpe 0.7-1.2 if ETH fakeout-fade aligns;
               direction-asymmetry possible (short side stronger).

Falsifier:     Bidirectional aggregate Sharpe < 0.5 → reject. Per-leg
               direction-asymmetry as secondary finding.
```

**Parent result**: N=166, Sharpe **+0.596**, PSR(>0) 78.9%, PSR(>1) 26.6%, WR 49.4%, Net +$117, MDD 9.7%, Sharpe CI [-0.88, +1.89]. **Aggregate falsifier (0.5) passes**; not deployment-grade (PSR>1 26.6% << 70%).

**Per-direction split** (the load-bearing finding):
- Long: N=89, Net -$16.87, WR 48.3%, Avg -$0.19/trade — flat-negative
- **Short: N=77, Net +$133.43, WR 50.6%, Avg +$1.73/trade — strong unidirectional edge**

**Short-subset standalone Sharpe** (extracted from bidirectional trades): **+0.91**. This is the unidirectional ceiling on this trigger × symbol × detector × window.

### Direction-restriction ablation hypothesis (FALSIFIED)

```
Hypothesis:    Drop the flat-negative long leg; retain identical short
               trigger. Single-leg version should isolate the parent's
               short-side economics (+$1.73/trade × N=77 → Sharpe ~1.3).

Mechanism:     Same as parent short side. Direction restriction is the
               only mutation.

Observable:    Same data + capital + fees. SL/TP/time-stop unchanged
               from parent.

Expected:      Sharpe ~1.3, PSR ~92%+ per pickup-file estimate.

Falsifier:     Sharpe < 1.0 at N ≥ 30 → reject.
```

**Ablation result**: N=**90** (NOT 77), Sharpe **+0.489**, PSR(>0) 74.2%, PSR(>1) 21.9%, WR 45.6%, Net +$73.60, MDD 8.9%, Sharpe CI [-0.97, +1.78]. **Falsifier fires.**

**Walk-forward (4-fold)**: 2/4 positive, 1/4 Sharpe > 1 ann. Fold range [-1.23, +2.41] — high variance. Single-window Sharpe 0.49 is concentrated in 1 fold; not temporally consistent.

**Cause of failure (named)**: position-blocking-as-inadvertent-filter. The single-leg ablation fires 90 shorts vs the parent's 77 — 13 additional shorts (16%) fired only because no long position was open to block them. Cross-strategy overlap: 76/77 (99%) of parent's shorts ARE in the ablation; 14/90 (16%) of ablation's shorts are NEW.

**Economics of the 14 new shorts** (computed by subtraction):
- Net contribution: +$73.60 (ablation total) - $133.43 (parent's 77 shorts in-bidirectional) = **-$59.83**
- Per-trade: -$59.83 / 13 ≈ **-$4.60 per trade — net-losing**

The 14 unblocked shorts were systematically losing. The bidirectional engine's position-occupancy logic was inadvertently keeping them out.

## Why position-blocking acts as a filter on ETH {#position-blocking-mechanism}

The ETH bull-regime window 2024-05/2026-05 produces correlated trigger conditions:
- When ETH is trending up (bull-regime sustained), `failed_upside_breakout` (short trigger) and `failed_downside_breakout` (long trigger) tend to fire in clustered periods — both wick types appear during volatile intra-stable-phase moments.
- When a long position is open from `failed_downside_breakout`, a subsequent `failed_upside_breakout` is blocked. Many such cases occurred during transient up-moves that were not high-quality short setups (they were continuation-bound rather than reversion-bound).
- The bidirectional engine's "no opposing position while long is open" rule inadvertently filtered out the continuation-bound up-wicks while letting through the reversion-bound up-wicks (which occurred when long position was NOT open).
- **Selection bias**: the surviving 77 shorts in bidirectional were precisely the ones where the market state didn't simultaneously support a long entry — implying weaker bull-pressure at the moment of the short signal. Those shorts had cleaner reversion-bound character.

This isn't a deliberate filter — it's a side effect of position-occupancy logic. But it's economically meaningful (Sharpe 0.91 vs 0.49 — DOUBLES the unidirectional Sharpe).

## Cross-symbol check (SOL): position-blocking is NEUTRAL

Symmetric test on `failed_breakout_phase_fade_sol`:
- Parent SOL: N=162, Sharpe -0.11. Per-direction: long N=74 / +$51 / Sharpe **+0.44**; short N=88 / -$83 / Sharpe negative.
- Ablation (sol_long_only): N=81 (7 additional longs), Sharpe **+0.44** (essentially identical to parent's pure long subset).
- The 7 additional longs contributed +$1.06/trade — slightly positive, neutral on aggregate Sharpe.

**Reading**: SOL's structural long-bias regime amplifier character means even the previously-blocked longs benefit from bull-regime structural absorption. Position-blocking is NEUTRAL on SOL where it was VALUE-ADD on ETH. Symbol-specific.

## Methodological implications

### Direction-restriction ablation methodology sharpening

Current methodology (pickup-file P1.2 implicit framework): "drop the failing leg, expect Sharpe lift from parent aggregate to working-leg standalone."

Sharpened methodology (post-this-batch):

1. **Compare ablation against parent's same-side SUBSET, not parent aggregate.** ETH: parent aggregate 0.60 vs parent short subset 0.91. The "lift" claim depends on which baseline.

2. **Account for position-blocking-as-filter side effect.** Ablation removes the bidirectional filter; the resulting trade population is different from the parent's same-side subset. If the bidirectional architecture has filter value on this symbol, ablation may REDUCE Sharpe vs same-side subset.

3. **Diagnostic test before ablation**: compute parent's per-side standalone Sharpe FIRST. If parent SHORT subset standalone is materially > parent aggregate, the bidirectional architecture has filter value; ablation may not improve standalone metrics even though it isolates the working direction.

### When position-blocking is value-add vs value-neutral

Hypothesis (untested cross-symbol): position-blocking is value-add when:
- The symbol has STRONG fakeout-fade character (ETH) — clustered fade/continuation conditions; opposing-side openness predicts low-quality continuation-bound signals on the failing side
- The detector produces clustered firing periods (multiple fade signals close in time)

Position-blocking is value-neutral when:
- The symbol has STRUCTURAL ABSORPTION character (SOL long-bias amplifier) — even continuation-bound signals on the working side still get absorbed into the bias direction
- The detector produces sparse firing (less position-overlap pressure)

N=2 cross-symbol evidence (ETH value-add, SOL neutral). Not enough for codification; flag for cross-session recurrence.

## What this rejects

- **Deployment of `failed_breakout_phase_fade_eth_short_only` as a standalone strategy.** Falsifier-fail at single-window Sharpe 0.49 < 1.0 bar + walk-forward variance.
- **The naive direction-restriction ablation methodology** (compare ablation vs parent aggregate without checking same-side subset).
- **`failed_breakout_phase_fade_eth` for deployment-grade tier** — research-grade Sharpe 0.60 with PSR>1 26.6% is well below deployment thresholds.

## What this DOESN'T reject

- **The parent strategy as RESEARCH-GRADE** — short-side standalone Sharpe 0.91 is the cleanest unidirectional finding in the failed_breakout family. Research-grade for cross-strategy comparison, mechanism validation, mutation-arc anchoring.

- **The SHORT-side mechanism on ETH**. The bidirectional context where SHORT trades achieve +$1.73/trade × N=77 / Sharpe 0.91 is the validated form. Deployment would require the bidirectional architecture for the filter side-effect — single-leg deployment loses the filter.

- **The phase data layer or trigger logic**. Mechanically clean. The failure is in the ablation methodology assumption.

- **Position-management-as-inadvertent-filter as a discoverable phenomenon**. The discovery is preserved as a methodology learning; future direction-restriction ablations should account for it.

- **Mutation directions for further range-fade variants on ETH** (different flow filter, different multi-bar pattern, different timeframe, different detector) — these remain valid future candidates per range_tunnel rejection arc's enumerated untested space.

## Cross-symbol non-generalization {#cross-symbol}

Per established codification:
- BTC: continuation character makes fade family wrong-direction; failed_breakout_phase_fade_btc rejected separately (N=161, see [[failed-breakout-phase-fade-btc-2026-05]]).
- SOL: long-bias regime amplifier; SOL ablation arc has its own rejection entry (see [[failed-breakout-phase-fade-sol-with-long-only-ablation-2026-05]]).

## Related artifacts

- Parent strategy: the backtesting lab (moved 2026-05-19)
- Ablation: the backtesting lab (moved 2026-05-19)
- Parent rerun (2026-05-19): the backtesting lab
- Ablation run: the backtesting lab
- Walk-forward (ablation): the backtesting lab
- Companion arc rejection: [[range-tunnel-wyckoff-eth-multi-bar-flow-2026-05]]

## Related

- [[range-tunnel-wyckoff-eth-multi-bar-flow-2026-05]] — companion arc; this entry completes the explanation for why range_tunnel v2 underperformed (wrong selectivity competed with position-blocking-as-filter and lost)
- [[../mechanisms/scaffold-as-mechanism]] — sharpening: direction-restriction comparison methodology + position-blocking-as-inadvertent-filter
- [[../mechanisms/failed-breakout-reversal]] — parent mechanism page; ETH-specific position-blocking note
- [[failed-breakout-phase-fade-sol-with-long-only-ablation-2026-05]] — sibling rejection (SOL variant; cross-symbol asymmetry context)
- [[../../foundation/asset-classes/btc-market-structure]] — per-symbol asymmetry framework

## Sources

- 2026-05-19 batch backtests + walk-forward analysis (`runs/failed_breakout_phase_fade_eth_20260519_*`, `runs/eth_short_only_wf_20260519/`)
- 2026-05-18 prior batch decomposition (for parent's per-direction split anchor)
- Companion arc analysis: the research task queue
- Research process task (N=1 codification candidate): the research task queue (Signal 5: position-management-as-inadvertent-filter)
