---
title: "Does the Fear & Greed Index Predict Bitcoin? A Contradiction Pair (Gessie 2026 vs Cavalheiro et al. 2024)"
status: growing
created: 2026-07-12
updated: 2026-07-12
tags: [source, academic, sentiment, fear-greed, contradiction-pair, granger-causality, var, behavioral, day-swing-applicable]
---

# Does the Fear & Greed Index Predict Bitcoin? A Contradiction Pair (Gessie 2026 vs Cavalheiro et al. 2024)

> **TL;DR**: Two peer-reviewed papers reach **opposite Granger-causality verdicts** on the Crypto Fear & Greed Index (FGI) as a Bitcoin predictor. **Gessie (2026)** — 2018–2025 **daily** data, N=1,924, VAR(8), *with an out-of-sample test* — finds ∆FGI does **not** Granger-cause returns and adds **no OOS forecasting gain** (OOS R² ≈ −1.1%), while returns **strongly** Granger-cause ∆FGI (F=152.5). **Cavalheiro et al. (2024)** — **weekly** data over a **single year** (Jul 2022–Jun 2023), in-sample only — find the FGI *does* predict BTC/ETH returns. The pair resolves cleanly on sample length, frequency, levels-vs-changes, and the presence/absence of an OOS test: **net reading — F&G is REACTIVE CONTEXT, not a standalone signal.** This page is a deliberate contradiction pair per the wiki's Knowledge Evolution Protocol.

## Citation

