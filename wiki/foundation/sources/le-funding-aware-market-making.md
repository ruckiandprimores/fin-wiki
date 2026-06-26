---
title: "Funding-Aware Optimal Market Making for Perpetual DEXs (Le 2026)"
status: growing
created: 2026-06-26
updated: 2026-06-26
tags: [source, academic, preprint, market-making, perpetual-futures, funding-rate, inventory-control, hjb, dex, trading]
---

# Funding-Aware Optimal Market Making for Perpetual DEXs (Le 2026)

> **TL;DR**: Extends classical (Avellaneda-Stoikov) market making by treating the **perp funding rate as a stochastic state variable** (Gaussian Ornstein-Uhlenbeck) inside a coupled **inventory-funding HJB control** — so optimal bid/ask quotes become a joint function of **inventory AND funding state**, not static spreads. On Hyperliquid simulations it improves mean ETH/BTC P&L while lowering inventory RMS vs the classical baseline. Directly relevant to the grid-bot / market-making tracks and covers the self-flagged funding-rate-design gap. Research-grade (single-author preprint; simulation, not live).

## Citation

Nam Anh Le, *Funding-Aware Optimal Market Making for Perpetual DEXs*, arXiv:2605.06405 (May 2026). [arxiv.org/abs/2605.06405](https://arxiv.org/abs/2605.06405).

## Position in lineage

Connects two threads the wiki holds separately: the **carry/funding** mechanism ([[carry-koijen-moskowitz-pedersen-vrugt|Carry]], [[../../trading/mechanisms/funding-cycle-dynamics|funding-cycle dynamics]]) and **market-making/inventory control** ([[bouchaud-trades-quotes-prices|Bouchaud]], [[../market-structure/dealer-economics|dealer economics]]). Its claim: funding isn't only a carry to harvest — it's a **state variable that should shape the quotes themselves**.

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: liquidity provision (market making) + carry / yield (funding cash flow).
- **Axis 2 — Driver**: capacity constraint (inventory risk) — inventory creates both mark-to-market exposure *and* a state-dependent funding cash flow.
- **Axis 3 — Crypto applicability**: **direct** — crypto perpetual DEX; funding-rate-design-adjacent (the self-flagged gap).

## Key findings

- **Funding as OU state**: funding modeled as a Gaussian Ornstein-Uhlenbeck diffusion; the optimization couples inventory and funding.
- **Method**: a monotone finite-difference **Hamilton-Jacobi-Bellman** scheme; bid/ask quote offsets recovered from discrete inventory value differences.
- **Core innovation**: optimal quotes depend **jointly on inventory level and funding state** (classical AS decouples funding).
- **Empirics**: on **Hyperliquid ETH/BTC/SOL** perps, 100-seed simulations — the funding-aware HJB *"improves mean ETH/BTC performance while lowering inventory RMS relative to classical Avellaneda-Stoikov."* **SOL** does **not** achieve a Pareto improvement under risk-scaling of the baseline.

## Why it matters / honest scope

The most operationally-relevant of the microstructure preprints: it gives a principled way for an inventory-carrying market maker (grid-bot / MM family) to **let the funding regime move the quotes** rather than treating funding as a side carry. **Honest scope (verifier-flagged):** single-author **preprint**; results are **100-seed holdout simulations, not live P&L**; the SOL result is weaker (no Pareto improvement). Ingest as a research-grade design pattern to validate, not a deployable policy.

## Related

- [[../../trading/mechanisms/funding-cycle-dynamics]] — funding as the state this exploits
- [[../../trading/mechanisms/delta-neutral-funding-harvest]] — the carry-harvest counterpart
- [[carry-koijen-moskowitz-pedersen-vrugt]] — funding = cost-of-carry, the parent mechanism
- [[../market-structure/dealer-economics]] — the market-maker's inventory problem
- [[bouchaud-trades-quotes-prices]] — optimal market-making / inventory control theory
- [[he-manela-fundamentals-perpetual-futures]] — perp-funding pricing fundamentals

## Sources

- [arXiv:2605.06405](https://arxiv.org/abs/2605.06405) (abstract verified in-session 2026-06-26).
- Deep-research adversarial pass (2026-06-26): funding-aware HJB ETH/BTC improvement + inventory-funding coupling confirmed 3-0; flagged simulation-only / single-author.
