---
schema_version: "1.0.0"
document_id: "5c00c21ec82fbd42d88c49bc13868041ad08669f0b31e34832825cc373697574"
company_key: "yc-icepanel"
company: "IcePanel"
source_id: "yc-icepanel-news-import-9cf2a09ec197"
canonical_url: "https://icepanel.io/blog/2026-08-10-c4-modelling-enterprise-ai-agents"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-10T18:29:49.603482+00:00"
fetched_at: "2026-08-10T18:29:50.390913+00:00"
content_hash: "sha256:6881b0363690824b3a0d7456102aa9eefb10a9336b8de80d4ad68ce4355ca12f"
---

# Why Your Microsoft Foundry Deployment Keeps Failing Security Reviews

In one anonymized engagement, we were three weeks from go-live on a supply chain agent deployment for a Fortune 50 manufacturer. The platform was Microsoft Foundry (formerly Azure AI Foundry). Seventeen agents. Live connections to SAP, Snowflake, and three EDI systems. Demand forecasting, supplier risk scoring, material planning, running autonomously overnight, surfacing $89 million in exposed supplier spend that nobody had quantified before.


The technology worked. The CIO killed it anyway.


Not because of security concerns. Not because of cost. Because when the enterprise security team asked the question every review asks before anything goes to production (“walk us through the architecture”), the team produced a diagram that nobody outside the engineering group could interpret.


We rebuilt the documentation using C4. Two weeks later, the engagement was back on track. Same Foundry deployment. Same agents. Different diagrams.


This is the pattern we see on enterprise AI Foundry deployments. The technology is sound. The communication of the architecture often is not.


---


## Why Platform-Based Agent Deployments Create a Distinct Documentation Challenge


When you build on Foundry, you inherit significant capability. The Agent Service handles orchestration. Azure AI Search can be connected for knowledge retrieval. Azure AI Content Safety can be integrated for guardrails. Microsoft Foundry Agent Framework supports multi-step workflows and human approval patterns, though where those controls are actually enforced is an application and workflow design choice your team makes, not something the platform guarantees automatically. Document it explicitly.


This is the right architectural decision. You’re not reinventing orchestration or building a vector database from scratch. You’re configuring, connecting, and extending a production-grade platform.


But this creates a documentation problem that traditional architecture diagrams handle poorly.


When a security reviewer looks at your deployment, they need to understand three distinct layers:


1. **What the platform provides** — what Microsoft is responsible for operating
2. **What you configure** — the agents, tools, knowledge connections, and guardrails your team defines
3. **What you connect** — the enterprise systems, identity providers, and data sources the platform touches


A single architecture diagram collapses all three layers. The result answers no question clearly enough for any audience. Not the CISO, not the VP of Engineering, not the board.


The C4 model: the Context, Container, Component, Code model created by Simon Brown, provides four levels of abstraction that map naturally to this challenge. We apply C4 with an additional lens specific to platform-based deployments: at each level, we explicitly distinguish what the platform provides from what your team configured from what connects externally. That overlay is ours, not C4’s, and it’s worth naming that distinction when presenting to architecture audiences who know the model.


## C4 Applied to an Azure AI Foundry Deployment


### Level 1 — Context: The Enterprise Boundary


At the context level, your entire Foundry deployment is one box. The diagram shows only who interacts with it and what external systems it touches.


For a supply chain deployment:


- **Supply chain planners and procurement teams** interact via a web dashboard or Teams integration
- **SAP S/4HANA** — external system, read-only connection for inventory and supplier data
- **Snowflake** — external system, read-only connection for historical analytics
- **Model inference endpoint(s)** — Foundry’s model catalog supports multiple model providers; document which you are using and under what access pattern
- **Entra ID** — external identity provider handling all authentication


Nothing inside the box. No Foundry internals. No agent names. No infrastructure.


A board member reads this in 60 seconds and understands the enterprise boundary. A legal team understands what data flows where. A CFO understands what external dependencies exist.


This is the diagram that gets security reviews scheduled. It also establishes a shared vocabulary for every technical diagram that follows.


### Level 2 — Container: Platform, Configuration, and Connection


This level is where C4 becomes particularly effective for Foundry deployments because it makes the distinction between platform-managed infrastructure and team-owned configuration explicit and visible.


We organize the container diagram into two zones. This is our deployment-specific overlay on C4, not a native C4 construct, worth stating clearly to architecture audiences.


**Zone 1 — Azure AI Foundry (platform-managed)**


These are services your team connects to and configures, not infrastructure you operate:


- **Agent Service** — Microsoft-managed orchestration runtime. Your team defines agents, registers tools, and sets behavioral instructions. Microsoft operates the service.
- **Azure AI Search** *(if configured)* — a connection your team may choose to establish for knowledge retrieval. Not a default, an explicit deployment choice depending on your knowledge grounding requirements.
- **Azure AI Content Safety** *(if configured)* — an optional integration for guardrail enforcement. Some deployments use this; others implement guardrails through model instructions, tool validation, or custom evaluation pipelines. Document your actual choice.
- **Microsoft Foundry Agent Framework** — for new work, use Microsoft Foundry Agent Framework or application-level orchestration for multi-step workflows and human approval patterns. Document where those controls are enforced, at the platform level, the application level, or both. Note: Prompt Flow is no longer recommended for new Foundry deployments and is scheduled for retirement in 2027.


