---
schema_version: "1.0.0"
document_id: "eb14e1bddd66ab6d347336ad341895112619b9b6625b9a247d554189b9726bab"
company_key: "yc-unbound"
company: "Unbound"
source_id: "yc-unbound-news-import-958423178c7b"
canonical_url: "https://getunbound.ai/blog/aasb-buyers-guide"
published_at: "2026-04-08T00:00:00+00:00"
first_seen_at: "2026-07-24T05:53:51.023478+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:4a0b53ae76f28f9195ae577efbc14f284db05c4068723f8d9e1ca21e9f0fe8ea"
---

# AASB Buyer's Guide: How to Evaluate Agent Access Security Platforms

**You have AI coding agents in your environment. Maybe you approved them. Maybe your developers adopted them on their own. Either way, code is being read, prompts are being sent to external APIs, MCP servers are being connected, and you currently have limited or zero visibility into any of it.**


This guide walks through the capabilities that matter, the questions to ask vendors, and the patterns that separate a real Agent Access Security Broker (AASB) from a checkbox product. If you are still framing the problem, start with[What is an AASB?](https://getunbound.ai/blog/what-is-aasb) .


## Start with Your Actual Risk Profile


Before scoring vendors, score your own exposure.


**How many AI coding agents are active in your org?** If you do not know, that is your first finding.


**Do your developers use MCP?** If agents connect to external tools through MCP, MCP-native governance is a hard requirement.


**What data do your agents have access to?** Map the data exposure before you map the tooling. Code, secrets, customer data, infrastructure credentials, and CI/CD pipelines are common categories.


**What compliance obligations apply?** SOC 2, ISO 27001, NIST AI RMF, EU AI Act, PCI DSS 4.0, HIPAA, FedRAMP, and FFIEC all increasingly require demonstrable governance of automated and non-human access.


## Seven Capabilities That Define a Real AASB


### 1. Agent Discovery and Inventory


Automatically detects AI coding agents in your environment, including unsanctioned ones. Inventories sub-agents, agent rules, IDE extensions, and MCP servers. You cannot secure what you cannot see.


### 2. Posture Analysis


Surfaces risky configurations: auto-approve enabled in production-adjacent environments, broad tool permissions, unsafe allowlists, MCP servers with excessive scope, agent rules that bypass guardrails. Discovery alone produces an inventory. Posture turns it into a risk register.


### 3. Real-Time Session Monitoring


Observes sessions in real time: what was asked, what context was accessed, what tools were invoked, what commands were executed, what data moved. Network-level monitoring tells you traffic went to api.anthropic.com. Session-level monitoring tells you the agent read a production .env file before sending the prompt.


### 4. Semantic Policy Engine


Policies based on what an agent is doing, not just where traffic flows. "Block any agent from including credentials in prompts" is fundamentally different from "block traffic to this IP." Look for an audit-to-warn-to-approve-to-block enforcement path so you can roll out policy progressively.


### 5. MCP-Native Governance


Understands MCP at the protocol level. Inventories servers, validates configuration, enforces per-tool permissions, and monitors the data flowing through MCP connections. A platform that does not understand MCP is operating with a critical blind spot, because MCP is the fastest-growing extension of agent reach into enterprise systems.


### 6. DLP for Agent Workflows


Detects and blocks sensitive data across prompts, model responses, tool calls, terminal output, and MCP messages. Traditional DLP has no coverage for these channels.


### 7. Audit Trail and Compliance Reporting


Produces tamper-evident logs organized for auditors. Maps cleanly to SOC 2, ISO 27001, NIST AI RMF, EU AI Act, and PCI DSS 4.0 evidence requirements for automated and non-human access.


## Questions to Ask Every Vendor


Bring these to a live demo. Slide decks do not count.


1. "Show me what you see when a developer uses Cursor with three MCP servers connected."
2. "Can I write a policy that blocks agents from executing destructive shell commands in production but allows them in staging?"
3. "How do you handle an agent or MCP server we have never seen before?"
4. "Walk me through detecting a developer's agent sending AWS credentials in a prompt."
5. "What happens when an agent switches MCP servers mid-session?"
6. "Show me the audit log for a 30-minute Claude Code session."
7. "How do you handle agents that operate outside the browser, including CLI tools and local model runtimes like Ollama?"
8. "Show me how the platform maps to the OWASP Top 10 for Agentic Applications."


## Red Flags


**"We monitor the model API."** Table stakes, not a platform. Watching the egress endpoint tells you nothing about what the agent did before the request was sent.


**"Our CASB covers this."** It does not. See[AASB vs CASB](https://getunbound.ai/blog/aasb-vs-casb) .


**"We support all agents."** Ask which ones, then ask for a live demo of each.


**"We sandbox the agent."** Containment is not governance. Sandboxes either trade away the productivity reason developers adopted agents in the first place, or they leave the same dangerous actions intact inside the box.


**"That is on the roadmap."** Roadmap features are not features.


**"It is just prompt scanning."** AI coding agents take terminal actions, MCP actions, and file actions. Inspecting the prompt is one of many control points, not the whole control plane.


## Evaluation Framework


## How Unbound Maps to This Framework


Unbound is the first AASB. The platform was built to cover the seven capabilities above without making developers leave the tools they already use.


- Discovery and inventory across Cursor, Claude Code, GitHub Copilot, Windsurf, Amazon Q, sub-agents, agent rules, and MCP servers.
- Posture analysis for auto-approve, autonomy, allowlists, and over-permissive MCP scopes.
- Real-time monitoring of terminal commands, MCP actions, file access, and prompt content.
- A semantic policy engine with progressive enforcement: audit, warn, approve, block.
- Native MCP governance, including allow-listing, per-tool permissions, and data flow inspection.
- DLP across prompts, tool calls, and MCP messages.
- Audit trail mapped to SOC 2, ISO 27001, NIST AI RMF, EU AI Act, and PCI DSS 4.0 control families.


## Take Action


**Start free.** Sign up at[getunbound.ai/free](https://www.getunbound.ai/free) and get visibility into AI coding agents, MCP servers, and risky configurations across your development organization.


**Book a technical assessment.** See discovery, posture, and policy enforcement in action at[getunbound.ai/book-demo](https://www.getunbound.ai/book-demo) .
