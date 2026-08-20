---
schema_version: "1.0.0"
document_id: "826de5af265133c47dcbaed1ac40e294aea0e8bc0d4361e0b08bf6dcfbea5e05"
company_key: "yc-shadeform"
company: "Shadeform"
source_id: "yc-shadeform-news-import-b903e9afa9f0"
canonical_url: "https://shadeform.com/resources/articles/nvidia-blackwell-ultra-b300s-now-available-on-shadeform"
published_at: "2026-02-12T12:00:00+00:00"
first_seen_at: "2026-07-24T00:27:10.014959+00:00"
fetched_at: "2026-07-28T22:20:36.551441+00:00"
content_hash: "sha256:f54144772329f1a420ed34f3889bd080d810ce5f1c1204dc655602e369ca5345"
---

# Blackwell Ultra B300s, Now Available on Shadeform

### Blackwell Ultra Has Arrived on Shadeform


NVIDIA’s latest accelerator, the Blackwell Ultra B300, is now available to[deploy on-demand](https://platform.shadeform.ai/?gputype=B300&utm_source=blog_b300_lander) on Shadeform, with reserved clusters for both HGX B300 and GB300 available[via inquiry](https://shadeform.com/features/reserved-commitments?utm_source=blog_b300_lander) .


Powered by NVIDIA’s upgraded Blackwell architecture, the B300 expands on the B200 with:


- 1.5× higher NVFP4 throughput
- 2× faster attention layer acceleration
- 1.5× memory increase to 288GB per GPU


These improvements translate into up to 2× faster training cycles, 1.45× higher inference throughput, and a future-proof architecture capable of running increasingly complex open models with higher concurrency and lower cost per token.


### Deploying Blackwell Ultra on Shadeform


NVIDIA HGX B300 and GB300 deployments are coming online across cloud platforms in limited quantities.


Shadeform’s unified platform centralizes these deployments into one console, providing teams with access to the largest pool of Blackwell Ultra capacity, and the flexibility to scale across multiple cloud providers.


HGX B300 instances are available to[deploy on demand](https://platform.shadeform.ai/?gputype=B300&utm_source=blog_b300_lander) in 1×, 2×, 4×, and 8× partitions, with additional capacity coming to Shadeform in Q2 2026.


Multi-node HGX B300 and GB300 clusters are available to[reserve today](https://shadeform.com/features/reserved-commitments?utm_source=blog_b300_lander) across North America and Europe, with flexible terms ranging from one month to three years.


### Training Performance


In the latest MLPerf 5.1 Training report, Blackwell Ultra (GB300) swept all 7 benchmarks, delivering significant improvements in time-to-train over both Hopper (H100) and the base Blackwell architecture (GB200).


Most notably, the Blackwell Ultra system completed pre-training of Llama 3.1 405B nearly twice as fast as the base Blackwell system, and over four times as fast as the Hopper system.


System


Time-to-train Llama 3.1 405B (minutes)


8x GB300 NVL72 (512 GPUs)


64.605


8x GB200 NVL72 (512 GPUs)


121.757


64x HGX H100 (512 GPUs)


269.122


Higher NVFP4 throughput, faster attention execution, and expanded memory allow Blackwell Ultra to sustain higher utilization across both compute- and memory-bound phases of training.


This translates into shorter training cycles, faster iteration on model and data changes, and the ability to train larger models and longer-context sequences that were previously constrained by compute or memory limits.


### Inference Performance


Blackwell Ultra also delivers substantial improvements in inference workloads.


In the latest MLPerf Inference v5.1 report, Blackwell Ultra (GB300) set a new record with 45% higher DeepSeek-R1 throughput compared to the base Blackwell system (GB200), demonstrating substantial gains for large-scale reasoning and long-context workloads.


System


Deepseek-R1 toks/s (Offline)


Deepseek-R1 toks/s (Server)


GB300 NVL72 (72 GPUs)


420,659.00


209,328.00


GB200 NVL72 (72 GPUs)


289,712.00


167,578.00


These gains come primarily from improvements in attention execution, token generation efficiency, and memory handling. Larger working sets remain resident on device, and faster KV-cache and attention operations sustain higher throughput during long-context and multi-step reasoning tasks.


This translates into higher tokens-per-second per GPU, lower latency under sustained load, and improved cost efficiency. At current on-demand pricing, B300 instances carry an average premium of roughly 18% over B200s while delivering up to 45% higher throughput, reducing effective cost per token.


### Designed for Next Generation Model Architectures


Model architectures continue to evolve toward larger parameter counts, longer context windows, and multimodal inputs. These workloads increasingly stress memory bandwidth and attention throughput rather than raw compute alone.


Blackwell Ultra is designed for this shift. With up to 288GB of HBM3e per GPU, higher NVFP4 throughput, and faster attention execution, it provides the memory headroom and sustained performance needed to run modern large-scale models efficiently.


Larger working sets remain on device, long-context workloads maintain higher throughput, and multi-modal inputs are less likely to produce utilization drops common in previous-generation hardware.


As the pace of open-model releases accelerates, infrastructure needs to support multiple generations of workloads, not just the models available today. Blackwell Ultra enables teams to adopt larger, more complex architectures as they emerge, and deploy them at scale with leading performance.


### Get Started with Blackwell Ultra


Blackwell Ultra provides a clear advantage for teams training and deploying large-scale models by enabling significantly shorter training cycles, record-breaking throughput, and reduced bottlenecks for the most demanding model architectures.


Teams that need immediate access can deploy HGX B300 instances on demand through Shadeform’s unified platform.


For organizations planning large training runs or sustained inference workloads, reserved HGX B300 and GB300 clusters are available across multiple providers with flexible terms.


To get started:


- [Deploy now](https://platform.shadeform.ai/?gputype=B300&utm_source=blog_b300_lander) to launch B300 instances on demand.
- [Submit a reservation request](https://shadeform.com/features/reserved-commitments?utm_source=blog_b300_lander) to secure B300 and GB300 cluster capacity.
