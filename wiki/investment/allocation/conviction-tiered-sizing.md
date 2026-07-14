---
title: "Conviction-Tiered Sizing — Druckenmiller's Top-Decile-Only Discipline"
status: seedling
created: 2026-05-04
updated: 2026-05-04
tags: [allocation, schwager, druckenmiller, position-sizing, meta-mechanism, conviction, day-swing-applicable]
---

# Conviction-Tiered Sizing — Druckenmiller's Top-Decile-Only Discipline

> **TL;DR**: Position size scales with **conviction percentile**, with most capital concentrated on the top 1-3 setups per period; below-threshold conviction levels = no trade. Most published position-sizing rules (e.g., [[investment/allocation/equity-management-rules|Martinez 3-5% per trade]]) treat sizing as constant-per-trade. Druckenmiller's framework treats sizing as **conviction-weighted**: 1× / 3× / 6× normal, with named entry criteria for each tier. The asymmetry creates outsized P&L from the rare triple-aligned trades. **This is a meta-mechanism — it's not about how to trade individual positions, but about which trades to size up on.** From Stanley Druckenmiller (Soros Fund Management, Duquesne Capital), interviewed in [[foundation/sources/schwager-market-wizards|*The New Market Wizards* 1992]].

## Why This Page Exists

Most wiki content on sizing is **per-trade** ([[investment/allocation/equity-management-rules|equity-management-rules]], [[foundation/market-wisdom/sit-tight-discipline|pyramid system]]). Per-trade sizing answers "how much do I risk on this specific entry?" — typically 0.25%-2% of capital.

Druckenmiller's framework is **across-trades**: among all the trades you could take this week, which deserve outsized size, which deserve normal size, and which deserve no size at all? This is meta-mechanism territory — it sits *upstream* of per-trade rules. Per-trade rules apply *after* you decide a trade is worth taking; conviction-tiered sizing decides *whether and how aggressively* to take a trade in the first place.

Without explicit conviction tiering, the default is constant-size trading: every trade gets the same risk budget. This *underweights* the rare high-conviction setups (where edge is concentrated) and *overweights* the marginal setups (where edge is thin). The trader's day/swing horizon benefits significantly from this discipline because most days don't have high-conviction setups; constant-size trading dilutes returns by deploying capital on noise.

## The Druckenmiller Framework

### Schwager's quotes from the NMW 1992 interview

> "George [Soros] and I always had this great rule: if you're convinced you're right, you put on a really big position." — Druckenmiller (paraphrased from secondary sources; source: [[foundation/sources/schwager-market-wizards|Schwager NMW]])

> "The way to build long-term returns is through preservation of capital and home runs… When you have tremendous conviction on a trade, you have to go for the jugular. It takes courage to be a pig." — Druckenmiller

> "Many managers will book their profits when they're up 30 or 40 percent, and then they spend the rest of the year trying not to lose what they've made… My philosophy is that the only way to deliver a long-term, above-average track record is to swing for the fences when you have a great trade."

### The framework

| Tier | Size multiplier | Frequency | Required conviction signals |
|------|----------------|-----------|----------------------------|
| **Tier 0 (no trade)** | 0× | Most of the time | Below-threshold conviction; no edge identified; pre-setup waiting |
| **Tier 1 (normal)** | 1× | Several per week | Single-mechanism setup; standard risk-budget per [[investment/allocation/equity-management-rules]] |
| **Tier 2 (elevated)** | 3× | Few per month | Two confirming mechanisms; macro-tailwind aligned; setup at structural level |
| **Tier 3 (high-conviction / "jugular")** | 6×+ | Few per year | Triple-confirmation (Marcus rule); macro-tailwind + setup + variant-perception; rare alignment of structural + flow + sentiment |

### Why this isn't just "more leverage on confident trades"

The discipline isn't aggressive sizing — it's **selective aggressive sizing**. Three traders run different rules:
- **Reckless aggressive**: 6× sizing on every conviction trade. Gets ruined when the conviction is wrong.
- **Constant size**: 1× on every trade. Misses the home runs that generate excess returns.
- **Druckenmiller**: 6× only when the setup is *systematically rare*. Most trades = 1×; rare trades = 6×.

