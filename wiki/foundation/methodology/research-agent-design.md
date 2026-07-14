---
title: "The Strategist — Design of an AI Strategy-Research Agent"
status: growing
created: 2026-06-26
updated: 2026-06-26
tags: [methodology, agent-design, research-process, human-in-the-loop, architecture]
---

# The Strategist — Design of an AI Strategy-Research Agent

> **TL;DR**: This wiki is consumed by an AI research agent — *the strategist* — whose job is to turn knowledge into decisions: discover candidate strategies, subject them to validation discipline, and surface what is worth a human's attention. This page documents the agent's **design** as a reusable pattern. It deliberately describes the *architecture and disciplines*, not any live results, positions, or private operational state — those are not part of the knowledge base.

## The three-layer architecture

The agent sits in a three-layer system, with a strict separation of concerns:

1. **Knowledge base (this wiki)** — persistent, systematized knowledge: mechanisms, market structure, validated and rejected ideas, methodology. It accumulates and compounds; it does not make decisions.
2. **The strategist (research agent)** — consumes the knowledge base plus a set of capabilities, and produces decisions and research outputs. It is the part that *thinks*; the wiki is the part that *remembers*.
3. **State** — the current operational picture (what is being worked on, what is pending). This is private and outside the knowledge base.

The load-bearing idea: **knowledge and cognition are separated.** A knowledge base that also tried to track live state would couple two things that change at very different rates and for different reasons.

## Read/write separation

The agent **reads** the wiki but does not **write** to it during research. Wiki updates happen in dedicated knowledge-curation sessions, where new findings are encoded in the wiki's voice and conventions. This keeps the knowledge base a deliberate, curated artifact rather than a transcript of every research session — and it forces the question "is this finding durable enough to systematize?" before anything lands.

The symmetric rule also holds: knowledge-curation sessions don't reach into the agent's private operational state. Each layer owns its own writes.

## Coordinator over specialized arms

The strategist is designed as a **coordinator**, not a monolith. Rather than answering from its own training knowledge, it dispatches specialized research *arms* and synthesizes their outputs:

- a **mechanism-translation arm** (take a mature-market strategy, analyse whether the economic force transfers to crypto — see [[../idea-sourcing-methodology|idea sourcing]]);
- a **research / verification arm** (gather and check external evidence);
- a **capability-gap arm** (name what tooling or data the research is blocked on).

The value is in the *coordination* — wiki sweep + independent research + capability assessment upstream of synthesis. Without that discipline, an agent collapses into "a generic language model with some context," which has near-zero edge (see [[../../ARCHITECTURE|ARCHITECTURE]] §Part I.2 on AI's structural limits).

A related discipline: **capability-blocked ≠ research-blocked.** A missing data feed blocks *backtesting*, not *thinking*. Research can and should run ahead of tooling — the research is what sharpens the decision of whether the tooling is worth building.

## Two disjoint operating modes

The agent runs in two distinct sub-modes that do not cross over:

| Mode | Horizon | Lineage | Question |
|---|---|---|---|
| **Value investing** | multi-year | Buffett / Lynch / Graham | What will be worth more in 2–5 years, and at what price is it a fair entry? |
| **Day/swing trading** | ≤ ~1 week | Wyckoff / funding-cycle / microstructure | Where is the market now, and what mechanism is exploitable this week? |

Their arms, disciplines, and outputs are kept separate — a momentum-microstructure mindset and a circle-of-competence mindset answer different questions and shouldn't be blended.

## The research loop

In trading mode the agent runs a closed loop:

```
idea  →  structured hypothesis  →  DSL spec  →  backtest  →  encode result in wiki
  ↑                                                              │
  └──────────────── mutation operators on failure ──────────────┘
```

Every step is gated by discipline drawn from the methodology layer:

- **Mechanism-first + falsifier + expected-net-edge** stated *before* the backtest, from independent data ([[../methodology/deployment-edge-threshold|deployment-edge threshold]], [[../../ARCHITECTURE|ARCHITECTURE]] §Part I.4).
- A multi-gate **deployment-decision stack**: statistical significance → translation-loss → economic significance → path-quality ([[path-quality-diagnostics|path-quality diagnostics]], [[forecast-vs-tradeable-gap|forecast-vs-tradeable gap]]).
- On failure, **mutation operators** generate the next idea — failed ideas often carry partial truths.

Implementation of the backtest step lives in a separate, private testing environment; only *validated, durable findings* are promoted into this knowledge base.

## The core design conclusion: decision-support over full automation

The most important finding the agent produced about *itself* is a limit. Across many independent detector designs, **prospective market direction proved unpredictable** — a directional classifier could not be made to generalize out of sample. What *does* work is causal **state detection** (describing where the market is — trend, volatility state, structural breaks) and **risk gating** (sizing, de-risk discipline) — not predicting *which way* it goes next.

The design consequence is decisive: **the deliverable is decision-support, not full automation.** The machine describes the objective state (its proven strength); a human makes the directional / de-risk judgment (the part that resists automation, because it needs context a model can't have) and issues the command. This is the **human-in-the-loop** pattern — the human is the external risk gate the model can't be.

Stated as a prior the agent now carries: **gate state and risk, not side.**

## Honest limitations

- **The agent has no skin in the game.** Its disciplines (falsifiers, net-edge floors, the graveyard) exist precisely to compensate for that — see [[../../ARCHITECTURE|ARCHITECTURE]] §Part I.2.
- **Coordination is only as good as the arms.** A research arm that fabricates or over-claims poisons synthesis; verification is not optional.
- **Mode discipline is fragile.** The temptation to blend the value and trading frames is constant and has to be actively resisted.
- **The conclusion above is horizon-specific.** "Direction unpredictable" is a finding about short-horizon prediction under the tested designs, not a universal claim.

## Related

- [[../../templates/research-agent-template|Research-Agent Operating Doc — reusable template]] — the fill-in-the-blanks version of this design, ready to drop into an agent's context file
- [[../../ARCHITECTURE|Wiki Architecture]] — the schema + domain principles the agent operates under (and §Credits — the LLM-Wiki lineage)
- [[../idea-sourcing-methodology|Idea Sourcing Methodology]] — where good directives come from (the front of the research loop)
- [[deployment-edge-threshold|Deployment-Edge Threshold]] / [[path-quality-diagnostics|Path-Quality Diagnostics]] / [[forecast-vs-tradeable-gap|Forecast-vs-Tradeable Gap]] — the deployment-decision gate stack
- [[strategy-validation-discipline|Strategy-Validation Discipline]] — statistical-significance gate
- [[online-regime-detection|Online Regime Detection]] — the causal state-detection the human-in-the-loop design relies on

## Sources

- Synthesized from the project's own research-process design (2026); generalized to remove live operational specifics.
