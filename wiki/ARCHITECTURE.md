---
title: "Wiki Architecture — Schema, Conventions & Domain Principles"
status: evergreen
created: 2026-06-26
updated: 2026-06-26
tags: [meta, architecture, schema, conventions, contributing]
---

# Wiki Architecture

> **What this is**: the design of this knowledge base — its structure, page conventions, the domain principles that govern what goes in it, and the workflows for growing it. Read this first if you want to understand how the wiki is built or contribute to it.
>
> This is a public, living research wiki on **trading** (day/swing horizon) and **investing** (long-term conviction). It is a learning accumulator and an idea-generation engine — a record of what was tried, what carried signal, and what died and why.

## One wiki, two branches

A shared foundation feeds two distinct branches:

```
foundation/     ← Shared: market wisdom, structure, macro, sources
    ↓
├── trading/    ← Day/swing: mechanisms, backtest results, rejected ideas
└── investment/ ← Long-term: conviction theses, allocation, watchlist
```

**Why one wiki, not two:** cross-pollination is the value — trading observations inform investment timing; investment theses inform trading regime detection. One daily habit beats two under-maintained ones. Structure emerges from content.

## Directory structure

```
wiki/
├── ARCHITECTURE.md              # This file — schema + domain principles
├── index.md                     # Master catalog (navigation)
├── foundation/                  # SHARED KNOWLEDGE
│   ├── market-wisdom/           # Behavioral classics, timeless patterns
│   ├── market-structure/        # Price discovery, manipulation, flow
│   ├── asset-classes/           # Crypto, FX, commodities, equities
│   ├── macro/                   # Rates, dollar, liquidity cycles, regimes
│   ├── methodology/             # Validation discipline, research process
│   └── sources/                 # Annotated literature catalog
├── trading/                     # BRANCH 1: Day/Swing
│   ├── mechanisms/              # Mechanism families, entry/exit logic
│   ├── backtest-results/        # What worked, performance data
│   └── rejected/                # Graveyard — ideas that failed + why
├── investment/                  # BRANCH 2: Long Horizon
│   ├── theses/ · allocation/ · watchlist/ · invalidated/
├── questions/                   # Open questions index (research priorities)
├── glossary/                    # Concept definitions
└── templates/                   # Page templates
```

---

## What belongs here — the two-genre rule

This is a **knowledge base**, and its job is to cut through financial noise — not to add to it. Every page answers exactly one of two questions, and the reader always knows which:

1. **How does this work / what's true?** → durable **knowledge** (foundation, mechanisms, allocation frameworks, theses, glossary).
2. **How do you analyze *this instrument*?** → **analysis** / case study (the instrument analyses under `investment/instruments/`).

A third question — **what should I do *right now*?** — does **not** belong here. A BUY / WAIT / tranche / sizing verdict, or a live daily-state readout, is a **decision**, and decisions (plus live position/state) are the **strategist's operational domain**, kept outside this knowledge base. (The old wiki state cards were moved out to the operational layer on 2026-07-14 for exactly this reason.)

**The line, concretely:**
- **Analysis (in):** mechanism, thesis logic, risk map, falsifier, and how to *read* a valuation multiple. Durable — it doesn't rot.
- **Decision (out):** "buy/wait/avoid at these levels," tranche, sizing, live position or P&L.
- An instrument page **may** carry a **dated, cordoned** valuation snapshot as *illustration of how to read a multiple* — under a `## Snapshot (as of DATE — perishable, illustration only)` heading — **never** as an entry recommendation.

**Why:** the wiki is meant to be shareable (build-in-public). A public buy call is the noise; the teachable, durable value is the *method*. Framed this way, the instrument analyses are the investing-side equivalent of the `trading/rejected/` graveyard — worked examples of reasoning, not tips.

---

## Part I — Domain principles (the knowledge core)

These are the load-bearing convictions that govern what counts as a good page. They compound across sessions; read them before doing strategy research.

### 1. Market character — crypto ≈ pre-1980s US equities

Crypto today resembles pre-regulation US equity markets more than current equities: retail-dominated, lightly regulated, manipulation common, flow-driven, high information asymmetry, weak market-maker obligations.

