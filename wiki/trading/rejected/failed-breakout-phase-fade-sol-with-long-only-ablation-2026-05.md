---
title: "failed_breakout_phase_fade_sol + long-only ablation — REJECTED for deployment; SOL long-bias regime amplifier confirmed at sub-deployment magnitude (2026-05)"
status: evergreen
created: 2026-05-19
updated: 2026-05-19
tags: [rejected, failed-breakout-reversal, phase-conditioned, direction-asymmetry, long-bias-regime-amplifier-confirmed, position-blocking-neutral-on-sol, sol, day-swing, sub-deployment-magnitude]
---

# failed_breakout_phase_fade_sol + long-only ablation — REJECTED for deployment; SOL long-bias regime amplifier confirmed at sub-deployment magnitude (2026-05)

> **Lead learning**: The parent `failed_breakout_phase_fade_sol` (bidirectional) refutes at aggregate Sharpe -0.11 because the SHORT side drags (N=88 / -$83 / WR 41%). Per-direction split shows LONG side has the working edge: N=74 / +$51 / Sharpe **+0.44**. The direction-restriction ablation `failed_breakout_phase_fade_sol_long_only` cleanly isolates this (N=81, Sharpe **+0.42**, essentially identical to parent's long subset). **Both falsifier-fail the 0.5 bar by ~0.08.** SOL long-bias regime amplifier character is confirmed (mechanism produces predicted long-side edge) but the magnitude is sub-deployment. Position-blocking-as-filter check shows NEUTRAL impact on SOL (vs value-add on ETH) — N=2 cross-symbol evidence that position-blocking is symbol-specific. **Verdict: REJECT for deployment; preserved as research-grade evidence for SOL long-bias framework.**

## What we learned

1. **SOL long-bias regime amplifier confirmed at trigger-family level.** The failed-breakout-fade mechanism on SOL has positive long-side edge (Sharpe 0.44 standalone) and negative short-side edge (-$83 net). This aligns with the per-symbol asymmetry framework's prediction: SOL down-breaks reverse upward at 71%, making `failed_downside_breakout` (long entry) the working direction; SOL up-breaks continue at 87.5%, making `failed_upside_breakout` (short entry) structurally wrong.

2. **Direction-restriction ablation produces parity-with-subset on SOL** (vs Sharpe-loss on ETH). The 7 additional unblocked longs in the single-leg ablation contributed +$1.06/trade — slightly positive, NEUTRAL impact on aggregate Sharpe. Position-blocking-as-filter is value-neutral on SOL.

3. **Sub-deployment magnitude is informative**. The mechanism IS producing the predicted character (long-side edge consistent with framework), but the per-trade edge ($0.69-0.73/trade) at SOL's volatility makes the standalone Sharpe ~0.4. Below the 0.5 research-grade bar, well below deployment-grade. **The framework's prediction validates qualitatively but doesn't yield deployable strategy at this operational form on this trigger.**

4. **Position-blocking value-add is symbol-specific.** N=2 cross-symbol evidence (this batch):
   - ETH (fakeout-fade character): position-blocking VALUE-ADD (Sharpe 0.91 with filter vs 0.49 without)
   - SOL (long-bias amplifier): position-blocking NEUTRAL (Sharpe 0.44 with vs 0.44 without)
   
   Hypothesis (untested): position-blocking value-add correlates with symbol's structural fakeout-fade character; structural absorption character (SOL long-bias) suppresses the filter's value. Codification candidate, N=2 cross-symbol but only one operational form tested.

## Hypothesis (Structured Format)

### Parent strategy hypothesis (REFUTED at aggregate)

```
Hypothesis:    During SOL `is_stable=True` phase, intra-bar wick exceeds
               prior-bar period running extreme but closes back inside.
               Stop-runners offside; fade the wick direction. ETH-symmetric
               mechanism transferred to SOL.

Mechanism:     Raschke turtle-soup + phase-data-layer + SOL TBD-character.

Observable:    SOLUSDT 4h, 2024-05-20 → 2026-05-08. SL/TP/time-stop
               identical to ETH variant.

Expected:      Uncertain — SOL's per-symbol asymmetry character was not
               yet codified at strategy design time. ETH transfer-test
               by default.

Falsifier:     Bidirectional aggregate Sharpe < 0.5 → reject.
```

**Parent result**: N=162, Sharpe **-0.113**, PSR(>0) 40.2%, PSR(>1) 5.1%, WR 45.1%, Net -$32, MDD 15.8%, Sharpe CI [-1.64, +1.16]. **Aggregate falsifier fires.**

**Per-direction split**:
- Long: N=74, Net +$51.39, WR 50.0%, Avg +$0.69/trade — modest positive edge
- Short: N=88, Net -$83.42, WR 40.9%, Avg -$0.95/trade — net-losing, drags aggregate

**Long-subset standalone Sharpe** (extracted from bidirectional): **+0.44**. Sub-deployment but research-grade indication that long-side mechanism has edge consistent with SOL long-bias amplifier character.

### Direction-restriction ablation hypothesis (MARGINAL FALSIFIER-FAIL)

```
Hypothesis:    Drop the failing short leg; retain identical long
               trigger. Single-leg version should isolate the parent's
               long-side standalone economics (+$0.69/trade × N=74).

Mechanism:     Same as parent long side. Direction restriction is the
               only mutation.

Observable:    Same data + capital + fees. SL/TP/time-stop unchanged.

Expected:      Sharpe ~0.6 per pickup-file estimate.

Falsifier:     Sharpe < 0.5 at N ≥ 30 → reject.
```

**Ablation result**: N=81 (vs parent's 74 longs — 7 additional), Sharpe **+0.424**, PSR(>0) 72.0%, PSR(>1) 20.0%, WR 48.1%, Net +$58.84, MDD 8.8%, Sharpe CI [-1.08, +1.74]. **Falsifier fires marginally** (0.42 < 0.5 by 0.076).

**Cause of (marginal) failure**: not position-blocking-related (the filter is neutral on SOL). The mechanism's per-trade magnitude is just below deployment-grade. SOL bull-regime structural absorption is real but the trigger captures only a modest slice of it.

## Why position-blocking is NEUTRAL on SOL {#position-blocking-neutral}

Contrast with ETH where position-blocking was value-add:

- **ETH (fakeout-fade)**: when a `failed_downside_breakout` (long) is open, a subsequent `failed_upside_breakout` (short) signal is often CONTINUATION-bound (the up-wick is part of a recovery from the downward fakeout that triggered the long). Blocking these prevents counterproductive short entries.

- **SOL (long-bias amplifier)**: when a `failed_downside_breakout` (long) is open, a subsequent `failed_upside_breakout` (short) signal still occurs during a market state where long-bias absorption is active. The blocked shorts would have been net-losing in either case (SOL bull-regime structural up-bias); blocking them is value-neutral because they weren't going to add value.

- **Symmetric view**: position-blocking acts as a filter only when the blocked signals on the opposite direction are SYSTEMATICALLY LOSING given the regime state at the time. ETH bull-regime fakeout-fade clustered firings produce systematic losers in the blocked-side; SOL bull-regime structural absorption produces neutral or slightly-positive losers (still positive bias on the working direction).

This is N=2 cross-symbol evidence but only one trigger family tested. Codification candidate pending recurrence.

## What this rejects

- **Deployment of `failed_breakout_phase_fade_sol` (bidirectional)** at aggregate Sharpe -0.11.
- **Deployment of `failed_breakout_phase_fade_sol_long_only`** at Sharpe 0.42 (marginal falsifier-fail).
- **The naive transfer assumption** that ETH-validated mechanism transfers to SOL at deployment magnitude (mechanism transfers qualitatively, magnitude sub-deployment).

## What this DOESN'T reject

- **The parent strategy as RESEARCH-GRADE evidence** for SOL long-bias regime amplifier framework. Long-side standalone Sharpe 0.44 is consistent with the framework's prediction.
- **Future LONG-side strategies on SOL** with different operational forms — different trigger geometry, different exit configuration, different flow filter. The framework supports mechanism class; this trigger is just sub-magnitude.
- **The phase data layer or trigger logic mechanically**. Clean; failure is in edge magnitude on this symbol.
- **Symbol-specific position-blocking-value framework** as a research direction. N=2 cross-symbol evidence (this rejection + ETH companion) suggests the value-add depends on structural character; codification candidate.

## Cross-symbol non-generalization {#cross-symbol}

Per established codification:
- ETH (fakeout-fade): companion rejection [[failed-breakout-phase-fade-eth-with-position-blocking-finding-2026-05]]. Position-blocking value-add. SHORT side carries.
- BTC (continuation): separate rejection [[failed-breakout-phase-fade-btc-2026-05]] (mechanism wrong-direction; both legs fail).
- SOL (this entry): long-bias amplifier. LONG side carries at sub-deployment magnitude. Position-blocking neutral.

The mechanism class fits the per-symbol asymmetry framework with three distinct outcomes:
- BTC: full rejection (wrong direction for continuation character)
- ETH: research-grade for short side at deployment-magnitude (with position-blocking filter)
- SOL: research-grade for long side at sub-deployment-magnitude (no filter benefit)

## Related artifacts

- Parent strategy: the backtesting lab (moved 2026-05-19)
- Ablation: the backtesting lab (moved 2026-05-19)
- Parent rerun (2026-05-19): the backtesting lab
- Ablation run: the backtesting lab
- Companion ETH rejection: [[failed-breakout-phase-fade-eth-with-position-blocking-finding-2026-05]]

## Related

- [[failed-breakout-phase-fade-eth-with-position-blocking-finding-2026-05]] — sibling rejection (ETH variant; position-blocking value-add)
- [[failed-breakout-phase-fade-btc-2026-05]] — sibling rejection (BTC variant; full mechanism rejection)
- [[../mechanisms/failed-breakout-reversal]] — parent mechanism page; SOL-specific long-side note
- [[../mechanisms/scaffold-as-mechanism]] — cross-symbol non-generalization
- [[../../foundation/asset-classes/sol-market-structure]] — long-bias regime amplifier framework; N+1 evidence

## Sources

- 2026-05-19 batch backtests (`runs/failed_breakout_phase_fade_sol_20260519_*`)
- Per-symbol asymmetry framework: [[../../foundation/asset-classes/sol-market-structure]]
- Companion arc: the research task queue
