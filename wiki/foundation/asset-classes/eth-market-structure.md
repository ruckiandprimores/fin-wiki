---
title: "Ethereum (ETH) — Market Structure for Strategy Design"
status: growing
created: 2026-07-14
updated: 2026-07-14
tags: [asset-class, eth, ethereum, market-structure, pos, staking, mev, pbs, builder-market, ethena, basis-trade, funding-rate, literature-gap]
---

# Ethereum (ETH) — Market Structure for Strategy Design

> **Lead framing**: ETH's market structure is shaped by two mechanics BTC does not have: **Proof-of-Stake validator/staking economics** (a structural yield floor competing with — and potentially suppressing — perp funding/basis) and a **heavily concentrated MEV supply chain** (Proposer-Builder Separation, where a tiny number of builders capture the large majority of block value). This page synthesizes three papers ingested 2026-07-14: an empirical measurement of builder-market concentration ([[foundation/sources/eth-builder-market-decentralization|Yang, Nayak, Zhang]]), a mechanism-design SoK of the PBS/ePBS pipeline ([[foundation/sources/sok-proposer-builder-separation|Koegler]]), and a theoretical model of Ethena's delta-neutral staking-basis strategy ([[foundation/sources/ethena-optimal-control-staking-basis|Lorig]]). **Sits alongside [[btc-market-structure|BTC's]] and [[sol-market-structure|SOL's]] asset-class pages** in the per-symbol mechanism-asymmetry framework — this page is the ETH structural counterpart, but sourced from a different literature cluster (MEV/PBS + Ethena theory) than the BTC/SOL pages' lab-empirical anchor. *Descriptive market-structure analysis, not a trading recommendation.*

## Why this page exists

The wiki's per-symbol asymmetry framework already covers BTC (continuation character, hedge-fund basis-trade anchored) and SOL (long-bias amplifier, retail-leveraged derivatives + nascent institutional layer). ETH was "referenced but deferred" in the SOL page (per that page's Why-this-page-exists section). This page fills that gap — not with a fourth lab-empirical breakout-character study, but with the **structural** picture: what's mechanically different about ETH's market plumbing (staking, MEV/PBS) that any ETH-specific strategy design needs to account for.

## 1. PoS validator/staking economics {#staking-economics}

Ethereum moved to Proof-of-Stake at the Merge (Sept 2022). Validators lock ETH to participate in consensus and earn:

- **Base issuance reward** — protocol-level reward for proposing/attesting, funded by new ETH issuance (this is the "risk-free" staking yield floor, roughly analogous to a sovereign short rate in the ETH-denominated economy)
- **Priority fees + MEV** passed through from the block's builder via the PBS auction (see §2) — per [[foundation/sources/sok-proposer-builder-separation|Koegler]], proposal rewards are cited as **~12% of protocol-sponsored incentives**, with MEV/priority fees making up the remainder of a proposer's realized income (VERIFIED, from the fetched SoK text)

**Share of supply staked**: the wiki's existing [[sol-market-structure|SOL market-structure page]] cites ETH's staked share at **28-30%** of circulating supply (vs BTC's structural 0%, PoW has no staking, and SOL's much higher 65-75%). This figure predates the present ingest and was not re-verified against a primary source in this session — carried forward as an existing wiki value, **flagged here as (REPORTED — not independently re-verified in this ingest)**.

**Structural implication**: staking yield is a **standing, ETH-denominated risk-free-ish return** available to any ETH holder without taking directional derivatives exposure. This matters directly for §3 below — it's a competing return that a delta-neutral strategy (long spot/stETH + short perp) can stack on top of the funding-rate carry, which BTC's non-yielding spot cannot.

## 2. The MEV supply chain — Proposer-Builder Separation {#pbs-mev-chain}

