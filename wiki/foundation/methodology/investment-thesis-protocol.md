---
title: "Investment Thesis Protocol"
status: growing
created: 2026-05-04
updated: 2026-06-11
tags: [methodology, investment, thesis, protocol, exit-mechanism, frame-coherence, falsifier-exit]
---

# Investment Thesis Protocol

> **TL;DR**: 9-section flow for transforming an investment hunch into a validated thesis. Sequential by output structure; arms run distributed where dependencies allow. Each section carries inline gap flags — the artifact self-audits rather than papering over weak evidence. Vertical-agnostic; what differs across crypto / real estate / commodities / tech-AI / etc. is the data sources [[../sources/vertical-data-sources|money-flow-audit cites]] and the historical-analog choice in §4.

## The problem this page addresses

The wiki's the wiki architecture (ARCHITECTURE.md) INVEST-THESIS Expanded workflow names the canonical 5-field block (Thesis / Catalyst / Risk / Position / Invalidation) and the watchlist → thesis → invalidated lifecycle. That's the **shape** of the artifact. This page is the **deep-dive protocol** — the structured rigor that fills step 2 (*"Deep dive — market, team, tokenomics, competition, narrative timing"*) with arm reports + section-level gap flags.

The protocol is the investment-thesis research flow used alongside this wiki.

Sister methodology page on the trading side: [[../idea-sourcing-methodology]] (where directives come from). That page handles Problem #1 from the team's 4-problem framing; this page handles the long-horizon equivalent.

## When to invoke

- Long-horizon directive arrives that doesn't fit day/swing trading
- Cross-pollination from trading research surfaces a worth-tracking project
- A non-crypto vertical (defense / food / tech-AI / healthcare / real estate) requires conviction-building outside trading's mechanism-family logic

Not for trading directives — those route to the mechanism-translation arm + DSL via [[../idea-sourcing-methodology]].

## The 9-section flow

```
1. Raw hunch                     verbatim, preserved
2. Macro decomposition           actual shift / why-now / forces / counter-forces / macro falsifier
3. Value chain mapping           layers / where created / where captured / bottlenecks / margin migration
4. Frameworks / literature       frameworks / domain literature / historical analog / consensus vs variant
5. Money flow validation         gov / institutional / corporate / smart money / flow trajectory / contrarian signal
6. Target identification         pure-plays / indirect / ETFs / picks-and-shovels / private / filter pass
7. Stress test                   strongest counter / falsification triggers / tail risks / crowding / time decay / path dependency
8. Entry / sizing / exit         conditions, sizing logic, add/trim, exit triggers, review cadence
9. Verdict                       conviction 1-5 + asymmetry + action recommendation
```

Each section's report includes **gap flags** — what evidence was thin, what was inferred, what couldn't be verified. Inline, not buried in an end-of-doc summary. This is what makes the artifact self-auditing.

## Arm dispatch

| Section | Arm |
|---|---|
| Pre-flow | wiki-sweep — direct match / cluster / partial-truth |
| §2 macro | macro-decomposition |
| §3 value chain | value-chain-mapping |
| §4 frameworks/lit | wiki-sweep + mechanism-translation |
| §5 money flow | money-flow-audit |
| §6 targets | target-identification |
| §7 stress test | adversarial-audit + falsifier-audit |
| §8 entry/exit | (synthesized from §6 + §9) |
| §9 verdict | confidence-calibration |
| Post-flow | capability-gap + wiki-refinement |

Arms run in parallel where dependencies allow. Macro + wiki-sweep can run before value chain. Money-flow + framework-lit can run after macro. Targets after value chain + money flow.

## Operating principles

1. **Decomposition before validation.** Break the macro idea into value-chain layers before asking *"is this true?"*
2. **Money flow > narrative.** A compelling story without capital behind it is noise. Money-flow-audit is the operational form of research process worldview prior #2 (*"AI edge baseline ≈ 0"*) for investment-thesis output — it gates the thesis the way falsifier-audit gates a strategy package.
3. **Surface uncertainty, don't hide it.** Gap flags inline make weakness visible.
4. **Same logic across verticals.** Protocol is vertical-agnostic; what differs is the data sources [[../sources/vertical-data-sources|money-flow-audit cites]].
5. **Distinguish beneficiary from value-capturer.** A trend benefits many; only some monetize it. The load-bearing distinction in §3.

## Vertical scope

