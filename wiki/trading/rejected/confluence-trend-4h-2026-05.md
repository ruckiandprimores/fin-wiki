---
title: "confluence_trend_4h — 3-axis confluence dilutes trend mechanism (2026-05)"
status: evergreen
created: 2026-05-06
updated: 2026-05-06
tags: [rejected, trend-following, confluence-stack, vol-expansion, ema-bias-scaffold, deployment-grade-fail, paired-falsifier-fail, day-swing]
---

# confluence_trend_4h — 3-axis confluence dilutes trend mechanism (2026-05)

> **Lead learning**: 3-axis confluence (HTF-bias + vol-expansion + breakout-direction) on top of an HTF-EMA-bias trend scaffold is **mechanically dilutive** in the 2025-26 BTC perp 4h regime — the additional axes reduce trade count without lifting per-trade edge enough to clear friction. Confluence-as-quality-filter only works if the *added* axis selects bars where the base mechanism's edge is amplified. Vol-expansion didn't amplify; it just constrained trade count. The strategy underperformed its own parent (`ema_bias_breakout_4h_v2`) on every metric — predicted to be 0.30 Sharpe better; observed 1.00 Sharpe worse. Verdict: REJECT (deployment-grade fail + paired-falsifier fail vs parent).

## What we learned

1. **Confluence ≠ quality filter by default.** Adding axes only amplifies edge if the added axis selects bars where the base mechanism does *better*. Vol-expansion bars in this window were not better-trend bars — they were bars where stops fired faster on bigger ATR moves, eating per-trade edge. The strategy spec assumed the axes would interact constructively; they interacted dilutively.

2. **Confluence selects fewer-and-larger bars; smaller stops + bigger ATR = more stop-outs.** Vol-expansion increases ATR-derived stop distance proportionally, but per-trade ATP didn't scale because trend bars in 2025-26 were already vol-expanded. Net: same trades the parent took, but with worse stop placement.

3. **Train→val correlation 0.006 = parameter selection unreliable.** Single 1y window cannot discriminate between parameter regions for this strategy. Per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] §2.2 + [[foundation/methodology/strategy-validation-discipline|§9 PBO via CSCV]], parameter selection on this train→val split is essentially random.

4. **The parent (ema_bias_breakout_4h_v2) IS the mechanism here.** The HTF-EMA-bias scaffold delivers what edge the regime offers; secondary axes are redundant in this regime. This is the [[trading/mechanisms/scaffold-as-mechanism|scaffold-as-mechanism]] finding crystallized at the within-batch level: confluence_trend × ema_bias_v2 timing correlation = +0.711.

## Hypothesis (Structured Format)

