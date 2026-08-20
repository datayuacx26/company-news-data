---
schema_version: "1.0.0"
document_id: "526c45c35ba0e4b23a6f8574a04566994aeefa6b55f1a416cba81de644352883"
company_key: "vertiv-holdings-llc-class-a-common-stock"
company: "Vertiv Holdings LLC"
source_id: "vertiv-holdings-llc-class-a-common-stock-news-import-f0997059a52b"
canonical_url: "https://www.vertiv.com/en-us/insights/articles/blog-posts/full--stack-infrastructure-planning-for-high----density-ai-deployments/"
published_at: null
first_seen_at: "2026-07-24T06:56:37.102534+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:47f63719d073272cbd1336a244c55d52a0151d3944703fac447dd68aa8605f94"
---

# Full-stack infrastructure planning for high-density AI deployments

###### *“If \[power* *and* ** *cooling systems\] are not aligned, the data center can’t respond properly to dynamic loads or sudden* *therm* *al* *changes.”*


*— Martin Olsen, VP Segment Strategy & Development, Vertiv*


###### *"We have to bring these elements \[compute, memory, networking, and AI acceleration\] closer together because* *w* *e're* ** *running* *into limitations in semiconductor processes, power distribution, thermal limits, and so many more."*


*— Jim McGregor, Founder and Principal Analyst, TIRIAS Research*