**Implication:** pre-modern-regulation trading wisdom (Livermore, Wyckoff, Edwards & Magee) is *more* applicable than 21st-century institutional quant literature. Tape reading, Wyckoff accumulation/distribution, stop-hunting, squeeze plays, and (legal, on-chain-visible) insider-flow tracking all have crypto analogs.

### 2. AI's structural limitations in idea generation

AI-generated trading ideas drawn from public knowledge have **expected edge ≈ zero or negative** once you account for crowding, survivorship bias, and regime change. AI pattern-matches surface plausibility, not deep validity; it has no skin in the game; its corpus is survivorship-biased; it conflates correlation with causation; it misses the meta-game; it remixes rather than invents. The dominant failure mode is **plausible nonsense** — strategies that sound technical but violate basic economic logic.

This is arrestable through prompt discipline at the input layer, but it means AI is **not** an autonomous idea generator.

### 3. Reframing AI's role

AI **is** a rapid prototyping accelerator, a systematic prober of known anomaly types, an idea adapter/mutator, and — the dominant productive mode — a **cross-market translator**.

### 4. Mechanism-first & hypothesis-first framing

**Every proposed strategy must articulate WHO is moving money, WHY, and WHEN.** No naked indicator conditions.

| Bad (empty) | Good (testable) |
|---|---|
| "Buy when RSI < 30" | "Buy when RSI < 30 because forced unwinding by stop-clustered late longs creates predictable mean reversion over 2–4 bars" |

**Structured hypothesis format:**

```
Hypothesis:  What is true about the market
Mechanism:   Why it is true (whose behavior, what structure)
Observable:  How you measure the hypothesis condition
Expected:    Direction, magnitude range, time horizon
             PLUS expected_net_edge:
               - source:       literature | OOS window | theoretical/Fermi | held-out chunk
               - value:        bp/trade or $/yr on a $1K reference
               - independence: confirmed NOT overlapping the intended backtest window
             OR an explicit "research-grade only" caveat with reason.
Falsifier:   What would convince you the hypothesis is wrong
             (must include a net-of-friction P&L floor from independent data)
```

`expected_net_edge` is required for the same reason a falsifier is: a hypothesis without it is incomplete. Computing it from the same data the backtest will use is overfit by construction — acceptable sources are prior literature, a different time window, a Fermi estimate, or a strictly held-out chunk. Strategies follow from hypotheses, not the reverse.

### 5. Cross-market transfer — the dominant idea-generation mode

Instead of asking AI to invent from nothing (its weakest mode), feed it mature-market strategies from FX, commodities, equities, and hedge-fund literature and ask it to translate or mutate for crypto. Translation is what LLMs do well; the search space is finite and enumerable; faithful translation requires identifying *why* a strategy works, which surfaces "this doesn't transfer" early.

**Honest caveat:** the famous transfers are gone (edge ≈ 0). The opportunity is in less-obvious mid-tier mechanisms, crypto-native mutations (funding, on-chain, liquidations, basis), and revival of strategies arbitraged away in equities but still live in low-cap crypto.

### 6. The categorization scheme (3 axes)

Every seed-library entry is tagged on three axes:

- **Axis 1 — Economic mechanism**: carry/yield · momentum/trend · mean-reversion/pairs · volatility risk premium · liquidity provision · behavioral exploitation · microstructure/order-flow · manipulation-aware · macro/regime · event-driven · calendar/cycle.
- **Axis 2 — Behavioral/structural driver**: specific cognitive bias · forced flow (margin calls, redemptions, dealer hedging, rebalancing) · capacity constraint · information asymmetry · coordination problem · regulatory friction.
- **Axis 3 — Crypto applicability**: direct analog already running (low priority) · plausible but lightly documented (verify saturation) · mutation needed (primary focus) · no analog (skip).

### 7. Mutation operators (evolving ideas from results)

When a backtest fails or partially succeeds: parameter sweep · timeframe shift · entry refinement · exit modification · asset rotation · regime filter · inverse test · **direction restriction** (drop the failing side, keep the asymmetric edge — distinct from a wholesale inverse test; the highest-leverage operator when a symmetric grid shows asymmetric long vs short P&L). Failed ideas often contain partial truths; mutation surfaces them. Direction restriction is regime-conditional, not mechanism-permanent — respect *observed* asymmetry, and reverse it when the regime that produced it shifts.

### 8. Known limitations (track these)