```
Hypothesis:    3-axis confluence trend-following (HTF-EMA-bias +
               vol-expansion + breakout-direction-aligned bar trigger)
               concentrates trend signal beyond bare HTF-bias breakout.

Mechanism:     HTF-EMA-bias selects regime-aligned bars; vol-expansion
               (rising ATR) selects bars with breakout potential;
               breakout-direction-aligned bar trigger selects entry
               timing. Three independent confirmation axes should
               concentrate edge per trade above parent strategy that uses
               only HTF-bias + breakout.

Observable:    Backtest grid 2025-05-03 → 2026-04-28, BTCUSDT perp 4h,
               $1000 capital, fee 0.04%. Pre-registered paired falsifier
               vs parent ema_bias_breakout_4h_v2: must beat parent by
               ≥+0.30 Sharpe AND ≥3× per-trade ATP. Strategy-level
               falsifier: deployment-grade gate (net ≥ $120/yr).

Expected:      medSh ≈ +1.20 (parent +0.89 + 0.3 gap); medATP ≈ +$2.00
               (3× parent's +$0.62); net ≈ +$60/yr (above $120/yr threshold
               at higher capital).
               expected_net_edge:
                 source: parent baseline + theoretical concentration
                         from 3-axis confluence
                 value: +$60/yr at $1K
                 independence: same-window estimate vs same-window test
                              — research-grade-only verdict default per
                              Part I §4

Falsifier:     Paired vs parent: if Sharpe gap < +0.20 OR ATP ratio
               < 2× parent, novelty claim falsified. Strategy-level:
               net < $120/yr → deployment-grade fail.
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp 4h, $1000, fee 0.04%)

| Metric | Confluence | Parent (ema_bias_v2) | Gap | Predicted | Observed |
|---|---:|---:|---:|---|---|
| medSh | **−0.11** | +0.89 | **−1.00** | +0.30 better | 1.00 worse |
| medATP | **−$0.16** | +$0.62 | **−$0.78** | 3× better | dilutive |
| %PF>1 | 42.3% | 82.8% | −40.5pp | — | — |
| %val>0 | 60.7% | 97.2% | −36.5pp | — | — |
| trades/yr | 68.1 | 69.2 | similar | — | — |
| gross/yr | −$11 | +$43 | — | — | — |
| friction (1bp×$5K, $1/trade) | −$68 | −$69 | — | — | — |
| **net/yr** | **−$79** | **−$27** | — | ≥+$120 | both fail |

**Param-grid gap noted**: `stop_atr_mult` was [1.0, 2.5] only — missing [1.5, 2.0] region where parent's best Sharpe lived. `confluence_trend_4h_v2` (this batch) tests rerun with `stop_atr_mult ∈ [1.0, 1.5, 2.0, 2.5, 3.0]`. If v2 still loses to parent, mechanism class closed for this op form.

### v2 forclosure run (2026-05-06)

`confluence_trend_4h_v2` ran the full `stop_atr_mult` sweep [1.0, 1.5, 2.0, 2.5, 3.0] (13,122 grid combos, 9,099 eligible). Verdict: **REJECT confirmed at every level.**

| Falsifier | Pre-reg threshold | Observed | Verdict |
|---|---|---|---|
| Strategy-level deployment gate | net ≥ $120/yr | −$0.27/yr median | FIRED |
| Hypothesis-level Sharpe | ≥ +1.19 (parent +0.89 + 0.30) | +0.082 (gap −0.81) | FIRED |
| Hypothesis-level ATP | ≥ +$0.93 (1.5× parent's +$0.62) | −$0.006 (dilutive) | FIRED |

**Stop-region partial truth confirmed:** the [1.5, 2.0] gap WAS real — stop=1.0 gives med val −$17, stop=2.0 gives +$20, stop=3.0 gives +$12. But the recovered edge isn't enough to flip either falsifier. **Param-region miss was secondary; mechanism-level dilution is dominant.**

Train→val correlation: −0.011 (essentially zero). Both>0 = 24.6% (matches independence prediction 24.5%) — apparent param island is coherent-shaped noise, not edge.

**v2 forclosure goal achieved:** the REJECT verdict cannot be blamed on missing the param region. Confluence stack is mechanically dilutive on TS-momentum scaffold in 2025-26 BTC perp 4h regime. **Strategy class closed for this op form.**

Per [[trading/mechanisms/scaffold-as-mechanism|scaffold-as-mechanism N=7]]: v2 timing correlation with `ema_bias_breakout_4h_v2` is **+0.754** — confirms same-scaffold mutation. The v2 entry doesn't escape the scaffold.

See the backtesting lab and the backtesting lab (#1) for full per-strategy analysis.

## Falsifier verdict: BOTH FIRED

- **Strategy-level (deployment-grade gate)**: net −$79/yr vs threshold ≥+$120/yr → **FIRED**
- **Paired vs parent**: Sharpe gap −1.00 (predicted ≥+0.20) → **FIRED**; ATP ratio dilutive (predicted ≥2×) → **FIRED**

Falsifier discipline operated correctly — both layers fired with specific reasons.

## What partial truth remains?

- **Vol-expansion as a confluence input on this scaffold is rejected for this regime.** Not "wrong" universally — confluence in different regimes (compressed-vol-rich, e.g. 2023-mid) might select differently. Untested.
- **The parent mechanism (ema_bias_breakout_4h_v2) extracts what edge the regime offers.** Confluence framing here was an attempt to amplify; data says no amplification possible via this axis combination in this regime.
- **v2 rerun (2026-05-06) closed the foreclosure test:** [1.5, 2.0] stop-region IS materially better than [1.0, 2.5], but recovered edge insufficient to flip any falsifier. Param-region miss was secondary; mechanism-level dilution is dominant. See "v2 forclosure run" subsection above.

## Reason classification

- [x] **Falsifier fired** — both strategy-level AND paired falsifier vs parent fired
- [x] **Mechanism dilutive in current regime under default parameterization**
- [x] **Same-scaffold mutation** — high timing correlation (+0.711) with parent confirms not a distinct mechanism per [[trading/mechanisms/scaffold-as-mechanism]]
- [ ] No falsifier articulated (clean pre-registration)
- [ ] Already arbitraged (cannot distinguish from "regime-absent")
- [ ] Graveyard duplicate

## Trial count on dataset (per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] §2.2)

≥18 strategies tested on BTCUSDT perp 2025-05/2026-04 window so far (13 prior + 5 in 2026-05-05 batch). Falsifier-fired-with-both-layers result is robust to multiplicity correction (negative across all metrics, paired comparison fires independently of statistical-significance gate).

## Caveats per [[foundation/methodology/strategy-validation-discipline|validation discipline]]

- **Single-window backtest** — 2025-05/2026-04 only. Per [[foundation/macro/regime-conditional-edge-2025-26|W2 finding]], the broader cohort had concentrated edge in Q4 2025 + Q1 2026; confluence rejection is window-specific.
- **PSR/DSR not pipeline-implemented** — confidence intervals not computed. Direction is robust to multiplicity correction; magnitude is not certified.
- **Walk-forward / multi-window absent** — pipeline limitation per [[foundation/methodology/strategy-validation-discipline|§13 crypto-specific concerns]].
- **Train→val correlation 0.006** — parameter selection essentially random; per the [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] crypto walk-forward critique, paired controls or regime-conditioned tests are the substitute, not multi-year walk-forward.

## Key Takeaways

- **Confluence is dilutive when the added axis doesn't select bars where the base mechanism does better.** Vol-expansion in 2025-26 BTC 4h was redundant with the trend regime, not amplifying.
- **The HTF-EMA-bias scaffold IS the mechanism in this regime.** Per [[trading/mechanisms/scaffold-as-mechanism]] finding (timing corr +0.711 with parent), confluence_trend_4h is a same-mechanism mutation, not a distinct strategy.
- **Both falsifier layers fired** — strategy-level deployment-grade AND paired vs parent. Discipline operated correctly.
- **v2 rerun closed (2026-05-06)** — full [1.0, 3.0] stop sweep ran; param-region miss objection formally closed. 3/3 falsifiers fired again. Strategy class closed for this op form.
- **Real learning**: confluence claims need to specify *which* base-mechanism bars the added axis amplifies. "More axes = more quality" is the slop framing.

## Related

- [[trading/rejected/ema-bias-breakout-4h-v2-2026-05]] — parent strategy; the underlying scaffold this confluence sits on
- [[trading/mechanisms/scaffold-as-mechanism]] — companion design pattern; explains the +0.711 timing correlation
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — bias-filter > separate regime gate (this confluence is an embedded-bias-filter at parent level + extra axis added on top; the extra axis layer is the failure mode)
- [[trading/mechanisms/moving-averages]] — canonical bias-filter mechanism (EMA20/EMA50)
- [[foundation/methodology/strategy-validation-discipline]] — Marcos' Third Law (trial count); paired falsifier discipline
- [[foundation/methodology/deployment-edge-threshold]] — net-of-friction floor
- [[foundation/macro/regime-conditional-edge-2025-26]] — W2 cohort regime context

## Sources

- 2026-05-05 batch stocktake: the backtesting lab §"`confluence_trend_4h`"
- the backtesting lab — strategy spec + pre-registration
- Zarattini 2025 + Murphy ch on volume confirmation — vol-expansion confluence mechanism source
