---
schema_version: "1.0.0"
document_id: "68f6d965d8c101d94898dca12ff5c79c11587e179f2f856e3f12ad0f11909555"
company_key: "thornburg-income-builder-opportunities-trust-common-stock"
company: "Thornburg Income Builder Opportunities Trust"
source_id: "thornburg-income-builder-opportunities-trust-common-stock-news-import-db74a70386e8"
canonical_url: "https://www.thornburg.com/article/from-grid-to-gpu-tracing-ais-power-bottleneck-through-the-building/"
published_at: "2026-08-06T17:42:31+00:00"
first_seen_at: "2026-08-07T13:53:42.700152+00:00"
fetched_at: "2026-08-07T13:53:43.989466+00:00"
content_hash: "sha256:321d5375e0fa03c12959e1c0919acfbbc38642b28f82991e0ebb3dbd7f99d3db"
---

# From Grid to GPU: Tracing AI's Power Bottleneck Through the Building

Global Equity


# From Grid to GPU: Tracing AI’s Power Bottleneck Through the Building


[Baadal Chaudhary, CFA Equity Research Analyst](https://www.thornburg.com/people/baadal-chaudhary/)


-
-
-
-
-
-


28 Jul 2026


6 min read


Why is AI data center power constrained beyond the grid? Explore how rack density, high-voltage DC distribution, and liquid cooling are reshaping AI infrastructure investing.


The market has reached a working consensus on AI power: there is not enough of it, and closing that gap will take years. Interconnection queues are stretching three to ten years, transformer lead times have doubled since 2021, and a global scramble for generation capacity has made the grid-level bottleneck impossible to ignore. What remains less settled is what happens after electricity crosses the substation — and why that question may matter as much to investors as the grid debate itself.


#### **Watts and Wafers**


At GTC 2026, Jensen Huang described AI as a five-layer cake: energy at the foundation, then chips, infrastructure, models, and applications at the top. His framing was deliberate: “Energy is the first principle of AI infrastructure and the binding constraint on how much intelligence the system can produce.” Every token generated, every inference served, every model trained traces back to electrons moving through physical systems.


We think about this tension as “Watts and Wafers.” The wafers, aka the chips, accelerators, and memory, have scaled at a pace that has repeatedly surprised even the most informed investors. However, the watts have not kept up. Not because the demand wasn’t foreseeable, but because the physical systems that deliver, convert, and manage power operate on timelines measured in years, not quarters. Transformers take two to five years to procure. Switchgear queues can stretch up to three years. Grid interconnection in Northern Virginia currently runs seven years. Simply put, the wafers keep getting faster, but the watts keep running out of juice.


The innovation gap is at the core of the AI power investment debate. So far though, the debate has concentrated on the grid. The more granular question is what happens to power once it crosses the substation? We think that is what will power the next chapter.


#### **The Power Chain Below the Grid**


A data center is not simply a building that consumes electricity. It is a precision power conversion and distribution system. Power arrives at medium voltage from the grid, steps down through transformers, flows through switchgear that routes and protects it, passes through (Uninterruptible Power Supply) systems that condition it and provide backup, and then travels through power distribution units before reaching the rack. Each handoff involves conversion losses, lead times, and engineering complexity.


The industry organizes this chain into two domains: ‘Grey Space’ and ‘White Space.’ Grey space houses the mechanical and electrical backbone — transformers, switchgear, UPS systems, generators, and chillers. White space is the IT room itself, where compute, storage, and networking live. That boundary, which has historically been stable, is now disappearing.


##### **Data Center Infrastructure Cost Mix Per MW**


##### **Cost Per MW By Category**


The is rack power density: as AI racks absorb more power per square foot than a city block, the rack itself is becoming the unit of infrastructure constraint — not the grid. A traditional server rack drew 5 to 15 kilowatts. The H100, launched in 2022, pushed AI racks to 60 kilowatts. The GB200, launched in 2024, runs at 100 to 137 kilowatts. Vera Rubin (NVIDIA’s next-generation AI accelerator platform), arriving in 2026, is projected at 200 to 300 kilowatts per rack. Rubin Ultra, slated for 2026, exceeds 600 kilowatts. Average deployed rack density across the industry hit 27 kilowatts in 2026, up from 7 kilowatts in 2021.


Air cooling fails above 40-50 kilowatts per rack. At one megawatt per rack and traditional low-voltage distribution, delivering that power would require roughly 200 kilograms of copper busbars per rack, which is physically impractical. These are not analyst projections — each generation carries confirmed shipment roadmaps with orders already placed well in advance of deployment. The building stack was not designed for this. It is being redesigned in real time — rethinking how power is converted, how heat is removed, and how much water and energy a facility consumes in the process.


##### **Power Capacity Per Rack (KW) Across Various Generations of Nvidia Platforms vs. Traditional Server Rack**


#### **Two Transitions Reshaping the Stack**


Two architectural shifts are reshaping how power flows within the building.


The first is the move to 800-volt high-voltage direct current (DC) distribution. Conventional data centers distribute power as alternating current, converting it multiple times before it reaches the chip, with each conversion losing energy as heat. This is the same principle that causes a phone charger to warm up while plugged into an outlet. High-voltage DC eliminates several of those conversion stages, sending power at a higher voltage directly to the rack, much like a highway moves more traffic than a series of local roads. At 800 volts, current requirements drop by roughly fifteen times compared to traditional low-voltage systems, reducing the copper required by over 40% and lifting end-to-end efficiency to 92 to 95% versus 75 to 85% for conventional architectures. The transition is real and underway, but it is not a problem the industry has fully solved. The adoption timeline remains contested, with small-volume shipments beginning in late 2026 and meaningful revenue contributions debated between 2027 and 2028. The winning architecture is also an open question, with NVIDIA pushing native 800V while hyperscalers, including Meta and Google, favor a bipolar ±400V approach.


The second shift is liquid cooling. Above 50 kilowatts per rack, air cooling is physically insufficient — moving enough air to absorb the heat would require more floor space than the compute itself. Liquid cooling uses water or a dielectric fluid to absorb heat directly at the chip, dramatically reducing facility energy overhead. Standard coolant distribution units now carry lead times of 8 to 12 weeks following aggressive capacity expansions. High-end units certified for NVIDIA and Google deployments remain constrained at roughly one year.


These transitions are unfolding against a backdrop of severe grid access constraints. Utility interconnection studies alone take two to four years, queue positions are non-transferable, and there is no fast-track mechanism for even the largest hyperscaler projects. That timeline mismatch has accelerated the adoption of behind-the-meter power generation — on-site gas turbines, fuel cells, and battery storage — as a structural workaround rather than a temporary bridge. Roughly one-third of all planned U.S. data center capacity is now expected to incorporate on-site generation. This does not simplify the building-level electrical stack; it intensifies it. On-site generation connects directly to high-voltage DC buses, compressing the conversion chain and making the quality of internal electrical infrastructure the determining factor in system efficiency.


#### **What the Supply Chain Is Telling Us**


Lead times are a useful signal of where stress is truly concentrated. High-voltage transformers inside the building now carry lead times of two to five years, up from approximately 50 weeks in 2021, and continue to lengthen. Lead times for medium- and high-voltage switchgear run from one to three years and continue to worsen. Standard UPS systems and coolant distribution units have largely normalized as vendors expanded capacity aggressively through 2025. The bifurcation is the point: electrical infrastructure is the persistent constraint, and mechanical and cooling have largely caught up.


Company backlogs confirm the same picture. Eaton, an XYZ company, reported data center orders up 240% in the Americas and total backlog up 31% year over year. Management at Eaton and its peers, Vertiv and Schneider Electric, have each noted that their addressable content per megawatt is approaching double traditional levels for AI deployments. This is driven by the complexity of power management at densities the industry had not previously designed for.


##### **Equipment Lead Times: Electrical Infrastructure vs. Cooling (weeks)**


#### **Where Things Get Complicated**


There are two complications worth tracking. First, regulatory friction is becoming a structural constraint on the pace of buildout. At least 13 U.S. states have introduced moratorium legislation, with roughly 34 gigawatts of capacity now classified as stranded or delayed. Virginia took it one step further, recently passing a per-kilowatt-hour electricity tax specifically targeting AI data center consumption. These developments affect geography and timing, not the underlying infrastructure thesis, but they are material to which markets are built and on what schedule.


Second, the 800V transition reshuffles value within the stack. As distribution architecture moves upstream toward higher voltages, certain portions of both ‘grey space’ and ‘white space’ may structurally lose relevance. Not every incumbent in the electrical chain benefits equally from the transition, and the question of who captures incremental content per megawatt is still being answered.


#### **Next Steps: Follow the Power**


Nvidia’s Huang put energy at the base of his five-layer cake for a reason: every layer above it depends on what happens below. The grid debate has made that visible at the macro level. What is less visible — and where we think the more differentiated investment work lies — is the chain of conversion, conditioning, and thermal management that stands between the substation and the chip. That chain has its own lead times, its own architectural transitions, and its own evolving winners. As the AI buildout matures, the analytical debate will keep moving in the same direction the power does: **down the stack, and further inside the building** .


## **Important Information**


The views expressed are subject to change and do not necessarily reflect the views of Thornburg Investment Management, Inc. This document is for informational purposes only and does not constitute a recommendation or investment advice and is not intended to predict the performance of any investment or market. It should not be construed as advice as to the investing in or the buying or selling of securities, or as an activity in furtherance of a trade in securities.


This is not a solicitation or offer for any product or service or an offer or solicitation for the purchase or sale of any financial instrument, product or service sponsored by Thornburg or its affiliates. Nor is it a complete analysis of every material fact concerning any market, industry, or investment. Data has been obtained from sources considered reliable, but Thornburg makes no representations as to the completeness or accuracy of such information and has no obligation to provide updates or changes. Thornburg does not accept any responsibility and cannot be held liable for any person's use of or reliance on the information and opinions contained herein. The views expressed herein may change at any time after the date of this publication. There is no guarantee that any projection, forecast or opinion in this material will be realized.


Investments carry risks, including possible loss of principal.


## Discover more about:


### [Economy](https://www.thornburg.com/insights/?tag=economy#results)


### [Global Equities](https://www.thornburg.com/insights/?tag=global-equities#results)


### [Investment Tactics](https://www.thornburg.com/insights/?tag=investment-tactics#results)


### [Markets & Economy](https://www.thornburg.com/insights/?tag=markets-economy#results)


#### Stay Connected


Subscribe now to stay up-to-date with Thornburg’s news and insights.


[Subscribe](https://www.thornburg.com/subscribe/)


#### Related Content


Global Equity


[Looking Beyond the Leaders: A Mid-Year Perspective on Global Equities](https://www.thornburg.com/article/looking-beyond-the-leaders-a-mid-year-perspective-on-global-equities/)


Global Equity


[Considering the Cost of Future Growth](https://www.thornburg.com/article/considering-the-cost-of-future-growth/)


Global Equity


[Equity Income Builder Fund Update: Staying Grounded in Fundamentals](https://www.thornburg.com/article/equity-income-builder-fund-update-staying-grounded-in-fundamentals/)


### More Insights


[Thornburg Income Builder Opportunities Trust Announces Distribution](https://www.thornburg.com/thornburg-income-builder-opportunities-trust-announces-distribution/) Thornburg Income Builder Opportunities Trust (NASDAQ: TBLD) announced its monthly distribution.


3 Aug 2026


4 min read


[READ](https://www.thornburg.com/thornburg-income-builder-opportunities-trust-announces-distribution/)


[Markets & Economy](https://www.thornburg.com/insights/?eyebrow=markets-economy#results)[Looking Beyond the Leaders: A Mid-Year Perspective on Global Equities](https://www.thornburg.com/article/looking-beyond-the-leaders-a-mid-year-perspective-on-global-equities/) Despite geopolitical uncertainty and market volatility, global equities continue to offer opportunities beyond the narrow group of stocks driving index returns.


[By Matt Burdett](https://www.thornburg.com/people/matt-burdett/)


22 Jul 2026


9 min read


[READ](https://www.thornburg.com/article/looking-beyond-the-leaders-a-mid-year-perspective-on-global-equities/)


[Markets & Economy](https://www.thornburg.com/insights/?eyebrow=markets-economy#results)[Considering the Cost of Future Growth](https://www.thornburg.com/article/considering-the-cost-of-future-growth/) A historic second-quarter rally pushed U.S. equities further above historical valuation norms, making future returns increasingly dependent on earnings growth.


[By Adam Sparkman, CFA](https://www.thornburg.com/people/adam-sparkman/)


17 Jul 2026


2 min read


[READ](https://www.thornburg.com/article/considering-the-cost-of-future-growth/)


[Markets & Economy](https://www.thornburg.com/insights/?eyebrow=markets-economy#results)[What’s Driving Equity Returns?](https://www.thornburg.com/article/whats-driving-equity-returns-ucits/) The Equity Income Builder UCITS Fund has generated strong long-term results with less reliance on broad market multiple expansion.


[By Adam Sparkman, CFA](https://www.thornburg.com/people/adam-sparkman/)


15 Jul 2026


4 min read


[READ](https://www.thornburg.com/article/whats-driving-equity-returns-ucits/)


[Thornburg Income Builder Opportunities Trust Announces Distribution](https://www.thornburg.com/thornburg-income-builder-opportunities-trust-announces-distribution-2026-07-01/) Thornburg Income Builder Opportunities Trust (NASDAQ: TBLD) announced its monthly distribution.


1 Jul 2026


4 min watch


[READ](https://www.thornburg.com/thornburg-income-builder-opportunities-trust-announces-distribution-2026-07-01/)


[Fixed Income](https://www.thornburg.com/insights/?eyebrow=fixed-income#results)[Don’t Wait for the All Clear](https://www.thornburg.com/article/dont-wait-for-the-all-clear/) Elevated yields, shifting macro dynamics, and nuances in the credit market are making bonds worth a second look. For investors sitting in cash or money market instruments, now could be the time to put that money to work in fixed income.


[By Thornburg Investment Management](https://www.thornburg.com/people/thornburg-investment-management/)


17 Jun 2026


4 min read


[READ](https://www.thornburg.com/article/dont-wait-for-the-all-clear/)


## Our insights. Your inbox.


Sign up to receive timely market commentary and perspectives from our financial experts delivered to your inbox weekly.


[Subscribe](https://www.thornburg.com/my-dashboard/subscriptions)
