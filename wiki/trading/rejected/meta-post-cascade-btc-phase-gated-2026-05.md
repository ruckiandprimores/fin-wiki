---
title: "meta_post_cascade_btc_prod_v1_phase_gated — rejected by bear-regime falsification"
status: evergreen
created: 2026-05-18
updated: 2026-05-19
tags: [rejected, post-cascade, mean-reversion, phase-gated, regime-conditioned, bear-regime, cross-regime-falsifier, bull-only-mechanism, regime-gate-vs-bias-filter-evidence]
---

# meta_post_cascade_btc_prod_v1_phase_gated — rejected by bear-regime falsification

> **Lead learning**: The phase-gated meta_post_cascade strategy passes bull-regime validation strongly (N=36, PSR>0 = 95.8%, walk-forward 4/4 folds positive — *the strongest walk-forward consistency of any v8 candidate*) but is decisively falsified in bear regime at moderate sample size (N=28, PF 0.82, PSR>0 = 35.7%, Sharpe -0.33 with 95% CI [-3.38, +1.15]). The 60-percentage-point PSR collapse is the largest of the v8 cohort. **Phase-gating worked as a bias filter for the bull regime but didn't carry cross-regime conditioning** — N=2 evidence for the `regime-gate-vs-bias-filter` codification's central distinction (paired with funding_extreme_long_short_btc bear rejection same session). **Verdict: REJECT for cross-regime deployment.** Bull-regime-only deployment is preserved as a separate question; this rejection is about cross-regime structural transferability.

## What we learned

1. **Walk-forward 4/4 within a regime does not equal cross-regime robustness.** The strategy had the strongest bull-regime walk-forward of any v8 member — 4/4 folds positive, median PSR 67%. The bear-regime sample (N=28) is COMPARABLE to bull-regime fold sizes — so this isn't a small-sample artifact: the mechanism legitimately works across all bull-regime sub-periods AND legitimately fails across bear-regime sub-periods at adequate-but-not-overwhelming sample. **Walk-forward measures temporal consistency; cross-regime measures structural transferability. Both are needed.**

2. **Phase-gating as bias filter ≠ regime-gate as conditioning attribute.** Per [[../mechanisms/regime-gate-vs-bias-filter|regime-gate-vs-bias-filter]] codification: a regime gate that filters trades to clean signal periods (bias filter) does not necessarily encode regime conditioning. Stable phases exist in bear regimes too — but the post-cascade-mean-reversion mechanism *within those stable phases* doesn't transfer. The `is_stable` filter was specifying a sub-population, not a regime.

3. **Post-cascade mean-reversion is regime-dependent in crypto.** In bull regimes, the post-cascade mechanism captures institutional "buy the dip" flow recovering after forced-liquidation cascades. In bear regimes, post-cascade behavior is dominated by *continuation* — the cascade is part of a larger trend-down, and rebounds are short-lived. The mechanism inverts. **Consistent with [[../mechanisms/liquidation-cascade-aftermath|liquidation-cascade-aftermath]] — the recovery dynamic varies by regime context.**

4. **The most-surprising falsifier teaches the most.** This wasn't the weakest-by-PSR candidate (that was funding_extreme). This was Tier B by all bull-regime metrics. **Surprise from cross-regime evidence > surprise from PSR/walk-forward** for the validation framework's load-bearing weight. Walk-forward within-regime can produce false-confident verdicts.

## Hypothesis (Structured Format)

```
Hypothesis:    After liquidation cascades, price mean-reverts within
               2-6 bars during stable consolidation phases. Phase-
               gating to is_stable filters out trending-regime
               cascades where mean-reversion doesn't apply.

Mechanism:     Post-cascade mean-reversion (liquidation-cascade-aftermath
               family) gated by stable-phase data layer.

Observable:    BTCUSDT 4h, 2024-05-20 → 2026-05-08 (bull) +
               2022-04-01 → 2023-09-30 (bear). 4 bps fee, $1000 capital.

Expected:      Bull: edge persists per bull-regime walk-forward 4/4 +
               PSR>0 95.8%. Bear: edge persists IF the mean-reversion
               mechanism is genuinely regime-conditional via the phase
               gate. UNRESOLVED at strategy design time.

Falsifier:     Bear-regime PSR(>0) < 0.70 OR PF < 1.0 at adequate N.
```

