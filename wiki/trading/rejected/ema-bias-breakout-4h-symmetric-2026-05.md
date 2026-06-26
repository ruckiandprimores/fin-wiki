---
title: "ema_bias_breakout_4h symmetric parent — rejected (mechanism real, deployment-grade fail); 2026-05"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [rejected, trend-following, breakout, mechanism-validated-but-edge-below-threshold, deployment-grade-failure, day-swing]
---

# ema_bias_breakout_4h symmetric parent — rejected (mechanism real, deployment-grade fail); 2026-05

> **TL;DR**: Trend-aligned breakout with EMA bias filter. Strategy-level falsifier **passed** (medSh +0.54; gross +$21/yr). Paired falsifier (counter-bias mirror) also **passed** with margin (+1.37 Sharpe gap, well above +0.20 threshold). **Mechanism is REAL.** But deployment-grade gate fails: net = gross +$21 − friction $71 = **−$50/yr** at $1K capital. Below $120/yr (12%) deployment threshold. Per [[foundation/methodology/deployment-edge-threshold|deployment-edge methodology]]: mechanism-validated-but-edge-below-threshold is its own rejection class. Partial truth: bias-filter > separate regime-gate empirical evidence (per [[trading/mechanisms/regime-gate-vs-bias-filter]] N=4 supporting cases).

## Hypothesis (Structured Form)

```
Hypothesis:    Trend-aligned breakout entries gated by EMA(20)/EMA(50)
               directional bias filter outperform breakout entries
               without bias filter.

Mechanism:     Murphy ch 9 (Moving Averages as bias filter) + Han, Kang,
               Ryu (2024) momentum + Zarattini (2025) trend-following
               literature: every entry IS bias-aligned by construction.
               Filter and entry signal co-evaluated at same timescale
               (matched).

Observable:    BTCUSDT perp 4h, 2025-05-03 → 2026-04-28, $1000 capital,
               fee 0.04%, 9216 parameter combinations.

Expected:      medSh ≥ +0.30; gross ATP ≥ $0.30; pass paired falsifier
               (counter-bias mirror should produce significantly worse
               result if mechanism is real).
               expected_net_edge: gross ≥ $5/trade for deployment grade.

Falsifier:     Strategy-level: medSh < 0 OR win rate < 30%.
               Paired: counter-bias produces equal-or-better Sharpe.
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp, $1000, fee 0.04%)

| Metric | Value | Falsifier criterion | Fired? |
|--------|---:|---|:---:|
| Median Sharpe | **+0.54** | ≥ 0 | NO (passed) |
| Median win rate | **0.36** | ≥ 30% | NO (passed) |
| Counter-bias Sharpe | **−0.83** | < parent (paired) | NO (paired falsifier passed; +1.37 gap) |

**Strategy-level falsifier passed. Paired falsifier (counter-bias) passed with +1.37 Sharpe gap (well above +0.20 threshold). Mechanism is REAL.**

## Net-of-Friction (per [[foundation/methodology/deployment-edge-threshold|deployment-edge methodology]])

| Component | Value |
|---|---:|
| Gross/yr ($1K capital) | +$21 |
| Friction/yr (1bp roundtrip × $5K position × ~71 trades) | $71 |
| **Net/yr** | **−$50** |

**Below deployment-grade threshold** ($120/yr at $1K = 12% return). Mechanism-validated-but-edge-below-threshold.

## Falsifier verdict: STRATEGY-LEVEL PASSED; DEPLOYMENT-GRADE FAILED

This is the canonical case for why the Part I §4 sharpening (added 2026-05-05) requires `expected_net_edge` upstream of backtest. The pre-registered falsifier passed; the deployment-grade gate (added later as separate methodology) fails. Without the deployment-grade gate as part of pre-registration, the strategy gets marked as productionize candidate when it shouldn't be.

## Partial Truth Retained

- **Mechanism validated**: paired falsifier passed with +1.37 Sharpe gap. The bias-filter mechanism is real and the counter-mirror confirms it.
- **Bias-filter > separate regime-gate** empirical evidence: this strategy is one of N=4 supporting cases in [[trading/mechanisms/regime-gate-vs-bias-filter|the design pattern page]] (paired with `htf_aligned_breakout_4h` which separately fails — confirms embedded bias filter > separate regime gate).
- **Counter-bias mirror archived as control**: per task spec, `ema_bias_breakout_4h_counter` was a paired-falsifier instrument; never a strategy in its own right; functionally complete. No separate rejected/ entry needed.

## Trial Count on Dataset

≥13 strategies tested in this batch. Strategy-level falsifier passed; deployment-grade gate fails. Trial count is robust to multiplicity correction at the *strategy-level* judgment; the deployment-grade-fail verdict is independent of trial multiplicity (it's a friction calculation, not a statistical test).

## Reason Classification

- [ ] Strategy-level falsifier fired
- [x] **Deployment-grade gate fired** — below net-edge threshold
- [x] **Mechanism validated** — paired falsifier passed (+1.37 Sharpe gap)
- [x] **Below deployment threshold** — net −$50/yr at $1K
- [ ] No falsifier articulated
- [ ] Mechanism absent

## Key Takeaways

- **Mechanism is REAL** — paired falsifier (counter-bias mirror) passed with +1.37 Sharpe gap. This is statistical-grade validated.
- **Deployment-grade fails** — mechanism produces gross edge below realistic friction at any sensible scale.
- **Worked case for the 2026-05-05 PM-2 methodology sharpening**: had `expected_net_edge` been required upstream (per Part I §4), this strategy would have been flagged as research-grade-only at hypothesis stage. Added retrospectively after the fact.
- **Partial truth: bias-filter > separate regime-gate** validated; one of N=4 supporting cases in [[trading/mechanisms/regime-gate-vs-bias-filter]].
- **Worth re-attempting at deployment-grade**: stacking with longer holds OR larger ATR thresholds (lower trade frequency → less friction) might clear threshold; would require *new* hypothesis with own pre-registration per Marcos' Second Law.

## Related

- [[trading/rejected/ema-bias-breakout-4h-v2-2026-05]] — heat-map-refined v2; same mechanism class; verdict reversal
- [[trading/rejected/htf-aligned-breakout-4h-2026-05]] — separate-HTF-gate variant; failed for different reason (gate hurts)
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — design pattern; this strategy is one of N=4 supporting cases
- [[trading/mechanisms/moving-averages]] — canonical bias-filter mechanism page
- [[foundation/market-wisdom/bias-filter-vs-entry-signal]] — taxonomy; embedded bias filter classification
- [[foundation/methodology/deployment-edge-threshold]] — net-edge gate
- [[foundation/methodology/strategy-validation-discipline]] — paired-falsifier discipline
- Part I §4 — Structured Hypothesis Format (sharpened 2026-05-05 to require expected_net_edge)

## Sources

- the backtesting lab — symmetric parent
- the backtesting lab — counter-bias mirror (archived)
- the backtesting lab — heat-map-refined variant (also rejected)
- the backtesting lab — full reasoning
- Han, Kang, Ryu (2024) — momentum literature
- Zarattini (2025) — trend-following literature
- Murphy, *Technical Analysis of the Financial Markets* ch 9 — bias-filter mechanism foundation
