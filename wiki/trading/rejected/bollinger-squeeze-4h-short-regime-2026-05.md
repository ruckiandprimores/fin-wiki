---
title: "bollinger_squeeze_4h short_regime — INCONCLUSIVE (selection bias); 2026-05"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [inconclusive, vol-cycle, regime-conditioning, selection-bias-flagged, mutation-pattern, day-swing]
---

# bollinger_squeeze_4h short_regime — INCONCLUSIVE (selection bias); 2026-05

> **TL;DR**: Numerically strong result (medSh +2.24; gross +$102/yr; net +$60/yr) but **only 63 of 10,368 grid runs eligible (0.6%)** — all 63 share `squeeze_window=3 AND squeeze_pct=0.25` (single parameter combination). Apparent positive could be selection bias on degenerate parameter subset rather than genuine regime-gate edge. **NOT productionizable**; required follow-up is matched-parameter comparison (run grid restricted to single combination on both regime-gated and control; compare like-for-like). Until that runs, verdict is INCONCLUSIVE — listed in `rejected/` because it cannot proceed to backtest-results without resolution. Worked example for [[foundation/methodology/strategy-validation-discipline|validation discipline]] §7.1 (asset-class universes warning, applied at parameter-class scale).

## Hypothesis (Structured Form)

```
Hypothesis:    Stacking direction restriction (drop entry_long) AND
               regime gate (slow EMA 50/200 bearish) on Bollinger squeeze
               outperforms unconditional short-only by enough margin to
               compensate for the additional restriction.

Mechanism:     Combined filters concentrate strategy on highest-edge
               regime + direction. Theoretical max-confidence trade.

Observable:    BTCUSDT perp 4h, 2025-05-03 → 2026-04-28, $1000 capital,
               fee 0.04%, 10368 parameter combinations.

Expected:      Sharpe gain ≥ +0.30 over short-only control.

Falsifier:     Indeterminate if eligible-runs subset is too narrow
               (selection bias suspect); definite reject if Sharpe gap
               negative.
```

## Result — Numerically Strong But Suspect

| Metric | Value | Note |
|--------|---:|---|
| Median Sharpe | **+2.24** | Apparent strong pass |
| Gross/yr ($1K capital) | +$102 | Apparent above-threshold |
| Friction/yr | ~$42 | Standard |
| **Net/yr** | **+$60** | Apparent positive |
| Eligible runs | **63 / 10,368 (0.6%)** | **Selection bias flag** |
| Shared parameter values across 63 | `squeeze_window=3` AND `squeeze_pct=0.25` | Single parameter combination |

## Falsifier verdict: INDETERMINATE (selection bias prevents clean reading)

