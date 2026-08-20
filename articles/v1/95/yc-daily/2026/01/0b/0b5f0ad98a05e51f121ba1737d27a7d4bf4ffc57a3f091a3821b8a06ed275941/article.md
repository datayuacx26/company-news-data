---
schema_version: "1.0.0"
document_id: "0b5f0ad98a05e51f121ba1737d27a7d4bf4ffc57a3f091a3821b8a06ed275941"
company_key: "yc-daily"
company: "Daily"
source_id: "yc-daily-rss-fbcbe287eccb"
canonical_url: "https://www.daily.co/blog/pipecat-cloud-is-now-generally-available/"
published_at: "2026-01-08T21:50:17+00:00"
first_seen_at: "2026-07-20T23:23:39.254949+00:00"
fetched_at: "2026-07-28T22:23:45.657833+00:00"
content_hash: "sha256:2d082d2367dbd2d9b053fe401c86718fd00ca86da9a9a48bea49d3d66974bf31"
---

# Pipecat Cloud is Now Generally Available

## Deploy and scale your Pipecat agents on enterprise-grade infrastructure


[Pipecat Cloud](https://docs.pipecat.ai/deployment/pipecat-cloud/introduction) is now GA, following an nine-month beta period with more than 1,000 teams building and scaling voice agents on Pipecat Cloud’s global infrastructure.


We built Pipecat Cloud to handle the low-level operational and scaling challenges of voice AI so that teams building enterprise voice agents can focus on agent code and business logic. Engineering and product teams rely on Pipecat Cloud for auto-scaling, multi-region deployments, world-class redundancy and resilience, and compliance and data security, all while avoiding any vendor lock-in.


Pipecat Cloud supports direct connections to telephony providers like Twilio and bundles value-added services like the industry-leading[Krisp](https://krisp.ai/) VIVA noise reduction models and Daily WebRTC transport.


With the help of our developer community and their feedback, today Pipecat Cloud is powering voice AI across use cases like agentic interviewers; enterprise healthcare workflows like patient intake and schedule reminders; embedded hardware platforms; and more.


# Build on open source, “docker push” to Pipecat Cloud


Pipecat Cloud is the managed service for[Pipecat](https://docs.pipecat.ai/getting-started/introduction) , the most widely used voice agents and multimodal AI framework.


Pipecat’s architecture is built around a programmable, AI-native multimodal pipeline. It’s fully open source, and composable, to support how engineers build and enterprises preserve strategic value: use any model and easily swap them out; integrate with any data store; connect to AI-native observability and eval tooling; run on any transport; leverage cross-platform libraries.


But voice AI developers also face a second challenge, scaling voice agent infrastructure. Deploying at scale with production reliability involves complexities like configuring optimal network routing, implementing rolling deploys with connection-aware drain times, avoiding cold starts, managing long-running connections, allocating CPU efficiently in Kubernetes, and more. (See Section 10 of the Voice AI & Voice Agents: An Illustrated Primer.)


Pipecat Cloud is built by Daily, and reflects our 10 years of experience building the world’s leading global realtime developer infrastructure. Our platforms and tooling are trusted by industry leaders like NVIDIA, Mercor, Descript, Epic, Vapi, and Tavus.


With Pipecat Cloud, you build your voice agent leveraging Pipecat’s open source core, add your custom code, and then “docker push” to Pipecat Cloud.


Pipecat is vendor neutral by design, and in designing Pipecat Cloud we followed the Pipecat principles that flexibility and avoiding lock-in are key values.


- The code you deploy to Pipecat Cloud is “just” Pipecat code. Anything you run on Pipecat Cloud you can self-host exactly the same way. All of the deployment code is open source and the Pipecat Cloud lifecycle events are fully documented.
- Pipecat Cloud leverages Daily’s global infrastructure and includes Daily WebRTC at no additional cost, but Pipecat Cloud also supports direct connections to telephony providers, WebSocket network transport, and the non-commercial peer-to-peer SmallWebRTCTransport module.


# Engineering enterprise-grade service


Over the past nine months, our engineering team has focused on:


**Fast agent start times** :


- P99 agent start times are < 1 second
- We automatically over-provision as your scale increases and we give you control over how many “reserved instances” you want to keep alive during low-traffic periods. A reserved instance is 1/20th the cost of an active instance.


**Multi-region hosting:** host voice agents where your users are


- us-west (Oregon)
- us-east (Virginia)
- eu-central (Frankfurt)
- ap-south (Mumbai)


**Delivering features that help your agents succeed**


- Krisp VIVA noise cancellation
- Smart Turn model access - native audio turn detection, with open weights and open datasets.
- Agent profiles for use cases like video avatars and screen sharing that need more CPU
- Observability for usage and performance metrics


**Network transport flexibility:** You can configure your agents for direct client connections using WebRTC, WhatsApp, Twilio, and more.


- [Daily](https://docs.pipecat.ai/deployment/pipecat-cloud/guides/daily-webrtc) (WebRTC &[PSTN](https://docs.pipecat.ai/deployment/pipecat-cloud/guides/telephony/daily-dial-in) ) — Daily is recognized as the top WebRTC developer platform by third-party analysts like Tsahi Levent-Levi. You can use Daily WebRTC and buy phone numbers for telephony connections directly from Daily.
- [SmallWebRTC](https://github.com/pipecat-ai/pipecat-examples/tree/main/p2p-webrtc/pipecat-cloud) — a direct peer-to-peer transport that is particularly useful if you have a regulatory or security requirement not to route traffic through WebRTC servers.
- [WhatsApp](https://docs.pipecat.ai/deployment/pipecat-cloud/guides/whatsapp)
- [Twilio](https://docs.pipecat.ai/deployment/pipecat-cloud/guides/telephony/twilio-websocket)
- [Telnyx](https://docs.pipecat.ai/deployment/pipecat-cloud/guides/telephony/telnyx-websocket)
- [Plivo](https://docs.pipecat.ai/deployment/pipecat-cloud/guides/telephony/plivo-websocket)
- [Exotel](https://docs.pipecat.ai/deployment/pipecat-cloud/guides/telephony/exotel-websocket)


**Reliability:** Kubernetes redundancy, logging, and observability


**Improving the developer experience and supporting automation** : Use the Pipecat Cloud REST API to[set up CI/CD workflows](https://docs.pipecat.ai/deployment/pipecat-cloud/guides/ci-with-github-actions) that automatically deploy updates to your agent.


**HIPAA:** Adding compliance enablement for[HIPAA](https://docs.pipecat.ai/deployment/pipecat-cloud/security/hipaa) workflows, plus advanced privacy and security controls relating to SOC 2 and other certifications.


Our roadmap ahead includes further support for enterprise scale, including expanded regions and SOC 2 for Pipecat Cloud. (Daily’s WebRTC infrastructure is SOC 2 compliant.)


**Single-tenant enterprise Pipecat Cloud:**[Contact us](https://www.daily.co/company/contact) if you need to run Pipecat Cloud in your VPC.


# Transparent pricing


Pipecat Cloud pricing is simple: $0.01 per running agent. You can add on, as needed, reserved instances, audio recording, and enterprise support. For enterprise customers, we can also bundle your AI inference costs (transcription, LLM, and voice models) into a single bill.


Our[Capacity Planning Guide](https://docs.pipecat.ai/deployment/pipecat-cloud/guides/capacity-planning) walks you through how to budget for active agents and reserved agents, as your scale increases.


# Enabling developers, supporting realtime AI


The mission behind our work at Daily is to support the development of voice and multimodal AI, from enterprise voice agents to new use cases like the[robot personal assistant demo](https://www.linkedin.com/posts/kwkramer_this-robot-assistant-from-the-nvidia-ces-activity-7414510205469786112-dkEM/) that opened the NVIDIA CES Keynote this year.


Pipecat is a vendor-neutral, open source framework that started life inside Daily as our internal tooling for realtime, conversational AI. Pipecat is now used by thousands of startups, scale-ups and enterprises, all of the foundation AI labs, and technology giants like NVIDIA and AWS.


Pipecat Cloud is the hosting platform we designed to solve infrastructure pain points we were hearing about from many or our customers and partners.


To talk with other developers building on the frontier of realtime AI, join the[Pipecat Discord](https://discord.gg/pipecat) .


If you’re new to voice agents, you can find the Pipecat quickstart[here](https://docs.pipecat.ai/getting-started/quickstart) . Thanks again to the Pipecat developer community and all of the engineers who have contributed to Pipecat.


#### Never miss a story


Get the latest direct to your inbox.
