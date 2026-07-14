---
title: "Koegler — SoK: Current State of Ethereum's Enshrined Proposer Builder Separation"
status: growing
created: 2026-07-14
updated: 2026-07-14
tags: [source, academic, crypto-native, ethereum, mev, pbs, epbs, mev-boost, relay, sok, day-swing-applicable]
---

# Koegler — SoK: Current State of Ethereum's Enshrined Proposer Builder Separation

> **TL;DR**: Systematization-of-Knowledge (SoK) paper mapping the Proposer-Builder Separation (PBS) supply chain — proposer / builder / relay / searcher — and the proposed on-chain "enshrined PBS" (ePBS, EIP-7732) upgrade. **Full text fetched and read (arxiv.org/html/2506.18189) — the architecture description and mechanics below are VERIFIED against the paper.** Presented to The Science of Blockchain Conference 2025. Describes today's MEV-Boost system (off-chain, relay-mediated) and its trust weaknesses — no minimum relay stake, no penalty for a relay submitting a faulty bid, and the observation that **most notable relays are run by Flashbots itself**, a further centralization point. Explains ePBS's mechanics: builders post payload headers + bids directly in beacon blocks, a Payload Timeliness Committee enforces delivery (honest on-time builders get a fork-choice weight boost), and builders now stake to participate. Complements [[foundation/sources/eth-builder-market-decentralization|the Yang/Nayak/Zhang empirical paper]] — this page is architecture/design, that one is measured severity. *Analysis, not advice.*

## Citation

**Koegler, M.** *SoK: Current State of Ethereum's Enshrined Proposer Builder Separation*. arXiv:2506.18189 [cs.CR, cs.CY]. Submitted June 22, 2025 (v1, only version). 12 pages, 2 figures. Presented to The Science of Blockchain Conference 2025.

- arXiv: https://arxiv.org/abs/2506.18189
- HTML (fetched): https://arxiv.org/html/2506.18189

## 3-Axis Tags

- **Economic mechanism**: Market microstructure / auction design — block-space auctions mediated by trusted relays, moving toward an on-chain mechanism
- **Behavioral/structural driver**: Regulatory/protocol friction + information asymmetry — relays hold unilateral, unpenalized power over bid delivery; ePBS attempts to convert this into an enforceable, staked, on-chain commitment
- **Crypto applicability**: Direct (Ethereum-native protocol design; no cross-market translation)

## The PBS supply chain (VERIFIED from fetched text)

| Role | Function |
|---|---|
| **Proposer** | Validator selected to propose the block; earns proposal rewards (paper cites ~12% of protocol-sponsored incentives) plus MEV/priority fees passed through from the builder |
| **Builder** | Specialized entity that "maximizes slot value through transaction ordering and block structuring"; constructs optimized blocks and bids for the right to have them proposed |
| **Relay** | Trusted intermediary that "protects the builder's payload and the proposer's slot" — holds the builder's block contents secret from the proposer until payment is committed, and vice versa |
| **Searcher** | Identifies MEV opportunities and bundles them into MEV bundles submitted to builders, "lightening their computational load by pre-compiling extractable transaction instances in exchange for a fee" |

**Adoption**: the paper cites that **"upwards of 90% of validators"** currently use MEV-Boost (Figure 1: "MEV-boost slot prominence since the merge").

## Current system flow and trust weaknesses (VERIFIED)

Transactions reach builders either via the public mempool or via "MEV-share affiliated RPC endpoints" (where searchers may bundle them first). Builders construct blocks and submit bids to proposers **through relays** — proposers never see the block contents directly, only the relay-attested bid value, until they've committed to it.

**Trust vulnerabilities the paper flags**:

- **No minimum relay stake and limited relay accountability** — "relays have the capacity to submit faulty bids without any financial penalty."
- A relay could, in principle, "structure its block, bypass bidding rounds, submit a fraudulent bid, claim the MEV, and leave" — with reputational damage as the only consequence.
- **Builder-side centralization risk**: "if a builder ever gets a leg up on others, they can easily monopolize the block creation market," describing a "cyclical dominance" dynamic where a leading builder's expanding "bidding reservoir" goes uncontested by competitors.
- **Named centralization point**: "most notable relays are run by Flashbots themselves" — i.e., the very entity that popularized MEV-Boost also runs much of the trusted-intermediary layer that's supposed to keep builders and proposers honest with each other.

