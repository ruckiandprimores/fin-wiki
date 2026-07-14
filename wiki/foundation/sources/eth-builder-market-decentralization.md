---
title: "Yang, Nayak, Zhang — Decentralization of Ethereum's Builder Market"
status: growing
created: 2026-07-14
updated: 2026-07-14
tags: [source, academic, crypto-native, ethereum, mev, pbs, builder-market, centralization, proposer-loss, day-swing-applicable]
---

# Yang, Nayak, Zhang — Decentralization of Ethereum's Builder Market

> **TL;DR**: Empirical study of Ethereum's MEV-Boost builder market. **Full text fetched and read (arxiv.org/html/2405.01329) — all figures below are VERIFIED against the paper text**, not secondary reporting. Headline finding: **two builders produced >85% of Ethereum blocks and won >90% of MEV-Boost auctions** over the study window (Sept 2022 – Oct 2024). The centralization causes **quantified, material proposer losses** — 5.6-11.5% from market-power "inequality" (the dominant loss channel) plus 0.5-1.7% from plain uncompetitiveness — and losses **worsened sharply into 2024** (pre-2024 median proposer loss 4.1% → 2024 median 21.2%). A second finding — **80% of block value now derives from private order flow**, concentrated in a handful of pivotal wallet/builder integrations — shows the centralization problem is compounding through vertical integration, not just builder-count concentration. The paper argues current mitigation proposals (ePBS, MEV burn, Execution Tickets) don't fix this because they preserve the same underlying supply chain. *Analysis, not advice.*

## Citation

**Yang, S., Nayak, K., Zhang, F.** *Decentralization of Ethereum's Builder Market*. arXiv:2405.01329 [cs.CR]. v1 submitted May 2, 2024; v5 (current, read for this page) April 29, 2025.

- arXiv: https://arxiv.org/abs/2405.01329
- HTML (fetched): https://arxiv.org/html/2405.01329

## 3-Axis Tags

- **Economic mechanism**: Market microstructure / MEV auction design — proposer-builder separation (PBS) as a two-sided market with severe concentration on the builder side
- **Behavioral/structural driver**: Capacity constraint + vertical integration — dominant builders compound their edge via private order-flow deals with wallets/searchers, entrenching a "cyclical dominance" advantage
- **Crypto applicability**: Direct (Ethereum-native; no cross-market translation needed — this is domain knowledge about ETH market structure itself)

## Key empirical findings (all VERIFIED from fetched full text)

### 1. Builder concentration

| Metric | Value | Window |
|---|---|---|
| Top 2 builders' share of blocks produced | **>85%** | as of writing (through Oct 2024) |
| Top 2 builders' share of MEV-Boost auctions won | **>90%** | Fig. 1, "MEV-boost slot prominence since the merge" |
| Study window | Sept 2022 – Oct 2024 | partial-bid dataset |

### 2. Proposer losses — two distinct channels

The paper decomposes proposer loss into **uncompetitiveness** (auction didn't attract enough competing bidders) and **inequality** (market power let a builder pay less than the block's true value even in a "competitive" auction):

| Loss channel | Range across study periods | Share of auctions affected |
|---|---|---|
| Uncompetitiveness | **0.5% – 1.7%** | 91.8% of auctions were "competitive" (bid CI ≥ 0); only 75.1% were "efficient" |
| **Inequality (primary driver)** | **5.6% – 11.5%** | Increases with MEV magnitude in the slot |

**2024 trend is the load-bearing number**: pre-2024 median estimated proposer loss was **4.1%**; the **2024 median rose to 21.2%** — a ~5x deterioration. This is the paper's clearest signal that centralization costs are compounding, not stabilizing.

### 3. Private order flow — the compounding mechanism

