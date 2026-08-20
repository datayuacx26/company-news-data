---
schema_version: "1.0.0"
document_id: "0ee5fa60fbf83a5a831921e2a2476e8da0be8ea868a56bbf3f6413d3251a168f"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/governed-ai-enterprise-controls-snowflake-coco"
published_at: "2026-08-18T13:00:00+00:00"
first_seen_at: "2026-08-18T16:32:59.768845+00:00"
fetched_at: "2026-08-18T16:33:00.835701+00:00"
content_hash: "sha256:269938c1eb81d8bbeea9693a8763cf0e64c4d9970924e2052990687e214d0581"
---

# Governed AI for Every Builder: Enterprise Controls in Snowflake CoCo

In July, we wrote about Snowflake[CoCo's ability to scale enterprise AI with trust](https://www.snowflake.com/en/blog/snowflake-coco-built-to-scale-enterprise-ai/) , organized around three ideas: governing AI costs, grounding AI in enterprise context and bringing trusted AI into the places where work already happens.


Building on that foundation, we are expanding our governance capabilities today. While per-user quotas for AI cost management are now generally available, we are also introducing three new capabilities (generally available soon) that extend governance beyond spend to access, behavior, and tooling. This allows organizations to define boundaries not only for AI costs, but for what AI can reach, how it behaves, and which external systems it can interact with.


Together, these controls give platform and security teams a clear solution to enterprise AI access: Administrators get enough visibility and control to open CoCo broadly across their organizations, and builders get an agent that inherits those guardrails automatically. As a result, governance stays in the background and work moves forward.


## Cost governance: Per-user quotas are now generally available


Per-user quotas are now generally available across every CoCo surface: CoCo in Snowsight, CoCo CLI and CoCo Desktop.


[Administrators set daily and monthly AI credit limits](https://docs.snowflake.com/en/user-guide/cortex-code/credit-usage-limit) per user, and Snowflake enforces them automatically. When a user reaches a limit, their access is blocked with no custom code, stored procedures or manual intervention required. Access resets at the next cycle boundary.


Daily and monthly limits are evaluated independently. A data engineer who asks CoCo to iteratively refactor a suite of legacy stored procedures, running dozens of prompts to get the output right, no longer shows up as a surprise line item at the end of the month. Daily limits cap usage at the threshold you set and block further requests until the next cycle.


Usage remains fully queryable through SNOWFLAKE.ACCOUNT_USAGE, with per-request detail by user, model and token type. You can also ask CoCo directly "Which users are consuming the most credits this month?" and get an answer with the underlying data attached.


## Three layers of governance, enforced before a session starts


Cost is one dimension. This launch introduces three additional controls, all generally available soon1, that govern what an agent can access and do.


Figure 1: Three layers of governance: Query level, team level and organization level


**MDM,** GA soon, pushes organization-wide policy to every CoCo installation when configured. Administrators define which Model Context Protocol (MCP) servers users are allowed to connect to, which models are available to users and which tool actions require approval. Policy applies consistently whether a developer launches CoCo in Snowsight, CoCo CLI or CoCo Desktop, and users cannot opt out of an enforced setting.


**Agent profiles** define defaults per team or role: the default model, preinstalled skills and tool access. A data engineering team and a finance analytics team can run different CoCo settings without either team configuring anything themselves. Administrators set the profile once, and role-based access control (RBAC) applies it.


**Restricted session scope (RSS)** , GA soon, constrains what SQL an agent can run until an appropriate role is active in the session. This is the control that answers the blast radius question directly, because the agent is no longer implicitly trusted with everything the user's credentials permit.


Each of these operates at a different scope, and each is enforced before a session begins rather than monitored after the fact.


## Governed MCP and tool access through Cortex AI Gateway


Accessing MCP servers is one of the ways agent context extends beyond Snowflake, connecting agents to the systems teams already use: Jira, Slack, Google Workspace, internal APIs and thousands more.


CoCo now connects to MCP servers through Tools by[Cortex AI Gateway](https://www.snowflake.com/en/blog/enterprise-ai-security-agentic-mcp-governance/) , PuPr soon, the centralized MCP gateway built on technology from Snowflake's[Natoma](https://www.snowflake.com/en/blog/snowflake-acquire-natoma-governed-agentic-access/) acquisition.


Figure 2: Tools by Cortex AI Gateway


Governance policies are defined in the Horizon Catalog and enforced at the tool-call level, which gives administrators four controls that did not previously exist:


- **Server-level allowlisting** : Administrators select which MCP servers are available organization-wide. Disabled servers are invisible to end users.
- **Tool-level policy** : Individual tools can be disabled without disabling an entire server, so a Salesforce read tool, for example, can stay available while a delete tool might not.
- **Rate limits:** Per-server caps on tool calls prevent a misconfigured agent from saturating a rate-limited external API.
- **Comprehensive audit trail** : Tool calls record who requested the action, what permissions they held, whether the action was allowed and what the outcome was.


Paired with managed settings, which prevent users from configuring their own MCP servers, external tool calls an agent makes is approved in advance and auditable afterward.


## What this means for administrators


As a result of these changes, administrators can now write an AI access policy that supports security reviews.


Before these controls, teams often had to limit CoCo to limited groups, a sandbox schema or a manual approval process. The productivity gains were clear, but the risk was harder to measure.


Quotas make individual spend bounded and enforceable. Managed settings make policy consistent across every surface. RSS makes an agent's data reach explicit. And Cortex AI Gateway makes external tool access approved and logged. Each of these maps to a specific question a security reviewer may ask — questions that now have concrete answers.


## What this means for builders


Governance can sound like friction, but the goal is to give more developers access.


Many developers work in sandboxes because platform teams can't approve production access. The right controls make approval easier and give developers more room to build.


None of this adds work to a builder's day either. Agent profiles are applied by role, so the right model and skills are already loaded when a developer opens a session. Governed MCP connections appear in the client automatically once an administrator enables them, and the developer authenticates once per service rather than locating server URLs and editing JSON configuration files. In the normal case, the governance layer is invisible. It surfaces only when a user is about to do something that would have been a problem regardless.


## Get started


[Download CoCo Desktop](https://www.snowflake.com/en/product/snowflake-coco/downloads/) or open CoCo directly in the Snowsight interface.


Per-user quotas are available in your account today. To set your first limit, see["Cost controls for CoCo" in the Snowflake documentation](https://docs.snowflake.com/en/user-guide/cortex-code/credit-usage-limit) . For organization-wide policy enforcement, see "[Managed settings](https://docs.snowflake.com/en/user-guide/cortex-code/managed-settings) ."


For a deeper look at how tool-call governance works across the platform, read "[Snowflake Launches Cortex AI Gateway and Advanced AI Security at Black Hat 2026](https://www.snowflake.com/en/blog/enterprise-ai-security-agentic-mcp-governance/) ."


1 This content contains forward-looking statements, including about our future product offerings, and are not commitments to deliver any product offerings. Actual results and offerings may differ and are subject to known and unknown risk and uncertainties. See our latest 10-Q for more information.
