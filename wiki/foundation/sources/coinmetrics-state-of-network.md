---
title: "Coin Metrics — 'State of the Network' (Realized Cap / MVRV primitive)"
status: growing
created: 2026-06-26
updated: 2026-06-26
tags: [source, practitioner, on-chain, crypto-native, realized-cap, mvrv, valuation, cross-market-transfer, day-swing-applicable]
---

# Coin Metrics — "State of the Network"

> **TL;DR**: Coin Metrics' weekly research letter, and the home of the **Realized Capitalization** primitive (Carter & Le Calvez, 2018) — which values each coin at the price it **last moved on-chain**, approximating the network's **aggregate cost basis**. Realized cap is the denominator of MVRV (market cap ÷ realized cap). It is genuinely crypto-native (no TradFi equivalent), so it both fills the on-chain analytics gap **and** is itself a worked example of the wiki's cross-market-transfer thesis: a valuation primitive that *only* on-chain data makes possible. Independent publisher from [[glassnode-week-onchain|Glassnode]] → de-risks single-lineage.

## Citation

Coin Metrics, *State of the Network* (weekly newsletter); [coinmetrics.substack.com](https://coinmetrics.substack.com). Realized Cap originated by Nic Carter & Antoine Le Calvez (2018). Metric definitions: Coin Metrics docs / Network Data Pro.

## Position in lineage

Second ingest into the #1 self-flagged on-chain gap (CoinMetrics ×16 in the activity log). Paired with [[glassnode-week-onchain|Glassnode]] as a *different-publisher* on-chain source — the two together close the practitioner gap while reducing the single-author-lineage risk that the academic crypto-cluster carried.

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: valuation / cost-basis anchoring (realized cap = aggregate cost basis; MVRV = unrealized-profit/loss multiple).
- **Axis 2 — Driver**: information asymmetry — on-chain settlement history is public; cost basis is *observable*, unlike in equities.
- **Axis 3 — Crypto applicability**: **direct, crypto-native** — and an exemplar of crypto-native primitives that have **no cross-market analog** (the inverse of the transfer mode: not imported from TradFi, but native).

## Key findings (the primitive)

- **Realized Capitalization** — instead of `price × supply` (market cap), sum *each coin valued at the price it last transacted on-chain*. Economically: a gross approximation of the network's **aggregate acquisition cost** ("what the market paid, in aggregate").
- **MVRV = Market Cap ÷ Realized Cap** — the aggregate unrealized profit/loss multiple. MVRV > 1 ⇒ market in aggregate profit; < 1 ⇒ aggregate loss.
- **Realized Price = Realized Cap ÷ Supply** — the per-coin cost basis (shared definition with [[glassnode-week-onchain|Glassnode]]).

## Why it matters / honest scope

The realized-cap family gives the investing + trading branches a **cost-basis-anchored valuation frame** with no equity equivalent — useful for regime/bottom-zone reads (aggregate-loss territory has marked prior bear bottoms). **Critical caveat (verifier-flagged):** the framing of *MVRV as a turnkey top/bottom oscillator* with a backtestable entry rule was **refuted (1-2)** — ingest the **primitive** (cost-basis valuation), not an oscillator trading rule. Like Glassnode, this is a live recurring letter; re-pull at use.

## Related

- [[glassnode-week-onchain]] — companion on-chain practitioner source (cohort metrics)
- [[../../trading/mechanisms/on-chain-mechanisms]] — mechanism family
- [[../market-structure/crypto-carrier-features]] — why crypto-native primitives (on-chain cost basis) give classical valuation a second life
- [[../macro/btc-regime-taxonomy-2020-2025]] — aggregate-profit/loss crossover as a bottom-zone marker

## Sources

- [Coin Metrics — State of the Network](https://coinmetrics.substack.com) · Realized Cap (Carter & Le Calvez, 2018)
- Verified via deep-research adversarial pass (2026-06-26): realized-cap definition confirmed 3-0 across Coin Metrics + Glassnode docs; MVRV-as-oscillator edge claim **refuted** — primitive only.
