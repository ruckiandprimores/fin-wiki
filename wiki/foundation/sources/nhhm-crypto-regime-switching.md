---
title: "Non-Homogeneous HMM Regime Switching for Crypto (Koki, Leonardos, Piliouras)"
status: growing
created: 2026-06-07
updated: 2026-06-07
tags: [source, academic, regime-detection, hmm, nhhm, bull-bear-calm, regime-switching, one-step-ahead, filtering-vs-smoothing, day-swing-applicable]
---

# Non-Homogeneous HMM Regime Switching for Crypto (Koki, Leonardos, Piliouras)

> **TL;DR**: A **Non-Homogeneous Hidden Markov Model (NHHM)** — an HMM whose transition probabilities are **time-varying, driven by exogenous predictors** (rather than constant) — fit to BTC/ETH/XRP. Testing 2–5 states, the **4-state NHHM wins on one-step-ahead forecasting** and cleanly separates **bull / bear / calm** regimes on BTC (profit/risk-magnitude regimes on ETH/XRP). **Why it matters for the wiki**: this is the closest published analog to what the team is *building* — bull/bear/calm classification on the exact assets — and the non-homogeneous transitions (regime persistence conditioned on covariates) are a direct design input for "how long will this range last." **The critical caveat is the [[foundation/methodology/online-regime-detection#filtering-vs-smoothing|filtering-vs-smoothing]] lens**: one-step-ahead forecasting is *prospective* (filtered probabilities), but standard HMM regime *labelling* via Viterbi is *retrospective* (smoothed) — so read which inference mode each reported metric uses. Adjacent support for the regime workstream; part of the N=3 vol-state-bifurcation convergence.

## Citation

- Koki, C., Leonardos, S. & Piliouras, G. *Exploring the predictability of cryptocurrencies via Bayesian hidden Markov models* / *Regime switching forecasting for cryptocurrencies.*
- Preprint: arXiv:2011.03741 (2020) — https://arxiv.org/abs/2011.03741
- Journal: *Digital Finance* (Springer), 2024 — https://link.springer.com/article/10.1007/s42521-024-00123-2

## Position in Lineage

**This source builds on:**
- Hidden Markov Models (Baum-Welch; Viterbi) + non-homogeneous transition extensions
- Hamilton (1989) regime-switching tradition

**This source influenced (in this wiki):**
- [[foundation/methodology/online-regime-detection]] — Method-comparison table (HMM/NHHM row); the filtering-vs-smoothing caveat is anchored on this paper
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — published analog to the bull/bear/calm taxonomy

**Concept lineage map:**
- 4-state bull/bear/calm regime classification on BTC/ETH (one-step-ahead forecasting)
- Non-homogeneous (covariate-driven) transition probabilities → regime-persistence conditioning

## 3-Axis Tags

- **Economic mechanism**: Macro/regime (bull/bear/calm classification)
- **Behavioral/structural driver**: n/a (method paper)
- **Crypto applicability**: Direct (BTC/ETH/XRP empirics)

## Key findings

- **4-state NHHM** is the best model on one-step-ahead forecasting (beats 2/3/5-state and homogeneous variants).
- On BTC the 4 states map to **bull / bear / calm** (plus a transitional state); on ETH/XRP states correspond more to profit/risk-magnitude regimes.
- Non-homogeneous transitions (driven by exogenous predictors) capture regime persistence better than constant-transition HMMs.

## Why it matters / honest scope

- **Closest published analog to the team's build** — directional bull/bear/calm on the exact assets; the non-homogeneous transition is a concrete design input for range-duration.
- **Filtering-vs-smoothing is the load-bearing read**: forecasting (one-step-ahead) is prospective; Viterbi labelling is retrospective. The paper's reported separation may rely on smoothed labels — exactly the live-deployment trap [[foundation/methodology/online-regime-detection]] warns about. Use the filtered probabilities, not the Viterbi path, for any live-gating analog.
- **HMM weakness on multi-day ranges**: standard HMMs assume i.i.d.-within-regime, which breaks on a drifting 7-day range — this is why the team's frontier likely needs the path-dependent ([[foundation/sources/issa-horvath-online-regime-detection|signature-MMD]]) or score-driven ([[foundation/sources/tsaknaki-lillo-mazzarisi-bocpd|BOCPD]]) machinery rather than a plain HMM.
- Part of the **N=3 vol-state-bifurcation convergence** (with [[foundation/sources/msgarch-crypto-vol-regimes|MSGARCH]] and [[foundation/sources/almeida-bitcoin-risk-premia|Bitcoin VRP]]).
- Abstract-grade extraction; full PDF not deep-read.

## Related

- [[foundation/methodology/online-regime-detection]] — the regime backbone; HMM/NHHM row + filtering-vs-smoothing caveat
- [[foundation/sources/issa-horvath-online-regime-detection]] + [[foundation/sources/tsaknaki-lillo-mazzarisi-bocpd]] — the online-native methods that fix HMM's i.i.d.-within-regime weakness
- [[foundation/sources/msgarch-crypto-vol-regimes]] — vol-state-axis sibling
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — the wiki's retrospective bull/bear/calm taxonomy
- [[foundation/sources/lineage]]

## Sources

- arXiv:2011.03741 + *Digital Finance* 2024 (abstract-grade, 2026-06-07).
