---
schema_version: "1.0.0"
document_id: "7f083eac4c68c0ac5f549e9a9d27419453ada2726c6927f046bcdf2bd9d8d2a7"
company_key: "penguin-solutions-inc-common-stock"
company: "Penguin Solutions Inc."
source_id: "penguin-solutions-inc-common-stock-news-import-f9c47211aa5e"
canonical_url: "https://www.penguinsolutions.com/en-us/resources/blog/gpu-accelerated-hpc-oil-gas-bp-case-study"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-25T19:00:09.417744+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:17f0c54f2d36ebfadebea4ff93f32646fde05c7f5c99f840156527f22a15cfae"
---

# Fueling bp's Innovation and Operational Excellence

For upstream oil and gas exploration teams, seismic imaging directly shapes high-value drilling decisions. When geoscientists can interpret seismic data faster, evaluate more reservoir scenarios, and generate higher-fidelity subsurface predictions, they can refine well placement decisions with greater confidence. Distinctive seismic imaging, accelerated interpretation, and better modelling capability can improve capital efficiency and reduce uncertainty while supporting safer, more productive field development.


Center for High-Performance Computing at bp's Westlake campus in Houston, TX


For bp, a major integrated oil and gas company with a long and successful track record in leveraging high-performance computing (HPC) for business impact, the question wasn’t whether its current infrastructure was working. The question was how much further and how much faster the bp teams could go with the right advanced compute infrastructure.


To find the answer, bp collaborated with Penguin Solutions and NVIDIA to design and build upon their GPU-enabled HPC environment in Houston, Texas for modern energy workloads, with seismic processing as its primary mission.


The system will combine NVIDIA Blackwell GPUs, Penguin SMART Memory 3840 DIMMs, and liquid-cooled infrastructure engineered for performance, efficiency, and scalability. Penguin Solutions will also provide on-site deployment and integration support.


The result is a future-ready computing platform designed to help bp run sophisticated algorithms, process larger datasets, and unlock new opportunities across its operations. Designed to deliver more than 80x acceleration in seismic imaging workflows compared to bp’s CPU-based platforms, the system will also enable advanced reservoir simulation, where physics fidelity, numerical precision, and throughput directly influence production and storage predictions.


## Why Modernize with GPU-Accelerated HPC?


Today’s operational environments have tighter safety and environmental requirements that place new demands on compute infrastructure. In addition, oil and gas operators must address reservoirs that are more complex, increasing expectations for faster, data-driven decisions across upstream and downstream operations.


For bp, GPU-enabled HPC modernization was a strategic investment that aligned infrastructure capability with the company’s priorities around reliability, safety, and long-term competitiveness.


A conventional hardware refresh only improves existing capacity. A growth program expands what teams can evaluate, model, and execute. GPU-accelerated architectures reduce time-to-solution, increase model fidelity, and enable teams to run more seismic propagation, reservoir simulation, computational fluid dynamics, and advanced analytics scenarios within operationally relevant timeframes.


## Defining Success Before Designing Architecture


Before the first rack was designed, Penguin Solutions collaborated with bp to define success in business terms. Three bp-required outcomes provided a decision framework that guided every architectural choice and tradeoff that followed:


1. **Industry-leading innovation:** The approach needed to enable seismic techniques, reservoir simulation models, and analytical workflows that were not viable on CPU-based infrastructure, providing more compute power to expand what bp’s geoscience and engineering teams could accomplish and extend the range of effectively managed workflows.
2. **Peak performance:** The approach also needed to create an environment where HPC resources could keep pace with increasing model and business complexity by eliminating advanced analytics bottlenecks, enabling faster decision making, and upleveling throughput and time-to-solution.
3. **Operational excellence:** Ultimately, bp needed to translate faster, richer analysis across value streams into safer operations, more efficient upstream and downstream assets, and more productive teams, with success defined not only by utilization rates but also by measurable operational impact across the business.


## Translating Strategy into Requirements


Seismic propagation workloads place heavy demands on infrastructure. For example, the accuracy of a subsurface image depends on how precisely the simulation captures wave behavior in complex geological formations. Available compute power affects how much detail teams can process, how quickly they can iterate, and how effectively they can generate high-quality images.


> ‍ *“By combining Penguin Solutions’ HPC expertise with NVIDIA’s GPU technology, we’re enabling our teams to use some of the most advanced physics available in seismic imaging,”* said Richard Corfield, Vice President of Imaging Technology at bp. *“The ability to run increasingly sophisticated algorithms at scale is helping us generate insights faster, improve operational performance, and unlock greater value from our data. This is a clear example of how technology innovation translates directly into business impact.”*


