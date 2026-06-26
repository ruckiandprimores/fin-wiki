---
title: "Deribit Insights + Block Scholes — Crypto Options/Vol Research"
status: growing
created: 2026-06-07
updated: 2026-06-07
tags: [source, crypto-native, options, volatility, vrp, dvol, vol-regime, skew, term-structure, deribit, block-scholes, day-swing-applicable]
---

# Deribit Insights + Block Scholes — Crypto Options/Vol Research

> **TL;DR**: **Deribit Insights** is the single best *free* crypto-native options/vol research source — public research essays + education + the weekly **Block Scholes** "Crypto Derivatives Analytics Report" (free, mirrored on Deribit). Mechanism-bearing, not just dashboards. The flagship piece for this wiki — *"Bitcoin Options: Finding edge in four years of volatility regimes"* — classifies vol regimes by **term-structure state** (contango ~77.5% of the time), spot-vol correlation, and skew, and quantifies the variance risk premium (~**+15 vol points** of implied-over-realized overpricing in contango). Block Scholes' **Senti-Meter / Risk Appetite Index** is a ready-made regime composite aggregating **funding rate + implied yield + vol-smile skew** into one sentiment-regime read. **Why it matters**: closes the wiki's VRP/options capability gap *on the concept side* (the Deribit feed itself remains a separate data-capability decision) and contributes a **vol-state regime axis** (term-structure contango/backwardation) to the regime workstream. First crypto-native *options/vol* primary source — companion to [[foundation/sources/hayes-essays|Hayes]] (macro/funding) and the academic perp cluster.

## Citation / access

- **Deribit Insights**: https://insights.deribit.com — free public research, education, exchange updates.
- **Block Scholes Research**: https://www.blockscholes.com/research — weekly Crypto Derivatives Analytics Report free (also mirrored on Deribit Insights); deeper data/API paywalled.
- Flagship: [Bitcoin Options: Finding edge in four years of volatility regimes](https://insights.deribit.com/industry/bitcoin-options-finding-edge-in-four-years-of-volatility-regimes/).
- DVOL: [Demystifying DVOL Futures](https://insights.deribit.com/industry/demystifying-dvol-futures/); [DVOL — Deribit Implied Volatility Index](https://insights.deribit.com/exchange-updates/dvol-deribit-implied-volatility-index/).
- Representative weekly: [Crypto Derivatives Analytics Report — Week 8 2026](https://insights.deribit.com/industry/crypto-derivatives-analytics-report-week-8-2026/).

## Position in Lineage

**This source builds on:**
- [[foundation/sources/sinclair-volatility-trading|Sinclair (2013)]] — general vol-trading mechanics (vol cone, variance premium, skew, term structure); Deribit/Block Scholes apply these to crypto with live anchors
- [[foundation/sources/hayes-essays|Hayes]] — crypto-native companion on the macro/funding side

**This source influenced (in this wiki):**
- [[trading/mechanisms/variance-risk-premium-crypto]] — crypto-anchored VRP magnitude + regime-conditioning
- [[trading/mechanisms/vol-regime-detection]] — term-structure-state vol-regime classification
- [[trading/mechanisms/gamma-exposure-regime]] — vol-regime companion
- [[trading/mechanisms/options-gamma-perp-effects]] — DVOL / dealer-flow context

**Concept lineage map — concepts this source contributes:**
- Vol-regime classification by **term-structure state** (contango ~77.5% / backwardation)
- Crypto VRP magnitude anchor (~+15 vol pts overpricing in contango)
- DVOL index + DVOL-futures mechanics (concept side of the wiki's DVOL capability gap)
- Block Scholes **Senti-Meter** — funding + implied-yield + skew composite regime indicator

## 3-Axis Tags

- **Economic mechanism**: Volatility risk premium; macro/regime (vol-state classification); liquidity provision (options market making)
- **Behavioral/structural driver**: Information asymmetry (dealer-side flow non-replicable by retail); forced flow (dealer hedging)
- **Crypto applicability**: Direct (entirely crypto options/vol; Deribit is the dominant BTC/ETH options venue)

## Key content for the wiki

### 1. Vol regimes by term-structure state (flagship piece)

- **Contango ~77.5% of the time** — implied-vol term structure is upward-sloping the large majority of the time; backwardation is the event/stress signal.
- **VRP ~+15 vol points** of implied-over-realized overpricing in contango → the harvestable premium ([[trading/mechanisms/variance-risk-premium-crypto]]).
- Vol regimes classified via **term-structure shape + spot-vol correlation + skew** — a vol-state-axis classifier complementary to the realized-vol cone in [[trading/mechanisms/vol-regime-detection]].
- Mechanism playbook by regime: straddle-selling (contango/calm), risk-reversal + butterfly (skew regimes), event-driven long-vol (pre-event backwardation).

### 2. DVOL — concept side of the capability gap

DVOL (Deribit's 30-day implied-vol index) + DVOL futures give the IV side that [[trading/mechanisms/vol-regime-detection]] and [[trading/mechanisms/variance-risk-premium-crypto]] need. Ingesting the *concept* now (definition, methodology, what the futures price) sharpens the later Deribit-feed buy decision — one feed unlocks DVOL + per-strike IV + GEX + VRP together.

### 3. Block Scholes Senti-Meter — a ready-made regime composite

Aggregates **funding rate + implied yield + vol-smile skew** into a single sentiment-regime expression — spans the funding ([[trading/mechanisms/funding-cycle-dynamics]]), vol, and skew axes at once. A candidate composite regime signal to compare against the [[foundation/methodology/regime-marker-data-sources|backtestable marker core]] and the alternative.me F&G baseline.

## Honest scope / limitations

- **Weekly reports are snapshots** — ingest the *methodology* (how they classify regimes / construct Senti-Meter), not the timely market calls. Per the wiki's recurring-ingest discipline (cf. [[foundation/methodology/hayes-essays-ingest]]), these are commentary, not evergreen mechanism unless the construction logic is extracted.
- **Concept vs feed** — the research is free and ingestable; the underlying per-strike IV / DVOL *data* is a separate capability/buy decision (Deribit feed).
- **Single-venue lens** — Deribit-centric; reflects the dominant but not the entire crypto options market.

## Related

- [[trading/mechanisms/variance-risk-premium-crypto]] — primary downstream consumer (VRP magnitude + regime-conditioning)
- [[trading/mechanisms/vol-regime-detection]] — term-structure-state classification
- [[trading/mechanisms/gamma-exposure-regime]] — dealer-gamma vol-regime companion
- [[trading/mechanisms/options-gamma-perp-effects]] — pinning/flush; DVOL context
- [[foundation/sources/sinclair-volatility-trading]] — the general-vol predecessor this applies to crypto
- [[foundation/methodology/regime-marker-data-sources]] — where the DVOL/options data sits in the source map (Deribit feed = paid capability)
- [[foundation/sources/lineage]] — source graph

## Sources

- Deribit Insights + Block Scholes pages as cited above (fetched 2026-06-07). Abstract/essay-grade extraction; full quantitative report tables not deep-extracted this pass.