## Enshrined PBS (ePBS, EIP-7732) mechanics (VERIFIED)

Five stated motivations for moving PBS on-chain:

1. Trustless exchange between proposers and builders (removes the relay's trust role)
2. Reduced validator computational demand
3. Faster network propagation
4. Stronger fork-choice guarantees through attestation timing
5. (implicit) removing the Flashbots-relay centralization point named above

**Core mechanic**: builders include payload headers (with their bid) directly in beacon blocks. Proposers monitor headers on-chain and select based on bid value — no off-chain relay needed to mediate the handoff. Once a builder is selected, "the bid from the block header is executed, the builder's MEV is extracted, and the builder reveals their execution payload."

**Accountability layer — Payload Timeliness Committee (PTC)**: a dedicated attestation committee oversees whether the winning builder actually delivered its payload promptly and honestly. The PTC's attestations feed into fork-choice weighting: **on-time, honest delivery earns the builder's block a 40% boost to its fork-choice weight** as chain head, while late/dishonest delivery can be penalized by the committee withholding that boost — protecting the proposer's payment guarantee (the paper notes "proposer payment will be issued regardless of a builder's performance," i.e. the proposer isn't the one who bears non-delivery risk).

**Builder staking requirement**: under ePBS, builders "act as a new leg of validation, earning their role a new staking requirement" — builders now have capital at risk in the protocol itself, rather than relying purely on relay-mediated reputation.

## MEV-management proposals discussed alongside ePBS (VERIFIED)

1. **Committee-driven smoothing** — attestation committees distribute MEV among their members to reduce block-to-block reward variance (dampens the incentive to fight over any single high-MEV slot).
2. **Burn auctions** — builders commit to burning ETH rather than paying the proposer directly, returning extracted value to all ETH holders collectively rather than concentrating it on whichever validator happened to propose that slot.
3. **Subjective burning** — builders burn a base fee set "somewhere below a builder's effective balance," with dedicated committees enforcing a minimum burn floor.

## Honest caveats

- **This is a systematization/survey paper, not new empirical measurement.** It does not itself quantify builder or relay market share (it references the qualitative "most notable relays are Flashbots" observation and the "90% of validators use MEV-Boost" adoption figure, but does not provide the block-share concentration numbers that [[foundation/sources/eth-builder-market-decentralization|the Yang/Nayak/Zhang paper]] does).
- **ePBS (EIP-7732) was a proposal, not a live mainnet mechanism, at the time of writing (June 2025).** This page does not know whether ePBS has since shipped, and the paper's critique of current MEV-Boost trust weaknesses should be read as "the problem ePBS is designed to solve," not as a description of a deployed fix.
- **Single-author paper**, conference-track SoK rather than a large empirical study — treat the architecture description as authoritative (it's describing publicly documented protocol mechanics) but the framing/emphasis as one author's synthesis.

## Implications for wiki priorities

- Supplies the **mechanism-design vocabulary** (proposer/builder/relay/searcher, ePBS, PTC, burn auctions) that [[foundation/asset-classes/eth-market-structure]] needs to describe the MEV supply chain structurally.
- Read together with [[foundation/sources/eth-builder-market-decentralization]]: this page explains **how the system is designed to work (and where the trust gaps are)**; that page measures **how concentrated and costly the gaps actually are in practice**.

## Related

- [[foundation/sources/eth-builder-market-decentralization]] — empirical companion (builder/relay concentration measured; proposer-loss quantified)
- [[foundation/asset-classes/eth-market-structure]] — synthesizes this paper's PBS/ePBS mechanics into the ETH structural picture
- [[foundation/sources/ethena-optimal-control-staking-basis]] — unrelated mechanism (staking-basis control) but same asset-class page ties both into the ETH structural synthesis

## Sources

- arXiv:2506.18189 (v1, Jun 22 2025), fetched in full via https://arxiv.org/html/2506.18189
- Abstract page: https://arxiv.org/abs/2506.18189
- Presented to The Science of Blockchain Conference 2025
