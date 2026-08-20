---
schema_version: "1.0.0"
document_id: "2673fcf6cf4f5a011c4fd25fbb29850c02452067d8b33618d6555d38e9a0751e"
company_key: "yc-sinai"
company: "SINAI"
source_id: "yc-sinai-news-import-71fd3cfc578b"
canonical_url: "https://www.sinai.com/post/ai-emissions-guide"
published_at: "2026-08-14T17:20:16.915+00:00"
first_seen_at: "2026-08-15T04:44:07.661532+00:00"
fetched_at: "2026-08-15T04:44:09.017117+00:00"
content_hash: "sha256:8dd141aaab738235f9229b165611591a871db8ba79bb3e4bb18d3bfd296045b2"
---

# AI Emissions: Where They Come From and How to Manage Them

*A practical guide to the carbon footprint of enterprise AI: what drives it, the water and waste behind the data center, how to evaluate vendors and regions, and how to track and reduce it while standards are still taking shape.*


AI is now part of how most companies operate, and it is starting to show up in their carbon inventory. The practical question is not whether AI has a footprint; it does, but how to account for your share of it and reduce it. This guide is a working playbook for both.


It covers how companies use AI, what you are actually accounting for, how to calculate your AI emissions, how to evaluate vendors and cloud regions, how to reduce what you find, and how purpose-built sustainability AI helps, with particular attention to the hard-to-abate sectors we work with.


## How companies are actually using AI


AI adoption inside large enterprises has moved from experiment to infrastructure. Teams use it to handle customer requests, write and review code, process documents, and pull insight out of data that used to sit untouched. Usage is climbing quickly, and every new use adds a small amount of compute somewhere in a data center.


