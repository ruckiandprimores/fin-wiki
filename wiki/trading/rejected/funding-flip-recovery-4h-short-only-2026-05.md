---
title: "funding_flip_recovery_4h_short_only — REJECTED (direction restriction insufficient); 2026-05"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [rejected, funding-cycle, direction-restriction, mechanism-real, edge-below-threshold, day-swing]
---

# funding_flip_recovery_4h_short_only — REJECTED (direction restriction insufficient); 2026-05

> **TL;DR**: Direction-restriction mutation (drop entry_long) on funding-flip-recovery confirms the asymmetric edge: gross +$11/yr at $1K capital vs symmetric parent's gross-negative. **Mechanism is real on short side**. But friction $50/yr eats the edge: net **−$39/yr** at $1K with $5K position. **Below deployment-grade threshold**. The mutation pattern (direction restriction) is validated as a discipline; *this specific strategy at this scale* doesn't clear threshold. Combined with [[trading/rejected/bollinger-squeeze-4h-symmetric-2026-05|bollinger short_only research-grade-only]], constitutes N=2 supporting cases for Part I §9 direction-restriction operator — pattern intact, individual deployment readiness not.

## Verdict-Change Note

This strategy was originally documented in the joint [[trading/backtest-results/direction-restriction-short-only-2026-05|direction-restriction-short-only joint backtest-results entry]] (2026-05-05 PM) alongside `bollinger_squeeze_4h_short_only`. The 2026-05-05 PM-2 deployment-edge methodology recalibrated both as borderline / research-grade-only. The 2026-05-05 PM-3 deployment-grade stocktake escalates this strategy specifically to **REJECTED** because Net −$39/yr is *negative*, not borderline.

The `bollinger_squeeze_4h_short_only` companion in the joint entry remains research-grade-only (Net +$8/yr — closest-to-passing in inventory; positive but below threshold).

The joint entry is updated to reflect the split: bollinger short_only research-grade-only stays in backtest-results/; funding-flip short_only moves to rejected/ via this entry.

## Hypothesis (Structured Form)

