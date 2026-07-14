---
title: "taker_buy_asymmetric_4h — novelty falsified, mechanism real but not distinct (2026-05)"
status: evergreen
created: 2026-05-06
updated: 2026-05-06
tags: [rejected, novelty-falsified, microstructure, taker-imbalance, htf-bias-scaffold, scaffold-as-mechanism, research-grade-pass, mutation-pending, day-swing]
---

# taker_buy_asymmetric_4h — novelty falsified, mechanism real but not distinct (2026-05)

> **Lead learning**: The strategy's central novelty claim — that taker-buy ratio extreme + HTF-bias + bar-direction-aligned constitutes a **distinct microstructure signal axis** — collapsed at the trade-timing level. Cross-strategy timing correlations: +0.677 with `confluence_trend_4h`, +0.704 with `ema_bias_breakout_4h_v2` (predicted <+0.4). Taker-imbalance is being **captured** by the HTF-bias trend scaffold, not differentiated from it. The strategy itself carries a stable +Sharpe distribution across 8,748 runs (medSh +0.82, %val>0 = 90%, %PF>1 = 70%) — the mechanism is real, just **not distinct** from trend at the timing level. This is the **canonical worked example** of [[trading/mechanisms/scaffold-as-mechanism|scaffold-as-mechanism]]. Verdict: novelty REJECT; mechanism research-grade pass + mutate (long-only direction restriction + sharpe-rerank pending).

## What we learned

