---
title: "Online Learning of Order Flow with Score-Driven BOCPD (Tsaknaki, Lillo & Mazzarisi 2023)"
status: growing
created: 2026-06-07
updated: 2026-06-07
tags: [source, academic, regime-detection, online, change-point, bocpd, score-driven, order-flow, market-impact, cross-market-transfer, day-swing-applicable]
---

# Online Learning of Order Flow with Score-Driven BOCPD (Tsaknaki, Lillo & Mazzarisi 2023)

> **TL;DR**: Uses **Bayesian Online Change-Point Detection (BOCPD)** — which maintains a real-time posterior over the **run length** (time since last regime change) and updates it as each observation arrives — to identify order-flow regime shifts **in real time** and produce online predictions of order flow and market impact. The contribution is a **score-driven extension** that drops BOCPD's usual **i.i.d.-within-regime** assumption, allowing temporal correlation and time-varying parameters *inside* each regime. On NASDAQ data it beats i.i.d.-within-regime models out-of-sample, shows well-specified residuals, recovers the concave price–time/volume signature of real large orders, and improves market-impact prediction by conditioning on regime. **Why it matters for the wiki**: it is Method 2 of the prospective-regime backbone in [[foundation/methodology/online-regime-detection]] — the canonical *online (filtering)* change-point framework, with the exact fix (within-regime dynamics) that stops a detector from fragmenting a non-i.i.d. multi-day crypto range into spurious regimes. The lower-lift online method to prototype first. **Honest scope**: empirical work is on **NASDAQ equity order flow**, so it is a Category-1 cross-market transfer to crypto (transfer burden per [[foundation/market-structure/crypto-carrier-features]]).

## Citation

Tsaknaki, Ioanna-Yvonni; Lillo, Fabrizio & Mazzarisi, Piero (2023, rev. May 2024). *Online Learning of Order Flow and Market Impact with Bayesian Change-Point Detection Methods.* arXiv:2307.02375.

- arXiv: https://arxiv.org/abs/2307.02375

## Position in Lineage

**This source builds on:**
- BOCPD (Adams & MacKay 2007) — the online run-length posterior framework
- Score-driven / GAS models (Creal–Koopman–Lucas; Harvey) — the within-regime time-variation
- Order-flow persistence / large-order-splitting microstructure (Lillo–Farmer lineage) — the empirical motivation

**This source influenced (in this wiki):**
- [[foundation/methodology/online-regime-detection]] — Method 2; the filtering-vs-smoothing backbone page

**Concept lineage map — concepts this source contributes:**
- Run-length posterior as a real-time (filtering) regime-shift estimate
- Score-driven within-regime dynamics → fixes i.i.d.-within-regime over-segmentation
- Regime-conditioned online prediction of order flow + market impact
- Concave price–time/volume relationship within a regime (large-order signature)

## 3-Axis Tags

- **Economic mechanism**: Microstructure / order flow (regime-conditioned market impact); macro / regime (online change-point)
- **Behavioral/structural driver**: Forced flow / large-order splitting (the source of order-flow persistence)
- **Crypto applicability**: Mutation needed (method is asset-agnostic; empirics are equity order flow — transfer required)

## What the method is

- **BOCPD** maintains, at each step, a posterior over the **run length** (bars since last change-point) and updates it online as observations arrive. It natively answers "has the regime just changed, given data up to now?" — pure **filtering**.
- **Score-driven extension**: standard BOCPD assumes observations are **i.i.d. within each regime** (false for financial data — order flow and returns are autocorrelated inside a regime). A score-driven (GAS-style) update lets parameters vary with time within a regime, so ordinary within-regime drift isn't mistaken for a change-point.

## Key findings (NASDAQ)

1. The score-driven model has **superior out-of-sample predictive performance** vs i.i.d.-within-regime models.
2. **Residuals are well-specified** in both distributional assumptions and temporal correlation.
3. **Within a regime**, price dynamics are **concave in time and volume** — mirroring actual large-order execution.
4. **Conditioning on regime** produces more accurate online predictions of order flow and market impact.

## Why it fits the team's problem

- **Real-time by construction** — "identify regime shifts in real-time and enable online predictions." Filtering, no hindsight. See [[foundation/methodology/online-regime-detection#filtering-vs-smoothing]].
- **Within-regime time-variation = the 7-day-range pain point.** A multi-day crypto range is not i.i.d. inside itself; a naive change-point detector over-segments it. The score-driven version tolerates within-range dynamics and flags only genuine breaks — directly attacking over-segmentation.
- **Lower lift than signature-MMD** — well-documented framework, parametric, clear filtering semantics; the recommended first prototype.

## Honest scope / limitations

- **Cross-market transfer** — empirics are NASDAQ equity order flow, not crypto. Transfer burden applies (24/7, retail-heavy microstructure; per [[foundation/market-structure/crypto-carrier-features]]).
- **Feature dependency** — the order-flow application assumes an order-flow series; on crypto it pairs with the perp-microstructure feeds ([[foundation/sources/temporal-dynamics-funding-rates]], [[foundation/sources/two-tiered-funding-rate-markets]]) or can be run on returns/funding series instead.

## Related

- [[foundation/methodology/online-regime-detection]] — Method 2; the page that operationalises this source
- [[foundation/sources/issa-horvath-online-regime-detection]] — Method 1 (signature-MMD); the higher-ceiling alternative
- [[foundation/methodology/team-stable-range-detector-methodology]] — the team's (smoothing) detector this method would upgrade to filtering
- [[foundation/idea-sourcing-methodology]] — Category-1 cross-market transfer (NASDAQ → crypto)
- [[foundation/sources/lineage]] — source graph

## Sources

- arXiv:2307.02375 abstract + method (fetched 2026-06-07). Details beyond the abstract not independently verified against the full PDF.
