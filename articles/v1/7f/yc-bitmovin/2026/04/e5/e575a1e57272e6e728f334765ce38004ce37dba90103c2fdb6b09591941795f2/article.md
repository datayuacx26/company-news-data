---
schema_version: "1.0.0"
document_id: "e575a1e57272e6e728f334765ce38004ce37dba90103c2fdb6b09591941795f2"
company_key: "yc-bitmovin"
company: "Bitmovin"
source_id: "yc-bitmovin-news-import-596f48821b52"
canonical_url: "https://bitmovin.com/blog/hackathon-live-encoding-sovereign-cloud/"
published_at: "2026-04-08T08:33:04+00:00"
first_seen_at: "2026-07-27T08:05:57.697851+00:00"
fetched_at: "2026-07-27T21:00:56.489221+00:00"
content_hash: "sha256:91c4412d4745cc2d89c9c6a5aa2e72ef93fea4cdc4c2691bdcd2d7e2737f59f1"
---

# Hackathon spotlight: live encoding on a sovereign cloud with Scaleway

## TL;DR


- Broadcasters and OTT platforms operating under strict data residency and regional compliance requirements are actively looking beyond hyperscalers for live encoding infrastructure.
- During a hackathon project, Bitmovin’s Live Encoder was deployed on Scaleway’s sovereign European cloud (Paris region), running a full production-grade live encoding pipeline without modifying the encoding workflow.
- The encoding workflow is cloud-agnostic by design. The same API-driven orchestration that manages encoding lifecycle on hyperscalers worked identically on Scaleway, confirming that Bitmovin’s Live Encoder treats infrastructure as interchangeable.
- The experiment validated three concrete use cases: region-specific compliance pipelines, encoding closer to controlled infrastructure, and hyperscaler alternatives.


---


## Table of Contents


For many streaming teams, where live encoding runs is no longer just a deployment choice, it is a constraint driven by data residency, compliance, and infrastructure control. Hyperscaler environments do not always meet these requirements, particularly for broadcasters and OTT platforms operating across regions with strict data governance policies. As a result, teams are starting to evaluate alternative cloud environments that offer greater control over where and how workloads are deployed.


For this hackathon project, we deployed Bitmovin’s Live Encoder on a sovereign European cloud environment provided by Scaleway to validate what it takes to run a production-grade live encoding workflow outside of traditional hyperscaler environments. And honestly, it turned out to be a genuinely fun problem to solve.


In this blog, we walk through the setup, the architecture, and what this experiment reveals about the future of live streaming infrastructure.


## **What we wanted to test**


Bitmovin’s Live Encoder is built to run across different cloud environments, but in practice, most deployments still run on hyperscalers. While these environments provide scale and availability, they are not always aligned with requirements around data residency, infrastructure control, or regional compliance.


For this project, we wanted to test how portable that workflow really is, specifically whether a full live encoding pipeline could run in a sovereign European cloud environment without changing the encoding setup or introducing additional complexity. This meant treating the infrastructure as interchangeable, rather than something the workflow depends on, and validating that ingest, encoding, and packaging could run end to end using the same API-driven orchestration.


## **The setup**


The workflow itself remained unchanged. Live streams were ingested via RTMP and SRT into Bitmovin’s Live Encoder, transcoded in real time into multiple renditions (H.264/HEVC), and packaged into adaptive HLS and DASH outputs for playback.


What changed was the infrastructure layer.


Instead of running on a hyperscaler, the full pipeline was deployed on compute instances provided by Scaleway in their Paris region, with storage acting as the origin for downstream delivery.


At a high level, the setup looked like this:


- **Ingest:** RTMP and SRT streams into the Live Encoder
- **Encoding:** Real-time transcoding into multiple renditions (H.264/HEVC)
- **Packaging:** HLS and MPEG-DASH outputs with segmented fMP4
- **Infrastructure:** Scaleway compute instances (Paris region)
- **Storage:** Origin layer for serving encoded segments and manifests
- **Control plane:** Bitmovin API managing the encoding lifecycle


From an engineering perspective, the constraint was simple. Keep the workflow consistent, and see if the infrastructure could support it without introducing additional complexity.


## **What made the difference**


One of the more unexpected aspects of this project was how much the infrastructure support influenced the outcome. In a hackathon setting, where time is limited and iteration speed matters, even small blockers can slow everything down.


Instead of relying on documentation or waiting on slower support loops, we were able to work directly with the team at Scaleway to shape the environment as we went. That meant we could adjust configurations in real time, while the encoding pipeline was running, rather than stopping to troubleshoot or rework the setup.


That shift made the difference. We spent our time validating the workflow, not working around the infrastructure.


## **Why this matters**


What this project proved is simple, the encoding workflow itself does not need to change to run in a sovereign cloud environment.


For teams operating under data residency or compliance requirements, that removes a major barrier. Instead of redesigning workflows or introducing additional layers, the same pipeline can be deployed in an environment that meets regional or regulatory constraints.


This also changes how infrastructure decisions can be made. Rather than defaulting to hyperscalers, teams can choose environments based on specific requirements, whether that is location, control, or cost, without impacting how the encoding workflow operates.


In practice, this opens up a few clear use cases:


- Deploying region-specific pipelines to meet local compliance requirements
- Running encoding closer to controlled infrastructure environments
- Expanding beyond hyperscalers without increasing operational complexity


## **Final thoughts**


Hackathons are usually about testing ideas quickly, but the useful ones answer real questions.


This one did.


We set out to see if the workflow would hold up in a different environment, and it did, without needing to change how it was built or operated, which made it worth exploring further and gave us a clear signal that the approach holds up beyond just a hackathon setup.


And honestly, it was a fun one to work through.


---


## FAQs


### What is sovereign cloud live encoding?


Sovereign cloud live encoding means running video encoding workloads on cloud infrastructure that keeps data within a specific jurisdiction, under local legal and regulatory control. For broadcasters and OTT platforms subject to data residency laws (such as GDPR in Europe), this ensures that live stream data never leaves a defined geographic or legal boundary.


### Can Bitmovin’s Live Encoder run on a sovereign European cloud?


Yes. Bitmovin’s Live Encoder was successfully deployed on Scaleway, a sovereign European cloud provider, running a full live encoding pipeline in the Paris region. The workflow, including RTMP/SRT ingest, multi-rendition transcoding, and HLS/DASH packaging, ran without any changes to the encoding setup.


### Does switching to a sovereign cloud require changing the live encoding workflow?


Bitmovin’s hackathon demonstrated that the encoding workflow itself does not need to change when moving to a sovereign cloud environment. The same API-driven pipeline runs identically regardless of the underlying infrastructure provider.


### What protocols and formats does Bitmovin’s Live Encoder support on sovereign cloud deployments?


Bitmovin’s Live Encoder supports RTMP and SRT ingest, real-time transcoding into multiple renditions using H.264 and HEVC, and output packaging into adaptive HLS and MPEG-DASH with segmented fMP4. These capabilities are fully available when deployed on sovereign cloud infrastructure such as Scaleway.


### How does Bitmovin’s Cloud Connect feature relate to sovereign cloud deployments?


Bitmovin’s[Cloud Connect](https://bitmovin.com/encoding-service/cloud-connect) capability is designed to let teams dynamically scale encoding worker nodes within their own cloud environment, including non-hyperscaler environments. This makes it a natural fit for sovereign cloud deployments where the customer needs control over where compute runs.