**Paper A (the null + reverse-causality result — VERIFIED, full text read 2026-07-12; open access CC BY):**
Louis Gessie (Shanghai University of Finance and Economics), *Do bitcoin returns move sentiment? Evidence from the crypto fear & greed index*, **Finance Research Open**, Vol. 2, Issue 1, 100094 (March 2026; online Jan 13 2026). DOI [10.1016/j.finr.2026.100094](https://doi.org/10.1016/j.finr.2026.100094). Single author; CRediT lists sole authorship.

**Paper B (the counterpoint — REPORTED, abstract + secondary only; full text paywalled):**
Everton Anger Cavalheiro (Univ. Federal de Pelotas), Kelmara Mendes Vieira (Univ. Federal de Santa Maria), Pascal Silas Thue (Univ. Federal de Pelotas), *The impact of investor greed and fear on cryptocurrency returns: a Granger causality analysis of Bitcoin and Ethereum*, **Review of Behavioral Finance**, Vol. 16, No. 5, pp. 819–835 (2024). DOI [10.1108/RBF-08-2023-0224](https://doi.org/10.1108/RBF-08-2023-0224).

## Position in lineage

First formal academic evidence in the wiki on the **composite-sentiment marker itself** — until now the FGI appeared only as a *data feed* ([[foundation/methodology/regime-marker-data-sources]] §1, alternative.me) and as a display row on the live operational BTC state card (outside this wiki). This pair calibrates **how that row should be read**. It also directly advances an open question already logged in [[foundation/methodology/regime-marker-data-sources]]: *"Does alternative.me F&G add signal beyond its components?"* — Gessie answers the next-day-return half: **no** (and the composite is partly mechanical: its 25% volatility block reads vol spikes as "fear" even on up days). Consistent with the [[foundation/methodology/online-regime-detection]] stance that prospective *direction* is the hard part — sentiment turns out to be a trailing description of state, not a lead on side. Sibling source page: [[foundation/sources/btc-return-predictability-141]] (141-predictor OOS sweep where sentiment is likewise not among the winners).

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: behavioral exploitation (sentiment as tradable information — tested and, at daily horizon, rejected).
- **Axis 2 — Driver**: cognitive bias — herding/recency (the FGI aggregates crowd fear/greed; the evidence says the crowd gauge *follows* price, i.e., recency in the index itself).
- **Axis 3 — Crypto applicability**: **direct** — the FGI is a crypto-native composite; both papers test it on BTC (B adds ETH).

## Key findings — Paper A (Gessie 2026) [VERIFIED — full text read]

- **Design**: daily log returns (CoinMarketCap close) vs **changes** in the index (∆FGI, alternative.me API), 2018-02-02 → 2025-09-05, strict business-day intersection with ∆VIX/∆EPU controls → **N=1,924**. Bivariate VAR, AIC lags p=8 (BIC-robust). Granger read as lead–lag, not structural causality. Changes (not levels) are used precisely because the FGI mechanically embeds price/volume/volatility.
- **Granger (daily)**: ∆FGI → returns **F=1.085, p=0.37** (null holds); returns → ∆FGI **F=152.5, p<0.001**. Weekly aggregation: p=0.61 vs p=0.02 — same asymmetry.
- **Magnitude (IRF)**: a 1σ return shock moves ∆FGI sharply next day (≈ −128.5 index-point swing at t=1, small +15.2 rebound at t=2, gone within a week). A ∆FGI shock has economically negligible effects on returns (CIs straddle zero at all horizons). The perverse day-1 sign is mechanical: the index's **25% volatility component reads a vol spike as "fear" even on an up day**.
- **Variance shares (FEVD, 10-day)**: ∆FGI explains **~0.4%** of return variance; returns explain **~36–37%** of ∆FGI variance.
- **Out-of-sample (the decisive layer)**: expanding window from t=600, one-step-ahead; AR benchmark MSE 1.0817×10⁻³ vs AR+∆FGI 1.0934×10⁻³ → **OOS R² ≈ −1.1%**; Diebold–Mariano finds no accuracy gain. Adding sentiment makes forecasts *slightly worse*.
- **Robustness**: weekly aggregation, ∆VIX/∆EPU controls, high- vs low-VIX regimes, positive/negative-return asymmetry (Wald p=0.26) — conclusion unchanged. **One fragile exception**: the **pre-2022 subsample shows a small ∆FGI → returns signal (p≈0.03)** that vanishes post-2021 (p=0.88), under BIC lags, and at weekly frequency. This exception matters for reading Paper B (below).
- **Author's own framing**: at the daily horizon the FGI is *"a thermometer of risk appetite that responds to price innovations rather than a thermostat that sets them."* Scope limits stated in-paper: applies to **composite** indices at **daily** frequency; text-based sentiment at intraday horizons is a different animal (Guégan & Renault 2021 find StockTwits tone matters up to ~15 min and dies at daily).

## Key findings — Paper B (Cavalheiro et al. 2024) [REPORTED — abstract + publisher summary only]

- **Design**: **weekly** data, **July 2022 – June 2023** (≈52 observations per asset), BTC + ETH; Granger causality plus Smooth Quantile Regression; cointegration tests.
- **Headline**: "the FGI notably predicts the returns of Bitcoin and Ethereum" — associations at specific lag intervals (the exact lags are behind the paywall; not verifiable this pass).
- **Nuance the abstract itself carries**: the authors also report a **feedback loop** between FGI and returns (i.e., bidirectional influence — partially *agreeing* with Gessie's returns→sentiment leg), that nonlinear Granger tests found **no** nonlinear structure, and that cointegration implies long-run co-movement (consistent with levels-based analysis).
- **Flag**: the characterization of this paper as finding prediction "at 1-day-to-1-week horizons" circulates in secondary descriptions but could **not** be confirmed — all accessible material says *weekly* frequency. Treat horizon claims finer than weekly as unverified.

## The contradiction and how to read it

Explicit contradiction, flagged per the Knowledge Evolution Protocol: **B says the FGI predicts returns; A says it does not and shows the causality runs the other way.** Resolution does not require either paper to be wrong:

1. **Sample window**: B covers a single year (Jul 2022–Jun 2023 — the post-FTX chop-and-recovery) with ~52 weekly points; A covers 7.5 years daily (N=1,924) *including* B's window. Small-window Granger rejections are exactly the kind of regime-local artifact A's own subsample analysis demonstrates (its pre-2022 p≈0.03 blip that dies in every robustness variation). Rolling-window work cited by A (Gaies et al. 2023) independently shows the FGI↔BTC direction-of-influence **flips across regimes** — so a one-year window can honestly find what a multi-year window honestly rejects.
2. **Levels vs changes**: B's cointegration framing implies FGI *levels*; A deliberately uses ∆FGI because the index mechanically embeds price/vol/volume — levels-based "prediction" partly re-discovers the index's own construction.
3. **Horizon/frequency**: B is weekly; A is daily with weekly robustness (where A still finds nothing). Neither tests intraday, where text-based sentiment evidence is strongest.
4. **The OOS asymmetry is the tiebreaker**: A runs a true out-of-sample forecast comparison and finds *negative* incremental value; B (from accessible material) is in-sample only. Per the wiki's own validation discipline, an in-sample Granger pass without OOS confirmation is a research-grade observation, not a signal.

**Net reading (the position this wiki adopts): the Fear & Greed Index is REACTIVE CONTEXT — a trailing, partly mechanical description of what price and volatility just did — with weak-to-no standalone predictive signal at daily-and-slower horizons. Contrarian value at extremes (buy extreme fear / sell extreme greed) is a separate, popular claim that NEITHER paper tests as a conditional trading rule — it remains unproven, not disproven.** The evidence-grade on the extremes claim stays open; a proper test would condition on extreme readings only and measure net-of-friction forward returns.

## Why it matters / honest scope

- **Calibrates the state card**: the live operational BTC state card (outside this wiki) carries an F&G row sourced from the state monitor's context block. This pair says that row is being used *correctly* — as a descriptive CONTEXT line, never blended into the causal card, never a side signal. The evidence now backs the design choice: **read F&G as "what the crowd gauge says after the move," not "what happens next."** Consistent with the wiki-wide prior that prospective direction is unpredictable — gate state/risk, not side ([[foundation/methodology/online-regime-detection]]).
- **Answers half an open question**: [[foundation/methodology/regime-marker-data-sources]] asked whether the composite adds signal beyond its components — for next-day BTC returns, no (and the composite's vol block double-counts a marker already tracked).
- **Honest scope**: Paper A is single-author, in a **new journal** (Finance Research Open launched 2025 — thin citation track record), though open-access with full methodology visible and internally consistent; its conclusions are explicitly limited to composite indices at daily frequency. Paper B is peer-reviewed in an established journal but was only accessible at abstract level — its exact lags, coefficients, and any OOS content could not be checked. The contradiction resolution above is this wiki's synthesis, not either paper's claim. Neither paper addresses intraday text-sentiment, funding/positioning data, or extremes-conditional rules.

## Related

- [[foundation/methodology/regime-marker-data-sources]] — the alternative.me F&G feed this evidence grades; open question partially answered
- The live operational BTC state card (outside this wiki) — whose sentiment CONTEXT row this calibrates
- [[foundation/sources/btc-return-predictability-141]] — companion 141-predictor OOS sweep (trend-technical wins; sentiment not among winners)
- [[foundation/methodology/online-regime-detection]] — prospective-detection methods; the "describe state, don't call side" frame this evidence supports
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — regime dependence of sentiment↔price direction (Gaies et al. rolling-Granger point)

## Sources

- [ScienceDirect — Finance Research Open 2(1) 100094](https://www.sciencedirect.com/science/article/pii/S305070062600006X) (Gessie 2026, open access CC BY) — **full text read 2026-07-12** via browser (WebFetch blocked); all Paper-A statistics quoted from the article body.
- [Emerald — Review of Behavioral Finance 16(5) 819–835](https://doi.org/10.1108/RBF-08-2023-0224) (Cavalheiro, Vieira & Thue 2024) — **abstract + publisher summary only** (paywalled); no accessible preprint found this pass.
- Crossref + OpenAlex metadata cross-check 2026-07-12 (authors, affiliations, journal identities, dates).
