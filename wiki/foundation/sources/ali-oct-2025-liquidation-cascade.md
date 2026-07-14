---
title: "Anatomy of the Oct 10-11 2025 Crypto Liquidation Cascade (Ali 2025)"
status: growing
created: 2026-06-26
updated: 2026-07-12
tags: [source, academic, preprint, liquidation-cascade, event-driven, macro-trigger, systemic-risk, garch, case-study, day-swing-applicable]
---

# Anatomy of the Oct 10-11 2025 Crypto Liquidation Cascade (Ali 2025)

> **TL;DR**: A paper-grade anatomy of the **largest liquidation event in crypto history** (~$19B forcibly closed, >1.6M traders) — triggered by a discrete macro shock (a 100% China-tariff post to Truth Social ~2 p.m. ET Oct 10 2025) that interacted with a record ETF-era leverage stack. Frames the episode as a **mixed microstructure + macro-fundamental cascade** (reflexive leverage→liquidity→volatility feedback), not pure fundamental repricing, with GARCH/DCC-GARCH + event-study evidence. The **full worked case study already lives in [[../../trading/mechanisms/liquidation-cascade-aftermath#7-october-10-11-2025-etf-era-19b-tariff-triggered-deleveraging-cascade|liquidation-cascade-aftermath §7]]**; this page is the formal source citation.

## Citation

Zeeshan Ali (University of Central Asia, Khorog, Tajikistan), *Anatomy of the October 10–11, 2025 Crypto Liquidation Cascade: Macroeconomic Triggers, Market Microstructure, and Systemic Risk Lessons*, SSRN Working Paper 5611392 (Oct 16 2025). Ingested via user-supplied PDF 2026-05-15 (PDF-grade) → re-surfaced as a formal source 2026-06-26.

## Position in lineage

The N=7 / "third-category" anchor of the cascade-aftermath mechanism family. Where [[he-manela-fundamentals-perpetual-futures|He-Manela]] supplies the positive-feedback momentum mechanism and [[bouchaud-trades-quotes-prices|Bouchaud]] the impact/liquidity theory, Ali supplies the **macro-to-microstructure transmission case study** — a dated, quantified worked example of a detector firing. Contemporaneous with the start of [[../macro/btc-regime-taxonomy-2020-2025|Regime 15]] (the Oct-2025 ATH → 2026-H1 bear).

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: event-driven (macro shock) + liquidation cascade (microstructure / forced flow) + macro-regime.
- **Axis 2 — Driver**: forced flow (margin calls + exchange auto-liquidation engines) + coordination problem (everyone exits at once) on a record leverage stack.
- **Axis 3 — Crypto applicability**: **direct** — crypto-specific cascade dynamics; the ETF-era leverage stack is the amplifier.

## Key findings

- **Scale**: ~$19B liquidations, >1.6M traders; BTC −14% to $104,782; ETH −15% to $3,447; S&P 500 −2.7%. Per-asset peak liquidations BTC $9.2B / ETH $4.8B / SOL $2.3B (BTC+ETH ≈ 74%). ~$7B hourly liquidation peak.
- **Trigger chain**: Trump 100% China-tariff post (~2 p.m. ET Oct 10) → BTC **conditional variance peaked at 0.10 shortly after 2 p.m.** — direct empirical confirmation of the tariff→vol-spike→cascade chain. BTC OI had peaked at $54.7B (82% above year-start — the largest aggregate BTC leverage on record).
- **Econometrics**: GARCH(1,1) α+β ≈ 0.92 across 10 cryptos (high persistence); DCC-GARCH cross-asset correlation **0.55 pre → 0.89 post** (BTC-ETH peak 0.91 within 12h); contagion ~15% stronger than the 2018 trade war. Event-study: tariff dummy **β=0.62, p<0.001** on BTC volatility, R²=0.52; **placebo test p=0.52** supports causality.
- **Author's proposal**: a **Tariff-Liquidity Index** `TLI = β₁·ΔTariff + β₂·ΔLiquidity + ε` as a geopolitical-risk monitor.

## Why it matters / honest scope

This is the wiki's strongest **paper-grade validation of the cascade-event detector** — the episode breached **4-5 of 5** pre-registered detector thresholds (see [[../../trading/mechanisms/liquidation-cascade-aftermath#7-october-10-11-2025-etf-era-19b-tariff-triggered-deleveraging-cascade|§7]]), and it forced the **third-category** (mixed microstructure + macro) extension of the Harris binary. **Honest scope (verifier-flagged):** single-author, **non-peer-reviewed working paper** — the *event* is independently corroborated (CoinDesk / CNN / CoinGecko: ~$19B, ~1.6M traders, the BTC path), but the *econometrics and the TLI* are the author's and unreplicated. Ingest as a quantified case study + candidate diagnostic, not a peer-reviewed authority.

## Related

- [[../../trading/mechanisms/liquidation-cascade-aftermath]] — §7 is the full worked case study this page cites
- [[../macro/btc-regime-taxonomy-2020-2025]] — Oct 2025 ATH → Regime 15; this cascade is the opening event
- [[../macro/treasury-vehicle-cascade-watch]] — the forced-selling/leverage-overhang companion watch
- [[he-manela-fundamentals-perpetual-futures]] — positive-feedback momentum mechanism beneath the cascade
- [[hyblock-coinglass-liquidation-heatmaps]] — how the heatmaps that visualize the leverage stack this cascade cleared are constructed (modeled estimates; over-count by design)
- [[bouchaud-trades-quotes-prices]] — impact/liquidity-fragility theory
- [[two-tiered-funding-rate-markets]] / [[temporal-dynamics-funding-rates]] — ETF-era venue concentration as correlated-cascade risk

## Sources

- [SSRN 5611392](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5611392) (Ali 2025) · event independently corroborated (CoinDesk / CNN / CoinGecko).
- PDF-grade content ingested 2026-05-15; deep-research adversarial pass (2026-06-26): scale + macro-trigger + structured-anatomy framing confirmed 3-0; single-author non-peer-reviewed flagged.
