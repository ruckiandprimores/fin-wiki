---
title: "volume_z_asymmetric_4h — asymmetry falsified and inverted; long-side mechanism real (2026-05)"
status: evergreen
created: 2026-05-06
updated: 2026-05-06
tags: [rejected, asymmetry-falsified, asymmetry-inverted, forecast-vs-tradeable-gap, volume-z, htf-bias-scaffold, long-side-partial-truth, research-grade-pass, mutation-pending, day-swing]
---

# volume_z_asymmetric_4h — asymmetry falsified and inverted; long-side mechanism real (2026-05)

> **Lead learning**: The W2 raw-data forecasting effect (high-vol-down bars → −0.091% fwd-4h, n=63, direction stable) **does not survive operationalization in the same window** with 2×ATR stops + 3×ATR targets and multi-bar holds. **Asymmetric prediction inverted**: predicted short ≥ 2× long; observed long ATP +$1.527 vs short ATP −$0.162 — long is 9× short, **opposite direction**. The forecasting magnitude (~0.09%) is too small relative to stop width (~2%) and trade horizon (multi-bar) — trades hit stops or time-stops before the predicted continuation registers. **This is the canonical worked example for** [[foundation/methodology/forecast-vs-tradeable-gap|forecast-vs-tradeable-gap]] **methodology page.** Strategy carries best train→val transfer in batch (+0.216) and stable +Sharpe distribution (medSh +0.74); long-side at moderate Z is the load-bearing partial truth. Verdict: REJECT (asymmetry falsified + deployment-grade fail) with `volume_z_long_only_4h` mutation queued (predicted +$12/yr — first non-zero positive in inventory after `bb_short_only`'s +$9).

## What we learned

1. **Forecasting effect ≠ tradeable strategy when stop width and trade horizon are incompatible with effect magnitude.** This is the load-bearing methodological discovery from the batch and the canonical worked example for [[foundation/methodology/forecast-vs-tradeable-gap]]. The forecasting effect (-0.09%) was ~5% of stop magnitude (2%) and the holding period (multi-bar) far exceeded the effect's measurement horizon (4h). Trades exited on stops or time-stops before the predicted continuation could register.

2. **Same-window forecasting evidence is a slop trap.** Even though the *mechanism literature* (informed-flow microstructure, Harris) is independent of test window, the *effect-size estimate* used in the asymmetric prediction was partly W2-derived from same window — and the operational result inverted. Per [[foundation/methodology/strategy-validation-discipline|LdP Second Law]] §2.1: any pre-registered estimate from data the backtest will use is overfit by construction. Going forward: forecasting effects validated only on independent data, with explicit horizon/stop-width compatibility check ([[foundation/methodology/forecast-vs-tradeable-gap|diagnostic checklist §3]]).

3. **Asymmetric prediction CAN be informatively wrong.** Pre-registering `short ≥ 2× long` as the asymmetric falsifier was load-bearing — the inversion (long 9× short) is *clean* knowledge, not "didn't work" knowledge. The pre-registered concern (asymmetry observed in 2025-26 bearish-dominant year may be regime-correlated, not mechanism-driven) was **the team's own audit catching what would happen**. Discipline at design time made the failure mode usable.

4. **Long-side mechanism IS real at moderate Z.** Stable +Sharpe distribution (medSh +0.74, %val>0 = 87%, %PF>1 = 71%) and best train→val transfer (+0.216) in the batch indicate mechanism reality. Long ATP +$1.527 (vs combined +$0.47) at moderate Z (1.0–2.0, per team's read) extracts the working long-side mechanism. **First non-zero positive in inventory after `bb_short_only` (+$9).**

5. **Volume-Z + HTF-bias is on the same HTF-EMA-bias scaffold as the trend cluster.** Cross-strategy timing correlations: +0.586 with `ema_bias_v2`, +0.65 with `confluence_trend`, +0.686 with `taker_buy`. Volume-Z secondary axis is a [[trading/mechanisms/scaffold-as-mechanism|scaffold-as-mechanism]] expression of HTF-trend, not a distinct mechanism. The "asymmetric" framing was the spec's attempt to differentiate; the data falsified that.

## Hypothesis (Structured Format)

```
Hypothesis:    Volume Z-score extreme + HTF-bias + bar-direction
               aligned trigger captures asymmetric forward-return —
               specifically, short side ≥ 2× long side per-trade
               edge (W2-derived: high-vol-down bars predict −0.091%
               fwd-4h, n=63).

Mechanism:     Volume clustering at extreme bars signals informed
               participation; combined with HTF-bias and bar
               direction, selects bars where directional flow has
               momentum continuation. The *asymmetric* claim:
               high-vol-down bars are more predictive than
               high-vol-up bars (per W2 raw-data scan).

Observable:    Backtest grid 2025-05-03 → 2026-04-28, BTCUSDT perp 4h,
               $1000 capital, fee 0.04%. Pre-registered:
                 (a) Strategy-level: net ≥ $120/yr deployment-grade
                 (b) Hypothesis-level (asymmetric): short ATP ≥ 2× long
                     ATP (load-bearing falsifier; the asymmetric
                     prediction IS the spec)

Stops:         2× ATR (~2% on BTC at typical ATR)
Targets:       3× ATR (~3%)
Hold:          multi-bar (until stop, target, or time-stop)

Expected:      Sharpe stable; ATP higher on short side; net edge
               primarily from short side capturing high-vol-down bars.
               expected_net_edge:
                 source: W2 raw-data scan (-0.091% fwd-4h, n=63);
                         partly test-window-derived (honestly flagged
                         in spec)
                 value: +$60-100/yr if asymmetry holds
                 independence: mechanism literature (Harris/Easley-
                              O'Hara) independent; effect-size
                              estimate test-window-derived → research-
                              grade-only verdict default; flagged in
                              spec per Part I §4

Falsifier:     Strategy-level: net < $120/yr → deployment fail
               Asymmetric prediction (LOAD-BEARING):
                 short ATP < 2× long ATP → asymmetry falsified
                 short ATP < long ATP → asymmetry inverted
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp 4h, $1000, fee 0.04%)

| Metric | Value | Falsifier | Fired? |
|---|---:|---|:---:|
| Median Sharpe | **+0.74** | < 0 | NO (research-grade pass) |
| Median ATP | +$0.47 | — | combined |
| %PF>1 | 71.0% | — | stable distribution |
| %val>0 | 87.1% | — | strong stability |
| corr(train, val) | **+0.216** | — | **best in batch** |
| trades/yr | 53.6 | — | — |
| gross/yr | +$25 | — | — |
| friction (1bp×$5K, $1/trade) | −$54 | — | — |
| **net/yr** | **−$29** | ≥ $120 | **FAIL** (closest to threshold in batch) |

| Asymmetric prediction falsifier (LOAD-BEARING) | Predicted | Observed | Verdict |
|---|---|---:|:---:|
| Long ATP | smaller | **+$1.527** | larger |
| Short ATP | ≥ 2× long | **−$0.162** | smaller |
| `short / long` ratio | ≥ 2.0 | **0.11** (negative) | **INVERTED** |
| %L>0,S<0 | small | **48.9%** | dominant pattern |
| %L<0,S>0 | dominant | 6.0% | rare |

## Falsifier verdict: ASYMMETRY INVERTED + deployment-grade FIRED

- **Strategy-level (deployment-grade gate)**: net −$29/yr → **FIRED** (closest-to-threshold in batch)
- **Asymmetric prediction (load-bearing)**: predicted short dominates; observed long dominates 9× — **FIRED AND INVERTED**

This is the **most informative falsifier outcome** in the batch — not just "didn't work" but "inverted in a way that validates the W2-effect-translation concern." The pre-registered concern (asymmetry might be regime-correlated, not mechanism-driven) was confirmed exactly. Genuine knowledge.

## Critical methodological lesson — forecast-vs-tradeable gap

This rejection IS the canonical worked example documented at [[foundation/methodology/forecast-vs-tradeable-gap|forecast-vs-tradeable-gap]]:

- **L1 (forecasting effect)**: −0.091% fwd-4h on high-vol-down bars (n=63 in W2 scan). Real, direction-stable.
- **L2 (tradeable strategy)**: 2×ATR stops (~2%) + 3×ATR targets + multi-bar hold. **Effect magnitude / stop ratio = 0.045 (target ≥ 3 per diagnostic §3.1)** → predictably fails translation.
- **L3 (deployment)**: pre-empted by L2 failure.

Diagnostic checklist verdicts (had they been applied at design time):
- §3.1 magnitude ratio: 0.09% / 2% = **0.045 ≪ 3 — FAILS**
- §3.2 horizon match: trade hold (multi-bar) / L1 measurement (4h) = **>1.5 — FAILS**
- §3.3 mechanics compatibility: bar-close-to-bar-close mid prices vs market-order through spread + multi-bar holds — **MISMATCH**

Triple checklist failure. The L1→L2 translation was predictably going to fail; the asymmetric prediction was an artifact of trying to make L1 evidence carry trade-mechanics weight it couldn't bear. The canonical worked example **is now wiki-encoded** so future strategy specs can avoid the same trap.

## What partial truth remains?

| Component | Status | Evidence | Salvage condition |
|-----------|--------|----------|-------------------|
| **Asymmetric prediction (short ≥ 2× long)** | ❌ Falsified and inverted | Long 9× short, opposite direction | Closed — asymmetric framing rejected for this mechanism |
| **W2 forecasting effect (−0.09% fwd-4h on high-vol-down)** | Real at L1; doesn't translate to L2 in this form | Diagnostic checklist triple-failure | Different trade mechanics (sub-bar exits, position-sizing modulation) — not pursued |
| **Long-side mechanism at moderate Z** | ✅ **Real (load-bearing partial truth)** | medSh +0.74; long ATP +$1.527; %val>0 = 87%; corr(train,val) = +0.216 | Mutation `volume_z_long_only_4h` at moderate Z [1.0, 2.0]; predicted net +$12/yr |
| **Train→val transfer mechanism** | ✅ Best in batch (+0.216) | Cross-window stability indicator | Use as research-grade strategy + OOS test on 2024 window |
| **Volume-Z + HTF-bias on HTF-EMA-bias scaffold** | Same scaffold as trend cluster | Timing corr +0.59-0.69 | Per [[trading/mechanisms/scaffold-as-mechanism]], not a distinct mechanism |

The **load-bearing partial truth**: long-only-at-moderate-Z extracts the working long-side mechanism. Estimated math from grid data:
- Long-only ATP ≈ +$1.53 (vs combined +$0.47)
- Long-only trades/yr ≈ 22 (vs combined 54)
- Long-only gross ≈ +$34, friction ≈ −$22, **net ≈ +$12**

Net is small but *positive without selection bias* — first such candidate in the inventory after Task 4's `bb_short_only` (+$9). Still below 12% threshold. **Worth productionizing as research-grade strategy + OOS-testing on 2024 window.**

## Reason classification

- [x] **Falsifier fired** — both deployment-grade AND asymmetric-prediction falsifier
- [x] **Asymmetry inverted** — informative falsifier outcome; pre-registered concern (regime-correlated asymmetry) confirmed
- [x] **Same-scaffold mutation** — timing corr +0.59-0.69 with HTF-trend cluster; per [[trading/mechanisms/scaffold-as-mechanism]] same mechanism class
- [x] **Forecast-vs-tradeable-gap** — canonical worked example; L1 effect doesn't survive L2 translation
- [x] **Long-side mechanism real (research-grade pass)** — best train→val transfer in batch; mutation queued
- [ ] No falsifier articulated (clean pre-registration with asymmetric falsifier load-bearing)
- [ ] Already arbitraged
- [ ] Graveyard duplicate

## Trial count on dataset (per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] §2.2)

≥18 strategies tested on BTCUSDT perp 2025-05/2026-04 window. volume_z adds a research-grade-pass-with-asymmetry-falsified result; trial count recorded.

## Caveats per [[foundation/methodology/strategy-validation-discipline|validation discipline]]

- **Single-window backtest** — 2025-05/2026-04 only. Mutation OOS test on 2024 (bullish-dominant) is **load-bearing** for productionize verdict — would test whether long-only edge persists when long is regime-aligned.
- **W2 forecasting effect was test-window-derived** — per [[foundation/methodology/strategy-validation-discipline|LdP Second Law]] + [[foundation/methodology/deployment-edge-threshold|§4.5]]: same-data round-trips are overfit by construction. The asymmetric prediction's failure was overdetermined: regime drift dominated bar-level forecast drift, AND the forecast-magnitude/stop-ratio failed translation. Going forward: forecasting effects validated only on independent data + horizon/stop-width compatibility check.
- **PSR/DSR not pipeline-implemented** — Sharpe +0.74 uncorrected for trial-count multiplicity.
- **Slippage modeling absent** — long-only-moderate-Z mutation may flip net more positive under realistic measured slippage.

## Mutation candidates queued (per stocktake §"volume_z" mutation list)

**`volume_z_long_only_4h`** (LOAD-BEARING) — drop the bidirectional design (asymmetry inverted, short side noise); restrict `vol_z_threshold ∈ [1.0, 2.0]` (per team's moderate-threshold finding); long-only entry on bullish HTF bias.

Predicted net +$12/yr at $1K capital — first non-zero positive in inventory after `bb_short_only`'s +$9. Still below 12% threshold but *positive without selection bias*. Worth productionizing as research-grade + OOS-testing on 2024.

## Key Takeaways

- **Asymmetric prediction FALSIFIED AND INVERTED** — pre-registered concern (regime-correlated asymmetry) confirmed; informative falsifier outcome.
- **Canonical worked example for [[foundation/methodology/forecast-vs-tradeable-gap]]** — L1 effect (~0.09%) was 5% of stop magnitude (2%); L1→L2 translation failed predictably under the diagnostic checklist.
- **Same-window forecasting evidence is a slop trap** — even with mechanism-literature independence, effect-size estimates from same data fail.
- **Long-side mechanism real at moderate Z** — best train→val transfer in batch (+0.216); long ATP +$1.527; mutation `volume_z_long_only_4h` predicted net +$12/yr.
- **Same-scaffold expression of HTF-trend mechanism** — timing corr +0.59-0.69 with trend cluster.
- **Discipline lesson encoded**: forecasting effects need (a) data independence per LdP Second Law AND (b) magnitude/horizon/mechanics compatibility check per forecast-vs-tradeable-gap diagnostic. Both layers required.

## Related

- [[foundation/methodology/forecast-vs-tradeable-gap]] — **canonical worked example**; L1 effect doesn't survive L2 translation
- [[foundation/methodology/strategy-validation-discipline]] — LdP Second Law (data independence); §2.1 operationalization references this case
- [[foundation/methodology/deployment-edge-threshold]] — §4.5 (predictive vs descriptive estimation); friction floor
- [[trading/mechanisms/scaffold-as-mechanism]] — same scaffold as trend cluster (timing corr +0.59-0.69); secondary axis ≠ distinct mechanism
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — companion design pattern; HTF-bias scaffold context
- [[trading/rejected/taker-buy-asymmetric-4h-novelty-2026-05]] — sibling on same scaffold (timing corr +0.686)
- [[trading/rejected/confluence-trend-4h-2026-05]] — sibling on same scaffold (timing corr +0.65)
- [[foundation/sources/harris-trading-exchanges]] — informed-flow microstructure foundation
- [[foundation/macro/regime-conditional-edge-2025-26]] — W2 cohort regime context (long-side regime-aligned in trend cluster's positive sub-window)

## Sources

- 2026-05-05 batch stocktake: the backtesting lab §"`volume_z_asymmetric_4h`" + §"Asymmetry test"
- W2 raw-data scan: the backtesting lab Finding 4 (high-vol-down bars predict −0.091% fwd-4h, n=63)
- the backtesting lab — strategy spec + pre-registration (with asymmetric falsifier as load-bearing test)
- Harris *Trading and Exchanges* — informed-flow microstructure foundation
- Auto-memory: a standing convention (LdP Second Law); a standing convention (extracted under new lead-with-learnings discipline)
