---
schema_version: "1.0.0"
document_id: "f9bf6fe2049fdd21c6eaa85d3e36084890108aa9ac2aa391e7da73eeaf7a0dc5"
company_key: "yc-cerebrium"
company: "Cerebrium"
source_id: "yc-cerebrium-news-import-eb465eef12b4"
canonical_url: "https://cerebrium.ai/blog/how-distillabs-is-delivering-50percent-lower-inference-costs-with-production-grad"
published_at: "2026-02-28T19:52:48+00:00"
first_seen_at: "2026-07-21T13:06:54.599340+00:00"
fetched_at: "2026-07-28T21:57:38.048658+00:00"
content_hash: "sha256:d9da6dc992f16a4a99bbc01d6aeb2bfeb99fea319390f100ac15abcc43a46a15"
---

# How DistilLabs is Delivering 50% Lower Inference Costs with Production-Grade Autoscaling on Cerebrium

How DistilLabs is Delivering 50% Lower Inference Costs with Production-Grade Autoscaling on Cerebrium


### Introduction


[Distil Labs](https://www.distillabs.ai/) is the developer platform for building task-specific SLMs with LLM-level accuracy. Companies train models on Distil Labs in a matter of hours, replacing expensive large language models with smaller, faster, and dramatically more cost-efficient models. This enables their customers to preserve output quality, while significantly reducing inference costs and latency.


In production, their workloads span both training and inference with usage patterns fluctuating from low, steady traffic to bursts reaching hundreds of requests per second per customer. For Distil Labs, maintaining steady inference operations through bursts is not optional - it’s paramount.


### The Challenge


As Distil Labs refined its product, it became clear that model optimizations alone would not unlock sustainable unit economics. Infrastructure would be integral to enable cheaper, faster inference at scale. And managing sophisticated hyperscale-grade GPU infrastructure internally would require significant engineering effort - effort that would pull the team away from building their core product.


Their system needed to dynamically scale from zero to hundreds of requests in under a minute. It needed to support fully custom model deployments, including custom weights and API definitions. It needed access to a variety of GPU types, be distributed across regions globally enabling data sovereignty. And most critically, it needed fast cold starts and cost efficiencies at scale.


Distil Labs quickly realized solving their infrastructure challenges, meant partnering with the right provider.


### Evaluation & Implementation


After reviewing a multitude of vendors, including both cloud and on-prem solutions, Cerebrium was selected as the key partnership, combining key requirements into one wholistic platform solution.


-


Dedicated AI Onboarding Engineer


-


Granular Deployment Controls


-


Configurable Autoscaling based on traffic patterns


-


Deploy across regions globally


-


Optimized cold starts


-


Competitive On Demand Pricing


The onboarding process was highly collaborative. Within the first week, Cerebrium enabled Distil Labs to programmatically spin deployments up and down per customer request while monitoring usage at a granular level per deployment and meeting all their infrastructure needs.


### The Results


Instead of investing development cycles into GPU orchestration, autoscaling logic, and reliability engineering, Distil Labs was able to immediately concentrate on their core USP – improving models and delivering customer value instead of maintaining infrastructure.


Dynamic scaling at both the deployment and per-instance level became essential to their roadmap. Without the ability to spin instances up and down easily - or to create multiple deployments as needed - they would not have been able to as easily offer competitive pricing. For Distil Labs, infrastructure wasn’t just a backend decision - it was the key to making efficient, small-model inference viable at scale.


Today, Distil Labs runs up to 150 requests per second per model during high-traffic periods, with multiple models deployed concurrently. Autoscaling handles traffic spikes smoothly, and reliability has reached production-grade stability. Upcoming GPU memory snapshot improvements promise even faster cold starts, further strengthening performance under bursty demand.


Beyond the platform itself, the working relationship has been a differentiator. The Cerebrium team integrates directly into Distil Labs’ Slack and remains highly responsive, acting less like a vendor and more like an infrastructure partner.


> *Our customer was running at single-digit millions of requests per day at ~1s p99 latency. With Cerebrium, we reduced their inference costs by 50%, and increased accuracy from 83% to 92%, while keeping latency and reliability consistent at production scale.*
