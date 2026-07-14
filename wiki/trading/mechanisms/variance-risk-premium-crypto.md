---
title: "Variance Risk Premium (Crypto) — Options-Based Insurance Premium Harvest"
status: seedling
created: 2026-05-18
updated: 2026-05-18
tags: [mechanism, options, deribit, variance-risk-premium, vrp, short-vol, delta-hedge, insurance-premium-harvest, day-swing, capacity-constrained-edge, regime-conditional]
---

# Variance Risk Premium (Crypto) — Options-Based Insurance Premium Harvest

> **TL;DR**: Crypto options buyers (retail directional speculators + protection-seekers) systematically pay implied volatility above realized — averaging **~14% annualized on BTC** (Almeida, Grith, Miftachov 2024, arXiv 2410.15195), **~7× the SPX equivalent**. Delta-hedged short-vol strategies harvest this premium. Premium is **regime-conditional and counterintuitively LARGER in low-vol regimes** (LV cluster 17% vs HV cluster 12%). DVOL hit a 2-year low of 37% in 2025, placing the current regime in the favorable LV cluster. **VRP did NOT decay analogously to [[../foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class|Borri's broad-carry collapse]]** (different counterparty stack — options buyers haven't been arbed away the way leveraged longs were). Tail risk is short-gamma (vol spikes inflict losses faster than delta hedges adjust).

## Position in the wiki — extending vol-axis coverage

The wiki already has substantial vol-axis coverage that this page extends and integrates:

| Existing page | What it covers | Relation to this page |
|---|---|---|
| [[../glossary/variance-premium]] | General variance-premium concept (Sinclair Ch 4/11 quantification) | Glossary base — this page is the crypto-specific operational mechanism |
| [[trading/mechanisms/vol-regime-detection]] | IV-RV spread as regime classifier | Regime layer — informs WHEN to harvest |
| [[trading/mechanisms/skew-based-positioning-signals]] | Vol skew as positioning signal | Skew decomposes the variance premium across strikes |
| [[trading/mechanisms/options-gamma-perp-effects]] | Dealer gamma flow into perp price | Adjacent — gamma exposure as a separate options-derived mechanism |
| [[trading/mechanisms/term-structure-dynamics]] | IV term structure shape | Term structure conditioning — VRP varies by tenor |
| [[../foundation/sources/sinclair-volatility-trading]] | Sinclair primary source | Theoretical foundation — Sinclair's Ch 4 quantification is the equity-side anchor |
| **this page** | **Operational mechanism: short-vol + delta-hedge to harvest VRP at crypto scale** | New: ties Sinclair theory + Almeida 2024 empirical + 2025-26 DVOL regime data into deployable mechanism family |

## Hypothesis (Structured Format) {#structured-hypothesis}

```
Hypothesis:    Crypto options implied volatility persistently exceeds
               subsequent realized volatility on BTC and ETH Deribit options
               at ~14% annualized for BTC (Almeida et al. 2024). The
               variance premium is regime-conditional: LARGER in low-vol
               regimes (counterintuitively). Delta-hedged short-vol
               positions (short strangle / straddle + continuous delta
               hedge) capture this spread.

Mechanism:     Who pays — options buyers, structurally biased toward
               retail directional speculators (long calls in bull mania,
               long puts in protection-seeking regimes) and institutional
               hedgers (long puts for portfolio protection). Why — loss
               aversion (Bakshi-Madan 2006), lottery preference, hedging
               demand for tail-risk insurance; market makers charge a
               premium to bear that risk. When — VRP is captured
               continuously while position is held; favored regime is
               low-vol cluster (DVOL <40 historically); avoid harvesting
               when BVIX² < RV for >30 consecutive days (mechanism
               inversion per Almeida).

Observable:    Deribit BTC/ETH option chain (IV per strike + tenor);
               continuous delta-hedge requires spot or perp leg for delta;
               PnL = funding received from option sale (theta-equivalent)
               + delta-hedge P&L (small) − gamma P&L on realized vol −
               vega P&L on IV changes − fees.

Expected:      Sharpe ~0.8 on BTC delta-hedged short straddle (Almeida
               academic backtest, 2017-2022 sample); 18.5% cumulative
               returns / Sharpe 2.1 in March 2025 hybrid-ML simulation
               (Springer Discover Analytics 2025).
               PLUS expected_net_edge:
                 - source: Almeida, Grith, Miftachov (2024 arXiv) BTC VRP
                          = 0.14 annualized; SPX equivalent ~0.02 (sample
                          window Jul 2017 - Dec 2022; fully independent of
                          any 2026 H2 deploy window)
                          + Coindesk Aug 2025 DVOL 37% (independent
                          empirical anchor confirming current regime is LV
                          cluster favorable for harvest)
                 - value: 8-15% gross APR net of friction at retail-mid
                          scale; Sharpe 0.8-2.0 range subject to
                          delta-hedge frequency + venue cost structure
                 - independence: confirmed — Almeida sample window
                          predates intended deploy by ~3 years; DVOL is
                          public price-level observation, no overlap with
                          any backtest window
               Research-grade for the wiki; deployment-grade verdict
               requires capability investment (Deribit feed + delta-hedge
               execution) per this research program capability roadmap.

Falsifier:     (a) BVIX² < RV for >30 consecutive days on BTC (Almeida
               criterion) → premium has inverted; pause short-vol. (b)
               Realized Sharpe <0.5 over 60+ consecutive trading days at
               deployment scale → mechanism failure or friction
               miscalibration; reassess. (c) Skew-decomposition flip
               sustained (OTM-call premium > OTM-put premium for >60 days)
               → indicates entered euphoria-stage bull mania; tail risk
               extreme; require risk-control overhaul before continuing.
```

## The premium — empirical magnitudes {#empirical-magnitudes}

### BTC vs SPX baseline

Per **Almeida, Grith & Miftachov (Oct 2024 arXiv, sample Jul 2017 – Dec 2022, 7.8M option transactions on Deribit)**:

| Asset | Annualized Variance Premium | Notes |
|---|---|---|
| **BTC** | **0.14** | Risk-neutral variance 0.72 vs realized variance 0.58 |
| SPX equivalent | ~0.02 | Equity-options benchmark from same methodology |
| **BTC / SPX ratio** | **~7×** | Revise upward from earlier 3-5× estimates |

### Regime conditioning — the counterintuitive finding {#regime-conditional}

| Regime cluster | BVIX² (risk-neutral) | RV (realized) | Premium | Notes |
|---|---|---|---|---|
| High-vol (HV) | 0.86 | 0.74 | **0.12** | Smaller relative premium despite higher absolute spread |
| Low-vol (LV) | 0.46 | 0.29 | **0.17** | **Larger relative premium** because risk-neutral / realized ratio widens at low vol |

**Why this matters operationally**: The intuitive expectation ("high IV = more premium to harvest") is wrong. The mechanism's edge is **largest when vol is low** — when option premiums look "cheap" in absolute terms but rich relative to realized.

This mirrors but is sharper than Sinclair's equity finding ([[../glossary/variance-premium]]): SPX VIX < 20 had 27.75% spread vs VIX > 50 having -11.52%. Same shape, larger magnitude in crypto.

### Current regime (Q1 2026) — favorable for harvest {#current-regime}

- **DVOL hit a 2-year low of 37%** in August 2025 (Coindesk Aug 2025) — places market in LV cluster (premium 0.17)
- Cumulative VRP harvest in Q1 2026 would be operating in the regime where Almeida finds the largest spread
- **2025 hybrid-ML simulation** (Springer Discover Analytics 2025): 18.5% cumulative returns, Sharpe 2.1 — March 2025 backtest window
- Quant Buffet 2024 BTC delta-hedged short straddle implementation: "*the risk-premia observed in options on Deribit is negative and significant, so that strategies selling volatility are expected to generate positive risk-adjusted performance in the long-term*"

## Why VRP did NOT decay analogously to broad-carry {#vrp-vs-broad-carry}

**The critical regime-decay framing**: [[../foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class#carry-trajectory|Borri et al.'s (2025)]] broad-carry trajectory 6.45 → 4.06 → NEGATIVE in 2025 is a wiki-level alarm for ANY crypto premium-harvesting mechanism. Naive expectation: VRP decays the same way. **Reality (per research as of 2026-05-18)**: VRP did NOT decay analogously. Three reasons:

1. **Different counterparty stack.** Broad-funding-rate carry pays because leveraged longs collectively over-pay shorts. ETF-era flows (cash-and-carry by institutions arbing GBTC/IBIT vs spot) compressed that. VRP pays because options *buyers* (retail directional speculators + protection-seekers) systematically overpay sellers (market makers, vol funds). The buyer cohort has not been arbed away the same way — Deribit's options market is still 85-90% concentrated on one venue; the buyer base is still dominantly retail-directional.

2. **DVOL hit a 2-year low of 37% in 2025**, placing the market squarely in the LV regime where Almeida shows VRP is HIGHEST (0.17). If anything, the regime in 2025-26 is *favorable* for VRP harvest, not adverse.

3. **Practitioner backtests confirm.** Quant Buffet's BTC delta-hedged short straddle and the Springer 2025 hybrid-ML strategy both report positive risk-adjusted performance in 2025 — distinct from broad-carry's 2025-negative.

**Wiki-level discipline implication** {#non-transfer-discipline}: regime-decay findings on one premium-harvesting mechanism family do NOT automatically transfer to adjacent families. Counterparty-stack analysis is the discriminator. This is N=1 worked example of the principle; watch for N=2 codification trigger.

## The construction — operational design {#construction}

### Basic forms

| Variant | Position | Delta-hedge | Notes |
|---|---|---|---|
| **Short straddle** | Short ATM call + short ATM put, same strike + tenor | Continuous via perp/spot | Maximum theta capture; symmetric tail risk |
| **Short strangle** | Short OTM call + short OTM put, different strikes | Continuous via perp/spot | Smaller premium / wider protection band; preferred for higher-vol regimes |
| **Short OTM puts** | Short OTM puts only | Continuous via perp/spot | Captures OTM-put-skewness component specifically per [[../glossary/variance-premium#three-components]] |
| **Short OTM calls** | Short OTM calls only | Continuous via perp/spot | Captures bull-euphoria lottery premium when skew-decomposition flips (see §regime applicability) |

### Friction breakdown {#friction-breakdown}

| Component | Magnitude | Notes |
|---|---|---|
| Option sale premium (gross) | Theta-equivalent ~$5-20/contract/day depending on tenor | Smallest component daily but cumulative is the edge |
| Delta-hedge transaction cost | 0.02-0.05% per hedge × hedges/day | Higher delta-hedge frequency → more friction; tradeoff |
| Gamma P&L on realized vol | Negative on realized > IV; positive on realized < IV | The actual P&L source — Sharpe depends on this |
| Vega P&L on IV changes | Negative if IV rises; positive if IV falls | Acts as carry around the gamma core |
| Deribit fees | 0.03-0.05% per side typically | Lower for market-makers |
| Spread / slippage | 1-5% on illiquid strikes; 0.1-0.5% on majors | Concentrate on BTC/ETH high-OI strikes |

### Hedge cadence — the operational tradeoff {#hedge-cadence}

Continuous delta-hedge is theoretically optimal but practically expensive. Discrete cadence (every 4h, every 8h, on Δ-drift threshold) trades hedge cost vs gamma slippage.

- **8h cadence** (aligned with funding settlements): rough institutional baseline
- **4h cadence**: Sinclair-style equity-options-derived default
- **Δ-drift threshold (e.g., |Δ| > 0.1)**: dynamic — fewer hedges in low-vol, more in high-vol

**Open question** {#hedge-cadence-open}: optimal cadence for crypto Deribit specifically vs Sinclair's equity-options frame. Unknown without lab backtest.

## Failure modes — tail risk {#failure-modes}

### Short-gamma blowup

The canonical VRP failure mode: vol spikes faster than delta-hedge adjusts; gamma P&L compounds losses. Almeida explicitly notes:

> *"During March 15 – April 8, 2020, realized variance surpassed BVIX², creating a negative variance risk premium period."*

In that window, a delta-hedged short straddle would have produced material drawdowns. The 2026 equivalent windows:

- **Oct 10-11 2025 cascade** (per [[liquidation-cascade-aftermath]] §7): IV spiked faster than delta hedges; gamma loss accumulates
- **Feb 2026 Bybit hack window**: same dynamic, different trigger

**Discipline implication**: Even with the falsifier (a) circuit breaker (>30 days BVIX² < RV), short windows of acute vol-spike loss are unavoidable. Position-size the strategy assuming the worst window (10-30% drawdown in 2-5 days) is in the deploy window.

### Venue concentration

- **Deribit ~85-90% of crypto options market share** — single-venue tail risk
- No equivalent venue diversification possible for options today (Bit.com / CME crypto options have far smaller liquidity)
- Practical implication: VRP-only is implicitly a Deribit-concentration bet. Compose with [[delta-neutral-funding-harvest]] (multi-venue) to reduce systemic venue risk.

### Sinclair-style 2008-analog wipeout

Sinclair's quantification (equity-options): VRP > 50 had spread **−11.52%** — inverted; vol-sellers got destroyed. The crypto equivalent (sustained DVOL > 100 with realized > 100) hasn't been observed cleanly post-2021 cycle peak, but the structural mechanism is the same.

## Implications for strategy design

### What this mechanism IS

- **Options-based insurance-premium harvest** — paid by directional speculators + protection-buyers; captured by delta-hedged short-vol
- **Capacity-constrained edge** per a standing convention — Deribit options market ~$30B BTC+ETH combined; institutional vol funds have absorbed some but premium hasn't compressed to equity levels
- **Regime-conditional** with LV-favorable counterintuitive finding (premium 0.17 in low-vol vs 0.12 in high-vol)
- **Day/swing scope** — position holding typically days to weeks; SL/TP horizon dictated by gamma/vega exposure decay

### What this mechanism is NOT

- **Not the same as delta-neutral funding harvest** — see [[insurance-premium-harvest-overview#vrp-vs-funding-distinction|parent overview §distinction]]. Different counterparties + instruments + convexity
- **Not the same as broad-carry** — different counterparty stack; Borri 2025-negative doesn't transfer (see §vrp-vs-broad-carry)
- **Not a vol-direction bet** — VRP is the spread, not the level. A position long-vol or short-vol pre-event is a different mechanism

### Counter-evidence sought (open questions) {#open-questions}

- **Does the LV-favorable regime conditioning hold post-2022 in fresh sample?** Almeida sample is 2017-2022. Re-run on 2023-26 data to confirm the LV cluster premium > HV cluster premium pattern holds.
- **What's the empirical Sharpe at delta-hedge cadence range (1h / 4h / 8h / Δ-threshold)?** Unknown without lab backtest. Hedge cost vs gamma slippage tradeoff is the operational core.
- **Sinclair-Almeida reconciliation** — Sinclair's GARCH-grounded vol-regime framing (Ch 11) and Almeida's LV/HV cluster framing both inform when carry is favorable. Are they describing the same regimes? Cross-walk pending.
- **Skew-decomposition flip threshold** — at what sustained OTM-call > OTM-put premium spread does bull mania become tail-risk-extreme? Sinclair-style equity threshold may not translate.
- **Does Pendle Boros term structure on funding correlate with DVOL term structure?** If yes, the two insurance-premium families share a forward-looking regime signal (operational implication for portfolio composition).

## Honest caveats {#caveats}

- **Capability gap is real**: Deribit feed + delta-hedge execution + options-Greek computation are all currently outside the lab's day/swing-perp toolkit. This page documents the mechanism; deployment requires capability investment.
- **Almeida sample 2017-2022** — 4-year vintage; 2023-26 data not yet reflected in academic peer-review. Practitioner anecdotes (Quant Buffet, Springer 2025) suggest premium persists but rigorous post-2022 academic confirmation pending.
- **DVOL 2-year low** (Aug 2025) means we're at the FAVORABLE end of the regime distribution. A vol regime change (DVOL > 60 sustained) would shift the operating point — though Almeida shows positive premium even in HV regime.
- **Counterparty concentration**: Deribit at 85-90% market share is venue tail risk. Limited diversification possible until Bit.com / CME crypto options scale.
- **Short-gamma is fundamentally short-convexity** — pays steady, loses big. The Sharpe target should be earned, not assumed; expect 10-30% drawdowns in tail windows.
- **N=1 regime-decay non-transfer finding** per a standing convention. The "VRP did NOT decay like broad-carry" claim is one worked example; watch for N=2 confirmation (e.g., next major adverse-event window) before promoting to standard discipline.

## Regime applicability

| Regime | Mechanism status |
|---|---|
| Low vol / DVOL <40 (current Q1 2026) | **Favorable** — LV cluster premium 0.17; hedge cost low |
| High vol / DVOL 40-80 | Smaller premium (0.12) but still positive; hedge cost rising |
| Crisis / DVOL >100 | Pause per Almeida criterion if BVIX² < RV for >30 days |
| Bull mania / skew-decomposition flip | Caution — OTM-call premium > OTM-put premium signals lottery-buying regime; tail risk extreme |
| Post-cascade aftermath | Premium may have inverted briefly; wait for vol normalization |

## Adjacent mechanisms — distinction

### vs. [[delta-neutral-funding-harvest]]

Sister insurance-premium family. Different counterparties (options buyers vs leveraged longs), different instruments (options vs perps), different convexity (short-gamma vs linear), different tail behavior (vol-spike blowup vs venue/oracle failure). See [[insurance-premium-harvest-overview]] for the cross-mechanism comparison table.

### vs. [[vol-regime-detection]]

Vol-regime-detection is the regime classifier; this page is the harvest mechanism that USES the regime signal. They compose: vol-regime-detection identifies LV vs HV; this page exploits the LV cluster's 0.17 premium.

### vs. [[skew-based-positioning-signals]]

Skew decomposes the variance premium across strikes (ATM vs OTM-call vs OTM-put). This page is the umbrella mechanism family; skew-based-positioning-signals is the strike-decomposition layer that informs WHICH variant (short straddle vs short OTM puts vs short OTM calls) is optimal in a given regime.

### vs. [[options-gamma-perp-effects]]

That page covers dealer gamma flow leaking into perp price (gamma-pinning, post-expiry gamma flush). This page covers the harvest of variance premium independent of perp-price effects. The two are adjacent but distinct — gamma flow is a *signal* in the perp market; VRP is a *yield* in the options market.

### vs. [[../glossary/variance-premium]]

Glossary entry is the general concept (Sinclair Ch 4/11 quantification on SPY). This page is the crypto operational mechanism (Almeida 2024 + DVOL 2025 + Deribit-specific construction).

## Operationalization status

| Component | Wiki encoded | Lab implemented |
|---|---|---|
| Mechanism definition | ✅ this page | — |
| Deribit options data feed | ✅ wiki encoded | ❌ capability gap |
| Delta-hedge execution | ✅ wiki encoded | ❌ capability gap |
| Options Greek computation | ✅ wiki encoded | ❌ capability gap |
| DVOL regime classifier | ✅ wiki encoded | ❌ feed not in lab |
| Regime-conditioned harvest (LV vs HV) | ✅ §regime-conditional | ❌ no live test |
| Live deployment | — | ❌ capability gap |

## Sources

**Primary academic:**
- **Almeida, Grith, Miftachov (2024)** — "Risk Premia in the Bitcoin Market" — arXiv 2410.15195v2; sample Jul 2017 – Dec 2022, 7.8M Deribit option transactions; **single most rigorous academic source for crypto VRP** ([arxiv.org/html/2410.15195v2](https://arxiv.org/html/2410.15195v2))
- Alexander & Imeraj — "The Bitcoin VIX and Its Variance Risk Premium" (SSRN 3383734) — earlier BVIX construction methodology ([papers.ssrn.com/sol3/papers.cfm?abstract_id=3383734](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3383734))
- Du (2025) — "Pricing Cryptocurrency Options With Volatility of Volatility" — Journal of Futures Markets ([onlinelibrary.wiley.com/doi/10.1002/fut.70029](https://onlinelibrary.wiley.com/doi/10.1002/fut.70029))

**Practitioner / industry:**
- **Deribit Insights — "Bitcoin Options: Finding Edge in Four Years of Volatility Regimes"** — best applied piece on VRP regime-conditioning ([insights.deribit.com/industry/bitcoin-options-finding-edge-in-four-years-of-volatility-regimes/](https://insights.deribit.com/industry/bitcoin-options-finding-edge-in-four-years-of-volatility-regimes/))
- Quant Buffet — "Delta-Hedged Short Straddle BTC Strategy" (2024) — practitioner-implementation reference ([quantbuffet.com/2024/04/17/delta-hedged-short-straddle-btc-strategy/](https://quantbuffet.com/2024/04/17/delta-hedged-short-straddle-btc-strategy/))
- Springer Discover Analytics 2025 — "Hybrid ML and Stochastic Volatility for Crypto Trading" — 18.5% cumulative return / Sharpe 2.1 March 2025 backtest ([link.springer.com/article/10.1007/s44257-025-00046-1](https://link.springer.com/article/10.1007/s44257-025-00046-1))
- Coindesk Aug 2025 — DVOL 2-year low at 37% ([coindesk.com/markets/2025/08/15/bitcoin-and-strategy-lead-on-risk-adjusted-returns-as-volatility-falls](https://www.coindesk.com/markets/2025/08/15/bitcoin-and-strategy-lead-on-risk-adjusted-returns-as-volatility-falls))
- Sharpe2 — "Short Strangle Strategy: VRP-Calibrated Entry, Sizing, Management" ([sharpetwo.com/blog/short-strangle-strategy/](https://sharpetwo.com/blog/short-strangle-strategy/))
- Quantpedia — "Volatility Risk Premium Effect" strategy entry ([quantpedia.com/strategies/volatility-risk-premium-effect](https://quantpedia.com/strategies/volatility-risk-premium-effect))

**Theoretical foundation (equity-options):**
- [[../foundation/sources/sinclair-volatility-trading]] — Sinclair Ch 4 (quantification) + Ch 11 (decomposition) — equity-side anchor
- Coval, J., Shumway, T. (2001) — *Expected option returns*. Journal of Finance.
- Bakshi, G., Madan, D. (2006) — *A theory of volatility spreads*. Management Science.
- Carr, P., Wu, L. (2009) — *Variance risk premiums*. Review of Financial Studies.
- Kozhan, R., Neuberger, A., Schneider, P. (2011) — *The skew risk premium in the equity index market*.

**DVOL / Deribit infrastructure:**
- Deribit Insights — "DVOL — Deribit Implied Volatility Index" ([insights.deribit.com/exchange-updates/dvol-deribit-implied-volatility-index/](https://insights.deribit.com/exchange-updates/dvol-deribit-implied-volatility-index/))
- The Block — "BTC DVOL Index Variance Premium" ([theblock.co/data/crypto-markets/options/btc-dvol-index-variance-premium](https://www.theblock.co/data/crypto-markets/options/btc-dvol-index-variance-premium))
- Deribit Insights — "Demystifying DVOL Futures" ([insights.deribit.com/industry/demystifying-dvol-futures/](https://insights.deribit.com/industry/demystifying-dvol-futures/))
- Glassnode — "Taker-Flow-Based Gamma Exposure" ([insights.glassnode.com/gamma-exposure/](https://insights.glassnode.com/gamma-exposure/))

## Related

- [[insurance-premium-harvest-overview]] — parent / index; one-screen comparison vs delta-neutral funding harvest
- [[delta-neutral-funding-harvest]] — sibling: perp-based insurance-premium harvest
- [[vol-regime-detection]] — regime classifier (informs WHEN to harvest)
- [[gamma-exposure-regime]] — **added 2026-06-07**: VRP is largest in low-vol (often positive-GEX) regimes; dealer-gamma regime conditions vol-state
- [[skew-based-positioning-signals]] — strike decomposition (informs WHICH variant)
- [[term-structure-dynamics]] — VRP by tenor
- [[options-gamma-perp-effects]] — adjacent options mechanism (gamma flow into perp price)
- [[../foundation/sources/almeida-bitcoin-risk-premia]] — **added 2026-06-07**: the backbone source (BTC VRP ~14%; LV>HV regime-conditioning) now catalogued as a source page
- [[../foundation/sources/deribit-insights]] — **added 2026-06-07**: crypto-native vol-regime + DVOL research (contango ~77.5%; VRP ~+15 vol pts); practitioner companion to Almeida
- [[../foundation/methodology/regime-marker-data-sources]] — **added 2026-06-07**: where the Deribit/DVOL options data sits (paid capability) vs the free backtestable marker core
- [[../foundation/sources/sinclair-volatility-trading]] — theoretical foundation
- [[../foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class]] — broad-carry trajectory (does NOT transfer here; documented in §vrp-vs-broad-carry)
- [[../foundation/methodology/deployment-edge-threshold]] — Gate 3 economic significance; VRP must clear net-of-friction floor
- [[../foundation/methodology/small-sample-validation-discipline]] — N considerations for backtest validation
- [[../glossary/variance-premium]] — general glossary concept (companion)
- [[../glossary/vol-cone]] — companion glossary
- [[../glossary/iv-term-structure]] — companion glossary
- [[../glossary/vol-skew]] — companion glossary
