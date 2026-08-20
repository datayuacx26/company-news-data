---
schema_version: "1.0.0"
document_id: "c7af7656cf8ab88ec4516a29e56751be7a508e0406255f02df729ec9ba0e3ab3"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-f76a50baf54b"
canonical_url: "https://www.okta.com/newsroom/articles/fewer-tools-fewer-tokens-how-okta-cuts-agent-tool-selection-costs-before-they-happen/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T15:59:30.887584+00:00"
fetched_at: "2026-08-12T15:59:32.637311+00:00"
content_hash: "sha256:3c4b4a3c6629cb525dbde6ad68461889a6bdb591622220ad0abfbca8aaf4c7d7"
---

# Fewer tools, fewer tokens: How Okta cuts agent tool-selection costs before they happen

Every AI agent's model call includes a menu of every tool it might use. Even the ones it will never touch.


That's true whether the agent is connecting to Google Workspace, Slack, or an internal MCP server with a hundred tools bolted on over several years. Every tool an MCP server exposes gets rendered into the model's prompt as a schema, name, description, and parameters, and that happens on every single turn. You need to pay for the model to reason about that tool, whether or not it ever calls it.


We call this the tool tax. And it's been invisible to most AI teams, because it doesn't show up as a rejected call or a security incident. It shows up as a token bill that's harder to explain than it should be.


## AI agents are only as capable as what they're connected to


MCP servers are one of the fastest-growing ways enterprises connect agents to tools and data. That growth has created a "we need everything connected to everything else" pressure inside IT. Teams deploy more MCP servers faster than they can govern them, and the result shows up as both a security exposure and a cost problem.


More tools exposed per server means more tokens spent per agent decision, on tool selection alone, before any actual work happens. A single popular MCP server can have thousands of active users, and each of them pays the tool tax at every turn. The tool-count problem and the user-count problem compound.


Rejecting an unauthorized tool call after the fact doesn't get those tokens back. They were already spent in the prompt phase, before the model tried anything. And once an agent has been using a tool it shouldn't have access to, walking that back is its own project: Someone has to notice, investigate, and revoke the access. The tokens stay spent regardless.


Scoping upstream means there's nothing to claw back in the first place.


## Okta's blueprint asks a more specific version of "what can they connect to?"


The[blueprint for the secure agentic enterprise](https://www.okta.com/solutions/secure-ai/agentic-enterprise-blueprint/) asks every organization running AI agents to answer three questions: Where are my agents, what can they connect to, and what can they do?


This capability is the direct output of the second question, answered at a finer grain than most teams think to check. "What can they connect to" refers not just to the MCP servers an agent can reach, but also to the specific tools that agent is permitted to use for a particular user.


Least-privilege access means an agent should never be aware of resources, databases, or tools it isn't explicitly authorized to use. Stripping away unauthorized tools to secure the enterprise also reduces the tool tax.


## How it works


Okta scopes the tool list at the point where an agent connects to an MCP server. Within the Okta dashboard, an admin configures which tools a given identity is entitled to use. Okta then returns only the scoped set, rather than everything the server exposes.


A shorter list gets injected into the agent's prompt on every turn, which means less token spend per turn, before the agent ever attempts a call. Unauthorized tools are not in the list the model sees, and scope is checked again at runtime before any tool call executes.


Fill out the form to access this content.


## What that's worth


Scoping the tool list to what an agent's identity is actually authorized to touch removes most of the catalog before it ever reaches the model. In internal modeling across a realistic mix of permission levels, some scenarios cut visible tools by more than 90%. Tool-schema cost, the part of the bill tied to describing tools in the prompt, falls by roughly the same margin.*


Fill out the form to access this content.


## Why this is an identity and authorization problem, not a gateway problem


Gateways cap spend by key, team, or group, the finest grain their data gives them. That's a real capability, and for routing and rate-limiting, it's the right layer to own it. But capping spend after the fact makes whoever owns the gateway the token police almost by default: Someone has to set the limits, defend them when a team pushes back, and take the call when a user hits a wall mid-task. That's not a failure of the gateway. It's what happens when the only lever you have is a spending cap instead of a scope.


Gateways meter what already happened: tokens in, tokens out, dollars spent. They can cap the damage after a decision gets expensive. They can't stop the decision from being expensive in the first place because group-level information can't tell them what one specific agent, or the person behind that agent, is actually entitled to touch.


The identity layer can. With per-user and per-agent entitlements instead of group membership, Okta filters a tool list at a resolution gateways don't have access to. The gateway meters what gets through. The identity layer shrinks what there is to meter, so there's less left to police.


"Cost control for agents is best provided using identity governance tools that offer more granular control and precision without disrupting business processes. Okta's approach is an elegant way to do this because it leverages the same entitlement data that governs security, not a separate metering layer without that insight," says Paul Webber, Principal Cybersecurity Industry Analyst with Software Analyst Cyber Research.


## The bigger point


Cost is only one part of this. Stripping away tools and an identity that isn't authorized to use them also strips away what that identity could do if it were compromised. The same scoping that keeps the prompt lean keeps the blast radius small.


Least-privilege MCP reduces both the tokens a model has to reason about and the damage an attacker could do with the tools left standing.


What’s next? Check out our[AI simulator](https://www.okta.com/assessments/ai-blueprint) to run real attack scenarios, evaluate your coverage, expose security gaps, and understand how different controls can reduce risk and blast radius.


** **Methodology** . The reduction figures in this post come from Okta internal modeling, using Okta product data and public vendor documentation only. No customer data was used. We modeled a single MCP client with access to a catalog of enterprise tools, then compared the number of tools visible to the model before and after identity-based scoping. Tool-count reduction is calculated as one minus the ratio of scoped tools to unscoped tools. To estimate scoped exposure, we mapped Okta MCP Server tools to the OAuth scopes that unlock them, defined representative user segments (helpdesk read-only, helpdesk operator, app admin, brand and email admin, and super admin), and weighted each segment by an assumed share of monthly traffic. Tool-schema token cost tracks tool count nearly linearly, because each tool contributes a name, description, and parameter schema to the prompt on every turn, so percentage reductions in schema cost closely follow percentage reductions in tool count. Absolute token and dollar figures depend on average schema size, request volume, and model pricing, all of which vary by deployment, which is why this post reports reductions as percentages rather than dollars. Actual results will vary with your tool catalog, permission distribution, and model choice.*


About the Author


Harish Peri


SVP and General Manager for AI Security


Harish Peri is the SVP and General Manager for AI Security at Okta. In this role he is responsible for the product strategy, GTM strategy, commercial execution and customer experience for securing agentic AI. He has over 20 years of experience spanning engineering, product management, marketing and general management, across multiple industries including Financial Services, Technology and Human Capital Management.


Jenna Cline


SVP, Technology, Data & Intelligence


Jenna Cline is the Senior Vice President of Technology, Data, and Intelligence at Okta, where she leads the creation and scaling of workforce technology solutions. A champion of innovation and workplace efficiency, she recently spearheaded the rollout of dozens of AI tools across the organization, saving hundreds of thousands of hours in productivity. Prior to joining Okta, she helped drive the future of remote and hybrid work as the Head of Enterprise Technology at Atlassian.
