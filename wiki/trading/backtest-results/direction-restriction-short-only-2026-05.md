---
title: "Direction-restriction short-only controls — mutation pattern validated; bollinger research-grade-only; funding-flip REJECTED (2026-05)"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [backtest-results, mutation-pattern-validated, research-grade-only, direction-restriction, short-only, controls, behavioral-positioning-unwind, funding-cycle, vol-cycle, day-swing, split-verdict]
---

# Direction-restriction short-only controls — mutation pattern validated; bollinger research-grade-only; funding-flip REJECTED (2026-05)

> **TL;DR (revised 2026-05-05 PM-3 — verdict split):** Two short-only controls — `bollinger_squeeze_4h_short_only` and `funding_flip_recovery_4h_short_only` — were created Apr 28 as falsifier-instruments for regime-gated mutations. **Statistical-significance finding (intact)**: both controls produced large Sharpe gains attributable to **direction restriction alone** (drop `entry_long`, no other changes vs parent): **+2.03** for bollinger, **+0.61** for funding-flip. N=2 supporting cases for the direction-restriction mutation discipline. **The mutation-pattern finding stands** (per Part I §9 direction-restriction operator).
>
> **Per-strategy verdicts now SPLIT (per 2026-05-05 PM-3 deployment-grade stocktake):**
> - **`bollinger_squeeze_4h_short_only`** → **research-grade-only** (Net +$8/yr; closest-to-passing in inventory but below $120/yr deployment threshold). **Stays in this entry as the closest-to-passing case.**
> - **`funding_flip_recovery_4h_short_only`** → **REJECTED** (Net −$39/yr; frequent-but-tiny failure mode). **Moved to its own entry**: [[trading/rejected/funding-flip-recovery-4h-short-only-2026-05]].
>
> Caveat (still applies): 2025-26 was bearish-dominant; same logic in bullish year would favor long-only restrictions. Pattern is *respect observed asymmetry*, not *always shorts*.
>
> **Entry purpose (after split):** primary record of the **direction-restriction mutation pattern validation** (N=2 statistical-grade) + **bollinger short_only research-grade-only verdict** (closest-to-passing). The funding-flip rejection lives at its own page for graveyard-completeness.

## Strategies (joint entry)

| Strategy | Files | Mechanism family |
|----------|-------|------------------|
| `bollinger_squeeze_4h_short_only` | the backtesting lab | [[trading/mechanisms/moving-averages\|Bollinger / vol-cycle]] (compression → expansion); short-only sleeve, no regime gate |
| `funding_flip_recovery_4h_short_only` | the backtesting lab | [[trading/mechanisms/funding-cycle-dynamics]]; short-only sleeve, no regime gate |

3-axis tags: (1) per-strategy mechanism (vol-cycle / funding-cycle); (2) direction asymmetry (forced positioning unwind, regime-conditional); (3) **validated mutation pattern (direction restriction)**.

## Hypothesis (Joint Structured Format)

```
Hypothesis:    For both bollinger_squeeze_4h and funding_flip_recovery_4h,
               the symmetric parent framing dilutes an asymmetric edge.
               Dropping entry_long (single transformation, no other changes)
               produces ≥+0.30 Sharpe gap vs symmetric parent.

Mechanism:     Both parents' grids show structurally asymmetric
               long_pnl vs short_pnl distributions:
                 - bollinger: long_pnl med −$35.6, short_pnl med +$1.3
                 - funding-flip: long_pnl med −$26.9, short_pnl med +$24.9
                   (66% of grid runs short_pnl positive)
               The asymmetry IS the regime feature: 2025-26 BTC was
               bearish-dominant. The mechanism's signals fire on both
               sides; only the short side has favorable regime tailwind.
               Restriction strips the diluting losing side.

Observable:    Backtest grid 2025-05-03 → 2026-04-28, BTCUSDT perp,
               $1000 capital, fee 0.04%. Compare short-only grid medians
               vs symmetric parent grid medians.

Expected:      ≥+0.30 Sharpe gap in favor of short-only on both strategies.

Falsifier:     If either strategy's short-only Sharpe gap is < +0.30
               vs its symmetric parent, the asymmetry was not
               structurally exploitable through pure direction restriction
               (some other mutation would be required).
```