**Zone 2 — Team-configured and connected**


These are components your team owns and is responsible for:


- **Enterprise Data Tools** — SAP RFC connectors, Snowflake query executors, SharePoint document processors. Registered in the Agent Service tool catalog. The platform calls them; your team builds and maintains them.
- **Business Knowledge Index** — enterprise documents, SOPs, contracts indexed and made available for retrieval. Your team owns the indexing pipeline and content currency.
- **Audit Pipeline** — agent invocation logs forwarded to your client’s Log Analytics workspace or Microsoft Sentinel. Your team owns the pipeline; the client owns the data.
- **Identity and RBAC Configuration** — Entra ID integration mapping enterprise roles to agent permissions. Your team configures this; Entra ID enforces it.


This two-zone container diagram answers the question security teams actually ask: **“What is Microsoft responsible for and what are you responsible for?”**


It also surfaces a conversation that matters for regulated industries: which components are covered by Microsoft’s compliance certifications, and which components fall under the client’s own compliance scope?


### Level 3 — Component: Inside the Agent Service Configuration


At the component level, zoom into the Agent Service and show what your team actually configured, not what the platform is generally capable of.


For a supply chain deployment, the component diagram shows the specific agents defined, each with:


- **Registered tools** — the specific functions each agent is permitted to call, with input and output schemas visible
- **Knowledge connections** — which indexes or retrieval sources each agent has access to
- **Guardrail configuration** — the specific policies applied, whether through Azure AI Content Safety, model-level instructions, or custom validation logic


A supplier risk agent and a demand forecasting agent may share the same platform infrastructure but have completely different tool sets, knowledge access, and behavioral constraints. The component diagram makes those differences visible and auditable.


This is the diagram that answers the architect’s question: **“What did your team actually build versus what did you configure versus what did you inherit from the platform?”**


## C4 Is Necessary But Not Sufficient for Security Approval


C4 diagrams communicate architecture clearly. They are not a complete security package.


Enterprise security reviews for AI agent deployments, particularly in financial services, healthcare, and government, require additional artifacts alongside your C4 diagrams:


- **Data flow diagrams** showing trust boundaries, data classification at each hop, egress points, and retention policies
- **Threat models** identifying attack surfaces specific to agentic behavior: prompt injection, tool misuse, model inversion, unauthorized agent chaining
- **Identity and permissions matrix** mapping every agent to its permitted tools, knowledge sources, and approval thresholds
- **Human-in-the-loop documentation** specifying which agent actions require human approval before execution, and where those approval workflows are enforced — at the platform level, application level, or both
- **Audit evidence package** demonstrating that every agent invocation is logged, retained according to policy, and available to compliance teams


C4 gets you through the first stage of review, architectural comprehension, where reviewers are trying to understand what the system is. The artifacts above get you through the risk assessment stage, where reviewers determine whether it is safe to deploy.


Teams that skip C4 spend the entire review on comprehension and never reach a productive risk discussion. Teams that only do C4 reach the risk discussion but cannot close it. Both are required.


## Why Security Teams Respond Differently With C4


Without C4, enterprise Foundry security reviews follow a consistent pattern: questions accumulate, most of them trying to establish basic architectural facts before any risk assessment can begin.


With C4, the conversation restructures. The context diagram establishes the enterprise boundary quickly. The container diagram surfaces the platform-vs-configuration boundary, where meaningful security findings actually emerge. By the time component diagrams appear, reviewers are asking specific questions about specific agent configurations: tool permissions, guardrail policies, approval thresholds.


The review becomes a risk conversation instead of an archaeology exercise.


## This Pattern Transfers to AWS AgentCore


The same C4 approach applies directly to AWS AgentCore deployments. The platform-managed zone maps to Bedrock Agent Runtime, Bedrock Knowledge Bases, and Bedrock Guardrails. The team-configured zone maps to Lambda tool functions, S3 knowledge content, CloudWatch audit pipelines, and Cognito or Okta identity integration. The same categories of security artifacts apply, adapted to AWS’s services and your organization’s control environment. A companion post will cover AgentCore specifically.


## Start With the Box Nobody Wants to Draw


The Level 1 context diagram feels too simple. Engineers resist it because it shows none of the technical work. Architects resist it because it doesn’t show design decisions.


Draw it anyway.


The context diagram is the one that gets a security review scheduled, a budget approved, a board presentation completed. Every technical diagram underneath it is built on the foundation of a stakeholder who understood where the system sits and what it touches.


For enterprise agent deployments on Azure AI Foundry, the architecture is only as deployable as it is communicable.


Start with the box. Build everything else from there.


---


*Proxima Intelligence is an enterprise AI consulting firm specializing in production AI agent deployments for Fortune 100–500 enterprises across financial services, healthcare, manufacturing, and government. Learn more at[proximaintel.com/agents](https://proximaintel.com/agents) .*


*This article was co-written with IcePanel*
