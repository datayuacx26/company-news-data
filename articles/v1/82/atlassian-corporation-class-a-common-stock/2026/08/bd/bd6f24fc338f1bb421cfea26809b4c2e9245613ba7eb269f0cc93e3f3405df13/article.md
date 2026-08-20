---
schema_version: "1.0.0"
document_id: "bd6f24fc338f1bb421cfea26809b4c2e9245613ba7eb269f0cc93e3f3405df13"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-af8c2eac0472"
canonical_url: "https://www.atlassian.com/blog/rovo/ai-governance-built-in"
published_at: "2026-08-14T16:00:00+00:00"
first_seen_at: "2026-08-20T02:19:49.819587+00:00"
fetched_at: "2026-08-20T02:19:52.087777+00:00"
content_hash: "sha256:f64cee03a70e71cb56798ffc81f8b559e286ccd1fb166f06ef00cc8831d9cf17"
---

# Don’t Build AI Governance From Scratch. Just Turn It On.

Every organization is in the middle of an agentic transformation. Teams everywhere are rapidly building and deploying agents that access data, trigger workflows, and influence decisions, often without centralized oversight.


That shift happened fast. Faster than most organizations planned for. Gartner ® predicts that “by 2028, an average global Fortune 500 enterprise will have over 150,000 agents in use. Only 13% of organizations think they have the right AI agent governance in place.” The gap between those two numbers is where the risk lives.


Agents are gaining more autonomy, coordinating across tools, handing off tasks, and executing multi-step processes with minimal human intervention. That autonomy raises the stakes: a single misconfiguration can pull customer PII into an unapproved workflow and cascade across systems before anyone notices, exposing customer data, inviting regulatory scrutiny, and eroding the trust your organization spent years building.


For IT and security teams, the accountability falls to you. Atlassian helps you close that gap with AI governance built into the platform your teams already trust.


## The problem isn’t AI. It’s ungoverned AI.


When AI adoption outpaces governance, three problems emerge: sensitive data becomes harder to control, agents begin to proliferate without consistent boundaries, and teams lose visibility into what AI systems are doing across the organization.


This usually happens when AI is introduced outside the systems where work, permissions, and policies already live. Security teams are then left asking basic but critical questions: *What’s running? Who created it? What can it access? What actions can it take?*


And when those answers are unclear, the default response is often to slow adoption. But long term, that creates a different kind of risk: teams still want AI, so they find workarounds that are even harder to govern.


The better approach is to bring governance closer to the work itself. That means using AI that operates within the same platform, permissions model, and administrative controls your teams already trust.


## AI governance works better when it’s built into the Atlassian platform


Atlassian has been the home for teamwork for over two decades. The projects, code, and institutional knowledge stored in the Atlassian Teamwork Graph provide the context that transforms generic AI output into outcomes that understand your project history, team structures, and strategic goals.


Unlike vendors that require you to rebuild permissions, policies, and integrations from scratch, Atlassian’s AI, Rovo, runs on the same platform your teams already rely on. You do not need to recreate permissions, rebuild governance policies, or bolt on a separate administrative model just to start governing AI. The controls you already use for access, classification, and protection carry forward into AI experience. As agent usage expands, you can layer on agent-specific controls like scoped permissions, tool restrictions, and execution boundaries, without rearchitecting what’s already working.


## What AI governance looks like in practice


Governing AI isn’t a single control or a checkbox. It is a connected system that helps you protect sensitive data, define what autonomous systems are allowed to do, and maintain visibility into what actually happened.


At Atlassian, we think about AI governance in three parts:


- Securing the underlying data
- Setting boundaries for autonomous behavior
- Maintaining a record of everything that happened


Each part matters on its own, but the model works best when all three operate together.


### Securing the underlying data


AI security starts with data security. Before you scale AI across the organization, you need confidence in what data exists, how sensitive it is, where it lives, and who should be allowed to interact with it.


For example, a contractor asks AI a question and the response includes source code from a repository they were never supposed to see. Or a summary pulls in compensation data and surfaces it in a shared channel. These aren’t edge cases. They’re the predictable result of using AI on unorganized, unclassified, or unmonitored data.


