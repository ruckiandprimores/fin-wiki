---
title: "ema_bias_breakout_4h_v2 — REJECTED (verdict reversal under deployment-grade); 2026-05"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [rejected, verdict-reversal, trend-following, breakout, mechanism-validated, deployment-grade-failure, methodology-worked-case, day-swing]
---

# ema_bias_breakout_4h_v2 — REJECTED (verdict reversal under deployment-grade); 2026-05

> **TL;DR (verdict reversal)**: Originally classified as **productionize candidate** in the 2026-05-05 PM batch ingest (statistical-grade pass: Sharpe +0.89; %PF>1 82.8%; %val+ 97.2% — all 3 pre-registered thresholds passed). **The 2026-05-05 PM-3 deployment-grade stocktake reverses this verdict** under the sharpened arm-14 / worldview-prior-#2 / Part I §4 expected_net_edge discipline (added later 2026-05-05). Net = gross +$38 − friction $62 = **−$24/yr** at $1K capital with $5K position. Below deployment-grade threshold. **Pre-registered falsifier was statistical-significance only**; deployment-grade falsifier (now mandatory) would not have passed. **Worked case for why the discipline sharpening was necessary.** Replaces previous backtest-results entry; mechanism validation persists; productionize verdict revoked.

## Original Verdict (2026-05-05 PM batch — superseded)

The 2026-05-05 PM research process batch landed v2 as a productionize candidate at `wiki/trading/backtest-results/ema-bias-breakout-4h-v2-2026-05.md`. That entry recorded:

| Metric | Value | Pre-registered threshold | Passed? |
|--------|---:|---|:---:|
| Median Sharpe | +0.89 | ≥ +0.80 | ✓ |
| %PF>1 | 82.8% | ≥ 75% | ✓ |
| %val+ | 97.2% | ≥ 85% | ✓ |
| Median ATP | $0.60 | — | improved 2× over parent |

All three pre-registered thresholds passed by margin. Statistical-grade verdict: **PASS**. **Productionize-candidate framing was applied at time of original entry.**

## Verdict Reversal — Deployment-Grade Stocktake (2026-05-05 PM-3)

The 2026-05-05 PM-2 ingest added [[foundation/methodology/deployment-edge-threshold|deployment-edge threshold methodology]] and the 2026-05-05 PM-3 stocktake applied that methodology to existing inventory. Result for v2:

| Component | Value |
|---|---:|
| Gross/yr ($1K capital, ~62 trades) | +$38 |
| Friction/yr (1bp roundtrip × $5K position × 62 trades) | $62 |
| **Net/yr** | **−$24** |

Even at best-case slippage (0.5bp roundtrip), Net is +$6/yr — economically negligible. **Below $120/yr (12%) deployment-grade threshold.**

**Verdict revised**: REJECTED — deployment-grade gate fails. Productionize-candidate framing revoked.

## Why the Reversal Happened

The pre-registered falsifier was **statistical-significance only**. It checked:
- Median Sharpe ≥ +0.80 ✓
- %PF>1 ≥ 75% ✓
- %val+ ≥ 85% ✓

It did NOT check:
- Net-of-friction edge per trade
- Net-of-friction edge per year at deployment scale
- Trade frequency × per-trade friction = total friction-per-year

**The deployment-grade falsifier was missing.** Adding it retrospectively (per Part I §4 sharpening 2026-05-05 PM-2; [[foundation/methodology/deployment-edge-threshold|deployment-edge methodology]] 2026-05-05 PM-2) revealed that net-of-friction edge is below threshold.

This is the **canonical worked case** for why the discipline sharpening was necessary:
- Without `expected_net_edge` in the upstream hypothesis: strategy passes pre-registered thresholds → marked productionize candidate → deployment-grade gap discovered later.
- With `expected_net_edge` in the upstream hypothesis: strategy author would have computed expected net edge at design time; would have noted "62 trades × $1 friction roundtrip vs $0.60 gross ATP → net loses to friction"; would have either redesigned (lower turnover) or pre-registered as research-grade-only.

## What This Verdict Reversal Documents (Methodologically)

