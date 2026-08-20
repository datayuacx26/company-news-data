---
schema_version: "1.0.0"
document_id: "7a8cab17c968cdc79320b4ef07b2257aa7c974147bbf009b3acfe4b4d0af1473"
company_key: "axe-compute-inc-common-stock"
company: "Axe Compute Inc."
source_id: "axe-compute-inc-common-stock-rss-ffaba1d535a1"
canonical_url: "https://axecompute.com/mixture-of-experts-gpu-shortage/"
published_at: "2026-08-03T06:21:35+00:00"
first_seen_at: "2026-08-03T07:35:42.468083+00:00"
fetched_at: "2026-08-03T11:29:23.074207+00:00"
content_hash: "sha256:7933e20692d17dc76e1389118ec749d985b9ed2dd1c996a6110f8801859b4c9b"
---

# Mixture-of-Experts and the GPU Shortage

Mixture-of-experts models are sold as the efficient answer to the GPU crunch, because they cut the compute spent on each token. That framing measures the wrong resource. MoE reduces arithmetic, which is the abundant input, while keeping every expert loaded in memory, which is the scarce one. Since the GPU shortage is a memory and packaging shortage rather than a compute shortage, the rise of MoE deepens it. The first hardware designed around that fact is NVIDIA’s Vera Rubin platform.


**Key finding:** A mixture-of-experts model activates only a fraction of its parameters per token, yet all of them must sit in GPU memory. DeepSeek-V3 holds **671 billion** parameters but uses just **37 billion** per token. The binding constraint in the GPU shortage is memory bandwidth and high-bandwidth memory, where supply is tightest, so MoE trades the abundant resource for the scarce one. Vera Rubin answers with nearly triple the memory bandwidth per GPU and a doubled routing fabric, so MoE at scale moves first.


671B / 37B


DeepSeek-V3 total vs active parameters


3.0x vs 1.6x


Compute vs memory-bandwidth growth per two years


22 TB/s


HBM4 bandwidth per Rubin GPU, nearly triple Blackwell


## The Promise: More Model for Less Compute


The idea behind mixture-of-experts is elegant. A dense model runs every parameter for every token. An MoE model splits its feedforward layers into many smaller experts, and a router sends each token to only a few of them. The model can therefore grow enormous in total size while the compute spent on any single token stays small.


The 2021 Switch Transformer proved the approach, scaling sparse models to a trillion parameters at roughly constant compute per token. Mistral’s Mixtral 8x7B and DeepSeek-V3 then brought it into mainstream production. On paper, this looks like the cure for the GPU shortage: bigger, more capable models without a matching rise in arithmetic. That is exactly why the conclusion is misleading.


## The Catch: You Still Pay for Every Expert


A router activates a few experts per token, but it could route to any of them. So every expert has to be resident in GPU memory at all times. The compute follows the active parameters. The memory follows the total parameters. Those two numbers have come apart, and the larger one is the one that has to fit in the box.


The published models make the gap concrete. Mixtral 8x7B activates about 13 billion parameters per token, yet all 47 billion of its parameters occupy memory. DeepSeek-V3 is larger still: 671 billion total parameters, with 37 billion activated for each token. A team sizing hardware for the active compute alone would be unable to load the model at all. You provision for the total, and you pay for the total.


## The Bottleneck Was Never FLOPs


The efficiency story leaves out the binding constraint. The GPU shortage is a shortage of memory and the packaging that feeds it, and arithmetic has little to do with it. Gholami and colleagues, writing in IEEE Micro, measured the gap: peak server compute has scaled about 3.0 times every two years, while memory bandwidth has scaled only 1.6 times. They call this growing gap the memory wall. It means modern accelerators are starved for data long before they run out of math.


Supply makes the same point physically. The true chokepoint in the GPU market is high-bandwidth memory and advanced packaging, not logic transistors. TSMC’s CoWoS packaging integrates HBM with the GPU die, and its order book runs full into 2027. NVIDIA has reportedly reserved more than half of that capacity, according to TrendForce and DigiTimes. The scarce, rationed resource is precisely the one MoE consumes most.


## Why This Makes the Shortage Worse


Put the two facts together. MoE saves compute, the resource that is relatively abundant, and spends memory, the resource that is rationed. A 671-billion-parameter model occupies the high-bandwidth memory of many GPUs even when most of its experts are idle for a given token. To serve it, an operator needs more memory-rich accelerators, and those are the hardest GPUs to get.


