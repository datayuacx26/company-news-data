---
schema_version: "1.0.0"
document_id: "76ce4c34fee4b4cd054bee6ac7861db7ff865a26482c761dff144704670bc0d5"
company_key: "yc-aravolta"
company: "Aravolta"
source_id: "yc-aravolta-news-import-85aed3e09b97"
canonical_url: "https://www.aravolta.com/blog/missing-middle"
published_at: null
first_seen_at: "2026-07-24T07:29:59.544602+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:d9b203879a559428bbee4789537adc68e126e87b9806ff38cb23323ef8f38f8d"
---

# The Missing Middle of Data Centers

Every hospital running patient records, every bank clearing transactions, every manufacturer tracking its supply chain: they all depend on a data center that nobody is writing about. While the industry conversation has fixated on gigawatt AI campuses and nine-figure GPU clusters, the **3MW–25MW facility** quietly powering the rest of the global economy has been almost entirely left out of the narrative.


This is not a story about the past. It is a story about what is actually running right now. According to Goldman Sachs, 86% of global data center power today runs non-AI workloads. The facilities serving that 86% are not being replaced. They are being ignored while the engineers, capital, and supply chain capacity they need get redirected toward hyperscale AI campuses that serve a very different set of customers.


In the final months of 2025, something changed that reframed the whole picture. Intel reported an unexpected uptick in datacenter CPU demand and increased its 2026 capex guidance. AMD stated publicly that it expects the server CPU total addressable market to grow in the **"strong double digits"** in 2026. Frontier AI labs began competing directly with cloud providers for commodity x86 servers because they were running out of CPUs to feed their Reinforcement Learning training pipelines. The chip that supposedly became irrelevant the moment ChatGPT launched is now in short supply at the most advanced AI facilities in the world.[\[1\]](https://newsletter.semianalysis.com/p/cpus-are-back-the-datacenter-cpu)


CPUs never left. The infrastructure press just stopped covering them.


86%


of global data center power runs non-AI workloads - cloud (54%) + traditional (32%) vs. AI (14%)


[Goldman Sachs Research, 2025](https://www.goldmansachs.com/insights/articles/ai-to-drive-165-increase-in-data-center-power-demand-by-2030)


~75%


of data center workloads were non-AI in 2025 - AI represented "about a quarter" of all workloads


[JLL 2026 Global Data Center Outlook](https://www.jll.com/en-us/insights/market-outlook/data-center-outlook)


38 GW


of existing non-AI data center capacity globally in 2026 - the installed base running today's enterprise compute


[Programs.com, Jan 2026](https://programs.com/resources/data-center-statistics/)


48 MW


CPU & storage building at Microsoft Fairwater - supporting a 295 MW GPU cluster next door


[SemiAnalysis, Feb 2026](https://newsletter.semianalysis.com/p/cpus-are-back-the-datacenter-cpu)


1:6


CPU-to-GPU power ratio at Fairwater - and likely to worsen with future GPU generations


70%


of enterprise workloads remain outside centralized public cloud through 2027


[Gartner](https://www.gartner.com/en/newsroom/press-releases/2023-10-30-gartner-says-50-percent-of-critical-enterprise-applications-will-reside-outside-of-centralized-public-cloud-locations-through-2027)


## The Two-Speed Industry


The global data center market was valued at approximately **$354.75 billion in 2024** and is projected to reach $1.08 trillion by 2034, growing at an 11.5% CAGR.[\[5\]](https://www.polarismarketresearch.com/industry-analysis/data-center-market) The AI data center subset that commands all the headlines represents a fraction of that base - growing faster, certainly, but building on top of an enormous installed foundation of CPU-dense, general-purpose infrastructure that isn't going anywhere.


This is a two-speed industry. On one track: the hyperscalers. Amazon has committed over **$100 billion** in capital expenditure this year. Microsoft is investing $80 billion. Meta's CapEx could reach $65 billion.[\[6\]](https://brightlio.com/data-center-stats/) These numbers are real and staggering. They also distort perception of where the majority of the world's compute actually lives.


On the other track: the thousands of **3–25 MW facilities** housing ERP systems, databases, electronic health records, financial transaction processing, email, file storage, and the entire operational fabric of the global economy. These sites don't get a booth at Supercomputing. Their operators don't ring the opening bell at NASDAQ. But they quietly process the overwhelming majority of the world's business logic - and they run on CPUs.


Table 1. Hyperscaler 2025 capital expenditure commitments Company 2025 CapEx guidance Primary use


Amazon (AWS) > $100B AI infrastructure, hyperscale DC expansion


Microsoft $80B AI training and inference infrastructure


Meta $60–65B AI research and production serving


Google / Alphabet $75B TPU/GPU clusters, cloud AI


Source:[Brightlio, 255 Data Center Statistics (2025)](https://brightlio.com/data-center-stats/)


### Case Study: Microsoft Fairwater Real-world example


Sources:[SemiAnalysis (Feb 2026)](https://newsletter.semianalysis.com/p/cpus-are-back-the-datacenter-cpu) ·[SemiAnalysis - Microsoft AI Strategy Deconstructed (Nov 2025)](https://newsletter.semianalysis.com/p/microsofts-ai-strategy-deconstructed) ·[Microsoft Source](https://news.microsoft.com/source/features/ai/from-wisconsin-to-atlanta-microsoft-connects-datacenters-to-build-its-first-ai-superfactory/)


Microsoft's "Fairwater" AI data centers for OpenAI are among the most advanced facilities ever built - 315-acre campuses housing hundreds of thousands of NVIDIA GB200 and GB300 GPUs, connected via a dedicated AI Wide Area Network spanning Wisconsin and Atlanta, designed to train the next generation of frontier models. Phase 1 came online in early 2026.


But here is the detail that doesn't make headlines: each Fairwater campus is **comprised of two buildings** . One is the famous GPU building - approximately **295 MW** of dense NVIDIA Blackwell racks operating at 140 kW per rack and 1,360 kW per row, with 72-GPU NVLink-connected NVL72 racks and 800 Gbps GPU-to-GPU backend networking. The second building is a standard **48 MW CPU and storage facility** .


That 48 MW CPU building exists solely to support the GPU cluster next door. Tens of thousands of CPUs handle data storage, sharding, and indexing for GPU training runs; image and video decode for multimodal models; and - critically - the **Reinforcement Learning environments** that now require CPUs to compile, verify, and interpret code outputs in parallel to generate training rewards, keeping the GPU cluster from sitting idle.


The CPU-to-GPU power ratio at Fairwater is approximately **1:6** . SemiAnalysis notes this ratio may actually increase with future GPU generations like Rubin, as GPU performance-per-watt improves faster than CPU performance-per-watt, requiring proportionally more CPU capacity to keep pace. The world's most advanced AI factory still needs a CPU data center.


Table 2. Microsoft Fairwater - building-level specifications Building Power Rack density Primary role


GPU building ~295 MW 140 kW/rack (NVL72) AI training - NVIDIA Blackwell GB200/GB300


CPU & storage building ~48 MW Standard enterprise density Data management, RL environments, multimodal decode


CPU:GPU power ratio 1:6 - Expected to worsen with Rubin-generation GPUs


GPU networking - 800 Gbps backend NVLink across 72-GPU NVL72 racks


Campus footprint 315 acres - Wisconsin + Atlanta, connected via AI WAN


Sources:[SemiAnalysis (Feb 2026)](https://newsletter.semianalysis.com/p/cpus-are-back-the-datacenter-cpu) ·[Data Center Dynamics (Nov 2025)](https://www.datacenterdynamics.com/en/news/microsoft-launches-atlanta-fairwater-data-center-two-stories-no-ups-or-gen-sets/)


## CPUs Are Back - And Never Really Left


In the five years leading up to ChatGPT's launch in November 2022, Intel shipped over **100 million Xeon Scalable CPUs** to cloud and enterprise data centers. That installed base didn't evaporate when AI arrived. It kept running SAP, Oracle databases, hospital EHR systems, financial trading infrastructure, and government workloads - everything that keeps organizations operational regardless of what's happening in the frontier AI race.[\[1\]](https://newsletter.semianalysis.com/p/cpus-are-back-the-datacenter-cpu)


What has changed in 2026 is that CPU demand is now accelerating from two directions simultaneously: the traditional enterprise installed base continuing its refresh cycle (with AMD's Turin generation offering socket consolidation ratios of up to 10:1, retiring fleets of aging Cascade Lake servers), and a new AI-driven demand that even the largest GPU operators cannot avoid.


The new AI demand drivers, per SemiAnalysis's February 2026 analysis, are twofold. First: **Reinforcement Learning** training loops, where CPUs run code compilation, verification, and interpretation in parallel to generate rewards for AI models - creating a bottleneck that grows with each new GPU generation. Second: the explosion of **agentic and RAG inference** , where AI agents issue API calls to databases and web services at a scale no human could approach, creating a step-change increase in CPU-served network traffic. AWS and Azure are both doing massive buildouts of their own Graviton and Cobalt ARM CPU lines, as well as purchasing additional x86 commodity servers, to serve this demand.[\[1\]](https://newsletter.semianalysis.com/p/cpus-are-back-the-datacenter-cpu)


Table 3. CPU demand drivers in 2026 - traditional vs. AI-driven Driver Category Detail


Enterprise refresh cycle Traditional AMD Turin offers up to 10:1 socket consolidation vs. aging Cascade Lake fleets


ERP / database / EHR Traditional SAP, Oracle, Epic - CPU-bound workloads with no GPU substitution path


RL training environments AI-driven (new) CPUs compile, verify, and interpret code outputs to generate GPU training rewards


Agentic / RAG inference AI-driven (new) AI agents issuing massive volumes of API calls to CPU-served databases and services


Multimodal data decode AI-driven (new) Image and video preprocessing for training - CPU-handled before GPU ingestion


Hyperscaler ARM buildouts AI-driven (new) AWS Graviton, Azure Cobalt - large-scale CPU procurement to serve inference traffic


Source:[SemiAnalysis - CPUs Are Back: The Datacenter CPU Landscape in 2026 (Feb 2026)](https://newsletter.semianalysis.com/p/cpus-are-back-the-datacenter-cpu)


## The Power Density Divergence


The clearest way to understand the gap between the hyperscale AI world and the 3–25 MW middle market is through rack power density - because the physics are simply incompatible.


Table 4. Rack power density by facility type, 2025–2026 Facility type Typical rack density Cooling method Primary workloads


Traditional enterprise (3–25 MW) 5–15 kW Air - CRAH units, hot/cold aisle containment ERP, databases, EHR, email, file storage


Retail colocation 10–30 kW Air + optional in-row cooling Mixed enterprise, some inference


Modern AI inference cluster 30–60 kW Rear-door HX or direct-to-chip liquid Production inference (H100, L40S)


Hyperscale AI training (Fairwater) 140 kW (NVL72 rack) Liquid cooling - purpose-built infrastructure Frontier model training, GB200/GB300


Sources:[Data Center Dynamics (Nov 2025)](https://www.datacenterdynamics.com/en/news/microsoft-launches-atlanta-fairwater-data-center-two-stories-no-ups-or-gen-sets/) ·[SemiAnalysis (Feb 2026)](https://newsletter.semianalysis.com/p/cpus-are-back-the-datacenter-cpu)


A facility built in 2010 for 6–8 kW per rack cannot be retrofitted into a GPU training cluster. The electrical infrastructure, cooling plant, and structural loads are simply incompatible. But crucially: it doesn't need to be. The HR software, the hospital patient database, the bank's core ledger - none of these require 140 kW racks. They require uptime, compliance, latency, and cost predictability. The problem emerges when operators feel pressure to "AI-ify" infrastructure that was never built for AI training, or when capital allocation is distorted by hype rather than workload reality.


## Two Completely Different Buildings


The gap between a CPU data center and a GPU data center is not a matter of degree. It is a matter of kind. They are designed to different physics, built to different structural specifications, served by different electrical infrastructure, and cooled by fundamentally different methods. The assumption that one can be converted into the other - or that the same operator expertise transfers cleanly - is one of the more expensive misconceptions circulating in the market right now.


Here is the single number that makes this concrete: a standard 1U CPU server draws **300–500 watts** . A 4U GPU server with eight H100s draws **6,500–7,500 watts** - roughly 15–20 times more power from a box that occupies four times the rack space.[\[19\]](https://netrality.com/blog/high-density-colocation-ai-gpu-infrastructure/) Scale that across a rack and the difference becomes structural: a typical CPU rack drawing 6–8 kW sits comfortably under a standard 30-amp circuit. A fully populated GPU training rack drawing 140 kW requires a completely different power distribution architecture - 208V or 400V three-phase feeds, 600+ amp capacity, redundant PDUs on separate feeds, and electrical infrastructure that most existing enterprise facilities simply do not have and cannot be easily retrofitted to provide.


Table 10. CPU server vs. GPU server - hardware-level power and thermal comparison Metric CPU server (enterprise, 1U) GPU server (AI, 4U - 8x H100) Multiplier


Server power draw 300–500 W 6,500–7,500 W ~15–20×


Per-chip TDP (CPU vs. GPU) 150–350 W (Xeon / EPYC) 700–1,000 W (H100 / Blackwell) ~3–6×


Typical rack power draw 5–15 kW 60–140 kW (training); 30–60 kW (inference) ~10–20× (training)


Cooling method Air - CRAH/CRAC, hot/cold aisle containment Liquid - rear-door HX, direct-to-chip, or immersion Categorically different


Power circuit requirement Single 30A circuit (208V) 100–600A, 3-phase, multiple redundant feeds ~10–20× ampacity


Floor load (structural) ~100–150 lbs/sq ft ~200–400 lbs/sq ft (liquid manifolds + servers) ~2–3×


Cooling system share of facility power ~30–35% of total facility power ~38–40% of total facility power Higher in absolute terms


Sources:[Netrality - High-Density Colocation for AI and GPU Workloads (Dec 2025)](https://netrality.com/blog/high-density-colocation-ai-gpu-infrastructure/) ·[U.S. Congressional Research Service - Data Centers and Energy Consumption (2025)](https://www.congress.gov/crs-product/R48646) ·[Hanwha Data Centers - AI Data Center Power Requirements (Dec 2025)](https://www.hanwhadatacenters.com/blog/what-are-the-power-requirements-for-ai-data-centers/)


### The Cooling Problem Is a Physics Problem


The thermal management challenge in GPU facilities is not an engineering inconvenience - it is a hard physics constraint that determines what class of building can support what class of compute. NVIDIA's Blackwell GPUs generate up to **1,000 watts per chip** , more than three times the heat output of GPUs from just seven years ago.[\[20\]](https://mlq.ai/research/data-center-cooling/) Rack densities in AI training facilities have gone from 15 kW - already at the upper edge of what well-designed air cooling can manage - to 120–140 kW. Dell'Oro Group reports that liquid cooling revenue doubled in a single year, and projects racks reaching **600 kW in the near term** , with 1 MW configurations already under consideration.[\[21\]](https://blocksandfiles.com/2025/07/14/power-consumption-and-data-centers/)


Air cooling physically cannot remove heat fast enough at these densities. The heat is too concentrated in too small a space. At 30 kW per rack, conventional CRAH-based cooling struggles and hotspots emerge. Above 40 kW, air cooling alone is functionally insufficient. Above 60 kW, it is architecturally impossible without liquid assistance. The cooling infrastructure required at 140 kW - chilled water distribution loops, coolant distribution units (CDUs), direct-to-chip cold plates welded to server components, leak detection systems, and purpose-built mechanical rooms - has almost no overlap with the raised-floor CRAC unit environment that characterizes the typical 2005–2015 enterprise data center.


Table 11. Cooling technology by rack density - capability limits and deployment context Cooling method Max rack density supported How it works Retrofit to existing facility?


CRAH / CRAC air cooling Up to ~20 kW Chilled air circulated under raised floor and through hot/cold aisles; standard in pre-2015 enterprise builds Existing standard - no changes needed for CPU workloads


In-row cooling units ~15–25 kW Cooling units placed between racks in the row; delivers cold air directly adjacent to heat sources Moderate retrofit; requires row reconfiguration and chilled water supply


Rear-door heat exchangers (RDHx) ~40–72 kW Chilled-water radiator bolts to the rear door of an existing rack; cools exhaust air before it enters the room Best retrofit option for legacy facilities entering inference - "bolt-on" if chilled water is available


Direct-to-chip liquid cooling ~80–120 kW Cold plates mounted on CPUs and GPUs carry coolant directly to the chip; heat transferred to facility water loop via CDU Significant retrofit; requires water distribution infrastructure, CDUs, leak detection - feasible in phased upgrades


Immersion cooling 100–250 kW+ Entire servers submerged in dielectric fluid tanks; highest thermal efficiency, eliminates fans entirely Major infrastructure investment; structural floor loading, fluid handling, vendor compatibility - not a retrofit for most legacy sites


Sources:[KAD - Data Center Rack Density in 2025 (Dec 2025)](https://www.kad8.com/server/data-center-rack-density-in-2025-how-high-can-it-scale/) ·[Schneider Electric - Upgrade Legacy Data Centers with RDHx (Nov 2025)](https://blog.se.com/datacenter/2025/11/11/upgrade-legacy-data-centers-for-ai-workloads-with-rdhx-liquid-cooling/)


### What Most People Don't Know About the Numbers


The infrastructure conversation around AI focuses almost entirely on compute - GPUs, chips, training runs. The physical plant implications are less discussed, and some of the underlying data points are genuinely surprising even to experienced operators.


Table 12. Data points most operators don't have in front of them Data point Figure Why it matters Source


Liquid cooling revenue growth Doubled YoY in 2025 The transition from air to liquid is happening faster than most operators have planned for; liquid cooling is no longer experimental[Dell'Oro Group, Q1 2025](https://blocksandfiles.com/2025/07/14/power-consumption-and-data-centers/)


One GPU's daily energy use ~30 kWh/day - equivalent to a 4-person home NVIDIA ships hundreds of thousands of GPUs per quarter; each one is a residential power draw running continuously at full load[Blocks & Files, Jul 2025](https://blocksandfiles.com/2025/07/14/power-consumption-and-data-centers/)


Average rack density today ~15 kW/rack industry average; AI workloads require 60–120 kW The average facility is already far below where AI workloads need to run - most of the installed base is structurally incompatible with GPU training[Dell'Oro Group, 2025](https://blocksandfiles.com/2025/07/14/power-consumption-and-data-centers/)


Cooling's share of facility power 38–40% of total data center electricity For every dollar spent on compute power, operators spend another ~40 cents just moving heat - making cooling efficiency a first-order cost driver, not a second-order one[U.S. Congressional Research Service, 2025](https://www.congress.gov/crs-product/R48646)


Half of U.S. data centers are over 10 years old ~2,500 of ~5,000 U.S. facilities The majority of the installed base was built before current density standards; retrofitting these sites - not building new - is where most of the near-term work will happen[Uptime Institute via Infinitum, Oct 2025](https://goinfinitum.com/retrofit-revolution-why-reviving-old-data-centers-is-critical-for-the-power-squeeze-and-the-environment/)


Motor/cooling upgrade energy savings ~20% reduction in total energy consumption Replacing aging CRAH fan motors with IE5-standard systems alone - without touching compute hardware - delivers material efficiency gains and PUE improvement[Infinitum, Oct 2025](https://goinfinitum.com/retrofit-revolution-why-reviving-old-data-centers-is-critical-for-the-power-squeeze-and-the-environment/)


UPS efficiency: old vs. new Legacy UPS: 80–90% efficient. Modern UPS: 95%+ A 10-point UPS efficiency gap means a 3–25 MW facility is burning 300 kW–2.5 MW on losses alone - a non-trivial operating cost that modern replacements eliminate[Data Center Dynamics, 2024](https://www.datacenterdynamics.com/en/opinions/five-reasons-to-upgrade-your-legacy-data-centers-power-infrastructure/)


Computational power per sq ft growth 5× increase between 2020 and 2025 (projected) The same footprint is expected to carry five times the compute load it was designed for - the structural and electrical assumptions of legacy builds are being stress-tested from every direction[Gartner via Data Center Knowledge, 2024](https://www.datacenterknowledge.com/data-center-infrastructure-management/retrofitting-refurbishment-and-roi-for-legacy-data-centers)


## What to Do With Legacy Infrastructure: A Practical Guide


The most common mistake operators make when confronted with the AI infrastructure narrative is treating it as a binary: either your facility is AI-ready, or it's obsolete. Neither framing is accurate. The right question is not "can this building run GPU training?" - for most existing facilities, the answer is no, and that's fine. The right question is: "what is this building actually good for, and how do I make it better at that?"


The core principle from practitioners who have done this work: start from the building, not from the workload. Understand your power envelope, your floor loading, your cooling plant, and your utility connection before committing to any specific rack environment. Operators who decide first that they want to host AI inference and then discover their substation has no available capacity have wasted substantial planning time.[\[22\]](https://www.datacenterknowledge.com/data-center-infrastructure-management/retrofitting-refurbishment-and-roi-for-legacy-data-centers)


Table 13. Legacy facility decision matrix - four upgrade paths Path Best for What you do What you don't do Capex intensity


1. Efficiency modernization Facilities serving stable CPU workloads - ERP, EHR, financial systems - with no near-term density pressure Replace legacy UPS (target 95%+ efficiency), upgrade CRAH fan motors to IE5+, seal raised floor openings, improve hot/cold aisle containment, add DCIM metering Touch the cooling plant or power distribution - too costly for the workload profile Low - operational savings often fund the upgrade within 2–4 years


2. Density upgrade for inference Retail colocation operators or enterprise facilities with available chilled water capacity wanting to capture AI inference demand Add rear-door heat exchangers (RDHx) to a dedicated zone; upgrade power distribution to that zone to 30–60 kW/rack; add CDU if chilled water is available Attempt whole-facility conversion - zone-based approach preserves existing tenants and limits disruption Moderate - bolt-on RDHx is achievable without a full plant overhaul


3. Hybrid AI hub model Operators with sufficient land and utility capacity to add new construction adjacent to existing facility Build a purpose-built liquid-cooled pod or module adjacent to the existing air-cooled facility; run complementary workloads - CPU in old building, inference or light training in new pod Try to convert the main hall to 140 kW density - it will not work structurally or economically High for new build, but existing facility remains productive throughout


4. Strategic repositioning Facilities where power constraints, floor loading, or location make GPU-path upgrades uneconomical - but where CPU-workload demand remains strong Double down on the CPU workloads the facility is actually good at - enterprise colocation, healthcare, government, edge inference - and compete on uptime, compliance, and relationships rather than density Chase hyperscale or AI training business that the building cannot physically support Lowest - the moat is operational excellence, not infrastructure replacement


Sources:[Data Center Knowledge - Bridging the Gap (Apr 2025)](https://www.datacenterknowledge.com/next-gen-data-centers/bridging-the-gap-between-legacy-infrastructure-and-ai-optimized-data-centers) ·[Schneider Electric (Nov 2025)](https://blog.se.com/datacenter/2025/11/11/upgrade-legacy-data-centers-for-ai-workloads-with-rdhx-liquid-cooling/) ·[Data Center Knowledge - Retrofitting and ROI (2024)](https://www.datacenterknowledge.com/data-center-infrastructure-management/retrofitting-refurbishment-and-roi-for-legacy-data-centers)


### The Retrofit Sequence That Actually Works


For operators who have decided to pursue a density or inference upgrade, sequence matters as much as technology selection. The following order of operations reflects what practitioners have found works - and avoids the common failure mode of committing to a rack environment before the supporting infrastructure is validated.


Table 14. Recommended retrofit sequencing for 3–25 MW facilities pursuing density upgrades Step Action What you're determining


1 Utility power audit - available capacity at the substation, interconnect queue position, upgrade lead times Whether power growth is even possible, and on what timeline. Transformer backlogs of 24–36 months make this a gating constraint.


2 Structural engineering assessment - floor loading capacity, ceiling height, column spacing Whether 200–400 lbs/sq ft liquid-cooled rack loads are supportable; minimum 12–15 ft ceiling clearance required for CDUs and overhead manifolds


3 Cooling plant assessment - chilled water availability, cooling tower capacity, outdoor heat rejection infrastructure Whether RDHx is viable (requires chilled water supply); if not, whether a standalone CDU with dry coolers is feasible in the available yard space


4 Power distribution infrastructure - available ampacity, switchgear age, PDU architecture, UPS bypass capacity Maximum achievable rack density given existing electrical infrastructure; identifies whether a zone-based upgrade is feasible without replacing main switchgear


5 Zone selection - identify a specific contiguous area (typically one row or one pod) for the density upgrade Limits scope, protects existing tenants, and creates a production test environment before committing to full-facility changes


6 Deploy, measure, and iterate - instrument the upgraded zone with real-time thermal, power, and coolant telemetry before scaling Whether the assumptions from steps 1–5 hold under actual load; catches design gaps before they become expensive problems at scale


Sources:[Data Center Knowledge (2024)](https://www.datacenterknowledge.com/data-center-infrastructure-management/retrofitting-refurbishment-and-roi-for-legacy-data-centers) ·[DLR Group - Data Center Adaptive Reuse (Apr 2025)](https://www.dlrgroup.com/idea/data-center-adaptive-reuse-strategies/) ·[Schneider Electric (Nov 2025)](https://blog.se.com/datacenter/2025/11/11/upgrade-legacy-data-centers-for-ai-workloads-with-rdhx-liquid-cooling/)


### Is There Still a Place for Legacy Air-Cooled Infrastructure?


The answer is unambiguously yes - and the market evidence supports it. The 70% of enterprise workloads that have not migrated to public cloud are not going to migrate to 140 kW GPU clusters either. The regulatory, latency, and operational reasons that kept those workloads in regional Tier III colocation facilities in 2020 are identical in 2026. Legacy air-cooled infrastructure, properly maintained and selectively upgraded, is very much a going concern.


The competitive advantage for these facilities is not density - it is operational reliability, compliance track record, and the relationships that come from decades of serving the same enterprise and healthcare customers. The 12 MW facility running hospital EHR systems in Memphis does not need to host GB200 racks. It needs 99.982% uptime, HIPAA-compliant physical access controls, a staff that picks up the phone, and power costs that don't swing wildly with commodity markets.


What legacy operators should avoid is the reactive capex trap: spending on GPU-oriented infrastructure upgrades because the trade press says AI is coming, without a signed tenant commitment or a clear understanding of whether the facility's physical envelope can actually support the workload. The facilities that will struggle are not the ones that remained confidently CPU-focused - they are the ones that half-upgraded toward GPU density, spent the capex, and then discovered the power or cooling constraints that make full deployment impossible.


## Who Lives in the Middle?


The 3–25 MW segment is populated by three main categories of operator:


Table 5. Operator categories in the 3–25 MW segment Operator type Market size / share Typical workload Source


Enterprise-owned private DC ~70% of enterprise workloads not in public cloud Oracle, SAP, EHR, financial systems, government[Gartner \[8\]](https://www.gartner.com/en/newsroom/press-releases/2023-10-30-gartner-says-50-percent-of-critical-enterprise-applications-will-reside-outside-of-centralized-public-cloud-locations-through-2027)


Retail colocation 53–70% of $69–84B colo market (2024) 10–500 kW deployments; regional insurance, healthcare, finance[Grand View Research \[9\]](https://www.grandviewresearch.com/industry-analysis/data-center-colocation-market)


Tier III colocation 56–58% of colocation market share (2024) 99.982% uptime SLA; enterprise apps, compliance workloads[Mordor Intelligence \[10\]](https://www.mordorintelligence.com/industry-reports/data-center-colocation-market)


## The Energy Picture Tells the Real Story


The Lawrence Berkeley National Laboratory's 2024 U.S. Data Center Energy Usage Report offers the clearest view of where actual compute lives. In 2023, U.S. data centers consumed approximately **176 TWh of electricity** - 4.4% of total national energy use.[\[11\]](https://eta-publications.lbl.gov/sites/default/files/2024-12/lbnl-2024-united-states-data-center-energy-usage-report_1.pdf)


Conventional CPU servers consumed around **60 TWh** of that total in 2023, up from 30 TWh in 2014 - a doubling representing the steady growth of enterprise and cloud workloads. GPU-accelerated servers grew from under 2 TWh in 2017 to over 40 TWh in 2023 - a remarkable rate, but still below the conventional server base in absolute energy terms as of the most recent reported data.[\[6\]](https://brightlio.com/data-center-stats/)


Table 6. U.S. data center electricity consumption by server type (TWh) Server type 2014 2017 2023 Growth (2014–2023)


Conventional CPU servers ~30 TWh ~38 TWh ~60 TWh +100%


GPU-accelerated servers < 1 TWh < 2 TWh > 40 TWh > 4,000%


Total U.S. DC consumption ~70 TWh ~90 TWh ~176 TWh +151%


Sources:[Lawrence Berkeley National Laboratory, 2024 U.S. Data Center Energy Usage Report](https://eta-publications.lbl.gov/sites/default/files/2024-12/lbnl-2024-united-states-data-center-energy-usage-report_1.pdf) ·[Brightlio, 2025](https://brightlio.com/data-center-stats/)


The IEA estimates AI-focused data center electricity demand is growing at approximately **30% annually** , compared to **9% for conventional server workloads** . Both are growing. The non-AI base - still predominantly CPU infrastructure - represents approximately **38 GW of existing global capacity** that isn't being decommissioned. It's being quietly refreshed, maintained, and operated by the people that no one writes about.[\[4\]](https://programs.com/resources/data-center-statistics/)


## The Capital Misallocation Risk


Capital is flowing overwhelmingly toward gigawatt-scale AI campuses, and the semiconductor supply chain has reoriented almost entirely around GPU and AI accelerator production. Yole Group reports that GPUs alone represented **$100 billion** of the $209 billion total data center semiconductor market in 2024, with NVIDIA capturing **93% of server GPU revenue** .[\[13\]](https://www.yolegroup.com/press-release/data-center-semiconductor-trends-2025-artificial-intelligence-reshapes-compute-and-memory-markets/)


Table 7. Data center semiconductor market breakdown, 2024 Category Revenue (2024) Share of total Notes


GPU / AI accelerators ~$100B ~48% NVIDIA holds 93% of server GPU revenue


CPU (x86 + ARM) ~$14B ~7% Intel + AMD; demand uptick flagged Q4 2025


Memory (HBM + DRAM) ~$60B ~29% HBM3e dominates AI server configurations


Networking / other ~$35B ~17% InfiniBand, Ethernet switching, NICs


Source:[Yole Group - Data Center Semiconductor Trends 2025](https://www.yolegroup.com/press-release/data-center-semiconductor-trends-2025-artificial-intelligence-reshapes-compute-and-memory-markets/)


Meanwhile, the supply chain for the middle market has tightened severely. High-power transformers and chillers face delivery delays of **24–40 weeks** , with transformer backlogs stretching up to 36 months.[\[14\]](https://www.marketsandmarkets.com/Market-Reports/colocation-market-1252.html) Hyperscalers have the procurement leverage and balance sheets to navigate this. The operator of a 12 MW facility in Columbus, Ohio does not. Approximately **300,000 data center positions** are projected to remain unfilled in 2025, with the most specialized engineers being pulled toward hyperscale AI projects offering 30% wage premiums - away from the middle market that needs them just as much.


## Why the Middle Market Is Structurally Resilient


Despite the attention deficit, the 3–25 MW segment has structural features that make it durable - and in many cases, preferable to hyperscale alternatives for its target workloads.


Table 8. Structural resilience factors for the 3–25 MW segment Factor What it means in practice Data point Source


Regulatory immovability HIPAA, SOX, PCI-DSS, and federal data residency requirements anchor workloads to controlled environments 50% of critical enterprise apps outside public cloud through 2027[Gartner](https://www.gartner.com/en/newsroom/press-releases/2023-10-30-gartner-says-50-percent-of-critical-enterprise-applications-will-reside-outside-of-centralized-public-cloud-locations-through-2027)


Hybrid IT pendulum Cloud repatriation driven by egress costs, compliance overhead, and latency 75% of organizations considering moving AI workloads from public cloud back to colocation[CoreSite State of DC 2024](https://www.coresite.com/blog/data-center-industry-trends-comparing-2022-2023-2024-state-of-the-data-center-reports)


Distributed AI inference Inference is delivered regionally, not from centralized training clusters 30–50 kW/rack inference achievable in modernized middle-market facilities[Grand View Research](https://www.grandviewresearch.com/industry-analysis/data-center-gpu-market-report)


Non-AI absolute growth 30% share of capacity growth on a $354B+ base is larger in dollar terms than the total AI DC market was 3 years ago $1.5T non-AI capex projected through 2030[McKinsey, 2025](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/the-cost-of-compute-a-7-trillion-dollar-race-to-scale-data-centers)


## What the Middle Market Needs


If the 3–25 MW segment is structurally resilient, it nonetheless faces real modernization pressures that operators must address over the next 3–5 years.


Table 9. Four modernization imperatives for 3–25 MW operators Imperative Timeline Detail


Power density upgrades Near-term (1–3 yr) AMD Turin's 10:1 socket consolidation pushes rack density above 6–8 kW ceilings even on standard CPU refresh cycles. Electrical infrastructure upgrades required before the current generation ages out.[\[1\]](https://newsletter.semianalysis.com/p/cpus-are-back-the-datacenter-cpu)


Cooling modernization Near-term (1–3 yr) 2005-era CRAH units are inefficient vs. rear-door heat exchangers and in-row cooling retrofits. Full liquid cooling is a longer-horizon option for sites that begin hosting inference at moderate GPU densities.


AI inference readiness Mid-term (2–4 yr) 30–50 kW inference racks are achievable in modernized existing facilities. Healthcare, finance, and manufacturing inference deployments do not require becoming a hyperscale campus.


Renewable energy & sustainability Mid-term (2–4 yr) 72% of enterprises ranked sustainability as a key site-selection factor in 2024, up from 48% two years prior. Operators without a renewable procurement strategy face a growing competitive disadvantage in RFP processes.[\[14\]](https://www.marketsandmarkets.com/Market-Reports/colocation-market-1252.html)


### The Bottom Line


The AI buildout is real. The gigawatt campuses are real. But even Microsoft's Fairwater - the most advanced AI facility ever built - requires a dedicated 48 MW CPU building just to keep its GPU cluster running. The missing middle was never actually missing. It was just missing from the conversation. Roughly 38 GW of conventional data center capacity is operating globally right now, serving the 70% of enterprise workloads that haven't moved to hyperscale cloud and won't. That infrastructure needs to be upgraded, staffed, powered, and run. The operators who focus on it don't need to out-build the hyperscalers. They need to serve the market the hyperscalers are not building for. That market is not a niche. It is the backbone.


## Sources


1. [SemiAnalysis - CPUs Are Back: The Datacenter CPU Landscape in 2026 (Gerald Wong & Dylan Patel, Feb 2026)](https://newsletter.semianalysis.com/p/cpus-are-back-the-datacenter-cpu)
2. [Goldman Sachs Research - AI to Drive 165% Increase in Data Center Power Demand by 2030 (2025) - workload mix: cloud 54%, traditional 32%, AI 14%](https://www.goldmansachs.com/insights/articles/ai-to-drive-165-increase-in-data-center-power-demand-by-2030)
3. [JLL - 2026 Global Data Center Outlook (Jan 2026) - AI represented ~25% of workloads in 2025](https://www.jll.com/en-us/insights/market-outlook/data-center-outlook)
4. [Programs.com - Data Center Statistics 2026 (Jan 2026) - non-AI workloads: 38 GW globally](https://programs.com/resources/data-center-statistics/)
5. [Polaris Market Research - Global Data Center Market, 2025–2034](https://www.polarismarketresearch.com/industry-analysis/data-center-market)
6. [Brightlio - 255 Data Center Statistics (2025)](https://brightlio.com/data-center-stats/)
7. [Data Center Dynamics - Microsoft Launches Atlanta Fairwater Data Center (Nov 2025)](https://www.datacenterdynamics.com/en/news/microsoft-launches-atlanta-fairwater-data-center-two-stories-no-ups-or-gen-sets/)
8. [Gartner - 50% of Critical Enterprise Applications Outside Public Cloud Through 2027](https://www.gartner.com/en/newsroom/press-releases/2023-10-30-gartner-says-50-percent-of-critical-enterprise-applications-will-reside-outside-of-centralized-public-cloud-locations-through-2027)
9. [Grand View Research - Data Center Colocation Market Report](https://www.grandviewresearch.com/industry-analysis/data-center-colocation-market)
10. [Mordor Intelligence - Data Center Colocation Market Size & Trends](https://www.mordorintelligence.com/industry-reports/data-center-colocation-market)
11. [Lawrence Berkeley National Laboratory - 2024 U.S. Data Center Energy Usage Report](https://eta-publications.lbl.gov/sites/default/files/2024-12/lbnl-2024-united-states-data-center-energy-usage-report_1.pdf)
12. [IEA - Energy and AI: Energy Demand from AI (April 2025)](https://www.iea.org/reports/energy-and-ai/energy-demand-from-ai)
13. [Yole Group - Data Center Semiconductor Trends 2025](https://www.yolegroup.com/press-release/data-center-semiconductor-trends-2025-artificial-intelligence-reshapes-compute-and-memory-markets/)
14. [MarketsandMarkets - Data Center Colocation Market](https://www.marketsandmarkets.com/Market-Reports/colocation-market-1252.html)
15. [CoreSite - State of the Data Center 2024](https://www.coresite.com/blog/data-center-industry-trends-comparing-2022-2023-2024-state-of-the-data-center-reports)
16. [McKinsey - The Cost of Compute: A $7 Trillion Race to Scale Data Centers (2025)](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/the-cost-of-compute-a-7-trillion-dollar-race-to-scale-data-centers)
17. [Microsoft Source - From Wisconsin to Atlanta: Microsoft's First AI Superfactory](https://news.microsoft.com/source/features/ai/from-wisconsin-to-atlanta-microsoft-connects-datacenters-to-build-its-first-ai-superfactory/)
18. [SemiAnalysis - Microsoft's AI Strategy Deconstructed: From Energy to Tokens (Nov 2025)](https://newsletter.semianalysis.com/p/microsofts-ai-strategy-deconstructed)
19. [Netrality - High-Density Colocation for AI and GPU Workloads (Dec 2025) - CPU server 300–500 W vs. GPU server 6,500–7,500 W; circuit and cooling infrastructure requirements](https://netrality.com/blog/high-density-colocation-ai-gpu-infrastructure/)
20. [MLQ.ai - AI Data Center Cooling: Vertiv, Modine, Schneider (2025) - Blackwell GPU 1,000 W/chip; rack densities 120–132 kW; Vertiv $9.5B backlog](https://mlq.ai/research/data-center-cooling/)
21. [Blocks & Files - Power Consumption and Data Centers (Jul 2025) - Dell'Oro Group: liquid cooling revenue doubled, racks nearing 600 kW; one GPU = daily energy of a 4-person home](https://blocksandfiles.com/2025/07/14/power-consumption-and-data-centers/)
22. [Data Center Knowledge - Retrofitting, Refurbishment, and the ROI for Legacy Data Centers (2024) - start from the building, not the workload; compute per sq ft projected to 5× between 2020–2025](https://www.datacenterknowledge.com/data-center-infrastructure-management/retrofitting-refurbishment-and-roi-for-legacy-data-centers)
23. [Data Center Knowledge - Bridging the Gap Between Legacy Infrastructure and AI-Optimized Data Centers (Apr 2025)](https://www.datacenterknowledge.com/next-gen-data-centers/bridging-the-gap-between-legacy-infrastructure-and-ai-optimized-data-centers)
24. [Infinitum - Retrofit Revolution: Why Reviving Old Data Centers Is Critical (Oct 2025) - ~half of U.S. DCs over 10 years old; 20% energy savings from motor upgrades](https://goinfinitum.com/retrofit-revolution-why-reviving-old-data-centers-is-critical-for-the-power-squeeze-and-the-environment/)
25. [Schneider Electric - Upgrade Legacy Data Centers for AI Workloads with RDHx (Nov 2025)](https://blog.se.com/datacenter/2025/11/11/upgrade-legacy-data-centers-for-ai-workloads-with-rdhx-liquid-cooling/)
26. [Data Center Dynamics - Five Reasons to Upgrade Legacy Data Center Power Infrastructure (2024) - legacy UPS 80–90% vs. modern 95%+](https://www.datacenterdynamics.com/en/opinions/five-reasons-to-upgrade-your-legacy-data-centers-power-infrastructure/)
27. [DLR Group - Data Center Adaptive Reuse: 5 Strategies for Existing Buildings (Apr 2025)](https://www.dlrgroup.com/idea/data-center-adaptive-reuse-strategies/)
