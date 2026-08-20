---
schema_version: "1.0.0"
document_id: "a563b3b51a91f593060ac977858a5803575f647a1d228783cb585543cd1745d5"
company_key: "yc-omnistrate"
company: "Omnistrate"
source_id: "yc-omnistrate-news-import-f408a10afc71"
canonical_url: "https://omnistrate.com/blog/omnistrate-platform-update-december-2025-january-2026"
published_at: "2026-02-01T00:00:00+00:00"
first_seen_at: "2026-07-22T07:13:55.392025+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:49411d3a0c8b4d628e6df45d8193f763be732386fc34d04e7c3d03c951e88730"
---

# Omnistrate Platform Update (December 2025/January 2026)

## 🚀 Exciting New Features in Omnistrate


### Helm Onboarding | Update


We've streamlined the onboarding process for Helm charts.[Affinity rules are now dynamically injected during import](https://docs.omnistrate.com/build-guides/helm-charts-customize/?h=injection#automatic-affinity-injection-default) , ensuring your services utilize Omnistrate-provisioned infrastructure with zero manual configuration. This update also includes enhanced, out-of-the-box observability for all deployments.


### Oracle Cloud Infrastructure (Preview) | Now Available


Deploy directly into your own OCI account, or to your customers OCI account with BYOC, with a single command. This expansion into OCI removes the need for complex deployment pipelines or manual infrastructure setup.


Whether you're targeting OCI for its high-performance compute or responding to specific customer demands, you can now scale without the typical engineering overhead. Omnistrate ensures your service remains identical across AWS, Azure, GCP, and now OCI.[Get in touch](https://www.omnistrate.com/contact) with our team if you'd like to start deploying on OCI.


### New Web Code Editor | Alpha Release


We listened to your feedback and built a browser-based editing experience that matches the speed and precision of your local IDE. The new **Web Code Editor** provides a high-fidelity environment for managing your service specifications, featuring advanced syntax highlighting, real-time validation, and a layout optimized for complex YAML configurations.


This update bridges the gap between local development and cloud management, ensuring your workflow remains uninterrupted. Whether you are refining a single resource or refactoring your entire service definition, the editor provides the guardrails and performance necessary for enterprise-grade infrastructure. This feature is currently in Alpha. We are currently granting access to a limited number of teams to gather feedback. To request access for your organization, please[Contact us](https://www.omnistrate.com/contact) .


## Continuous Improvements


### Build Faster


-


**Network rules for Operators:** Introduces enhanced support for operator-based deployments by allowing you to define standardized networking rules. This simplifies firewall management and enables more efficient resource binpacking, drastically reducing the need for manual configurations.


-


**Out-of-the-Box Helm Observability:** Helm lifecycle visibility is now enabled by default. This provides immediate, "day-zero" insights into behavior during builds and rollouts, allowing teams to diagnose root causes and resolve deployment failures without any manual setup.


### Improved Operations


-


**Deletion Protection:** Implements "guardrail" safeguards to prevent your customers from performing accidental destructive actions.


-


**Final Automated Snapshots:** Automatically captures a state backup of your deployments immediately prior to instance deletion, ensuring a safety net for emergency recovery.


-


**Enhanced Audit Forensics:** Audit logs now include the additional information in audit logs. This provides critical client fingerprint context, allowing security teams to perform more granular forensic analysis.


-


**Workflow Recovery Controls:** Increases operational flexibility by adding the ability to manually cancel or restart stuck workflows for amenities and deployment cells. This allows operators to clear "dead-end" states and re-run failed stages without having to tear down and rebuild the entire environment.


-


**Parallel Amenities Deployment:** Optimizes rollout velocity by enabling concurrent amenities deployments. By shifting from a linear, sequential processing model to a parallel execution model, this update significantly reduces total deployment windows and prevents "head-of-line" blocking in the deployment queue.


### Improve Your Customer UX


- **Streamlined BYOC Traceability:** The deployment cell response now includes the Intermediary Account ID. This simplifies troubleshooting in complex multi-account environments by providing immediate context on the underlying infrastructure.


Additional documentation on these and other features[can be found here](http://docs.omnistrate.com/) .


## Neo-Cloud Scaling: Waitlist Available


Omnistrate is trialing a new capability to leverage specialized, high-performance GPUs on Neo-Clouds while remaining seamlessly connected to the rich ecosystem of services on AWS, Azure, or GCP.


**We are currently opening a waitlist** for startup teams interested in early access (10 spots, YC W26 only). By helping these founders solve infrastructure pains early, we are gathering the blueprints and best practices to help our entire customer base scale.


**Are you a W26 founder building in the Agentic space?**


👉[Book a 15-minute sync with our founders](https://calendly.com/omnistrate/15-min-meeting)


## Community Momentum: CLI & MCP Updates


Recent community contributions to the omnistrate-ctl Open Source repo have optimized the CLI for the Model Context Protocol (MCP). These updates keep our tools at the cutting edge of AI-native infrastructure management and reflect the community's shift toward agentic workflows.


👉[View the latest changes on GitHub](https://github.com/omnistrate-oss/omnistrate-ctl/releases)


## Trust Center


Verifiable governance is the backbone of secure software distribution. Our recently launched Trust Center offers a real-time look at the continuously monitored controls implemented at Omnistrate to ensure compliance within our security program.


👉[Review our live security protocols](https://trust.omnistrate.cloud/controls)


## Strategic Partnership: AWS & Omnistrate


Is your team paying a 'manual labor tax' to stay **FTR-compliant?** Our latest video breaks down how Omnistrate's private-label control plane abstracts away infrastructure complexity, allowing your senior architects to stop fixing pipes and start launching scalable AWS Marketplace offers in days, not years.


👉[Watch the 4 minute explainer](https://youtu.be/KYLKCRllkoM)


## Upcoming Events


- **NVIDIA GTC** | March 16-19 | San Jose, CA
- **RSAC 2026 Conference** | Mar 23–26 | San Francisco, CA
- **Data + AI Conference 2026** | June 15–18 | San Francisco, CA
- **AWS & Omnistrate - SaaS & AI Builder Day** | May 14 | San Francisco, CA


👉[See details & schedule time with our founders](https://omnistrate.com/events?utm_source=newsletter&utm_medium=email&utm_campaign=jan2026)


## Practical Aspects of Agentic SaaS


As SaaS evolves from fixed workflows to AI agents, the complexities of distribution and infrastructure become even more critical. **Join the bi-weekly Agentic SaaS webinars** to explore best practices for scaling this new era.


🌟[Join the LinkedIn group](https://www.linkedin.com/groups/9880017/)


**Don't miss our next episode!**


👉[Watch the full series and subscribe](https://youtube.com/playlist?list=PLT2Zisspnj0fsEqkag0AtmPnw3mRfF3j_)


## Get Omnistrate Platform Updates


Subscribe to receive the latest platform features, improvements, and community highlights delivered to your inbox. No spam. Only relevant platform updates.


👉[Subscribe for updates](https://omnistrate.us17.list-manage.com/subscribe?u=08ffbac64293e1abc50999571&id=4846368b0b)