## Bull-regime evidence (passes)

| Metric | Point | 95% CI | Reading |
|---|---|---|---|
| N trades | 36 | — | Adequate sample |
| Sharpe | 1.53 | (per PORTFOLIO_v8 walk-forward table) | Strong |
| PSR(>0) | 95.8% | — | Near-deployment-grade threshold |
| PSR(>1) | 58.0% | — | Borderline at the 70% deployment cutoff |
| Walk-forward folds | 4/4 positive | — | **Strongest in v8** |
| Walk-forward median PSR | 67% | — | Best of v8 |
| Long P&L | +$66 | — | Both directions profitable |
| Short P&L | +$129 | — | Both directions profitable |

## Bear-regime evidence (fails — with CI honesty)

| Metric | Point | 95% CI | Reading |
|---|---|---|---|
| N trades | 28 | — | Moderate — adequate per LdP framework |
| Sharpe | -0.33 | [-3.38, +1.15] | **CI upper bound positive — not slam-dunk by Sharpe alone** |
| PSR(>0) | 35.7% | — | Below 50% — actively skew-negative |
| PSR(>1) | 7.4% | — | Effectively zero |
| WR | 17.9% | [3.6%, 32.1%] | **Severe — CI upper bound still below 33%** |
| PF | 0.82 | [0.17, 1.93] | CI upper bound > 1 — possible noise-positive scenario in tail |
| MDD | 7.27% | [4.55%, 20.52%] | Larger MDD than bull regime |
| TP fires | 5 / 28 | — | Reduced from bull |
| SL fires | 23 / 28 | — | Geometry broken in bear |
| Long P&L | -$11 (WR 17%) | — | Both directions fail |
| Short P&L | -$24 (WR 19%) | — | Both directions fail |

**CI honesty caveat**: The bear-regime Sharpe 95% CI [-3.38, +1.15] crosses zero with positive upper bound. The PF 95% CI [0.17, 1.93] also crosses 1.0. These say there's a non-trivial chance the bear-regime point estimate is too negative — sample is moderate (N=28), bootstrap is honest. **But the WR collapse 35.7% → 17.9% with CI upper bound 32.1% (still well below bull's 36%) is the load-bearing signal — robust across the CI range.** PF and Sharpe CIs say "could be noise"; WR CI says "can't be noise — distribution structure changed."

## Falsifier verdict (CI-honest framing)

**High-confidence falsifier at moderate sample size** — not "decisive falsifier without caveat."

- WR collapse is robust across CI: 17.9% point estimate; CI upper bound 32.1% is still below the bull-regime 36% point estimate.
- PSR collapse is robust: 95.8% → 35.7% point-to-point. The full 95% CI on bear-regime PSR is wide but heavily weighted below 50%.
- Sharpe CI alone is ambiguous (-3.38 to +1.15), but PSR + WR + PF + exit-reason geometry (5 TP / 23 SL in bear vs presumably-different bull mix) all converge on the same direction.
- **Triangulating across metrics**: cross-regime PSR collapse + WR collapse + exit-geometry shift = high-confidence falsifier at N=28. NOT slam-dunk; NOT noise either.

**Verdict**: Reject for cross-regime deployment. Bull-regime-only deployment is a separate question; this rejection is about cross-regime structural transferability.

## What this rejects

- The strategy AS-IS — not deployment-grade outside bull-regime cycles
- The implicit assumption that phase-gating encodes regime conditioning
- The intuition that walk-forward 4/4 folds within a regime guarantees cross-regime edge
- Treating `_phase_gated` suffix as adequate regime-conditioning across cycles

## What this DOESN'T reject