- **80% of block value derives from private order flow** (order flow that never touches the public mempool) — reaching **>50% of blocks** by October 2024.
- **5 "pivotal" order-flow providers** identified, each exceeding a >50% pivotal-level threshold across 2+ consecutive weeks — meaning a handful of wallet/aggregator relationships can single-handedly swing which builder wins a given slot.
- **3 new builder-provider integrations discovered** during the study: Banana Gun ↔ Titan, jaredfromsubway.eth ↔ Beaver, Maestro ↔ Beaver.
- **Integration-attributable proposer losses (2024)**: Banana Gun+Titan **17,488.1 ETH (32.6% of all 2024 proposer losses)**; jaredfromsubway.eth+Beaver **2,480.5 ETH (4.6%)**; Maestro+Beaver **439.7 ETH (0.8%)**. A single wallet-builder pairing accounts for roughly a third of the year's total proposer loss.

### 4. Dataset / methodology (for provenance weight)

| Component | Size |
|---|---|
| Full-bid auctions analyzed | 301,479 (Apr–Dec 2023), from 326.6M raw full bids collected from the ultra sound relay; covers >80% of builders in 75% of auctions |
| Partial bids (broader window) | ~12 billion (Sept 2022 – Oct 2024) |
| Private transactions labeled | 112.2 million |
| On-chain blocks analyzed | 5.5 million (4.9 million were MEV-Boost blocks) |
| Relays connected | 13 (10 active during the study) |
| Builder identities resolved | 273 builder public keys / 102 builder addresses, via a modified Reth execution client for "true value" computation |
| Cross-validation | Against three independent public datasets |

### 5. Why the paper says current mitigation proposals fall short

The authors evaluate ePBS, MEV burn, and Execution Tickets and conclude none resolve the core problem, because:

1. These designs "retain the same underlying supply chain" — they change *how* bids are settled, not the concentration of who's capable of winning them.
2. They don't address the economic incentive for wallets/searchers to integrate exclusively with a dominant builder (the private-order-flow finding above).
3. They provide no security guarantee protecting smaller/independent order-flow providers from adverse behavior by a dominant builder.

The paper frames the centralization problem as **"a grand challenge that requires further, possibly interdisciplinary, study"** rather than a solved-by-protocol-change issue.

## Honest caveats

- **This is a single research group's empirical methodology** (custom Reth instrumentation + relay data collection) — the "true value" computation used to derive proposer loss is a modeled counterfactual (what a fully competitive/fully efficient auction would have paid), not a directly observed number. Treat the loss percentages as the paper's best estimate under its stated assumptions, not ground truth.
- **Window ends October 2024** — this page does not know whether concentration has since worsened, stabilized, or improved. No 2025-2026 data was fetched for this specific paper (the builder-market landscape may have shifted with relay/builder entrants since).
- **Correlation vs. causation on ePBS efficacy** — the paper's critique of ePBS/MEV-burn/Execution-Tickets is an argued position, not an empirical test of a live ePBS deployment (ePBS was not live during the study window). Cross-reference [[foundation/sources/sok-proposer-builder-separation|the ePBS SoK page]] for the mechanism design being critiqued.

## Implications for wiki priorities

- Sharpens [[foundation/asset-classes/eth-market-structure|the new ETH market-structure page]] — this is the primary quantitative anchor for "how concentrated is the MEV supply chain, and does it matter economically."
- Companion to [[foundation/sources/sok-proposer-builder-separation]] — that page describes the PBS/ePBS *mechanism design*; this page supplies the *empirical severity* of the problem ePBS is meant to solve.

## Related

- [[foundation/sources/sok-proposer-builder-separation]] — PBS/ePBS mechanism-design companion (same domain, different lens: architecture vs. empirics)
- [[foundation/asset-classes/eth-market-structure]] — synthesizes this paper's findings into the ETH structural picture
- [[foundation/asset-classes/btc-market-structure]] — sister asset-class page; BTC has no analogous builder-market layer (PoW, no PBS)

## Sources

- arXiv:2405.01329 (v5, Apr 29 2025), fetched in full via https://arxiv.org/html/2405.01329
- Abstract page: https://arxiv.org/abs/2405.01329