- **Money-flow-audit cites vertical-specific sources** — see [[../sources/vertical-data-sources]]
- **Worldview prior #1** (crypto ≈ pre-1980s US equities) only fires when vertical = crypto. For other verticals, historical analog is vertical-appropriate. Never fabricated (per worldview prior #7).
- **Frameworks invoked in §4** are pulled from training or wiki anchors (Porter's Five Forces, Wright's Law, Christensen's disruption, regulatory capture, network effects, two-sided markets, commodity supercycles, technology adoption curves). Per worldview prior #7, real episodes only — no fabricated historical analogs.

## Watchlist vs thesis

- **Watchlist** — observation noted, brief context. Lighter arm reports (typically §1-2 + §5 flow-trajectory glance + §9 watchlist trigger). Filed in `wiki/investment/watchlist/`.
- **Thesis** — promoted from watchlist after deep-dive. Full schema filled. Filed in `wiki/investment/theses/`.
- **Invalidated** — moved to `wiki/investment/invalidated/` with post-mortem when invalidation criteria fire (per wiki MAINTAIN protocol).

Templates: [[../../templates/investment-thesis]], [[../../templates/investment-watchlist]].

## Exit-mechanism frame coherence — price-stop (trading) vs falsifier-exit (investing) {#exit-mechanism-frame-coherence}

**Added 2026-06-11. N=1 evidence — held loosely**; promote to its own methodology page if the pattern recurs.

A position's exit mechanism must come from the **same frame as its entry thesis**:

- **Trading frame** (day/swing, ≤1wk): the thesis is a *price path*; the coherent exit is a **price-stop** (SL/TP) — and the stop geometry must clear horizon noise per the [[../../trading/mechanisms/universal-stop-target-prior#multi-horizon-coherence|multi-horizon coherence check]].
- **Investing/HODL frame** (multi-year): the thesis is *structural*; the coherent exit is a **falsifier-exit** — a named, measurable thesis-breaker (e.g. the L1-HODL sleeve's 90-day-ETF-net-outflow falsifier). A price-stop is *incoherent* in this frame: it converts drawdown (recoverable, expected, often the accumulation signal itself) into realized loss on the very volatility the frame was chosen to ride through. This is the §"Invalidation" field of the 5-field thesis block doing the work a stop does in trading.

**Mixing frames produces recognizable incoherent hybrids**: HODL-framed buys with 1% stops (noise-stopped accumulation), or swing trades held through thesis-dead drawdowns "because long-term."

**Venue corollary**: the same logic gates instrument choice. Spot drawdown is recoverable, so a falsifier-exit is workable; leveraged-futures drawdown is liquidation-terminal, so a price-exit is **structurally mandatory regardless of frame** — the frame question is moot once liquidation risk exists.

**Worked example (2026-06-11, live)**: a spot BTC accumulation under the HODL frame was deliberately entered **without** SL (falsifier-exit named instead), while the same session's futures position carried a mandatory SL. The no-SL reasoning held up under the position evaluator's counter-trend flag — the first machine-flag override on record, and it was frame-correct.

**Why this lives on this page**: the cleanest single-position test of "which mode is this position in" (value-investing vs trading, per the research process's mode-separation discipline) is *"what kind of exit does it have?"* — it operationalizes the mode boundary at the level of one order. Companion forecast-level discipline: [[forecast-vs-tradeable-gap]].

## Key Takeaways

- 9-section flow; each section carries inline gap flags
- Money-flow-audit gates the thesis the way falsifier-audit gates a strategy package
- Beneficiary ≠ value-capturer is the load-bearing distinction
- Vertical-agnostic; only money-flow source list and historical-analog choice vary by vertical
- Watchlist is lighter; thesis is full
- Exit mechanism must match the position's frame: price-stop for trading theses, falsifier-exit for structural/HODL theses; leveraged futures mandate a price-exit regardless of frame (N=1, held loosely)

## Related

- [[../idea-sourcing-methodology]] — sister methodology page for trading-side idea sourcing
- [[../sources/vertical-data-sources]] — empirical anchors per vertical (the §5 substrate)
- [[../../investment/allocation/conviction-tiered-sizing]] — Druckenmiller sizing logic for §8
- [[../../investment/allocation/equity-management-rules]] — risk management for §8

## Sources

- Research process: the research process shape #5
- Research process arms: the research process arms 9-12 (macro-decomposition, value-chain-mapping, money-flow-audit, target-identification)
- Wiki workflow: the wiki architecture (ARCHITECTURE.md) Part II INVEST-THESIS Expanded