- **Statistical significance ≠ economic significance.** This strategy demonstrated this concretely.
- **Pre-registered falsifiers must include net-of-friction floor**, not just statistical Sharpe / win-rate / val+ thresholds.
- **The discipline gap was real** — and now closed via the Part I §4 sharpening + [[foundation/methodology/deployment-edge-threshold]] methodology page.
- **Verdict reversals are honest audit signal.** The wiki preserves both the original "productionize candidate" framing (in the original entry's audit trail) and the corrected "rejected" framing here. Original backtest-results entry deleted; this entry supersedes.

## Mechanism Validation Persists

The mechanism (embedded bias filter aligning every entry with EMA(20)/EMA(50) directional bias) is **statistically validated**:
- Pre-registered statistical thresholds passed
- Companion entry [[trading/rejected/ema-bias-breakout-4h-symmetric-2026-05|symmetric parent]] also has paired falsifier (counter-bias mirror) passing with +1.37 Sharpe gap
- This strategy is one of N=4 supporting cases in [[trading/mechanisms/regime-gate-vs-bias-filter|the bias-filter > separate regime-gate design pattern]]

**The mechanism finding stands**; the deployment-readiness of *this specific parameterization* does not.

## Partial Truth — Direction-Aligned Improvement Captured

v2's signature finding was that short_pnl flipped from −$13.90 (parent) to +$2.90 (v2), while long_pnl was preserved. **The embedded bias filter captured directional asymmetry without requiring direction restriction.** This evidence supports:

- Part I §9 direction-restriction discipline (sharpened 2026-05-05) — N=3 supporting cases including v2's direction-aligned improvement
- [[trading/mechanisms/regime-gate-vs-bias-filter|design pattern]] — embedded bias filter > separate regime gate

These mechanism-level findings persist regardless of v2's deployment-grade rejection.

## What Would Need to Change for Deployment-Grade

Per the original entry's "What would need to change" section (still applicable):

1. **Reduce trade frequency** — longer holds, stricter entry filter, larger ATR thresholds. Friction-per-year scales with trade count.
2. **Increase per-trade edge magnitude** by 5–10× — requires a mechanism with larger absolute edge per setup (event-driven, multi-day holds) or different asset.
3. **Reduce per-trade friction** — maker-side execution (limit orders posting liquidity, capturing rebate); reduces net friction but adds fill-rate uncertainty.

None of these are simple parameter tweaks; each is a strategy redesign. Per Marcos' Second Law (no research under influence of backtest), this redesign should be a **new** strategy with its own pre-registration including upstream `expected_net_edge` — not a v3 of this one.

## Trial Count on Dataset

≥13 strategies tested on BTCUSDT perp 2025-05/2026-04 window in this batch. Per [[glossary/deflated-sharpe-ratio|DSR adjustment]]: trial multiplicity penalizes apparent statistical edge. v2's pre-registration didn't account for trial count; deployment-grade rejection is independent of multiplicity correction (it's a friction calc, not a statistical test).

## Reason Classification

- [ ] Strategy-level falsifier fired (statistical-grade passed)
- [x] **Deployment-grade gate fired** — net edge below threshold
- [x] **Mechanism validated** — paired falsifier passed; one of N=4 supporting cases in regime-gate-vs-bias-filter
- [x] **VERDICT REVERSAL** — was originally productionize candidate; deployment-grade methodology applied retrospectively
- [x] **Worked case for methodology sharpening** — demonstrates why `expected_net_edge` upstream-of-backtest discipline was necessary
- [ ] No falsifier articulated (it was articulated, just statistical-only)

## Key Takeaways

- **Verdict reversed**: was productionize candidate (statistical-grade pass) → now rejected (deployment-grade fail). Both verdicts are correct *under their respective gates*; both gates are required.
- **Statistical significance ≠ economic significance.** v2 is the canonical worked case.
- **Mechanism finding persists**: embedded bias filter captures direction asymmetry; bias-filter > separate regime gate (N=4); direction-restriction discipline (N=3).
- **Deployment-grade requires friction-aware design from start.** Retrofitting deployment-grade to a strategy designed for statistical-grade rarely works; redesign is the answer.
- **Methodology sharpening was load-bearing**: had `expected_net_edge` been mandatory upstream (per Part I §4 sharpening), v2 would have been flagged research-grade-only at hypothesis stage; productionize-candidate misclassification would have been prevented.

## Audit Trail

- **2026-05-05 PM** — original entry landed at `wiki/trading/backtest-results/ema-bias-breakout-4h-v2-2026-05.md` as productionize candidate
- **2026-05-05 PM-2** — deployment-edge methodology added; original entry recalibrated with deployment-grade-fail section, downgraded to "research-grade only"
- **2026-05-05 PM-3** — deployment-grade stocktake (Task 4) reverses verdict to REJECTED; this entry supersedes original; original `backtest-results/` file deleted

The verdict trail itself is wiki content — see [[log]] entries 2026-05-05 PM, PM-2, PM-3.

## Related

- [[trading/rejected/ema-bias-breakout-4h-symmetric-2026-05]] — symmetric parent (statistical-grade passed; deployment-grade failed; mechanism validated)
- [[trading/rejected/htf-aligned-breakout-4h-2026-05]] — separate-HTF-gate variant; failed at strategy level
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — design pattern; this strategy is one of N=4 supporting cases
- [[trading/mechanisms/moving-averages]] — canonical bias-filter mechanism page
- [[foundation/market-wisdom/bias-filter-vs-entry-signal]] — taxonomy
- [[foundation/methodology/deployment-edge-threshold]] — methodology that recalibrated this verdict
- [[foundation/methodology/strategy-validation-discipline]] — paired-falsifier discipline (mechanism validation)
- Part I §4 — Structured Hypothesis Format (sharpened 2026-05-05 to require expected_net_edge upstream of backtest)
- Part I §9 — Mutation Operators (direction-restriction discipline; v2 supports as N=3 case)

## Sources

- the backtesting lab — strategy files + pre-registration (statistical-only)
- the backtesting lab — parent
- the backtesting lab — counter-bias mirror (paired falsifier; archived)
- the backtesting lab — heat-map source for v2 parameter range refinement
- the backtesting lab — companion stocktake report; full reasoning for verdict reversal
