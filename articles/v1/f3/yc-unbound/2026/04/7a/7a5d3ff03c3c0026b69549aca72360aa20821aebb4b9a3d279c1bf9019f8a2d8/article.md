---
schema_version: "1.0.0"
document_id: "7a5d3ff03c3c0026b69549aca72360aa20821aebb4b9a3d279c1bf9019f8a2d8"
company_key: "yc-unbound"
company: "Unbound"
source_id: "yc-unbound-news-import-958423178c7b"
canonical_url: "https://getunbound.ai/blog/what-is-aasb"
published_at: "2026-04-06T00:00:00+00:00"
first_seen_at: "2026-07-24T05:53:51.023478+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:b319c08b9b06b16b250454a17f5fab074d73e326fb3af5301e0fd04ebf73623a"
---

# What is an Agent Access Security Broker (AASB)?

**AI coding agents are no longer experimental. Engineering teams use them to write code, query databases, trigger deployments, and act on production systems through tool-use protocols like MCP. Security teams are now dealing with a category of software that acts autonomously, accesses sensitive resources, and operates with the identity and permissions of the developer who invoked it.**


**Traditional security tooling was not built for this. Firewalls inspect network traffic. CASBs govern access to cloud applications. EDR watches endpoints. None of them can see what an AI coding agent is doing inside a session, what data it is pulling through an MCP connection, or whether a prompt contains credentials about to be sent to a third-party model provider.**


**An Agent Access Security Broker, or AASB, is a new category of security infrastructure built specifically for this problem. Unbound is the first AASB.**


## How We Got Here


**The shift from AI assistants to AI coding agents happened fast.**


In 2023, most enterprise AI usage was conversational. Someone pasted code into ChatGPT, got a suggestion, and copied it back. The security risk was real but contained: data left the perimeter through a browser tab, and the blast radius was limited to whatever the user chose to share.


By 2025, that model was obsolete. AI coding agents like GitHub Copilot, Cursor, Windsurf, and Claude Code do not wait for a user to paste code. They read entire repositories. They browse file systems. They execute shell commands. They make network calls. With MCP (Model Context Protocol), they connect directly to external tools and data sources: databases, APIs, cloud consoles, ticketing systems, CI/CD pipelines.


The implications are hard to overstate. An AI coding agent with MCP access is not a chatbot. It is an autonomous actor with the ability to read, write, and execute across the infrastructure it can reach. In most organizations, nobody is watching what it does.


That gap is the largest unmanaged expansion of the enterprise attack surface since cloud. It is also the gap an AASB fills.


## What an AASB Actually Does


An AASB sits between AI coding agents and the resources they interact with. It is a policy enforcement and visibility layer for agentic AI activity. Architecturally, it is analogous to a CASB. Functionally, it is purpose-built for the way agents work: tool calls, MCP connections, terminal commands, and data flows that traditional controls cannot see.


Three capabilities define an AASB.


### 1. Visibility and Discovery


Before you can govern AI coding agents, you need to see them. An AASB provides a real-time inventory of which agents are active in your environment, how they are configured, what tools and MCP servers they are connected to, and what risky settings (auto-approve, broad tool permissions, unsafe allowlists) are in place.


That includes agents your teams adopted through official channels and agents they started using on their own. Shadow agent usage is widespread, and most security teams have no telemetry on it at all.


The visibility layer captures agent sessions, tool invocations, MCP connections, terminal commands, and the full context of what an agent was asked to do and how it responded. This produces an audit trail most organizations currently lack entirely.


### 2. Risk Assessment and Posture Analysis


Discovery surfaces what exists. Posture analysis tells you which of those things are dangerous.


An AASB evaluates agent configurations against known risk patterns: auto-approve enabled in environments that touch production, MCP servers with excessive permissions, tool allowlists that include destructive commands, agent rules that bypass intended guardrails, and configurations that create paths from a developer workstation into sensitive systems.


This is the layer that turns an inventory into a risk register.


### 3. Governance and Policy Enforcement


Visibility and posture matter, but they are not enough on their own. An AASB enforces policy on agent behavior in real time.


That means rules like:


- Block any agent from including credentials, secrets, or PII in prompts
- Require human approval before destructive terminal commands run against production
- Allow MCP connections only to approved servers, and only with scoped permissions
- Audit every agent action with full session context for compliance
- Warn on risky commands, block on catastrophic ones, allow safe defaults


These policies operate at the action level, not the network level. An AASB understands the semantics of what an agent is doing, not just the packets it generates.


## Why CASBs Do Not Solve This Problem


CASBs were built to govern user access to cloud SaaS apps. They operate at the application layer, typically through API integrations or inline proxies that inspect traffic between users and cloud services.


They cannot answer questions like "what did the agent read from the local file system before sending a prompt to Anthropic's API?" or "which MCP servers is this Cursor instance connected to, and what data is flowing through those connections?"


The mismatch comes down to three things:


