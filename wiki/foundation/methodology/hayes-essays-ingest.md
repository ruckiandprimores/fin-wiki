---
title: "Hayes Essays Ingest — Methodology + Findings Ledger"
status: seedling
created: 2026-05-08
updated: 2026-05-08
tags: [foundation, methodology, recurring-ingest, crypto-native, hayes, essay-archive]
---

# Hayes Essays Ingest — Methodology + Findings Ledger

> **TL;DR**: Recurring-cadence workflow for harvesting (a) **macro-overlay calibration anchors** and (b) **occasional new mechanism content** from Arthur Hayes's Substack archive (`cryptohayes.substack.com`). Lower-cadence sister to [[foundation/methodology/investopedia-daily-ingest|Investopedia daily-ingest]]: Hayes publishes ~2-3 essays/month vs Investopedia's daily articles, but per-essay mechanism density is higher. The methodology section documents the workflow; the findings ledger below the methodology accumulates entries reverse-chronologically. Initial ingest 2026-05-08 PM-2 (9 essays curated; see [[foundation/sources/hayes-essays]] for the source-page output and the first ledger entry below).

## Why This Page Exists

Per [[foundation/sources/hayes-essays|Hayes essays source page]], Hayes is the wiki's first crypto-native primary source — closes Worldview Prior #1 (Part I §1) on the contemporary-calibration axis (the historical-analog axis was filled by Lefèvre/Wyckoff/Kindleberger). The initial 2026-05-08 PM-2 ingest established a substrate of 9 curated essays. **New essays publish at ~2-3/month cadence**; without a recurring-ingest discipline, those new essays will accumulate uncaptured and the source's contemporary-calibration value will decay relative to actual current state.

This page parallels the Investopedia daily-ingest methodology with three key adaptations:

