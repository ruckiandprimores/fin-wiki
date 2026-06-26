---
title: "funding_flip_recovery_4h short-regime gate — rejected (2026-05)"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [rejected, funding-cycle, regime-gating, short-only, mutation-rejected, falsifier-fired, behavioral-positioning-unwind, day-swing]
---

# funding_flip_recovery_4h short-regime gate — rejected (2026-05)

> **TL;DR**: Adding a slow-EMA(50/200) bearish-regime gate on top of short-only funding-flip-recovery **HURTS** vs unconditional short-only control. Pre-registered falsifier fired (Sharpe gap −0.16; control wins). Direction restriction stands; regime LAYER ON TOP rejected. Companion N=2 evidence (with `bollinger_squeeze_4h_short_regime` operating only on a degenerate parameter subset) overturns the prior worldview that regime-gating is a universal design pattern. Regime-gating is **mechanism-specific**.

## Hypothesis (Structured Format)

```
Hypothesis:    Adding a bearish-regime slow-EMA gate (50/200) to short-only
               funding-flip-recovery outperforms unconditional short-only on:
               (a) Median Sharpe gap ≥ +0.30 in favor of regime-gated; AND
               (b) Median ATP ratio ≥ 1.10× regime-gated/control.

Mechanism:     Funding-flip-recovery captures forced-positioning-unwind
               after a positive→negative funding regime flip (long-side
               positioning blowoff → cascade → reclaim). Bearish slow-EMA
               regime might focus the mechanism on windows where the unwind
               compounds with broader downtrend rather than fights it.

Observable:    Backtest grid 2025-05-03 → 2026-04-28, BTCUSDT perp,
               $1000 capital, fee 0.04%. Compare regime-gated grid vs
               unconditional short-only grid; report median Sharpe, ATP,
               %PF>1, %val+.

Expected:      If gate adds value, regime-gated Sharpe ≥+0.80; control
               (short-only) Sharpe ~+0.50; gap ≥+0.30.

Falsifier:     If regime-gated Sharpe TRAILS control by ≥0.10 OR ATP
               ratio < 0.90×, the regime layer is mis-specified
               (wrong direction, wrong scale, or noise). Strategy goes
               to rejected/ with this entry.
```

Pre-registered in the backtesting lab.

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp, $1000, fee 0.04%)

| Metric | Mutation (regime-gated) | Control (short-only) | Gap |
|--------|---:|---:|---:|
| Median Sharpe | +0.34 | +0.50 | **−0.16** (control wins) |
| Median ATP | $0.10 | $0.20 | 2× control wins |
| %PF>1 | 54.0% | 58.6% | −4.6pp |
| %val+ | 84.3% | 80.9% | +3.4pp (only metric where regime version wins) |
| Eligible runs | 4473/8748 (51%) | 2052/5184 (40%) | comparable filter rate |
| Median trades | 48 | 50 | comparable |

## Falsifier verdict: FIRED

