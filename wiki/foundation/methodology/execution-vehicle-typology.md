---
title: "Execution-Vehicle Typology — Three Roles for ETFs in a Value-Investing Book"
description: "The a/b/c role taxonomy for ETFs/wrappers in a direct-name value book: portfolio-averaging vs direct-asset-access vs concentration-overlay. The CoC-depth heuristic applies ONLY to role (a); a two-gate test (wrap quality → CoC depth) decides role (a) fit. Plus the 2026-07 sharpening: within a theme, the wrapper's index methodology IS the concentration decision — and the label is not the exposure."
status: growing
created: 2026-06-29
updated: 2026-07-17
tags: [methodology, etfs, allocation, execution, value-investing, typology]
---

# Execution-Vehicle Typology — Three Roles for ETFs in a Value-Investing Book

> **TL;DR**: An ETF is not one thing. In a book whose default is **direct names**, a wrapper
> plays one of **three distinct roles** — (a) a portfolio-averaging substitute for stock-picking
> skill, (b) an operational wrapper around a hard-to-custody asset, or (c) a tactical
> concentration overlay on top of direct positions. The "should I use an ETF or pick names?"
> heuristic (circle-of-competence depth) applies **only to role (a)**. Roles (b) and (c) are
> structural and CoC-irrelevant. Mis-classifying a wrapper's role is the common error.

This page is the durable reference the instrument analyses and living cards classify against.
It is *descriptive methodology*, not a buy list.

## The three roles

### (a) Portfolio-averaging mechanism — substitute for name-selection skill
A broad ETF used *because* the book lacks the depth to pick winners in that vertical. The ETF
buys the average so a weak stock-picker doesn't have to. **Weak circle-of-competence → higher
ETF share.** Example role: a healthcare wrapper as the pharma vehicle while CoC builds.

### (b) Direct-asset-access vehicle — operational wrapper around a hard-to-custody asset
A wrapper whose *only* job is custody/access — the asset is desirable but awkward to hold
directly. **100% structural; CoC is irrelevant.** Example role: physical-Bitcoin ETPs (BITC /
IB1T) or staked-ETH ETPs (CETH) for L1 HODL exposure; physical-gold ETCs (SGLN / SGLD) for a
hard-money leg. The choice between vehicles is custody / TER / liquidity, not stock-picking.

### (c) Sub-sector-concentration overlay — tactical tilt on top of direct positions
A thematic wrapper layered *over* a book of direct names to add a concentrated tilt the direct
positions don't cover. **≤2–3% sleeve typical.** Example role: a semiconductor UCITS that caps
the dominant name (VVSM) as a picks-and-shovels-silicon overlay, or a datacenter-REIT wrapper
over direct REIT positions.

## The two-gate test (inside role (a) only)

A candidate for role (a) must clear **two gates in order**:

1. **Wrap quality.** Does the ETF hold the *right names at sane weights*? The **"right names,
   wrong weights"** failure mode kills it: a cap-weighted wrapper that re-imports a concentration
   the book deliberately avoids fails here regardless of how good the underlying names are. (Worked
   case: every cap-weighted tech/semi ETF re-imports the top-weight chip name a name-excluding book
   is built to avoid → all fail the role-(a) core gate; only NVDA-diluting role-(c) overlays pass.)
2. **CoC depth.** Only if wrap quality passes do you ask: is the book's circle of competence in
   this vertical deep enough to pick names instead? Deep CoC → direct-first; shallow CoC → lean on
   the (passing) ETF.

Roles (b) and (c) skip both gates — (b) is pure custody, (c) is a sized-small tilt by design.

## The wrapper is the concentration decision (added 2026-07-17)

The "right names, wrong weights" failure mode above has a **general form**: within a theme, the
wrapper's *index methodology* — cap rules, exclusion screens, equal- vs cap-weighting, purity
thresholds — sets single-name concentration across a range **wide enough to dominate the outcome
of the theme call itself**. An investor who has decided "I want this theme" has made the *smaller*
decision; the wrapper then silently makes the bigger one. **Choosing the wrapper is choosing the
position.**

