---
schema_version: "1.0.0"
document_id: "54dec9604594fe7c4fd5654751a7d644e5dc56b449e843c0697b48d1dd97d3de"
company_key: "coreweave-inc-class-a-common-stock"
company: "CoreWeave Inc."
source_id: "coreweave-inc-class-a-common-stock-news-import-30332681496c"
canonical_url: "https://www.coreweave.com/news/coreweave-completes-industry-first-bring-up-of-nvidia-vera-rubin-nvl72"
published_at: "2026-06-01T00:00:00+00:00"
first_seen_at: "2026-07-25T00:41:32.785334+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:63d17fe0f2f01c541ed4ff7165ce0775a3d6d464838bc4a10bb58172e1af4a5d"
---

# CoreWeave Completes Industry-First Bring-Up and Validation of NVIDIA Vera Rubin NVL72

**LIVINGSTON, N.J. — June 1, 2026** —[CoreWeave](http://coreweave.com/) , Inc. (Nasdaq: CRWV), The Essential Cloud for AI™, today announced its bring up of[NVIDIA Vera Rubin NVL72](https://www.nvidia.com/en-us/data-center/vera-rubin-nvl72/) on CoreWeave Cloud. Leveraging its purpose-built software and engineering solutions, CoreWeave is the first AI cloud provider to bring up Vera Rubin, extending the CoreWeave platform’s support for NVIDIA hardware. The milestone achievement includes the completion of rigorous system-level validation for the entire rack scale architecture.


Agentic AI is reshaping infrastructure requirements. As models reach a trillion parameters, context windows extend to millions of tokens, and persistent reasoning sessions become standard, inference performance has emerged as the defining constraint on how quickly AI companies can operate and grow.


NVIDIA Vera Rubin NVL72 — featuring 72[NVIDIA Rubin GPUs](https://www.nvidia.com/en-us/data-center/technologies/rubin/) and 36[NVIDIA Vera CPUs](https://www.nvidia.com/en-us/data-center/vera-cpu/) per rack, connected via a 260 TB/s[NVIDIA NVLink](https://www.nvidia.com/en-us/data-center/nvlink/) 6th-generation fabric — delivers up to 10× better inference per watt, up to one-fourth fewer GPUs, and one-tenth the cost per million tokens compared to[NVIDIA Blackwell](https://www.nvidia.com/en-us/data-center/technologies/blackwell-architecture/) 1. With Vera Rubin, CoreWeave will deliver better results for customers.


"Our research depends on infrastructure that's both powerful and reliable, and CoreWeave has delivered on this as we've scaled across NVIDIA Hopper and Blackwell," said Craig Falls, head of Quantitative Research at Jane Street. "Their ability to deliver highly performant clusters with full cluster observability and a support team that engages deeply on hard problems gives us the confidence to partner with them on Vera Rubin. We are excited about the efficiency gains at rack scale translating into faster training runs and shorter iteration cycles for our researchers."


**Purpose-Built Infrastructure for Rack-Scale AI, Powered by CoreWeave Mission ControlTM**


To allow customer to take better advantage of Vera Rubin at production scale, CoreWeave developed a new set of purpose-built innovations:


- **Software-Defined Liquid Cooling:** Valvey is CoreWeave's programmable per-rack valve assembly which turns cooling from a passive mechanical system into a software-defined, rack-level control surface. Part of CoreWeave Mission Control, Valvey monitors flow rate, temperature, pressure, and leak-detection in real time, enabling automated isolation, emergency shutdown, and maintenance without disrupting neighboring racks on a shared cooling loop.
- **Unified Rack Control:** Racky is a new unified rack control appliance specifically designed for aggregating power, cooling, and environmental sensors into a standardized management surface, allowing each Vera Rubin rack to be managed as a cloud resource rather than a custom one-off build.
- **Multi-Rail, Multi-Plane Networking:** CoreWeave supports both NVIDIA Quantum-X800 InfiniBand and[NVIDIA Spectrum-X Ethernet](https://www.nvidia.com/en-us/networking/spectrumx/) with RDMA over Converged Ethernet RoCE, with a non-blocking, multi-rail, multi-plane RoCE fabric delivering 1.6 Tb/s of backend bandwidth per GPU. The Spectrum-X Ethernet architecture scales to configurations of hundreds of thousands of GPUs in two network tiers.
- **Secure, Scalable AI Cloud Operations** : CoreWeave is advancing secure, multi-tenant AI cloud operations with[NVIDIA BlueField-4 DPUs](https://www.nvidia.com/en-us/networking/products/data-processing-unit/) , enabling faster data access, lower latency, and stronger tenant isolation at scale. BlueField-4 offloads and accelerates infrastructure services, allowing tenants to run workloads across the full Vera Rubin computing platform while preserving control and security.


"The agentic era demands a fundamentally different approach to infrastructure, one that keeps pace with workloads that reason continuously, scale unpredictably, and operate in production around the clock," said Chen Goldberg, executive vice president of Product & Engineering at CoreWeave. "What separates infrastructure that performs in a lab from infrastructure that performs in production is the depth of engineering underneath it. With patent-pending innovations like Valvey and Racky, CoreWeave has done the full-stack orchestration work to enable Vera Rubin to perform the way it was designed to, not just in a lab, but at production scale for the world's most demanding AI teams.”


“Vera Rubin is the most capable AI platform NVIDIA has ever built,” said Ian Buck, vice president of Hyperscale and High-Performance Computing (HPC) at NVIDIA. “CoreWeave has consistently been at the frontier of deploying each new generation of NVIDIA architecture at scale, and their full-stack, end-to-end approach to Vera Rubin, from cooling to orchestration, is how the world’s most ambitious AI teams will push the next AI frontier.”


**Built on a Foundation of Deep Technical Partnerships**


Bringing a rack-scale platform like Vera Rubin NVL72 to production requires tight collaboration across the entire infrastructure stack. CoreWeave’s ecosystem of technology partners is central to how Vera Rubin reaches customers at speed and scale. Dell Technologies provided the architectural backbone for the platform through its high-performance PowerEdge XE9812 servers. The bring up also features Micron 7600 SSDs, delivering improved energy efficiency through one of the first liquid-cooled NVMe storage solutions deployed at rack-scale.


“Dell Technologies and CoreWeave share a commitment to delivering innovation that performs at the frontier of what AI demands,” said Michael Dell, chairman and chief executive officer at Dell Technologies. “The PowerEdge XE9812 was engineered for exactly this kind of density and precision. Working with CoreWeave to bring up the first NVIDIA Vera Rubin NVL72 rack is a direct validation of what enterprise-grade hardware can do when it’s paired with the right operational expertise.”


CoreWeave consistently delivers industry-leading performance, demonstrated by record-breaking[MLPerf benchmark results](https://www.coreweave.com/news/coreweave-delivers-leading-inference-performance-in-mlperf-r-benchmark) , its position as the only AI cloud to earn the top[Platinum](https://www.coreweave.com/news/coreweave-achieves-semianalysis-platinum-clustermax-tm-rating-for-the-second-consecutive-ranking-remaining-the-industrys-sole-platinum-provider) ranking in both SemiAnalysis ClusterMAX™ 1.0 and 2.0, and its[#1 ranking for inference speed and price-performance](https://www.coreweave.com/news/coreweave-achieves-1-ranking-for-inference-speed-and-price-performance-for-moonshot-ais-kimi-k2-6-model-in-independent-benchmark) for Moonshot AI’s Kimi K2.6 in independent inference benchmarking conducted by Artificial Analysis.


1https://[nvidianews.nvidia.com/news/nvidia-vera-rubin-platform](http://nvidianews.nvidia.com/news/nvidia-vera-rubin-platform)
