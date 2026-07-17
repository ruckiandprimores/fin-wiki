---
title: "Semiconductor Economics Primer — Eight Layers, Eight Moats"
status: growing
created: 2026-07-17
updated: 2026-07-17
tags: [foundation, asset-classes, semiconductors, moat, value-chain, foundry, eda, semi-cap, memory, cyclicality, investment, value-investing]
---

# Semiconductor Economics Primer — Eight Layers, Eight Moats

> **TL;DR**: Semiconductors are not one industry with one moat — they're an **eight-layer value
> chain**, and moat quality changes completely from layer to layer. The tightest monopolies sit
> in the *tools and software* (EUV lithography, EDA), not in the chips themselves; the most
> boom-bust layer (memory) sits closest to a physical commodity despite an oligopoly structure.
> **The layer determines the economics far more than the company does.** This page maps the
> chain layer by layer — what each does, who holds it, why the moat is what it is, and how
> cyclical it runs — as durable structural knowledge, not a name-by-name verdict. *Analysis, not
> advice: no rankings, no entry calls, nothing here says what to own or when.*

## Why One Chain, Eight Different Businesses

A semiconductor ends up in a phone or a datacenter only after passing through design software,
architecture licensing, chip design, physical manufacturing, the equipment that does the
manufacturing, memory, analog/power components, and finally test/assembly/packaging. Each stage
is a **separately-moated business** with its own competitive structure, capital intensity, and
cycle shape. Treating "semiconductors" as a single sector obscures this — a fabless AI-chip
designer and a DRAM producer share a supply chain but have almost nothing in common as
businesses (margin structure, cyclicality, customer concentration all differ).