- The **post-cascade mean-reversion mechanism class** broadly. Bull-regime-only deployments or strategies with EXPLICIT regime conditioning (BTC trend direction, ETF flow sign, regime-detector output) could still have edge — this rejection is about cross-regime structural transferability, not the mechanism's bull-regime validity.
- The **bull-regime evidence**. The strategy genuinely works in bull regimes — this is not a curve-fit point estimate but a real, walk-forward-consistent edge that happens to be regime-specific. **A bull-only deployment with explicit bull-regime gate is a separate proposal worth evaluating, not auto-killed by this rejection.**
- The **phase data layer as infrastructure**. The layer worked; what failed was the implicit assumption that phase-gating = regime conditioning.

## Methodology notes

### Sample-size caveats

- **N=28 is moderate, not overwhelming.** Per LdP small-sample validation framework, N=28 is adequate (rule-of-thumb floor ~30; closer to threshold than to abundant). Bootstrap CIs computed at 10,000 iterations.
- **Sharpe CI alone is ambiguous**: [-3.38, +1.15] crosses zero. A standalone Sharpe-based falsifier on this sample would NOT fire conclusively. The verdict requires triangulation across metrics.
- **WR CI is the load-bearing signal**: [3.6%, 32.1%] — upper bound 32.1% is STILL below bull-regime WR 36%. The CI honesty doesn't rescue the bear-regime sample; it sharpens what we can conclude.
- **PF CI [0.17, 1.93]**: upper bound > 1 is informative but not exonerating. The mass of the bootstrap distribution is below 1.

### Cross-regime evidence stack

- **N=2 evidence for `regime-gate-vs-bias-filter` codification.** Paired with `funding_extreme_long_short_btc` bear-regime rejection (same 2026-05-18 session — separate task entry pending). Both v8 candidates that failed bear regime did so via "bias filter" not "regime conditioning attribute." Codification now has empirical anchor across two strategies.

- **Bull-regime walk-forward 4/4 didn't predict bear failure.** This argues for the cross-regime axis as a mandatory validation dimension, not optional. Per [[../../foundation/methodology/strategy-validation-discipline]] — cross-regime PSR is now part of the validation stack.

- **The strategy was almost selected as a Tier B workhorse** by the 2026-05-15 ratings. Bear-regime validation would have caught this before any live capital was committed. **Cross-regime validation pays for itself** even at modest engineering cost.

### The "decisive vs high-confidence" framing distinction

Initial 2026-05-18 framing read as "decisive falsifier without caveat." Revised 2026-05-19 to "high-confidence falsifier at moderate N." The revision matters because:

- "Decisive" overstates Sharpe-CI confidence (the CI crosses zero with positive upper bound)
- "High-confidence" correctly attributes the conviction to the triangulation across PSR + WR + exit-reason geometry, not to any single metric
- Future readers can evaluate whether THEIR cross-regime evidence meets the same triangulation bar

The verdict doesn't change. The framing is honest about what the moderate sample can support.

## Related artifacts

- Strategy file: the backtesting lab (moved to tested 2026-05-18)
- Bear-regime backtest: the backtesting lab
- Bull-regime evidence: in the backtesting lab v8 member entry

## Related

- [[../mechanisms/regime-gate-vs-bias-filter]] — N=2 codification evidence anchor
- [[../mechanisms/liquidation-cascade-aftermath]] — parent mechanism page; post-cascade is regime-conditional
- [[../../foundation/methodology/strategy-validation-discipline]] — cross-regime axis as mandatory validation dimension
- [[../../foundation/methodology/small-sample-validation-discipline]] — CI honesty discipline applied here

## Sources

- 2026-05-18 research process session: bull-regime walk-forward + bear-regime backtest evidence pulled per the research arms §14 data-pull discipline
- Bear-regime backtest: the backtesting lab (Sharpe -0.325, PSR>0 35.7%, all CIs as reported above)
- Bull-regime walk-forward: PORTFOLIO_v8.md v8 member table; `runs/.../psr2/summary.json`
- Original task entry: `tasks/the research process` (superseded by this CI-honest version)