In sustainability specifically, AI now does real work across the reporting lifecycle:[ingesting and structuring messy source data](https://www.sinai.com/platform/measure) ,[collecting supplier emissions at scale](https://www.sinai.com/platform/engage) ,[drafting and checking disclosures](https://www.sinai.com/platform/report) , and[prioritizing reduction projects by cost and impact](https://www.sinai.com/platform/reduce) . That is a lot of value, and it is worth being clear-eyed about what powers it.


## What you are accounting for


AI runs on physical infrastructure. Every prompt uses compute in a data center that draws electricity, uses water for cooling, and relies on hardware that is eventually replaced. For context, data centers accounted for about 1.5% of global electricity in 2024, a share projected to roughly double by 2030 ([IEA, Energy and AI](https://www.iea.org/reports/energy-and-ai/energy-demand-from-ai) ). The footprint also includes cooling water ([IEA via MSCI](https://www.msci.com/research-and-insights/blog-post/when-ai-meets-water-scarcity-data-centers-in-a-thirsty-world) ) and the electronic waste from hardware turnover ([Nature Computational Science](https://www.nature.com/articles/s43588-024-00712-6) ), though both are improving as designs become more efficient.


The global totals are not really the point for your reporting. What matters is how to attribute your own usage and act on it. The rest of this guide is about exactly that: how to calculate your AI emissions, and how to reduce them.


## The debate is real, and the trajectory is improving.


It is worth being honest about the uncertainty here. Credible experts land in very different places on AI's footprint, and the headline projections vary widely. That is not evasion; it is a sign of how fast the ground is moving: usage, hardware, and the electricity grid are all changing at once, and vendor disclosure is still thin. Underneath the debate, though, the efficiency curve is bending in a hopeful direction.


### Why the estimates disagree


Forecasts of AI's energy and water use span a wide range because they rest on assumptions still in flux: how fast adoption grows, which kinds of models dominate, how clean the grid becomes, and how much of the hardware lifecycle you count. Reasonable analysts reach different totals from the same starting data. The takeaway is not that any one number is wrong, but that single figures deserve caution, and that you are better off measuring your own usage than relying on someone else's estimate.


### The hardware and cooling are getting better.


The direction of travel is encouraging. Each hardware generation delivers more compute per watt, and cooling, which has historically taken up to 40% of a data center's electricity, is being cut sharply by new designs. The latest AI systems move to full liquid cooling that runs hotter, reduces cooling energy, and in the right climate can bring cooling water use close to zero ([NVIDIA, 2026](https://blogs.nvidia.com/blog/liquid-cooling-ai-factories/) ). Efficiency is one of the three forces the IEA identifies as shaping AI's energy demand, alongside adoption and the changing grid ([IEA, Energy and AI](https://www.iea.org/reports/energy-and-ai/energy-demand-from-ai) ).


### But efficiency alone will not do it.


Here is the honest counterweight: for now, demand is still rising faster than efficiency is offsetting it, so AI's absolute footprint keeps growing even as each unit of compute gets cleaner. That is why the improving trajectory is a reason for optimism, not a reason to stop measuring. The two tend to move together, because the cheaper and cleaner compute gets, the more of it we use. Progress on efficiency and discipline on measurement are not in tension. You need both.


## Where AI emissions actually come from


### Inference, not training, drives the ongoing footprint.


When people picture AI's footprint, they picture training: the one-time, energy-hungry process of building a model. That is the smaller part. Across a model's lifetime, the large majority of the electricity it consumes goes to inference, the everyday use of the model, commonly cited at more than 90% ([Brookings, 2026](https://www.brookings.edu/articles/global-energy-demands-within-the-ai-regulatory-landscape/) ). Per query, the numbers look tiny, a fraction of a watt-hour for a typical text prompt ([per-query estimates](https://earth911.com/home-garden/your-ai-carbon-footprint-what-every-query-really-costs/) ), but that scales with volume and rises sharply for reasoning, agentic, and image or video workloads.


### Where AI lands in your carbon inventory


For most organizations that buy AI as a service, AI use is Scope 3, Category 1, purchased goods and services under the[GHG Protocol](https://ghgprotocol.org/) . If you run your own infrastructure, the electricity is Scope 2, and the hardware you buy is Scope 3. Knowing which bucket applies is the first step to accounting for it correctly, and it sits alongside the rest of your[Scope 3 footprint](https://www.sinai.com/solutions/supply-chain) .


## How to evaluate AI vendors and cloud regions


Two levers move AI's operational footprint more than almost anything else: where the compute runs, and who you buy it from.


### Region selection is the highest-leverage decision.


The carbon intensity of electricity varies enormously by grid, so the same workload can carry a very different footprint depending on where it runs. Choosing a lower-carbon region can cut operational emissions by roughly 30% to 80% for the same work, and by an order of magnitude in the cleanest grids, without changing the workload itself ([Google Cloud region carbon data](https://cloud.google.com/sustainability/region-carbon) ). For workloads that are not latency-sensitive, default to your cleanest available region and schedule flexible jobs for cleaner times and places.


### A vendor evaluation checklist


**When you evaluate an AI or cloud vendor, ask:**


*●* ***Data availability.*** *Do they report their own emissions, and publish efficiency metrics such as energy per token or per query?*


*●* ***Accounting method.*** *Do they separate market-based from location-based figures? Location-based reflects the physical grid, which matters for an honest estimate.*


*●* ***Infrastructure efficiency.*** *What is the power usage effectiveness (PUE) and water usage effectiveness (WUE) of the data centers handling your workload, and are they moving to more efficient cooling?*


*●* ***Clean energy.*** *How are they procuring renewable energy, and how additional is it? Are they adding new capacity rather than buying certificates alone?*


*●* ***Assurance and targets.*** *Do they provide third-party assurance on their reporting, and what have they committed to on renewables, water, and removals?*


### Procurement is a lever, not just a purchase.


Making sustainability part of how you choose AI vendors rewards the providers that disclose, improve, and stay transparent. The same expectations you place on any supplier belong here too.


## How to track and reduce AI emissions while standards evolve


Here is the honest state of play: there is no single, settled global standard for AI environmental measurement yet, and very few companies currently track AI consumption in their inventory. The category is new, vendor disclosure is thin, and for most organizations AI is still a small slice of Scope 3 today. But small and fast-growing is exactly the profile of a line item that becomes material before anyone is watching for it.


### A pragmatic way to measure


If you buy AI as a service, estimate from your own consumption: total tokens or queries, multiplied by a published per-token or per-query energy estimate, multiplied by the carbon intensity of the grid where the compute runs. Only organizations that buy compute directly from data centers, such as the model providers themselves, need to measure at the infrastructure level. Whatever method you use, document your boundaries and assumptions, whether you include training, whether you include embodied hardware, and how you amortize it. A rough, well-documented estimate now is easier to defend and improve than a perfect one you never start.


### Ways to reduce what you find


*●* ***Use AI intentionally.*** *Apply it where it drives real value rather than everywhere by default.*


*●* ***Right-size the model.*** *The largest model is rarely the best fit. A lighter model that does the job saves energy and cost.*


*●* ***Prompt efficiently.*** *Getting to the answer in fewer, better-structured steps cuts compute.*


*●* ***Choose region and timing.*** *Run flexible workloads in cleaner grids and at cleaner hours.*


*●* ***Keep it in the inventory.*** *Track AI as a Scope 3 line item so it stays visible as usage grows.*


## Why this matters more for hard-to-abate sectors


Most of the companies we work with are in hard-to-abate industries: steel, cement, chemicals,[metals and mining](https://www.sinai.com/industries/industry-metals-mining) , oil and gas, and heavy[manufacturing](https://www.sinai.com/industries/industry-industrials-manufacturing) . For these teams, the AI question cuts two ways.


First, they face the largest and most complex footprints, long capital cycles, and the heaviest regulatory pressure, from CSRD and CBAM to California SB 253 and beyond ([see the regulatory landscape](https://www.sinai.com/resources/regulatory-landscape) ). Every new emissions source matters, and AI is one more line to account for cleanly rather than leave in a blind spot.


Second, AI is disproportionately useful to exactly these companies. Their data is vast and messy, and their abatement choices are expensive and capital-intensive, which is where prioritization pays off most. Modeling reduction options on a[marginal abatement cost curve](https://www.sinai.com/post/plan-cost-effective-decarbonization-with-macc) turns a long list of possible projects into a defensible sequence, and helps teams progress toward[science-based targets](https://sciencebasedtargets.org/) . The small footprint AI adds is far outweighed by the decarbonization it can accelerate, provided both are measured honestly.


## Why purpose-built sustainability AI accelerates decarbonization


There is a difference between adding a general-purpose chatbot to a workflow and building AI specifically for carbon management. Purpose-built sustainability AI is designed for the parts of the job that slow teams down: turning messy source data into audit-grade numbers, and turning those numbers into a credible plan.


At SINAI, that shows up as[AI-driven ingestion and emission factor matching in Measure](https://www.sinai.com/platform/measure) , automated[utility data capture](https://www.sinai.com/post/utility-automation-for-enterprise-emissions-inventories) , and[AI-supported prioritization of reduction opportunities in Reduce](https://www.sinai.com/platform/reduce) , with abatement potential and cost-effectiveness modeled in marginal abatement cost curves. The same discipline that should apply to measuring AI's own footprint - traceability and defensible method - is what a purpose-built platform brings to your entire program.


This is where the market is heading. Independent analysts note that core carbon accounting capabilities are converging into table stakes, with AI-enabled features and measurement quality emerging as the real differentiators ([Verdantix Green Quadrant: Enterprise Carbon Management Software, 2026](https://www.verdantix.com/venture/report/green-quadrant--enterprise-carbon-management-software-2026) ). Purpose-built AI does not replace human judgment; it removes the manual grind, so your team can spend its time on decisions.


**Want to see purpose-built sustainability AI in action? Request a demo to see how SINAI turns manual carbon work into audit-grade results.**


## FAQ


### Are AI emissions Scope 1, 2, or 3?


For most companies that use AI as a purchased service, AI emissions fall under Scope 3, Category 1, purchased goods and services. If you run your own AI infrastructure, the electricity is Scope 2, and the hardware you buy is Scope 3.


### Do training or inference cause more AI emissions?


Once a model is deployed at scale, inference can become the dominant source of its operational energy use because it runs continuously. However, the exact share depends on the model, workload, query volume, and deployment setup.


### How much water does AI use?


More than most people realize. Cooling can account for 20% to 40% of a data center's energy, and global data center water use is estimated in the hundreds of billions of liters per year and rising. A short chatbot exchange uses roughly 10 to 25 milliliters once cooling and electricity generation are counted.


### Does AI create electronic waste?


Yes. Fast hardware turnover, often every two to five years, produces electronic waste containing toxic materials. One peer-reviewed study estimates generative AI could add 1.2 to 5 million tonnes of e-waste between 2020 and 2030, though reuse and lifespan extension could cut that substantially.


### How do I measure my company's AI emissions?


For most organizations, estimate from usage: total tokens or queries multiplied by a published per-token or per-query energy estimate, multiplied by the carbon intensity of the grid where the compute runs. Document your boundaries and assumptions so the estimate is defensible and can be improved over time.


### Does choosing a cloud region really reduce emissions?


Yes, significantly. Because grid carbon intensity varies widely by location, running the same workload in a lower-carbon region can cut operational emissions by roughly 30% to 80%, and by an order of magnitude in the cleanest grids, without changing the workload.


### Is AI becoming more energy efficient?


Yes, per unit of compute. Each hardware generation delivers more work per watt, and new liquid-cooling designs cut cooling energy and water. But total AI demand is still growing faster than those efficiency gains, so the absolute footprint continues to rise, which is why measuring your own usage still matters.


### Do we have to report AI emissions today?


There is no single global standard requiring it yet, and few companies track it currently. But AI is a fast-growing part of Scope 3, so starting with a simple, well-documented estimate now makes it far easier to manage as usage and expectations grow.


‍
