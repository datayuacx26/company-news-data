---
schema_version: "1.0.0"
document_id: "ab27240e8fbf3bba05a90679c8f17b2d007fd77b08eef187b4f240b577bd81fd"
company_key: "avepoint-inc-class-a-common-stock"
company: "AvePoint Inc."
source_id: "avepoint-inc-class-a-common-stock-news-import-1c9c9e9520bc"
canonical_url: "https://www.avepoint.com/blog/manage/microsoft-365-copilot-agent-types"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-04T17:48:55.588092+00:00"
fetched_at: "2026-08-04T18:43:13.169222+00:00"
content_hash: "sha256:edad984bd52126ce166aff16e195a0ccc91cd12fe9decdb64dd501c9028543d8"
---

# Which Microsoft 365 Copilot Agent Should You Build?

Microsoft 365 Copilot supports four agent creation options: SharePoint agents, Agent Builder, Copilot Studio, and declarative agents built with the Agents Toolkit in VS Code. They sit on a spectrum from simple to powerful. The right choice depends on your use case, who is building it, what data it needs, and what governance requirements your IT team needs to enforce.


## **Key Takeaways**


- **Four common ways to build Microsoft 365 Copilot agents.**


SharePoint agents, Agent Builder, Copilot Studio, and Agents Toolkit. These range from no-code to pro-code approaches. Start with the simplest option that solves the actual problem.
- **The orchestrator matters.**


Copilot Studio and declarative agents use different underlying orchestrators, which affect the depth of knowledge retrieval. Understand this before finalizing your architecture.
- **Governance requirements should drive tool selection.** The software development lifecycle (


SDLC) needs, audience size, external system access, and release management requirements all point toward different agent types.
- **Agent proliferation is a real operational risk.**


As creation becomes easier across all four methods, tracking ownership, data access, and cost exposure requires dedicated tooling — not just native admin controls.
- **Pay-as-you-go costs require proactive tracking.**


Agents running on metered consumption accumulate costs that do not surface until billing cycles close. Build cost visibility from the start.
- **Microsoft 365 is not the only environment to consider.**


Agent governance needs to account for agents running across platforms — not just Microsoft 365. Your governance strategy should reflect the full multicloud reality.
- **Start with visibility.** Before scaling agent deployment, establish a clear picture of what agents exist in your tenant, who owns them, and what data they can access.


## **What Are Microsoft 365 Copilot Agents?**


Microsoft 365 Copilot agents are specialized AI assistants built on top of the Copilot platform. Unlike general-purpose Copilot, each agent is scoped to specific instructions, knowledge sources, and connected systems — making it more focused and reliable for defined tasks. Agents can answer questions, automate workflows, or take actions across business systems.


Microsoft 365 Copilot is powerful, but it has a known limitation: When it has access to everything, it can return answers from the wrong place. An agent solves that by constraining what the AI knows and how it behaves. You define the instructions, the data sources, and the scope. The result behaves more like a specialist than a generalist.


Microsoft 365 now supports four distinct ways to build agents, each sitting on a spectrum from easy-to-create-but-limited to complex-but-fully-controlled. Choosing the wrong option for your use case doesn't just create a suboptimal experience — it creates governance problems. Agents you can't track, audit, or retire are a risk that compounds as adoption grows.


