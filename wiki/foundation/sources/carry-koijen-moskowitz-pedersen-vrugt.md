---
title: "Carry (Koijen, Moskowitz, Pedersen & Vrugt 2018)"
status: growing
created: 2026-06-26
updated: 2026-06-26
tags: [source, academic, carry, cross-market-transfer, funding-rate, mechanism, factor, day-swing-applicable]
---

# Carry (Koijen, Moskowitz, Pedersen & Vrugt 2018)

> **TL;DR**: The canonical generalization of *carry* beyond FX. Defines carry as the return a security earns **if its price stays unchanged** — a **model-free, ex-ante** characteristic — and shows it predicts returns **both cross-sectionally and over time** across global equities, bonds, currencies, commodities, US Treasuries, credit, and index options. A generic long-high-carry / short-low-carry strategy earns strong returns in every asset class studied. This is the strongest **cross-market-transfer** anchor not yet in the wiki: it is the structural parent of the crypto **perp funding** mechanism (perp funding *is* the spot-vs-perp cost-of-carry).

## Citation

Ralph Koijen, Tobias Moskowitz, Lasse Heje Pedersen, Evert Vrugt, *Carry*, **Journal of Financial Economics** 127(2):197-225 (2018). SSRN 2298565; NBER w19325; AQR.

## Position in lineage

The wiki already operationalizes funding-rate carry ([[../../trading/mechanisms/funding-cycle-dynamics]], [[../../trading/mechanisms/delta-neutral-funding-harvest]]) and has the FX-carry analog ([[forex-10-essentials]]) plus the crypto-specific perp-funding academic work ([[he-manela-fundamentals-perpetual-futures]]). This paper supplies the **unifying mechanism** beneath all of them — carry as one cross-asset signal — making the crypto-funding-as-carry bridge explicit and theory-grounded rather than analogical.

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: carry / yield differential (the canonical statement of it).
- **Axis 2 — Driver**: structural risk premium — carry compensates for exposure to global recession / drawdown risk (it has its own crash risk, like FX carry).
- **Axis 3 — Crypto applicability**: **mutation/transfer** — perp funding is the crypto instantiation of cost-of-carry; the mechanism is structural, not regime-fragile.

## Key findings

- **Carry is model-free and ex-ante**: the return assuming price is unchanged — computable in advance from the term structure / forward, no return model required.
- **Predicts cross-sectionally AND in time series** across equities, bonds, currencies, commodities, Treasuries, credit, options — "studied almost exclusively in currency markets... a unifying framework."
- A diversified cross-asset carry strategy earns high risk-adjusted returns; carry is **distinct from but complementary to value and momentum**.
- Carry has **downside/crash risk** concentrated in global-recession / risk-off states — it is a risk premium, not a free lunch.

## Why it matters / honest scope

**The cross-market-transfer keystone for the funding-rate family:** it says the edge the wiki's funding strategies harvest is one instance of a cross-asset structural premium — which both legitimizes the mechanism (not crypto-luck) and warns of its failure mode (carry unwinds in synchronized risk-off, exactly when crypto de-risks). **Honest scope:** carry's standalone effect sizes are real but modest (see [[../../trading/mechanisms/funding-cycle-dynamics|the funding-cycle academic anchors]] for the crypto-carry Sharpe trajectory and its decay); the value is the *mechanism unification*, not a turnkey signal. Pairs with the commodity-carry data point ([Macrosynergy / Gorton-Rouwenhorst] — cite-only supplementary).

## Related

- [[../../trading/mechanisms/funding-cycle-dynamics]] — crypto perp funding = the carry instantiation
- [[../../trading/mechanisms/delta-neutral-funding-harvest]] — delta-neutral carry capture
- [[he-manela-fundamentals-perpetual-futures]] / [[borri-liu-tsyvinski-wu-investable-asset-class]] — crypto-specific carry empirics (decay trajectory)
- [[forex-10-essentials]] — the FX-carry analog carry generalizes from
- [[../../trading/mechanisms/insurance-premium-harvest-overview]] — carry sits within the premium-harvesting family

## Sources

- [SSRN 2298565](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2298565) · JFE 127(2):197-225 · NBER w19325
- Verified via deep-research adversarial pass (2026-06-26): the carry definition + cross-asset-predictability claims confirmed 3-0 (abstract verbatim). Abstract/research-verified grade; full-PDF deep-dive optional.
