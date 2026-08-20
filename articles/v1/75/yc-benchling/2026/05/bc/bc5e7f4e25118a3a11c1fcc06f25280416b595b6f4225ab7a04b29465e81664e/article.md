---
schema_version: "1.0.0"
document_id: "bc5e7f4e25118a3a11c1fcc06f25280416b595b6f4225ab7a04b29465e81664e"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/announcing-benchling-inference-powered-by-baseten"
published_at: "2026-05-20T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:5c5a7dc3012daed1748721e40a2b3cd303139fc7b9ea69052361b194ed56b9b9"
---

# Announcing Benchling Inference powered by Baseten: Scalable GPUs for scientific AI

Today we are launching Benchling Inference, powered by[Baseten](https://www.baseten.co/) . Benchling Inference provides scalable and cost-effective GPU capacity to train and run scientific models, without managing infrastructure. It comes preloaded with today's top scientific models and the integrations to make in silico discovery work out-of-the-box for biopharma companies.


We frequently hear that scientific model workloads are too bursty for traditional compute. You wait on the physical lab, data comes in waves, then you need to run 100,000 predictions in a few hours before going quiet for days. Cost-effective capacity that scales up fast and also scales to zero is difficult to acquire and maintain.


We've been running Baseten internally for Benchling's own[Model Hub](https://www.benchling.com/blog/introducing-benchling-model-hub) . We’ve learned a lot about tailoring inference for drug discovery and now we want customers to have the same access.


## General-purpose inference wasn't designed for scientific workloads


The largest hyperscalers are on track to spend nearly[$700 billion](https://fortune.com/2026/04/30/big-tech-hyperscalers-will-spend-700-billion-on-ai-infrastructure-this-year-with-no-clear-end-in-sight-eye-on-ai/) on AI infrastructure in 2026. Most of that is optimized for LLM workloads with reserved compute and predictable demand.


There’s been an explosion in scientific models. Between 2020 and 2025, the number of new biology AI models released each year grew from[28 to more than 380](https://www.bvp.com/atlas/building-biology-native-data-infrastructure-for-the-ai-era) . The challenge for most computational biology teams: HPC queues with multi-week backlogs, sending spreadsheets back and forth with wet lab researchers, large GPU reservations for jobs that run a fraction of the time, and predictions rationed during active campaigns. Protein structure prediction, molecular docking, and sequence analysis all have different cold start characteristics, memory requirements, and throughput patterns.


The market is also split. Biotech startups can't get infrastructure at prices that work for them. Large pharma has invested heavily in HPC clusters, cloud commits, and computational teams, and new infrastructure has to work alongside that.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## How Benchling Inference works


Baseten has spent six years building at the leading edge of inference infrastructure: making it reliable and economical to run specialized models in production, across 15+ cloud providers, with cold starts in 5–10 seconds. The foundation is the[Baseten Inference Stack](https://www.baseten.co/resources/guide/the-baseten-inference-stack/) , a tightly integrated combination of a high-performance runtime (custom kernels, speculative decoding, KV cache optimizations) and inference-optimized infrastructure (intelligent request routing, autoscaling, active-active reliability across clouds). Every model deployed on Baseten seamlessly inherits these optimizations.


Benchling Inference combines Baseten with the scientific configuration and integrations that make inference work out-of-the-box for biopharma. By aggregating demand, we are also bringing better economics to biotech startups.


Scientists can either deploy third-party models or serve internal models built on their data from a unified compute environment. For teams with data sovereignty requirements, the Baseten Inference Stack runs identically in Baseten Cloud, your virtual private cloud (VPC), or a hybrid of both so predictions never have to leave your environment. Benchling also supports[NVIDIA NIM microservices](https://www.benchling.com/news/benchling-powers-ai-model-access-for-scientists-with-nvidia) for additional GPU acceleration on specific models and algorithms. Computational scientists working in Jupyter notebooks or via SDK can call inference directly through Benchling.


Benchling Inference is available now. Sign up for access[here](https://airtable.com/appd15HVMCC04bBE4/pag7cPkQ1fQDaJFmC/form) .
