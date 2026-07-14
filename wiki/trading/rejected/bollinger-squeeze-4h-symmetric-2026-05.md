---
title: "bollinger_squeeze_4h symmetric parent — rejected; 2026-05"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [rejected, vol-cycle, momentum, mutation-needed, mechanism-real-but-edge-below-threshold, symmetric-parent, day-swing]
---

# bollinger_squeeze_4h symmetric parent — rejected; 2026-05

> **TL;DR**: Symmetric vol-cycle compression-expansion (both directions tradeable) **fails** in 2025-26 BTC perp 4h regime: medSh −1.01; %PF>1 31.2%; net −$58/yr. Long side fails badly (medL$ −$36); short side breakeven (medS$ +$1). **Asymmetry exists** — captured by the `_short_only` mutation (research-grade-only +$8/yr; below deployment threshold) and by the `_short_regime` mutation (inconclusive due to selection bias). **Mechanism is real**; symmetric framing is the bug.

## Hypothesis (Structured Form)

```
Hypothesis:    Bollinger band squeeze (vol compression) followed by
               expansion is tradeable in either direction — the
               compression-expansion cycle dominates direction; bias
               is provided by entry signal not by external filter.

Mechanism:     Vol-cycle (Murphy ch 9 + standard TA literature):
               compression precedes expansion; squeeze breakout in either
               direction has higher probability than at random entry.
               Symmetric framing assumes both directions are tradeable
               by the same signal logic.

Observable:    BTCUSDT perp 4h, 2025-05-03 → 2026-04-28, $1000 capital,
               fee 0.04%, 13122 parameter combinations. Pre-registered
               falsifier: medSh ≥ 0; %PF>1 ≥ 50%.

Expected:      Sharpe +0.30 to +0.60 in either direction.
               expected_net_edge: gross ≥ $3/trade after fees.

Falsifier:     If strategy-level metrics fail OR if directional
               asymmetry is structural (one side consistently
               negative), the symmetric framing is the bug.
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp, $1000, fee 0.04%)

| Metric | Value | Falsifier criterion | Fired? |
|--------|---:|---|:---:|
| Median Sharpe | **−1.01** | ≥ 0 | **YES** |
| %PF>1 | **31.2%** | ≥ 50% | **YES** |
| Median long_pnl | **−$35.6** | — | (asymmetry confirmed) |
| Median short_pnl | **+$1.3** | — | (asymmetry confirmed) |

## Net-of-Friction (per [[foundation/methodology/deployment-edge-threshold|deployment-edge methodology]])

| Component | Value |
|---|---:|
| Gross/yr ($1K capital) | weakly negative |
| Friction/yr | ~$30-40 |
| **Net/yr** | **−$58** |

Below deployment-grade threshold; symmetric framing structurally diluting.

## Falsifier verdict: FIRED — symmetric framing is the bug

The directional asymmetry (long sleeve −$36 vs short sleeve +$1) is **structural**, not noise. The symmetric framing combines the failing long side with the breakeven short side; the result is net negative. Per Part I §9 direction-restriction discipline (sharpened 2026-05-05), this is the canonical asymmetric-trigger pattern.

## Partial Truth Retained — mechanism real, mutation needed

| Mutation | Verdict | Net/yr | Status |
|----------|---------|---:|--------|
| `bollinger_squeeze_4h_short_only` (drop entry_long) | research-grade-only | **+$8** | Closest-to-passing in inventory; below $120/yr deployment threshold; see [[trading/backtest-results/direction-restriction-short-only-2026-05]] |
| `bollinger_squeeze_4h_short_regime` (regime gate stack) | **INCONCLUSIVE** — selection bias | apparent +$102 | Only 63 of 10,368 grid runs eligible (0.6%); all share `squeeze_window=3 AND squeeze_pct=0.25`; see [[trading/rejected/bollinger-squeeze-4h-short-regime-2026-05]] |

**Mechanism story is real**: vol-cycle compression-expansion exists; the short-side capture in 2025-26 bearish regime is consistent with [[foundation/macro/regime-conditional-edge-2025-26|cohort regime concentration]]. The symmetric framing is what fails.

## Trial Count on Dataset

≥13 strategies tested in this batch. Falsifier fired with margin (medSh −1.01, well below 0 threshold) — robust to multiplicity correction.

## Reason Classification

- [x] **Falsifier fired** — symmetric framing produces strategy-level negative
- [x] **Asymmetric edge confirmed** — short side ≠ long side; symmetric framing dilutes
- [x] **Direction-restriction mutation discipline applies** — per Part I §9
- [ ] No falsifier articulated
- [ ] Mechanism absent
- [ ] Already arbitraged

## Key Takeaways

- **Symmetric framing is the bug**; mechanism is real. Direction restriction extracts the edge but doesn't reach deployment-grade alone.
- **Mutation discipline applies**: per Part I §9 direction-restriction operator (sharpened 2026-05-05 with this strategy as N=2 evidence).
- **Below deployment threshold even after mutation**: short_only +$8/yr is positive but small. Stacking (HTF + vol filter + regime gating) might clear threshold; investigate per Task 5 follow-up.
- **Asymmetry is regime-conditional**, not mechanism-permanent. 2025-26 was bearish-dominant; bullish-dominant year would invert the asymmetry direction.

## Related

- [[trading/backtest-results/direction-restriction-short-only-2026-05]] — joint entry; bollinger short_only research-grade-only
- [[trading/rejected/bollinger-squeeze-4h-short-regime-2026-05]] — INCONCLUSIVE companion; regime-gate selection-bias diagnostic
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — design pattern; embedded bias filter > separate regime gate
- [[trading/mechanisms/moving-averages]] — Bollinger lives in MA family; vol-cycle context
- Part I §9 — Mutation Operators (direction-restriction)
- [[foundation/methodology/deployment-edge-threshold]] — net-edge gate
- [[foundation/macro/regime-conditional-edge-2025-26]] — bearish-regime concentration

## Sources

- the backtesting lab — symmetric parent
- the backtesting lab — direction-restriction mutation (research-grade-only)
- the backtesting lab — regime-gate stacking (inconclusive)
- the backtesting lab — full reasoning