**The measurement**: for any thematic wrapper, ask two questions — *what is my single largest
name's weight?* and *what is the range of that weight across the available wrappers for this same
theme?* If the range is wide, the wrapper choice dominates the theme choice.

The structural sub-cases:

1. **Screens are silent position decisions.** An ESG/controversy screen that excludes a major name
   entirely means an investor who never formed a view on that name has taken a 0% position in it —
   by picking a wrapper.
2. **Cap rules cut both ways.** A 10% single-name cap protects against top-heaviness *and*
   mechanically underweights a winner. Neither is right per se; the point is that it is a real
   position decision, and it is being made either way — by the index committee if not by the investor.
3. **Domicile can change the exposure, not just the tax.** A UCITS "equivalent" of a US fund may
   track a *different index variant* (e.g. capped vs uncapped) — do not assume equivalence from the
   name; read the index methodology.

### The corollary — the label is not the exposure

Thematic indices drift away from their names, especially where the genuinely on-theme investable
universe is thin and the index has to reach. A robotics-labelled wrapper can end up holding generic
AI-infrastructure names; a legacy-automation wrapper can carry a large AI-chip weight. Either way,
buying the label **double-counts an existing AI sleeve** rather than adding the theme on the tin.
**Rule: verify holdings against the label before crediting a wrapper with a theme.** A thematic
ETF's name is marketing; its index methodology is the product.

### Evidence (N=2 sectors, 2026-07 — dated snapshot, perishable, illustration only)

Same-shelf single-name concentration ranges observed July 2026 (weights drift with price and
rebalances; the *range-width* is the durable point):

- **Semiconductors — NVDA weight across "semis ETF" wrappers**: ~7% (10%-capped UCITS variant;
  equal-weighted US fund similar) → ~8% (SOXX) → ~19% (SMH) → ~22% (cap-weighted MSCI wrapper). The
  UCITS twin of the famous US ticker tracks a *capped* index variant — materially less concentrated
  than the fund whose name it shares.
- **Defense — Rheinmetall weight across "defense ETF" wrappers**: **0%** (excluded entirely by an
  ISS controversy screen) → ~3% → ~4% → ~8% → **~11%**. A 0→11-point swing in a single name across
  one shelf — the wrapper choice *was* the single-name sizing decision, and the dispersion showed up
  directly in same-year returns when that name consolidated.

This extends the typology's original N=4 role evidence with N=2 independent sector cases of the
concentration-range effect. Layer-level context for the semis case:
[[foundation/asset-classes/semiconductor-economics-primer|the semiconductor economics primer]].

## Why role-tagging matters

The same ticker can be the right or wrong call depending on the role asked of it:
- A broad tech ETF is a **fine** role-(a) core for a generalist book, and a **wrong** role-(a)
  core for a book that excludes the top-weight name — same fund, opposite verdict, decided at the
  wrap-quality gate.
- A physical-BTC ETP is **irrelevant** to debate on stock-picking grounds — it's role (b); the
  only questions are custody, TER, and liquidity.
- A factor/thematic ETF sized at 20% is a different animal from the same ETF sized at 2% — role
  (c) is defined partly by its small size.

**Classification discipline:** every wrapper analysis tags its role (a / b / c) up front, then
applies only the test that role warrants.

## Related

- [[investment/allocation/conviction-tiered-sizing|Conviction-Tiered Sizing]] — how the direct names this typology sits alongside are sized
- [[foundation/market-wisdom/circle-of-competence|Circle of Competence]] — the depth heuristic that gates role (a) only
- The Tech Leaders + Key ETFs living card — the broad-ETF NVDA-concentration read in practice — is maintained operationally, outside this knowledge base
- The BTC State Card — the L1-HODL role-(b) wrappers in their live context — is likewise operational, outside this wiki
