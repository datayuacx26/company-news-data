---
schema_version: "1.0.0"
document_id: "36af3bd45d515ce8e5345f81af247d18fae9728497431bed0590ca0541ec154e"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-news-import-9b9c5ebd5b82"
canonical_url: "https://www.nutanix.com/blog/introducing-nutanix-agent-gateway"
published_at: "2026-05-26T09:00:00+00:00"
first_seen_at: "2026-07-24T12:20:54.900145+00:00"
fetched_at: "2026-07-28T22:12:43.834196+00:00"
content_hash: "sha256:207e93c8278579b00844ed4bca9418aa38701a9df97777d6b8b944060b7704bc"
---

# Introducing Nutanix Agent Gateway: Unified Governance and Cost Control for Enterprise AI

The generative AI landscape has undergone a fundamental shift. What began as reactive chatbots answering questions has evolved into autonomous agents that actively engage with enterprise tools, business systems, and LLMs. These same agents are now able to spawn sub-agents with the same level of capability. This evolution comes with a compounding challenge: as agent sprawl increases across organizations, so does the multiplier effect on token consumption and access control risks.


This challenge is why we built the Nutanix Agent Gateway solution. A unified, governed layer that is designed to provide cost control and governance capabilities to help manage all users of these autonomous agents. The Nutanix Agent Gateway is now GA and available as part of the Nutanix Enterprise AI 2.7 release.


## How Nutanix Agent Gateway Addresses These Challenges


Nutanix Agent Gateway is tightly integrated with[Nutanix Enterprise AI](https://www.nutanix.com/products/nutanix-enterprise-ai) ’s private inference stack, designed to provide enterprises with a consistent control layer across public hosted models and private self-hosted models.


At its core, Nutanix Agent Gateway acts as a centralized gateway that manages and helps secure traffic from agents to LLMs and from Model Context Protocol (MCP) servers* (in Tech Preview) accessing business tools such as GitHub and Stripe. Organizations can gain centralized policy enforcement, comprehensive usage visibility, and real-time token cost accountability across public cloud and self-hosted inference environments.


Nutanix Agent Gateway is designed to deliver centralized token observability across model vendors, enabling IT and platform teams to track usage, attribute costs, and help control excessive token consumption. This visibility allows teams to optimize their spend by shifting workloads to self-hosted local models, helping reduce the dependence on costly hosted providers.


Serving as the control layer between agents and enterprise tools through MCP servers, Nutanix Agent Gateway applies access control policies and tool-level filtering of MCP servers, enabling secure agent access to business tools and private data sources within a controlled environment aligned with enterprise governance requirements.


The Nutanix Agent Gateway along with Nutanix Enterprise AI’s private inference management, provide a mechanism to self host private models as well as consume them via the unified API while configuring user management and policies once and having them apply to both the gateway and private inference.


## Why AI Governance and Cost Control Matter Now


The shift from experimental AI to production-grade autonomous agents has happened faster than many governance frameworks can keep up. Agents are no longer isolated tools, they are deeply integrated in enterprise tools and external sources, executing complex, multi-step workflows.


Infrastructure teams without centralized control must confront associated operational and security risks. Developers who lack secure access to models and agents can create shadow AI, posing serious risks for data leakage and compliance standards. Critical questions arise: how do you enable secure AI access without slowing developers down? How do you maintain visibility and control at scale?


Gartner predicts that “by 2027, over 40% of agentic AI projects will be canceled due to escalating costs, unclear business value or inadequate risk controls.”1


This highlights why governance and cost control are becoming business-critical requirements for enterprise AI adoption. Without them, cost control and token usage become guesswork and compliance becomes reactive. Nutanix Agent Gateway provides infrastructure teams the centralized control to support governance, while giving developers the access they need to build without friction.


## Tackling the Operational Challenges of Agentic AI


Left ungoverned, agent systems can compound risk fast. Token consumption scales without any real visibility and access control gaps multiply across every tool interaction, leading to inconsistent governance. Nutanix Agent Gateway addresses this in the following ways:


- **Nutanix Agent Gateway Governance for MCP:** Set granular access control to MCP servers, enabling agents to securely connect to business tools and private data sources
- **Unified Observability:** Centralize visibility into token usage, MCP server access, and LLM activity
- **Audit Logs:** Record every MCP request with a comprehensive audit trail for AI governance
- **Unified API:** Access external provider models and self-hosted models through a single API, allowing developers the freedom to use the right model for the right use case
- **Granular Token-Based Rate Limiting:** Enforce token quotas and limits centrally that deliver real-time visibility into token usage across every agent and team


Nutanix Agent Gateway gives teams the flexibility to run the models, infrastructure, and environment that works best for them, helping to reduce the risk of vendor lock-in. It can be deployed rapidly in the public cloud or supported Kubernetes infrastructure, allowing you to be up and running with the potential for a lower total cost of ownership from day one.


## Get Started Today


Nutanix Agent Gateway is available to all Nutanix Enterprise AI customers. If you’re already a customer, you can begin exploring the latest capabilities today.


Learn more and get started at[https://www.nutanix.com/products/nutanix-enterprise-ai](https://www.nutanix.com/products/nutanix-enterprise-ai) .


*Tech Preview indicates these features should not be used in production environments.


1 - Gartner, Top Actions to Drive Success in Building Agentic AI Solutions, Aaron Harrison, Haritha Khandabattu, 9 April 2026


GARTNER is a trademark of Gartner, Inc. and/or its affiliates. ©2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. Kubernetes is a registered trademark of The Linux Foundation in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). This content may contain express and implied forward-looking statements, including related to Tech Preview releases and planned future general availability releases of NaAI, which are not historical facts and are instead based on our current expectations, estimates, and beliefs. The accuracy of such statements involves risks and uncertainties and depends upon future events, including those that may be beyond our control, and actual results may differ materially and adversely from those anticipated or implied by such statements. Any forward-looking statements included speak only as of the date hereof and, except as required by law, we assume no obligation to update or otherwise revise any such forward-looking statements to reflect subsequent events or circumstances.
