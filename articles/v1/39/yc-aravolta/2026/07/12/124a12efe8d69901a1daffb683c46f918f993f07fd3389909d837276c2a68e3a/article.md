---
schema_version: "1.0.0"
document_id: "124a12efe8d69901a1daffb683c46f918f993f07fd3389909d837276c2a68e3a"
company_key: "yc-aravolta"
company: "Aravolta"
source_id: "yc-aravolta-news-import-85aed3e09b97"
canonical_url: "https://www.aravolta.com/blog/ai-bubble-needs-to-accelerate"
published_at: null
first_seen_at: "2026-07-24T07:29:59.544602+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:3e7d5e68690e2a9519de0e31507e98da4af887eeda819c35a3b1cee357a3bb5f"
---

# The AI 'Bubble' Needs to Accelerate | Aravolta Blog

Every time a technology chart goes vertical, the bubble chorus starts singing. It happened with the railroads in the 1880s. It happened with the internet in the late 1990s. And now it is happening with Generative AI.


The narrative is seductive: too much money chasing too few ideas, a collapse is inevitable.


But the underlying data tells a far more complex story. When you look at who is building, how it is being funded, and what the capacity is actually being used for, the AI buildout is not necessarily a bubble.


It is true that there may be some over-exuberance, some misallocated resources, and misplaced bets, as there is in every growth market. However, there continues to be a **supply-side crunch** driven by the ever changing needs of AI (hint - changing cooling needs and new generations coming out every 6-months).


We are not burying fiber in the ground and hoping someone invents Netflix later.


Across the GPU-dense environments we monitor at Aravolta, the story is the same: new clusters are saturated, power is scarce, and operators are racing to stand up capacity, not tear it down.


This is not another tech boom. It is the **early industrialization of cognition** : a capital intensive, energy constrained expansion that lowers the marginal cost of reasoning itself. Wall street analysts have no clue how to price in how true machine intelligence can reshape our economy quite yet.


And unlike the internet era, this buildout requires many scarce inputs at the same time. We need chips and we need energy. We also need more research and to use machine intelligence to create even more intelligence. We have crossed the Rubicon in the quest for AGI - that march can't be turned around.


There are likely to be lots of people who made the wrong bet on the wrong technology at the wrong time - and that capital will be wiped out. Yet the fundamental principle behind “scale-pilling” remains the same - **more compute means smarter models which (eventually!) turns into GDP when the dust settles.**


Let's dive into the data.


## A Brief History On Scale Pilling


By history, I mean 2023/2024, which we call “The Age of Scaling”. Popularized by Dylan Patel on the Dwarkesh podcast and lots of researchers like Ilya Sutskever and Rich Sutton's 2019 “The Bitter Lesson” argued that compute trumps clever algorithms. A March 2023 X thread by Andrej Karpathy went viral, quipping “scaling is the new black” after GPT-4 demos. This fed into broader discourse - the fundamental belief was that we cannot afford not to build build build.


Bigger cluster = More training = Smarter Model = Intelligence to the moon = $$$ (hopefully?)


And no one wants to get left behind in this equation.


This is where the industry's psychology began to shift. For over a year, “just scale it” was the dominant mindset, a kind of inevitability narrative. The moment serious voices questioned that trajectory, it created a wave of uncertainty: if scaling isn't the only lever, then the economics of the entire buildout need to be re-evaluated. That's the backdrop for why markets reacted so violently.


Which is why Ilya's comment on Dwarkesh last week hit so hard: **“Scaling is not enough”** . That and Nvidia falling despite beating earnings and a very bearish market with Michael Burry headlines calling a bubble has led to today's climate.


**What everyone is missing is that two things can be true:**


1. Machine intelligence does not scale linearly with cluster size (aka, there are indeed, diminishing returns).
2. AI will embed itself in every corner of the economic machine and that means more and more compute will be needed (even if the compute becomes more efficient).


## Railroads, Fiber, and GPUs


To evaluate bubbles, ignore the hype. Look at CapEx as a percent of GDP, the real supply constraints, and the long term economic impact.


- We have already surpassed Dot com era infrastructure intensity.
- We are now moving toward railroad era intensity, but with far tighter physical constraints and far more immediate utility.
- And unlike fiber with DWDM, there is no way to magically multiply GPU capacity.


### Infrastructure Intensity & Outcomes Across Eras


Era Primary Asset Peak Investment (% of GDP) Supply Constraint The Economic Outcome


Railroads
(1870s–1890s)


Physical Track ~6% Land Rights & Steel Massive investor wipeouts, but eventually ~25% uplift to U.S. GDP through freight productivity and land value expansion.


Telecom
(1996–2001)


Fiber Optics ~1% Capital (Until DWDM) Utilization dropped to ~1%, crashing prices to zero. Value was only realized 15 years later by Web 2.0.


