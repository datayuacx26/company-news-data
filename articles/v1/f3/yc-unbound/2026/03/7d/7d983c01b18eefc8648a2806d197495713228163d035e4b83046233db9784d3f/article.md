---
schema_version: "1.0.0"
document_id: "7d983c01b18eefc8648a2806d197495713228163d035e4b83046233db9784d3f"
company_key: "yc-unbound"
company: "Unbound"
source_id: "yc-unbound-news-import-958423178c7b"
canonical_url: "https://getunbound.ai/blog/ai-coding-agent-aasb-glossary"
published_at: "2026-03-29T13:00:00+00:00"
first_seen_at: "2026-07-24T05:53:51.023478+00:00"
fetched_at: "2026-07-28T21:56:52.525405+00:00"
content_hash: "sha256:12bdc8e20255e48ccd5121ed535c139b0e8fb5a2973ef70bf2713a1e945a47e5"
---

# The AI Coding Agent and AASB Glossary

This glossary defines the core terms security, engineering, and compliance leaders need to reason about AI coding agents and the governance layer that controls them. The scope is deliberately narrow: AI coding tools, Model Context Protocol (MCP) servers, agent behavior, and the Agent Access Security Broker (AASB) category. General large language model or AI ethics terminology is out of scope.


## How to Use This Glossary


Each entry includes a short definition, a "why it matters" line, and cross-links to related terms. Scan by the first-letter section below, or jump directly to a term using your browser's table of contents. New terms will be added as the category develops.