Routing adds a second, less visible cost: communication. When experts sit on different GPUs, every token must travel to its experts and back, and that dispatch traffic is bursty, asymmetric, and highly synchronised, the pattern that most stresses an interconnect. The serving economics deepen the effect. Because all experts stay loaded, an MoE model is only cost-effective at high, sustained utilization, when enough traffic keeps the resident weights busy. At low or bursty utilization, the model still pays full memory rent for experts that rarely fire. Goldman Sachs Research projects token consumption rising 24 times by 2030, so that resident-memory cost scales across whole fleets, following the pattern we set out in[AI Inference Costs at Scale](https://axecompute.com/ai-inference-costs-at-scale/) .


MoE remains the right architecture, and it is measurably more capable per unit of compute. The mistaken assumption is that it relieves the hardware crunch. In reality it relocates the crunch from compute to memory and interconnect, and memory is where the crunch already lives.


## Why Vera Rubin Changes the MoE Equation


NVIDIA built its newest platform around this exact constraint. Each Rubin GPU pairs 288 GB of HBM4 with up to 22 TB/s of bandwidth, nearly triple a Blackwell GPU, and a Vera Rubin NVL72 rack holds 20.7 TB of GPU memory at roughly 1.6 PB/s. More experts sit resident per rack, and the bandwidth to read them grows faster than the capacity to hold them. For an architecture whose cost is dominated by resident weights and memory traffic, that is the dimension that matters.


The routing fabric moves in step. NVLink 6 doubles all-to-all bandwidth to 3.6 TB/s per GPU and 260 TB/s per rack, so seventy-two GPUs behave as one accelerator for expert dispatch. In addition, the NVLink 6 switch adds in-network compute that accelerates the collective operations MoE routing generates, and each Vera CPU contributes 1.5 TB of coherent LPDDR5X over a 1.8 TB/s link, opening offload paths for cold weights and KV cache. The interconnect stops being the wall the architecture keeps hitting.


The stated economics follow. NVIDIA reports massive-scale MoE inference at up to 10 times lower cost per token and MoE training with 4 times fewer GPUs, both measured against the previous Grace Blackwell (GB200) generation. Consequently, MoE at production scale is the clearest case where the new platform pays for itself, and it is why memory-bound workloads lead the migration. The timeline and reservation details are in[Vera Rubin early access](https://axecompute.com/vera-rubin-early-access/) .


## What Mixture-of-Experts Demands From Infrastructure


Axe Compute gives enterprises and AI innovators choice across hardware, geography, and deployment speed. Infrastructure that is live, not planned. Axe Compute Access provides the latest GPU compute options in as fast as 48 hours across numerous global locations, and Axe Compute Build delivers large-scale dedicated AI factories, all backed by enterprise-grade SLAs and support.


The infrastructure that suits MoE is specific. It needs high-memory GPUs to hold every expert, a high-bandwidth fabric to move tokens between experts on different devices, and sustained utilization to justify the resident weights. Above all, it rewards the ability to choose the right hardware rather than accept whatever is on offer.


That is a bare-metal problem more than a commodity-cloud one. A team running MoE in production benefits from dedicated capacity, sized for memory and kept busy, with the freedom to pick GPU memory and network fabric through a program like Axe Compute Build. The wider trade-off between dedicated and on-demand infrastructure is the subject of[Bare Metal vs Cloud GPU](https://axecompute.com/bare-metal-vs-cloud-gpu/) , and capacity planning across training and inference is covered in[Enterprise GPU Strategy in 2026](https://axecompute.com/enterprise-gpu-strategy-in-2026/) .


### Efficiency Redirects Demand


The lesson generalises. Efficiency gains in AI rarely reduce total demand; they redirect it. Mixture-of-experts is the clearest current example. It makes models cheaper to run per token and more demanding to host, and the hosting side is where the shortage bites. The teams that plan for memory and interconnect, and that time their move to the hardware built for both, are the ones that will keep these models served.


About Axe Compute


Axe Compute Inc. (NASDAQ: AGPU) is a neocloud AI infrastructure platform built on a fundamental premise: AI innovation should not be constrained by hardware choice or inventory limitations. Axe Compute gives enterprises and AI innovators choice across hardware, geography, and deployment speed through two delivery models: Axe Compute Access, providing the latest GPU compute options in as fast as 48 hours across numerous global locations, and Axe Compute Build, enabling enterprises to access large-scale dedicated AI factories, all backed by enterprise-grade SLAs and support. Axe Compute is headquartered in Pittsburgh, Pennsylvania. For more information, visit axecompute.com.


Plan for memory first.


Choice across hardware and geography · Capacity in as fast as 48 hours · Zero egress fees · Enterprise-grade SLAs


[Reserve Compute](https://portal.axecompute.com/)
Contact info@axecompute.com


## Frequently Asked Questions


### Do mixture-of-experts models reduce GPU demand?


Only on the axis that is already abundant. Mixture-of-experts models reduce compute per token, because a router activates a small subset of experts for each token. They do not reduce memory demand, because every expert must stay loaded in GPU memory. DeepSeek-V3 has 671 billion total parameters but activates only 37 billion per token, yet all 671 billion sit in memory. The binding constraint in the GPU shortage is memory, not compute, so MoE shifts demand toward the scarcer resource. Axe Compute provides the high-memory, sustained capacity these models require.


### Why does a mixture-of-experts model need so much GPU memory?


A router picks a few experts per token, but it could pick any of them, so all experts must be resident in GPU memory at all times. Mixtral 8x7B activates about 13 billion parameters per token, yet its full 47 billion parameters must be loaded. The memory footprint tracks total parameters, while the compute tracks only the active ones. That is why an MoE model cannot run on hardware sized for its active compute alone.


### What is the real bottleneck in the GPU shortage?


Memory bandwidth and high-bandwidth memory capacity, not raw arithmetic. Research on the memory wall shows server compute has grown about 3.0 times every two years while memory bandwidth has grown only 1.6 times. The physical chokepoint in supply is advanced packaging and HBM: TSMC’s CoWoS order book runs full into 2027. Mixture-of-experts models lean on exactly this constrained resource. Axe Compute provides bare-metal capacity built around memory and interconnect as much as FLOPs.


### Does Vera Rubin help with mixture-of-experts models?


It is the first platform designed around the MoE constraint. Each Rubin GPU carries 288 GB of HBM4 at up to 22 TB/s, nearly triple Blackwell, so more experts sit closer to more bandwidth. NVLink 6 doubles the all-to-all fabric that expert routing depends on, to 3.6 TB/s per GPU and 260 TB/s per rack, and adds in-network compute for the collective operations MoE dispatch generates. NVIDIA reports MoE inference at up to 10 times lower cost per token and MoE training with 4 times fewer GPUs, both measured against the previous Grace Blackwell (GB200) generation.


### What infrastructure do mixture-of-experts models need?


MoE models need high-memory GPUs to hold all experts, high-bandwidth interconnect such as InfiniBand or NVLink-class fabrics to move tokens between experts, and high, sustained utilization to be economical, since idle experts still occupy memory. That favours dedicated bare-metal clusters kept busy over bursty on-demand instances. Through Axe Compute Build, the client configures GPU memory, region, and network fabric against the workload, with capacity in as fast as 48 hours.


### Sources


- [Jiang et al. (Mistral AI), “Mixtral of Experts,” arXiv:2401.04088 (2024): each token accesses 47B parameters but uses 13B active during inference](https://arxiv.org/abs/2401.04088)
- [DeepSeek-AI, “DeepSeek-V3 Technical Report,” arXiv:2412.19437 (2024): 671B total parameters with 37B activated per token](https://arxiv.org/abs/2412.19437)
- [Fedus, Zoph, Shazeer, “Switch Transformers,” arXiv:2101.03961 (2021): scaling to trillion-parameter sparse models at roughly constant compute per token](https://arxiv.org/abs/2101.03961)
- [Gholami et al., “AI and Memory Wall,” IEEE Micro / arXiv:2403.14123 (2024): peak compute scaled ~3.0x per two years vs ~1.6x for memory bandwidth](https://arxiv.org/abs/2403.14123)
- [TrendForce: TSMC CoWoS advanced packaging (HBM integration) fully booked into 2027](https://www.trendforce.com/news/2025/12/08/news-tsmcs-cowos-l-s-reportedly-fully-booked-osat-partners-step-up-with-ases-cowop-in-focus/)
- [Goldman Sachs Research: token consumption projected to rise 24x by 2030](https://www.goldmansachs.com/insights/articles/ai-agents-forecast-to-boost-tech-cash-flow-as-usage-soars)
- [NVIDIA press release, January 5, 2026: NVLink 6 at 3.6 TB/s per GPU and 260 TB/s per rack with in-network compute for collective operations; massive-scale MoE inference at up to 10x lower cost per token and MoE training with 4x fewer GPUs versus the previous Grace Blackwell generation](https://nvidianews.nvidia.com/news/rubin-platform-ai-supercomputer)
- [NVIDIA Technical Blog, “Inside the NVIDIA Vera Rubin Platform”: 288 GB HBM4 at up to 22 TB/s per GPU; NVLink 6 switch in-network compute for MoE routing; Vera CPU 1.5 TB LPDDR5X coherent over 1.8 TB/s NVLink-C2C; MoE dispatch traffic characterised as bursty, asymmetric, and highly correlated](https://developer.nvidia.com/blog/inside-the-nvidia-rubin-platform-six-new-chips-one-ai-supercomputer/)
