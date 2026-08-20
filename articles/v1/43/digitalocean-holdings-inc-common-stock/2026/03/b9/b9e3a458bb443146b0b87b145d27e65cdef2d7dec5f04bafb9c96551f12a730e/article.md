---
schema_version: "1.0.0"
document_id: "b9e3a458bb443146b0b87b145d27e65cdef2d7dec5f04bafb9c96551f12a730e"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/nvidia-dynamo-1-now-available"
published_at: "2026-03-19T22:13:37.480+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:18:23.526103+00:00"
content_hash: "sha256:808cca0b06fa589df690a70a48ceebc29b122a391c08703e56911493919aae60"
---

# Meet the New Standard for High-Performance, Low-Cost Inference: NVIDIA Dynamo 1.0 is now available to DigitalOcean Customers

[<- Back to blog home](https://www.digitalocean.com/blog)


[NVIDIA Dynamo 1.0](https://developer.nvidia.com/blog/nvidia-dynamo-1-production-ready/) , which was released on Monday at NVIDIA GTC, is now available to DigitalOcean customers to help drive performance enhancements and cost efficiency. NVIDIA Dynamo 1.0 offers a 7x inference performance increase on NVIDIA GB200 NVL systems, and by pairing it with DigitalOcean’s Agentic Inference Cloud, customers can achieve higher performance at lower costs while benefiting from seamless deployment. Working together, DigitalOcean’s optimizations with NVIDIA have already achieved a 67% cost savings for customers like Workato, and this new generation of Dynamo can unlock even greater gains for businesses who run production-grade agentic workflows. DigitalOcean customers can get access to NVIDIA Dynamo 1.0 as a container image that can be run on a Droplet or can deploy directly on DigitalOcean Kubernetes with an inference runtime (vLLM, SGlang, TensorRT).


## What is NVIDIA Dynamo 1.0?


NVIDIA Dynamo is a cutting-edge, high-performance inference service framework specifically designed to accelerate and optimize large-scale generative AI and inference models. Dynamo is an orchestration layer that sits above engines like vLLM, SGLang, and NVIDIA TensorRT-LLM. Think of it as the distributed traffic controller for your GPU fleet, seamlessly orchestrating GPU and memory resources across a cluster and reducing bottleneck by intelligently routing requests


Key technical breakthroughs offered by Dynamo 1.0 include:


-


**7x Performance Boost:** When paired with NVIDIA Blackwell Ultra GPUs, Dynamo can increase inference performance by up to 7x, significantly lowering your cost per token.


-


**KV-Aware Routing:** Instead of simple round-robin load balancing, Dynamo routes requests to the specific GPUs that already have the relevant “memory” from previous turns of a conversation.


-


**Disaggregated Serving:** Dynamo splits the “prefill” (reading the prompt) and “decode” (generating the answer) phases across different GPUs to maximize utilization and reduce latency.


-


**Memory Offloading:** The KV Block Manager (KVBM) moves data between high-speed GPU memory and lower-cost storage tiers, allowing you to handle massive context windows without hitting memory limits.


## How DigitalOcean optimizes inference workloads with Dynamo to improve throughput and latency


Customers using NVIDIA Dynamo on DigitalOcean can benefit from strong price-to-performance as well as a simple setup and an environment that fits well with Dynamo Architecture, especially for tightly controlled GPU clusters and KV cache optimization and routing. DigitalOcean has already been delivering wins for customers with NVIDIA Dynamo. Recently, we[partnered with Workato’s AI Research Lab](https://www.digitalocean.com/blog/workato-nvidia-technical-deep-dive-agentic-inference-cloud) to scale agentic AI capabilities across its platform, which processes over 1 trillion automated workloads. To meet the rigorous efficiency and cost requirements of production-grade inference, the team deployed NVIDIA Dynamo with vLLM on[DigitalOcean Managed Kubernetes (DOKS)](https://www.digitalocean.com/products/kubernetes) .


Using NVIDIA Dynamo v0.4.1+ vLLM on DOKS, Workato achieved:


-


**67% higher throughput** per GPU with 79% lower end-to-end latency and 77% time-to-first-token compared to different configurations on identical hardware


-


**33% lower hardware cost** using a NVIDIA H200 GPU vs. a NVIDIA A100 GPU for equivalent performance


-


**67% lower model cost** while using half the GPUs


Check out the[technical blog for more on how Workato achieved](https://www.digitalocean.com/blog/workato-nvidia-technical-deep-dive-agentic-inference-cloud) these outsized results with DigitalOcean.


With the power of Dynamo 1.0 and the newly-available NVIDIA HGX B300s, we look forward to achieving even greater performance and cost improvements for customers like Workato.


## The future of inference optimization with NVIDIA and DigitalOcean


In addition to Dynamo 1.0, as part of this year’s NVIDIA GTC, we’re excited to share other product releases and updates to further enhance the capabilities of DigitalOcean’s Agentic Inference Cloud. These include our new AI-first Richmond Data Center, a seamless path to experiment with NVIDIA Agent Toolkit and NemoClaw and deploy to DigitalOcean, support for NVIDIA Nemotron 3 Super and other high-performance models, and more. Learn more about the latest[DigitalOcean and NVIDIA GTC announcements directly from our CTO](https://www.digitalocean.com/blog/building-ai-factory-for-agentic-era-nvidia-gtc) .


### About the author


Waverly Swinton


Author


[See author profile](https://www.digitalocean.com/community/users/wdolaman)


[See author profile](https://www.digitalocean.com/community/users/wdolaman)


Share


- [Ai Ml](https://www.digitalocean.com/blog/tags/ai-ml)
- [Product Updates](https://www.digitalocean.com/blog/tags/product-updates)


### Connect with our sales team


Connect with us to learn more about how you can operate inference systems with performance, reliability, and predictable economics at scale.


[Contact sales](https://www.digitalocean.com/company/contact/sales)