AI
(2025 Projected)


GPU / Energy ~1.4% Energy & Packaging Labor substitution + software automation give immediate OpEx ROI. To justify capex at this scale, AI must only generate ≥0.4–0.6%*.


* Economists use a 3–5 year window because that's the typical productive life of technology capital and the historical timeframe over which new infrastructure translates into measurable GDP gains. GDP Figures From Investment Research Partners.


**The takeaway:** we have already surpassed the Dot-com infrastructure intensity and are approaching “railroad” territory, but with much tighter physical constraints and far more immediate utility.


## Demand: Forecast vs. Backlog


**The single biggest difference between 2000 and 2025 is how demand shows up.**


### 2000s: Forecast Driven Fiber


In 1999, Global Crossing and friends trenched fiber across oceans based on models that said internet traffic would double every 100 days. That story collapsed when DWDM let operators pump 100x more data down the same glass. Supply became effectively infinite. Bandwidth prices fell through the floor. Utilization on many networks sat near ~1–3%. They built the highway before the cars existed, and then discovered the highway itself was infinitely expandable.


### Current Day: Backlog Driven Compute


Today, the sequence is reversed:


- **Silicon is pre-sold.** Nvidia's Blackwell generation is effectively sold out for 12 months. That's not “pipeline interest”; those are firm orders from hyperscalers who are fighting each other for allocation.
- **Capacity is pre-leased.** In core US markets like Northern Virginia, vacancy rates are under 1%, and ~70%+ of capacity under construction is already pre-leased before the slab is poured. Many colo projects do not even break ground until they presign usage with credible consumers and also sign the energy leases.


In 2000, the constraint was finding a customer. In 2025, the constraint is finding a megawatt.


## Why You Can't Multiply a GPU


The telecom bubble popped because of a single technology (Dense Wavelength Division Multiplexing, DWDM) which let operators multiply network capacity without deploying more physical capital. The bottleneck disappeared overnight. Prices followed.


**There is no equivalent “free multiplier” for AI compute:**


- You cannot push a firmware update and make an H100 do 100x more FLOPs.
- **Scaling requires new wafers, new CoWoS capacity, new HBM, new racks, and new megawatts.** All of that costs real money and takes real time. There are emerging architectures—Cerebras wafers, Recogni accelerators, TPUs, and other domain-specific chips—that may meaningfully reduce compute-per-watt. But those require new deployments, not upgrades to existing silicon. None have yet demonstrated the kind of 10–100× supply-side expansion that DWDM brought to fiber.
- **Lead times on high-voltage transformers are 3+ years.** Fabs take most of a decade. Transformers, switchgear, and cooling are all very real bottlenecks. For the operators we work with at Aravolta, the new currency isn't just megawatts, it's speed to market. In today's market, the first operator to stand up a liquid-cooled, power-dense hall doesn't win a contract, they win a waiting line. That's exactly why operators are turning to platforms like ours to accelerate provisioning, automate site readiness, and reduce the time from “hardware delivered” to “revenue-generating GPUs online.”


**There is a very real scarcity floor:** In 2000, capacity could explode with a software and optics upgrade. In 2025, capacity expands on 24–36 month cycles tied to fabs, power, and grid upgrades.


You can't code your way out of a thermodynamic shortage.


## TPUs are not the only Innovation


The big question underneath all the NVIDIA discourse is this: **If scaling is slowing and demand is exploding, where does the next supply-side breakthrough come from?**


While the industry loves to fixate on Google's TPUs (especially with Gemini being trained on them), Google has more incentive to make Gemini leaps and bounds better than instead making TPUs a broadly available commercial product. That leaves a wide strategic opening.


And this is where things get interesting: behind the scenes, a wave of alternative architectures is emerging—wafer-scale engines, LPUs, reconfigurable dataflow chips, RISC-V chiplets. These companies aren't just “competitors”; they represent the only plausible path to a 10×–30× improvement in compute-per-watt. If one of them hits, the entire supply chain shifts. That's why it's worth looking at who's real and what they're actually delivering.


Company Key Innovation (2025) Performance Edge vs. Nvidia Role in AI Ecosystem Funding/Valuation


Cerebras Wafer-Scale Engine-3 (WSE-3): 900K cores, 4T transistors on a single 46x46mm wafer; 7,000x GPU memory bandwidth. 179x faster molecular sims than Frontier supercomputer; 1/6th power for inference (Llama2-70B in 1 day). Training/inference for pharma (GSK agents) & UAE's Condor Galaxy; 6 new datacenters. $1.1B raise; $8.1B val; TIME's 2024 Best Invention.


Groq Language Processing Unit (LPU v2): SRAM-centric TSP with 80 TB/s on-die bandwidth; speculative decoding for 2-4 tokens/stage. 500+ tokens/s on Mixtral 8x7B (13x ChatGPT); 10x energy efficiency for LLMs. Real-time inference (McLaren F1 analytics); GroqCloud API beats H100 latency. $750M raise; $6.9B val; $1.5B Saudi commitment.


