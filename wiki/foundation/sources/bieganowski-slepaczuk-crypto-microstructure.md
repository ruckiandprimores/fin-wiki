---
title: "Explainable Patterns in Cryptocurrency Microstructure (Bieganowski & Ślepaczuk 2026)"
status: growing
created: 2026-06-26
updated: 2026-06-26
tags: [source, academic, preprint, market-microstructure, order-flow-imbalance, adverse-selection, explainability, shap, foundation-methodology]
---

# Explainable Patterns in Cryptocurrency Microstructure (Bieganowski & Ślepaczuk 2026)

> **TL;DR**: A SHAP-explainable study of crypto perpetual-futures microstructure: **order-flow imbalance (OFI), spread, and adverse selection** are the dominant predictors of short-horizon returns, with feature rankings and partial-effect shapes that the authors find **stable across five assets** spanning an order of magnitude in market cap. Gives a *testable, interpretable* order-book-footprint mechanism grounded in microstructure theory. Research-grade (preprint), sub-second/seconds horizon — a microstructure *concept*, not a turnkey day/swing signal.

## Citation

Bartosz Bieganowski & Robert Ślepaczuk (Univ. of Warsaw), *Explainable Patterns in Cryptocurrency Microstructure*, arXiv:2602.00776 (Feb 2026). [arxiv.org/abs/2602.00776](https://arxiv.org/abs/2602.00776).

## Position in lineage

The empirical/explainable crypto counterpart to [[bouchaud-trades-quotes-prices|Bouchaud's]] OFI-and-impact theory, and companion to [[wang-crypto-lob-inputs|Wang's LOB-inputs preprint]]. OFI is the modern, measurable form of the [[../market-wisdom/tape-reading|tape-reading]] footprint (Neill) — inferring large-flow intent from the order book.

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: microstructure / order-flow (OFI) + the adverse-selection cost of providing liquidity.
- **Axis 2 — Driver**: information asymmetry (informed flow leaves an OFI footprint).
- **Axis 3 — Crypto applicability**: **direct (crypto perp LOB) but HFT-horizon** — 1-second granularity / short-horizon returns; transfers as an order-book-footprint *concept*, not a ≤1-week signal.

## Key findings

- **Data**: Binance Futures perpetuals — **BTC, LTC, ETC, ENJ, ROSE** (spanning ~1 order of magnitude in market cap), **1-second** granularity, **Jan 2022 – Oct 2025**.
- **Mechanism**: order-flow imbalance, spread, and adverse selection predict short-horizon returns; OFI has a largely **monotone, concave-at-the-extremes** partial effect; wider spreads → diminished predictability; VWAP-to-mid asymmetry.
- **Method**: CatBoost pipeline with a direction-aware GMADL objective + time-series CV; **SHAP** for feature importance / dependence shapes; taker + maker backtests; a flash-crash case revealing systemic trading risk.
- Authors report **feature rankings + partial effects are stable across assets** despite heterogeneous liquidity/volatility.

## Why it matters / honest scope

OFI-as-footprint is the rigorous, explainable form of "read the tape for big-player intent" — and the SHAP partial-effect shapes make it falsifiable (a monotone-concave OFI→return curve is a testable claim). **Honest scope (verifier-flagged):** single-author-lab **preprint**; the predictive horizon is **sub-second/seconds (HFT)**, shorter than the wiki's day/swing branch — label as microstructure reference, not a turnkey signal. The authors' **cross-tier single-model transferability** framing was **refuted (1-2)** by the adversarial pass — the *stability of feature rankings* is the supported claim; a single transferable model across market-cap tiers is not. Pair with [[cong-crypto-wash-trading|wash-trading]] (OFI on a fake-volume venue is contaminated).

## Related

- [[bouchaud-trades-quotes-prices]] — OFI + impact theory
- [[wang-crypto-lob-inputs]] — companion crypto-LOB preprint
- [[../market-wisdom/tape-reading]] — OFI = the modern tape footprint
- [[../../trading/mechanisms/volume-analysis]] / [[../../trading/mechanisms/on-chain-mechanisms]] — flow-based crypto signals
- [[cong-crypto-wash-trading]] — clean-tape prerequisite for any OFI read

## Sources

- [arXiv:2602.00776](https://arxiv.org/abs/2602.00776) (abstract verified in-session 2026-06-26).
- Deep-research adversarial pass (2026-06-26): OFI→short-horizon-return mechanism confirmed 3-0; cross-tier single-model transferability **refuted 1-2**.