This post covers the architectural differences between all four agent types and the governance implications of each. The embedded webinar above (Part 2 of AvePoint's agentic webinar series) walks through live demos of all four options.


## What Is a SharePoint Agent and When Should You Use One?


A SharePoint agent is a document library-scoped AI assistant built directly inside SharePoint. It answers questions grounded only in the content of that library — no other data sources, no workflow automation. It requires no coding, no Copilot license for creation, and can be set up in minutes by any site owner with edit permissions.


SharePoint agents are the simplest option in the stack. Every document library in Microsoft 365 has the ability to host one. Site owners configure a name, a purpose, up to 20 content sources within the library, and a set of starter prompts.


The trade-off is intentional scope. A SharePoint agent only knows what is in that library. It will not pull from your organization's email, Teams channels, or any external system. For certain use cases – HR policies, product documentation, compliance reference materials – that constraint is an advantage. The agent cannot return content it does not have access to.


### SharePoint Agent at a Glance


- **Who builds it:**


Site owners and editors with no development experience
- **Knowledge scope:**


One document library (up to 20 configured sources)
- **License requirement:**


Works with M365 Copilot licensing or pay-as-you-go
- **Governance:**


Inherits SharePoint permissions automatically; manageable from the M365 Admin Center
- **What it cannot do:**


No cross-site knowledge, no workflow automation, no external system access, no conversation memory


If your team asks whether the agent can also pull from your Teams channel, the answer is no. That is Agent Builder territory.


## What Is Agent Builder in Microsoft 365 Copilot?


Agent Builder is a browser-based, no-code tool built into the Microsoft 365 Copilot experience. It lets users create agents that pull from multiple Microsoft 365 data sources – SharePoint, Teams, Outlook, OneDrive – and apply custom instructions about tone, scope, and behavior. Agents built here are accessible across the Microsoft 365 Copilot experience and can be shared with teams.


Agent Builder is accessible directly from the Copilot chat interface. No separate portal required. You describe what you want, or configure it manually — and within a few minutes you have an agent grounded on your organizational data.


Agent Builder lets you combine knowledge from multiple sources in a single agent. An HR agent, for example, can reference both a SharePoint policy library and a Teams channel where policy updates are discussed — something a SharePoint agent cannot do.


### Agent Builder at a Glance


- **Who builds it:**


End users, information workers, power users — no code required
- **Knowledge scope:**


Multiple Microsoft 365 sources such as SharePoint, Teams, Outlook, and OneDrive
- **License requirement:**


Included with Microsoft 365 Copilot license; free tier available for web-grounded agents; pay-as-you-go for tenant data access
- **Conversation memory:**


Yes, via Dataverse if configured
- **SDLC / application lifecycle management (ALM):**


Not designed for developer workflows or version control
- **What it cannot do:**


No external system integrations, no complex workflow automation, cannot be deployed outside Teams or Copilot


From a governance perspective, Agent Builder agents can be managed through the Microsoft 365 Admin Center. Admins can see an inventory of shared agents, block noncompliant ones, and enforce data-loss prevention (DLP) policies. As the number of agents grows, inventory management becomes a real operational challenge without additional tooling.


## What Is Copilot Studio for Agent Creation?


Copilot Studio is Microsoft's low-code, enterprise-grade agent creation platform built on Power Platform. It supports complex workflow automation, multi-step business logic, external system integrations (Salesforce, ServiceNow, SAP), and multichannel deployment across Teams, websites, and custom apps. It is designed for makers and developers building agents at the departmental or organizational scale.


Copilot Studio is a standalone web portal separate from the Microsoft 365 Copilot experience where Agent Builder lives. That separation matters for governance. Copilot Studio agents run on Power Platform, which means they come with environment-level policies, connector governance, and ALM across dev, test, and production environments.


For organizations building agents that need to take actions – creating support tickets, updating CRM records, triggering workflows, integrating with third-party APIs – Copilot Studio is typically the right choice.


### Copilot Studio at a Glance


- **Who builds it:**


Low-code makers, IT professionals, Power Platform developers
- **Knowledge scope:**


Microsoft 365 sources plus external data via connectors (89+ available); indexes external content into Dataverse
- **External integrations:**


Yes — Salesforce, ServiceNow, SAP, REST APIs, Power Automate workflows
- **License requirement:**


Separate Power Platform licensing required beyond the Microsoft 365 Copilot license
- **SDLC / ALM:**


Yes — environment staging (dev/test/prod) and version control support
- **Deployment:**


Multi-channel — Teams, websites, custom apps, email
- **Governance admin:**


Power Platform Admin Center; environment-level DLP, role-based access, auditing


One important technical distinction: Copilot Studio agents use a different underlying orchestrator than declarative agents built with VS Code. The same query against the same SharePoint content can return different numbers of documents depending on which orchestrator processes it. For compliance-sensitive retrieval use cases, that is a design decision worth understanding upfront.


## What Is a Declarative Agent Built with the Agents Toolkit?


A declarative agent built with the Microsoft 365 Agents Toolkit in VS Code is a developer-created agent that runs on the native M365 Copilot orchestrator. It gives developers precise control over instructions, knowledge sources, and connected systems — with full Git-based version control, CI/CD pipeline support, and deeper access to the M365 semantic index than browser-based tools provide.


Declarative agents are defined as files – JSON, manifest, YAML – which means they fit naturally into standard developer workflows: source control in GitHub or Azure DevOps, branching, pull requests, automated deployment pipelines.


The capabilities overlap significantly with Copilot Studio. Both support custom instructions, knowledge sources, external connectors, and actions. The practical differences come down to how you build, who builds, and which orchestrator processes queries.


### Declarative Agent (VS Code) at a Glance


- **Who builds it:**


Developers; requires VS Code and the Agents Toolkit
- **Knowledge scope:**


Full Microsoft 365 semantic index access; Graph connectors configured via Microsoft 365 Admin Center (available across all Microsoft 365 surfaces, not only Copilot)
- **Orchestrator:**


"Sydney" — same as native M365 Copilot; different from Copilot Studio's "Samba" orchestrator
- **SDLC / ALM:**


Strongest option — native Git workflows, full CI/CD pipeline support
- **Deployment:**


Microsoft 365 ecosystem (Teams, Copilot); not standalone external web without additional configuration


The orchestrator difference is the most underappreciated trade-off in this decision. The same query, the same SharePoint library, the same content but different orchestrators can return meaningfully different document counts in a knowledge retrieval call. For organizations with strict retrieval requirements or compliance use cases, that is not a footnote; it is a design constraint.


## How Do the 4 Microsoft 365 Copilot Agent Types Compare?


The four agent types span a spectrum from simple to powerful. SharePoint agents are fastest to deploy but most constrained. Agent Builder adds multisource Microsoft 365 knowledge with no code. Copilot Studio adds external integrations and enterprise ALM. Declarative agents give developers maximum control with full SDLC support and the native Microsoft 365 orchestrator.


**SharePoint Agent**


**Agent Builder**


**Copilot Studio**


**Declarative Agent**


**Who builds it**


Site owners


End users


Low-code makers


Developers


**Knowledge scope**


1 document library


Multiple M365 sources


M365 + external (Dataverse)


M365 + external (Admin Center)


**External integrations**


No


No


Yes (89+ connectors)


Yes (Graph connectors)


**Workflow automation**


No


No


Yes (Power Automate)


Limited (custom actions)


**Conversation memory**


No


Yes (Dataverse)


Yes (Dataverse)


No (default)


**Copilot license needed**


No


Yes / PAYG


Power Platform license


Yes


**SDLC / ALM**


None


None


Power Platform envs


Full Git / CI/CD


**Orchestrator**


M365 Copilot


M365 Copilot


Copilot Studio (Samba)


M365 Copilot (Sydney)


**Governance admin**


M365 Admin Center


M365 Admin Center


Power Platform Admin Center


M365 Admin Center


**Deployment channels**


SharePoint


Teams / Copilot


Teams, web, apps


Teams / Copilot


**Skill level needed**


None


None


Low-code


Developer


## How Do You Decide Which Microsoft 365 Copilot Agent Type to Build?


Choosing the right agent type comes down to five questions: Who is building it? What data does it need? What actions does it need to take? What are the SDLC requirements? How many users will access it? There is no universal right answer; the correct choice depends on the intersection of those factors for each specific use case.


The decision is not a flowchart. It is a conversation. Andrew Connell, who has spent 25 years building in the Microsoft 365 space, described it in the webinar as a "tug of war" — each answer to each question pulls the decision in a direction, and the trade-offs accumulate until one option clearly fits better than the others.


**1. Who is building and maintaining this agent?**


- **End users or information workers:** SharePoint agent or Agent Builder
- **Low-code power users:** Copilot Studio
- **Development team with Git workflows:** Declarative agent (VS Code)


**2. What data does the agent need access to?**


- **Single document library:** SharePoint agent
- **Multiple Microsoft 365 sources (SharePoint + Teams + Outlook):** Agent Builder or Copilot Studio
- **External systems (Salesforce, ServiceNow, SAP):** Copilot Studio
- **Maximum Microsoft 365 semantic index depth:** Declarative agent


**3. Does the agent need to take actions, or just answer questions?**


- **Read-only Q&A:** SharePoint agent or Agent Builder
- **Multistep actions, workflow triggers, external system writes:** Copilot Studio


**4. What are the SDLC and governance requirements?**


- **No formal requirements:** SharePoint agent or Agent Builder
- **Enterprise release management, staged deployments, version control:** Copilot Studio or Declarative agent


**5. How many users will access this agent?**


- **Individual or small team:** SharePoint agent or Agent Builder
- **Department or organization-wide:** Copilot Studio or Declarative agent


### Decision Tier Table


**Tier**


**Use Case**


**Best Fit**


**Tier 1: Quick wins**


Library-scoped Q&A, single-team FAQ bots


SharePoint Agent


**Tier 2: Cross-source knowledge**


Multisource M365 assistants, team productivity agents


Agent Builder


**Tier 3: Enterprise automation**


Workflow integration, external systems, scaled deployment


Copilot Studio


**Tier 4: Developer-controlled**


Full SDLC requirements, maximum retrieval control


Declarative Agent


## What Governance Risks Come with Each Microsoft 365 Copilot Agent Type?


Every agent type creates a different governance surface area. SharePoint agents inherit site permissions but have no lifecycle management built in. Agent Builder agents can be shared broadly with limited IT visibility at scale. Copilot Studio agents introduce a separate admin plane. Declarative agents require developer discipline. In all cases, agents that are not tracked, owned, or reviewed become a risk as the inventory grows.


This is the part of the agent creation conversation that most guides skip. The tool you choose determines where the agent lives, who can see it, how it is governed, and how you retire it.


- **SharePoint agents.**


Governance runs through the Microsoft 365 Admin Center and inherits SharePoint permissions. Solid baseline, but no native lifecycle management; nothing that surfaces when an agent is stale, who last updated it, or whether the grounding content is still current.
- **Agent Builder.**


Admins can view shared agents in the Microsoft 365 Admin Center and apply DLP and audit policies. The challenge is scale. As users across the organization create and share agents, the inventory grows quickly. Without systematic tracking of ownership and usage, you end up with agent sprawl — dozens of agents no one remembers creating, some still running against sensitive data.
- **Copilot Studio.**


More structured governance


– environment-level policies, connector controls, proper ALM – through Power Platform. — But this governance lives in a different admin center than the rest of your Microsoft 365 estate. For IT teams managing a combined environment, that means two separate visibility and control planes.
- **Declarative agents.**


File-based, which means Git is your governance. Strong for teams with developer discipline and established CI/CD pipelines. Less reliable when those practices are inconsistent or absent.


None of the four native tools answers the questions “What agents exist across my entire tenant?”, “What data are they touching?”, and “What are they costing me?”


## What Happens When Microsoft 365 Copilot Agents Proliferate Across Your Tenant?


Agent sprawl is the state where agents multiply faster than IT can track them. As more users and teams create agents across SharePoint, Agent Builder, Copilot Studio, and VS Code, the inventory becomes unmanageable without tooling designed for agent lifecycle management. Left unaddressed, agent sprawl creates audit gaps, unexpected pay-as-you-go costs, and unintended access to sensitive data.


When Copilot is enabled across your organization, agents start appearing. SharePoint agents are created by site owners. Agent Builder lets any licensed user spin up an agent and share it with their team. Copilot Studio adds departmental agents with external connectors. Declarative agents are delivered via developer deployment pipelines. Each creation method adds to a total inventory that the default admin tools were not designed to manage comprehensively.


Three specific risks emerge as the inventory grows:


**1. Untracked sensitive data access.**


An agent grounded in a document library that contains personnel files, financial records, or legal documents may be shared more broadly than the underlying permissions warrant. Without visibility into what each agent can access, you cannot enforce the same data policies you apply to human users.


**2. Pay-as-you-go cost accumulation.**


Agents that access tenant data outside of a Copilot license consume Copilot Credits on a usage basis. An agent that runs frequently without an owner tracking consumption can generate significant costs that do not surface until the billing cycle closes.


**3. No end-of-life process.**


Agents built for a specific project, campaign, or employee do not automatically deactivate. Without a lifecycle management process, they continue running; sometimes against data sources that have changed, sometimes with permissions that have expanded since the agent was created.


This is the gap that[AvePoint AgentPulse](https://www.avepoint.com/solutions/agentic-ai-governance) addresses. AgentPulse gives IT teams a single view of all agents across the tenant – including their type, owner, data access scope, pay-as-you-go cost exposure, and lifecycle status – regardless of which creation method was used to build them.


## What Are the Best Practices for Building Microsoft 365 Copilot Agents?


The most reliable approach is to start with the simplest tool that solves the immediate problem, validate the use case before scaling, and establish ownership and lifecycle expectations before deployment. Governance practices should be defined alongside the agent — not added retroactively when the inventory is already unmanageable.


**1. Match the tool to the actual requirement, not the most capable option.**


There is a tendency to jump to Copilot Studio or declarative agents because they feel more enterprise-grade. For many use cases, Agent Builder solves the problem just as well with significantly less overhead. Start simple.


**2. Define an owner before the agent goes live.**


Every agent should have a named owner responsible for its accuracy, performance, and eventual decommission. Ownerless agents are the primary driver of agent sprawl.


**3. Review grounding content as often as you review the content itself.**


An agent is only as accurate as the documents it is built on. If those documents become outdated, the agent's answers degrade. Build content review into the same schedule as the underlying source material.


**4. Treat pay-as-you-go exposure as a budget line item.**


Agents that run on metered consumption need cost visibility from day one. Track usage per agent, per department, per month.


**5. Have an end-of-life plan.**


What is the trigger for retiring an agent: A project ending? A policy changing? A headcount departure? Define it when you build the agent, not when someone asks why it is still answering questions about a deprecated product.


## Govern Every Agent — Not Just the Ones You Know About


As Microsoft 365 Copilot agents become easier to build, the challenge shifts from creation to governance.[AvePoint AgentPulse](https://www.avepoint.com/solutions/agentic-ai-governance) gives IT teams a single view of every agent across their tenant — including agent type, owner, data access scope, pay-as-you-go cost exposure, and lifecycle status — regardless of which tool was used to build it. If your organization is scaling agent deployment and needs visibility before the inventory becomes unmanageable, AgentPulse was built for exactly that.


## Frequently Asked Questions


### What is the difference between Agent Builder and Copilot Studio?


Agent Builder is a simplified, browser-based experience inside Microsoft 365 Copilot for creating agents without code — best for personal or small-team use cases grounded in M365 data. Copilot Studio is a standalone Power Platform tool designed for enterprise-scale agents that need external system integrations, complex workflow automation, multi-channel deployment, and formal release management. Agent Builder agents can be promoted to Copilot Studio when requirements grow.


### Do I need a Microsoft 365 Copilot license to use Agent Builder?


Not always. Agent Builder agents grounded on public web content are available at no additional cost. Agents that access tenant data – SharePoint, Teams, Outlook – require either a Microsoft 365 Copilot license or pay-as-you-go billing through Copilot Credits.


### What is the difference between a declarative agent and a Copilot Studio agent?


Both create customized agents on the Microsoft 365 Copilot platform with largely overlapping capabilities. The key differences are orchestrator and tooling. Declarative agents use the native Microsoft 365 Copilot orchestrator ("Sydney") with deeper semantic index access. Copilot Studio uses its own orchestrator ("Samba") and indexes external content into Dataverse rather than the Microsoft 365 Admin Center. Declarative agents fit naturally into Git-based developer workflows; Copilot Studio has its own ALM approach inside Power Platform.


### What is a SharePoint agent in Microsoft 365?


A SharePoint agent is a built-in AI assistant scoped to a specific SharePoint document library. It answers questions grounded only in that library's content, requires no code to create, needs no separate license for creation, and automatically inherits the library's existing permissions. Site owners can configure one from the document library interface in minutes.


### How does choosing an agent type affect Microsoft 365 governance?


Each agent type creates a different governance surface area. Agent Builder agents can be shared broadly by any user, creating inventory management challenges as adoption scales. Copilot Studio agents are governed through a separate Power Platform Admin Center. Declarative agents require developer discipline for safe deployment. In all cases, agents that are not tracked, owned, and reviewed introduce audit gaps, cost exposure, and potential data access risks that compound as the inventory grows.


### How do I govern Microsoft 365 Copilot agents across my tenant?


Native admin tools in the M365 Admin Center and Power Platform Admin Center provide baseline visibility and controls for their respective agent types. For organizations that need a single view across all agent types – including ownership, data access scope, pay-as-you-go cost, and lifecycle status – third-party tooling like AvePoint AgentPulse provides that consolidated governance layer across the full tenant inventory.


### Can I migrate an agent from Agent Builder to Copilot Studio?


Yes. Microsoft provides an upgrade path that copies an Agent Builder agent into Copilot Studio, preserving core configuration and instructions. The conversation flow model in Copilot Studio is different from the natural language instruction approach in Agent Builder, so treat the migration as an extension of the original work rather than a simple switch. Budget time for adapting the design to Copilot Studio's paradigm.


### How does the orchestrator choice affect Microsoft 365 Copilot agent results?


The orchestrator is the engine that processes queries, retrieves knowledge, and generates responses. Declarative agents use Microsoft's "Sydney" orchestrator — the same one powering native Microsoft 365 Copilot — which has deeper access to the M365 semantic index. Copilot Studio uses "Samba." Running the same query against the same SharePoint content can return different numbers of documents depending on which orchestrator is used — a meaningful difference for retrieval-sensitive compliance or accuracy requirements.