Recogni Pareto math for mixed-precision (up to 16-bit) inference; no QAT needed for >100B param LLMs. 1 petaFLOPS Scorpio chip: 300m object detection accuracy; gen2 for datacenter scale. Edge-to-cloud inference; $102M for automotive/enterprise. $175M total; gen2 focus on LLMs.


SambaNova SN40L RDU: Reconfigurable dataflow with 1 TB/s/node; multi-core compute-storage array. Fastest Llama 3.1 405B inference; 30x request serving boost. Enterprise Suite for agentic AI; SoftBank Japan datacenters. $1B+ total; “Most Respected Private Semi” 2024.


Tenstorrent Wormhole/Blackhole: RISC-V chiplets with Metalium compiler; Ascalon CPU for SPECINT. A100 parity at lower power; scalable to Galaxy servers. Open-source licensing; Bezos/Fidelity-backed for ADAS. $700M Series D; $2.6B val.


However, Zarbot (English translation by @jukan05) said it best:


“Nvidia's success goes far beyond the GPU hardware itself. Its true moat is the entire complex, ubiquitous accelerated computing solution. As Jensen said, they excel in scientific and engineering simulation, computer graphics, structured data processing, and classical machine learning. And the ‘single architecture’ covering from cloud (training) to edge devices (inference) means developers can use one set of code and toolchains to serve all scenarios. This development efficiency and ecosystem consistency are difficult for competitors to replicate in the short term.”


No one can maintain a lead forever - though it remains to be seen who the big winners are and what the true time scale is.


What this means for the buildout also remains to be seen - will we still need liquid cooling? Move to immersion? Will the energy needs decrease significantly with chip innovations? Or will we just deploy more and more scale?


## When Would This Become a Bubble?


To be fair, this can still go wrong. But “AI mentions on earnings calls” or “Nvidia's stock multiple” are not the metrics that matter. A real AI infrastructure bubble requires at least two of these structural breaks.


Furthermore, colocation markets provide a strong signal on enterprise activity.


### North America Colocation Market (H2 2024)


Metric Value


Inventory 13.6 GW


Vacancy 2.6%


2024 Absorption 4.4 GW


2024 Completions 2.6 GW


Under Construction 6.6 GW


Planned 22.9 GW


### Colocation Rents by Contract Size (H2 2024)


Contract Size USD/kW/mo


<250 kW $318


250 kW–1 MW $201


1–5 MW $152


5–20 MW $139


>20 MW $126


### Key Primary Markets: Inventory and Vacancy (H2 2024)


Market Inventory (MW) Vacancy 2024 Net Absorption (MW)


Northern Virginia 2,930 0.5% 452


Dallas-Fort Worth ~1,200 <2% ~500


Atlanta 1,000 ~2% 706


Chicago ~800 ~3% ~300


Silicon Valley ~600 5.5% ~200


Phoenix 603 ~4% ~150


Notes: Northern Virginia remains the largest market. Atlanta led absorption in 2024. Vacancy rates are record lows across primaries (overall 1.9%). Sources: CBRE H2 2024 Report.


Across every market signal—vacancy, absorption, rents, and power availability—the story is consistent: **we are structurally short on capacity, not long.** There is no sign of oversupply anywhere in the stack.


### The Bubble Dashboard


Metric Bubble Condition (“Crash” Signal) Current Status (Late 2025)


Vacancy Rates Data center vacancy in primary hubs (e.g., NoVA) spikes above 10%. Northern Virginia vacancy is <1%. Space is virtually nonexistent.


Spot Pricing Hourly rental for H100 GPUs crashes below electricity cost (e.g., <$1.50/hr). Spot prices stabilized at $2.85 - $3.50/hr, reflecting healthy demand/supply balance.


Value Realization Companies spend billions with zero measurable revenue uplift. Revenue Expansion & OpEx Collapse confirmed in Q3 2025 earnings (see below).


**None of these conditions are here today.** All of them point in the opposite direction. If you look at the Q3 2025 earnings prints, the “Revenue Expansion” phase has officially begun. We aren't just buying chips; we are selling intelligence.