```
Hypothesis:    Drop entry_long on funding-flip-recovery to capture
               the asymmetric edge (long sleeve negative, short sleeve
               positive) the symmetric parent demonstrates is structural.

Mechanism:     Single-direction sleeve isolates the regime-aligned half
               of the funding-flip-recovery mechanism. Direction restriction
               (per Part I §9) is the canonical mutation
               when symmetric grid shows asymmetric long_pnl vs short_pnl
               distributions.

Observable:    BTCUSDT perp 4h, 2025-05-03 → 2026-04-28, $1000 capital,
               fee 0.04%. Compare short-only grid medians vs symmetric
               parent.

Expected:      ≥+0.30 Sharpe gap in favor of short-only over symmetric.
               expected_net_edge: clear at minimum borderline-deployable
               threshold ($120/yr at $1K = 12% return).

Falsifier:     Asymmetric edge confirmed if Sharpe gap ≥ +0.30; net edge
               passes if gross/yr after friction ≥ $120/yr at $1K capital.
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp, $1000, fee 0.04%)

| Metric | Value | Threshold | Outcome |
|--------|---:|---|:---:|
| Median Sharpe | **+0.50** | vs symmetric −0.11 (gap +0.61) | Asymmetric-edge ✓ |
| Median ATP | $0.20 | — | Modest |
| %PF>1 | 58.6% | ≥ 50% | ✓ |
| %val+ | 80.9% | ≥ 80% | ✓ |
| Eligible runs | 2052/5184 (40%) | ≥ 10% | ✓ (no selection bias) |
| Median trades | ~50 | — | High frequency |

**Asymmetric edge confirmed (+0.61 Sharpe gap vs symmetric).** Direction restriction operator validated.

## Net-of-Friction (per [[foundation/methodology/deployment-edge-threshold|deployment-edge methodology]])

| Component | Value |
|---|---:|
| Gross/yr ($1K capital, ~50 trades × $0.20 ATP) | **+$10-11** |
| Friction/yr (1bp roundtrip × $5K position × 50 trades) | **$50** |
| **Net/yr** | **−$39** |

Even at best-case 0.5bp roundtrip slippage: gross $10 − friction $25 = **−$15/yr**. Still negative.

Compare: $120/yr deployment-grade threshold (12% on $1K). This strategy at face value doesn't hit even 1/10th of threshold (gross only $10-11; net negative).

## Falsifier verdict: ASYMMETRY CONFIRMED; DEPLOYMENT-GRADE FAILS

- **Statistical-grade asymmetric falsifier passed**: +0.61 Sharpe gap vs symmetric parent
- **Deployment-grade gate failed**: Net −$39/yr; far below threshold
- **Mutation pattern validated**: direction-restriction operator works as a discipline; this individual strategy doesn't reach deployment-grade

The result demonstrates that **asymmetric edge can be statistically real and economically negligible at the same time**. The 50-trade-per-year frequency × tiny per-trade edge × multiplicative friction structurally caps the deployment readiness.

## Why This Strategy Specifically Fails the Net-Edge Math

This is the **frequent-but-tiny failure mode** documented in [[trading/backtest-results/direction-restriction-short-only-2026-05|the joint entry]]:

> **Sparse-but-meaty beats frequent-but-tiny for deployment**

| Strategy | Trades/yr | Gross ATP | Gross/yr | Friction/yr | Net |
|----------|---:|---:|---:|---:|---:|
| `bollinger_squeeze_4h_short_only` | ~16 | $1.50 | $24 | $16 | **+$8** |
| `funding_flip_recovery_4h_short_only` (this) | ~50 | $0.20 | $11 | $50 | **−$39** |

The bollinger sparse-but-meaty version (16 trades, $1.50 ATP) has friction-per-year that doesn't dominate gross. The funding-flip frequent-but-tiny version (50 trades, $0.20 ATP) has friction structurally exceeding gross. Per [[foundation/methodology/deployment-edge-threshold|methodology §8]] mechanism implications: **lower turnover OR larger absolute edge per trade favors deployment-grade.**

## Partial Truth Retained

- **Direction-restriction mutation pattern validated**: this is N=2 of N=3 supporting cases for Part I §9 direction-restriction operator. Pattern intact; per-strategy deployment readiness varies.
- **Asymmetric edge is regime-conditional**: 2025-26 bearish regime favors short side; bullish regime would invert. The discipline is *respect observed asymmetry*, not *always shorts*.
- **Mechanism (funding-flip-recovery) is real but small**: a funding-carry strategy (different mechanism in same family — funding-rate carry, not funding-flip-recovery) is the canonical productionize member of [[trading/mechanisms/funding-cycle-dynamics|funding-cycle-dynamics]]. Funding-flip-recovery is open research direction.

## What Would Need to Change for Deployment-Grade

1. **Reduce trade frequency by 5-10×** — concentrate on highest-conviction setups (multi-confluence requirement: funding flip + on-chain whale flow + Coinglass liquidation cluster + macro regime confirmation). Reduces friction-per-year proportionally.
2. **Increase per-trade edge magnitude by 5-10×** — would require a different mechanism class entirely (event-driven, multi-day holds). Direct evolution of this strategy can't get there.
3. **Combine with other mutations** — stacking with HTF + vol filter MAY clear threshold; needs separate hypothesis testing per Marcos' Second Law.

None of these is a parameter tweak. Per Marcos' Second Law (no research under influence of backtest), redesign is a *new* strategy with its own pre-registration.

## Trial Count on Dataset

≥13 strategies tested in this batch. Strategy-level falsifier passed (asymmetric edge confirmed); deployment-grade gate fails. Trial count is robust at the *mutation-pattern* level (N=2 supporting cases is intact); deployment-grade-fail verdict is independent of trial multiplicity.

## Reason Classification

- [ ] Strategy-level falsifier fired (asymmetric edge confirmed; mutation pattern validated)
- [x] **Deployment-grade gate fired** — net edge below threshold (−$39/yr at $1K)
- [x] **Frequent-but-tiny failure mode** — friction structurally exceeds gross
- [x] **Mechanism real but operating at small magnitude** — partial-truth retained; deployment readiness specifically rejected
- [ ] No falsifier articulated
- [ ] Mechanism absent

## Key Takeaways

- **Asymmetric edge confirmed but small**: +0.61 Sharpe gap vs symmetric (statistical-grade pass) but only $10-11 gross/yr at $1K — friction-eats-the-edge case.
- **Mutation pattern (direction restriction) validated**: this is N=2 of N=3 supporting cases for Part I §9 discipline. The pattern stands; the specific strategy's deployment readiness doesn't.
- **Frequent-but-tiny failure mode**: 50 trades × $0.20 ATP × $1 friction roundtrip = friction structurally exceeds gross. Lesson: lower turnover OR larger per-trade edge for deployment-grade.
- **Compare to bollinger_squeeze_4h_short_only**: same direction-restriction mutation, same regime concentration, very different deployment readiness (16 trades × $1.50 ATP = sparse-but-meaty borderline-passing). Strategy-design parameter (turnover) drives deployment readiness more than mechanism.
- **Mechanism family standing**: a funding-carry strategy carry mechanism is canonical productionize member of funding-cycle-dynamics; funding-flip-recovery (this mechanism) is open research with different framing.

## Related

- [[trading/rejected/funding-flip-recovery-4h-symmetric-2026-05]] — symmetric parent (rejected; asymmetry confirmed)
- [[trading/rejected/funding-flip-recovery-regime-gate-2026-05]] — regime-gate-stacking variant (rejected; cross-strategy worldview update)
- [[trading/backtest-results/direction-restriction-short-only-2026-05]] — joint entry; mutation pattern validation; bollinger short_only research-grade-only companion
- [[trading/mechanisms/funding-cycle-dynamics]] — family page; a funding-carry strategy canonical productionize member
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — design pattern
- Part I §9 — Mutation Operators (direction-restriction)
- [[foundation/methodology/deployment-edge-threshold]] — net-edge gate; §8 mechanism implications (frequent-but-tiny failure mode)
- [[foundation/macro/regime-conditional-edge-2025-26]] — bearish-regime concentration

## Sources

- the backtesting lab — strategy files
- the backtesting lab — symmetric parent
- the backtesting lab — full reasoning
