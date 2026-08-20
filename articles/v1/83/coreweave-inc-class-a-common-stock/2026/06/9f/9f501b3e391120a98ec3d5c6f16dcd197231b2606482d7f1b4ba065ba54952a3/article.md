---
schema_version: "1.0.0"
document_id: "9f501b3e391120a98ec3d5c6f16dcd197231b2606482d7f1b4ba065ba54952a3"
company_key: "coreweave-inc-class-a-common-stock"
company: "CoreWeave Inc."
source_id: "coreweave-inc-class-a-common-stock-news-import-30332681496c"
canonical_url: "https://www.coreweave.com/news/coreweave-sets-new-ai-training-records-in-mlperf-r-training-v6-0-training-deepseek-v3-in-approximately-two-minutes"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-25T00:41:32.785334+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:44537d4bf5a06e2cca9b01bb372ef2c7470af1b5fa43c7c96bf29ac37048f502"
---

# CoreWeave Sets New MLPerf Training Records

**LIVINGSTON, N.J. — June 16, 2026** — CoreWeave, Inc. (Nasdaq: CRWV), The Essential Cloud for AI™, today announced record-breaking results in the MLPerf® Training v6.0 benchmark suite. Running on the same CoreWeave Cloud infrastructure available to customers today, CoreWeave delivered the fastest DeepSeek-V3 671B training performance in the benchmark, training one of the most computationally demanding models ever benchmarked in 2.02 minutes on 8,192 NVIDIA GB300 NVL72 GPUs — the largest GB300 cluster submitted in this round.


As frontier models reach trillion-parameter scale and agentic workloads become the new standard, training performance has emerged as a defining constraint on how quickly AI teams can iterate, experiment, and bring models to production. The gap between theoretical hardware performance and real-world training efficiency is determined not by silicon alone, but by how well networking, orchestration, scheduling, storage and software work together as a system. CoreWeave's MLPerf Training v6.0 results reflect the company's sustained investment in full-stack optimization and the operating standard[CoreWeave Mission Control](https://www.coreweave.com/news/coreweave-expands-mission-control-to-accelerate-enterprise-ai-adoption) brings, consistently turning cutting-edge hardware into reliable, production-ready training performance at scale.


"Training DeepSeek-V3 in two minutes on the largest GB300 cluster reflects years of metal-to-model engineering investment,” said Chen Goldberg, Executive Vice President of Product and Engineering at CoreWeave. “These results came from the same infrastructure our customers run in production today, not a benchmark-only setup. That's what an AI-native cloud is built to do.”


**Training DeepSeek-V3 671B in Approximately Two Minutes: The Largest GB300 Cluster in the Benchmark**


CoreWeave submitted three GB300 NVL72 configurations on DeepSeek-V3 671B, the benchmark's most demanding workload, achieving the fastest results across all Closed/Available-cloud submissions. On 8,192 GPUs across 2,048 nodes, CoreWeave hit target quality in approximately two minutes. Scaling down to 4,096 GPUs across 1,024 nodes, training was completed in 3.09 minutes. At 2,048 GPUs across 512 nodes, the result was 5.54 minutes. As the cluster size doubled at each step, training time improved predictably — a consistent, near-linear scaling efficiency that reflects full-stack optimization across every layer of the CoreWeave platform.


CoreWeave was the only submitter in the v6.0 round to scale a GB300 platform beyond 2,048 GPUs on DeepSeek-V3. The scaling story is as significant as the result demonstrating that full-stack optimization delivers more usable performance per GPU than raw scale alone. For AI teams operating under compute budgets, that scaling curve translates directly into faster training runs, shorter development cycles, and quicker time to production.


**Consistent Performance Across Every Cluster Size**


CoreWeave's MLPerf Training v6.0 results demonstrate that full-stack infrastructure advantages extend across deployment sizes, not just at frontier scale.


On NVIDIA GB300 NVL72, CoreWeave's 4,096-GPU deployment reached the Llama-3.1-405B reference quality target in 9.77 minutes, achieving near-parity with larger GB200 deployments while using 20% fewer GPUs. The run was built on NVIDIA NeMo Framework Release 26.04, with full CUDA graphs, Tensor/pipeline/context-parallel sharding tailored to the GB300 NVL72 topology, and NVIDIA Spectrum-X Ethernet running RoCE for scale-out fabric.


On a compact 8-node, 64-GPU NVIDIA HGX B200 cluster connected via InfiniBand, CoreWeave trained GPT-OSS-20B in 26.98 minutes and Llama-3.1-8B in 16.5 minutes. Through optimizations in orchestration, communication libraries, and distributed training configuration, CoreWeave delivered performance from the B200 platform that rivals larger and newer-generation deployments. This validated that CoreWeave's engineering advantages benefit customers at every scale, not just the largest clusters.


**The Infrastructure Behind the Results**


CoreWeave's MLPerf Training v6.0 results reflect optimizations across every layer of the stack:


- **Fleet-Wide Performance Consistency:** CoreWeave Mission ControlTM continuously performs health checks across the latest rack scale systems like GB300, validating hardware, firmware, network, and thermal health before and during large-scale training jobs. This reduces stragglers and ensures workloads run on a consistent, performance-qualified infrastructure baseline.
- **NVLink-Domain-Aware Scheduling:** CoreWeave SUNK is topology-aware by design, intelligently placing workloads to maximize locality and co-locating expert-parallel groups within the same NVL72 domain to minimize inter-rack communication for MoE workloads.
- **Optimized Network Performance:** CoreWeave employs a rail-aware networking strategy that balances traffic, ensuring bandwidth is utilized efficiently and preventing hotspots from developing within the fabric at multi-thousand-GPU scale.


"The gap between benchmark performance and production reality remains one of the most persistent challenges in AI infrastructure," said Brendan Burke, Research Director at Futurum Research. "CoreWeave's MLPerf Training v6.0 results, particularly training DeepSeek-V3 in two minutes on the largest GB300 cluster in the benchmark, demonstrate that full stack AI expertise compounds real-world performance gains as new hardware arrives. For AI researchers under pressure to race ahead of the field, that advantage separates leaders from followers."


**Built on a Foundation of Production-Ready Infrastructure**


CoreWeave's MLPerf Training v6.0 results were achieved on the same production infrastructure available to customers today. The networking fabric, scheduler, storage architecture, and CoreWeave Mission Control orchestration platform used in MLPerf are the same systems customers use to run real-world workloads. This was not a benchmark-only environment, it was a validation of the platform customers can access now.


These results build on a growing body of independent validation. CoreWeave is the only AI cloud to earn the top Platinum ranking in both SemiAnalysis ClusterMAX™ 1.0 and 2.0, and one of the leading providers for inference speed and price-performance for Moonshot AI's Kimi K2.6 in independent benchmarking by Artificial Analysis.


Learn more about CoreWeave's MLPerf Training v6.0 results on the[CoreWeave blog](https://www.coreweave.com/blog/coreweave-trains-deepseek-v3-in-two-minutes) .
