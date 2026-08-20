---
schema_version: "1.0.0"
document_id: "96f46b872b5af6c6204ea40bfe254ec01afaf7e397a60b1953c8a1e914a7750f"
company_key: "yc-unbound"
company: "Unbound"
source_id: "yc-unbound-news-import-958423178c7b"
canonical_url: "https://getunbound.ai/blog/owasp-agentic-risks-aasb"
published_at: "2026-03-18T00:00:00+00:00"
first_seen_at: "2026-07-24T05:53:51.023478+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:40842e395c8196670be503a7631ba7531c7121446ea23c514a60641e192a8c43"
---

# How Unbound AASB Addresses Key OWASP Risks for Agentic Applications

*AI coding agents have shifted enterprise risk from model usage alone to live access governance. Once an agent can read code, write code, run commands, call MCP servers, or move sensitive data, security teams need a control plane that makes those actions observable, governable, and enforceable.*


---


## Three Takeaways


- AI coding agents change the security question from model usage to **live access governance** : what can the agent see, touch, and do?
- An **[Agent Access Security Broker (AASB)](https://getunbound.ai/blog/what-is-aasb)** sits between agentic coding clients and the tools, systems, files, data, and actions they can reach.
- Unbound's differentiated value is **broad discovery, runtime visibility, MCP governance, guardrails, approvals, analytics, and heterogeneous tool coverage.**


> **Scope note.** AASB is the live governance layer for agent access and action. It complements — not replaces — IAM, secure runtimes, code scanning, and dependency hygiene.


**Source basis:** OWASP Top 10 for Agentic Applications (selected risks), Unbound's AASB whitepaper materials, and current public Unbound product messaging.


---


## Five OWASP Risks and How Unbound Responds


Each callout below summarizes the risk OWASP describes and the control point Unbound brings as an Agent Access Security Broker.


---


### ASI02 — Tool Misuse and Exploitation


**OWASP risk:** OWASP flags the misuse of legitimate tools when agents are steered by prompt injection, ambiguous instructions, unsafe delegation, or over-privileged access. The danger is not a fake tool; it is a real tool used in the wrong way.


**How Unbound AASB responds:**


- **Discover** 20+ AI coding tools, MCP servers, sub-agents, and agent rules actually in use.
- **Monitor** risky terminal runs and MCP actions in real time by user and application.
- **Enforce** sanctioned-tool and MCP policies, then warn, block, or redact unsafe activity through guardrails.


---


### ASI05 — Unexpected Code Execution (RCE)


**OWASP risk:** OWASP highlights how agentic systems can turn text into executable behavior through shell injection, unsafe package installs, deserialization, or chained tool actions. Generated code can bypass traditional review if execution is not governed.


**How Unbound AASB responds:**


- **Inspect and govern** high-risk terminal commands and execution paths before they run.
- **Apply approval workflows** and policy checks around elevated or destructive actions.
- **Reduce exposure** and blast radius by making execution observable and enforceable across agent workflows.


> **Scope note:** AASB reduces the path to RCE but does not replace sandboxing, runtime isolation, or host hardening.


---


### ASI09 — Human-Agent Trust Exploitation


**OWASP risk:** OWASP describes the way agents can exploit human over-trust through confident explanations, authority cues, or fabricated rationales that persuade users to approve unsafe actions without independent validation.


**How Unbound AASB responds:**


- **Replace blind approval** with policy-backed guardrails and human-in-the-loop control points.
- **Warn, block, or redact** risky outputs and sensitive data movement before the action lands.
- **Create auditable evidence** of who approved what, what the agent attempted, and where policy intervened.


---


### ASI10 — Rogue Agents


**OWASP risk:** OWASP defines rogue agents as malicious or compromised agents that drift beyond intended scope. Individual actions may look legitimate, but the overall behavior becomes deceptive, harmful, or parasitic across human-agent or multi-agent workflows. (For concrete examples of what this looks like in practice, see[Coding Agent Rogue Scenarios](https://getunbound.ai/blog/coding-agent-rogue-scenarios) .)


**How Unbound AASB responds:**


- **Surface hidden sub-agents** , unsafe configurations, and agent rules before drift becomes invisible sprawl.
- **Monitor live actions** across tools, terminals, files, and MCP connections.
- **Apply centralized governance** and least-privilege boundaries so suspicious behavior can be contained quickly.


---


### ASI04 — Agentic Supply Chain Vulnerabilities


**OWASP risk:** OWASP expands supply-chain risk beyond static dependencies to the live runtime ecosystem: MCP servers, plugins, registries, descriptors, third-party agents, models, and update channels that agents dynamically trust and load.


**How Unbound AASB responds:**


- **Inventory** the tools, MCP servers, and external dependencies agents actually connect to at runtime.
- **Enforce allowlists** and sanctioned connections so unsanctioned components stand out immediately.
- **Monitor live behavior** for unsafe tool calls and sensitive data movement triggered by compromised dependencies.


> **Scope note:** AASB governs runtime access and action; it should be paired with signing, provenance, and dependency hygiene upstream.


---


## OWASP Risk to Unbound AASB Control Mapping


This table maps each OWASP risk to the primary Unbound AASB control point and the business outcome it delivers.


OWASP Risk What OWASP Flags Primary Unbound AASB Control Point Business Outcome


**ASI02 — Tool Misuse** Legitimate tools are used unsafely because the agent is misled, over-scoped, or allowed to chain actions without control. Discovery of tools, sub-agents, rules, and MCP servers; real-time monitoring of terminal and MCP actions; sanctioned-tool enforcement. Moves tool risk from implicit trust to explicit policy.


**ASI05 — Unexpected RCE** Generated or delegated code becomes executable through shell commands, packages, unsafe eval, or chained tool paths. Execution-aware monitoring, action-level approval, and policy gates around destructive or elevated runs. Makes risky execution visible before it becomes a live incident.


**ASI09 — Human-Agent Trust** Users over-trust confident recommendations, previews, or rationales and approve unsafe actions. Guardrails, risk cues, sensitive-data controls, and audit-ready evidence around human approvals. Reduces dependence on intuition alone in high-impact workflows.


**ASI10 — Rogue Agents** Compromised or misaligned agents drift from intended goals and continue harmful behavior across workflows. Discovery of hidden agent structures, live activity monitoring, policy boundaries, and rapid containment controls. Turns behavioral drift into a governable event rather than an invisible one.


**ASI04 — Supply Chain** Third-party plugins, registries, MCP servers, descriptors, and other runtime dependencies inject unsafe behavior. Runtime inventory, sanctioned connection policy, and monitoring of live actions triggered by external components. Governs the live agent supply chain where static controls stop short.


---


## Where AASB Fits in the Agent Control Path


Unbound's role as the control plane sits between AI coding agents and the tools, systems, and data they can reach. The AASB layer creates a single governance boundary between agentic coding clients — Cursor, Claude Code, Copilot, Windsurf, and others — and the resources they can act on: terminals, file systems, MCP servers, APIs, databases, and infrastructure.


This architecture means every action flows through a policy layer that can discover, assess, and enforce before the action lands.


---


## Closing the Gap


OWASP's agentic risk model points to the same architectural gap again and again: agents are being given live access to tools, systems, data, and actions without a dedicated governance layer.


Unbound's AASB platform closes that gap by:


- **Discovering** the real agent estate across your organization
- **Monitoring** risky terminal and MCP activity in real time
- **Enforcing** policy on sanctioned tools and connections
- **Applying guardrails** around sensitive or high-impact actions


That does not eliminate the need for secure runtimes, IAM, or dependency hygiene. It does establish the control plane enterprises need if they want to adopt AI coding agents without losing visibility, boundaries, or auditability.


**Unbound is strongest where OWASP risk intersects live tools, MCP connectivity, terminal execution, sensitive data movement, and human approvals.**


---


## Take Action


**[Start free](https://getunbound.ai/risk-assessment)** — Sign up for the Unbound free tier and begin discovering the agents, tools, and configurations running across your development organization today.


**[Book a demo](https://getunbound.ai/book-demo)** — See how Unbound maps to your specific environment, compliance requirements, and risk posture with a guided platform walkthrough.


---


*This post references the OWASP Top 10 for Agentic Applications (Version 2026, December 2025), published by the OWASP Gen AI Security Project — Agentic Security Initiative under Creative Commons CC BY-SA 4.0.*
