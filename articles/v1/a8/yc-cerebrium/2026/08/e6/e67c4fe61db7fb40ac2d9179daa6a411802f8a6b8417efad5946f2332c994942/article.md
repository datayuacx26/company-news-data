---
schema_version: "1.0.0"
document_id: "e67c4fe61db7fb40ac2d9179daa6a411802f8a6b8417efad5946f2332c994942"
company_key: "yc-cerebrium"
company: "Cerebrium"
source_id: "yc-cerebrium-news-import-eb465eef12b4"
canonical_url: "https://cerebrium.ai/blog/scaling-resemble-ais-real-time-deepfake-models-with-cerebrium"
published_at: "2026-08-13T14:37:43+00:00"
first_seen_at: "2026-08-20T03:11:50.289453+00:00"
fetched_at: "2026-08-20T03:11:51.847201+00:00"
content_hash: "sha256:a9338574f6c392d80ed99d11744547e28a7d79a76be1d0941ca2e0da87d4995b"
---

# Scaling Resemble AI’s Real-Time Deepfake Detection Models with Cerebrium

Scaling Resemble AI’s Real-Time Deepfake Detection Models with Cerebrium


## Introduction


Resemble AI is trying to answer one of the hardest questions on the internet in under 300 milliseconds: is this real?


Its[deepfake detection models](https://www.resemble.ai/products/detect) analyze audio, images, and video inside live workflows such as phone calls and identity verification, where identifying a fake after the interaction ends can already be too late.


Resemble AI understands that problem from both sides. Its researchers have spent years building and studying generative technology, including the company’s open-source[Chatterbox voice models](https://huggingface.co/ResembleAI/chatterbox) , which have been downloaded more than 17 million times. That experience gives the team a close view into how synthetic media is created, how generation techniques evolve, and where detection systems can fail.


Its researchers have shown, for example, that something as simple as playing synthetic speech through a physical speaker and re-recording it can significantly degrade conventional deepfake detectors. As new attack techniques emerge and expand through channels such as image and video, the team has to turn those discoveries into production detection models quickly.


Doing that requires infrastructure capable of serving a growing portfolio of detection models with[different hardware requirements](https://cerebrium.ai/docs/toml-reference/toml-reference) and traffic patterns, at[real-time latency](https://cerebrium.ai/use-cases/voice) and across regions.


That’s where Cerebrium comes in.


## The Challenge


Resemble AI’s detection models sit directly inside production workflows, from live voice calls to image identity verification and fraud detection. Infrastructure performance isn’t simply an optimization; it directly affects the product customers experience.


The team had previously been running workloads with another infrastructure provider but began running into limitations as it scaled:


1.


**Cold starts weren’t fast enough for real-time use cases.** For detection running during a live interaction, waiting for infrastructure to become available can make the product unusable.


2.


**Reliability needed to match the importance of the workloads.** Resemble AI serves enterprises and government organizations where an unavailable detection endpoint can interrupt a critical customer workflow.


3.


**Different detection models required different infrastructure.** Deepfake detection spans multiple modalities and model architectures, from audio to image and video analysis. Resemble AI needed to deploy and scale those models without designing a separate infrastructure stack for each one.


4.


**Customers increasingly required regional deployment and data residency.** Supporting enterprises in new markets meant being able to run workloads in specific regions rather than routing every customer through the same infrastructure footprint.


Resemble AI needed a platform that could solve all of these problems without turning its research team into an infrastructure team.


## Why Cerebrium


After hearing positive feedback from other real-time companies like[Tavus](https://cerebrium.ai/blog/how-tavus-scaled-human-like-ai-experiences-with-cerebrium) , SuperDial, and LiveKit, Resemble AI decided to explore an infrastructure partnership with Cerebrium.


Serverless GPU infrastructure allows[capacity to expand with demand](https://cerebrium.ai/docs/scaling/scaling-apps) rather than requiring Resemble AI to permanently provision for peak traffic. That matters when a large verification batch, customer test, or security incident can cause traffic to increase suddenly.


[Fast cold starts](https://cerebrium.ai/blog/reducing-gpu-cold-starts-with-memory-snapshots-restoring-cuda-workloads-in-second) make the same infrastructure suitable for[latency-sensitive](https://cerebrium.ai/use-cases/image-and-video) , real-time detection endpoints. Cerebrium’s multi-region infrastructure and failover capabilities also give Resemble AI the reliability and geographic flexibility it needs. Workloads can be deployed closer to customers, while regional deployment options help Resemble AI satisfy data-residency requirements and pursue deals in markets that would otherwise be difficult to serve.


Importantly, Resemble AI gets those capabilities without sacrificing developer experience. Researchers can take a new detection model from experimentation to a production endpoint without managing the underlying GPU fleet, autoscaling infrastructure, or[regional deployment architecture](https://cerebrium.ai/docs/deployments/multi-region-deployment) themselves.


The integration itself was straightforward: Resemble AI could bring its existing Dockerfile, define the[deployment configuration in code](https://cerebrium.ai/docs/container-images/defining-container-images) , and fit Cerebrium into its existing[CI/CD pipelines](https://cerebrium.ai/docs/deployments/ci-cd) with minimal additional lift. From there, the team can deploy different detection architectures while configuring the hardware and scaling behavior around the needs of each workload, without being tied to a single cloud provider as Cerebrium integrates its platform with cloud networks around the world.


The Cerebrium team also works directly with Resemble AI as new infrastructure requirements emerge.


> *We talk directly with people who understand our platform needs and infrastructure deeply, rather than routing through a support tier. For a team our size, that is effectively a key extension of our own engineering capacity.*


## The Results


Since moving its detection workloads to Cerebrium, Resemble AI has been able to spend more of its engineering effort on the models themselves rather than the infrastructure surrounding them.


That has helped the team build an unusually fast research-to-production loop. Resemble AI now releases updated detection models weekly and sometimes multiple times per week. The company estimates that this is **30–100x more frequent** than the quarterly or annual model refresh cycles common among other detection vendors.


That speed matters in an adversarial field. Every major improvement in generative media creates new artifacts for Resemble AI’s researchers to study. The faster the team can identify those signals, retrain its models, and deploy them into customer-facing APIs, the faster customers can be protected against the newest generation of synthetic content.


Cerebrium also gives Resemble AI a common infrastructure layer across its detection portfolio, including deepfake detection, watermarking, and identity-related workloads. Different models can run on the hardware and scaling configurations that fit them best without requiring the team to build and maintain separate infrastructure for each one.


Multi-region deployment also gives Resemble AI the ability to support customers across the US, UK, and EU, helping it serve the data-residency requirements of the different markets in which it operates.


As Resemble AI expands deepfake detection across more customers, modalities, and regions, its infrastructure needs to keep pace with a growing range of models, deployment environments, and latency requirements. Cerebrium gives the team the flexibility to serve those detection models reliably across regions while continuing to iterate quickly.


Resemble AI is building the intelligence layer for detecting synthetic media. Cerebrium gives it the infrastructure to ship it.