Per [[foundation/sources/sok-proposer-builder-separation|Koegler's SoK]] (VERIFIED from fetched text), the PBS pipeline has four roles:

| Role | Function |
|---|---|
| Proposer | The validator selected to propose a block; earns proposal reward + MEV/fees relayed from the builder |
| Builder | Constructs the actual block, ordering transactions to maximize extractable value, and bids for the right to have it proposed |
| Relay | Off-chain trusted intermediary that mediates the builder-proposer handoff (holds builder's block secret until payment commits) |
| Searcher | Finds MEV opportunities, bundles them, sells the bundle to builders |

### Concentration is severe and getting worse

Per [[foundation/sources/eth-builder-market-decentralization|Yang, Nayak, Zhang]] (VERIFIED from fetched full text, Sept 2022 – Oct 2024 window):

- **Two builders produced >85% of Ethereum blocks and won >90% of MEV-Boost auctions.**
- Proposer loss from builder market power ("inequality" channel) is **5.6-11.5%** of value, the dominant loss driver (vs. 0.5-1.7% from plain uncompetitiveness).
- **Estimated median proposer loss jumped from 4.1% (pre-2024) to 21.2% (2024)** — concentration cost is compounding, not stabilizing.
- **80% of block value now derives from private order flow** (bypassing the public mempool entirely), concentrated in a handful of wallet-builder integrations — one single pairing (Banana Gun ↔ Titan) accounted for **32.6% of all 2024 proposer losses**.
- **~90% of validators use MEV-Boost** (adoption figure, per Koegler).
- A structural centralization point named in the SoK: **"most notable relays are run by Flashbots themselves."**

### The proposed fix — ePBS (EIP-7732) — and its stated limits

Per Koegler, enshrined PBS moves the builder-proposer handoff **on-chain**: builders post payload headers + bids directly in beacon blocks; a Payload Timeliness Committee enforces honest, timely delivery (a 40% fork-choice weight boost for compliant builders); builders now stake capital to participate, rather than relying on relay reputation alone.

**But** — per Yang, Nayak, Zhang's explicit argument — ePBS (and MEV burn, and Execution Tickets) **"retain the same underlying supply chain"**: they change how the auction settles, not who is capable of winning it or the economic incentive for order-flow providers to integrate exclusively with a dominant builder. The paper frames unwinding this concentration as an open, interdisciplinary problem — not solved by the mechanism-design changes currently proposed.

**Structural takeaway**: ETH's MEV/PBS layer is a genuinely distinct piece of market plumbing that BTC (PoW, no block auction) and, to a lesser/differently-shaped extent, SOL (different validator/leader-schedule design) don't share in the same form. Concentration here is a real, measured, worsening cost borne by validators — a structural tax on ETH block production that has no BTC analog.

## 3. Staking yield, Ethena, and the ETH funding/basis floor {#staking-yield-and-basis}

### The mechanism as far as the papers confirm it

[[foundation/sources/ethena-optimal-control-staking-basis|Lorig's paper]] models Ethena's core construction — long stETH + short ETH perp, delta-neutral — and its central finding (VERIFIED) is that **the protocol's own trading permanently compresses the basis it depends on for income**: buying stETH pushes spot up, shorting the perp pushes the perp price down, and the combined effect (μ₁+μ₂) erodes future funding income. The optimal strategy self-limits its accumulation speed for exactly this reason (an "inventory brake" term in the solved control law), and near any horizon/redemption date the model prescribes rapid unwinding **regardless of the basis level** ("liquidation urgency near expiry").

This is a **self-impact** mechanism — it says a large-enough delta-neutral staking-basis strategy compresses its own edge as it grows, independent of anything BTC-specific. It is a **different claim** from "ETH's staking yield structurally caps ETH perp funding/basis below BTC's" — the Ethena paper does not make that second claim. It treats the funding rate as an *exogenous* mean-reverting process, not something derived from staking-yield competition.

### The claim the wiki cannot currently verify — and the literature gap {#literature-gap}

The intuitive structural story — "ETH offers a competing staking yield that spot ETH holders can already earn risk-free, so ETH perp funding/basis should sit structurally lower than BTC's (which has no yield-bearing spot alternative)" — is **plausible but unconfirmed by any paper in this ingest, and, per a 2026-07-12 research pass, no published paper doing a direct ETH-vs-BTC perp-basis comparison through this staking-yield lens currently exists.** This is an **open literature gap**, not a resolved finding. Concretely:

- Lorig's paper models funding q as exogenous — it neither confirms nor denies a staking-yield-sets-a-floor relationship.
- No OI-share or realized-yield figures for Ethena were found in the fetched papers. In particular, a commonly-repeated claim that Ethena's short position represents on the order of ~5% of ETH perpetual open interest **could not be verified in this ingest and is NOT stated in the fetched paper** — it should be treated as **(REPORTED — unverified)** if it appears elsewhere, and not cited as fact from this source cluster.
- The wiki's existing empirical anchor for Ethena is [[trading/mechanisms/delta-neutral-funding-harvest]] (yield trajectory 18% APY 2024 → 3.72% Q1 2026 on sUSDe, TVL $14B peak → $5.92B Q1 2026) — those figures come from that page's own sourcing (Stablecoin Insider, Coin Metrics), not from the three papers ingested here.

**Standing open question for future ingests**: find (or commission) a direct empirical ETH-vs-BTC perp-basis/funding comparison that controls for the staking-yield differential. Until then, "ETH funding structurally lower than BTC's because of staking yield" stays a hypothesis, not a codified wiki finding.

## What this means for ETH strategy design (descriptive) {#strategy-design-implications}

- **MEV/PBS concentration is a validator-economics cost, not directly a price-action signal** — it affects who profits from including transactions, not (on current evidence) the direction or magnitude of ETH's price moves. Its relevance to trading strategy is indirect: extreme private-order-flow concentration could, in principle, correlate with information advantages for large integrated builder-searcher pairs at moments of high on-chain activity (e.g., liquidation cascades, large DEX trades) — this is speculative and not tested by any paper in this ingest.
- **Staking yield as a standing floor return** is structurally relevant to any ETH delta-neutral construction (per [[trading/mechanisms/delta-neutral-funding-harvest]]): a long-stETH + short-perp position stacks staking yield on top of funding-rate carry, which is why ETH-based delta-neutral yield (Ethena) can differ in composition from a BTC-based equivalent (spot BTC earns no yield analog) even when funding rates alone happen to be similar.
- **Self-impact basis compression (Lorig)** is a scale-dependent structural ceiling on any single large delta-neutral participant's realized carry on ETH — the bigger the position, the more the strategy erodes its own future funding income, independent of external market conditions. This is a mechanism worth tracking as a **capacity/scale caveat** for any ETH funding-harvest analysis, alongside the wiki's existing capacity-constrained-edge framing for the mechanism generally.
- **No lab-empirical breakout-character finding for ETH is established in this page** — unlike [[btc-market-structure]] (continuation character, N=3 lab evidence) and [[sol-market-structure]] (long-bias amplifier, N=2 lab evidence), this page is sourced from the MEV/PBS + Ethena-theory literature cluster, not the backtesting-lab empirical cluster. The wiki's prior lab-derived ETH breakout-character note — "fakeout-fade" at phase transitions, referenced in [[btc-market-structure]] and [[sol-market-structure]] — is a **separate, pre-existing lab finding**, not sourced from or extended by this page's three papers. Treat the two ETH characterizations (lab-empirical fakeout-fade vs. this page's structural MEV/staking picture) as complementary, not overlapping.

## Honest-scope note on this ingest {#honest-scope}

All three source papers were fetched and read at **full-text grade** via arXiv's HTML rendering (`arxiv.org/html/<id>`) — not abstract-only. Figures marked VERIFIED above were checked against that fetched text. Figures carried forward from other wiki pages (ETH staked-share 28-30%; Ethena yield/TVL trajectory) are flagged as such and were not re-verified against a primary source in this session. No figure in this page was fabricated or estimated without a stated source.

## Open empirical questions

1. **The ETH-vs-BTC basis-comparison literature gap** (§3) — highest-priority gap; no direct published comparison exists as of the 2026-07-12 research pass.
2. **Has builder-market concentration (>85% top-2 share) changed since October 2024** (the end of the Yang/Nayak/Zhang study window)? New relay/builder entrants, or ePBS progress, could have shifted this.
3. **Does ePBS's Payload Timeliness Committee mechanism actually reduce the "inequality" loss channel once live**, or does the paper's critique (same underlying supply chain) hold up empirically? Untested — ePBS was not live during either paper's writing.
4. **What is Ethena's actual current share of ETH perpetual open interest**, and has the self-impact basis-compression mechanism (Lorig) been empirically detected in Ethena's realized yield decay? The 18% → 3.72% APY compression documented in [[trading/mechanisms/delta-neutral-funding-harvest]] is consistent with self-impact compression but also consistent with pure external carry compression (ETF-arb absorption, per that page) — decomposing the two causes is untested.
5. **Does the ~28-30% ETH staked-share figure still hold?** Not re-verified in this ingest; a primary-source recheck (e.g., beaconcha.in / Rated Network data) would upgrade this from carried-forward to verified.

## Related

- [[foundation/sources/eth-builder-market-decentralization]] — empirical builder/relay concentration + proposer-loss anchor
- [[foundation/sources/sok-proposer-builder-separation]] — PBS/ePBS mechanism-design anchor
- [[foundation/sources/ethena-optimal-control-staking-basis]] — Ethena self-impact basis-compression theory
- [[btc-market-structure]] — sister asset-class page; BTC has no PBS/staking analog (PoW, no validator yield)
- [[sol-market-structure]] — sister asset-class page; source of the carried-forward 28-30% ETH staked-share figure and the fakeout-fade lab-empirical reference
- [[foundation/sources/he-manela-fundamentals-perpetual-futures]] — general perp-pricing/arbitrage academic anchor (not ETH-specific, but the basis-mechanics vocabulary this page uses)
- [[foundation/sources/two-tiered-funding-rate-markets]] — cross-venue funding-rate empirics (ETH-inclusive dataset, per that page's GARCH-persistence-by-symbol table)
- [[trading/mechanisms/delta-neutral-funding-harvest]] — the wiki's empirical home for Ethena/USDe yield, TVL, and the Oct 2025 cascade; this page's theoretical complement

## Sources

- [[foundation/sources/eth-builder-market-decentralization]] — Yang, Nayak, Zhang, arXiv:2405.01329 (full text)
- [[foundation/sources/sok-proposer-builder-separation]] — Koegler, arXiv:2506.18189 (full text)
- [[foundation/sources/ethena-optimal-control-staking-basis]] — Lorig, arXiv:2605.11263 (full text)
- Carried-forward (not re-verified this session): ETH staked-share 28-30%, per [[sol-market-structure]]; Ethena yield/TVL trajectory, per [[trading/mechanisms/delta-neutral-funding-harvest]]