- **Cadence is 1-2 weeks**, not daily (matches Hayes's publishing rhythm)
- **Per-essay triage is load-bearing**: Hayes mixes mechanism-content essays with pure-punditry essays at roughly 1:1 ratio in the visible archive, and titles are misleading (metaphor-titled essays often have substantive mechanism content mid-essay). Triage cannot be skipped or automated.
- **Yield shape is opposite to Investopedia**: Investopedia has *frequent* articles with *low* per-article principle yield (mostly regime markers); Hayes has *infrequent* essays with *high* per-essay calibration yield (cross-link sharpenings + occasional new mechanism). Honest yield expectation: ~0-2 essays/month requiring wiki updates; yield comes in bursts not steady stream.

A recurring Hayes review is therefore **not "stay caught up" but "pull in calibration when Hayes sees something we should encode."**

## The Workflow Protocol

### Step 1 — New-essay detection (3 minutes)

**Primary path**: open `cryptohayes.substack.com/archive?sort=new` (default sort = newest). Check the visible top 5-10 essay tiles. Compare to the most-recent Findings Ledger entry below to identify *new* essays since last ingest.

**Note on archive limitation**: Substack archive pagination beyond page 1 is broken in the public view (page 2/5/8/12 all return the same recent tiles in our 2026-05-08 testing). This means the workflow can ONLY catch essays published *since* the last ingest — historical backfill via deeper archive pagination is not currently feasible.

**Cadence options**:
- **Per-essay** (lowest discipline): user pings me when they see a new Hayes essay drop. Lightest; relies on user ambient awareness.
- **Bi-weekly cadence**: every 1-2 weeks, run the full workflow; expect 1-3 new essays per cycle. Operationally healthy.
- **Monthly cadence**: every 4 weeks; expect 2-6 new essays. Risk of more new essays than triage budget allows.

**Recommended default**: bi-weekly. Hayes's ~2-3/month publishing rate produces 1-2 essays in a typical 2-week window — small enough to triage carefully, frequent enough to avoid backlog.

### Step 2 — Triage (5-15 minutes per essay)

Apply the rubric from [[foundation/sources/hayes-essays|hayes-essays source page]] §Curation:

**In-scope (proceed to extraction)**:
- Funding/basis mechanics, perp microstructure, positioning extremes
- Cascade events (post-mortems on specific cascade episodes)
- BTC vs alts rotation, halving cycle, ETF flow regime
- Dollar liquidity → crypto transmission
- Bank-issued stablecoin / treasury-flow / fiscal-dominance content
- New crypto-native mechanism content (perp DEX innovations, on-chain primitives, etc.)

**Out-of-scope (skip)**:
- Pure geopolitical commentary without mechanism implications
- Single-thesis political punditry (US/China/Trump-administration policy without crypto-mechanism connection)
- Personal anecdote essays without operational mechanism content
- Trade-thesis essays (do not extract specific trade ideas; only underlying mechanism framing)

**Stop condition (per-cycle)**: if 0 of cycle's new essays are mechanism-bearing, log "skip — punditry-only cycle" in ledger and move on. Don't force-extract from punditry.

**Critical caveat**: title-based triage substantially under-counts mechanism content. Hayes's stylistic signature is to open with metaphor / personal anecdote (skiing, dating, geopolitics) → pivot to mechanism analysis mid-essay → close with trade thesis. **A 2-minute spot-check WebFetch with prompt "does this essay discuss mechanism X, Y, Z?" is required before classifying as out-of-scope** for any essay where the title is ambiguous (most are).

### Step 3 — Content extraction (10-20 minutes per in-scope essay)

For each in-scope essay, extract:

1. **2-3 specific mechanism claims** with quoted sentences. Quotes are load-bearing — they're the auditable evidence that Hayes actually said this.
2. **1-2 calibration anchors**: specific dates, magnitudes, episodes Hayes references (e.g., "$1.2T treasury issuance Jan 2022 – Dec 2024"; "$2.5T drained from RRP via short-bill issuance"; "MOVE >130 trigger threshold"). These are the operational regime-markers; without them the calibration is unfalsifiable.
3. **One-line theme summary** suitable for the source-page Curated Essay Inventory table.
4. **Target wiki cross-link**: which existing wiki page benefits from this content?

Use the routing table below to map essay-theme → target page.

### Step 4 — Routing to wiki pages (5-15 minutes)

Each in-scope essay produces a **cross-link sharpening** on an existing wiki page (or, occasionally, a new wiki page). Per task spec: *Hayes adds episode evidence + practitioner narrative to existing mechanism pages, not new mechanism pages.*

**Routing table** (essay-theme → wiki-page target):

| Essay theme | Primary cross-link target | Sub-target |
|-------------|--------------------------|------------|
| **Dollar liquidity → BTC transmission** | [[foundation/macro/btc-regime-taxonomy-2020-2025]] §Hayes contemporary calibration | [[glossary/dollar-milkshake-thesis]] |
| **Perp funding mechanics** (Hayes design-history) | [[trading/mechanisms/funding-cycle-dynamics]] §Hayes contemporary calibration | [[glossary/funding-rate-carry]] + [[glossary/basis-trade-crypto]] |
| **Stablecoin → treasury flow** | [[foundation/macro/btc-regime-taxonomy-2020-2025]] (regime markers) + [[glossary/basis-trade-crypto]] | — |
| **Cycle calibration / multi-currency credit-impulse** | [[foundation/market-wisdom/bubble-cycle-stages]] §Hayes contemporary calibration | [[foundation/macro/btc-regime-taxonomy-2020-2025]] |
| **Cascade events / post-mortems** | [[trading/mechanisms/liquidation-cascade-aftermath]] §Source Pointers (Hayes line) | [[foundation/market-structure/bubbles-crashes-circuit-breakers]] |
| **Positioning extremes / squeezes** | [[trading/mechanisms/behavioral-positioning-unwind]] §Hayes contemporary calibration | [[trading/mechanisms/funding-cycle-dynamics]] |
| **Forward-cascade-trigger speculation** (Hayes thesis-mode essays) | [[trading/mechanisms/liquidation-cascade-aftermath]] §Source Pointers (forward-watch) | [[foundation/market-structure/bubbles-crashes-circuit-breakers]] |
| **Perp DEX vs CEX valuation** ($HYPE-style) | [[foundation/market-structure/dealer-economics]] | [[foundation/market-structure/microstructure-themes]] |
| **NEW mechanism not in routing table** | Surface as candidate-new-mechanism-page; flag for user review before creating | — |

**Cross-link sharpening shape**: append 2-5 lines to the target page's existing "## Hayes contemporary calibration" subsection (created in 2026-05-08 PM-2 initial ingest). Format:

```markdown
- "<Essay Title>" (<Month Year>) — <one-line mechanism claim with quoted anchor or specific number>. <Optional: cross-link or implication>.
```

If a target page does NOT yet have a "## Hayes contemporary calibration" subsection (e.g., bubbles-crashes-circuit-breakers, dealer-economics), CREATE one before "## Related" using the same pattern as the 5 existing subsections.

### Step 5 — Findings ledger entry + yield audit (3 minutes)

After processing the cycle's new essays, append a single entry to the Findings Ledger below covering all essays in this cycle. Format:

```
### YYYY-MM-DD — <cycle window>
**New essays since last ingest**: <count>
**In-scope (mechanism-bearing)**: <count>
**Out-of-scope (punditry / trade-thesis only)**: <count>
**Cross-link sharpenings landed on**: <comma-separated wiki pages>
**Calibration anchors added**: <count>
**New mechanism candidates surfaced**: <count, or "none">
**Yield**: brief assessment

[Per-essay details below if any in-scope essays this cycle]
```

**Yield expectations** (calibrated against initial 2026-05-08 PM-2 ingest):
- Most cycles: 1-2 essays in-scope; 1-2 cross-link sharpenings added; 0 new mechanism candidates
- Occasional cycle: 0 essays in-scope (pure punditry cycle) — log skip and move on
- Rare cycle: a foundational mechanism essay (e.g., another "Adapt or Die"-class design narrative) — may warrant new wiki page

If yield is consistently zero over 3+ cycles, **revisit the protocol** — Hayes may have shifted his content mix away from mechanism content (this happens for political-cycle reasons; it has happened in 2024 election runs historically per secondary sources).

## Findings Ledger Structure

Append-only, reverse-chronological (newest at top). The 2026-05-08 PM-2 initial ingest is the canonical first entry pattern.

## Findings Ledger

### 2026-05-08 PM-2 — Initial ingest (canonical example)

**Cycle window**: full visible Substack archive (2024-late through 2026-04). One-time foundational ingest, not a recurring-cycle entry.

**New essays since last ingest**: 18-20 visible (broken pagination beyond page 1 caps the view)
**In-scope (mechanism-bearing)**: 9 (Adapt or Die, Hallelujah, The BBC, Quid Pro Stablecoin, Long Live the King!, This Is Fine, Fatty Fatty Boom Boom, Frowny Cloud, $HYPE Man)
**Out-of-scope (punditry / trade-thesis only)**: ~10 (No Trade Zone, iOS Warfare, Woomph, Suavemente, Love Language, Snow Forecast, KISS of Death, Trump Truth, The Ugly, Ski Cut, The Genie)

**Cross-link sharpenings landed on**:
- [[trading/mechanisms/funding-cycle-dynamics]] — Adapt or Die primary-source narrative
- [[trading/mechanisms/behavioral-positioning-unwind]] — ADV/OI + MOVE>130 calibration
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — 5-marker dollar-liquidity overlay table
- [[foundation/market-wisdom/bubble-cycle-stages]] — multi-currency credit-impulse refinement
- [[trading/mechanisms/liquidation-cascade-aftermath]] — forward-cascade-trigger watch (This Is Fine)

**New pages created**: 4 (1 source + 3 glossary)
**Calibration anchors added**: ~12 (MOVE >130, SRF $1.2T treasury intermediation, RRP→T-bill ~$2.5T, 2015-17 yuan-credit-impulse, $557B AI-displacement scenario, $6.8T stablecoin/T-bill capacity, IORB elimination $3.3T, P/E 12x vs CME 26-40x, 2% capital-controls tax on $33T, etc.)
**New mechanism candidates surfaced**: 0 specifically — all 9 essays mapped to existing wiki pages per routing table; no genuinely-new-mechanism-not-in-wiki content emerged

**Yield**: foundational/high — first ingest sweeps years of accumulated content in one pass, so yield is biased high vs steady-state. Future bi-weekly cycles will surface 0-2 in-scope essays per cycle.

**Reduced scope acknowledged**: Pre-2024 BitMEX-blog classics (Dollar Milkshake / Maelstrom intro / 2020-2023 cascade post-mortems) inaccessible (404 / blog migrated / Substack pagination broken). Wayback Machine fallback not attempted in this initial ingest. Future-cycle option: investigate Wayback Machine archives if user prioritizes the historical-cascade-narrative gap.

**Per-essay extraction details**: see [[foundation/sources/hayes-essays|hayes-essays source page]] §Curated Essay Inventory + §Mechanism Themes Extracted. Not duplicated here to avoid bloat.

**Action items from this ingest**:
- ✅ All 9 essays routed; source page + 3 glossary entries + 5 cross-link sharpenings landed
- ✅ This methodology page created to support recurring future ingests
- ⚠ **Companion crypto-native sources flagged for follow-up** (separate ingest cycle, lower priority): CoinMetrics State of the Network, Glassnode research / Insights, arXiv q-fin crypto microstructure papers — for mutual-confirmation discipline given single-author lineage risk
- ⚠ **Wayback Machine backfill option** flagged but not pursued — would require ~10-15 exploratory fetches with uncertain payoff; queue as user-discretion task
- ⚠ **Capability roadmap implication**: Hayes's dollar-liquidity overlay markers (MOVE, SRF, RRP, FRED-style macro) suggest a capability-roadmap-position-#10 candidate for macro-data feed (FRED API + bond vol indices) beyond crypto-internal feeds; flag for next capability-roadmap revision

---

*New entries should be added above this initial example. Maintain reverse-chronological order.*

## Honest Caveats / Failure Modes

- **Single-author lineage risk** persists. Hayes is one practitioner; the wiki's behavioral-pre-regulation cluster has multi-author confirmation (Lefèvre + Wyckoff + Kindleberger). Recurring Hayes ingest *amplifies* this risk by accumulating Hayes's framings without parallel counter-perspective. **Mitigation**: queue companion crypto-native sources (CoinMetrics, Glassnode research, arXiv crypto microstructure) as recurring-ingest companions before the wiki gets Hayes-overweight on the crypto-native axis.
- **Confirmation-bias accumulation**. Hayes is dollar-bearish and BTC-bullish by long-running thesis. Recurring ingest of his work will steadily accumulate dollar-bearish framings. **Mitigation**: at each cycle, flag any framing that depends on Hayes's macro thesis being correct and tag it explicitly as forward-looking-not-validated in the cross-link sharpening. The wiki's role is mechanism extraction, not thesis adoption.
- **Trade-thesis bleed risk**. Hayes regularly publishes specific trade ideas (target prices, sizing recommendations, position calls). The methodology *explicitly excludes* these from extraction, but in practice trade-thesis content is often woven into mechanism essays (e.g., "Adapt or Die" closes with trade thesis after the perp-mechanism history). **Mitigation**: the extraction discipline is "mechanism framing only" — when Hayes writes "based on this mechanism, I'm long X," extract the mechanism but explicitly drop the long-X. Audit the cross-link sharpenings periodically to ensure trade-thesis content didn't bleed through.
- **Forward-looking thesis vs validated history**. Several mechanism claims Hayes makes are *predictions* (asymmetric QT magnitude, AI-displacement cascade timing, capital-controls tax revenue projection). The methodology should treat these as **regime-watch inputs** (worth tracking when the predicted condition appears), not as established mechanisms. Honest cross-link sharpenings should tag forward-looking content explicitly.
- **Cadence-discipline drift**. The Investopedia methodology has been used twice (2026-05-04 entries) since creation — that's 2 entries in ~4 days, which is below daily cadence and suggests discipline-drift risk. The Hayes methodology has a more forgiving 1-2 week cadence, but the same drift risk applies. **Mitigation options**: (a) accept opportunistic cadence (better than no cadence); (b) use Claude Code hooks / scheduled-agent infrastructure to schedule remote-agent reminders; (c) couple Hayes-ingest cadence to Investopedia-ingest cadence (do both same session bi-weekly).
- **Substack archive limitations persist**. Pagination broken; foundational pre-2024 essays inaccessible. The methodology cannot retroactively fix this — it only catches new essays going forward. The historical gap remains until/unless Wayback-Machine backfill is pursued separately.
- **"Hayes is repeating himself" failure mode**. After 3-6 months of ingests, Hayes is likely to repeat thematic content (dollar liquidity overlay restated; fiscal dominance restated). New-calibration-anchor yield will drop to near-zero on most cycles; the value becomes *update calibration anchors with new specific numbers* (e.g., "MOVE>130 trigger threshold" → "MOVE>140 in current regime per Q3 essay"). **Mitigation**: shift cycle-yield audit to "did Hayes update specific numbers?" rather than "did Hayes introduce new mechanism?"
- **Source-quality regression risk**. Hayes may shift his content mix away from mechanism toward punditry over time (this is observable in late-2024 / early-2025 essays vs his 2022-2023 output per secondary references). If the in-scope count drops to <30% over 3+ consecutive cycles, **flag the source for reduced-priority** — use to confirm what's already in wiki rather than expecting new calibration.

## How to Operationalize the Cadence

Three paths from least-to-most operationally heavy:

### Path A — Manual bi-weekly (lightest)

User invokes me every 1-2 weeks: "do the Hayes ingest." I:
1. Fetch `cryptohayes.substack.com/archive?sort=new`
2. Compare to most-recent ledger entry; identify new essays
3. Triage per Step 2 rubric
4. Extract per Step 3 from in-scope essays
5. Apply cross-link sharpenings per Step 4 routing table
6. Append ledger entry per Step 5

Pros: simple; no infrastructure; user controls when. Cons: requires user discipline; tendency to lapse; bi-weekly is more forgiving than daily but still drift-prone.

### Path B — Couple to Investopedia cadence (medium)

If Investopedia-ingest discipline is being maintained (per [[foundation/methodology/investopedia-daily-ingest|investopedia-daily-ingest]]), couple Hayes-ingest to that workflow: every ~14th Investopedia ingest, also run Hayes ingest. Operationally one coupled-task.

Pros: leverages an existing discipline rather than creating a new one; bi-weekly cadence emerges naturally. Cons: depends on Investopedia discipline holding (which has its own drift risk).

### Path C — Scheduled remote agent (heaviest)

Using the `schedule` skill, set up a remote agent to run bi-weekly (e.g., every other Sunday 23:00 UTC) that:
1. Fetches Substack archive
2. Compares against latest ledger entry to detect new essays
3. For each new essay: WebFetch + apply Step 2 triage rubric
4. For in-scope essays: extract per Step 3, apply Step 4 routing
5. Append ledger entry; surface to user in next session

Pros: most reliable; doesn't require user discipline. Cons: requires scheduling infrastructure; agent triage may be lower-quality than human-loop triage (Hayes's metaphor-titled style is harder for an autonomous agent to disambiguate); remote agent should be coupled to a user-review step rather than auto-publishing changes.

**Recommended default for now**: Path A (manual bi-weekly) until cadence pattern is established; revisit toward Path C if cadence holds for 2+ months.

## Related

- [[foundation/sources/hayes-essays]] — the source page this methodology operationalizes
- [[foundation/methodology/investopedia-daily-ingest]] — sister methodology page; daily-cadence equivalent
- [[glossary/dollar-milkshake-thesis]] — defined here
- [[glossary/basis-trade-crypto]] — defined here
- [[glossary/funding-rate-carry]] — defined here
- [[trading/mechanisms/funding-cycle-dynamics]] — primary cross-link target for perp/funding essays
- [[trading/mechanisms/behavioral-positioning-unwind]] — primary cross-link target for positioning essays
- [[trading/mechanisms/liquidation-cascade-aftermath]] — primary cross-link target for cascade essays
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — primary cross-link target for dollar-liquidity overlay essays
- [[foundation/market-wisdom/bubble-cycle-stages]] — primary cross-link target for cycle/credit-impulse essays
- Part I §1 — Worldview Prior #1 (crypto ≈ pre-1980s US equities); Hayes is the contemporary-calibration counterpart to the historical-analog cluster
