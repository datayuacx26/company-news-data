---
schema_version: "1.0.0"
document_id: "ce9e21ab65458f0d574f5a56570d7e5f9f2729bbeeefe65ccffe185e90a34706"
company_key: "yc-aravolta"
company: "Aravolta"
source_id: "yc-aravolta-news-import-85aed3e09b97"
canonical_url: "https://www.aravolta.com/blog/the-real-environmental-impact-of-datacenters"
published_at: null
first_seen_at: "2026-07-24T07:29:59.544602+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:49f33b3f977633d7ee3e6f2fee343d77e1e2e9343e22d8e14b5c46fd26f7fa62"
---

# The Real Environmental Impact of Data Centers

## One Burger, One Data Center


Let's start with a stat that reframed this whole topic for us.


xAI's Colossus 2 is the largest data center ever constructed. It draws roughly 1.3 to 1.4 GW at full capacity. Over 500,000 GPUs. More electricity than the city of San Diego. A typical data center runs at 5 to 10 MW ([IEA](https://www.iea.org/commentaries/what-the-data-centre-and-ai-boom-could-mean-for-the-energy-sector) ). At that scale, a single typical data center uses roughly the same amount of water as one or two holes on a golf course. Colossus is 150 times larger. It is, by a wide margin, the single most power-hungry and water-hungry computing facility on the planet. ([Introl](https://introl.com/blog/xai-colossus-2-gigawatt-expansion-555k-gpus-january-2026) )


SemiAnalysis ran a detailed water footprint comparison between Colossus 2 and an average In-N-Out Burger location. The data center's blue water footprint: roughly 346 million gallons per year. The burger joint, once you account for the water embedded in beef production: 147 million gallons. ([SemiAnalysis](https://newsletter.semianalysis.com/p/from-tokens-to-burgers-a-water-footprint) )


What Annual Water Consumption Notes


Colossus 2 (largest DC ever built) **~346 million gallons** 1.3 GW, 500K+ GPUs, more power than San Diego


Average 18-hole golf course **~200 million gallons** 150 acres of maintained turf


One In-N-Out location **~147 million gallons** Including water embedded in beef production


Average U.S. household **~120,000 gallons** ~329 gallons/day