Pre-registered as fallback controls for the regime-gated mutations in the backtesting lab{bollinger_squeeze_4h_short_regime, funding_flip_recovery_4h_short_regime}/` README files.

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp, $1000, fee 0.04%)

| Strategy | Median Sharpe | Parent (symmetric) | Sharpe gap | Threshold | Passed? |
|----------|---:|---:|---:|---|:---:|
| `bollinger_squeeze_4h_short_only` | **+1.02** | −1.01 | **+2.03** | ≥ +0.30 | ✓ |
| `funding_flip_recovery_4h_short_only` | **+0.50** | −0.11 | **+0.61** | ≥ +0.30 | ✓ |

Both gains attributable to **direction restriction alone** — no regime gate, no parameter retuning, no other changes vs parent.

| Strategy | Median ATP | %PF>1 | %val+ | Eligible runs |
|----------|---:|---:|---:|---|
| `bollinger_squeeze_4h_short_only` | $1.50 | 77.1% | 66.8% | 1509/13122 (11.5%) |
| `funding_flip_recovery_4h_short_only` | $0.20 | 58.6% | 80.9% | 2052/5184 (39.6%) |

## Verdict (split per 2026-05-05 PM-3 stocktake): mutation pattern validated; per-strategy verdicts diverge

Both validate **statistically** as direction-restriction wins over symmetric parents. The mutation pattern (drop the failing direction's entry; keep the working side) is real edge. They are **statistical alternatives** to the regime-gated mutations which either:
- Hurt: `funding_flip_recovery_4h_short_regime` Sharpe +0.34 vs this control's +0.50 (see [[trading/rejected/funding-flip-recovery-regime-gate-2026-05]])
- Operate at narrow degenerate parameter range: `bollinger_squeeze_4h_short_regime` Sharpe +2.24 BUT n=63/10368 (0.6%); see [[trading/mechanisms/regime-gate-vs-bias-filter]] for the selection-bias diagnostic

The direction-restriction control is the cleaner mutation **statistically**: same parameter space breadth, single transformation, robust statistical gain.

**Applying [[foundation/methodology/deployment-edge-threshold|deployment-edge threshold methodology]] (PM-2) and 2026-05-05 PM-3 stocktake split:**

| Strategy | Gross ATP | Trades/yr | Gross/yr ($1K) | Friction/yr (1bp ×$5K × N) | **Net** | Verdict | Wiki home |
|----------|---:|---:|---:|---:|---:|---|---|
| `bollinger_squeeze_4h_short_only` | $1.50 | ~16 | $24 | $16 | **+$8** | **research-grade-only** (closest-to-passing) | This entry |
| `funding_flip_recovery_4h_short_only` | $0.20 | ~50 | $10 | $50 | **−$39** | **REJECTED** | [[trading/rejected/funding-flip-recovery-4h-short-only-2026-05]] |

**Reading the table (final verdict):**
- **bollinger short_only is research-grade-only**: +$8/yr is positive but below $120/yr (12%) deployment-grade threshold. **Closest-to-passing in entire 2026-05-05 inventory.** Kept alive in research substrate (this entry); not productionizable at current scale; flagged for Task 5 follow-up (combined with stacking — HTF + vol filter — may clear threshold).
- **funding-flip short_only is REJECTED**: −$39/yr fails deployment-grade gate decisively. Frequent-but-tiny failure mode (50 trades × $0.20 ATP × $1 friction roundtrip = friction structurally exceeds gross). Moved to dedicated rejected/ entry for graveyard completeness.

Why the asymmetry: bollinger has 3× higher trade count for funding-flip but funding-flip's gross ATP is 7.5× lower → friction-per-year dominates funding-flip's gross-per-year. **Sparse-but-meaty (bollinger) deployable; frequent-but-tiny (funding-flip) not deployable.** This is the [[foundation/methodology/deployment-edge-threshold|methodology page §8]] mechanism implication: lower turnover OR larger absolute edge per trade favors deployment-grade.

**The mutation-pattern finding stands.** Direction restriction is a real statistical edge in the 2025-26 bearish-dominant regime; this entry's evidence for the Part I §9 mutation operator is intact. **The deployment-readiness of the specific *strategies* in this entry is what's been recalibrated.**

## Pattern signal (load-bearing across strategies)

Same window, same parent-symmetric → control transition produced **>+0.5 Sharpe gain in both cases**. Combined with `ema_bias_breakout_4h_v2`'s direction-aligned improvement (also direction-aligned via embedded bias filter — short_pnl flipped from −$13.90 to +$2.90), N=3 in this batch supporting the direction-restriction pattern.

This evidence promoted **direction restriction** from implicit to explicit mutation operator in Part I §9 (sharpening landed 2026-05-05).

## Trial count on dataset (per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] §2.2)

≥13 strategies tested on BTCUSDT perp 2025-05/2026-04 window so far. Trial count recorded; per [[glossary/deflated-sharpe-ratio|DSR adjustment]], multiplicity correction is *not* pipeline-implemented but the +2.03 and +0.61 Sharpe gaps are large enough that a moderate DSR adjustment would not flip the verdict from passing. Magnitude of gap is more robust to multiplicity correction than borderline passes.

## Caveats per [[foundation/methodology/strategy-validation-discipline|validation discipline]]

- **Regime conditional, not mechanism permanent.** 2025-26 was bearish-dominant. Same logic in bullish year would have favored long-only restrictions. The discipline is **respect observed asymmetry**, not **always shorts**. Walk-forward / multi-window confirmation is the missing instrumentation per [[foundation/methodology/strategy-validation-discipline]] §11. Stationarity of the asymmetry across regimes is unknown.
- **Out-of-window validation absent.** Heat-map zones may be 2025-26-specific.
- **PSR/DSR not pipeline-implemented.** Multiplicity-corrected confidence not certified; magnitude of Sharpe gap (+2.03 / +0.61) is robust to moderate adjustment but not formally tested.
- **Production sizing not addressed by this entry.** Median ATP $0.20-$1.50 on $1000 capital — modest. This is **edge validation, not capacity research.**
- **Mechanism not invalidated.** The parents' symmetric grids being negative does NOT mean the underlying mechanisms are absent — short side captured the regime-aligned half. Long side may activate in different regime conditions.

## What this entry does NOT certify

- Edge persistence in non-bearish-dominant regimes
- Capacity at productionize allocation
- Multi-instrument generalization (BTCUSDT-only)
- Multiplicity-corrected DSR confidence

## Mutation operator framework update (companion)

This evidence is the load-bearing data for the Part I §9 sharpening landed 2026-05-05 — direction-restriction promoted from implicit (a sub-case of "Inverse test") to explicit mutation operator with named trigger condition (asymmetric long/short P&L distributions).

Trigger condition documented:
- Either side's median P&L is opposite-signed to the other across ≥40% of grid runs, OR
- One side's median P&L is more than 2× the magnitude of the other

The transformation is **high-leverage AND clean** (one-line strategy file change); should be the FIRST mutation tried when asymmetry appears. **Do not stack** with regime gate by default — see [[trading/mechanisms/regime-gate-vs-bias-filter]] for the empirical reason.

## Key Takeaways

- **Direction restriction alone produces +0.61 to +2.03 Sharpe gain (statistical-grade)** on these two strategies — single one-line transformation, no parameter retuning. The **mutation-pattern finding stands**.
- **Per-strategy verdicts split (PM-3 stocktake)**: bollinger short_only **research-grade-only** (+$8/yr — closest-to-passing in inventory; below threshold); funding-flip short_only **REJECTED** (−$39/yr — moved to [[trading/rejected/funding-flip-recovery-4h-short-only-2026-05]]).
- **N=2 supporting cases for direction-restriction mutation pattern; N=3 in batch** when v2's direction-aligned improvement is included. The pattern-level evidence is intact even though specific strategies' deployment readiness is not.
- **Sparse-but-meaty beats frequent-but-tiny for deployment**: bollinger (16 trades, $1.50 ATP) clears at +$8/yr; funding-flip (50 trades, $0.20 ATP) fails at −$39/yr. Per [[foundation/methodology/deployment-edge-threshold|methodology §8]]: lower turnover OR larger per-trade edge favors deployment-grade.
- **Asymmetry is regime-conditional, not mechanism-permanent.** Respect observed asymmetry; reverse the restriction when regime shifts. Walk-forward / multi-window confirmation is the missing instrumentation on the *statistical* side; on the *economic* side, cross-regime friction calibration is also missing.
- **Don't stack with regime gate.** [[trading/rejected/funding-flip-recovery-regime-gate-2026-05]] documents the failed stacking attempt.
- **Lesson for future hypotheses**: per Part I §4 sharpening 2026-05-05 PM, every structured hypothesis must specify `expected_net_edge` upstream of backtest. These mutations were pre-registered with statistical-significance thresholds only; the deployment-grade question wasn't asked upstream — caused the productionize-candidate misclassification.
- **Bollinger short_only path-forward**: combined with stacking (HTF + vol filter + regime gating *carefully*) may clear threshold; flagged for Task 5 follow-up. Currently kept alive in research substrate (this entry serves as that home in absence of dedicated `wiki/trading/research-grade/` folder).

## Related

- [[trading/rejected/funding-flip-recovery-4h-short-only-2026-05]] — **funding-flip short_only rejected entry** (split out PM-3 stocktake); REJECTED due to frequent-but-tiny failure mode
- [[foundation/methodology/deployment-edge-threshold]] — methodology that recalibrated then split this entry's verdict (PM-2 + PM-3); both strategies are worked examples
- Part I §9 — Mutation Operators (direction-restriction promoted to explicit operator with trigger condition)
- Part I §4 — Structured Hypothesis Format (sharpened 2026-05-05 PM to require `expected_net_edge`)
- [[trading/mechanisms/funding-cycle-dynamics]] — funding-flip parent's mechanism family
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — design-pattern page; this entry is the direction-restriction half of N=4 supporting cases
- [[trading/rejected/funding-flip-recovery-regime-gate-2026-05]] — companion rejection (regime-gate-on-top hurts vs this control)
- [[trading/rejected/turtle-soup-4h-2026-05]] — companion rejection with same short-asymmetry partial truth (untested as short_only mutation)
- [[trading/rejected/ema-bias-breakout-4h-v2-2026-05]] — companion pass with direction-aligned improvement via embedded bias filter
- [[foundation/market-wisdom/bias-filter-vs-entry-signal]] — signal taxonomy; direction restriction is a degenerate bias filter
- [[foundation/methodology/strategy-validation-discipline]] — Marcos' Third Law + DSR adjustment context
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — regime classification (the load-bearing context for the asymmetry)

## Sources

- the backtesting lab — bollinger short-only control + pre-registration
- the backtesting lab — funding-flip short-only control + pre-registration
- the backtesting lab — bollinger symmetric parent
- the backtesting lab — funding-flip symmetric parent
- the backtesting lab — asymmetry first surfaced
