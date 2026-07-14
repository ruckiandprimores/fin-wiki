---
title: "Better Inputs > Deeper Nets in Crypto LOBs (Wang 2025)"
status: growing
created: 2026-06-26
updated: 2026-06-26
tags: [source, academic, preprint, market-microstructure, limit-order-book, machine-learning, data-quality, foundation-methodology]
---

# Better Inputs > Deeper Nets in Crypto LOBs (Wang 2025)

> **TL;DR**: On crypto limit-order-book forecasting, **feature engineering + preprocessing beat architectural depth**: interpretable baselines (logistic regression, XGBoost) with good inputs match or exceed deep nets (DeepLOB, Conv1D+LSTM), with faster inference. Empirical support for the wiki's load-bearing principle that **data/signal quality dominates model architecture**. Research-grade (single-author preprint), HFT-horizon — ingest as methodology support, NOT as a day/swing edge.

## Citation

Haochuan Wang, *Exploring Microstructural Dynamics in Cryptocurrency Limit Order Books: Better Inputs Matter More Than Stacking Another Hidden Layer*, arXiv:2506.05764 (Jun 2025). [arxiv.org/abs/2506.05764](https://arxiv.org/abs/2506.05764).

## Position in lineage

Sits atop [[bouchaud-trades-quotes-prices|Bouchaud's microstructure foundation]] (the LOB primitives) and operationalizes the **data-quality-over-architecture** discipline the wiki applies in [[../methodology/bar-level-validation-overlay|bar-level validation]] and [[../methodology/backtest-analysis-diagnostics|backtest-analysis diagnostics]]. Companion to [[bieganowski-slepaczuk-crypto-microstructure|Bieganowski-Ślepaczuk]] (the explainable-OFI preprint).

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: microstructure / order-flow (LOB short-horizon forecasting).
- **Axis 2 — Driver**: information asymmetry + capacity (order-book footprints of flow).
- **Axis 3 — Crypto applicability**: **direct (crypto LOB) but HFT-horizon** — sub-second to multi-second, shorter than the wiki's ≤1-week branch; transfers as *methodology*, not a turnkey signal.

## Key findings

- Benchmarks a spectrum — **logistic regression, XGBoost → DeepLOB, Conv1D+LSTM** — on **BTC/USDT LOB snapshots from Bybit**, sampled **100 ms to multiple seconds**.
- *"With data preprocessing and hyperparameter tuning, simpler models can match and even exceed the performance of more complex networks, offering faster inference and greater interpretability."*
- **Feature engineering + noise filtering proved more impactful than architectural depth.**

## Why it matters / honest scope

Direct empirical anchor for **"data/signal quality dominates architecture"** — before iterating on model/strategy complexity, fix the inputs. Reinforces the wiki's validation discipline (the same finding underlies why bar-level wrongness or cadence mismatch swamps architecture choices). **Honest scope (verifier-flagged):** single-author **unrefereed preprint**, and the finding aligns with established ML-finance consensus (not extraordinary). The companion claim that *LOB imbalance is THE day/swing mechanism* was **refuted (1-2)** — ingest as methodology support, not as an edge claim; the horizon is HFT, not day/swing.

## Related

- [[bouchaud-trades-quotes-prices]] — the LOB/impact theory beneath this
- [[bieganowski-slepaczuk-crypto-microstructure]] — companion explainable-OFI crypto preprint
- [[../methodology/bar-level-validation-overlay]] / [[../methodology/backtest-analysis-diagnostics]] — the data-quality discipline this supports
- [[../methodology/deployment-edge-threshold]] — clean inputs upstream of any edge estimate
- [[../../trading/mechanisms/volume-analysis]] — order-book/flow signals

## Sources

- [arXiv:2506.05764](https://arxiv.org/abs/2506.05764) (abstract verified in-session 2026-06-26).
- Deep-research adversarial pass (2026-06-26): data-quality-over-architecture finding confirmed 3-0; LOB-imbalance-as-day/swing-edge companion **refuted 1-2**.