Reservoir simulation adds another layer of computational intensity. A reservoir simulator is a digital replica of underground rock formations that predicts how oil, water, and gas flow through the subsurface as pressure and operating conditions change. The numerical precision of these simulations is key - which is why double precision, or FP64, matters for these and other applications, and why bp’s architecture benefits from the FP64 capability of the NVIDIA HGX™ B200 platform.


For bp, modernization required finding the right balance between compute power, time-to-solution, precision, and business value. Although more compute power can enable better science, the architecture also needed to achieve operational efficiency and scalable growth within disciplined cost parameters across several interdependent design areas.


### Compute, Memory, and I/O Balance


GPU density needed careful alignment with memory bandwidth, storage performance, and I/O throughput. A dense GPU cluster can create new bottlenecks if memory, networking, or storage can't keep pace. By incorporating eight NVIDIA Blackwell GPUs per node, paired with two x86 CPUs and Penguin’s SMART Memory 3840 DIMMs, Penguin Solutions helped bp architect a balanced environment where compute, memory, and data movement work together seamlessly across seismic imaging, reservoir simulation, and computational fluid dynamics workloads.


### Data Movement and Storage Architecture


Seismic datasets can be extremely large and data movement can affect cost, performance, and workflow efficiency. The expansion design needed to account for where data originates, how teams process it, how much data they retain, and how storage and networking can keep GPUs supplied with data.


Penguin Solutions helped bp evaluate storage and data movement requirements so the environment could support both current workflows and future growth.


### Scalability and Optionality


Because bp expects compute demand to continue to grow, Penguin Solutions helped the company plan for further expansion, scalability, and interoperability across GPU generations and vendor ecosystems, reducing lock-in risk and supporting future growth of the HPC center.


### Liquid Cooling


The GPU densities for this class of HPC workload require advanced cooling strategies. bp prepared its site for liquid cooling while Penguin Solutions provided the design and implementation support. This approach helped protect performance, improved energy efficiency, and supports long-term infrastructure reliability.


## The Ecosystem: bp, NVIDIA, and Penguin Solutions, a Formula for Success


This project required close coordination among bp, NVIDIA, and Penguin Solutions.


For bp, where seismic imaging is the fundamental enabler of every well drilled, the acceleration delivered through growing and optimizing high-performance computing has direct consequences for drilling quality, reserve recovery, and capital efficiency. The same foundation also strengthens precision-intensive reservoir simulation, helping teams connect subsurface understanding to more reliable and efficient energy delivery.


The resulting infrastructure is designed to be managed, not just operated. It is modular, accessible, and supported by Penguin’s teams throughout implementation. The significant 2026 expansion with Penguin Solutions units will nearly double HPC capability for seismic imaging, AI-driven analytics, weather modeling, digital rock simulation, and reservoir simulation.


---


In addition to supplying GPU chips, NVIDIA was instrumental in driving acceleration across the entire stack. Built on NVIDIA HGX B200, the new platform integrates Blackwell GPUs connected with NVIDIA Quantum InfiniBand to give bp low-latency fabric for distributed seismic data processing, reservoir simulation, and artificial intelligence (AI) workflows.


NVIDIA worked with bp to optimize key codes for GPU acceleration and incorporate NVIDIA CUDA-X™ libraries into its workloads, including NVIDIA cuFFT for FFT-based acoustic and elastic wave propagation, NVIDIA nvCOMP with Bitcomp for wavefield compression, and NVIDIA cuFile APIs for GPUDirect Storage-enabled wavefield storage.


---


For bp, the decision to work with Penguin Solutions was a matter of expertise. Energy HPC isn’t generic enterprise compute infrastructure. The workload profiles, data volumes, and performance requirements for seismic propagation and reservoir simulation demand design decisions that a general system integrator might not get right. Penguin engaged early with validated reference architectures and direct experience in with large-scale AI, HPC, and[energy-specific infrastructure deployments](https://www.penguinsolutions.com/industries/oil-gas-energy) .


Penguin’s design and build process includes pre-integration and at-scale validation under real workload conditions prior to deployment. The expected acceleration in seismic imaging workflows isn’t a gradual improvement realized over months of tuning. It is the expected outcome of an architecture that was right-sized and validated well before go-live. Faster seismic imaging means more sophisticated subsurface products and better-informed well placement decisions.


> *“What stood out throughout the project was Penguin’s ability to translate our requirements into a solution that met both our performance and operational goals,”* said Elizabeth L'Heureux, Principal Head of HPC at bp. *“The modular node design creates a more maintainable infrastructure, making the environment easier to manage, support, and evolve as our needs change over time.”*


## What’s Next for bp?


bp’s current deployment is a platform for growth. As this project moves from design and build to deployment and management, the focus will shift to continuous optimization, fine-tuning seismic and reservoir simulation performance, and evolving the environment alongside growing computational requirements. This scalable set-up positions bp to embark on future AI initiatives and meet expanding operational demands as AI adoption accelerates across the oil and gas industry.
