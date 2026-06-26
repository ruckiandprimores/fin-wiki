---
title: "Value Investing Sourcing Funnel — How to Find Candidates"
status: growing
created: 2026-05-09
updated: 2026-05-09
tags: [foundation, methodology, value-investing, sourcing, screening, circle-of-competence, special-situations, 13f-cloning, watchlist]
---

# Value Investing Sourcing Funnel — How to Find Candidates

> **TL;DR**: The wiki has methodology for **evaluating** investment candidates ([[foundation/methodology/investment-thesis-protocol]]) but not for **finding** them. This page closes that gap. Six load-bearing sourcing methods (quantitative screens / 13F cloning / special situations / Greenwald's obscure-stocks / cycle-triggered sectors / discount-to-NAV vehicles) feed a 5-stage funnel that compresses thousands of candidates → dozens of names → 3-5 watchlist entries → 1-2 thesis-ready candidates per quarter. **CoC gates everything upstream** (Pabrai's 2-3 minute reject); price discipline gates downstream (Graham's margin of safety, target 25-40% below intrinsic). Companion methodology to investment-thesis-protocol on the long-horizon side, analogous to [[foundation/idea-sourcing-methodology|idea-sourcing-methodology]] on the trading side.

## Why This Page Exists

The 2026-05-07 research process session surfaced AI-infrastructure REITs as the **narrowest first thesis** at the intersection of two strong-CoC areas. But the next thesis (and the ones after) won't be handed over by the research process — they need to **emerge from a deliberate sourcing process**. Without an explicit funnel:

- "Looking for value-investing candidates" becomes pattern-of-the-week chasing
- High-CoC zones produce 0 candidates while low-CoC zones produce many bad ones
- Watchlist rots: entries get added but not periodically re-evaluated
- Catalyst entry is missed (good thesis at wrong time = no return)

This page is the **input layer**: how to construct a steady flow of candidates that actually reach the [[foundation/methodology/investment-thesis-protocol|9-section thesis flow]]. It is sister methodology — sourcing produces the input that thesis flow processes.

## The 5-Stage Funnel

```
Stage 1: Universe (~10,000+ public equities + REITs + BDCs + CEFs)
            ↓ CoC filter (~2-3 minute reject per Pabrai)
Stage 2: In-CoC names (~50-200 per area)
            ↓ Quantitative + qualitative screens
Stage 3: Screen survivors (~10-30 per area)
            ↓ Competitive-advantage (Greenwald) check
Stage 4: Watchlist (~3-5 active entries; capped)
            ↓ Catalyst + price (margin of safety) trigger
Stage 5: Thesis-ready (1-2 per quarter)
            ↓ [[foundation/methodology/investment-thesis-protocol|9-section thesis flow]]
        Position taken (or rejected)
```

**Funnel ratios are honest expectations, not targets.** A high-quality funnel surfaces 5-15 thesis-ready candidates per year — most never reach position size; most positions take 2-4 quarters from watchlist entry to entry trigger.

### Stage 1 → 2: CoC Filter (the structural gate)

Per [[foundation/market-wisdom/circle-of-competence|Buffett's framework]] and Pabrai's operationalization: **most businesses get rejected within 2-3 minutes** for one of two reasons — outside CoC, or quick price/market-cap glance doesn't make it interesting.

**Operationalize**:
- **Map the boundary**: list strong-CoC areas (current state per this research program: AI/software, real estate, crypto-as-trading); weak-CoC under-construction (pharma, defense); skip-list (food, commodities)
- **Use intersection bonuses**: AI/software + real estate = datacenter REITs. AI/software + power-grid economics = power-infra REITs / utilities. Compound CoCs are higher-confidence than single-CoC.
- **Honest re-evaluation cadence**: CoC is not static. Every 6-12 months re-map. Pharma upgraded from skip → weak-watchlist 2026-05-07 is the recent example.

**Quick reject criteria** (Pabrai-style 2-3 minute filter):
1. **Outside CoC area** → reject
2. **Inside CoC but business model unclear after 2 minutes of reading** → reject
3. **Market cap < threshold** (typical Greenblatt: $50-100M minimum) → reject for liquidity
4. **Ownership / governance issues** (controlling shareholder hostile to minority; complex VIE structures) → reject
5. **Accounting opacity** (financial statements unintelligible) → reject

### Stage 2 → 3: Screening Methods (six load-bearing approaches)

Six sourcing methods, each anchored in named methodology. Apply 2-3 simultaneously for breadth; rotate the dominant method as cycle/sector signals shift.

#### Method 1 — Greenblatt's Magic Formula

**The most-validated quantitative value screen.** Rank stocks by combined ranking of:
- **Earnings yield** = EBIT / Enterprise Value
- **Return on capital** = EBIT / (Net fixed assets + Net working capital)

Combined-rank top stocks; minimum market cap $50-100M; **exclude financials and utilities** (different financial structures distort the ratios).

**Backtest**: 2003-2015 US application: 11.4% annualized vs S&P 500 8.7%. Greenblatt's published track records are stronger but use earlier windows.

**When to use**: as a *broad scan* across in-CoC universe to surface names you might not have known about. The output is a list to triage, not a buy list.

**Honest caveats**: Magic Formula has had multi-year underperformance windows (2010-2014 was tough; large-cap tech dominance disadvantaged the formula). Treat as one input, not a strategy.

**Tool**: [magicformulainvesting.com](https://www.magicformulainvesting.com/) (Greenblatt's free site); supplement with GuruFocus / AAII screeners.

#### Method 2 — 13F Cloning (Pabrai's primary method)

**Mohnish Pabrai's primary source of investment ideas is 13F filings from value managers he admires.** This isn't lazy — it's high-leverage: the work of finding and analyzing has been done by someone with skin-in-the-game and longer time horizon.

**Approach**:
- Track 13F filings of: Buffett (Berkshire), Klarman (Baupost), Marks (Oaktree), Einhorn (Greenlight), Pabrai (Pabrai Funds), Gayner (Markel), Gates Foundation, Marks-style activist investors
- New positions and significant adds are the highest-signal events
- Filter against own CoC — only clone what you understand
- **13F lag is real**: filings publish ~45 days after quarter-end, so "fresh" cloning is 45-135 days old. Acceptable for long-horizon value investing; useless for trading.

**When to use**: second-pass discipline after Magic Formula. The Magic Formula tells you "what's quantitatively cheap"; 13Fs tell you "what professional value-investors have allocated to and why."

**Honest caveat**: cloning preserves the skin-in-the-game evidence but loses the *exit discipline*. You're cloning entry but not seeing the basis cost or position-management logic. Read shareholder letters / interviews to fill in the *why* before sizing.

#### Method 3 — Special Situations (Greenblatt's "Stock Market Genius" approach)

**Spinoffs, restructurings, recapitalizations, post-bankruptcy emergences — situations where structural sellers create predictable pricing inefficiencies.**

**Spinoff economics** (Greenblatt's flagship category):
- Spinoffs typically outperform the market by ~10% annually on average
- Mechanism: spinoff stock isn't sold, it's **given** to shareholders who often dump immediately without regard to fundamentals (mandate constraints, index exclusion, taxable-event aversion, "I don't want this small new company")
- **The forced-seller dynamic creates the opportunity**

**Other special-situation categories**:
- **Post-bankruptcy emergences** — equity often resets at low EV; surviving operating business may be cleanly capitalized
- **Activist campaigns** — when activists target operational improvement (not just M&A advocacy), follow-on capital appreciation can be large
- **Stub equities / orphan equities** — small residual after major M&A leaves micro-cap that's index-orphaned
- **Conglomerate breakups** — sum-of-parts often exceeds whole; activist or management-driven breakups unlock the discount

**Greenblatt's filter for spinoffs**:
- **Insider participation**: management compensation tied to spinoff performance? Are insiders *retaining or increasing* their position?
- **Why was it spun off?** (legitimate strategic separation vs dumping ground for liabilities)
- **Is the spinoff small relative to parent?** (smaller = more index-orphan dynamics, more retail dump)

**When to use**: ongoing — read [Stock Spinoff Investing](https://stockspinoffinvesting.com/) and similar; major corporate-action databases. Special-situations sourcing is **complementary to fundamental screening**, not a substitute.

#### Method 4 — Greenwald's Obscure-Stocks Search

**Bruce Greenwald's principle: concentrate the search where you're likely to be on the right side of the trade.** This means specifically looking in places professional analysts and indexers don't:

- **Small caps** with low analyst coverage
- **Spin-offs** (overlap with Method 3)
- **Boring stocks** (no narrative; no story; just durable cash flow)
- **Distressed / bankrupt** names (overlap with Method 3)
- **Recently disappointed** (post-earnings-miss; post-guidance-cut)

**Compared to "well-known with competitive advantages"**: Greenwald notes that famous wide-moat names are typically overpriced. The arbitrage is finding wide-moat names that *aren't* widely known to be wide-moat — usually because they're small, boring, or out-of-fashion.

**Greenwald's competitive-advantage typology** (used in Stage 3 → 4 below):

| Category | Sub-types |
|---|---|
| **Supply-side advantages** (cost) | Proprietary technology; cheap inputs; learning/experience; economies of scale; government licenses/tariffs/patents/subsidies |
| **Demand-side advantages** (customer captivity) | Habit; switching costs; search costs |

**The discipline**: when evaluating a Stage-3 candidate, **identify which type of moat applies and why**. If you can't articulate the moat in those categories, the moat probably isn't there — narrative is filling in for substance.

#### Method 5 — Cycle-Triggered Sector Scans

**Sector dispersion within market cycles creates opportunity windows.** When a sector has been beaten down for 2-3 years, dispersion within the sector typically widens — high-quality survivors trade at multi-decade-low valuations alongside legitimately distressed names.

**Current cycle examples (May 2026)**:
- **REITs**: Still depressed since Fed 2022 rate cycle; REIT discounts to NAV at multi-year highs in some sub-sectors (per [[foundation/methodology/reit-evaluation-methodology|REIT methodology]])
- **AI-infrastructure**: Late-Boom phase per [[foundation/macro/ai-cycle-calibration-2024-2026|AI cycle calibration]] — picks-and-shovels (REITs, power, GPU suppliers) still pricing-power-positive
- **Pharma**: 2025-2030 LOE wave creates dispersion — companies with strong pipelines trading near LOE-exposed peers per [[foundation/asset-classes/pharma-economics-primer|pharma primer]]
- **Distressed retail / office**: Secular declines, but high-quality survivors at deep discount

**When to use**: at portfolio-construction level. When concentration is too high in one sector, scan adjacent under-loved sectors for diversification candidates. **Cycle-triggered scans should be deliberate, not opportunistic** — pick the sector first, then screen within it.

#### Method 6 — Discount-to-NAV Vehicles (closed-end funds, BDCs, REITs)

**Specific structural opportunity**: closed-end funds (CEFs), business development companies (BDCs), and listed REITs can trade persistently below NAV due to retail-investor sentiment, liquidity friction, or distribution-yield perception.

**Mechanics**:
- CEFs: fixed share count + market price; price/NAV discount can swing widely with sentiment
- BDCs: similar mechanics; primarily debt-portfolio holders; need credit-quality verification
- REITs: market price vs NAV (per REIT methodology); "P/NAV ratio" tracks the discount/premium

**Approach**:
- Screen for >15% discount to NAV
- Cross-check NAV calculation reasonableness (CEFs usually mark-to-market daily; REITs require cap-rate-based bottoms-up)
- Verify the discount has *narrowed before* (stable structural discount = trap; cyclical discount = opportunity)
- Distribution policy stability — high yield with shrinking NAV is melting-ice-cube

**When to use**: complementary to other methods. NAV-discount screens surface mechanical opportunities; the other methods surface fundamental opportunities.

### Stage 3 → 4: Watchlist (the holding tank)

After screening produces survivors, the watchlist is the **discipline buffer**. Munger / Buffett wisdom: most action is patience. Watchlist size should be **capped at 5-10 active entries** — beyond that, attention dilutes and you can't track catalysts properly.

**Watchlist entry should specify**:
- **Why it's interesting** (one-paragraph thesis at watchlist-time)
- **Conservative intrinsic value estimate** (NAV / DCF / SOTP — pick one anchored in [[foundation/methodology/investment-thesis-protocol|thesis methodology]])
- **Margin-of-safety entry trigger price** (typically 25-40% below intrinsic; Buffett uses up to 50%)
- **What would change the thesis** (degrades to "delete" — not just price)
- **Catalyst window** — when do you expect resolution?

**Periodic watchlist review** (recommended quarterly):
- Has new information changed conservative intrinsic value?
- Has price moved toward entry trigger?
- Have catalysts crystallized or invalidated?
- Should this be deleted (thesis broken) or escalated (move to Stage 5)?

Per the [[templates/investment-watchlist|investment-watchlist template]] for the format.

### Stage 4 → 5: Catalyst + Price Trigger (the entry gate)

**Two conditions must be met simultaneously**:

1. **Price within margin-of-safety zone** (target intrinsic × 0.6-0.75)
2. **Catalyst path identified** (something must happen for value to be realized in 2-5 years; without catalyst path, "cheap" is a value trap)

**Catalyst types**:
- **Operational** (margin recovery, revenue acceleration, contract win)
- **Capital structure** (buyback, debt repayment, dividend initiation, recapitalization)
- **Corporate event** (M&A, spinoff, breakup, activist intervention)
- **Macro / cycle** (sector rotation, rate environment shift, regulatory change)
- **Earnings normalization** (cyclical trough → average earnings)

**No catalyst** → keep on watchlist; don't position. **Catalyst without margin of safety** → keep on watchlist; wait for price.

When both conditions trigger, candidate moves to Stage 5: full thesis flow. Per [[foundation/methodology/investment-thesis-protocol|9-section thesis protocol]], not all Stage-5 candidates become positions — the thesis flow can still reject.

## CoC Integration (the structural through-line)

Every stage filters against CoC, but the dominant filter shifts:

| Stage | Dominant filter | CoC role |
|---|---|---|
| **1 → 2** | CoC | Primary gate — most rejections happen here |
| **2 → 3** | Quantitative screens | CoC verifies survivor relevance |
| **3 → 4** | Competitive advantage | CoC enables moat-articulation |
| **4 → 5** | Catalyst + price | CoC verifies catalyst plausibility |
| **5 → position** | Thesis flow | CoC delivers conviction-tier sizing input |

**Boundary expansion**: CoC grows by deliberate substrate-building (the [[foundation/asset-classes/pharma-economics-primer|pharma primer]] is a CoC-building exercise). Don't try to invest in weak-CoC areas at conviction tier without first running CoC-build sessions to graduate them.

**Per this research program feedback memory** "Sizing tier is the honest-scope lever, not artifact rigor": same thesis flow at full rigor regardless of CoC depth, but **weak-CoC areas get Tier-2 sizing** + parallel CoC-build path. Don't downgrade artifact form for weak-CoC.

## Worked Example: AI-Infra REIT Funnel

**Stage 1 → 2 (CoC)**: AI/software (strong) ∩ real estate (strong) → datacenter REITs flagged. Map: EQIX, DLR, IRM as US-listed; private-side comparable: Brookfield, Blackstone, KKR datacenter portfolios.

**Stage 2 → 3 (screens)**:
- Magic Formula: financials/utilities excluded; REITs technically excluded (financial-structure issues with the formula). **Use REIT-specific screen instead**: AFFO yield + AFFO growth rate ranking.
- 13F: check Buffett (none; not in CoC for him traditionally), Marks/Oaktree (some real-estate exposure), specialty REIT-focused funds (Cohen & Steers, etc.)
- Special situations: any datacenter REIT spinoff or restructuring activity in last 24 months? (As of May 2026: minimal.)
- Greenwald: small/obscure → IRM is smallest; EQIX/DLR are large-cap, well-followed (less obscure-edge here)
- Cycle-triggered: AI-infrastructure picks-and-shovels per [[foundation/macro/ai-cycle-calibration-2024-2026|AI cycle calibration]] — late-Boom call supports
- Discount-to-NAV: check current P/NAV for EQIX, DLR; if both at significant discount = signal; if both at premium = late-cycle signal but not necessarily reject

**Stage 3 → 4 (Greenwald moat check)**:
- **Datacenter REIT moat type**: predominantly **supply-side** (economies of scale; power-grid permitting moat — multi-year leadtime locks out competitors; long-term hyperscaler customer contracts as customer captivity)
- Articulable in 2 sentences? Yes ("Datacenter REITs' moat is grid-permitted power capacity in major markets — a 3-7 year permitting leadtime that competitors can't bypass. Hyperscaler tenant capex is multi-year contractual; switching costs are high once a build-to-suit is signed.")
- → **Watchlist entry candidate**

**Stage 4 → 5 (catalyst + price)**:
- **Catalyst**: AI-cycle late-Boom continuation (per AI-cycle calibration page); mark-to-market lease pricing 30-50% above expiring leases compounds for 3-5 years until reset complete
- **Price discipline**: target conservative intrinsic value via [[foundation/methodology/reit-evaluation-methodology|REIT methodology]] (NAV + AFFO yield); margin of safety 25-40% below intrinsic
- **Invalidation**: hyperscaler capex revision DOWN; depreciation policy change indicating shorter useful life

**Stage 5**: full [[foundation/methodology/investment-thesis-protocol|9-section thesis flow]] — pending in research process queue.

## Pharma-Specific Funnel Adaptation

Pharma is on weak-CoC list per this research program — the funnel adapts:

- **Stage 1 → 2**: tighter CoC filter; only consider names where therapeutic-area dynamics can be reasonably understood (oncology, autoimmune, GLP-1 obesity are higher-CoC for educated layperson; rare-disease, neurological are harder)
- **Stage 2 → 3**: standard screens BUT **also explicit LOE schedule check** — any company with >25% revenue facing LOE in <5 years gets flagged for restructuring-risk
- **Stage 3 → 4**: moat check uses pharma-specific question template per [[foundation/asset-classes/pharma-economics-primer|pharma primer]]
- **Stage 4 → 5**: Tier-2 sizing applies (per CoC-aware sizing); CoC-build runs in parallel — ingest 1-2 therapeutic-area-specific resources before scaling exposure
- **Stage 5**: standard thesis flow

**Don't skip steps to compensate for weak CoC.** Per the memory a standing convention: same flow at full rigor, smaller position. The funnel is the same; the dial is sizing, not analysis depth.

## Anti-Patterns (what NOT to do)

- **Story-stock chasing**: "I love this product, the company must be a good investment" — skips Stage 2 screening. Product enthusiasm ≠ shareholder-return potential.
- **Recency-bias screening**: searching only "recently down stocks" produces value traps; legitimate distressed survivors require fundamental analysis, not just price action.
- **Unfiltered 13F cloning**: copying high-profile fund moves without CoC filter or price discipline. Cloning at high prices = chasing narrative; cloning at low prices = legitimate co-investing.
- **Watchlist bloat**: >20 active entries makes monitoring impossible; quality-check fails. Cap at 5-10 active entries.
- **Catalyst ignorance**: cheap-without-catalyst = value trap. Patience requires expected resolution path, not just price below intrinsic.
- **Weak-CoC over-sizing**: pharma at Tier-1 when CoC is weak = avoidable mistake. Per this research program: weak-CoC = Tier-2 sizing + CoC-build path.
- **Sourcing without sector context**: Magic Formula across the entire universe ignores cycle/sector dispersion. Apply screens *within* CoC areas, not globally.
- **Substituting price drops for thesis revisions**: when stock drops 20% before catalyst, that's not "double-down opportunity" automatically — verify the original thesis still holds.

## Honest Caveats / Failure Modes

- **Sourcing produces opportunity, not certainty**. The funnel surfaces candidates; they still fail the thesis flow. **Acceptance rate from watchlist → position should be ~10-30%**, not 100%.
- **Funnel ratios are imprecise**. The "10,000 → 1-2 thesis-ready/quarter" ratios are illustrative. Individual investors may surface 0 candidates in weak-CoC quarters and 5+ in strong-CoC quarters. Don't force constant flow.
- **6 sourcing methods are not exhaustive**. Other methods exist (private-market comparables, founder-letter reading, employee-review-site signals). The 6 listed are the **most-validated**; others can supplement.
- **Cycle-triggered sector scans require cycle-call discipline**. The [[foundation/macro/ai-cycle-calibration-2024-2026|AI cycle calibration]] page is the substrate for "where in cycle" — without it, sector calls are vibes.
- **13F lag**. Filings are 45-135 days old at access time. Acceptable for value investing; suboptimal for shorter-horizon work.
- **Magic Formula has multi-year underperformance windows**. Don't abandon mid-window; don't extrapolate single-year results. Treat as *one input*, not a strategy.
- **CoC is the largest single constraint**. Most of the universe is permanently out-of-CoC for any individual investor. Aiming for "I'll invest in anything" is incompatible with concentration discipline. Embrace narrowness.
- **Watchlist atrophy**. If watchlist isn't actively reviewed, entries decay. Quarterly cadence is a minimum.

## Open Questions for Future Investigation

1. **Empirical funnel-ratio audit**: track actual sourcing → thesis → position conversion rates over 12 months to calibrate funnel expectations. Currently illustrative-only.
2. **Specialty-screener coverage**: which screeners best cover REITs, BDCs, CEFs (where Magic Formula doesn't apply)? Need NAREIT-grade screening.
3. **13F filers to track**: current shortlist is generic value-investor names. Curate to filers whose CoC overlaps with user's CoC areas (AI/software + RE + crypto-trading-adjacent).
4. **Special-situations databases**: are there low-cost paid databases that surface upcoming spinoffs / restructurings systematically? (StockSpinoffInvesting Substack is one source; others?)
5. **CoC-build cadence**: pharma CoC-build is N=1 (this primer). What's the right cadence — one new CoC area per quarter? Per year?
6. **Greenblatt's 13F vs Magic Formula reconciliation**: Greenblatt himself runs both. Do they agree often? When they disagree, which wins empirically?

## Related

- [[foundation/methodology/investment-thesis-protocol]] — the downstream methodology this funnel feeds into; sister page on the long-horizon side
- [[foundation/idea-sourcing-methodology]] — analogous sourcing methodology on the trading side; this page is the long-horizon parallel
- [[foundation/market-wisdom/circle-of-competence]] — the structural through-line of the funnel
- [[foundation/market-wisdom/margin-of-safety]] — Stage 4 → 5 price-trigger anchor
- [[foundation/market-wisdom/wonderful-business-fair-price]] — Buffett's framing; quality + price discipline integration
- [[foundation/market-wisdom/lynch-six-categories]] — Lynch's stock taxonomy; orthogonal lens to Greenblatt/Greenwald methods
- [[foundation/market-wisdom/perfect-stock-attributes]] — Lynch's 13 attributes; useful as Stage-3 qualitative checklist
- [[foundation/market-wisdom/stocks-to-avoid-lynch]] — Lynch's anti-patterns; Stage 1-2 filter input
- [[foundation/sources/buffett-essays]] — capital-allocation framework throughout the funnel
- [[foundation/sources/one-up-on-wall-street]] — Lynch on amateur-edge sourcing methods
- [[foundation/macro/ai-cycle-calibration-2024-2026]] — cycle-triggered sector context for current AI-infra-REIT thesis
- [[foundation/methodology/reit-evaluation-methodology]] — Stage-3 quantitative screening for REIT candidates
- [[foundation/asset-classes/pharma-economics-primer]] — Stage-3 framework for pharma-specific funnel adaptation
- [[templates/investment-watchlist]] — Stage 4 watchlist entry template
- [[templates/investment-thesis]] — Stage 5 thesis template

## Sources

**Greenblatt's Magic Formula + Special Situations**:
- [Magic Formula Investing — Wikipedia](https://en.wikipedia.org/wiki/Magic_formula_investing)
- [Magic Formula Investing site (Greenblatt)](https://www.magicformulainvesting.com/)
- [Magic Formula Screen — AAII](https://www.aaii.com/stocks/screens/46)
- [Magic Formula Investing: Understanding & Implementing — StableBread](https://stablebread.com/magic-formula-investing/)
- [Joel Greenblatt: How Spinoffs and Special Situations Beat the Market — Acquirer's Multiple](https://acquirersmultiple.com/2025/07/joel-greenblatt-how-spinoffs-and-special-situations-beat-the-market/)
- [3 Biggest Takeaways From Greenblatt's Special Situation Class — Stock Spinoff Investing](https://stockspinoffinvesting.com/3-biggest-takeaways-from-joel-greenblatts-special-situation-class/)
- [Joel Greenblatt Class Notes On Special Situation Investing — archive.org](https://archive.org/stream/JoelGreenblattClassNotesOnSpecialSituationInvesting/Joel-Greenblatt-Class%20notes%20on%20Special%20situation%20investing_djvu.txt)
- [Joel Greenblatt — Wikipedia](https://en.wikipedia.org/wiki/Joel_Greenblatt)

**Pabrai + 13F cloning**:
- [13F Idea Generation: A Value Investor's Guide — Substack](https://superfluousvalue.substack.com/p/13f-idea-generation-a-value-investors-guide)
- [Mohnish Pabrai's Playbook: Reciprocation, Mental Models, CoC — Acquirer's Multiple](https://acquirersmultiple.com/2025/05/mohnish-pabrais-playbook-reciprocation-mental-models-and-circle-of-competence/)
- [Shameless Cloning 2.0: Pabrai's Idea, Upgraded for 2026 — Substack](https://www.compoundwithrene.com/p/shameless-cloning-20-pabrais-idea)
- [Is Cloning Top Fund Managers a Good Way to Go? — eInvesting for Beginners](https://einvestingforbeginners.com/cloning-top-fund-managers-daher/)

**Greenwald (Columbia value tradition)**:
- [Greenwald Explains Value Investing Principles — Columbia Business School](https://business.columbia.edu/insights/chazen-global-insights/greenwald-explains-value-investing-principles)
- [Understanding Competitive Advantage — Heilbrunn Center, Columbia](https://www8.gsb.columbia.edu/valuethe investing layer)
- [Bruce Greenwald on the Future of Value-Oriented Investing — MOI Global](https://moiglobal.com/bruce-greenwald-on-the-second-edition-of-value-investing/?pdf=74552)
- [Value Investing: From Graham to Buffett and Beyond Notes — Medium](https://medium.com/@peter.simon419/value-investing-from-graham-to-buffett-and-beyond-by-greenwald-notes-d2c97d014ee5)

**Margin of safety + watchlist process**:
- [Why Margin of Safety Is Misunderstood — Old School Value](https://www.oldschoolvalue.com/investing-strategy/margin-of-safety-investment-framework/)
- [Margin of Safety in Value Investing — GrahamValue](https://www.grahamvalue.com/blog/margin-safety-value-investing)
- [The Investment Checklist: Charlie Munger's Method — Picture Perfect Portfolios](https://pictureperfectportfolios.com/the-investment-checklist-charlie-mungers-method/)
- [Essential Value Investing Principles — Waterloo Capital](https://waterloocap.com/value-investing-principles-guide/)

**Discount-to-NAV vehicles**:
- [Closed-End Funds — Nareit](https://www.reit.com/the investing layer)
- [BDC Investing: A Comprehensive Guide — VanEck](https://www.vaneck.com/us/en/blogs/income-the investing layer)
- [2 Closed-End Funds Focused On Beaten Down REITs — Seeking Alpha](https://seekingalpha.com/article/4771562-2-closed-end-funds-focused-on-beaten-down-reit-investments)

---

*Methodology page — `growing` status. Will refine to `evergreen` after first complete sourcing → thesis → position cycle that exercises this funnel end-to-end (currently substrate; not yet exercised).*