Sources:[SemiAnalysis](https://newsletter.semianalysis.com/p/from-tokens-to-burgers-a-water-footprint) ·[Introl](https://introl.com/blog/xai-colossus-2-gigawatt-expansion-555k-gpus-january-2026) ·[IEA](https://www.iea.org/commentaries/what-the-data-centre-and-ai-boom-could-mean-for-the-energy-sector) ·[USGA](https://www.usga.org/course-care/water-resource-center/how-much-water-golf-courses-need.html) ·[EPA](https://www.epa.gov/watersense/statistics-and-facts)


The largest data center ever built, housing over $18 billion in GPUs alone, uses about 2.5x the water of one fast food restaurant and less than two average 18-hole golf courses. A single burger's water footprint (245 gallons) equals using Grok 30 times a day for 668 years.


That does not mean water consumption is a non-issue. According to the IEA, a 100 MW facility accounts for approximately 530,000 gallons of total water consumption per day, including both direct cooling and indirect water use from electricity generation ([IEA](https://www.iea.org/reports/energy-and-ai) ). Scale to 1 GW and you are consuming water at the rate of roughly 26,000 households. In water-stressed regions like Arizona, Texas, and parts of Northern Virginia, that is a real problem with real community impact.


But when you compare compute-per-gallon against industries nobody thinks twice about, the perspective shifts. Data centers start looking less like the villain and more like one of the most resource-efficient sectors in the economy.


There is also a tradeoff that most reporting misses entirely. PUE and WUE (power and water usage effectiveness) are adversarial metrics. Many facilities achieve their low PUE numbers specifically by using evaporative cooling, which trades electricity overhead for water consumption. You reduce your energy waste by literally evaporating hundreds of thousands of gallons daily. So when a facility brags about a PUE of 1.2, it is worth asking what their WUE looks like. Often, the answer is uncomfortable.


And the water story is getting better fast. Not because of environmental pressure, but because of physics. You cannot air-cool an H100 at 700W in a dense rack at reasonable cost. The thermal demands of AI accelerators are pushing the entire industry toward liquid cooling. Direct liquid cooling and immersion systems reject heat via dry coolers with near-zero water consumption. They are 50 to 1,000 times more efficient at heat transfer than air ([Lawrence Berkeley National Lab](https://datacenters.lbl.gov/liquid-cooling) ). New AI builds are specifying liquid cooling from day one. xAI itself is building a water recycling plant to cool Colossus 2 with recycled municipal wastewater, which would make it effectively a net-zero water facility.


The environmental benefit is a side effect of the thermal math.


In Finland, data center waste heat will supply 40% of district heating for 250,000 people.


## Heating a Quarter Million People with Server Exhaust


Every watt a data center consumes becomes heat. Most facilities vent it into the atmosphere. That is starting to change, and the scale of what is already happening is underappreciated.


Microsoft and energy utility Fortum are building one of the largest waste heat recovery systems in the world in Finland. Data center heat will supply roughly **40% of district heating for 250,000 people** across Espoo, Kauniainen, and Kirkkonummi, going live this year. One data center campus. A quarter million people heated with energy that would otherwise be wasted. ([Microsoft](https://news.microsoft.com/europe/2024/11/14/microsoft-and-fortum-to-provide-clean-district-heating/) )


It is not an isolated case. In Denmark, a data center provides waste heat free of charge to nearly 11,000 homes. In Paris, the Equinix PA10 facility captures enough server heat to warm 1,000 homes or keep the Olympic Aquatics Centre's pools at temperature. Amazon's Tallaght project in Ireland saved 1,100 tonnes of CO₂ in its first year alone ([Amazon](https://www.aboutamazon.eu/news/aws/how-amazon-is-recycling-heat-from-data-centres-to-warm-homes-in-dublin) ). A 100 MW facility produces roughly 100 MW of waste heat. The technology to capture it is mature. The only real barrier has been proximity, and that is changing as more facilities get purpose-built with heat recovery designed in from day one.


In northern Europe, where building heating accounts for roughly 40% of total energy consumption, this is not marginal. It is transformative. Germany's new Energy Efficiency Act mandates that new data centers must utilize at least 10% of waste heat starting July 2026, rising to 20% by 2028 ([Germany EnEfG](https://www.bmwk.de/Redaktion/EN/Artikel/Energy/energy-efficiency-act.html) ). Regulation is catching up to what the economics already support.


The shift from air cooling to liquid cooling cuts both water usage and energy waste simultaneously.


## Getting More Efficient, Fast


Global data center electricity consumption hit roughly 415 TWh in 2024 ([IEA](https://www.iea.org/reports/energy-and-ai/energy-demand-from-ai) ). About 1.5% of global electricity demand. That sounds alarming until you put it next to the rest of the ledger:


Sector % of Global Electricity What It Does


Industrial processes ~42% Manufacturing, mining, refining


Commercial buildings ~18% Offices, retail, restaurants


Air conditioning ~10% Cooling buildings globally


Data centers **~1.5%** **Commerce, healthcare, finance, AI, communications**


Bitcoin mining ~0.6% One asset class


Sources:[IEA](https://www.iea.org/reports/energy-and-ai/energy-demand-from-ai) ·[IEA: Future of Cooling](https://www.iea.org/reports/the-future-of-cooling) ·[Congressional Research Service](https://www.congress.gov/crs-product/R48646)


Data centers underpin effectively all of modern commerce, communication, healthcare, finance, and logistics. For 1.5% of the grid, you are getting a lot of civilization.


And that 1.5% is getting more efficient every year. The trajectory across the industry is moving in the right direction, and it is moving quickly.


PUE across the industry keeps dropping. A decade ago, a PUE of 2.0 was common. Today the industry average sits at 1.56 ([Uptime Institute](https://uptimeinstitute.com/resources/research-and-reports/uptime-institute-global-data-center-survey-results-2024) ), and best-in-class facilities are running at 1.1 or below. Google reports fleet-wide PUE of 1.09 ([Google](https://datacenters.google/efficiency/) ). Every point of PUE improvement means less energy wasted on cooling, lighting, and power distribution per watt of actual compute delivered. The curve is not flattening. It is still accelerating.


Chips are getting better too. Each generation of GPU and accelerator delivers more compute per watt than the last. Energy density is going up, which sounds scary, but it means the same workload that required an entire rack five years ago can run on a fraction of the hardware today. More work per chip, fewer chips per job, less total energy per unit of useful output. The direction is clear even if the absolute numbers keep climbing.


And as we covered with cooling: the shift to liquid cooling is not just eliminating water consumption. It is also improving PUE simultaneously. Air cooling systems are the biggest contributor to overhead energy in most facilities. Replace them with direct liquid cooling and you cut both water usage and energy waste at the same time. The old tradeoff between PUE and WUE (power and water usage effectiveness) is dissolving. The next generation of facilities will use less of both per compute watt.


Every digital transaction displaces physical processes with their own, often larger, environmental footprint.


## What the Compute Actually Displaces


This is the part of the environmental ledger that almost never gets counted.


The 415 TWh data centers consume does not exist in a vacuum. It replaces physical processes that have their own, often much larger, energy and carbon footprints. Remote work, which runs entirely on data center infrastructure, saves an average of **3.6 tons of CO₂ per worker per year** , a 58% reduction in work-related emissions ([Stanford/PNAS](https://www.pnas.org/doi/10.1073/pnas.2304099120) ). Multiply that across tens of millions of remote and hybrid workers and the numbers get very large very quickly. Telehealth visits replace hospital trips. Digital logistics reduce empty truck miles. AI-optimized agriculture cuts fertilizer waste. Streaming a movie consumes a fraction of the energy embedded in manufacturing, shipping, and disposing of a physical disc.


Consider a more concrete example. The U.S. banking system operates roughly 72,000 branches, each consuming energy for lighting, HVAC, commute-driven traffic, paper processing, and physical security. Every transaction that moves online, every check deposited by phone, every wire initiated from a laptop displaces a fraction of that physical infrastructure's footprint. The same logic applies to retail (e-commerce warehouses are far more energy-efficient per transaction than millions of storefronts), to media, to education, to government services. Data centers did not create demand out of thin air. They absorbed demand that previously required buildings, vehicles, paper, and fuel.


BCG estimated AI applications could cut global emissions by 5 to 10% by 2030, or 2.6 to 5.3 billion metric tons annually ([Scientific American](https://www.scientificamerican.com/article/ai-and-data-centers-could-cut-more-climate-change-causing-emissions-than/) ). You can argue about the precision of that number. You cannot argue that the displacement effect is zero. And if it is even a fraction of the estimates, it dwarfs the 415 TWh data centers consume to enable it.


The relevant question is not "how much energy do data centers use?" It is "how much energy would the world use without them?" Nobody asks that question because there is no clean counterfactual. But the directional answer is obvious: more.


Data center electricity consumption is projected to more than double by 2030.


## Now for the Honest Part


Data centers are adding massive new load to a grid that was already struggling. That is not spin. That is the reality.


Global data center electricity consumption hit roughly 415 TWh in 2024. About 1.5% of global electricity demand. That number is projected to more than double by 2030 ([Gartner](https://www.gartner.com/en/newsroom/press-releases/2025-11-17-gartner-says-electricity-demand-for-data-centers-to-grow-16-percent-in-2025-and-double-by-2030) ). And it is landing on a grid that is already maxed out. In Northern Virginia, the densest data center market on Earth, Dominion Energy has warned that new connections could face multi-year wait times. Texas ERCOT is grappling with how to accommodate tens of gigawatts of new data center load without compromising residential reliability. PJM, which manages the grid for 13 eastern states, has a queue of over 260 GW in interconnection requests, the majority tied to data centers.


The U.S. grid was not built for this. For decades, electricity consumption was roughly flat. Now it is climbing, and data centers are a major reason why.


There is also a carbon accounting credibility problem that deserves calling out. Microsoft's sustainability report showed total emissions up 23.4% since 2020 ([Microsoft](https://blogs.microsoft.com/on-the-issues/2025/05/29/environmental-sustainability-report/) ). Location-based Scope 2 more than doubled, from 4.3 million to nearly 10 million metric tons CO₂. Google's hit 11.5 million metric tons, up roughly 50% in five years ([Google](https://blog.google/outreach-initiatives/sustainability/environmental-report-2025/) ). And then Google managed to report a 12% *decrease* in data center emissions while simultaneously disclosing a 27% *increase* in electricity consumption. That math works on paper (renewable energy certificates and power purchase agreements) but not in physics. RECs are paper instruments. You buy a certificate from a wind farm in Oklahoma and claim it offsets your coal-powered Virginia facility. The electrons do not care about your accounting.


The grid is getting stressed. That is a fact. The question worth asking is what happens next.


Tech companies have committed to over 25 GW of nuclear capacity, more than a quarter of America's existing fleet.


## What Happens Next: 25 Gigawatts of Nuclear and a Grid Getting Rebuilt


The companies stressing the grid are also the ones writing the biggest checks to upgrade it.


Microsoft signed a 20-year deal to restart Three Mile Island's 835 MW reactor ([DCD](https://www.datacenterdynamics.com/en/news/three-mile-island-nuclear-power-plant-to-return-as-microsoft-signs-20-year-835mw-ai-data-center-ppa/) ). $1.6 billion from Constellation Energy. Google signed the first U.S. corporate SMR fleet deal with Kairos Power for 500 MW ([DOE](https://www.energy.gov/ne/articles/google-announces-agreement-kairos-power-deploy-smrs) ). Amazon locked in 1.9 GW of nuclear capacity through 2042. Meta signed for over 2.1 GW. Collectively, big tech has committed to **over 25 GW of data center-linked nuclear capacity** . To put that in proportion: the entire U.S. nuclear fleet, 94 reactors built over decades, totals 97 GW. Tech companies are financing the equivalent of **over a quarter of America's existing nuclear capacity** . No other private sector has come close.


Company Capacity Type Timeline


Microsoft **835 MW** Three Mile Island restart (Constellation) 20-year PPA


Google **500 MW** SMR fleet (Kairos Power) First U.S. corporate SMR deal


Amazon **1.9 GW** Nuclear portfolio Through 2042


Meta **2.1+ GW** Nuclear portfolio Multiple deals


Combined big tech **25+ GW** Nuclear + SMR Various


U.S. nuclear fleet (all 94 reactors) 97 GW Reference Built over decades


Sources:[DCD](https://www.datacenterdynamics.com/en/news/three-mile-island-nuclear-power-plant-to-return-as-microsoft-signs-20-year-835mw-ai-data-center-ppa/) ·[DOE](https://www.energy.gov/ne/articles/google-announces-agreement-kairos-power-deploy-smrs) ·[EIA](https://www.eia.gov/todayinenergy/detail.php?id=65104)


These are not renewable energy certificates. These are physical power plants being built or restarted because a creditworthy buyer showed up with a 20-year contract. Nuclear had been stuck for decades. Everyone agreed it was the best path to clean baseload. Nobody could finance it. The capital risk was too high, the permitting timeline too long, the political will too thin. Data center operators broke the logjam. They are the first customer in history with the combination of scale, credit quality, and demand certainty needed to de-risk a nuclear investment. Without data center demand, Three Mile Island stays shut. Without data center demand, SMRs stay in the pilot phase.


And it is not just nuclear. In 2025 alone, tech companies signed **14 geothermal PPAs totaling 635 MW** , triple the volume of 2024. Fervo Energy closed a $462 million Series E to build enhanced geothermal in Utah ([Fervo](https://fervoenergy.com/) ). Google signed a 150 MW deal with Ormat ([Google](https://blog.google/outreach-initiatives/sustainability/google-geothermal-energy/) ). Enhanced geothermal runs 24/7, produces near-zero emissions, has a tiny land footprint. An entirely new clean energy category is being pulled into commercial viability because data centers need power and are willing to pay for it.


Data centers are also uniquely suited to absorb stranded renewable energy. An estimated 30 to 40% of renewable generation currently goes unused ([Soluna/DCK](https://www.datacenterknowledge.com/build-design/soluna-ceo-on-stranded-renewables-ai-data-centers-and-grid-revolution) ). Transmission bottlenecks, limited local demand, no storage. Wind farms curtail output. Solar generates power nobody consumes. This is arguably the central unsolved problem of the energy transition: we can build renewables, but we cannot always use what they generate.


Compute, unlike manufacturing, is interruptible. You can pause a training run, shift a batch job, reschedule a rendering queue. An aluminum smelter cannot shed load without destroying product. A data center can shed or absorb hundreds of megawatts within minutes.


This is not theoretical. Google already participates in demand response programs with utilities including Indiana Michigan Power, Tennessee Valley Authority, and Omaha Public Power District. During grid events, Google receives advance notice and activates an algorithm that generates hour-by-hour instructions for specific facilities to defer non-urgent compute, including machine learning workloads, rescheduling them after the event passes or rerouting them to data centers on different grids entirely. The data center becomes, functionally, a giant battery that the grid can call on. ([Google Cloud](https://cloud.google.com/blog/products/infrastructure/using-demand-response-to-reduce-data-center-power-consumption) )


Companies like Soluna have built their entire model around co-locating at renewable sites and consuming power that would otherwise be curtailed. UChicago research on "Zero-Carbon Cloud" computing found that excess renewable supply is available roughly 80% of the time, enough to run delay-tolerant workloads almost entirely on energy that was being wasted ([UChicago](https://www1.cs.uchicago.edu/news/breaking-through-greener-cloud-using-stranded-power) ). Duke research showed that spatial workload shifting (moving compute to where clean power is abundant) reduces solar curtailment by up to 61%.


Data centers are not just a load on the grid. They are a balancing mechanism. A grid with large, flexible data center demand can absorb more variable renewable generation without curtailment, reducing the need for natural gas peakers. As more operators adopt this approach, data centers start looking less like a drain on the energy system and more like the demand-side infrastructure that makes high renewable penetration actually work.


The grid is getting stressed. But it is also getting rebuilt. And the companies doing the stressing are writing the checks.


Seven major tech companies signed the White House Ratepayer Protection Pledge in March 2026.


## Making Them Pay Their Fair Share


Private investment is necessary but not sufficient. The government is starting to make sure that data center growth does not come at everyone else's expense. And this is the piece that actually matters.


On March 5, 2026, Amazon, Google, Meta, Microsoft, OpenAI, Oracle, and xAI signed the White House "Ratepayer Protection Pledge" ([White House](https://www.whitehouse.gov/fact-sheets/2026/03/fact-sheet-president-donald-j-trump-advances-energy-affordability-with-the-ratepayer-protection-pledge/) ). The commitments are specific: pay the full cost of all new power generation required for their data centers, fund all grid upgrades, negotiate separate utility rate structures, and make backup generation available to communities during grid emergencies. The explicit guarantee: **residential electricity prices will not increase because of data center buildout** .


Read that again. These companies are volunteering to be the grid's anchor tenant, absorbing infrastructure costs that would otherwise be socialized across all ratepayers. Try to think of another industry that has ever made a comparable commitment.


Germany is pushing from the other direction with its Energy Efficiency Act: mandating waste heat utilization, setting PUE thresholds, requiring public reporting of energy performance. The EU is moving toward similar frameworks. The pattern is clear. Governments are drawing lines around what data center operators owe to the communities and grids they depend on. And the industry, so far, is meeting those requirements rather than fighting them.


This is what turns private investment into public benefit. Without policy, companies optimize for their own bottom line. With the right policy, the same investment that serves their data center demand also upgrades grid infrastructure, builds clean generation, and protects ratepayers. Whether it holds, and whether similar frameworks scale globally, is going to determine a lot about how this story ends.


Whether data centers end up as a net positive or negative for the environment is genuinely in the balance.


## The Next Five Years Will Decide Everything


Here is where we stand. Data centers are adding real strain to real infrastructure. They consume meaningful amounts of electricity and water. Location-based carbon emissions are climbing at every major operator, no matter what the market-based reports say.


On the other side of the ledger: 25 GW of nuclear being financed. 635 MW of geothermal in a single year. A quarter million people being heated with waste energy. A technology trajectory pointing toward near-zero water consumption. Workload consolidation delivering order-of-magnitude efficiency gains. Binding commitments to cover the full cost of grid impact.


Whether data centers end up as a net positive or net negative for the environment is genuinely in the balance right now. It comes down to two things: policy and energy buildout. Do the nuclear plants actually get built? Does liquid cooling outpace new evaporative deployments in water-stressed regions? Do governments keep pushing operators toward full-cost accountability? Do the pledges hold and scale? The inputs are all there. The outcome is not guaranteed.


From where we sit, we believe data centers will be a net positive. Not because the industry is inherently virtuous. Because data centers are acting as a **forcing function** . They are creating demand for clean energy at a scale that justifies investment no other buyer could underwrite. They are pulling nuclear out of regulatory limbo. They are making geothermal commercially viable for the first time. They are giving grid operators the economic justification to build transmission and generation capacity that benefits everyone, not just the data centers themselves.


The grid is going to get built out. It is going to get greener. And a meaningful share of the reason will be that data center demand forced the issue. That forcing function, messy and imperfect as it is, will provide a better energy infrastructure for everyone. Not because anyone planned it that way. Because the economics and the physics leave no other path.


### Want Real-Time Visibility Into Your Facility's Efficiency?


Aravolta's DCIM platform gives operators the telemetry they need to optimize PUE, track power consumption, and make data-driven sustainability decisions across their fleet.


### Sources


#### Water Consumption & Cooling


- [SemiAnalysis: From Tokens to Burgers - Water footprint comparison, 346M gallons/year, 668-year burger equivalency](https://newsletter.semianalysis.com/p/from-tokens-to-burgers-a-water-footprint)
- [Introl: xAI Colossus 2 Gigawatt Expansion - 555,000 GPUs, city-scale power comparisons](https://introl.com/blog/xai-colossus-2-gigawatt-expansion-555k-gpus-january-2026)
- [EESI: Data Centers and Water Consumption](https://www.eesi.org/articles/view/data-centers-and-water-consumption)
- [IEA: Energy and AI - 530,000 gallons/day per 100 MW (direct + indirect water)](https://www.iea.org/reports/energy-and-ai)
- [Lawrence Berkeley National Lab: Liquid Cooling - 50-1,000x efficiency vs air](https://datacenters.lbl.gov/liquid-cooling)
- [Vertiv: Liquid and Immersion Cooling Options](https://www.vertiv.com/en-us/solutions/learn-about/liquid-cooling-options-for-data-centers/)
- [SiliconAngle: xAI Will Spend $18B+ on NVIDIA Chips for Colossus 2](https://siliconangle.com/2025/10/06/xai-will-reportedly-spend-18b-nvidia-chips-colossus-2/)


#### Waste Heat Recovery


- [Microsoft/Fortum Finland District Heating - 40% of heating for 250,000 people](https://news.microsoft.com/europe/2024/11/14/microsoft-and-fortum-to-provide-clean-district-heating/)
- [Amazon: Tallaght Waste Heat Project - 1,100 tonnes CO₂ saved in year one](https://www.aboutamazon.eu/news/aws/how-amazon-is-recycling-heat-from-data-centres-to-warm-homes-in-dublin)
- [Germany Energy Efficiency Act (EnEfG) - 10% waste heat mandate (July 2026), 20% by 2028](https://www.bmwk.de/Redaktion/EN/Artikel/Energy/energy-efficiency-act.html)


#### Power Consumption & Efficiency


- [IEA: Energy Demand from AI - 415 TWh baseline, 945 TWh 2030 projection](https://www.iea.org/reports/energy-and-ai/energy-demand-from-ai)
- [IEA: The Future of Cooling - Air conditioning at ~10% of global electricity](https://www.iea.org/reports/the-future-of-cooling)
- [Gartner: Electricity Demand for Data Centers to Double by 2030](https://www.gartner.com/en/newsroom/press-releases/2025-11-17-gartner-says-electricity-demand-for-data-centers-to-grow-16-percent-in-2025-and-double-by-2030)
- [Uptime Institute: Global Data Center Survey 2024 - Industry PUE of 1.56](https://uptimeinstitute.com/resources/research-and-reports/uptime-institute-global-data-center-survey-results-2024)
- [Congressional Research Service: Data Centers and Their Energy Consumption](https://www.congress.gov/crs-product/R48646)
- [Google Data Centers: Efficiency - Fleet-wide PUE of 1.09](https://datacenters.google/efficiency/)


#### Carbon Emissions & Sustainability Reports


- [Microsoft Environmental Sustainability Report 2025 - 23.4% emissions increase, Scope 2 doubling](https://blogs.microsoft.com/on-the-issues/2025/05/29/environmental-sustainability-report/)
- [Google Environmental Report 2025 - 11.5M metric tons, 12% decrease vs 27% consumption increase](https://blog.google/outreach-initiatives/sustainability/environmental-report-2025/)
- [IEEE Spectrum: Data Center Sustainability Metrics - Market-based vs location-based accounting](https://spectrum.ieee.org/data-center-sustainability-metrics)


#### Nuclear & Geothermal


- [DCD: Three Mile Island Nuclear PPA - 835 MW, $1.6B investment](https://www.datacenterdynamics.com/en/news/three-mile-island-nuclear-power-plant-to-return-as-microsoft-signs-20-year-835mw-ai-data-center-ppa/)
- [Kairos Power: Google SMR Deal - First U.S. corporate SMR agreement, 500 MW](https://www.energy.gov/ne/articles/google-announces-agreement-kairos-power-deploy-smrs)
- [EIA: U.S. Nuclear Fleet Capacity - 94 reactors, 97 GW total](https://www.eia.gov/todayinenergy/detail.php?id=65104)
- [Fervo Energy: $462M Series E for Enhanced Geothermal in Utah](https://fervoenergy.com/)
- [Google: 150 MW Ormat Geothermal Deal](https://blog.google/outreach-initiatives/sustainability/google-geothermal-energy/)


#### Grid Flexibility & Demand Response


- [Google Cloud: Using Demand Response to Reduce Data Center Power Consumption](https://cloud.google.com/blog/products/infrastructure/using-demand-response-to-reduce-data-center-power-consumption)
- [Google: Making Data Centers More Flexible to Benefit Power Grids](https://blog.google/inside-google/infrastructure/how-were-making-data-centers-more-flexible-to-benefit-power-grids/)
- [UChicago: Zero-Carbon Cloud - 80% renewable availability finding](https://www1.cs.uchicago.edu/news/breaking-through-greener-cloud-using-stranded-power)
- [Yale Clean Energy Forum: From Grid Strain to Grid Gain](https://cleanenergyforum.yale.edu/2025/11/12/from-grid-strain-to-grid-gain-how-data-centers-are-rewiring-the-energy-market)
- [Soluna: Stranded Renewables and AI Data Centers - 30-40% curtailment estimate](https://www.datacenterknowledge.com/build-design/soluna-ceo-on-stranded-renewables-ai-data-centers-and-grid-revolution)
- [Inside Climate News: Stranded Renewable Energy and Data Centers](https://insideclimatenews.org/news/19062025/inside-clean-energy-stranded-renewable-data-centers/)


#### Policy & Ratepayer Protection


- [White House: Ratepayer Protection Pledge - Full text](https://www.whitehouse.gov/articles/2026/03/ratepayer-protection-pledge/)
- [White House Fact Sheet: Ratepayer Protection Pledge Details](https://www.whitehouse.gov/fact-sheets/2026/03/fact-sheet-president-donald-j-trump-advances-energy-affordability-with-the-ratepayer-protection-pledge/)
- [The Hill: Trump Signs Agreement with Big Tech](https://thehill.com/policy/technology/5767824-trump-signs-agreement-with-big-tech-to-cover-data-center-electricity-costs/)


#### Displacement Effects


- [Stanford: Remote Work Carbon Savings - 3.6 tons CO₂/year, 58% reduction](https://www.pnas.org/doi/10.1073/pnas.2304099120)
- [Scientific American: AI Could Cut More Emissions Than It Creates - BCG analysis](https://www.scientificamerican.com/article/ai-and-data-centers-could-cut-more-climate-change-causing-emissions-than/)
- [Nature Sustainability: Net-Zero Pathways for AI Servers - 73% carbon and 86% water reduction](https://www.nature.com/articles/s41893-025-01681-y)
