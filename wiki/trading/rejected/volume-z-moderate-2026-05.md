---
title: "volume_z_long_moderate_4h — moderate-Z hypothesis: lower bound CONFIRMED, upper bound FALSIFIED (2026-05)"
status: evergreen
created: 2026-05-06
updated: 2026-05-06
tags: [rejected, partial-truth, volume-analysis, volume-z-score, moderate-not-extreme, credible-vs-incidental, mechanism-reframe, scaffold-residual, day-swing]
---

# volume_z_long_moderate_4h — moderate-Z hypothesis: lower bound CONFIRMED, upper bound FALSIFIED (2026-05)

> **Lead learning**: The team's 2026-05-05 hypothesis "moderate volume-spike thresholds work better than extreme ones" — operationalized as Z band [1.0, 2.0] — is **partially confirmed and partially falsified**. **Lower bound CONFIRMED**: restricting Z>1.5 (rejecting weak-volume bars) does help (medSh +1.14 at Z>1.5 vs +0.49 at Z>0.75). **Upper bound FALSIFIED**: restricting Z<2.0 HURTS (medSh +0.01 at Z<1.5 vs +0.97 at Z<2.5). **Empirical optimal: [1.5, 2.5], not predicted [1.0, 2.0].** Mechanism reframe: the relevant question for volume isn't "extreme vs moderate" — it's **"credible vs incidental."** Credibility threshold ~1.5σ; everything above is signal, nothing below is. The two-mutation stack (direction restriction P3 + threshold restriction) failed to lift ATP to predicted +$2.00 (achieved +$0.67); cell concentration via threshold restriction works only for the lower bound. **Verdict: REJECT as a deployment-grade strategy under the moderate-Z framing; partial truth retained at the [1.5, 2.5] band as an embedded filter for trend-aligned mechanisms.**

## What we learned

1. **Volume-Z as quality filter has a credibility floor (~1.5σ) but no ceiling.** The original team finding (lower-Z bars dilute the signal) holds. The framing extension (also restrict the upper bound, treat extreme-Z as separately dilutive) does not — extreme-Z bars in the [2.0, 2.5] band IMPROVE the strategy, not dilute it.

2. **Mechanism reframe: "credible vs incidental" replaces "moderate vs extreme."** Volume bars below ~1.5σ are likely incidental (random-flow noise); bars above are credible (informed-flow signature). There is no "too credible" zone in the testable Z range.

3. **Two-mutation stacks compound prediction error.** Predicted ATP lift came from two assumptions: (a) direction restriction P3 concentrates per-trade economics (false: scaffold-residual when working direction IS the scaffold — see [[trading/mechanisms/scaffold-as-mechanism|N=7 conditional P3]]); (b) threshold restriction concentrates the dilutive bars out (true for lower bound only). Combined ATP +$0.67 vs predicted +$2.00 — both mutations partially failed for different reasons.

