---
schema_version: "1.0.0"
document_id: "7d48d425586b31c5463afdd1778f4f80596432cc9aa71464d3e67af6b5aa877f"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-e942a00c1738"
canonical_url: "https://www.paloaltonetworks.com/blog/2026/07/announcing-general-availability-of-prisma-airs-ai-gateway/"
published_at: "2026-07-16T14:50:53+00:00"
first_seen_at: "2026-07-25T01:08:05.069426+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:ea23893244fa157662dc7d66b49359767c17a1f907fa246fd8ea7ac6b17fbfdb"
---

# Announcing the General Availability of Prisma AIRS AI Gateway

[Agent Security](https://www.paloaltonetworks.com/blog/ai-security/category/agent-security/)


[AI Governance](https://www.paloaltonetworks.com/blog/ai-security/category/ai-governance/)


[AI Security](https://www.paloaltonetworks.com/blog/category/ai-security/)


[Announcement](https://www.paloaltonetworks.com/blog/category/announcement/)


[Develop AI Safely](https://www.paloaltonetworks.com/blog/ai-security/category/develop-ai-safely/)


[Products and Services](https://www.paloaltonetworks.com/blog/category/products-and-services/)


Every modern enterprise is moving from an organization run by software to one orchestrated by AI, creating a tension between velocity and control. To resolve this tension, organizations require a unified architecture. Today, we are announcing the general availability of the


[Prisma AIRS AI Gateway](https://www.paloaltonetworks.com/ai-security/ai-gateway) , the AI control plane for the enterprise.


Driven by the absolute conviction that an AI Gateway is foundational to the modern AI infrastructure stack,


**we are bringing AI innovations from the**[Portkey acquisition](https://www.paloaltonetworks.com/company/press/2026/palo-alto-networks-completes-acquisition-of-portkey-to-secure-ai-agents) **to Prisma AIRS just six weeks after closing** . You can now scale AI at machine speed without compromising on enterprise-grade security and control.


# Your AI Footprint is Outpacing Controls


Our next-generation firewall telemetry reveals that


**MCP activity climbed from 11% late last year to 41.4% by mid-2026** .


**Monthly AI transaction volume grew twelve-fold over the same six months; some individual sessions moved hundreds of megabytes of enterprise data outbound.** The AI footprint you can govern today is the smallest it will ever be.


Some of the most rapid AI adoption is happening with – coding assistants, enterprise agents and copilots.


- **Coding agents** access code repositories, file systems, configurations, and credentials. When this context, including source code and secrets, is sent to a frontier model, it risks exposing sensitive data, and a runaway loop could burn a fortune in tokens overnight.


- **Enterprise agents** run with broad access and standing privileges sharing context with each other. A single agent stretched beyond its scope can massively increase the risk of data breach, impact business reputation and customer trust.


- **Copilots** now sit inside the SaaS and productivity tools employees already use. Copilots with broad access can surface data an employee was never meant to see.


As this enterprise data leaves when an AI request goes out, it creates a security and governance challenge at an unprecedented scale and speed.


## What breaks when every team adopts AI


AI adoption rapidly outpaces the security and governance infrastructure meant to secure it:


- **Shadow AI (Cost and usage are both hidden)** : You can't see the AI your teams already use or what it costs.


- **Data Exposure (Sensitive data leaves without a trace)** : AI interactions can expose sensitive data, bypass policy and trigger unsafe outputs.


- **Agents Overstep (Actions run without accountability):** Agents act across systems without clear identity, permission, or accountability. Keys get shared and agents run with broad, standing privileges, so no one can say which identity authorized a specific action or reverse it when an agent takes a wrong turn at machine speed.


Faced with this, most leaders either


***block traffic entirely*** or


***stay permissive and promise to govern later*** . Both approaches fail because they skip the critical step: seeing what agents do at runtime and controlling their actions while they happen.


# Accelerate AI Adoption with Control


To scale AI adoption safely, you need a single control plane sitting between every AI interaction and the backend models. Prisma AIRS AI Gateway brings AI governance, identity and runtime controls together in one place. With the AI Gateway, you can:


- **Discover AI usage** : Know exactly which apps, models, users, teams and agents are active, what they are accessing, and what they cost in a single unified view.


- **Govern AI interactions** : Enforce central rules for model access, tool use and budgets while inspecting prompts and responses inline to prevent data leaks.


- **Secure every agent** : Verify agent identities and enforce just-in-time, least-privilege access so autonomous systems only touch what they need, when they need it.


# How the AI Gateway works


Prisma AIRS AI Gateway sits inline between every AI interaction, model provider, and agentic interaction (Agent/AI App to LLMs, MCP Tool Calls, and A2A). It acts as a unified LLM, MCP, and A2A Gateway with a single enforcement point for all operational and security controls. Your teams keep using their existing coding assistants, enterprise agents, and copilots, while enforcement moves to the infrastructure layer where the platform team can own it.


Capabilities delivered through this unified control plane are:


**Observability**


Every request maps to a single unified view: tracking usage, users, projects, token counts, latency and cost so you can retire shadow AI infrastructure immediately.


**** **Governance**


Centrally define approved models, tools, and access without touching developer configurations. To drive FinOps and usage management, track every request's cost, tokens, and latency by team or project. Answer cost questions instantly, shut down unsanctioned AI usage, and proactively enforce budgets and rate limits before access is granted.


**** **Coding Assistant Security**


Protect credentials, proprietary code and development systems from risky AI actions. The gateway replaces raw provider keys with scoped credentials per user and team.


**** **Operational Controls**


Apply data protection, usage limits and policy checks as AI interactions happen. Traffic distributes via a Universal API across providers ensuring quotas are enforced and outages never stall your pipeline.


**** **Agent Identity** **Security**


Establish trusted identities by binding a verifiable, ephemeral identity to agents at execution. The AI Gateway acts as an enforcement point to enable only authenticated agents to make approved calls.


**** **Runtime Security**


Powered by


[Prisma AIRS AI Runtime Security](https://www.paloaltonetworks.in/prisma/prisma-ai-runtime-security/ai-runtime-security) , the gateway inspects every prompt and response inline, stopping source code, secrets and customer data from leaving the network while neutralizing prompt injection attempts aligned with the OWASP LLM Top 10 and


[OWASP Top 10 for Agentic Applications](https://www.paloaltonetworks.in/resources/ebooks/owasp-agentic-top-10-survival-guide) .


Watch


Anand Oswal break down[why an AI gateway is foundational to this architecture](https://www.linkedin.com/posts/prisma-by-palo-alto-networks_every-new-ai-agent-creates-another-decision-activity-7485719130600386560-V_xs?utm_source=share&utm_medium=member_desktop&rcm=ACoAAA1b7mQBC_U4GoIfhag74wOxV2Ncb_9zM1Q) .


# The AI Control Plane for the Enterprise


Prisma AIRS AI Gateway brings AI governance, identity and runtime controls together in one place. Point products filter text strings or route a single API call and they buckle under real enterprise volume.


Most products fail at scale. Prisma AIRS AI Gateway is built on an architecture tested in the most demanding enterprises for their ever evolving AI workloads:


- **68 Trillion+ tokens processed** in the last month alone.


- **Sub-millisecond routing latency** and inline inspection secures AI interactions without degrading user experience.


- **99.999% availability** helps ensure your AI operational pipeline does not experience a single point of failure.


Learn about[Prisma AIRS AI Gateway](https://www.paloaltonetworks.com/ai-security/ai-gateway) and register for our webinar to see the AI Gateway in action.


Your developers have already adopted their agents. The agentic enterprise is a reality. As the recently named


["company to beat" in AI Security Platforms by Gartner](https://www.gartner.com/en/documents/8015569) , Palo Alto Networks is uniquely positioned to help you.[Let’s build it securely, together](https://www.paloaltonetworks.com/ai-security/ai-gateway#contact) .