**Scope.** CASBs see cloud application access. AI coding agents operate across local environments, model APIs, MCP servers, terminals, and cloud services simultaneously.


**Granularity.** CASBs operate at the user and application level. AI agent governance requires visibility at the session, prompt, tool-invocation, and command level.


**Protocol.** CASBs understand HTTP, SAML, and OAuth. They do not understand MCP, function-calling, or the internal tool-use protocols agents use.


For a detailed breakdown, see[AASB vs CASB: What's the Difference?](https://getunbound.ai/blog/aasb-vs-casb)


## The Threat Model for AI Coding Agents


The risks are not theoretical. They are documented.


**Data exfiltration through prompts.** Every prompt sent to an external model API is a potential data leak. Agents automatically include file contents, code, and environment variables as context. The EchoLeak vulnerability in Microsoft Copilot demonstrated how prompt-borne data could exfiltrate without user awareness.


**Malicious or compromised MCP servers.** An agent that connects to a compromised MCP server can be instructed to take arbitrary actions. The Amazon Q Developer supply chain incident in 2025 showed how a malicious update to a developer-facing AI tool could ship destructive code to production environments.


**Privilege escalation through tool use.** Agents inherit the permissions of the user who invoked them. The Replit agent that deleted a production database in 2025 acted within its granted permissions. There was no exploit. There was no policy stopping it.


**Prompt injection through repository content.** If an agent reads a repository that contains adversarial instructions in comments, configuration files, or markdown, it can be manipulated. Cursor RCE vulnerabilities and Salesforce Agentforce ForcedLeak are recent examples.


**Shadow agent proliferation.** Developers adopt new AI coding tools constantly. Most organizations have no mechanism to detect when a new agent or MCP server appears.


These risks are formalized in the OWASP Top 10 for Agentic Applications (v2026, published December 2025). For full mapping to AASB controls, see[OWASP Agentic AI Risks and AASB](https://getunbound.ai/blog/owasp-agentic-risks-aasb) .


## What to Look For in an AASB


1.


**Agent discovery and inventory.** Can the platform automatically detect AI coding agents and MCP servers, including unsanctioned ones?


2.


**Real-time session monitoring.** Can it observe agent activity at the session, prompt, tool, and terminal command level?


3.


**Policy engine with semantic awareness.** Can you write policies based on what an agent is doing, not just where traffic is going?


4.


**MCP-native governance.** Does it understand MCP at the protocol level, including server inventory, scoped permissions, and data flow?


5.


**Data loss prevention for agent workflows.** Can it detect and block sensitive data across prompts, responses, tool calls, and MCP messages?


6.


**Human-in-the-loop approvals.** Can it route high-risk actions to a developer or security reviewer before they execute?


7.


**Integration with the existing security stack.** Does it feed telemetry into your SIEM and identity provider?


8.


**Audit trail and compliance reporting.** Can it produce evidence aligned to SOC 2, ISO 27001, NIST AI RMF, EU AI Act, and PCI DSS 4.0 requirements for automated access?


For a complete evaluation framework, see[AASB Buyer's Guide](https://getunbound.ai/blog/aasb-buyers-guide) .


## Where AASB Fits in the Security Stack


## Getting Started


If your engineering teams are using AI coding agents, you already have the risk profile AASBs were designed for. Most organizations underestimate their exposure because the agents do not show up in CASB, EDR, or DLP telemetry.


**Start free.** Sign up for Unbound at[getunbound.ai/free](https://www.getunbound.ai/free) for immediate visibility into the AI coding agents, MCP servers, and risky configurations across your development organization.


**Book a demo.** See discovery, posture analysis, and policy enforcement in action at[getunbound.ai/book-demo](https://www.getunbound.ai/book-demo) .


## Frequently Asked Questions


**What does AASB stand for?** AASB stands for Agent Access Security Broker. It is a control plane that provides visibility, posture analysis, and policy enforcement for AI coding agents in enterprise environments.


**How is an AASB different from a CASB?** A CASB governs user access to cloud SaaS applications. An AASB governs AI coding agent behavior, including tool use, MCP connections, terminal commands, and data flows that fall outside the scope of cloud application access. They are complementary.


**Do I need an AASB if I already have DLP?** Traditional DLP does not monitor data flowing through AI agent prompts, tool invocations, or MCP connections. An AASB closes that blind spot.


**What is MCP, and why does it matter?** MCP (Model Context Protocol) is the standard protocol for connecting AI agents to external tools and data sources. Each MCP server is an extension of your agent's reach. An AASB provides governance over MCP connections specifically.


**Which AI coding agents does an AASB cover?** A well-architected AASB covers Cursor, Claude Code, GitHub Copilot, Windsurf, Amazon Q Developer, and others. Coverage should be agent-agnostic.


**How long does it take to deploy Unbound?** Most teams achieve discovery and visibility within a day of signing up for the free tier. Policy enforcement is typically rolled out in audit mode first, then progressively moved to warn, approve, and block as the organization tunes thresholds.
