---
schema_version: "1.0.0"
document_id: "167219e43028b43c877814f46f99bb6727709d4f8fa0b24f39514553933b5c32"
company_key: "astera-labs-inc-common-stock"
company: "Astera Labs Inc."
source_id: "astera-labs-inc-common-stock-rss-6ea615f4bb0d"
canonical_url: "https://www.asteralabs.com/what-nvidia-gtc-2026-said-about-the-future-of-ai-connectivity/"
published_at: "2026-03-26T18:20:16+00:00"
first_seen_at: "2026-07-20T23:21:25.038635+00:00"
fetched_at: "2026-07-28T22:00:16.885857+00:00"
content_hash: "sha256:7e8abc7372b655e1a66110669107fdb16375244a94b309a7b9bd4f742d92161e"
---

# What NVIDIA GTC 2026 Said About the Future of AI Connectivity

NVIDIA GTC has always been a window into where the AI Infrastructure industry is heading. This year, what that window revealed was a compute layer fragmenting, deliberately and by design. Jensen Huang’s keynote introduced not one new architecture but several: Vera Rubin for high-throughput GPU compute, the Groq LPU integrated as a decode accelerator for latency-sensitive inference, and a purpose-built CPU rack optimized for agentic workloads.


The through-line connecting all of it: as the accelerator landscape diversifies, the fabric joining those accelerators becomes more consequential, not less. For those of us in the connectivity ecosystem, GTC 2026 made that argument in concrete terms.


Here’s what stood out.


### Inference has a bandwidth problem, and it lives in the KV cache


The inference inflection Jensen described is driven by reasoning models that think before they answer, agentic systems that act across long context windows, and token generation volumes growing faster than anyone projected. It creates a specific and measurable pressure on memory.


At the center of it is KV cache: the stored key-value vectors from prior tokens that allow a model to maintain context without recomputing everything from scratch. As context windows lengthen, KV cache grows proportionally. As agentic systems run longer trajectories and handle more conversation turns, it compounds further. GPU HBM is already occupied by model weights and active computation, so KV cache has traditionally been stored on SSDs. This introduces latency that directly increases inference response times and limits how many concurrent requests a GPU can serve; at the context lengths modern reasoning models demand, that latency is no longer a minor tax. It results in a throughput ceiling.


