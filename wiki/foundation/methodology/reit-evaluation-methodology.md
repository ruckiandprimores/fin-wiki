---
title: "REIT Evaluation Methodology — FFO, AFFO, NAV, Cap Rates"
status: growing
created: 2026-05-08
updated: 2026-05-08
tags: [foundation, methodology, reit, real-estate, ffo, affo, nav, cap-rate, datacenter-reit, investment]
---

# REIT Evaluation Methodology — FFO, AFFO, NAV, Cap Rates

> **TL;DR**: Standard equity metrics (EPS, P/E, EBITDA) **systematically distort REITs** because of large non-cash real-estate depreciation. The four pillars of REIT analysis are **FFO** (recurring earning power), **AFFO** (sustainable distributable cash flow — most relevant for dividend coverage), **NAV** (asset-by-asset intrinsic value via cap-rate-discounted NOI), and **cap rates** (the discount rate that prices individual properties). Datacenter REITs are a special case: power is the binding constraint; mark-to-market lease pricing 30-50% above expiring leases is the live AI-cycle signature. Companion methodology to [[foundation/macro/ai-cycle-calibration-2024-2026]] for AI-infrastructure REIT thesis work.

## Why Standard Metrics Fail for REITs

GAAP requires depreciation of real estate over 27.5-39 years. The economic problem: **real estate typically appreciates over time** (or holds value with maintenance), so accounting depreciation is a **non-cash distortion**, not an economic reality.

Consequence:
- **EPS** dramatically understates a REIT's actual earning power
- **P/E** is meaningless as a valuation multiple — REIT P/Es look optically cheap or absurdly expensive depending on depreciation accounting choices
- **EBITDA** is somewhat better (adds back depreciation) but still includes one-time gains/losses on property sales, distorting recurring profitability

REIT-specific metrics emerged (NAREIT codified FFO in 1991) to fix the depreciation distortion and surface true recurring cash flow.

## The Four Pillars

### 1. FFO (Funds From Operations)

**NAREIT standard formula**:
```
FFO = Net Income
    + Real estate depreciation & amortization
    - Gains on sales of property
    + Losses on sales of property
    + (other adjustments per NAREIT definition)
```

**What it captures**: recurring earning power from real estate operations, stripped of non-cash depreciation distortion and one-time property-sale gains.

**Use cases**:
- **P/FFO multiple** — REIT analog of P/E; primary relative-valuation tool
- **FFO growth rate** — measures organic earning-power growth
- **FFO per share trajectory** — tracks dilution-adjusted earning power over time

**Limitation**: FFO does NOT subtract the maintenance capex required to keep properties in earning condition. A REIT with rapidly aging assets can show strong FFO while bleeding cash. AFFO fixes this.

### 2. AFFO (Adjusted Funds From Operations)

**Most relevant single metric for REITs.**

**Formula** (no NAREIT standard — analyst judgment required, but common form):
```
AFFO = FFO
     - Recurring maintenance capex (leasing commissions, tenant improvements, building maintenance)
     - Straight-line rent adjustment (smooths contractually-stepped lease income)
     + (other normalizations)
```

**What it captures**: sustainable cash flow that a REIT can distribute as dividends without depleting its asset base.

**Use cases**:
- **Dividend coverage ratio** = AFFO / Dividends paid; <100% means dividend is being paid out of property sales or capital raises (red flag)
- **AFFO per share growth** — the cleanest "sustainable per-share earning power" growth metric
- **P/AFFO** — relative-valuation multiple closest to a sustainable-earnings P/E

**Why no standard formula**: maintenance capex is judgment-laden. Aggressive REITs classify near-recurring capex as "growth" or "redevelopment" to inflate AFFO. **Always check whether AFFO maintenance-capex assumption is plausible vs trailing 5-year average.**

### 3. NAV (Net Asset Value)

**The intrinsic-value approach** — bottom-up sum of property values, minus liabilities.