1. **"Novel signal axis" claims need timing-correlation pre-tests.** Spec marketed taker-imbalance as informed-flow microstructure (Harris/Easley-O'Hara) — implicitly predicting low correlation with HTF-trend strategies. Data falsified the novelty claim cleanly: timing correlation +0.68 with both `confluence_trend_4h` and `ema_bias_breakout_4h_v2`. The spec didn't pre-register a falsifiable timing-correlation prediction; future specs should. **This case promotes the discipline from implicit to explicit** at companion page [[trading/mechanisms/scaffold-as-mechanism|scaffold-as-mechanism]] §3.2.

2. **Same-scaffold strategies fire in the same bars, regardless of secondary axis.** Once HTF-EMA-bias + bar-direction-aligned scaffold is present, secondary axes (vol-expansion, taker-imbalance, volume-Z) are **expressions** of the same underlying force, not distinct mechanism classes. The taker-imbalance secondary axis selects a *subset* of bars the trend scaffold also selects; selection is correlated with bar-direction-aligned breakout because both are downstream of the same regime drift.

3. **Mechanism real ≠ mechanism deployable.** Stable positive Sharpe distribution across 8,748 runs (medSh +0.82, ATP $0.36) means the mechanism IS extracting edge — it's just (a) the same edge as ema_bias_v2's $0.62 ATP, *slightly worse*, and (b) net −$55/yr after friction. ATP $0.36 below ema_bias_v2's $0.62 — taker-imbalance via HTF-bias is *slightly worse expression* of the trend mechanism than breakout via HTF-bias.

4. **Adversarial pre-registration concern was load-bearing.** The spec's adversarial-audit pre-registered "HFT capture at sub-second; 4h aggregation = residual after fast operators". The data validates exactly this: small-but-real ATP at retail-accessible 4h horizon. The mechanism's high-frequency form was already captured by HFT operators; what remains at 4h is a residual that mirrors the HTF-bias trend signal.

5. **Adversarial-audit + falsifier-discipline operational test passed.** The pre-registered concerns and falsifiers all fired *informatively* — not "didn't work" but "novelty claim collapses, mechanism real but not distinct, here's the partial truth." This is the discipline working as designed; the strategy's "failure" produced more usable knowledge than a same-shape "success" would have.

## Hypothesis (Structured Format)

```
Hypothesis:    Taker-buy ratio extreme + HTF-bias + bar-direction
               aligned trigger constitutes a distinct microstructure-
               flow signal axis vs HTF-trend scaffold strategies.

Mechanism:     Taker imbalance signals informed-flow continuation
               (Harris/Easley-O'Hara). When taker-buy ratio is at
               extreme high AND HTF bias bullish AND bar direction
               aligned bullish, informed buyers are expressing
               directional conviction; trend-continuation entry
               captures the move. Cross-axis: bar-direction-aligned
               trigger on top of HTF-bias is the trend scaffold;
               taker imbalance is the *additional* informed-flow
               microstructure axis being layered.

Observable:    Backtest grid 2025-05-03 → 2026-04-28, BTCUSDT perp 4h,
               $1000 capital, fee 0.04%. Pre-registered:
                 (a) Strategy-level: net ≥ $120/yr deployment-grade
                 (b) Hypothesis-level (paired): timing correlation
                     with `confluence_trend_4h` < +0.4 (distinct axis)

Expected:      medSh +0.6 to +0.9 if microstructure axis is distinct;
               ATP higher than HTF-trend strategies if informed-flow
               selection adds quality.
               expected_net_edge:
                 source: Harris microstructure framework + 2025-26
                         retail futures activity literature
                 value: +$60-100/yr 2025-26 (high if axis distinct)
                 independence: mechanism literature independent;
                              effect-size estimate test-window-
                              derived → research-grade-only
                              verdict default

Falsifier:     Strategy-level: net < $120/yr → deployment fail
               Novelty claim: corr(taker_buy, confluence_trend) > +0.4
                 → axis NOT distinct; same-mechanism mutation
                 (this is the load-bearing falsifier for novelty)
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp 4h, $1000, fee 0.04%)

| Metric | Value | Falsifier | Fired? |
|---|---:|---|:---:|
| Median Sharpe | **+0.82** | < 0 | NO (research-grade pass) |
| Median ATP | **+$0.36** | — | smaller than parent ($0.62) |
| %PF>1 | 70.3% | — | stable distribution |
| %val>0 | 90.1% | — | strong stability |
| corr(train, val) | +0.061 | — | low (parameter selection unreliable; sharpe-rerank mutation candidate) |
| Long ATP | +$0.58 | — | direction asymmetry detected |
| Short ATP | +$0.19 | — | 3× edge on long side |
| %both+ | 46% | — | both directions positive on average |
| trades/yr | 86.0 | — | — |
| gross/yr | +$31 | — | — |
| friction (1bp×$5K, $1/trade) | −$86 | — | — |
| **net/yr** | **−$55** | ≥ $120 | **FAIL** |

| Hypothesis-level paired falsifier (NOVELTY) | Predicted | Observed | Verdict |
|---|---|---:|:---:|
| corr(taker_buy, confluence_trend_4h) | < +0.4 (distinct axis) | **+0.677** | **FIRED** |
| corr(taker_buy, ema_bias_breakout_4h_v2) | < +0.4 (distinct axis) | **+0.704** | **FIRED** |
| corr(taker_buy, volume_z_asymmetric_4h) | should differ | +0.686 | **FAIL (redundant)** |

## Falsifier verdict: Strategy-level FIRED; novelty FIRED on 3 comparators

- **Strategy-level (deployment-grade gate)**: net −$55/yr → **FIRED**
- **Novelty claim (paired falsifier vs trend cluster)**: timing correlations all >+0.6 → **FIRED on 3 comparators**

The novelty failure is the **load-bearing** result. The strategy *works* as a strategy (research-grade) — just not as a *distinct mechanism*. **Per [[trading/mechanisms/scaffold-as-mechanism|scaffold-as-mechanism]] §3.2**: this is the canonical worked example.

## What partial truth remains?

| Component | Status | Evidence | Salvage condition |
|-----------|--------|----------|-------------------|
| **Novelty claim (distinct microstructure axis)** | ❌ Falsified | timing corr +0.68-0.70 with trend cluster | Closed — taker-imbalance secondary axis collapses to trend scaffold timing |
| **Mechanism (taker imbalance + HTF-bias) reality** | ✅ Real | medSh +0.82; %val>0 = 90%; 8,748 stable runs | Useful as alternative *expression* of HTF-trend mechanism |
| **Long-side direction asymmetry** | Partial truth retained | long ATP $0.58 vs short ATP $0.19 (3× edge) | Mutation `taker_buy_long_only_4h` (this batch) tests direction-restriction |
| **Sharpe-rerank parameter selection** | Discipline gap | corr(train, val) = +0.061 means current ranking is essentially random | Mutation: rerun with `--ranking sharpe_ratio` instead of `train_pnl_equity`; might surface stable param regions |
| **Dropped HTF-bias gate (`taker_buy_no_htf_bias_4h`)** | Untested | Tests whether taker imbalance carries *standalone* signal or only as slow-bias amplifier | Mutation candidate; if standalone signal exists, taker as regime-detection axis is novel; if not, confirms HTF-bias proxy |
| **Higher-frequency primary timeframe (1h)** | Untested | 4h is residual after HFT capture; 1h may capture more signal | Engine supports it; defaults written for 4h |

The **load-bearing partial truth**: the mechanism IS real and grid-stable. It's just the same edge as the HTF-trend scaffold expressed via taker imbalance. Mutations test (a) direction restriction and (b) standalone-without-HTF to extract whatever specific edge taker-imbalance carries beyond what the trend scaffold already captures.

## Reason classification

- [x] **Falsifier fired** — both deployment-grade AND novelty paired-falsifier
- [x] **Same-scaffold mutation** — high timing correlation with HTF-trend cluster (+0.68-0.70) confirms not a distinct mechanism; canonical [[trading/mechanisms/scaffold-as-mechanism]] worked example
- [x] **Mechanism real but not distinct** — research-grade pass on the strategy's own merits
- [x] **Partial truth: long-side direction asymmetry** (3× edge); mutations queued
- [ ] No falsifier articulated (clean pre-registration with novelty falsifier load-bearing)
- [x] **Already arbitraged at high frequency** — adversarial pre-reg concern (HFT captures sub-second; 4h is residual) confirmed
- [ ] Graveyard duplicate

## Trial count on dataset (per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] §2.2)

≥18 strategies tested on BTCUSDT perp 2025-05/2026-04 window. taker_buy adds a research-grade-pass-with-novelty-falsified result; trial count recorded.

## Caveats per [[foundation/methodology/strategy-validation-discipline|validation discipline]]

- **Single-window backtest** — 2025-05/2026-04 only. Mutation OOS test on 2024 (bullish-dominant) would test whether long-only edge persists when long is regime-aligned.
- **PSR/DSR not pipeline-implemented** — Sharpe +0.82 is uncorrected for trial-count multiplicity; with 18+ trials on dataset, DSR adjustment likely lowers significance.
- **Slippage modeling absent** — taker_buy may flip net positive under realistic measured slippage (taker-imbalance typically fires at moments of higher liquidity, lower slippage).
- **Param-selection unreliability** (corr(train, val) = +0.061) — current ranking via train_pnl_equity is essentially random; mutation rerun with sharpe_ratio ranking is required before any productionize step.

## Mutation candidates queued (per stocktake §"taker_buy" mutation list)

1. **Rerun with `--ranking sharpe_ratio`.** Current ranking by `train_pnl_equity` with corr=0.06 is essentially random selection. Sharpe-based ranking might surface stable param regions across val partition.
2. **`taker_buy_long_only_4h`.** Direction restriction; long-only on bullish HTF bias. Predicted gross ≈ $24, friction ≈ $40 → still −$16, but cleaner test of long-side edge isolated.
3. **`taker_buy_no_htf_bias_4h`.** Drop the HTF-EMA-bias gate to test whether taker imbalance carries *standalone* signal or only as a slow-bias amplifier. **Most informative mutation** for the scaffold-as-mechanism question.

## Key Takeaways

- **Novelty claim (distinct microstructure axis) FALSIFIED at timing level** — corr +0.68-0.70 with HTF-trend cluster. Canonical [[trading/mechanisms/scaffold-as-mechanism]] worked example.
- **Mechanism research-grade pass on own merits** — stable +Sharpe distribution across 8,748 runs; just not distinct from trend.
- **HFT-residual hypothesis confirmed** — adversarial pre-reg concern about 4h aggregation post-HFT-capture validated by data.
- **Long-side direction asymmetry partial-truth** — 3× edge on long side; direction-restriction mutation pending.
- **Standalone-without-HTF mutation is the most informative next test** — resolves whether taker-imbalance carries any signal independent of slow-bias scaffold.
- **Future strategy specs should pre-register falsifiable timing-correlation predictions** when claiming novel signal axes — the discipline is now wiki-encoded at companion page.

## Related

- [[trading/mechanisms/scaffold-as-mechanism]] — canonical worked example for this page; sets the framework
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — companion page; HTF-bias scaffold N=4 cluster context
- [[trading/rejected/ema-bias-breakout-4h-v2-2026-05]] — paired falsifier comparator (parent strategy); rejected separately at deployment-grade gate
- [[trading/rejected/confluence-trend-4h-2026-05]] — sibling on same scaffold (timing corr +0.677)
- [[trading/rejected/volume-z-asymmetric-4h-asymmetry-2026-05]] — sibling on same scaffold (timing corr +0.686)
- [[foundation/sources/harris-trading-exchanges]] — informed-flow microstructure literature source
- [[foundation/methodology/strategy-validation-discipline]] — Marcos' Third Law; novelty-claim discipline
- [[foundation/methodology/forecast-vs-tradeable-gap]] — companion methodology (L1→L2 translation)

## Sources

- 2026-05-05 batch stocktake: the backtesting lab §"`taker_buy_asymmetric_4h`" + §"Cross-strategy correlation matrix"
- the backtesting lab — strategy spec + pre-registration
- Harris *Trading and Exchanges* — informed-flow microstructure foundation
- Easley-O'Hara order-flow toxicity literature