CXL-attached memory is gaining traction as the right middle tier: higher bandwidth and lower latency than storage, with the capacity to extend well beyond what HBM alone can hold. The demonstrations at GTC put numbers on this. Astera Labs’ Leo CXL memory controller,[deployed in Penguin Solutions’ KV cache server](https://www.asteralabs.com/inference-tokenomics-how-cxl-memory-expansion-improves-ai-economics/) , showed a 3.6x memory expansion with 75% higher GPU utilization and 2x inference throughput compared to baseline configurations. The memory hierarchy for AI inference is being actively redefined, with HBM handling the hot path, CXL absorbing KV cache overflow, and NVMe-over-Fabrics handling deeper storage. Connectivity is what makes those tiers work together.


### Distributed inference means moving KV cache across the network, fast


When inference is running at scale, it typically isn’t running on a single GPU. Prefill, the phase that processes input context, is compute-intensive. Decode, the phase that generates output tokens one at a time, is memory-bandwidth-intensive. These two phases have different hardware affinities, and modern inference frameworks increasingly separate them onto different GPU pools, routing the KV cache between them.


NVIDIA’s Dynamo manages this disaggregated serving and demonstrated significant throughput gains from the approach, including 15x more DeepSeek R1 throughput on GB200 NVL72 in cited benchmarks. But the software orchestration depends on the underlying network to move KV cache quickly and predictably between nodes. KV-aware routing, which Dynamo uses to direct requests toward nodes that already hold the relevant cached context, only delivers its benefit if the latency of retrieving that cache is low enough to matter. That makes the scale-out fabric, along with the smart cable modules and signal conditioning enabling it, a direct participant in inference performance rather than background infrastructure.


### Mixture of Experts made scale-up bandwidth a first-order inference problem


Most of the frontier reasoning models in production today use a Mixture of Experts (MoE) design. Rather than activating all model parameters for every token, an MoE router selects a small subset of specialized expert layers per inference step. DeepSeek’s architecture, which Ian Buck’s Monday afternoon talk walked through in detail, activates eight experts out of 384 per layer, making it a trillion-parameter model that behaves more like a 32 billion-parameter model during inference.


MoE models are more parameter-efficient per inference step than dense models of equivalent capability. But those experts are distributed across GPUs in the rack, and every decode step requires those GPUs to exchange intermediate activations across the scale-up fabric. Buck quantified the difference plainly: a high-bandwidth switched scale-up fabric across 72 GPUs delivers around 1,800 GB/s of inter-GPU bandwidth per GPU, while Ethernet as a scale-up medium delivers closer to 100 GB/s. That 18x gap determines whether the scale-up fabric is a bottleneck during token generation.


As MoE becomes the dominant architecture for large-scale inference, and the evidence from GTC suggests it already is, scale-up fabric bandwidth becomes a direct input to inference throughput, not just a training consideration. PCIe-based fabrics and UALink, the open memory-semantic standard purpose-built for scale-up, are increasingly relevant here precisely because they address this communication pattern without the overhead of network-layer protocols.


### The accelerator stack is fragmenting, and connectivity is what holds it together


NVIDIA CEO Jensen Huang Spotlights NVIDIA Vera Rubin


One of the subtler but more consequential things Jensen described at GTC was a rack architecture composed of fundamentally different compute types, each matched to a different phase of the inference pipeline. Vera Rubin handles high-throughput prefill. The Groq LPU, now integrated into the Vera Rubin platform following NVIDIA’s acquisition of the Groq team, handles low-latency decode by exploiting its massive on-chip SRAM for token generation without the memory bandwidth constraints that limit GPU performance at that phase. A purpose-built CPU rack, optimized for single-threaded performance and high memory bandwidth, handles the tool execution and orchestration that agentic workloads demand. These are not variations on a single architecture. They are distinct silicon types, running distinct workloads, connected inside the same infrastructure.


That composition is a connectivity design challenge in a way that homogeneous GPU clusters are not. Each component type brings different interface requirements, application tuned memory capacities and throughput, different performance profiles, and different protocol preferences. The fabric joining them needs to handle those differences without becoming the bottleneck, which means it needs to be purpose-built for AI workloads, validated across silicon from multiple vendors, and observable enough to diagnose problems before they surface in production.


The trend extends beyond what was on stage. Hyperscalers have been deploying custom silicon alongside general-purpose GPUs for some time, and the diversity of accelerator types in production racks is only increasing. Open standards, including PCIe, UALink, CXL, and Ethernet, give infrastructure architects the flexibility to compose those racks across silicon generations and vendors. Custom connectivity paths serve the teams optimizing for specific performance envelopes. What both approaches share is a requirement for connectivity that is purpose-built for AI workloads, validated across the components it joins, and observable enough to manage at scale. That requirement only grows as the compute tier diversifies.


### The optical transition is underway


One of the more pointed questions circulating at GTC was whether copper or optical would define the next generation of scale-up fabrics. Jensen answered it directly: Nvidia is going to do both. The Vera Rubin platform supports copper scale-up through the Kyber rack at NVLink 144, and optical scale-up through Oberon to NVLink 576. The Spectrum X co-packaged optics switch, which integrates the optical interface directly onto the switch silicon rather than through a pluggable module, is already in production.


That last point deserves attention. Co-packaged optics (CPO) has been a topic of industry conversation for years, but production deployment is a different milestone. By moving the optical interface onto the switch package itself, CPO eliminates the electrical path between switch and optical module, reducing power consumption, improving signal integrity at high data rates, and enabling the kind of port density that pluggable optics struggle to sustain as bandwidth per lane continues to climb.


CPO is, however, the end of a transition that is still in its early stages for most of the market. The more immediate shift underway is toward linear optics (LPO and NPO), which removes the DSP from the optical module to reduce power and latency.[This trend was referenced recently in an Astera Labs blog](https://www.asteralabs.com/scaling-the-ai-rack-how-astera-labs-is-approaching-optics-for-scale-up/) . The tradeoff is that without a DSP performing signal correction inside the module, the responsibility for link quality shifts to the switch-side SerDes and the firmware managing it. Optics and switch need to be co-developed and managed under a single telemetry system to make that work reliably at scale.


### Observability becomes performance infrastructure


Every AI data center operates within a fixed power envelope. Once you’ve built a gigawatt facility, that ceiling doesn’t move. What does move is how efficiently the compute inside it runs relative to that ceiling.


Jensen introduced NVIDIA DSX, a digital twin platform that models AI factory infrastructure—thermal, electrical, networking—and uses that model to dynamically manage workloads and power allocation across the full data center. Ian Buck’s point was that most facilities today are leaving significant capacity on the table: racks provisioned for peak load that rarely runs at peak, power budgets that aren’t dynamically redistributed when demand shifts, cooling overhead that isn’t adjusted in real time. His estimate was that smarter management across those variables could yield a 2x improvement in token throughput from the same physical infrastructure.


For connectivity, this raises the bar on telemetry. Retimers, fabric switches, smart cable modules, and memory controllers are continuous sources of high-fidelity operational data. The fidelity of system-level optimization depends on visibility into the physical layer, signal integrity, equalization state, and forward error correction behavior—where performance degradation actually begins. For PCIe 6 and NVLink switches, even marginal signal degradation translates directly into lost effective bandwidth.


Observability, therefore, becomes a closed-loop system: monitoring link health, bandwidth utilization, thermal margins, and error rates, while continuously tuning signal behavior and adapting to changing conditions.


#### *What GTC 2026 made clear*


The accelerators are getting faster and more specialized. The models running on them are getting larger, more architecturally complex, and more demanding in how they move data. The result is an AI infrastructure environment where the interconnects, memory controllers, and switching fabric surrounding the GPU are under more pressure and doing more work than at any prior point in the industry’s development.


Connectivity used to be the part of the rack that nobody talked about at keynotes. That’s changing.


*Peter Lo is the Principal PR & Content Marketing Lead at Astera Labs.*