**[Atlassian Guard](https://www.atlassian.com/software/guard/guard-premium)** gives security teams the controls to classify, monitor, and secure that foundation before AI ever touches it with:


- [Content scanning](https://support.atlassian.com/security-and-access-policies/docs/what-is-a-content-scan/) – built-in and custom detections to discover what sensitive data already exists across your environment, and flag new risks in real time as users create content.
- [Automatic classification](https://support.atlassian.com/security-and-access-policies/docs/configure-data-classification-rules/) **–** apply sensitivity labels based on rules your team defines, so there’s no ambiguity about what needs protecting or how to handle it.
- [Org-level data security policies](https://support.atlassian.com/security-and-access-policies/docs/what-is-a-data-security-policy/) **–** block export, restrict sharing, and enforce protections at scale by mapping policies directly to your classification labels.


Those controls cover your data foundation. The second layer of protection is specifically focused on your AI interactions, the data that can be pulled from external sources, and the surfaces where it appears.[Rovo connector security](https://www.atlassian.com/roadmap/cloud/connector-data-security?search=connector&selectedProduct=access;rovo) scans third-party connector data in real-time and prevents sensitive content from entering our environment.[Rovo Chat security](https://www.atlassian.com/roadmap/cloud/rovo-chat-security?search=Rovo&selectedProduct=access;rovo) scans user-level data to block or redact sensitive data from entering prompts and surfacing in responses.


For organizations with stricter isolation, encryption, or sovereignty requirements, Atlassian also supports additional deployment and protection options, including Atlassian-hosted LLMs, customer-managed keys, and data residency choices. Together, these capabilities help extend your existing data security posture into AI without requiring a separate governance layer.


### Setting boundaries for autonomous behavior


As more teams deploy autonomous AI, you need granular controls that adapt to your organization’s needs.


Let’s look at another real-life scenario: a sales team deploys an agent to automate pipeline updates. It connects to your CRM, starts pulling customer contract data into a Confluence workflow you didn’t approve, and inherits full access permissions from the rep who built it last Tuesday. Each part of that scenario is avoidable when agents are governed with explicit boundaries from the start.


Atlassian helps administrators set those boundaries through controls such as:


- [Agent permissions](https://support.atlassian.com/rovo/docs/rovo-agent-permissions-and-governance/) – ensure that only teams approved to build agents can do so.
- *Coming soon:* **agent identities** – give every agent explicit, role-scoped access that can be audited and revoked independently. Set org-level defaults so every new agent is automatically scoped to the right apps and groups.
- [MCP tool controls](https://support.atlassian.com/security-and-access-policies/docs/Configure-Atlassian-Rovo-MCP-server-permission/) – govern which external systems agents can connect to, what actions they’re permitted to take, and require IDP verification through Okta before anyone connects via MCP, so access is controlled at the front door.
- And to make agent sprawl easier to manage, we’ve built a[centralized console](https://support.atlassian.com/rovo/docs/track-how-often-a-rovo-agent-is-used/) to view and manage every agent across your org. *Coming soon* **:** we’re adding the ability to take bulk actions to make managing agent sprawl even easier.


When these controls live in the same administrative surface teams already use, governance becomes easier to operationalize. Instead of adding friction, it makes agent usage more consistent, more centralized, and easier to manage at scale.


### Maintaining a record of everything that happened


Knowing what AI is authorized to do is one thing. Proving what it actually did is another.


Imagine if your CISO asks, “An agent made a decision that affected a customer account. Can you tell me what it did, what data it used, and whether it followed policy?” To answer those questions, you need records, reporting, and operational visibility.


Atlassian supports that visibility through capabilities such as:


- [Audit logs](https://support.atlassian.com/security-and-access-policies/docs/view-audit-log-activities/) – provide a traceable record of agent actions and what ran.
- [Rovo insights](https://support.atlassian.com/organization-administration/docs/gain-insights-into-rovo-ai-activity/) – monitors AI activity across the organization, including which agents are running, how often, and who’s using them.
- [Platform usage](https://support.atlassian.com/organization-administration/docs/what-is-platform-usage/) – tracks Rovo credit consumption across your organization so finance and IT can forecast spend, identify where AI is being used most, and make informed decisions about where to scale.
- And *coming soon* , the **ROI calculator** will help you answer whether the investment is paying off, with a clear picture of which agents are delivering results and which need attention.


Together, these give you a continuous line of sight into adoption, compliance, and cost, not just after something goes wrong, but as a default operating posture.


## The road ahead


The shift to agentic AI isn’t slowing down, and neither is the accountability that comes with it. But governance doesn’t have to be what slows you down. The organizations that will move fastest are the ones that use the foundations they’ve already built. If you’re not sure where your gaps are, that’s the place to start. Atlassian can help you get there.


1. *Gartner Press Release, “Gartner Identifies Six Steps to Manage AI Agent Sprawl,” 28 April 2026.*[Gartner Identifies Six Steps to Manage AI Agent Sprawl](https://www.gartner.com/en/newsroom/press-releases/2026-04-28-gartner-identifies-six-steps-to-manage-artificial-intelligence-agent-sprawl) GARTNER is a trademark of Gartner, Inc. and/or its affiliates.


---


### Getting started


If you’re an organization admin, follow the steps below. For everyone else,[send a request to your admin](https://www.atlassian.com/try/cloud/signup?bundle=teamwork-collection&primaryCollectionProduct=rovo.ondemand&edition=premium) to get Rovo enabled for your team.


1. Go to[Atlassian Administration](https://support.atlassian.com/organization-administration/docs/explore-an-atlassian-organization/) . Select your organization if you have more than one.
2. Select **Rovo** , then **Rovo access** .
3. Toggle on **Enable Rovo features**
