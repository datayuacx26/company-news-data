---
schema_version: "1.0.0"
document_id: "0a6e148f3bc911728b3d244870f8014a76d5d586266d223fc7f08b7c5b5b6560"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/product-innovation/identity-governance-for-ai-agents/"
published_at: "2026-06-17T07:00:00+00:00"
first_seen_at: "2026-07-24T07:13:30.013467+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:8562fdb7bd3beb3da7fadf9e2050a01fc3bdd6834dee78e45cb0137030bc969b"
---

# Identity governance for every agent handoff, action, and tool

***Editor’s note:** Learn how to unlock this in the[latest streamcast episode](https://www.okta.com/webinars/hub/okta-streamcast-special-agentic-enterprise-blueprint/) , “The blueprint for the secure agentic enterprise”, where we're moving from theory to proof. You’ll learn exactly how the blueprint for the secure agentic enterprise turns into practical controls for tools like Claude Code and Cursor.*


Your AI teams can't afford to let governance gaps slow down deployment. They need to put AI agents to work now.


According to a[2026 survey](https://www.gravitee.io/state-of-ai-agent-security) of senior technology leaders, 90% of organizations have no way to govern what agents in production are actually doing. Around 54% have already suffered a security incident related to an agent acting unexpectedly.


These agents call APIs, read sensitive records, delegate tasks to other agents, and make decisions without waiting for human input. The question isn't whether your organization has agents operating this way; it's whether you have any meaningful control over what they're doing when they do. For most enterprises today, the honest answer is no.


The reason that the answer is no is almost always an identity governance problem. Every AI agent in production either runs through a governed identity layer or around one. Most are running around one.


## Where identity governance now reaches further


To be a secure agentic enterprise, you need a blueprint that helps you answer three critical questions:


1. Where are my agents?
2. What can they connect to?
3. What can they do?


The capabilities covered in this blog are new enhancements that further advance the[blueprint for the secure agentic enterprise](https://www.okta.com/solutions/secure-ai/agentic-enterprise-blueprint/) on exactly the last two questions. They help AI teams ship faster, and security teams get the real-time policy enforcement, response controls, and auditability they need to say yes.


### What can they connect to?


Somewhere across your organization right now, an AI agent may be quietly handing off work to another agent.


Here's the problem: Every one of those handoffs is an access event the enterprise needs to govern. And without an identity layer, most security teams have no visibility into them.


Most enterprises are building multi-agent workflows in two ways:


- **User-initiated workflows:** An employee asks an executive assistant agent to build a quarterly business review deck. The agent orchestrates a swarm of agents, each acting on the user's behalf and scoped to what the user is allowed to access. The human is the anchor while identity flows through every handoff.
- **Machine-initiated workflows:** A monitoring service detects an anomaly and triggers a diagnostics agent, which delegates to a log-analysis agent to dig deeper. No user session, no human in the loop. The service's authority flows down through every agent in the chain.


Both patterns share the same governance need: Every handoff is an access event that must be authorized, scoped, and logged. When AI agents communicate without a dedicated identity layer, organizations risk losing the ability to verify the identity of the actor behind each request, enforce policy consistently, or trace actions back to their source.


#### Agent-to-Agent Connections: Secure your multi-agent workflows


Agent-to-Agent Connections, a new capability within[Okta for AI Agents](https://www.okta.com/products/govern-ai-agent-identity/) , closes that gap. It lets you define per-agent connection policies, which upstream services and agents can invoke this agent, their scope, and how long the permission lasts. Identity is preserved across every handoff, so every connection remains verifiable throughout the workflow.


Three principles make this work:


- **Explicit allowlists** define which agents can call which other agents.
- **Scoped permissions** give each downstream agent only what it needs for its task.
- **A verifiable chain** travels with every token, so the audit log captures who authorized what.


This is what it means to govern what agents can connect to at the agent-to-agent layer: Which agents can call which other agents, under what conditions, with what scope, and with evidence that can be produced for any auditor or incident response team that asks.


*Availability: Agent-to-Agent Connections Early Access is now available. Read more about[securing your multi-agent workflows with Agent-to-Agent Connections](https://www.okta.com/blog/product-innovation/securing-agent-to-agent-connections/) .*


#### Secure AI coding assistants like Claude Code, Microsoft 365 Copilot, and Cursor


Agent-to-agent connections are one side of the connectivity problem. Another major gap is the ungoverned AI tools your developers are already using to connect to your systems. These include AI coding assistants such as Claude Code, Cursor, Microsoft 365 Copilot, GitHub Copilot, and Glean.


These tools are embedded in daily workflows and connected to internal systems such as Jira, ServiceNow, GitHub, and your internal APIs.


The problem is how they are getting in. Most of these tools weren't built to authenticate through an identity provider. They get access through hard-coded tokens, ad hoc credentials, and API keys stored in configuration files. There's no centralized authorization. There's no audit trail. Security teams often have no visibility into which agents accessed which systems, on whose behalf, or what they did when they got there.


The MCP Bridge addresses this without requiring changes to the agents themselves. A self-hosted, identity-aware proxy sits between your agents and the Model Context Protocol (MCP) tools they connect to. Every call flows through the bridge. Every agent authenticates through your identity provider. Every action lands in your audit log. The agent does not need to be rewritten. The developer also does not need to change their workflow.


The bridge runs in the customer's environment, so data stays within the perimeter. No external service sees what passes through. The customer controls the deployment, the configuration, and the data.


*Availability: The MCP Bridge is a Professional Services offering and requires a Statement of Work (SOW).*


### What can they do?


Governing what agents can do requires the identity layer to go further than initial access. An access token gets an agent through the door; it does not govern what the agent does once it gets there.


#### AI agent authorization: Enforce runtime authorization for every AI agent action


A user’s permissions may change after a token is issued. A contractor may roll off a project. A patient may withdraw consent. A manager may go on vacation. Static roles and access tokens cannot keep up with those changes. Instead of asking, “Does this user or agent have access to the system?” AI agent authorization asks a more precise question: “Can this agent, acting for this user, take this action on this resource right now?”


That distinction matters because authorization conditions change constantly. Fine-Grained Authorization (FGA) is authorization that reads context rather than just role. It checks whether the user an agent is acting on behalf of has the required relationship, current attributes, and policy conditions to take a specific action on a specific resource, at runtime.


That includes direct and inherited relationships, dynamic attributes such as clearance level or on-duty status, and policy thresholds, such as quantity limits, dollar amounts, or data classification rules.


For example, a procurement agent can approve invoices up to $10,000 for the user it's acting on behalf of, but not above that threshold. FGA checks the user's approval limit at runtime, not just their role.


Governing what agents can do requires more than static permissions set at deployment. It requires continuous evaluation of every action, across every agent, in real time. That is how you control what an agent *actually* does.


*Availability: This is a use case implementation of[FGA](https://www.okta.com/blog/product-innovation/setting-the-bar-for-authz-okta-fine-grained-authorization-is-now-generally-available/) and[Okta for AI Agents](https://www.okta.com/products/govern-ai-agent-identity/) .*


#### Kill switch: Shut down a rogue agent the moment you need to


Governance built in from the start reduces the likelihood that something will go wrong, but it doesn't mean it won't. Agents are unpredictable by design. They make decisions, move across systems, and can take actions no one anticipated. When that happens, security teams need to respond immediately.


Many AI agent deployments today have no way to shut down an agent when it behaves unexpectedly. When the board asks what the response plan is if an agent goes rogue, most CISOs don't have an answer.


That changes with a kill switch. For agents connecting through Okta, revoking access cuts off access across connected resources in a single action from the Okta Admin Console. You don't need to confirm an agent is compromised to use it. A misconfiguration, an out-of-scope action, or behavior that raises a question. Any of these is reason enough to deactivate the agent immediately, while you do further investigation into what happened.


The result? AI teams can ship knowing there's a safety net in place. Security teams can approve deployments knowing they can respond if something goes wrong. When you know you can stop it, you can approve it.


*Availability: The kill switch feature is Generally Available and included in[Okta for AI Agents](https://www.okta.com/products/govern-ai-agent-identity/) . Coming soon: Detection that reads risk signals from across the Okta ecosystem, raises the agent's risk score, and triggers the kill switch automatically.*


## Every agent needs an identity layer


These capabilities address different parts of the agent governance problem, but they share a common premise. The identity primitives that secure your people and applications need to extend to agents, too. This includes governing the connections between them, the actions they take, the tools they use, and your ability to stop them when something goes wrong.


Governing agents in this way requires an authoritative, real-time identity layer that already spans your workforce and applications. Okta sits at that layer.


The enterprises that get this right early won't just reduce incidents. They'll move faster. Discovery, security, and governance built in from the start don't slow AI deployment. They're what make scaling possible. Build the foundation now, and your security team stops being the bottleneck on agent rollouts and starts being the reason they succeed.


To learn more about these updates, including demos, watch[the streamcast webinar](https://www.okta.com/webinars/hub/okta-streamcast-special-agentic-enterprise-blueprint/) .


*These materials are intended for general informational purposes only and are not intended to be legal, privacy, security, compliance, or business advice. You are responsible for obtaining security, privacy, compliance, or business advice from your own professional advisors. Any mention of future products, features, functionalities, or certifications in this blog is for informational purposes only. These items are not commitments to deliver and should not be relied upon to make purchasing decisions. © Okta, Inc. and its affiliates. 2026.*


About the Author


[Ariel Zommer Staff Product Marketing Manager Ariel Zommer is a Staff Product Marketing Manager at Okta. Ariel is passionate about cloud technology and how it is transforming the way we work, live, and collaborate every day. Prior to Okta, Ariel has held strategic roles at Socure, OneLogin, and Signifyd.](https://www.okta.com/blog/author/ariel-zommer/)