The asymmetry is in *which trades are sized up*, not in *how* sizing-up works. Tier-3 trades have explicit named entry criteria — they aren't "trades I feel good about." A trade that doesn't meet the named Tier-3 criteria gets Tier-1 size *even if I feel highly confident* — because confidence without named criteria is the failure mode this rule prevents.

## How To Operationalize It

### Step 1 — Define the tier criteria

For each tier, write down explicit named criteria. The criteria should be:
- **Pre-committed**: written down before the trade, not retrofit
- **Observable**: verifiable from data, not subjective ("I feel good")
- **Falsifiable**: a trade that meets the criteria but performs worse than tier-1 baseline indicates the criteria are mis-specified

Example tier criteria for crypto day/swing:

| Tier | Required signals |
|------|-----------------|
| **Tier 1 (1×)** | Setup matches one mechanism in Apr 28 mechanism families (e.g., pivotal-point breakout, group-leader divergence, accumulation-distribution spring) with at least 3 confluence factors |
| **Tier 2 (3×)** | Tier-1 criteria + macro-regime aligned ([[foundation/market-wisdom/bubble-cycle-stages\|bubble-cycle stage]] favorable for the trade direction) + setup at structural level (cycle-level prior high/low, halving-cycle-anchored level) |
| **Tier 3 (6×)** | Tier-2 criteria + variant-perception articulated (the trade is against [[foundation/idea-sourcing-methodology\|consensus]] in a specific way) + multi-channel confluence (price + volume + on-chain flow + funding/OI + cross-asset confirmation) |

The criteria should be *independent* across tiers — a Tier-3 setup is not just "stronger Tier-2"; it requires *different* qualifying factors.

### Step 2 — Size with explicit multipliers

If your normal per-trade risk is 1% of capital:
- Tier 1 = 1% risk
- Tier 2 = 3% risk
- Tier 3 = 6% risk (subject to portfolio-level concentration limit)

Note: Tier-3 risk of 6% per trade implies a single Tier-3 trade can hit -6% drawdown if stopped. This is *significant* — that's why the discipline is reserved for systematically rare setups. A trader who takes 5 Tier-3 trades per month has mis-calibrated the criteria.

### Step 3 — Track frequency

Log every trade by tier. After 6 months, audit:
- **Tier 3 frequency**: should be a few per year, not a few per month
- **Tier 1 / Tier 2 / Tier 3 win rates**: Tier 3 should have markedly higher win rate AND higher average return than Tier 1
- **If Tier 3 isn't outperforming markedly**: the criteria are too loose; tighten them

### Step 4 — Audit emotional override

The critical failure mode: sizing up *because of feeling*, not *because of named criteria*. Discipline requires every Tier-3 trade to pass the explicit named criteria *before* sizing up. If you're tempted to take a Tier-3 size on a trade that doesn't meet Tier-3 criteria, the answer is Tier-1 size.

This is where the [[foundation/sources/lefevre-reminiscences|Cotton King vulnerability]] applies: charismatic counter-narratives, social pressure, fear of missing out — all push toward emotional Tier-3 sizing on Tier-1 setups. The named-criteria discipline is the defense.

## Connection to Other Wiki Concepts

### vs. [[investment/allocation/equity-management-rules|Equity-management rules]]

Equity-management is *per-trade*: max risk per trade, R:R ratio, expectancy. Conviction-tiered sizing is *across-trade*: which trades get how much. They compose:
- Within Tier 1: equity-management's 1% rule applies → 1% risk per trade
- Within Tier 2: equity-management's rule applies *to the multiplied size* → 3% risk per trade
- Within Tier 3: 6%, subject to concentration limit

### vs. [[foundation/market-wisdom/sit-tight-discipline|Sit-tight discipline]]