**Formula**:
```
For each property: Property Value = Forward NOI / Market Cap Rate

NAV = Σ (Property Values)
    - Outstanding Debt
    - Preferred Stock
    + Cash & Equivalents
    + Value of non-real-estate assets (development pipeline, joint ventures)

NAV per share = NAV / Diluted Shares Outstanding

P/NAV ratio = Stock Price / NAV per share
```

**Use cases**:
- **Premium/discount to NAV** — REITs trading at >NAV (P/NAV >1.0) imply the market expects above-average growth or capability; <NAV implies opposite
- **Margin of safety** — buying at significant discount to NAV provides downside protection (per [[foundation/market-wisdom/margin-of-safety|Graham's central concept]])
- **Cycle indicator** — across the REIT sector, when most REITs trade >NAV the property cycle is hot; when <NAV, it's distressed

**Limitation**: NAV is sensitive to the cap rate assumption. A 25 basis-point shift in cap rate (5.0% → 5.25%) can move NAV 5-10%. Multiple-cap-rate sensitivity analysis (low / mid / high) is standard.

### 4. Cap Rate (Capitalization Rate)

**The discount rate that prices individual properties.**

**Formula**:
```
Cap Rate = Net Operating Income (NOI) / Property Value

Equivalent: Property Value = NOI / Cap Rate
```

**Cap rate intuition**: it's the unlevered yield a buyer accepts on a stabilized property. **Lower cap rate = higher property value** (paying more for the same NOI = lower yield). High-quality / low-risk properties trade at low cap rates (e.g., trophy office in NYC: 4-5%); secondary-market industrial: 6-8%; distressed retail: 8%+.

**Cap rate determinants**:
- **Risk-free rate**: cap rates broadly track 10y Treasury + risk premium
- **Property quality / location**: prime > secondary
- **Tenant credit quality**: investment-grade > non-investment-grade
- **Lease duration**: long-term + escalators > short-term
- **Sector**: trophy office < datacenter < industrial < retail < hotel (rough current ordering 2026)

**Cap rate spread to 10y Treasury** is a cycle indicator:
- **Spread ~150-250 bps**: typical late-cycle / hot-property-market
- **Spread ~250-400 bps**: typical mid-cycle
- **Spread >400 bps**: distressed / opportunity territory
- **Spread <150 bps**: bubble territory (peak property prices, low risk premium)

## P/FFO Multiple Reference Bands (May 2026)

Cross-sector relative-valuation guide:

| Sector | Typical P/FFO range (2026) | Notes |
|---|---|---|
| **Datacenter (AI-infra)** | 22-28× | Premium for AI-cycle pricing power; mark-to-market growth bridge |
| **Industrial / Logistics** | 18-22× | E-commerce structural tailwind |
| **Multifamily / Apartments** | 16-20× | Demographic + housing-shortage support |
| **Healthcare** | 14-18× | Rate-sensitive; aging demographic |
| **Net-lease (Realty Income, etc.)** | 14-17× | Bond-proxy; rate-sensitive |
| **Office** | 8-14× | Distressed; secular WFH headwind |
| **Mall / Retail** | 9-13× | Distressed; structural decline |
| **Hotel / Hospitality** | 8-12× | Cyclical; high operating leverage |

**Honest caveat**: these bands shift with rate cycles. In a low-rate environment (2020-2021), all bands shifted +30-50% higher. In a rising-rate environment (2022-2023), -20-30% lower. Use as relative ordering, not absolute.

## Operational Metrics (track for any REIT thesis)

### Occupancy

- **Stabilized portfolio occupancy**: target >95% for high-quality REITs (>90% datacenter is impressive given long build-out)
- **Trend** matters more than level — declining occupancy is the early-distress signal
- **Same-property occupancy** (excluding new acquisitions/dispositions) is the cleanest read

### Lease Expiration Ladder

- **Concentration risk**: a REIT with >20% of leases expiring in any single year faces re-leasing risk concentrated in one rate/cycle environment
- **Weighted Average Lease Term (WALT)** — duration-equivalent for the lease portfolio; longer = more stable, less mark-to-market upside
- **Mark-to-market upside**: if current asking rents > expiring lease rents, the lease ladder is a built-in earnings growth bridge — see datacenter section below

### Same-Store NOI Growth

- **Best single operating metric** — strips acquisition/disposition effects
- **Decomposition**: rent growth + occupancy change + expense control
- **Cycle indicator**: same-store NOI growth >5% = strong cycle; <2% = mid-cycle; <0% = late-cycle / recession

### Development Pipeline

- **Yield-on-cost** (developing yield) vs **acquisition yield** (cap rate) — gap measures development premium
- **Funded vs unfunded pipeline** — funded reduces capital-raise risk
- **Pre-leased %** — high pre-lease (e.g., 70%+ for datacenter) reduces speculative risk

## Balance Sheet (track for any REIT thesis)

### Leverage Metrics

- **Debt / EBITDA**: target <6× for investment-grade; >7× concerning; >8× distress
- **Net Debt / Total Asset Value**: target <40%; >55% high-leverage
- **Loan-to-Value (LTV)**: target <50% on average property basis

### Coverage Metrics

- **Fixed Charge Coverage** = EBITDA / (Interest + Preferred Dividends) — target >2.5×
- **Interest Coverage** = EBITDA / Interest — target >3.5×
- Lower coverage = less margin for rate increases or NOI weakness

### Debt Maturity Ladder

- **Average debt maturity**: target >5 years for stable portfolio
- **% of debt fixed-rate vs floating-rate**: high-quality REITs >85% fixed
- **Concentration risk**: any single year's maturities >20% of total debt is a refinancing risk concentration

## Datacenter REIT Specifics (the research process target sector)

The datacenter sector deviates from generic REIT analysis in several material ways. Per [[foundation/macro/ai-cycle-calibration-2024-2026]], AI-cycle dynamics dominate near-term datacenter pricing.

### Power Is the Binding Constraint

- **Hyperscaler capacity expansion is now power-limited, not silicon-limited**
- Datacenter site selection prioritizes power availability + grid interconnection over land cost or proximity to fiber
- Power-permitting lead times: 3-7 years for substantial new capacity in major US markets
- **Implication**: existing datacenter capacity has scarcity rents that can't be quickly arbitraged away even if demand slows

### Mark-to-Market Lease Pricing

- **Current state (2026)**: Digital Realty / Equinix reporting **new-lease pricing 30-50% above expiring leases**
- **Vacancy in major markets**: <3% — well below historical norm
- **Lease structure**: typically 3-10 years with escalators; mark-to-market reset on renewal
- **Compounding mechanic**: 30-50% pricing power compounds across multi-year lease portfolio. A REIT with 10-15% of leases expiring annually + 30-50% mark-to-market gets ~3-7% portfolio NOI growth FROM PRICING ALONE, before any new development

This is the **single most distinctive datacenter REIT signature** of the current cycle — and the **first signal to watch for AI-cycle deceleration**: if mark-to-market growth drops below ~15%, supply is catching demand.

### Power-Adjusted Capacity Metrics

Standard occupancy doesn't capture datacenter capacity properly. Use:
- **MW (megawatt) capacity** rather than square-footage
- **MW utilization** = MW leased / MW available
- **MW under construction** = the development pipeline in power terms
- **MW pre-leased** = % of new builds with signed leases at delivery

Datacenter REIT comparisons require MW-normalization, not SF-normalization.

### Hyperscaler Tenant Concentration

- Datacenter REITs have **significant tenant concentration**: hyperscaler customers (Microsoft, Google, AWS, Meta, Oracle) are typically top-10 tenants representing 30-50% of revenue
- **Investment-grade credit** of these tenants partially offsets concentration risk
- **Tail risk**: if any hyperscaler cancels/scales back significantly, single-tenant concentration ratio spikes
- **Counterargument**: hyperscaler capex is multi-year contractual; cancellations are commercially expensive

### Key Datacenter REITs (May 2026)

- **Equinix (EQIX)** — interconnection-focused (500K+ global interconnections); colocation business model; premium P/FFO
- **Digital Realty (DLR)** — wholesale + colo hybrid; 300+ facilities; 5,000+ customers; targeting 2026 core FFO/share $7.90-8.00 (~8% growth)
- **Iron Mountain (IRM)** — hybrid storage + datacenter; lower-quality datacenter exposure but cheap entry
- **Equinix vs Digital Realty differentiation**: EQIX = retail + interconnection density; DLR = wholesale + scale + power-per-rack capacity

## Datacenter REIT Specific Risks

- **Power grid bottleneck resolution**: if grid expansion accelerates 2027-2029, supply could overshoot demand on a 2-3 year lag
- **Hyperscaler self-build trend**: hyperscalers are increasingly building owned datacenters vs leasing — REIT-leased % could compress
- **Geographic exposure**: heat-stressed markets (Phoenix, Atlanta) face cooling cost inflation; northern markets (Virginia, Iowa) less exposed
- **Cap-rate expansion risk**: if rates rise, datacenter cap rates compress relative to property NOI growth — this is the rate-risk channel that affects all REITs
- **AI-cycle deceleration**: per [[foundation/macro/ai-cycle-calibration-2024-2026|AI cycle calibration]], any monetization-gap-doesn't-close scenario flows through to datacenter pricing power

## Honest Caveats / Failure Modes

- **AFFO is judgment-laden**. Two REITs with identical operating performance can report different AFFO based on what counts as maintenance vs growth capex. **Always cross-check** maintenance-capex assumption vs trailing 5-year average vs sector norm.
- **NAV is point-in-time**. A NAV calculation reflects the cap rate environment when computed. NAV from 2021 (low rates) and NAV from 2024 (high rates) for the same portfolio differ dramatically. **Always use cap rates consistent with the current rate environment**.
- **P/FFO multiples differ across sectors**. Comparing a datacenter P/FFO (24×) to a retail P/FFO (10×) is meaningless without sector context. The ranges in the table above are minimum-context.
- **Datacenter REIT is regime-specific**. The current 30-50% mark-to-market pricing is an AI-cycle-specific phenomenon; pre-2023, datacenter REIT mark-to-market was <10%. Decisions made on current pricing assume the cycle continues.
- **Dividend yield is not a reliable buy signal**. High dividend yield often signals stress (price has fallen, dividend hasn't been cut yet but probably will be). AFFO coverage ratio is more important than yield level.
- **Public REITs vs private real estate**: publicly-traded REITs have liquidity premium/discount to private property values. NAV calculations from private-market cap rates may not match public-market trading values for 12-18 months.
- **Rate sensitivity is asymmetric**: REITs are long-duration assets; +100 bps in 10y Treasury typically compresses REIT prices 8-15%. **Never go long REITs without a Fed-path view**.
- **Tax structure matters**: REITs avoid corporate tax by distributing 90%+ of income. International investors face withholding tax (typically 15% for US REITs). Holding REITs in tax-deferred accounts is structurally optimal.

## Open Questions for Future Investigation

1. **What's the right yield-on-cost gate** for AI-infra-REIT new development? (current development yields cited at 9-12% by EQIX/DLR for AI-tenanted projects vs 6-8% acquisition cap rates — gap is unusually wide)
2. **How should we adjust AFFO for datacenter-specific GPU-cooling capex** that didn't exist in pre-AI datacenter operations?
3. **What's the historical track record of REIT NAV calls?** (suspicion: NAV-discount entry has worked, NAV-premium entry has failed — but needs empirical verification)
4. **Are sovereign-fund / private-credit AI-infra deals comparable to public REITs**? (Brookfield, Blackstone, KKR all building $10B+ private AI-infra portfolios; pricing benchmarks may be diverging from public REIT cap rates)
5. **Power purchase agreement (PPA) structure**: how should we evaluate REITs with long-term PPAs that lock energy costs vs those with floating exposure?

## Related

- [[foundation/macro/ai-cycle-calibration-2024-2026]] — sister page; cycle calibration for AI-infra REIT thesis
- [[foundation/methodology/investment-thesis-protocol]] — 9-section flow that uses this methodology in §6 (target identification) and §7 (stress test)
- [[foundation/market-wisdom/margin-of-safety]] — Graham's foundation; NAV-discount entry is the REIT operationalization
- [[foundation/market-wisdom/owner-earnings]] — Buffett's GAAP correction; AFFO is the REIT analog of owner earnings
- [[foundation/market-wisdom/circle-of-competence]] — datacenter REIT sits at intersection of AI/software (strong CoC) + real estate (strong CoC)
- [[foundation/sources/buffett-essays]] — capital allocation framework applies to REIT capital decisions
- [[templates/investment-thesis]] — full thesis template; §6 (target identification) and §8 (sizing) cite this methodology
- [[templates/investment-watchlist]] — lighter template; uses P/FFO + AFFO yield as primary triage filters

## Sources

- [REIT Valuation Methods — Financial Edge Training](https://www.fe.training/free-resources/real-estate/reit-valuation-methods/)
- [Real Estate and REIT Valuation: NAV, FFO, AFFO, Cap Rates — IB Interview Questions](https://ibinterviewquestions.com/guides/valuation-investment-banking/real-estate-reit-valuation-nav-ffo-affo-cap-rates)
- [REIT Valuation Using NAV and FFO — AnalystPrep CFA Level 2](https://analystprep.com/study-notes/cfa-level-2/reit-share-value-calculation-using-net-asset-value-p-ffo-p-affo-and-discounted-cash-flow-approaches/)
- [FFO vs AFFO Explained — Private Equity Bro](https://privateequitybro.com/ffo-explained-the-core-metric-behind-reit-valuation-multiples/)
- [P/FFO Definition, Formula — Corporate Finance Institute](https://corporatefinanceinstitute.com/resources/commercial-real-estate/p-ffo/)
- [Real Estate Modeling Quick Reference — Breaking Into Wall Street](https://samples-breakingintowallstreet-com.s3.amazonaws.com/80-BIWS-RE-Valuation.pdf)
- [Why DLR and EQIX Are the Best Data Center REITs for AI — MarketWise](https://marketwise.com/the investing layer)
- [Best Data Center REITs for 2026 — Motley Fool](https://www.fool.com/the investing layer)
- [Digital Realty, Equinix Ramp Datacenters as AI Drives Demand — S&P Global](https://www.spglobal.com/market-intelligence/en/news-insights/articles/2025/6/digital-realty-equinix-ramp-up-datacenters-as-ai-drives-demand-90542889)
- [Data Center REITs See Robust Demand Despite Power Constraints — Nareit](https://www.reit.com/news/articles/data-center-reits-see-robust-demand-despite-power-supply-constraints)
- [Top Data Center REITs: EQIX, DLR, IRM — HeyGoTrade](https://www.heygotrade.com/en/blog/Top-Data-Center-REITs-EQIX-DLR-IRM/)
- [Data Center REITs: Own Real Estate Behind AI — Chilton Capital](https://chiltoncapital.com/2025/08/01/data-center-reits-own-the-real-estate-behind-ai-august-2025/)

---

*Methodology page — `growing` status. Will refine to `evergreen` after first complete REIT-thesis iteration that uses it (currently substrate; thesis flow not yet exercised against it).*
