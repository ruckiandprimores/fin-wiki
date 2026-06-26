---
title: "Markov-Switching GARCH for Crypto Volatility Regimes (Ardia lineage; 292-coin study)"
status: growing
created: 2026-06-07
updated: 2026-06-07
tags: [source, academic, volatility, vol-regime, msgarch, regime-switching, one-step-ahead, prospective, vol-state-axis, day-swing-applicable]
---

# Markov-Switching GARCH for Crypto Volatility Regimes

> **TL;DR**: Markov-Switching GARCH (MSGARCH) lets volatility-process parameters switch between regimes (typically high-vol vs low-vol). The foundational crypto study (Ardia, Bluteau, Rüede 2019, *Finance Research Letters*) shows BTC vol dynamics have **regime changes** that single-regime GARCH misses; a large follow-up (Univ. of Guelph WP 2022-02) fits up to 27 GARCH specs with up to 3 regimes across **292 cryptocurrencies** and finds **two volatility regimes (high/low) exist for essentially all of them**, with MSGARCH **beating single-regime models on out-of-sample one-step-ahead VaR and density forecasts**. **Why it matters for the wiki**: this is the **volatility-state axis** the team separates from bull/bear direction — and the key property is that the vol-state is callable **one-step-ahead (prospectively)**, not just in hindsight. External empirical support (large-N) that a high-vol-vs-calm bifurcation is real and forward-callable. Adjacent support for [[foundation/methodology/online-regime-detection]]; mechanism = volatility clustering + leverage effect.

## Citation

- Ardia, D., Bluteau, K. & Rüede, M. (2019). *Regime changes in Bitcoin GARCH volatility dynamics.* Finance Research Letters. (ScienceDirect S1544612318306781 / S027553191830669X lineage.)
- 292-coin study: *On the Volatility of Cryptocurrencies*, University of Guelph Working Paper 2022-02 — https://www.uoguelph.ca/economics/repec/workingpapers/2022/2022-02.pdf
- Tooling: the `MSGARCH` R package (Ardia, Bluteau, Boudt, Catania, Trottier).

## Position in Lineage

**This source builds on:**
- GARCH (Bollerslev 1986) / regime-switching (Hamilton 1989) — combined as MSGARCH
- [[foundation/sources/sinclair-volatility-trading|Sinclair]] vol-clustering stylized facts (the wiki's general-vol reference)

**This source influenced (in this wiki):**
- [[foundation/methodology/online-regime-detection]] — Method-comparison table; the well-supported, one-step-ahead vol-state half
- [[trading/mechanisms/vol-regime-detection]] — model-based companion to the percentile vol-cone

**Concept lineage map:**
- Regime-switching volatility (high/low vol states) in crypto, large-N validated
- One-step-ahead (prospective) vol-state forecasting beating single-regime GARCH

## 3-Axis Tags

- **Economic mechanism**: Macro/regime (volatility-state classification); volatility risk premium (downstream)
- **Behavioral/structural driver**: Volatility clustering; leverage effect (forced de-risking amplifies down-vol)
- **Crypto applicability**: Direct (crypto-specific empirics; 292 coins)

## Key findings

- **Two volatility regimes (high/low) exist across essentially all 292 coins** — the high-vol-vs-calm bifurcation is not BTC-specific.
- **MSGARCH beats single-regime GARCH on out-of-sample one-step-ahead VaR + density forecasts** → the vol-state is forward-callable (prospective), the property the team needs for live gating.
- Asymmetric-vol specifications (TGARCH/EGARCH) add incremental fit but the regime-relevant content is the two-state result.

## Why it matters / honest scope

- **Vol-state axis, prospectively callable** — directly supports the team's design choice to make vol-state a *separate* axis from bull/bear direction, and the choice of one-step-ahead methods for it.
- **N=3 convergence** (with [[foundation/sources/nhhm-crypto-regime-switching|NHHM]] and [[foundation/sources/almeida-bitcoin-risk-premia|Bitcoin VRP]]) on a high-vol/calm bifurcation callable forward — see [[foundation/methodology/online-regime-detection#method-comparison]].
- **Honest scope**: MSGARCH gives a *vol-state* label, **not** bull-vs-bear *direction* (the harder frontier). Abstract/working-paper-grade extraction here; full per-spec tables not deep-read. Implementation needs a GARCH-regime fit in the lab (capability gap, not data gap — runs on existing OHLCV).

## Related

- [[foundation/methodology/online-regime-detection]] — the vol-state half of the regime backbone
- [[trading/mechanisms/vol-regime-detection]] — percentile vol-cone companion (model-free vs MSGARCH model-based)
- [[foundation/sources/nhhm-crypto-regime-switching]] — direction-axis sibling (bull/bear/calm HMM)
- [[foundation/sources/almeida-bitcoin-risk-premia]] — VRP regime-conditioning (the third convergence point)
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — realized-vol marker
- [[foundation/sources/lineage]]

## Sources

- Ardia et al. 2019 + Guelph WP 2022-02 (abstract/WP-grade extraction, 2026-06-07). Quantitative VaR tables not independently re-verified.
