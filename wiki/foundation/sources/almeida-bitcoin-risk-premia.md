---
title: "Risk Premia in the Bitcoin Market (Almeida, Grith, Miftachov, Wang 2024)"
status: growing
created: 2026-06-07
updated: 2026-06-07
tags: [source, academic, options, variance-risk-premium, bvrp, bitcoin-premium, regime-conditional, deribit, vol-state-axis, day-swing-applicable]
---

# Risk Premia in the Bitcoin Market (Almeida, Grith, Miftachov, Wang 2024)

> **TL;DR**: Estimates the **Bitcoin Premium (BP)** and **Bitcoin Variance Risk Premium (BVRP)** by comparing options-implied risk-neutral densities to physical return distributions (Deribit, Jul 2017–Dec 2022). This is the backbone citation already underpinning [[trading/mechanisms/variance-risk-premium-crypto]] (~14% annualized BTC VRP, ~7× the SPX equivalent). **The regime hook is what earns it a catalogued source page**: the **BVRP is regime-conditioned and inverts the equity pattern** — *lower* in the high-vol regime (~0.12) and *higher* in the calm/low-vol regime (~0.17) — and upside-protection demand (BP) concentrates in low-vol regimes. So the harvestable vol premium's sign/magnitude is itself set by the regime → a working regime classifier could **gate vol-premium harvesting**, linking the options/VRP thread to the regime workstream. Part of the N=3 vol-state-bifurcation convergence.

## Citation

- Almeida, C., Grith, M., Miftachov, R. & Wang, Z. (2024). *Risk Premia in the Bitcoin Market.* arXiv:2410.15195. https://arxiv.org/abs/2410.15195

## Position in Lineage

**This source builds on:**
- Risk-neutral-density estimation from options (Breeden-Litzenberger lineage); variance-risk-premium literature (Carr-Wu, Bollerslev)
- [[foundation/sources/sinclair-volatility-trading|Sinclair]] VRP/variance-premium concepts (the wiki's general-vol reference)

**This source influenced (in this wiki):**
- [[trading/mechanisms/variance-risk-premium-crypto]] — primary consumer (VRP magnitude + LV>HV regime-conditioning; already cited inline pre-2026-06-07, formalized here)
- [[foundation/methodology/online-regime-detection]] — third point of the N=3 vol-state-bifurcation convergence

**Concept lineage map:**
- Bitcoin VRP (BVRP) magnitude (~14% annualized) and Bitcoin Premium (BP)
- **Regime-conditioned VRP** — BVRP higher in calm (~0.17) than high-vol (~0.12); inverts equity pattern

## 3-Axis Tags

- **Economic mechanism**: Volatility risk premium; macro/regime (regime-conditioned premium)
- **Behavioral/structural driver**: Information asymmetry + protection demand (options buyers pay for insurance/upside)
- **Crypto applicability**: Direct (Deribit BTC options empirics)

## Key findings

- **BTC VRP ≈ 14% annualized** — substantially larger than SPX (~7×); the core harvestable premium for [[trading/mechanisms/variance-risk-premium-crypto]].
- **BVRP is regime-conditioned and inverts equities**: *higher* in calm/low-vol (~0.17), *lower* in high-vol (~0.12). The favorable harvesting regime is the calm one — counterintuitive vs the equity intuition that you get paid most when vol is high.
- **Bitcoin Premium / upside-protection demand concentrates in low-vol regimes** — buyers pay up for convexity when things are quiet.

## Why it matters / honest scope

- **VRP × regime linkage** — the premium's sign/magnitude is regime-dependent, so the regime workstream and the VRP-harvest mechanism are coupled: a working regime classifier gates *when* to harvest. Links the options thread to [[foundation/methodology/online-regime-detection]].
- **N=3 convergence** — with [[foundation/sources/msgarch-crypto-vol-regimes|MSGARCH]] and [[foundation/sources/nhhm-crypto-regime-switching|NHHM]], a third independent finding that crypto behavior bifurcates by a high-vol/calm state.
- **Honest scope**: sample ends Dec 2022 (pre-ETF era) — the regime mix has shifted since (ETF flows, institutionalization); re-anchor the magnitudes against current DVOL before deployment. Abstract-grade extraction; the regime split is retrospective (the *harvesting* implication is the deployable observation, not a live regime call). Capability-gap-blocked on Deribit options data, like the rest of the VRP family.

## Related

- [[trading/mechanisms/variance-risk-premium-crypto]] — primary downstream consumer
- [[foundation/methodology/online-regime-detection]] — VRP-gating link; N=3 convergence
- [[foundation/sources/msgarch-crypto-vol-regimes]] + [[foundation/sources/nhhm-crypto-regime-switching]] — the other two convergence points
- [[foundation/sources/deribit-insights]] — practitioner companion (Deribit vol-regime + DVOL)
- [[trading/mechanisms/insurance-premium-harvest-overview]] — premium-harvest family context
- [[foundation/sources/lineage]]

## Sources

- arXiv:2410.15195 (abstract-grade, 2026-06-07). Already cited inline in [[trading/mechanisms/variance-risk-premium-crypto]]; this page formalizes it as a catalogued source.