For context on the category as a whole, start with the[ACSM vs AASB post](https://getunbound.ai/blog/acsm-vs-aasb-governance-not-management) . For MCP-specific risk, see[Every Known MCP Attack Pattern, Mapped](https://getunbound.ai/blog/every-known-mcp-attack-pattern-mapped) .


---


## A to C


### Agent Access Security Broker (AASB)


The security category that governs what AI coding agents can access and what actions they can take. AASB sits between the agent and enterprise systems, providing discovery, posture assessment, and runtime enforcement. Think of it as CASB for AI coding agents.


**Why it matters:** Existing categories (CASB, EDR, SAST, DLP) do not cover agent behavior. AASB is the new control plane.


**See also:** Governance Gap, Control Plane, CASB comparison.


### Agent Mode


The autonomous operational mode of an AI coding tool, distinct from autocomplete or suggestion modes. In agent mode, the tool reads files, edits code, executes terminal commands, and calls external tools through MCP servers without line-by-line developer approval.


**Why it matters:** Agent mode is where the security model changes. An agent-mode action can reach operations (file deletion, command execution, egress calls) that an autocomplete suggestion cannot.


**See also:** Auto-Approve, Blast Radius, Human-in-the-Loop.


### Agent Rules


Instructions embedded in a repository or workstation that steer agent behavior. Examples include` .cursorrules` ,` CLAUDE.md` , and` .windsurfrules` files. They shape default tool use, style, and sometimes permissions.


**Why it matters:** Agent rules can override organizational expectations. A rule file can instruct an agent to auto-approve operations that the security team wanted reviewed. Discovery of these files is part of AASB posture assessment.


**See also:** Discovery, Over-Permissioned, Shadow AI.


### Auto-Approve


A configuration setting that lets an AI coding agent take actions without per-action human confirmation. Applies to file edits, terminal commands, and MCP tool calls.


**Why it matters:** Unbound scan data shows 30 to 50 percent of agent configurations have auto-approve enabled, often on dangerous operation classes. It is one of the highest-impact controls an AASB policy can adjust.


**See also:** Agent Mode, Blast Radius, Progressive Enforcement.


### Blast Radius


The scope of systems, data, or accounts affected by a single agent action. A read-only query has a small blast radius. A DROP TABLE on a production database has a large one.


**Why it matters:** Governance policy should scale with blast radius. Read operations can often run in audit-only mode. Destructive or exfiltration-prone operations warrant approval or block.


**See also:** Progressive Enforcement, Human-in-the-Loop.


### Chain-of-Trust


The transitive trust relationship when an agent invokes an MCP server, which invokes a sub-agent, which invokes another MCP server. Each hop can alter the data, credentials, or instructions that reach the next one.


**Why it matters:** Trust does not stop at the first MCP connection. Chain-of-trust abuse is a distinct attack class in the[MCP attack pattern taxonomy](https://getunbound.ai/blog/every-known-mcp-attack-pattern-mapped) .


**See also:** MCP Connection, Sub-agent, Tool Poisoning.


### Claude Code


Anthropic's terminal-based AI coding agent. Supports file editing, terminal command execution, MCP server connections, sub-agents, and hook-based customization.


**Why it matters:** One of the two most prominent AI coding agents in enterprise environments. Its hook system offers on-host runtime controls that complement but do not replace an AASB layer.


**See also:** Cursor, Hooks, MCP Server.


### Control Plane


The centralized layer that issues policy decisions and collects telemetry across distributed agents and their tool connections. AASB provides the control plane for AI coding agents.


**Why it matters:** Without a control plane, policy enforcement is local and inconsistent. Central governance requires a single source of truth for policy.


**See also:** Policy as Code, Enforcement.


### Cursor


Anysphere's IDE-based AI coding agent. Supports agent mode, MCP servers, background agents, and workspace-level rules.


**Why it matters:** Claims adoption inside a large share of the Fortune 500. Any AASB program needs first-class Cursor discovery and policy support.


**See also:** Agent Mode, Agent Rules.


---


## D to I


### Discovery


The first pillar of AASB. Continuous inventory of AI coding agents, their configurations, MCP server connections, sub-agents, agent rules, and the permissions associated with each.


**Why it matters:** Organizations typically find 3 to 4 times more AI coding tools in active use than IT or security was tracking.


**See also:** Shadow AI, Posture Assessment.


### Enforcement


The third pillar of AASB. Applying policy to agent actions in real time. The enforcement spectrum, from lightest to strictest: audit, warn, approve, block.


**Why it matters:** Discovery and assessment produce the risk numbers. Enforcement reduces them over time.


**See also:** Progressive Enforcement, Policy as Code.


### Governance Gap


The space between existing security tools (SAST, DAST, SCA, EDR, CASB, DLP, IAM) and the live behavior of AI coding agents. None was built to observe or control what an agent does in real time.


**Why it matters:** The governance gap is why AASB exists as a category. Every unfilled gap is a specific control AASB provides.


**See also:** AASB, Control Plane.


### Governed Path


The default route an AI coding agent takes when policy has been applied well: it can use sanctioned tools and MCP servers freely, while destructive or data-sensitive actions require approval. The governed path is where compliance is also the easiest path for the developer.


**Why it matters:** Governance that turns the safe path into the slow path gets routed around by developers. The governed path concept aligns productivity and control rather than putting them in tension.


**See also:** Progressive Enforcement, Developer Velocity.


### GitHub Copilot


Microsoft and GitHub's AI coding agent. Available in suggest mode and an expanding agent mode, with MCP support rolling out across editors.


**Why it matters:** Reportedly 20 million users across 77,000 enterprises. The scale makes it a priority target for any AASB policy program.


**See also:** Agent Mode, MCP Server.


### Hooks


Runtime interception points inside an AI coding agent. Claude Code exposes PreToolUse, PostToolUse, UserPromptSubmit, Stop, and SubagentStop hooks. Hooks run local scripts before or after specific agent actions.


**Why it matters:** Hooks are a first line of on-host enforcement. They are tool-specific, local-only, and not a substitute for a cross-tool control plane. See the[Claude Code Hooks cheat sheet](https://getunbound.ai/blog/claude-code-hooks-security-cheat-sheet) .


**See also:** Control Plane, Progressive Enforcement.


### Human-in-the-Loop (HITL)


An enforcement mode where an agent action cannot complete until a human explicitly approves it. Typically used for high-blast-radius operations such as production writes, destructive commands, or data exports.


**Why it matters:** HITL is the bridge between audit-only and full block. It preserves agent utility for high-risk operations while keeping a human on the decision.


**See also:** Blast Radius, Approval Workflow.


---


## M to P


### MCP (Model Context Protocol)


An open protocol, originally published by Anthropic, that standardizes how AI agents connect to external tools, data sources, and services. Servers expose capabilities. Clients (agents) call them.


**Why it matters:** MCP turned AI coding agents from self-contained tools into networked actors. Most of the new agent risk surface lives on the MCP boundary.


**See also:** MCP Server, MCP Connection, Chain-of-Trust.


### MCP Connection


The live session between an agent and a specific MCP server. Each connection carries a set of exposed tools, authentication state, and a trust boundary.


**Why it matters:** Unbound data shows an average of 8 to 15 MCP connections per developer environment. Each is a potential path for data exfiltration, prompt injection, or privileged action.


**See also:** Over-Permissioned, Tool Poisoning.


### MCP Gateway


An intermediary that sits between agents and MCP servers, brokering calls, enforcing policy, and producing audit logs. A gateway is one architectural pattern for AASB enforcement.


**Why it matters:** Gateways make it possible to apply uniform policy to many MCP servers without modifying each one.


**See also:** Policy as Code, Control Plane.


### MCP Server


A process, container, or hosted service that implements the MCP protocol and exposes tools to agents. Examples include GitHub MCP, Slack MCP, filesystem MCP, and database MCP servers.


**Why it matters:** Servers are the concrete units an AASB policy governs. Allow-lists, deny-lists, and per-tool permissions are expressed at the server level.


**See also:** MCP Connection, Tool Poisoning.


### Over-Permissioned


A configuration state where an MCP connection, auto-approve flag, or agent scope grants broader access than the developer actually needs for daily work. The typical signal is production write access in a tool used primarily for reads.


**Why it matters:** Unbound scan data shows 83 percent of MCP connections are over-permissioned. Reducing these is the highest-impact posture improvement an AASB program delivers.


**See also:** Blast Radius, Progressive Enforcement.


### Policy as Code


Expressing governance rules for agent behavior in a machine-readable, version-controlled format. Similar to infrastructure-as-code, applied to AASB policy.


**Why it matters:** Policy as code enables review, audit, and rollback of governance decisions the same way engineering teams handle infrastructure.


**See also:** Control Plane, Enforcement.


### Progressive Enforcement


The staged path from audit-only visibility to hard blocks. Audit, then warn, then approve, then block. Teams move along the spectrum as policy confidence grows.


**Why it matters:** Hard blocks rolled out on day one tend to break developer workflows. Progressive enforcement is the sequencing pattern that ships without generating the pushback that stalls adoption.


**See also:** Governed Path, Human-in-the-Loop.


### Prompt Injection


A class of attack where untrusted text steers an AI agent to take unintended actions. In coding agents, the injection surface includes READMEs, docstrings, issue text, PR comments, and MCP tool output.


**Why it matters:** Detection alone is unreliable. Policy-based enforcement on sensitive actions is the stronger control. See[Prompt Injection in Coding Agents](https://getunbound.ai/blog/prompt-injection-in-coding-agents) .


**See also:** Indirect Injection, Tool Poisoning.


---


## Q to Z


### Red Teaming


Adversarial testing of agent and MCP configurations, covering direct and indirect prompt injection, tool poisoning, exfiltration chains, and confused-deputy scenarios.


**Why it matters:** Red teaming produces the concrete scenarios an AASB policy needs to cover. MITRE ATLAS adaptations are increasingly common as the reference framework.


**See also:** Prompt Injection, Tool Poisoning, Chain-of-Trust.


### Sandbox


A constrained execution environment that limits what the agent can reach (filesystem, network, processes). Examples include E2B, Docker-based sandboxes, and remote ephemeral environments.


**Why it matters:** Sandboxing reduces blast radius by removing access. It is complementary to AASB, not a replacement. Governance controls what the agent does inside the reachable surface.


**See also:** Blast Radius, AASB.


### Shadow AI


AI coding agents and MCP servers installed and used by developers without IT or security approval. The AI-specific parallel to shadow IT.


**Why it matters:** The scale is significant. Organizations typically find 3 to 4 times more agents in use than they were tracking. See[Shadow AI Coding Agents](https://getunbound.ai/blog/shadow-ai-coding-agents) .


**See also:** Discovery, Agent Rules.


### Sub-agent


An agent spawned or directed by another agent. Examples include Claude Code's sub-agent pattern and multi-agent orchestrations where a planner agent delegates to specialists.


**Why it matters:** Sub-agents inherit and sometimes expand the parent's permissions. They widen the chain-of-trust surface and require their own discovery and policy treatment.


**See also:** Chain-of-Trust, Discovery.


### Tool Poisoning


An attack where a malicious or compromised MCP server presents tool definitions that manipulate agent behavior. Includes typosquatted MCPs, function shadowing, and descriptions crafted to bias tool selection.


**Why it matters:** Tool poisoning bypasses input filters because the payload is the tool metadata itself. Server allow-listing and provenance checks are the primary defenses.


**See also:** MCP Server, Prompt Injection.


### Windsurf


Codeium's IDE agent. Supports agent mode, workspace rules, and MCP connectivity.


**Why it matters:** A growing presence in enterprise fleets, particularly where teams want an alternative to Cursor or Copilot. AASB discovery must cover it.


**See also:** Cursor, Agent Mode.


---


## Related Categories at a Glance


Category What it covers What it does not cover


AASB AI coding agent behavior, MCP server calls, runtime action policy Traditional cloud app access, endpoint process telemetry, application source scanning


CASB Cloud app access, data flows to and from SaaS AI agent-initiated actions, MCP tool calls, on-host agent behavior


AppSec (SAST, DAST, SCA) Code vulnerabilities, application runtime weaknesses, dependency risks The agent that produced the code and the tools it can reach


EDR Process, file, and network telemetry at the endpoint Agent intent, MCP tool calls, prompt injection, auto-approve posture


AASB is complementary to these categories, adding a control plane where none previously existed rather than replacing any of the tooling that already works in its own scope.


## Run a Discovery Scan


Most of the terms in this glossary describe controls you can only apply once you know what is in your environment.


**Start free.** Run a free agent discovery scan to see which of these apply to your organization. Sign up at[getunbound.ai/free](https://www.getunbound.ai/free) .


**Book a demo.** See AASB discovery, assessment, and enforcement running against a live environment at[getunbound.ai/book-demo](https://www.getunbound.ai/book-demo) .


---


*This glossary is updated as the AASB category and MCP ecosystem evolve. Suggest a term or flag an error by contacting the Unbound Security team.*
