---
title: "funding_flip_recovery_4h symmetric parent — rejected (asymmetry confirmed); 2026-05"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [rejected, forced-flow, funding-cycle, mechanism-real, mutation-needed, symmetric-parent, day-swing]
---

# funding_flip_recovery_4h symmetric parent — rejected (asymmetry confirmed); 2026-05

> **TL;DR**: Wyckoff Spring/Upthrust via funding rate signature, **symmetric framing** (both long-side and short-side recovery). Result is essentially flat (medSh −0.11; net −$66/yr) but asymmetry is **structural**: medL$ −$27, medS$ +$25. The mechanism is real but operates only on the short side in 2025-26 BTC bearish-dominant regime. Direction restriction (`_short_only` mutation) extracts the asymmetric edge but doesn't reach deployment-grade either (rejected separately at [[trading/rejected/funding-flip-recovery-4h-short-only-2026-05|the short_only entry]]). Symmetric parent serves as the falsifier-instrument that confirmed the asymmetry — its purpose (validating the asymmetry) is functionally complete; the symmetric framing itself is the bug.

## Hypothesis (Structured Form)

```
Hypothesis:    Wyckoff Spring/Upthrust via funding-rate signature:
               positive→negative funding flip (long-side positioning
               blowoff → cascade → reclaim) is symmetric; reverse pattern
               (negative→positive flip) is also tradeable as short
               cascade recovery.

Mechanism:     Forced-positioning unwind after funding regime flip
               (per [[trading/mechanisms/funding-cycle-dynamics|funding-cycle-dynamics]]
               family). Either direction's positioning extreme should
               be tradeable on funding-flip event.

Observable:    BTCUSDT perp 4h, 2025-05-03 → 2026-04-28, $1000 capital,
               fee 0.04%, 5184 parameter combinations.

Expected:      medSh +0.30 to +0.60; symmetric long_pnl ≈ short_pnl
               (within +/-30%).

Falsifier:     If strategy-level metrics fail OR if directional
               asymmetry is structural (one side consistently negative),
               the symmetric framing is the bug.
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp, $1000, fee 0.04%)

| Metric | Value | Falsifier criterion | Fired? |
|--------|---:|---|:---:|
| Median Sharpe | **−0.11** | ≥ 0 | YES (essentially flat) |
| Median long_pnl | **−$26.9** | — | (asymmetry confirmed) |
| Median short_pnl | **+$24.9** | — | (asymmetry confirmed) |
| %PF>1 | ~50% | — | (parent breakeven) |

## Net-of-Friction (per [[foundation/methodology/deployment-edge-threshold|deployment-edge methodology]])

| Component | Value |
|---|---:|
| Gross/yr ($1K capital) | weakly negative |
| Friction/yr | ~$50 |
| **Net/yr** | **−$66** |

Below deployment-grade threshold. Symmetric framing dilutes the asymmetric edge.

## Falsifier verdict: STRATEGY-LEVEL FIRED; ASYMMETRY CONFIRMED

The directional asymmetry (long sleeve −$27 vs short sleeve +$25) is **structural** and worth more than the symmetric verdict. Per Part I §9 direction-restriction discipline: the asymmetry trigger fires (long_pnl and short_pnl opposite-signed across ≥40% of grid runs). Mutation called for.

## Partial Truth Retained — mechanism real, mutation needed

| Mutation | Verdict | Net/yr | Status |
|----------|---------|---:|--------|
| `funding_flip_recovery_4h_short_only` (drop entry_long) | REJECTED — direction restriction insufficient | **−$39** | See [[trading/rejected/funding-flip-recovery-4h-short-only-2026-05]]; gross +$11, friction $50 |
| `funding_flip_recovery_4h_short_regime` (regime gate stack) | REJECTED — regime gate hurts | apparent +$60-ish; gap −0.16 vs control | See [[trading/rejected/funding-flip-recovery-regime-gate-2026-05]]; cross-strategy worldview update |

**Asymmetric edge is real (+$25 short-side gross median P&L); below deployment threshold even after isolation. Combined with other mechanism stacks may eventually clear threshold; current direct mutations don't.**

## Mechanism Family Standing

The `funding-cycle-dynamics` family is partially validated:
- a funding-carry strategy (live; documented in [[trading/mechanisms/funding-cycle-dynamics]] family page) is the canonical productionize-grade member of this family — operates on funding-rate carry, not funding-flip-recovery; different mechanism
- `funding_flip_recovery_4h_*` mutations all rejected: symmetric parent, direction restriction, regime-gate stacking — none clear deployment-grade alone
- The funding-flip-recovery mechanism itself remains theoretically valid (Wyckoff Spring framework + funding-rate signature); operationalization needs different framing

## Trial Count on Dataset

≥13 strategies tested in this batch. Strategy-level falsifier fired (medSh −0.11 essentially flat); robust to multiplicity correction.

## Reason Classification

- [x] **Falsifier fired** — symmetric framing produces strategy-level breakeven/negative
- [x] **Asymmetric edge confirmed** — short side ≠ long side; symmetric framing dilutes
- [x] **Mutation discipline applies** — direction restriction extracts edge but doesn't reach threshold
- [ ] No falsifier articulated
- [ ] Mechanism absent (mechanism is real)
- [ ] Already arbitraged

## Key Takeaways

- **Symmetric framing is the bug**; mechanism is real on short side in 2025-26 bearish regime.
- **Mutation discipline applies** per Part I §9: this is N=2 (with bollinger_squeeze) supporting the direction-restriction operator.
- **Direction restriction insufficient alone** for deployment-grade: short_only mutation +$11 gross/yr is below $120/yr (12%) threshold even before friction.
- **Asymmetric edge is regime-conditional**: bullish-dominant regime would invert (long-side recovery becomes the asymmetric-edge side). Pattern is *respect observed asymmetry*, not *always shorts*.
- **Family standing partial**: a funding-carry strategy (carry mechanism) is family canonical productionize-grade; funding-flip-recovery (this mechanism) is open research direction needing different framing.

## Related

- [[trading/rejected/funding-flip-recovery-4h-short-only-2026-05]] — direction-restriction mutation (REJECTED)
- [[trading/rejected/funding-flip-recovery-regime-gate-2026-05]] — regime-gate-stacking mutation (REJECTED; cross-strategy worldview update)
- [[trading/backtest-results/direction-restriction-short-only-2026-05]] — joint entry; mutation-pattern finding
- [[trading/mechanisms/funding-cycle-dynamics]] — family page; a funding-carry strategy is canonical productionize-grade member
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — design pattern; one of N=4 supporting cases
- Part I §9 — Mutation Operators (direction-restriction)
- [[foundation/methodology/deployment-edge-threshold]] — net-edge gate
- [[foundation/macro/regime-conditional-edge-2025-26]] — bearish-regime concentration
- [[foundation/sources/wyckoff-method]] — Spring/Upthrust mechanism foundation

## Sources

- the backtesting lab — symmetric parent
- the backtesting lab — direction restriction mutation
- the backtesting lab — regime-gate stack
- the backtesting lab — full reasoning