Sit-tight is about *holding* through reactions on a confirmed position. Conviction-tiered is about *committing* size in the first place. They reinforce each other:
- Tier-3 trades require sit-tight more than Tier-1 (the 6% drawdown on a Tier-3 stop is psychologically harder; sit-tight discipline is essential)
- Sit-tight benefits from conviction-tiered: holding through a -3% reaction is easier when you've named the Tier-3 criteria and the trade is still meeting them

### vs. [[foundation/market-wisdom/universal-wizards-lessons|Universal wizards lessons]]

Lesson 7 (Patience for Setups) explicitly names this: "Most wizards' setups are rare. Most of their time is spent waiting." Druckenmiller is the most-explicit operationalizer.

### vs. [[trading/mechanisms/cyclical-timing|Cyclical timing]]

Cyclical-timing setups (Lynch's P/E inversion at cycle troughs / peaks) are Tier-3 candidates by their nature — rare, high-conviction, cycle-anchored. The two pages compose: cyclical-timing identifies *when* a Tier-3 setup is available; conviction-tiered sizing tells you *how to size it*.

## Crypto Application

The framework translates with high fidelity. Crypto's **24/7 + leverage + retail dominance** environment is uniquely hostile to *constant-size* trading because the noise rate is high. Conviction-tiered sizing is correspondingly *more* valuable in crypto than in equities.

### Crypto-specific Tier criteria

For BTC/ETH day/swing:

**Tier 1 (1×) examples:**
- Standard pivotal-point breakout in trending regime with volume confirmation
- Group-leader divergence short on confirmed pattern
- Wyckoff spring on a single asset with liquidation cluster confirmation

**Tier 2 (3×) examples:**
- Tier-1 setup + BTC dominance regime aligned with the trade direction (e.g., long BTC during BTC.D rising)
- Tier-1 setup + halving-cycle phase favorable
- Tier-1 setup + macro tailwind ([[foundation/market-wisdom/bubble-cycle-stages|Boom or early-Euphoria]] regime, or post-Panic recovery)

**Tier 3 (6×) examples:**
- Tier-2 setup + cycle-level structural level (e.g., 4-year halving anchor; cycle-level prior high/low)
- Tier-2 setup + variant-perception articulated (CT consensus says X; thesis is Y; named catalyst Z)
- Tier-2 setup + multi-channel confluence: price + volume + on-chain whale flow + funding/OI + ETH/BTC ratio + breadth

### Tier-3 frequency in 2020-2022 cycle

Per [[foundation/macro/crypto-2020-2022-cycle|the worked example]], Tier-3 setups in retrospect:
- Late-March 2020 capitulation low — bubble-cycle Stage 5 panic exhaustion + macro Fed pivot + on-chain accumulation
- Q4 2020 BTC breakout above 2017 ATH ($20K) — pivotal-point break + halving-cycle phase + institutional adoption (MSTR, Tesla)
- November 2022 panic-bottom ($15.5K) — bubble-cycle Stage 5 panic exhaustion + cascade exhaustion + funding floored + multi-channel confluence

That's ~3 Tier-3 setups in a 32-month cycle. Frequency is consistent with Druckenmiller's framework (rare, systematically named).

### What's NOT a Tier-3 setup in crypto

The discipline matters most when it prevents bad sizing:
- "BTC making new highs, feeling FOMO" — *not* Tier-3; emotional sizing
- "Influencer says alt is going to 100x" — *not* Tier-3 (per [[foundation/idea-sourcing-methodology|Pattern C: tip-as-hope-delivery]])
- "Funding rate is extreme; everyone is wrong" — *Tier-2 candidate at most*; needs additional confluence
- "Setup looks great" — *Tier-1*; no Tier-3 named criteria met

## Honest Caveats

- **Survivorship bias warning**: Druckenmiller's success is exceptional; the same framework applied with worse criteria-calibration produces worse results. The framework is *necessary* (constant-size trading underweights rare setups) but not *sufficient* (good criteria-calibration is the actual edge).
- **Tier-3 risk is significant**: 6% per trade implies that a 50% drawdown can occur with ~12 consecutive Tier-3 losses. This is high. The discipline is *only* viable if Tier-3 criteria are sufficiently rare and edge-rich that the win rate justifies the size.
- **Frequency calibration is hard**: most traders mis-classify their conviction. The audit step (after 6 months, check Tier-3 frequency vs win rate) is essential.
- **Concentration limit**: portfolio-level concentration limit (e.g., max 15% capital in any single Tier-3 position) prevents catastrophe from a single mis-classification. This is *separate* from per-trade risk.
- **Method-personality fit caveat**: per [[foundation/market-wisdom/universal-wizards-lessons|Lesson 5]], not every trader is suited to the wide sizing range. A trader who can't psychologically handle a -6% Tier-3 drawdown shouldn't size to Tier-3. Calibrate the multipliers to your own psychology — for some traders, 1× / 2× / 4× is the maximum range.
- **Backtesting Tier-3 is statistically hard**: with only ~3 Tier-3 setups per 30-month cycle, the sample size for in-sample testing is small. Out-of-sample validation is *especially* important. Cross-cycle calibration is the only way to know if Tier-3 criteria generalize.
- **Cotton King vulnerability is amplified**: charismatic counter-narratives + Tier-3 sizing = catastrophic loss potential. Rule: every Tier-3 trade must pass *named criteria*, not feeling. If you're emotionally over-confident, the answer is Tier-1 size.

## Key Takeaways

- Position size scales with conviction *percentile*, not feeling. Most trades = Tier 1; rare trades = Tier 3.
- Each tier has explicit named criteria (pre-committed, observable, falsifiable).
- Tier 3 is reserved for *systematically rare* setups (a few per year), not "trades I feel good about."
- Asymmetric P&L: rare Tier-3 trades produce most of the long-term outperformance.
- Crypto-specific Tier-3 examples: cycle-level capitulation lows, pivotal-point breaks at cycle highs, multi-channel confluence + variant perception.
- Audit frequency: Tier-3 too frequent = criteria too loose; tighten until rate is "few per year."
- Compatible with [[investment/allocation/equity-management-rules|equity-management]] (per-trade rules apply within tier) and [[foundation/market-wisdom/sit-tight-discipline|sit-tight]] (Tier-3 holds need extra discipline).
- **Cotton King risk**: emotional override of named criteria is the failure mode; named criteria are the defense.
- Method-personality fit applies: not every trader can run wide sizing range.

## Related

- [[foundation/sources/schwager-market-wizards]] — source; Druckenmiller NMW 1992
- [[foundation/market-wisdom/universal-wizards-lessons]] — Lesson 7 (Patience for Setups) names this discipline
- [[investment/allocation/equity-management-rules]] — per-trade rules that apply within each tier
- [[foundation/market-wisdom/sit-tight-discipline]] — discipline for holding Tier-3 trades through reactions
- [[foundation/market-wisdom/bubble-cycle-stages]] — regime-conditioning input for tier criteria
- [[foundation/market-wisdom/composite-operator]] — Tier-3 trades often coincide with CO phase transitions
- [[trading/mechanisms/cyclical-timing]] — cyclical setups are Tier-3 candidates by nature
- [[trading/mechanisms/accumulation-distribution]] — multi-channel confluence spring/upthrust setups are Tier-2/3 candidates
- [[foundation/idea-sourcing-methodology]] — Pattern C (tip-as-hope-delivery) guards against emotional Tier-3 sizing
- [[foundation/market-structure/crypto-carrier-features]] — multi-channel confluence carriers used in tier criteria
- [[foundation/methodology/behavioral-structure-design]] — design-time discipline; this page operates within Tier-1 default sizing and feeds the structure-design's "sizing upstream of frame" principle

## Sources

- Schwager, *The New Market Wizards* (1992), Stanley Druckenmiller interview. Source: [[foundation/sources/schwager-market-wizards]]. Note: secondary-source reconstruction; primary text not in wiki.
- Distillation notes at `raw/books/schwager-market-wizards/distillation.md` Section D.10.
- Cross-source: Druckenmiller's later Sohn Conference and *Capital Allocators* podcast appearances articulate this framework consistently.