- **Cursor:** Fastest SaaS company ever to reach $1B ARR (24 months)
- **JPMorgan:** AI coding tools boosted productivity by 20% across 60,000+ technologists.
- **Walmart:** AI saved 4 million developer hours, equivalent to ~2,000 full-time work years. Route optimization eliminated 30M unnecessary miles and 94M lbs of CO₂ using AI routing.
- **Meta:** AI-powered “Advantage+” (Ad Engine) now driving a $60B+ revenue run rate, achieved both +14% ad impressions AND +10% higher ad prices
- **Adobe:** AI-related revenue exceeded $5B; Firefly AI-First ARR approaching $500M.
- **Salesforce:** Closed 5,000 Agentforce deals in just months as enterprises upgrade from old chatbots. Internal data shows the system achieving an 84% resolution rate with only 2% human escalation.
- **Duolingo:** Launched 148 new AI-generated language courses in one year, which previously was a decades-long workload. “Duolingo Max” AI tier increased ARPU by 6%, 51% user growth.
- **ZipRecruiter:** AI cut time-to-hire by up to 30% and boosted screening efficiency for 64% of recruiters.


## The Two Market Reality: Froth vs. Bedrock


We need to stop treating AI as one monolithic asset class. There are two distinct markets here, and only one of them is showing bubble behavior.


### Layer 1: The Consumer Layer


This is where skepticism makes sense. Thousands of startups raised money in 2023 to build simple interfaces around foundation models. Most of them will not survive.


- Many analysts expect 85 to 90 percent of these companies to fail within three years.
- Thin moats and high churn make this layer extremely competitive.
- These companies often depend entirely on someone else's models and infrastructure.
- The foundation model providers are moving up-stack, watching what works on their own ecosystem and then building it themselves: the Amazon Basics playbook.
- They see the usage data, they see which workflows take off, and they just… ship their own version. The playbook is simple: startups test the market, OpenAI/Anthropic/Google watches what sticks, and then ships the “Amazon Basics” version at platform scale. Rewind, Adept, Jasper and countless others learned this the hard way.


**But this layer is not just noise. It is already producing durable winners.**


- New tech giants like OpenAI and Anthropic are here to stay.
- A new generation of AI native software companies is showing real revenue and real value. Examples include Cursor, Lovable, and others that are scaling faster than early SaaS leaders.
- Just like the Dot com era, many companies will die, but a few will become generational giants.


Even the failures in this layer help the system. Every failed startup has already spent its venture dollars on GPU compute. They end up subsidizing the infrastructure layer.


### Layer 2: The Infrastructure Utility Layer


This layer is completely different. It includes the data centers, the power, the cooling, and the silicon. It behaves like a utility, not a speculative bet.


- **The spend becomes a recurring operational cost** , similar to electricity or cloud storage. That stickiness is already visible in Aravolta's own data. Once an operator instruments their power, cooling, and asset lifecycle workflows with us, they don't rip it out. They expand it. AI infrastructure behaves like a utility stack, and the operational software that sits on top behaves like one too.
- **Infrastructure providers benefit from long term, sticky usage patterns** across industries.


## National Security and the Race to Sovereign AI


If the private market slows, the government becomes the buyer of last resort for AI infrastructure. We've seen more examples of this popping up, from the Genesis Mission (likened to a modern Manhattan Project) to the massive federal mobilization of cloud resources.


It seems that talks of climate change have completely fallen off the public agenda, replaced by a rush to own the intelligence layer. Project Stargate and the recent AWS federal commitments position compute not as a commercial commodity, but as a **national security asset** .


This creates a hard demand floor that does not depend on consumer hype.


### The Sovereign “Put Option”


While VC Twitter debates whether chatbots have product-market fit, the federal government is effectively underwriting the physical buildout.


- **The $50 Billion Anchor:** AWS recently announced a $50 billion investment specifically for US government/defense regions (Top Secret, Secret, and GovCloud). This involves 1.3 Gigawatts of capacity, roughly a nuclear reactor's worth of power, dedicated solely to national security.
- **The Genesis Mission:** An Executive Order has launched a “Manhattan Project” for AI, directing the DOE and National Labs to build “scientific foundation models.” This isn't a startup looking for revenue; this is the state nationalizing the science layer of AI.


However, simply having the government write checks for power doesn't guarantee efficiency. If the US is serious about accelerating AI capacity, we don't just need more chips and more megawatts; we need the software layer that turns physical data centers into productive AI factories.


We are moving from an era of “build at all costs” to “operate with precision.” Whether the buyer is a hyperscaler or the Department of Defense, the need for truth is the same. They need to know if that hardware is being utilized effectively, or if it's burning out due to thermal stress.


**That's the layer Aravolta is building.** We provide the telemetry that turns compute into a measurable, performant investment.


## Conclusion


When you zoom out, this does not look like the Dot com era. It looks like the **start of a new industrial revolution** . It is slow, physical, expensive, and inevitable.


The real risk for the United States is not building too much.


The real risk is failing to build fast enough and letting another country become the world's cognitive superpower.


The question is not “Is this a bubble” but “How do we get more compute, more megawatts, and more useful work out of every cluster before someone else does?” That requires a new kind of operational software—the layer that helps GPU data centers extract more useful work from every megawatt. **That's exactly the layer companies like Aravolta are now building.**