4. **Optimal band [1.5, 2.5] surfaced via param sensitivity, not pre-registered.** The pocket evidence is a heat-map-derived discovery, not a falsifier outcome. Per [[foundation/methodology/strategy-validation-discipline|Marcos' Second Law]] §2.1 (no research under influence of backtest), the [1.5, 2.5] band is research-grade-only until tested on independent data. Pre-registered bands [1.0, 2.0] failed; refined bands need fresh test.

## Hypothesis (Structured Format)

```
Hypothesis:    Restricting volume_z_asymmetric_4h to long-only entries
               (P3 mutation) and to Z band [1.0, 2.0] (threshold
               mutation) concentrates per-trade economics by removing
               two dilutive cell-classes simultaneously: (a) short-side
               trades that lack edge in 2025-26 bullish-bias regime
               (per parent strategy's asymmetric P&L); (b) extreme-Z
               bars that may represent noise events (cascades, liquidations,
               news shocks) with poor follow-through.

Mechanism:     P3: parent's long_pnl was the working direction; concentrating
               trades there should lift Sharpe and ATP. Threshold:
               moderate-Z bars (Z 1-2) represent informed-flow signature
               with sustainable follow-through; extreme-Z bars (Z>2.5) and
               weak-Z bars (Z<1.0) both represent edge cases (forced
               flow, random noise) with poor expected continuation.

Observable:    Backtest grid 2025-05-03 → 2026-04-28, BTCUSDT perp 4h,
               $1000 capital, fee 0.04%. Pre-registered hypothesis-level
               falsifier vs parent volume_z_asymmetric_4h: must beat
               parent on medSh ≥+0.94 (parent +0.66 + 0.30) AND ATP
               ≥+$2.00 (parent ATP +$1.53 × concentration factor).

Expected:      medSh ≈ +0.94; medATP ≈ +$2.00; train→val correlation
               ≥ +0.20.
               expected_net_edge:
                 source: parent baseline (volume_z_asymmetric_4h
                         medSh +0.66, ATP +$1.53) + concentration factor
                         from removing dilutive cell-classes
                 value: +$50/yr at $1K capital
                 independence: same-window estimate vs same-window test
                              — research-grade-only verdict default per
                              Part I §4 expected_net_edge
                              independence rule (LdP Second Law)

Falsifier:     Hypothesis-level: 4 thresholds (medSh +0.94; ATP +$2.00;
               train→val corr +0.20; %val>0 ≥85%). 3 of 4 fired below
               threshold = falsifier fired. Strategy-level: net <
               $120/yr → deployment fail.
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp 4h, $1000, fee 0.04%)

### Pre-registered falsifier outcomes

| Falsifier (hypothesis-level) | Pre-reg threshold | Observed | Verdict |
|---|---|---|---|
| medSh | ≥ +0.94 | **+0.66** | **FIRED** |
| ATP | ≥ +$2.00 | **+$0.67** | **FIRED HUGELY** (best subset reaches only +$1.37) |
| train→val correlation | ≥ +0.20 | **−0.02** | **FIRED** |
| %val>0 | ≥ 85% | 93% | PASSES (regime-driven, not strategy-driven) |

**3 of 4 hypothesis-level falsifiers fired.** Pre-registration discipline operated correctly.

### Param sensitivity surfaced the partial-truth (post-hoc analysis)

#### Lower bound (`vol_z_low`)

| vol_z_low | median Sharpe | median ATP |
|---:|---:|---:|
| 0.75 | +0.49 | +$0.43 |
| 1.00 | +0.66 | +$0.68 |
| **1.50** | **+1.14** | **+$1.37** |

**Lower bound CONFIRMED.** Restricting Z>1.5 (rejecting weak-volume bars below the credibility floor) does help. Narrow-band prediction holds at the lower bound.

#### Upper bound (`vol_z_high`)

| vol_z_high | median Sharpe | median ATP |
|---:|---:|---:|
| 1.5 | +0.01 | −$0.08 |
| 2.0 | +0.73 | +$0.78 |
| **2.5** | **+0.97** | **+$1.00** |

**Upper bound FALSIFIED.** Restricting Z<2.0 HURTS — including bars Z 2.0-2.5 IMPROVES the strategy. The "extreme is dilutive" framing fails for the upper bound in this regime.

#### Optimal pocket evidence (run #19634)

`vol_z_low=1.5, vol_z_high=2.5` (the empirical optimum, NOT the pre-registered [1.0, 2.0] band):
- Train +$257 / Val +$220
- Sharpe **4.49**
- 22 trades, ATP **+$21.67**, win-rate 50%

**Caveat per LdP Second Law**: this is heat-map-derived. Research-grade-only until tested on independent data. The pocket informs `meta_trend_v1`'s `bullish_volume_credible` sub-strategy lineup but should not be promoted to deployment-grade without OOS test.

## Mechanism reframe: credible vs incidental

The original team framing ("moderate volume works better than extreme") was anchored on a noise-vs-signal binary that the data doesn't support in this regime. The reframe:

| Z range | Old framing | New framing |
|---|---|---|
| Z < 1.5 | "not extreme — should be tradable as moderate" | **Incidental volume** — random flow, no edge |
| 1.5 ≤ Z ≤ 2.5 | "moderate" (target band) | **Credible volume** — informed-flow signature with sustainable follow-through |
| Z > 2.5 | "extreme — should be filtered out" | **Still credible** in the testable Z range up to ~2.5; potentially noise above (untested at higher) |

**The credibility threshold matters; the upper bound is empirically not where extreme-noise lives in the [1.5, 2.5] tested range.** Whether a true upper bound exists at higher Z (≥3) is untested in this batch.

## Falsifier verdict: 3 of 4 FIRED

- **medSh, ATP, train→val correlation** all fired below threshold → hypothesis-level **FIRED**
- **%val>0 passed** (93% > 85%) but regime-driven, not strategy-driven (parent ema_bias_v2 also hits 97% in this regime — not a discriminating signal in 2025-26)

Falsifier discipline operated correctly — the failure isolated where the prediction was wrong (per-trade economics + train→val transfer), and the param-sensitivity surfaced where the partial truth lives (lower bound only).

## Scaffold residual diagnosis

Per [[trading/mechanisms/scaffold-as-mechanism|scaffold-as-mechanism N=7]]: this strategy's timing correlation with `ema_bias_breakout_4h_v2` is **+0.384** — the long-only mutation halves the cluster correlation from parent's +0.586 but the scaffold signature is intact. Both mutations layered on parent failed to escape the HTF-EMA-bias scaffold:

- **Direction restriction P3** is **scaffold-residual** here (parent's long_pnl was just bias-aligned bars; restricting to long-only doesn't lift per-trade economics — half the trades, identical economics)
- **Threshold restriction** worked at the lower bound (credibility floor) but didn't compound with P3 in the way pre-registration assumed

The combined two-mutation strategy is a same-mechanism mutation, not a distinct strategy. Diversification value low; deployment value at the proposed framing absent.

## What partial truth remains?

- **"Credible-volume-bars (Z>1.5) carry signal; incidental-volume-bars (Z<1.5) don't."** Lower-bound credibility floor is real; applies as embedded filter in trend-aligned mechanisms.
- **The [1.5, 2.5] band as an embedded filter** for trend-following strategies on the HTF-EMA-bias scaffold. The pocket evidence (Sharpe 4.49 at run #19634) is research-grade-only on this window but a viable seed for `meta_trend_v1`'s `bullish_volume_credible` sub-strategy.
- **Update mechanism wisdom for future strategies**: from "moderate volume works better" → "credible (Z>1.5) volume works; incidental (Z<1.5) doesn't add." The upper bound is unknown in tested range and should not be assumed without testing.

## Reason classification

- [x] **Falsifier fired** — 3 of 4 hypothesis-level falsifiers fired
- [x] **Same-scaffold mutation** — timing corr +0.384 with bias-cluster anchor; scaffold-residual per [[trading/mechanisms/scaffold-as-mechanism]]
- [x] **Partial truth retained** — lower-bound credibility floor confirmed; mechanism reframe surfaced
- [x] **Heat-map-derived pocket** — [1.5, 2.5] band is post-hoc; research-grade-only per LdP Second Law
- [ ] No falsifier articulated (clean pre-registration)
- [ ] Already arbitraged
- [ ] Graveyard duplicate

## Trial count on dataset (per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] §2.2)

≥19 strategies tested on BTCUSDT perp 2025-05/2026-04 window so far (13 prior + 5 in 2026-05-05 + ≥6 in 2026-05-06). Falsifier-fired result is robust to multiplicity correction (3 of 4 falsifiers fired below threshold; param-sensitivity-derived pocket explicitly flagged research-grade-only).

## Caveats per [[foundation/methodology/strategy-validation-discipline|validation discipline]]

- **Single-window backtest** — 2025-05/2026-04 only. Per [[foundation/macro/regime-conditional-edge-2025-26|W2 finding]], edge concentrated in Q4 2025 + Q1 2026. The [1.5, 2.5] partial-truth band may be regime-specific (bullish-bias trends + 2025-26 vol regime).
- **Heat-map-derived pocket** — [1.5, 2.5] band surfaced via param sensitivity, not pre-registered. Per LdP Second Law, this is research-grade-only.
- **Train→val correlation −0.02** — parameter selection essentially random for this strategy on this window (consistent with `confluence_trend_4h` finding); per Marcos' Third Law crypto walk-forward critique, paired controls or regime-conditioned tests are the substitute.
- **Upper bound at Z>2.5 untested** — this batch tested up to Z=2.5; whether a true noise band exists at higher Z (cascades, news shocks) requires expanded grid.

## Key Takeaways

- **Lower bound CONFIRMED, upper bound FALSIFIED.** Original team finding "moderate volume works better than extreme" partially holds: credibility floor (Z>1.5) is real, ceiling (Z<2.0) is not.
- **Empirical optimal [1.5, 2.5], not predicted [1.0, 2.0].** Heat-map-derived; research-grade-only until OOS-tested.
- **Mechanism reframe**: "credible vs incidental" replaces "moderate vs extreme." Credibility threshold ~1.5σ; no testable upper noise band in [1.5, 2.5].
- **Two-mutation stack failed** to lift ATP to predicted +$2.00 (achieved +$0.67). Direction restriction P3 was scaffold-residual; threshold restriction worked at lower bound only.
- **Scaffold residual**: timing corr +0.384 with bias-cluster anchor; mutations layered on parent didn't escape HTF-EMA-bias scaffold. Per N=7 conditional P3 finding.
- **Real learning**: volume-Z thresholds need separate calibration for floor (credibility) vs ceiling (noise). Don't assume symmetric "moderate-is-best" framings; test each bound independently.

## Related

- [[trading/rejected/volume-z-asymmetric-4h-asymmetry-2026-05]] — parent strategy; asymmetric prediction falsified AND inverted (long 9× short)
- [[trading/mechanisms/scaffold-as-mechanism]] — N=7 evidence; `volume_z_long_moderate_4h` is one of the long-only mutations halving cluster correlation but preserving scaffold signature; conditional P3 mutation operator finding
- [[trading/mechanisms/volume-analysis]] — parent mechanism page; credibility-floor reframe applies broadly
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — embedded > separate pattern; volume-Z embedded filter is an example
- [[foundation/methodology/strategy-validation-discipline]] — Marcos' Second Law (no research under influence of backtest); heat-map-derived pocket flagged
- [[foundation/methodology/deployment-edge-threshold]] — net-of-friction floor; even pocket #19634 needs OOS validation before deployment-grade promotion
- [[foundation/methodology/forecast-vs-tradeable-gap]] — translation-loss methodology; team's L1 finding (lower-Z dilutes) translated cleanly only at the floor
- [[foundation/macro/regime-conditional-edge-2025-26]] — W2 cohort regime context

## Sources

- 2026-05-06 batch stocktake: the backtesting lab (#6 verdict + cross-strategy synthesis)
- the backtesting lab — strategy spec + pre-registration + grid output (run #19634 pocket)
- the backtesting lab — parent strategy
- Original team-finding source: the backtesting lab — moderate-Z hypothesis pre-registration
- the backtesting lab — meta-strategy lineup; `bullish_volume_credible` sub-strategy uses [1.5, 2.5] band
- the backtesting lab — Phase B cluster correlation (+0.384 with bias anchor)