Mutation Sharpe **trails** control by 0.16 (threshold required ≥+0.30 in mutation's favor). ATP is half the control's. Per spec: *"Regime detector is wrong direction or wrong scale; investigate vol-regime, funding-regime, or ADX-style alternatives."*

The +3.4pp lead in %val+ is the only metric where regime version edges out — but on its own it does not pass the falsifier and is plausibly a small-sample fluctuation given the comparable trade counts and worse Sharpe / ATP / %PF>1.

## Partial truth retained

**Direction restriction (drop entry_long) IS validated.** Control (short-only, no regime gate) shows +0.50 Sharpe vs parent symmetric's −0.11 — that's a +0.61 Sharpe gap from direction restriction alone, no regime gate. See companion entry [[trading/backtest-results/direction-restriction-short-only-2026-05]] for the validated mutation, and [[trading/mechanisms/funding-cycle-dynamics]] for the family page.

What this rejection isolates: the **regime layer on top** of the already-validated short-only is the bug. Direction restriction works; stacking a slow-EMA gate on top adds noise.

## Cross-strategy pattern (load-bearing for worldview)

Same regime-gate design pattern applied to `bollinger_squeeze_4h_short_regime` produced **opposite-direction** failure mode: gate appears to add value at narrow squeeze settings (medSh +2.24) but only on **63 of 10,368 parameter combinations (0.6%)** — all sharing `squeeze_window=3` AND `squeeze_pct=0.25` (most permissive squeeze settings). Apparent edge is partly real on those 63 runs but partly selection bias (broader parameter space wasn't testable because gate eliminated trade volume). See [[trading/mechanisms/regime-gate-vs-bias-filter]] for the worked example.

> **Worldview update.** Regime-gating-as-universal-design-pattern is **rejected**. It is mechanism-specific: the bias filter embedded in `ema_bias_breakout_4h_v2` outperforms the same-window separate regime gates by margin. The new page [[trading/mechanisms/regime-gate-vs-bias-filter]] documents the empirical pattern with N=4 supporting cases.

## Trial count on dataset (per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] §2.2)

Strategies tested on BTCUSDT perp 2025-05/2026-04 window so far (via the backtesting lab): **≥13 trials** including parents, mutations, and controls. Per [[glossary/deflated-sharpe-ratio|DSR adjustment]], this rejection is *less* sensitive to multiplicity correction than passes are — a falsifier-fired result trails control regardless of trial count. But the trial count is recorded for the symmetric-strategy fairness of any future analysis on this dataset.

## Reason classification

- [x] **Falsifier fired in backtest** — pre-registered ≥+0.30 Sharpe gap not met (gap was −0.16)
- [x] **Mechanism layer rejected** — direction restriction stands, regime layer on top is the bug
- [ ] No falsifier articulated (pre-registration was clean)
- [ ] Already arbitraged
- [ ] Mechanism not present in current crypto microstructure
- [ ] Graveyard duplicate

## What partial truth remains?

| Component | Status | Evidence |
|-----------|--------|----------|
| **Direction restriction (short-only)** | ✅ Validated as mutation | Control's +0.50 Sharpe vs parent's −0.11 — see [[trading/backtest-results/direction-restriction-short-only-2026-05]] |
| **Underlying funding-flip mechanism (long-positioning unwind)** | Open / family-context | Family page [[trading/mechanisms/funding-cycle-dynamics]]; mechanism not invalidated by this test, only the regime layer was |
| **Slow-EMA regime gate as design pattern** | ❌ Rejected as universal | See [[trading/mechanisms/regime-gate-vs-bias-filter]] |

The mutation's failure does not invalidate the funding-flip mechanism itself — the short-only control validates that mechanism + direction restriction together. Future revisits should test alternative regime detectors (vol-regime, funding-regime concentration, ADX) but **not** slow-EMA on this mechanism.

## Caveats per [[foundation/methodology/strategy-validation-discipline|validation discipline]]

- **Single-window backtest** — 2025-05 → 2026-04 only. Out-of-window validation absent. The 2025-26 window was bearish-dominant; same logic in a bullish year may produce different asymmetry. Per [[foundation/methodology/strategy-validation-discipline]] §13 crypto-specific concerns: regime non-stationarity caveats apply.
- **PSR/DSR not pipeline-implemented** — confidence intervals on the −0.16 Sharpe gap are not computed. Direction of result (control wins) is robust to multiplicity correction; magnitude is not certified.
- **Walk-forward / multi-window absent** — pipeline limitation per [[foundation/methodology/strategy-validation-discipline]] §11.

## Key Takeaways

- **Regime-gating-as-universal-design-pattern: REJECTED.** Mechanism-specific. The same gate that hurts here may help elsewhere — but it is not a default.
- **Direction restriction is the load-bearing improvement** — control wins by +0.61 Sharpe vs symmetric parent purely from dropping `entry_long`. See [[trading/backtest-results/direction-restriction-short-only-2026-05]] for the full direction-restriction evidence and Part I §9 for the mutation-operator framework.
- **Stacking adds noise** — once a strategy's edge is captured by a clean transformation (direction restriction), adding a second filter on top tends to over-restrict.
- **Falsifier discipline worked.** Pre-registered ≥+0.30 Sharpe gap was specific and binary; the −0.16 result is unambiguous. Per Part IV: "every hypothesis must specify what would prove it wrong."

## Related

- [[trading/backtest-results/direction-restriction-short-only-2026-05]] — validated companion mutation (the control that won)
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — the design-pattern page synthesizing this rejection with 3 other supporting cases
- [[trading/mechanisms/funding-cycle-dynamics]] — family page; the underlying mechanism is not invalidated, only the regime layer
- [[trading/mechanisms/behavioral-positioning-unwind]] — sibling family
- [[foundation/sources/wyckoff-method]] — Spring/Upthrust analog (forced-unwind framing)
- [[foundation/methodology/strategy-validation-discipline]] — Marcos' Third Law trial counting + falsifier discipline
- Part I §9 — Mutation Operators (regime filter rejected as default; direction restriction promoted)
- [[trading/rejected/turtle-soup-4h-2026-05]] — companion 2026-05-05 rejection (different mechanism class, same window)

## Sources

- the backtesting lab — strategy files + pre-registration
- the backtesting lab — control that won (validated companion)
- the backtesting lab — symmetric parent
- the backtesting lab — heat-map analysis surfacing the asymmetry
