---
schema_version: "1.0.0"
document_id: "7f2ebcc07dad8ab2b4e05cae846812f05c9b422830536f70d989ae566d341757"
company_key: "yc-ionworks"
company: "Ionworks"
source_id: "yc-ionworks-news-import-f340ab61a31b"
canonical_url: "https://ionworks.com/blog/volta-battery-report-2025"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:30.651124+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:4a0241e3c584479934a9e743aabda302e9019ca210543601657c61b8579ade70"
---

# What the Battery Report 2025 means for simulation teams

The Volta Foundation published its[Battery Report 2025](https://voltafoundation.org/) in January. At 773 pages, compiled by 120+ battery professionals from 90+ institutions, it is the most comprehensive annual snapshot of where the battery industry stands and where it's heading.


We read the whole thing. This post pulls out the findings most relevant to battery modeling and simulation teams, and connects them to the workflow problems we work on every day at[Ionworks](https://ionworks.com/about) .


The headline numbers set the context (pp. 6, 28). Global xEV sales surged 22% to 21.6 million units in 2025, with BEV demand up 28%. BESS deployments surpassed 100 GW added in a single year, taking cumulative global capacity to 267 GW / 610 GWh. Average battery pack prices fell 9% to a record low of $108/kWh, with China LFP packs at $84/kWh.


At the same time, the industry is restructuring. Northvolt filed for bankruptcy. Powin entered Chapter 11. Gigafactory plans were cancelled across the US and Europe (KORE Power in Arizona, Gotion in Michigan, ACC in Italy, among others). Early-stage VC funding fell to $14 billion, down 50% compared to 2023, while infrastructure-stage capital surged. The report predicts at least five battery startups will pivot to monetizing patents and IP rather than raise another round.


More demand. Fewer companies. Tighter capital. The teams that survive this environment will be the ones that cover more ground with less.


## The report now treats software and data as a top-level section


The 2025 edition includes a dedicated Software/AI section (pages 456 to 484) that covers the full software stack across the battery lifecycle: R&D data infrastructure, multiscale modeling tools (from[DFN](https://ionworks.com/models/types/electrochemical/dfn) and[SPM](https://ionworks.com/models/types/electrochemical/spm) to pack-level[thermal models](https://ionworks.com/models/types/thermal) ), manufacturing analytics, BMS, field analytics, EMS, and BESS optimization.


The opening summary line is worth quoting in full:


> "Efficient battery data management emerges as a key differentiator because it is a hard requirement to enable AI in downstream processes. It turns battery data from a potential liability into a true asset and competitive advantage."


This is new. Prior editions of the Battery Report treated software as a supporting topic. The 2025 edition places it alongside cell design, manufacturing, and applications as a primary dimension of the industry. The "Battery Development Value Chain" diagram (p. 25) maps three activities at the core of battery development: Materials Identification, Modeling & Simulation, and Battery Testing.


For simulation teams, the implication is straightforward. Data management is not adjacent to modeling. It is the foundation that modeling depends on. Teams that treat test data,[parameterized models](https://ionworks.com/blog/parameter-estimation-for-battery-models) , and simulation results as structured, linked objects build a compounding advantage. Teams working from scattered files and disconnected notebooks do not.


## The bottleneck moved from test throughput to data quality


The report's R&D data section (p. 463) states this explicitly: "The constraints in 2025 shifted from purely maximising test volume to the quality and structure of underlying data."


This is a significant shift. For years, the conversation in battery R&D has centered on test throughput: more channels, faster cyclers, higher parallelism. The report argues that automated high-throughput benches now generate data at volumes that enable AI-driven analysis. The constraint moved downstream, to what happens after the data is collected.


The report introduces "data moat" language: "Teams that apply rigorous metadata and schemas to test protocols build proprietary datasets that rivals cannot easily match, establishing a lasting edge." It lays out a stack of foundational technologies for R&D data: standardized data models and schemas, high-integrity data capture across cycler formats, automated data quality and validation, semantic and contextual lab metadata (traceability, lineage), and predictive diagnostics built on top.


This maps directly to what we built[Ionworks Measure](https://ionworks.com/solutions/measure) to solve. Structured ingestion from Maccor, Neware, BioLogic, Arbin, Novonix. Every measurement linked to its cell specification and experimental context. Parameterized models in[Ionworks Train](https://ionworks.com/solutions/train) that bundle a model type, validated parameter set, and cell specification with full provenance. The report describes the infrastructure layer that battery R&D needs. For our customers, that layer already exists.


## The speed numbers are real


The report's lab automation section (p. 464) cites specific acceleration figures from major institutions:


70% reduction in overall development cycles. Material validation timelines compressed from months to days (PNNL). AI-driven material screening completed in 80 hours for tasks that traditionally take years (Microsoft). Cell design iteration cycles reduced from two weeks to a single day (LG Energy Solution). Up to 80% reduction in development costs through digital twins. Traditional battery development: 36 to 60 months. AI-boosted development: 9 to 15 months.


These are not our claims. They come from PNNL, Microsoft, LG, and Samsung. But the mechanism underneath every one of them is the same: structured data feeding parameterized models feeding[simulation sweeps](https://ionworks.com/solutions/battery-protocol-simulator) . The speed does not come from AI alone. It comes from the simulation infrastructure that makes AI useful.


The report's own distinction between Large Language Models and Large Quantitative Models (p. 481) reinforces this. LLMs handle language, workflow, and knowledge synthesis. LQMs integrate physics and chemistry-based models with scientific datasets for quantitative prediction. Both matter. But the quantitative predictions that actually inform cell design, protocol optimization, and[degradation forecasting](https://ionworks.com/blog/degradation-in-lithium-ion-batteries) require physics-based simulation grounded in real data. That is the layer[Ionworks builds](https://ionworks.com/solutions/predict) .


## Agentic AI arrived in the battery stack


The report devotes a full page (p. 479) to agentic AI in battery manufacturing and R&D. It describes multi-agent systems for electrolyte innovation (University of Bayreuth), AI agents translating battery data into actionable insights (Electra Vehicles), and YAML-based agentic workflows for automated data extraction and analysis (PDF Solutions). Separately, the report highlights AI-driven detection of[lithium plating](https://ionworks.com/blog/lithium-plating) during fast charging (p. 474) and[SEI](https://ionworks.com/blog/sei) -related degradation modeling as active application areas.


It also names Model Context Protocol (MCP), first introduced by Anthropic, as an interoperability layer for agent-to-tool communication across the battery supply chain. The pattern matters because LLMs are general reasoners, not domain specialists. An LLM that can call a physics-based simulation API and interpret the results is qualitatively more useful for engineering than one that can only discuss battery science in prose. The bottleneck for AI in R&D is not the reasoning capability. It is whether the agent has access to tools that encode real domain knowledge: which cycler conventions to apply, how to parameterize a model correctly, when a plating constraint binds. We publish[ionworks-skills](https://github.com/ionworks/ionworks-skills) , an open-source set of agent skills that encode exactly this kind of battery domain knowledge, so that AI coding agents can process cycling data, run simulations, and operate the Ionworks platform without guessing at conventions that quietly produce wrong results.


A separate page (p. 477) lists "engineering workflow orchestration" and "augmented simulations & testing" as key AI applications in product development. The three "wishes" the report identifies for battery AI: an AI-discovered golden recipe for materials, an autonomous factory, and a universal lifetime prediction model applicable to all chemistries.


The third wish is closest to our work. A universal lifetime prediction model requires parameterization infrastructure that works across chemistries, with structured data underneath and repeatable[simulation workflows](https://ionworks.com/solutions/simulation-os) on top. The report describes exactly the architecture that Ionworks was built for: agents that call simulation APIs, ingest data, run parameterization, and execute design sweeps.


Most simulation software still assumes a human at a GUI. The report's own mapping of modeling software (p. 459) shows a spectrum from "model using code" ([PyBaMM](https://ionworks.com/solutions/pybamm-for-battery-teams) , MATLAB) to "model via a user interface" (desktop applications) to "model as a service" (send cells and data, receive a built model). Ionworks occupies the space where code-level depth meets managed, API-first infrastructure. The same interface serves a human engineer in a browser and an AI agent calling the REST API.


## Chemistry diversification makes parameterization harder


The report documents a chemistry mix fragmenting faster than at any point in the last decade.


LFP is consolidating toward 50% of global EV battery demand, with 4th-generation LFP and LMFP chemistries emerging. Korean producers (LG, Samsung SDI, SK On) are pivoting from NMC-only strategies to include LFP and BESS. Sodium-ion is shipping commercially: CATL's Naxtra cells, Peak Energy's 4.75 GWh contract with Jupiter Power. Solid-state pilot lines are running at Honda, Factorial (375 Wh/kg validated with Stellantis), EVE Energy, Samsung SDI, Gotion (0.2 GWh line, 90% yield), and ProLogium (4th-generation technology).[Silicon anode](https://ionworks.com/blog/batteries-101-anodes) development is advancing at TDK, Group14 ($463M Series D), and GM+LG (LMR batteries targeting 400-mile range, production from 2028).


Every new chemistry requires fresh parameterization. Every new cell format, every new formation protocol, every shift in[electrode loading](https://ionworks.com/blog/electrode-essentials) or electrolyte composition. The report itself notes that parameterization is "extremely time and resource intensive" because of identifiability constraints: each experiment isolates a specific parameter subset, and combining experiments to determine the full set is what makes it slow.


This is the problem[Ionworks Train](https://ionworks.com/solutions/train) was built for. For standard chemistries (LFP, NMC variants, NCA), parameterization takes seconds. For novel chemistries requiring custom fitting pipelines, days instead of months. Teams that need to[optimize electrode designs](https://ionworks.com/solutions/optimize) across those chemistries get to the design sweep faster when the parameterization step is not the bottleneck. As the chemistry mix fragments, the teams that can parameterize faster cover more design space. The ones working through manual fitting pipelines fall further behind with every new material announcement.


## R&D speed is now a survival question


The consolidation numbers in the report are stark. Beyond Northvolt and Powin, the report documents insolvencies at CustomCells, Nikola, Li-Cycle, and Natron Energy. Cancelled or paused gigafactory investments across the US and Europe total tens of billions of dollars. The funding environment has shifted decisively from "scale at any cost" to "prove unit economics and contracted demand."


The report's 2026 predictions include: average battery pack prices below $100/kWh, LFP at 55% of global lithium-ion demand, BESS deployments reaching 120 GW / 360 GWh, and at least five startups pivoting to IP monetization.


For battery R&D teams, the message is clear. Physical testing alone cannot cover the design space fast enough. Simulation compresses the development cycle. Structured data makes simulation reproducible. (We built a[calculator](https://ionworks.com/roi-calculator) to quantify what that looks like for a specific team.) The combination of the two is what turns modeling from a specialist activity into an engineering process that scales across a team, whether built internally or jumpstarted through a[consulting engagement](https://ionworks.com/services/consulting) .


The Battery Report 2025 is the first edition to treat software and data infrastructure as a primary dimension of the battery industry. That reflects a shift that simulation teams have felt for years: the bottleneck is not the physics, and it is not the test equipment. It is the gap between the data teams already have and the engineering decisions they need to make faster.


The report's "data moat" framing maps directly to the six dimensions we use to assess whether a team's data infrastructure is ready for AI-assisted simulation: data capture, metadata and traceability, protocols, analysis pipelines, modeling, and knowledge. (Our[AI-readiness assessment](https://ionworks.com/ai-ready) scores your team across all six and shows where the gaps are.)


The teams closing that gap are doing it with structured data, parameterized models, and simulation workflows that run repeatably, whether a human runs them or an agent does.


---


*The full Battery Report 2025 is available from the[Volta Foundation](https://voltafoundation.org/) . It is published under a Creative Commons Attribution 4.0 International License.*


**Ready to see how structured simulation workflows change what your team can cover in a quarter?**[Book a demo](https://ionworks.com/demo)
