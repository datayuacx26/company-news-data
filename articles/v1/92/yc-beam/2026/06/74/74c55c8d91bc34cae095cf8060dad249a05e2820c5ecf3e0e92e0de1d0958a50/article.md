---
schema_version: "1.0.0"
document_id: "74c55c8d91bc34cae095cf8060dad249a05e2820c5ecf3e0e92e0de1d0958a50"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/nvidia-b200-pricing"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-22T21:28:59.137691+00:00"
fetched_at: "2026-07-28T21:23:18.688239+00:00"
content_hash: "sha256:3784f477262feebfc8cfc93a7a8747318faf9772b4507cbc9027b0dcc79250ff"
---

# NVIDIA B200 Pricing (June 2026)

[← All posts](https://www.beam.cloud/blog) Engineering


# NVIDIA B200 Pricing (June 2026)


Nathanael Chiang


June 18, 2026


2 min read


The NVIDIA B200 is the flagship of the Blackwell generation: 192 GB of HBM3e, 8 TB/s of memory bandwidth, and native FP4 support that roughly doubles inference throughput over FP8. Every team running frontier-scale models wants one. It also has the most fragmented pricing in the market right now.


The breakdown below compares current B200[on-demand pricing](https://www.beam.cloud/blog/nvidia-rtx-a6000-pricing) . Supply is still catching up to demand, so the spread between neoclouds and hyperscalers is wide, and that spread is where the savings sit.


## Takeaways


- Neoclouds list B200 on-demand at $3.70 to $6.00/hr; hyperscalers charge 3 to 4x more.
- AWS p6-b200 lands near $14.24/hr per GPU, the most expensive tier in the market.
- Spot pricing drops as low as $2.12 to $2.74/hr for checkpoint-friendly workloads.
- Beam starts at $3.93/hr, at the floor of the on-demand market, on a serverless runtime rather than a raw VM.


## Current on-demand B200 prices (per GPU)


Provider SKU / Instance On-Demand $/GPU-hr* Notes


Beam B200 SXM6 $3.93 Serverless runtime, per-second billing


Spheron B200 SXM6 $3.70 Marketplace floor; spot ~$2.74


Lambda 1x B200 $4.99 8x node at $5.29/GPU


Nebius B200 SXM6 $5.50 Per-hour billing, no spot


RunPod B200 SXM6 (Secure) $5.89 Per-minute billing


CoreWeave N/A ~$6.50 Contract-oriented, estimated


Azure ND B200 v6 ~$14.00 Full VM shape


AWS p6-b200 (8x B200) $14.24 $113.93/hr node / 8 GPUs


*Normalized to a single B200, even when only multi-GPU nodes are offered.*


*Last update: June 18, 2026*


## Methodology


Why you can trust these numbers:


- On-demand only: no reservations or prepaid contracts.
- Same silicon: every figure refers to the NVIDIA B200 (192 GB) SKU.
- Public price lists: pulled in June 2026 from each provider's published pricing pages.
- U.S. regions, USD: prices elsewhere vary by 5 to 20%.


## Why the B200 spread is so wide


The B200 rollout is still underway, which is why pricing has not settled the way H100 pricing eventually did. MSRP runs $30,000 to $40,000 per GPU in clusters of 8 or more, each card draws about 1,000W, and most hyperscaler capacity is committed to enterprise contracts. On-demand buyers compete for the slice neoclouds make available, and the neoclouds, with leaner overhead, undercut the hyperscalers by three or four times.


The 1-year reserved discount on B200 is also the steepest in the market, roughly 39% off posted on-demand, because providers compete hardest for committed Blackwell capacity.


## Where Beam fits


Beam's B200 SXM6 starts at $3.93/hr, at the bottom of the on-demand range, level with the cheapest marketplace floors and about 3.6x cheaper than AWS p6. The difference is what the rate includes. Rather than a bare VM you manage yourself, Beam runs your workload on a serverless runtime with per-second billing, so you do not pay for idle GPU time between jobs. For burst inference and intermittent training, that scale-to-zero behavior often matters more than the hourly number.


## Why this matters for developers


- Memory you can use: 192 GB HBM3e plus FP4 fits 70B+ models on a single GPU with room for large batch sizes.
- The hyperscaler premium is large: AWS and Azure B200 instances cost 3 to 4x the neocloud rate for the same silicon, before egress and networking.
- Spot is the biggest lever: if your workload checkpoints, spot B200 around $2.12/hr leads on cost per token.


We refresh these numbers monthly.


Nathanael Chiang


Published June 18, 2026


Keep Reading


## More from the Beam blog


[Engineering Tinker Model Pricing: What Fine-Tuning Costs in 2026 See what fine-tuning costs on Tinker, including worked cost examples and where renting GPUs gets cheaper. Tim Huynh](https://www.beam.cloud/blog/tinker-model-pricing)[Engineering What Is a Container, Really? Five Years of GPU Infrastructure Five years of GPU infrastructure at Beam — from ECS and Knative cold starts to a custom container runtime, FUSE lazy-loading, and a trustless binary. Luke Lombardi](https://www.beam.cloud/blog/what-is-a-container-really)


$30 free credit


refreshed monthly


## Start shipping on infra
you won’t outgrow.


Run sandboxes and GPU workloads on your cloud, and scale out to ours when you need to. No infra to manage.


[Start Building](https://platform.beam.cloud/)[Read the docs](https://docs.beam.cloud/)