Rack densities are climbing faster than traditional methods can accommodate. As graphics processing unit (GPU) clusters pack tightly together to maintain connectivity, power delivery, thermal management, and physical space compress simultaneously. The infrastructure required to support[artificial intelligence (AI)](https://www.vertiv.com/en-us/solutions/ai-hub/) deployments is increasingly in deficit.


In this[DatacenterDynamics (DCD) episode](https://www.datacenterdynamics.com/en/broadcasts/dcd-compute-storage-networking/2026/2026-trends-and-outlooks-ai-and-high-density-computing/) “2026 Trends and Outlooks: AI and High-Density Computing,” Kat Sullivan, Head of Channels, Compute at DCD, sat down with[Martin Olsen](https://www.linkedin.com/in/martintolsen/) , VP Segment Strategy & Development, Vertiv, and[Jim McGregor](https://www.linkedin.com/in/tekstrategist/) , TIRIAS Research Founder and Principal Analyst, to examine how operators should respond as AI loads continue to rise. Their discussion covers three practical areas:


- Treating compute, power, and cooling as a single system,
- Planning for mixed rack densities, and
- Using prefabrication to reduce field labor and integration risk.


##### Kat Sullivan, DCD: What market trends are you seeing as AI accelerates existing changes or creates new ones?


###### Jim McGregor, TIRIAS:


AI demand keeps outpacing available capacity. The technology isn’t mature, models are growing, and the number of specialists is increasing. We’re pushing to run more inference at the device level, but model size and complexity make that hard. The gap between AI’s pace and what data centers can support is widening, so capacity constraints are now constant.


##### KS: As high-performance workloads expand, what is Vertiv focusing on?


###### Martin Olsen, Vertiv:


Vertiv builds the power and cooling systems around modern compute. Rack densities that were up to 35 kilowatts (kW) have moved rapidly to up to 140 kW, and new generations are pushing toward 240 to 250 kW per rack. That stresses power delivery to a limited physical space and raises thermal loads. At 100 megawatts (MW) to 1 gigawatt (GW) sites, traditional chiller plants can dominate site layout. Facilities also operate with mixed densities, so most deployments combine direct liquid cooling for GPU racks with about a 20% air component, adding complexity across power distribution, cooling loops, and airflow.


##### KS: Does “full-stack” thinking extend beyond facilities into orchestration and software?


###### JM:


Yes. We increasingly use AI to manage AI because the system is too complex to operate manually. Platforms span from devices to cloud clusters, so the stack has to be modeled end-to-end. Electronic Design Automation (EDA) tools from Siemens, Cadence, and Synopsys are being pushed to simulate from semiconductor IP through system-level thermal and power behavior. These tools incorporate AI, and engineers use AI to run them. Automation is now necessary.


##### KS: What does the shift from processor-focused design to full-stack thinking mean in practice?


###### MO:


A single processor or GPU isn’t enough on its own. It has to be paired with memory, networking, software, and the model frameworks that make it usable. Most organizations rely on this kind of integrated stack because building each component independently isn’t practical. We apply the same idea to physical infrastructure. Power systems, cooling systems, white space, and management tools must be designed together instead of as separate parts. If they aren’t aligned, the data center can’t respond properly to dynamic loads or sudden thermal changes.[Vertiv™ OneCore](https://www.vertiv.com/en-us/products-catalog/facilities-enclosures-and-racks/integrated-solutions/vertiv-onecore-prefabricated-hybrid-built-data-center/) follows this approach. It’s a prefabricated AI-factory design built to operate as one system, from grid-level power delivery through chip-level thermal management. This reduces integration issues and helps maintain performance as workloads fluctuate.


###### JM:


We have to bring these elements closer together because we're running into limitations in semiconductor processes, power distribution, thermal limits, and so many more areas within the same rack architecture. For the first time, our industry is finally looking toward the future and trying not to be reactionary.


##### KS: What constraints are most likely to slow AI scaling, and which power/thermal approaches are closest to keeping pace?


###### MO:


Growth isn’t stopping, but key constraints are real:


Power availability


Moving capacity from the grid to the site is difficult. Some operators are evaluating small modular reactors; the technology is mature, but commercial adoption is early and highly regulated. There are also large, low-speed generators similar to big ships out there in the world. They got up to 30 MW generators on them that can supplement supply.


Labor


Gigawatt-scale builds can require thousands of electricians and mechanical contractors; some sites run with around 7000 workers during construction. Prefabrication shifts more work into factory environments and reduces field-integration effort.


Stranded capacity


Traditional redundancy leaves unused headroom. Designs that reclaim margin without impacting reliability can free capacity. Seasonal conditions matter as well like colder months mean lower mechanical load and can free capacity for compute.


Warm water loops


Operating around 40°C-45°C instead of 15°C-20°C enables more hours on dry coolers rather than chillers, reducing thermal energy use and freeing electrical capacity for GPUs. Chillers still have a role depending on climate and workload.


###### JM:


Nobody is planning a data center around a single power source. Over the past two years, it's become clear that relying solely on the grid brings real challenges and trade-offs. That's why layered approaches — using batteries, supercapacitors, on-site generation, and potentially SMRs (Small Modular Reactors) — are gaining momentum and hold significant potential.


##### KS: Final thoughts for operators planning 2026 builds?


###### MO:


Density growth is too fast for traditional methods. Treat infrastructure as a full stack and use prefabricated architectures to move faster and lower integration risk. Expect the landscape to change every few months and plan for iteration.


###### JM:


Expect continued acceleration throughout the decade. Watch the memory and storage side since it’s the primary capacity risk in the near term. Also expect periodic DeepSeek moments. I would expect at least one of those every year.


###### Building what’s next


Densification is advancing from every direction: memory, networking, processing, and AI acceleration. Each one has its own trajectory, demanding greater alignment. Operators planning power, cooling, and compute as separate workstreams will find integration complexity outpacing capacity.


For builds that need tighter integration and less field complexity, consider a prefabricated approach that engineers the powertrain and thermal chain as a single system to handle dynamic GPU loads. Explore how Vertiv™ OneCore applies full-stack power and cooling design to support high-density AI deployments and reduce integration risk:[Vertiv™ OneCore: Prefabricated, Hybrid-Built Data Center](https://www.vertiv.com/en-us/products-catalog/facilities-enclosures-and-racks/integrated-solutions/vertiv-onecore-prefabricated-hybrid-built-data-center/)
