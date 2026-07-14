# fin-wiki

A public, living **knowledge base for trading and investing research** — covering both the day/swing horizon (mechanisms, backtests, what failed and why) and the long-term horizon (conviction theses, allocation, watchlists).

It is a **knowledge graph and memory**: a learning accumulator that compounds across sessions, an idea-generation engine grounded in mechanism-first discipline, and an honest record of what carried signal and what died.

## What's inside

```
wiki/
├── ARCHITECTURE.md   ← start here: schema, conventions, domain principles
├── index.md          ← master catalog (navigation)
├── foundation/       ← shared: market wisdom, structure, macro, methodology, sources
├── trading/          ← day/swing: mechanisms, backtest results, rejected ideas
├── investment/       ← long-horizon: theses, allocation, watchlist
├── questions/        ← open research questions (priorities surface)
└── glossary/         ← concept definitions
```

## Start here

- **[wiki/ARCHITECTURE.md](wiki/ARCHITECTURE.md)** — how the wiki is built: the two-branch structure, the mechanism-first domain principles, page conventions, and the ingest/research workflows. Read this first.
- **[wiki/index.md](wiki/index.md)** — the master catalog; browse every page by section.

## Guiding principles (the short version)

- **Mechanism-first.** Every claim names *who* moves money, *why*, and *when*. No naked indicator conditions.
- **Falsifiers required.** An idea you can't disprove is worthless.
- **Honest about AI limits.** AI-generated ideas have expected edge ≈ 0 without domain validation; they're flagged and scrutinized.
- **Track the graveyard.** Failed ideas get full post-mortems — they're as valuable as the wins.
- **No overfit worship.** In-sample metrics mean nothing without out-of-sample validation.

See [ARCHITECTURE.md](wiki/ARCHITECTURE.md) for the full design.

## Scope & disclaimer

This is a **research knowledge base** — notes, hypotheses, mechanism studies, and methodology. It is **not financial advice**, not a signal service, and not a record of anyone's positions or returns. Backtest figures are research artifacts (typically normalized to a $1K reference) and are in-sample unless explicitly stated otherwise. Do your own research.

## Contributing

The wiki is meant to grow. New mechanisms, source ingests, backtest write-ups, and thesis pages are all welcome — follow the page conventions and quality standards in [ARCHITECTURE.md](wiki/ARCHITECTURE.md). A good contribution links bidirectionally to related pages and, for any strategy claim, states a mechanism and a falsifier.

## Credits

Built on the **LLM-Wiki** pattern by Andrej Karpathy ([gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)) — a persistent, LLM-maintained knowledge base that compiles sources into a compounding artifact instead of re-deriving them per query (in the lineage of Vannevar Bush's 1945 *Memex*). On top of the wiki sits an AI research agent; its design is documented in [research-agent-design](wiki/foundation/methodology/research-agent-design.md) and published as a reusable [research-agent template](wiki/templates/research-agent-template.md). See [ARCHITECTURE.md §Credits](wiki/ARCHITECTURE.md).