The organizing question for each layer, same as [[foundation/asset-classes/pharma-economics-primer|pharma's patent moat]]
or [[foundation/asset-classes/defense-economics-primer|defense's monopsony moat]]: **who holds
the structural power in this layer, and why can't it be competed away?**

**Data-confidence note:** market-share figures throughout are industry-standard approximate
figures (the kind that circulate across sell-side and trade-press coverage), not freshly
re-sourced from primary filings for this page. Treat percentages as "roughly this order of
magnitude and roughly this concentrated," not as precise-to-the-point figures — and expect
some drift as the AI-driven fabless mix (layer 3) shifts fastest of any layer.

## The Eight Layers

### 1. EDA / Design Software

**What it does:** the software used to design a chip — schematic capture, simulation, place-and-route,
verification — before a single transistor is fabricated. Every chip in existence was designed inside
one of a handful of EDA toolchains.

**Players (approximate):** Synopsys ~30-32%, Cadence ~30%, Siemens EDA ~13-15% — effectively a
**duopoly-plus-one** at the leading edge, with a long tail of point-tool vendors below.

**Moat, and why:** the moat is not the software itself but the **switching cost around it** —
career-level engineer training (chip designers spend years learning one toolchain), per-node
**foundry co-optimization** (EDA vendors and TSMC jointly certify process design kits, or PDKs,
for each new node — an EDA tool that isn't PDK-certified for the latest node is unusable for
leading-edge design), and the cost of a design re-spin if a team switches mid-project. This is a
structural, multi-generational lock-in, closer to enterprise-software switching costs than to a
typical hardware moat.

**Cyclicality:** ~80% of EDA revenue is subscription/recurring. This is the **least cyclical
layer in the entire chain** — closer to enterprise software economics than to semiconductors.
Chip design activity dips in downturns but licenses are multi-year commitments, so revenue
smooths through the cycle far more than any downstream layer.

**Where value capture sits:** captured at the point of *access* — you cannot design a
competitive modern chip without licensing one of these toolchains, regardless of who eventually
manufactures or sells the chip.

### 2. IP / Architecture

**What it does:** licenses the instruction-set architecture (ISA) and processor core designs that
fabless companies build chips around, rather than each company designing an ISA from scratch.

**Players (approximate):** ARM holds roughly ~99% of smartphone application-processor
architecture licensing — closer to a true monopoly than anything else in the chain outside
lithography.

**Moat, and why:** the moat is **not the silicon design** — it's the ISA plus the decades of
software, compilers, operating-system support, and developer tooling built around it. A
competing ISA needs not just competitive hardware but an entire software ecosystem to match,
which is why RISC-V (a royalty-free, open ISA) has made real headway at the **low end**
(microcontrollers, embedded, cost-sensitive designs) but has not displaced ARM at the high end,
where the software stack is a decade-plus ahead. This is a wide moat with a visible,
credible, structurally different long-term challenger — worth tracking as a moat-erosion case
study distinct from anything in pharma or defense (no patent cliff, no contract-type swing; the
threat here is a slow ecosystem migration).

**Cyclicality:** licensing/royalty revenue tracks unit shipments of the devices built on the
architecture — moderately cyclical, smoothed by the breadth of end markets (phones, IoT,
automotive, now datacenter) an ISA licensor serves.

**Where value capture sits:** captured per-unit, at volume, across nearly every device category
that ships a processor — the widest reach of any layer even though the per-unit take is small.

### 3. Fabless Logic / Accelerators

**What it does:** designs chips (without owning manufacturing) and has them fabricated at a
foundry. This is the layer producing CPUs, GPUs, AI accelerators, and custom ASICs.

**Players (approximate):** NVIDIA ~80-90% of the AI-accelerator market; in custom ASIC design
services, Broadcom ~55-60% and Marvell ~13-15%.

**Moat, and why:** for the AI-accelerator leader, **the moat is the software ecosystem (CUDA),
not the chip.** A competing accelerator can match or approach NVIDIA silicon on paper
specifications, but the switching cost is the years of software, libraries, and trained
engineering practice built on top of one programming model — a software lock-in structurally
similar to the EDA layer's, just one level up the stack. For custom-ASIC design houses, the moat
is closer to a services relationship: deep co-design engagements with a small number of
hyperscale customers, high switching costs once a design is committed to silicon.

**Cyclicality:** this is **secular AI demand riding on top of a genuinely cyclical hardware
base** — semiconductor demand has always run in inventory and capacity cycles, and layering a
structural growth story on top does not remove that cyclicality, it just makes the cycle harder
to read in the data (see [[foundation/macro/ai-cycle-calibration-2024-2026]] for the live
calibration of exactly this ambiguity).

**Structural tension worth naming plainly:** the largest customers of the AI-accelerator
incumbent are simultaneously its largest funders of **competing, in-house silicon** — every
hyperscaler running large accelerator-purchase programs also runs an internal custom-chip
program. This is not a prediction about outcome, just a structural fact about who the customer
base is and what they're building in parallel.

**Where value capture sits:** captured at the design/IP layer of the chip (the architecture and
software around it), even though the *manufacturing* of the physical die happens one layer down,
at the foundry.

### 4. Foundry

**What it does:** physically manufactures chips designed by fabless companies (and by
integrated device manufacturers who also outsource some production).

**Players (approximate):** TSMC ~62-67% of foundry revenue overall, and **~90%+ of
leading-edge capacity (≤5nm)** — the leading-edge share is the number that matters, since that's
where the highest-value chips (AI accelerators, latest-generation mobile/PC processors) are
manufactured.

**Moat, and why:** this is **the widest moat among the physical (non-software) layers of the
chain.** It rests on a decade-plus **cumulative process-node lead**, yields other fabs cannot
match at comparable node sizes, and a **learning-curve/capex advantage that compounds** — each
new node is funded by cash flow from the prior node's volume, and the leader's head start means
it is always deploying the next node's capex from a larger base than any challenger. This is not
a moat that can be bought; it has to be earned generation over generation, and a gap once opened
is very difficult to close.

**The durable, non-obvious point:** the discount markets sometimes apply to leading-edge foundry
exposure is **geopolitical (concentration in Taiwan), not an economic weakness in the business
itself.** The moat, judged purely on manufacturing economics, is arguably the strongest in the
entire chain. The risk premium attached to it is a separate, geography-driven variable layered
on top — worth keeping conceptually distinct from moat quality.

**Cyclicality:** cyclical at the revenue line (foundry utilization tracks the semiconductor
capex cycle closely) but riding a **secular growth trend** in total silicon demand — the
cyclicality is real but happens around a rising trend line, not a flat one.

**Where value capture sits:** captured on **manufacturing at the leading edge specifically** —
trailing-edge (mature-node) foundry capacity is a much more commoditized, multi-player market
with far thinner margins.

### 5. Semiconductor Capital Equipment ("Semi-Cap")

**What it does:** builds the machines that foundries and memory makers use to actually
manufacture chips — lithography, etch, deposition, process control, and metrology tools.

**Players (approximate):** ASML holds effectively **100% of EUV lithography and 100% of
High-NA EUV** (the next-generation tool). Process-control/inspection is more of an oligopoly:
Applied Materials, Tokyo Electron, Lam Research, and KLA collectively hold roughly ~50-57% of
that sub-segment.

**Moat, and why — arguably the single widest moat in the whole chain:** an EUV lithography
machine is built from roughly 100,000 parts, incorporates optics supplied uniquely by Zeiss and
a light source supplied uniquely by Cymer (both effectively captive to ASML), and **no other
company even attempts to build a competing machine.** This is not "market leadership" in the
usual sense — it is the closest thing in the entire semiconductor chain to a true, unchallenged
technological monopoly. Every leading-edge chip in the world, regardless of who designs it or
which foundry manufactures it, passes through an ASML machine at some point.

**Cyclicality:** the **classic capex-cycle layer.** Semi-cap equipment orders are the most
directly capex-driven revenue line in the entire chain — foundries and memory makers place
large, lumpy equipment orders tied to capacity-expansion decisions, so semi-cap revenue swings
harder and faster than foundry or fabless revenue in either direction. This is the layer where
the boom/bust shape of the "silicon cycle" is most visible in the numbers.

**Where value capture sits:** captured at the point of **irreplaceable technological capability**
— EUV specifically. Non-EUV, more commoditized tool categories (some deposition, some etch
sub-segments) look more like a normal oligopoly with real competitive pressure.

### 6. Memory

**What it does:** produces DRAM (working memory) and NAND (storage memory) — the layer with the
most direct exposure to commodity-style pricing dynamics despite a concentrated player base.

**Players (approximate):** DRAM is essentially a **3-player oligopoly** — Samsung, SK Hynix, and
Micron historically holding ~95% combined, with Chinese entrant CXMT now taking roughly ~8% and
rising. NAND is more fragmented (roughly six meaningful players, plus Chinese entrant YMTC).

**Moat, and why — "oligopoly on structure, commodity on product":** the player count is small
(oligopoly), but the *product itself* is largely interchangeable between suppliers at a given
generation — DRAM from one maker is broadly substitutable for DRAM from another, unlike an
EUV machine or a CUDA-locked accelerator. That combination — few suppliers, fungible product —
produces the textbook commodity-oligopoly dynamic: pricing power exists in principle (few
sellers) but discipline is hard to sustain in practice (each player is tempted to run capacity
hard in an upcycle, which seeds the next downturn). This is structurally why memory is **the
most boom-bust layer in the chain** — the moat is real (three players, high fixed-cost barriers
to a fourth entrant) but doesn't produce the pricing stability that EDA's or ASML's moats do.

**HBM as a temporary exception:** High-Bandwidth Memory (the memory type packaged directly
alongside AI accelerators) currently commands a materially higher moat than commodity DRAM/NAND,
for three reasons that are worth distinguishing because they have different durability: (1)
**genuine technical difficulty** (stacking dies with through-silicon vias is hard to execute at
yield — durable until competitors catch up on process), (2) **customer qualification** (accelerator
makers qualify a specific supplier's HBM into their design — moderately durable, a design-cycle
timescale), and (3) **sold-out capacity** (a function of current demand exceeding current
supply — the least durable of the three, resolves as capacity is added).

**The wafer-cannibalization mechanic (a genuine structural feature, worth understanding on its
own terms):** HBM is roughly **3-4× as wafer-intensive per bit** as conventional DRAM, because
of the stacking and interposer overhead. This means every wafer a memory maker allocates to HBM
production is wafer capacity *not* available for conventional DRAM — so a demand surge in HBM
mechanically tightens the supply of ordinary DRAM even without any change in ordinary-DRAM
demand. This is a structural linkage between two sub-markets that would otherwise look unrelated,
and it's a distinctive feature of this layer not seen in the same form anywhere else in the
chain.

**Cyclicality:** the most violently boom-bust layer in the entire eight-layer chain — memory
pricing has historically swung by multiples (not percentages) peak-to-trough within a single
cycle.

**Where value capture sits:** captured unevenly across the cycle — concentrated in upcycles when
pricing power briefly asserts itself, given back in downcycles when oversupply drives price
below cost for some producers.

### 7. Analog / Power / Mixed-Signal

**What it does:** produces the components that interface the digital world with the physical
world — power management, sensors, signal conversion, connectivity — the parts of an electronic
system that are not pure digital logic.

**Players (approximate):** a fragmented oligopoly — Texas Instruments, Analog Devices,
Infineon, NXP, STMicroelectronics, Microchip, ON Semiconductor, among others, each with meaningful
but non-dominant share across a wide catalog of product families.

**Moat, and why — the quality-compounder corner of the chain:** analog moats look structurally
different from digital-logic moats. Products have **10-20 year lifecycles** (an analog part
designed into an industrial system stays in that system's bill of materials for its entire
production run, unlike digital logic which refreshes every node), companies carry **100,000+
SKU catalogs** (breadth itself is a barrier — a competitor would need to replicate an enormous
product range to compete broadly, not just beat one part), and each design carries **per-socket
design-win lock-in** — once an analog part is designed into a customer's product, replacing it
requires the customer to requalify the entire system, which they have little incentive to do for
a mature, working design. Customer bases here also tend to be far more **diversified** across
end markets (industrial, automotive, medical, consumer) than the concentrated hyperscaler/mobile
customer bases further up the chain.

**Cyclicality:** cyclical, but **shallower** than memory or leading-edge logic — the combination
of long product lifecycles, diversified end markets, and **far lower capital intensity**
(analog fabs run on trailing-edge process nodes; there is no 2nm arms race in this layer) means
analog businesses don't see the capex-driven boom/bust amplitude that semi-cap or memory see.

**Where value capture sits:** captured through breadth and durability rather than through a
single technological chokepoint — no analog company has anything resembling ASML's or TSMC's
structural monopoly; the moat is the aggregate of thousands of individually-unremarkable but
collectively durable design wins.

### 8. Test / Assembly / Materials / Substrates

**What it does:** the back-end of the chain — testing finished chips, assembling/packaging them
into their final form, and supplying the raw materials (wafers, chemicals, gases) consumed
throughout manufacturing.

**Players (approximate):** automated test equipment (ATE) is an **Advantest + Teradyne
duopoly**. Outsourced assembly and test (OSAT — companies like ASE and Amkor) is more
commoditized, **except** in **advanced packaging**, which has become a genuine bottleneck in the
AI supply chain (CoWoS-type packaging capacity has been a documented constraint on how fast
AI-accelerator supply can scale, independent of wafer or design capacity). Materials suppliers
(Entegris being a representative example) sell consumables — wafers, specialty chemicals,
filtration — that are **consumed regardless of which fabless company or foundry wins** any given
design cycle.

**Moat, and why:** ATE's duopoly moat resembles semi-cap's, at smaller scale — high
R&D intensity, deep integration with customer test flows. OSAT's moat is thinner and more
commoditized in its base business, **except** where advanced packaging has created a genuine,
temporary chokepoint layered on top of an otherwise-commoditized business. Materials suppliers
run something closer to a **razor-blade/consumable model** — their revenue scales with total
industry wafer volume, largely agnostic to which company's chip that wafer becomes, which makes
this sub-layer's revenue more diversified across the *outcome* of the fabless/foundry
competition than almost any other layer.

**Cyclicality:** test and OSAT track the underlying unit-volume cycle fairly directly; materials
suppliers are somewhat smoothed by their exposure to aggregate wafer volume rather than to any
single product cycle.

**Where value capture sits:** thinly distributed across a commoditized base, with two notable
exceptions where real moats exist within the layer — ATE's duopoly, and advanced-packaging
capacity specifically.

## Durable Insights (Why This Page Exists)

- **Moat quality varies enormously by layer, and the layer matters more than the company.**
  The strongest, most defensible monopolies in the entire semiconductor value chain live in the
  *tools and software* — EUV lithography (semi-cap) and EDA — not in the chips themselves. A
  company's moat is largely inherited from which layer it operates in before it's a function of
  anything company-specific.
- **The cyclicality gradient runs from near-secular to violently boom-bust, layer by layer:**
  EDA (near-secular, subscription-like) → analog (shallow cycle, long product lifecycles) →
  foundry (cyclical revenue riding a secular trend) → semi-cap (classic capex cycle) → memory
  (the most violently boom-bust layer of all). Knowing which layer a business sits in tells you
  more about its cycle shape than its growth narrative does.
- **"Picks and shovels" has layers of its own.** The intuitive picks-and-shovels framing (sell
  the tools, not the product, and profit regardless of who wins) is not one homogeneous
  category — the deeper toward the actual tools you go (EDA, semi-cap, materials), generally
  the wider the moat and the more the business profits from industry activity *regardless of
  which fabless company or which AI architecture ultimately wins.* A picks-and-shovels
  framing at the fabless-adjacent layer (e.g. a custom-ASIC design house) still carries real
  customer-concentration and design-cycle risk that a materials supplier several layers back
  does not.
- **A low P/E means opposite things in different layers.** In a near-secular layer (EDA), a
  depressed multiple more often signals mispricing of a stable business. In a violently
  cyclical layer (memory, semi-cap), a depressed multiple can simply mean **peak earnings priced
  as if they were sustainable** — the classic cyclical-valuation trap, where the multiple looks
  cheapest right before earnings fall and looks most expensive right before they recover. See
  [[foundation/methodology/commodity-cyclical-valuation-inversion]] for the general methodology
  behind reading multiples correctly by cyclicality regime — this chain is close to a
  textbook illustration of why the same P/E number requires opposite interpretations depending
  on which of the eight layers it's attached to.
- **Concentration of criticality is a systemic, plainly statable fact:** essentially all
  leading-edge silicon on Earth depends on two companies for two irreplaceable steps — ASML
  for the lithography tool that makes the leading-edge process possible, and TSMC for the
  manufacturing capacity and yield that actually produces the chips at that leading edge. This
  is not a prediction or a risk call; it is a description of the chain's current topology, and
  it's worth holding in mind independent of any single company's valuation.

## Honest Caveats / Failure Modes

- **Market-share figures are approximate and will drift.** These are widely-cited,
  industry-standard approximations rather than freshly re-sourced primary-filing numbers (see
  the data-confidence note above). The fabless layer (3) in particular is the fastest-moving —
  AI-accelerator share, hyperscaler in-house silicon share, and custom-ASIC share are all live
  variables changing faster than the rest of the chain.
- **Layer boundaries are not always clean.** Some companies straddle layers (an integrated
  device manufacturer that both designs and fabricates; a foundry that also does advanced
  packaging) — the eight-layer framing is a structural simplification, useful for understanding
  moat mechanics, not a strict taxonomy every company fits into cleanly.
- **The AI-driven demand surge is a live variable, not a settled structural fact.** Several
  layers in this chain (fabless accelerators, HBM, advanced packaging) currently sit inside an
  active demand cycle whose durability is genuinely uncertain — see
  [[foundation/macro/ai-cycle-calibration-2024-2026]] for the ongoing calibration of that
  question. This page describes chain *structure*, which moves on a decade timescale; it does
  not describe where any layer sits in its current cycle.
- **Geopolitical risk is concentrated exactly where economic moat is strongest.** The
  foundry layer's discount is explicitly geopolitical rather than economic (per layer 4 above) —
  worth remembering that the widest economic moats in this chain (foundry, semi-cap) are also
  where geographic concentration risk is most acute, and those two facts are not the same risk
  even though they're often discussed together.
- **This page is structure, not valuation.** No layer or company here is more or less
  "attractive" as a matter of this page's content — cyclicality and moat quality describe how a
  business behaves, not whether its current price reflects that correctly.

## Open Questions {#open-questions}

1. **How durable is the RISC-V erosion at the low end of the IP/architecture layer (2),
   and does it show any credible sign of climbing toward the high end where ARM's software
   moat currently holds?** Would change the multi-decade read on the widest non-lithography
   software moat in the chain.
2. **Does advanced-packaging capacity (layer 8) resolve into a durable oligopoly moat, or is
   it a transient bottleneck that dissolves once OSAT players build out CoWoS-equivalent
   capacity?** Determines whether layer 8's temporary exception becomes a permanent one.
3. **How does the wafer-cannibalization mechanic between HBM and conventional DRAM (layer 6)
   evolve as memory makers build HBM-dedicated capacity rather than reallocating from existing
   lines?** The mechanic as described assumes shared capacity; dedicated capacity would sever
   the linkage.
4. **Is China's rising share in DRAM (CXMT) and NAND (YMTC) a genuine long-run structural
   shift in the memory oligopoly's player count, or a subsidized entrant that behaves
   differently in a downturn than the incumbent three?** A fourth disciplined player changes
   memory's cycle dynamics materially; a fourth undisciplined player might not.

## Related

- [[foundation/asset-classes/pharma-economics-primer]] — structural sibling: patent/regulatory
  moat anatomy, for comparison against this chain's tool/software moat anatomy
- [[foundation/asset-classes/defense-economics-primer]] — structural sibling: monopsony/contract
  moat anatomy, the third primer in this set
- [[foundation/methodology/execution-vehicle-typology]] — how exposure to a layer is naturally
  expressed (direct name vs ETF wrapper) once moat/cyclicality per layer is understood
- [[foundation/methodology/commodity-cyclical-valuation-inversion]] — the general methodology
  for why the same multiple reads oppositely across cyclicality regimes; this chain is a
  layer-by-layer illustration of the point
- [[foundation/macro/ai-cycle-calibration-2024-2026]] — the live demand-cycle calibration that
  several of these layers (fabless accelerators, HBM, advanced packaging) currently ride
- [[investment/theses/ai-infrastructure-bottleneck]] — thesis-layer treatment of the
  infrastructure bottlenecks this primer's structural layers underpin

## Sources

Research compiled 2026-07-16 from industry-standard, widely-cited approximate share figures
circulating across sell-side and trade coverage of the semiconductor value chain (EDA, IP
licensing, foundry, semi-cap equipment, memory, analog, and back-end/materials segments). Per
the data-confidence note above, treat as directionally reliable rather than freshly re-verified
against primary filings; a follow-up pass citing primary 10-K/annual-report disclosures per
layer is a natural next step if this page graduates past `growing` status.

---

*Sector primer — `growing` status. Plays the same role for the semiconductor value chain that
[[foundation/asset-classes/pharma-economics-primer]] and
[[foundation/asset-classes/defense-economics-primer]] play for their respective verticals:
structural CoC-build substrate, not thesis validation. The eight-layer structure is durable
(decade-timescale); any specific cyclical positioning within a layer is not covered here and
would stale quickly if it were.*
