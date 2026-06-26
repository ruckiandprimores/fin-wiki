---
title: "Crypto Wash Trading (Cong, Li, Tang & Yang 2023)"
status: growing
created: 2026-06-26
updated: 2026-06-26
tags: [source, academic, manipulation-aware, wash-trading, forensics, data-quality, volume, benford, day-swing-applicable]
---

# Crypto Wash Trading (Cong, Li, Tang & Yang 2023)

> **TL;DR**: Peer-reviewed forensic study (Management Science 2023; NBER w30783; Yale Cowles) that detects fabricated ("wash") trading by testing whether reported trade data obey three statistical regularities of *authentic* trades — **Benford's-Law first-digit distribution, round-number clustering, and a power-law (Pareto) size tail**. Headline magnitude: wash trading averages **~77.5% (mean) / ~79.1% (median)** of reported volume on **unregulated** exchanges (>80% on Tier-2), while **regulated** venues show normal patterns. This is the wiki's first rigorous *manipulation-aware data-quality* anchor — it directly impairs any volume-based day/swing signal.

## Citation

Lin William Cong, Xi Li, Ke Tang, Yang Yang, *Crypto Wash Trading*, **Management Science** 69(11):6427-6454 (2023). NBER Working Paper w30783; [Yale Cowles PDF](https://cowles.yale.edu/sites/default/files/2022-11/cryptowashtrading040521-crypto-wash-trading.pdf).

## Position in lineage

Fills the **manipulation-aware** branch with a concrete, falsifiable forensic methodology (prior coverage was conceptual — [[../market-structure/market-manipulation-taxonomy|manipulation taxonomy]]). Also a **data-quality gate** feeding the methodology layer: reported volume is an unreliable input on many venues, which bears directly on [[../methodology/deployment-edge-threshold|net-edge estimation]] and any volume-conditioned mechanism.

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: manipulation-aware (fabricated volume) + microstructure (the tape's statistical signature).
- **Axis 2 — Driver**: regulatory friction — unregulated venues fake volume to climb rankings / attract listings; regulated venues don't.
- **Axis 3 — Crypto applicability**: **direct** — crypto exchange tape; the methodology is a venue/data-vetting screen.

## Key findings (the methodology)

Authentic trade-size data obey three regularities that fabricated trades violate:
1. **Benford's Law** — the distribution of first significant digits of trade sizes.
2. **Round-number clustering** — humans/real order flow cluster at round sizes in a characteristic way.
3. **Power-law / Pareto-Lévy tail** — the size distribution has a fat power-law tail.

Per-exchange deviation from these gives a **falsifiable** wash-trading estimate. Magnitudes: **~77.5% mean / 79.1% median** of volume fake on unregulated exchanges; **>80%** on Tier-2; regulated exchanges (e.g. the major venues actually used for trading) show **normal** patterns.

## Why it matters / honest scope

**Load-bearing for the trading branch:** volume is an input to many day/swing signals (breakouts, Wyckoff effort-vs-result, liquidation reads) — if 77%+ of a venue's volume is fake, the signal is contaminated. Use = **venue/volume-quality vetting** before trusting a volume signal. **Honest scope (verifier-flagged):** this is an **exchange-level batch forensic screen**, *not* a real-time per-trade entry signal; the magnitude estimates are ~2019-20 vintage; and the regulated venues that matter operationally showed clean patterns, so the practical takeaway is "vet the venue + the data feed," not "don't trade." Open question carried: is there a post-2020 magnitude update or an on-tape real-time variant?

## Related

- [[../market-structure/market-manipulation-taxonomy]] — conceptual manipulation catalog this operationalizes
- [[../../trading/mechanisms/volume-analysis]] — the volume signals this data-quality gate guards
- [[../methodology/deployment-edge-threshold]] — friction/edge estimation depends on clean volume
- [[../market-structure/insider-trading]] — adjacent crypto-tape transparency themes

## Sources

- [Cowles PDF](https://cowles.yale.edu/sites/default/files/2022-11/cryptowashtrading040521-crypto-wash-trading.pdf) · Management Science 69(11):6427-6454 · NBER w30783
- Verified via deep-research adversarial pass (2026-06-26): three-regularity methodology + 77.5%/79.1%/>80% magnitudes confirmed verbatim, 3-0. Full-PDF deep-dive pending (abstract/research-verified grade).
