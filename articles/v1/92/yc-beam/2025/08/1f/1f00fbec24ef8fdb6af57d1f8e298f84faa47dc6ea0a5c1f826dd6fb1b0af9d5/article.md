---
schema_version: "1.0.0"
document_id: "1f00fbec24ef8fdb6af57d1f8e298f84faa47dc6ea0a5c1f826dd6fb1b0af9d5"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/top-serverless-gpu-providers"
published_at: "2025-08-15T02:34:34.251+00:00"
first_seen_at: "2026-07-22T21:28:59.137691+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:6fb6b11bc0f1d444d3c9bacb0349f51cc4fc383920123b08d15c76b07feaba01"
---

# The Top Serverless GPU Providers in 2025, Ranked by Cold Start

[← All posts](https://www.beam.cloud/blog) Company


Tutorials


# The Top Serverless GPU Providers in 2025, Ranked by Cold Start


Eli Mernit


August 15, 2025


4 min read


With the explosion of AI, a new class of cloud providers have emerged to offer on-demand GPU workloads for startups, indie hackers, and enterprises. These platforms are increasingly used for generative AI, ranging from image diffusion models to LLMs.


Today, a number of cloud providers offer serverless GPUs. In 2025, the top platforms differ not only in pricing and GPU types, but in **cold start performance –** a key factor for the UX of latency-sensitive applications. Cold start latency occurs when models need to be initially loaded before providing inference services.


In this article, we compare the top five serverless GPU providers - **Beam** , **RunPod** , **Google Cloud Run** , **Baseten** , and **Replicate** - ranked by their cold start speeds.


**Methodology**


Cold start times were measured across several model use-cases:


- Medium LLM: LLaMA 3 8B
- Small LLM: GPT-2
- Image Model: Flux, Stable Diffusion
- Audio Model: Whisper
- Headless Browser: Chromium + WebGL
- Generic Container: ffmpeg


We benchmarked these by creating an app for each provider and deploying it to their cloud. We ran 100 requests on each provider, spread across several days. We tried to run evaluations across the same GPU hardware and compute resources (CPU cores and memory), and succeeded.


We’ve based our evaluation on a number of metrics:


**Speed** : deployment time, cold boot latency, round-trip request time, and TTFT for LLMs.


**Cost** : cost per GPU hour, billing unit (seconds vs. minutes), charges for idle time


**Developer-experience** : Speed-to-deployment, learning curve, configuration required, feature completeness


**Reliability** : uptime, latency, network performance


## **Serverless GPU Benchmarking**


Serverless GPU providers vary based on a number of factors, but one of the most important is the cold start. Because serverless GPUs turn off automatically between requests, the speed to restart the server is a really important factor when choosing a provider. Below, we’ll share benchmarks among the different providers.


## **1.[Beam](https://beam.cloud/)**


**Cold Start:** ~2–3 seconds


**GPU Support:** T4, A10G, A100, H100, RTX 4090


**Free Tier:** $30 free credit per month


Beam is an open-source serverless platform for GPU workloads. It specializes in fast cold boots, with containers starting in <1 second.


Beam’s fast cold-start is made possible by its open-source container runtime,[beta9](https://github.com/beam-cloud/beta9) . Unlike other providers which are built on Docker, Beam runs a custom container runtime which lazy loads container images from a distributed cache.


In addition, Beam supports GPU checkpoint restore, which makes it possible to save a snapshot of the GPU process to avoid having to reload model weights in between cold boots.


In terms of developer experience, Beam provides a Python-native interface and a Python SDK. The entire process of building an app is Python; there are no YAML files or configuration required.


## **2.[RunPod](https://runpod.io/)**


**Cold Start:** 6–12 seconds


**GPU Support:** T4, L4, A100, H100, consumer RTX GPUs


**Free Tier:** No


RunPod provides on-demand GPU compute with a focus on containerized workflows. It supports a wide range of consumer and data center GPUs, and includes a FlashBoot feature to reduce startup times for active workloads. Developers deploy via Docker, giving flexibility but requiring more setup expertise.


Because of its Docker-based interface, RunPod can have a steeper learning curve than others.


**Pros:**


- Best pricing for GPU workloads
- Sub-second cold start with FlashBoot
- Bring your own container


**Cons:**


- Learning curve for setup
- No default free tier


## **3.[Google Cloud Run](https://cloud.google.com/run)**


**Cold Start:** 20-30 seconds


**GPU Support:** L4 hardware only (as of mid-2025)


**Free Tier:** No GPU credits; $300 for new accounts


Google Cloud Run is part of Google Cloud Platform and provides serverless container hosting with recent support for L4 GPUs. It integrates tightly with other GCP services, supports per-second billing, and automatically scales to zero when idle. GPU support is currently limited compared to specialized providers.


**Pros:**


- Easy integration into the existing GCP ecosystem
- True per-second billing
- Scales to zero automatically


**Cons:**


- Slower cold boot
- Limited GPU choices
- More setup required (knowledge of Docker needed)


## **4.[Baseten](https://baseten.co/)**


**Cold Start:** 16-60 seconds


**GPU Support:** T4, L4, A10G, A100, H100 (MIG supported) – wide range of hardware and GPU instances


**Free Tier:** Yes – Free deployment tier, pay-per-use


Baseten is a managed platform for deploying and scaling machine learning models. It offers a Python SDK, a packaging system called Truss, and built-in batching and autoscaling. Baseten supports multiple GPU types, including MIG instances.


**Pros:**


- Built-in autoscaling and batching with task queue support
- Easy model deployment with Truss


**Cons:**


- Slower cold starts
- Per-minute billing (less granular)


## **5.[Replicate](https://replicate.com/)**


**Cold Start:** Instant for public models; 60+ seconds for custom deployments


**GPU Support:** T4, A100, H100 (limited selection of gpu hardware and gpu instances)


**Free Tier:** Yes, only for public models


Replicate is a model hosting platform with a large public library of pre-trained models for tasks like image generation and transcription. Public models load instantly, while custom models run in private containers with longer cold starts. Replicate can help developers seeking no-code or low-code model deployment options.


**Pros:**


- Massive model library (no code to deploy)
- Easy to test models


**Cons:**


- Long cold starts for private models
- Idle time is billed on custom deployments


## Conclusion


If you’re building an AI product and need the best cold start performance with affordability and ease of use, **Beam** , **RunPod** , and **Baseten** stand out as the top choices for 2025.


*All three top performers (Beam, RunPod, Baseten) cost under $20/month for 100k 1-second inference calls on a T4 GPU.*


*Note* : All cold start times are based on benchmarks as of June 2025. Actual performance may vary depending on the app configuration and code used.


Eli Mernit


Published August 15, 2025


Keep Reading


## More from the Beam blog


[Tutorials Serverless GPU for Reinforcement Learning Fan out thousands of RL rollouts across serverless GPUs with one .map() call. Snapshot environments, scale to zero between updates, run on your own cloud. Tim Huynh](https://www.beam.cloud/blog/serverless-gpu-reinforcement-learning)[Tutorials Batch Inference on Serverless GPU Learn how to fan out batch inference across GPUs with one .map() call. Tim Huynh](https://www.beam.cloud/blog/batch-inference-serverless-gpu)


$30 free credit


refreshed monthly


## Start shipping on infra
you won’t outgrow.


Run sandboxes and GPU workloads on your cloud, and scale out to ours when you need to. No infra to manage.


[Start Building](https://platform.beam.cloud/)[Read the docs](https://docs.beam.cloud/)