Single-symbol backtests miss multi-instrument archetypes (pairs, basis, BTC-conditional alt). Walk-forward/OOS is the discipline, but crypto regimes are non-stationary — prefer paired controls and regime-conditioned tests on the deploy window over naive multi-year walk-forward. "Already running in crypto?" claims are unreliable and need human verification. AI-generated ideas need extra scrutiny.

---

## Part II — Page conventions

### Frontmatter (required)

```yaml
---
title: "Page Title"
status: seedling | growing | evergreen
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [mechanism, momentum, crypto]
---
```

🌱 `seedling` — early notes · 🌿 `growing` — solid foundation, still developing · 🌳 `evergreen` — comprehensive, actively maintained.

### Page structure

A short blockquote **TL;DR**, H2 content sections, a **Key Takeaways** list, a **Related** list of `[[wikilinks]]`, and a **Sources** list.

### Linking

Internal pages: `[[page-name]]` or `[[folder/page-name]]` (resolved by basename, Obsidian-style). Glossary terms: `[[glossary/term]]`. External sources: `[text](url)`. Link liberally and bidirectionally — a new page should link to related pages, and they should link back.

---

## Part III — Workflows

- **INGEST** — read a source fully → extract mechanisms → tag on 3 axes → update/create pages (a single source may touch 5–15 pages) → add to `sources/` → update `index.md` → log it.
- **TRADE-IDEA** — start from a seed mechanism → analyse the crypto transfer → state the structured hypothesis → if it survives review, backtest → record results; if it dies, write a `rejected/` post-mortem.
- **INVEST-THESIS** — watchlist → deep dive → structured thesis (thesis / catalyst / risk / position / invalidation) → track; dead theses move to `invalidated/` with a post-mortem.
- **QUERY** — find relevant pages, synthesize with `[[citations]]`, and offer to save valuable answers as new pages. Good answers compound — don't let them disappear into chat.
- **LINT** — periodically check for contradictions (resolve and document), staleness (flag with date), orphans, gaps, and rejected ideas worth revisiting after a regime shift.
- **GROW** — every session should leave the wiki with 3–5 open questions, a list of next sources to ingest, and clear next steps.

---

## Part IV — Quality standards

- **Claims need mechanisms** — who moves money, why, when. No naked indicator conditions.
- **Falsifiers required** — an unfalsifiable hypothesis is worthless.
- **Track the graveyard** — rejected ideas get full post-mortems; the graveyard prevents re-testing failures.
- **Honest about AI limitations** — flag AI-generated ideas and apply extra scrutiny.
- **No overfit worship** — good in-sample metrics mean nothing without out-of-sample validation; flag all results as in-sample until proven otherwise.
- **Knowledge evolves** — when new information contradicts a page, flag the contradiction on both pages, investigate (regime change? new data? original error?), resolve with evidence, and document the resolution. Markets evolve; the wiki must evolve with them.

---

## Part V — The agent that uses this wiki

The wiki is the *knowledge and memory* layer; it is consumed by an AI research agent (the *strategist*) that turns knowledge into decisions. The agent's design — the three-layer architecture, the coordinator-over-arms pattern, the research loop, and the human-in-the-loop conclusion — is documented as knowledge in [[foundation/methodology/research-agent-design|The Strategist — Design of an AI Strategy-Research Agent]], and a reusable, domain-neutral version of its operating doc is published as [[templates/research-agent-template|the research-agent template]]. Those describe the *design*; the agent's live operational state (active tests, queues, results) is private and deliberately outside this knowledge base.

---

## Credits

This wiki is a direct implementation of the **LLM-Wiki** pattern described by Andrej Karpathy ([gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)): a persistent, LLM-maintained markdown knowledge base that *compiles* sources into a compounding artifact, instead of re-deriving them on every query as classic RAG does. The three-layer structure (immutable raw sources → LLM-generated wiki → schema), the ingest / query / lint operations, the `index.md` + `log.md` bookkeeping files, and the human-curates / LLM-maintains division all come from that note — itself in the lineage of Vannevar Bush's 1945 **Memex**. This repository is the trading/investing instance of that idea, plus the [[foundation/methodology/research-agent-design|research-agent]] layer that sits on top of it.

---

> **Remember:** a wiki that isn't changing is a wiki that's dying. And an idea without a mechanism is just noise.