The +2.24 Sharpe is computed on a degenerate subset — the only 63 parameter combinations where the regime gate left enough trade volume to evaluate. **All 63 share the most-permissive squeeze settings.** Apparent edge is partly real (those 63 runs do well under the bearish regime gate) but partly selection bias (broader parameter space wasn't testable because gate eliminated trade volume).

This is the canonical worked example documented in [[trading/mechanisms/regime-gate-vs-bias-filter|regime-gate-vs-bias-filter]] §Selection-bias diagnostic.

## What This Result Cannot Distinguish

- **Hypothesis A**: regime gate genuinely adds value at all parameter combinations, but min-trade-filter eliminates ineligible ones for measurement reasons. The 63 eligible are representative. Verdict: **edge is real**.
- **Hypothesis B**: regime gate hurts at most parameter combinations (eliminates trades); only at the most-permissive squeeze settings does enough trade volume survive that the strategy can be evaluated; on those 63, the gate happens to align with the genuine bearish-regime edge that ALSO operates without the gate. Verdict: **edge is not real (selection bias)**.

The current grid output cannot distinguish A from B.

## Required Follow-Up Test (matched-parameter comparison)

To resolve: re-run grid restricted to the single parameter combination (`squeeze_window=3, squeeze_pct=0.25`) on both:
1. The `_short_regime` strategy (with regime gate)
2. The `_short_only` control (without regime gate)

Compare like-for-like:
- If `_short_regime` outperforms `_short_only` materially (≥ +0.30 Sharpe gap) at the same parameter combination → Hypothesis A confirmed; regime gate adds value
- If they're comparable at the same parameter combination → Hypothesis B confirmed; the apparent edge is selection bias

This test is owner-flagged for Task 5 follow-up. **Until it runs, this strategy stays in INCONCLUSIVE status.**

## Net-of-Friction Status

Even granting the +$60/yr result at face value, this is **below the $120/yr (12%/yr) deployment-grade threshold** at $1K capital. So even Hypothesis A confirmation wouldn't make this productionize-grade — it would just promote it from "selection bias suspect" to "research-grade-only with edge."

## Cross-Strategy Pattern Significance

The companion [[trading/rejected/funding-flip-recovery-regime-gate-2026-05|funding_flip regime-gate rejection]] shows the same regime-gate-stacking pattern produces the OPPOSITE failure mode: full parameter-space coverage, but Sharpe gap NEGATIVE vs short-only control. Combined evidence:

| Strategy class | Short-only control | Short-regime stacking | Verdict |
|---|---|---|---|
| Funding-flip recovery | +0.50 Sharpe | +0.34 Sharpe (gap −0.16) | Regime-gate-stack hurts |
| Bollinger squeeze | +1.02 Sharpe | apparent +2.24 BUT only 0.6% eligible | INCONCLUSIVE (selection bias) |

**Cross-strategy worldview update**: regime-gating-as-universal-design-pattern is rejected per [[trading/mechanisms/regime-gate-vs-bias-filter]]. Stacking either hurts (funding-flip) or operates at degenerate parameter subset (bollinger). Mechanism-specific corollaries documented in the design-pattern page.

## Trial Count on Dataset

≥13 strategies tested in this batch. The selection-bias diagnostic is itself part of the trial-count audit — degenerate-subset results should be flagged in trial accounting.

## Reason Classification

- [x] **INCONCLUSIVE** — selection bias prevents clean verdict
- [ ] Falsifier fired (definitively)
- [ ] Falsifier passed (definitively)
- [x] Required follow-up flagged (matched-parameter test)
- [x] Below deployment-grade threshold even at face-value reading

## Key Takeaways

- **Numerically strong result is suspect**: only 0.6% of parameter grid eligible.
- **Cannot distinguish "edge is real" from "selection bias"** without matched-parameter follow-up test.
- **NOT productionize candidate** at any reading — even granting face value, $60/yr below $120/yr threshold.
- **Worked example** for [[foundation/methodology/strategy-validation-discipline|validation discipline]] §7.1 selection-bias warning at parameter-class scale.
- **Pairs with funding-flip regime-gate rejection** to confirm: regime-gating-as-universal-design-pattern is rejected; stacking is mechanism-specific failure mode.
- **Required follow-up**: matched-parameter test of single combination (`squeeze_window=3, squeeze_pct=0.25`) on both regime-gated and control; resolves Hypothesis A vs B.

## Related

- [[trading/mechanisms/regime-gate-vs-bias-filter]] — design pattern; this is the canonical selection-bias diagnostic worked example
- [[trading/rejected/bollinger-squeeze-4h-symmetric-2026-05]] — symmetric parent (rejected; full parameter coverage)
- [[trading/backtest-results/direction-restriction-short-only-2026-05]] — short-only control (research-grade-only)
- [[trading/rejected/funding-flip-recovery-regime-gate-2026-05]] — companion regime-gate-stacking failure (different failure mode)
- [[foundation/methodology/strategy-validation-discipline]] — §7.1 selection-bias context
- [[foundation/methodology/deployment-edge-threshold]] — net-edge gate (face-value $60/yr below threshold)

## Sources

- the backtesting lab — strategy files
- the backtesting lab — full reasoning
