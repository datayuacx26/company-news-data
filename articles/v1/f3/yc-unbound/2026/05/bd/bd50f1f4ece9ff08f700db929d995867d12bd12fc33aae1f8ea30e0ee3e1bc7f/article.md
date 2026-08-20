---
schema_version: "1.0.0"
document_id: "bd50f1f4ece9ff08f700db929d995867d12bd12fc33aae1f8ea30e0ee3e1bc7f"
company_key: "yc-unbound"
company: "Unbound"
source_id: "yc-unbound-news-import-958423178c7b"
canonical_url: "https://getunbound.ai/blog/unbound-full-enforcement-github-copilot"
published_at: "2026-05-19T13:00:00+00:00"
first_seen_at: "2026-07-24T05:53:51.023478+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:1dc60a7ede5d79e27cfa88802420a060f3a155123ea91ec0ef13d53d728e85d4"
---

# Unbound Brings Full Enforcement to GitHub Copilot

*Copilot's new lifecycle hooks mean the market's most widely deployed AI coding agent can finally be audited, controlled, and governed with zero compromises.*


Yesterday,[May 18th 2026, GitHub shipped postToolUse hooks in the Copilot CLI](https://github.com/github/copilot-cli/releases) . It is the lifecycle event that fires after Copilot invokes a tool and before the result lands in the agent's context. It is the same primitive that Claude Code, Codex, and Cursor have exposed for months. Copilot was the last holdout.


That hook is the reason **Unbound AI** can now govern Copilot the same way it governs every other AI coding agent in the enterprise. Audit. Warn. Approve. Block. Identical policy surfaces across Claude Code, Codex, Cursor, and now Copilot.


This is the missing piece. Copilot is the agent with the deepest install base in regulated companies and was, until yesterday, also the agent with the thinnest enforcement surface.


## What postToolUse hooks actually are


A hook is a callback registered against an agent lifecycle event. When the event fires, the agent pauses, hands control to the hook, and waits for a return value before continuing.


The hook taxonomy that matters for governance is small. preToolUse fires after the model proposes a tool call but before the tool runs. A policy engine can inspect the proposed action and approve, modify, or block it. postToolUse fires after the tool runs but before the result is incorporated into the agent's context. A policy engine can inspect what came back, scrub secrets or PII, redact, or stop the result from informing the agent's next step.


Both events are required for real enforcement. preToolUse stops actions you do not want the agent to take. postToolUse stops information you do not want the agent to act on. Without postToolUse, an MCP server can return a credential, a customer record, or a piece of regulated data, and the agent plans its next move around it before any control point sees the payload.


Claude Code, Codex, and Cursor have shipped both hook types for some time. Unbound has been writing policy against them since they launched. Copilot had the broader install base and a slower hook surface. Discovery and observability worked. Enforcement did not. As of yesterday, it does.


## Why this matters at Copilot's scale


The math on Copilot is unique among AI coding agents. The other tools in the category are growing fast on a developer-driven basis. Copilot grew on an enterprise-procurement basis, which is why its footprint inside regulated companies is larger than the next several agents combined.


Current numbers from public reporting:


- 20 million developers using Copilot, with 4.7 million paid subscriptions as of January 2026
- Deployed at approximately 90 percent of Fortune 100 companies
- More than 50,000 organizations on Copilot Business and Copilot Enterprise
- 42 percent share of the AI coding tools market in 2025
- Copilot now generates 46 percent of code written by developers who use it


For a CISO whose stack already includes a SAST tool for code, an EDR for endpoint, and a CASB for SaaS, Copilot is the surface that has been hardest to see. Most enterprises run official Copilot adoption alongside unofficial Cursor and Claude Code usage. Coverage of the unofficial tools was already possible with Unbound. Coverage of the official one, running in agent mode with MCP servers attached, was not.


## What Unbound governs on Copilot today


With postToolUse in place, Copilot moves into the same enforcement profile that Unbound already applies to the other three vendors.


**Discover.** Unbound inventories every Copilot installation across the org, including version, configured hooks, MCP servers attached, and which sub-agents and rules are present in` .github/copilot/settings.json` or` ~/.copilot/hooks` . The same scan covers Claude Code, Codex, Cursor, Cline, and any tool that speaks MCP.


**Assess.** Unbound scores each Copilot deployment against the same risk model used for the other agents. Auto-approve enabled. Broad write permissions on MCP connections. Unsanctioned tool usage. Configuration drift from the org standard. The unified scoring matters because Copilot risk no longer reads differently than Cursor risk on the same dashboard.


**Enforce.** This is the part that was missing. Unbound can now write policy against Copilot agent behavior at four levels:


- **Terminal command controls.** Audit, warn, or block commands the Copilot agent attempts to run, including the destructive patterns (` rm -rf` ,` git push --force` ,` chmod 777` across project roots) that Unbound flags by default.
- **MCP action governance.** Per-tool permissions on the Copilot MCP servers, including read versus write, scoped by repository or environment.
- **Data guardrails.** Secret scanning, PII detection, and pattern matching on data returned to Copilot through postToolUse, with redaction or blocking before the agent acts on it.
- **Human-in-the-loop approval.** High-blast-radius actions, including writes to production systems, schema changes, and mass deletes, can route to a reviewer before Copilot proceeds.


The enforcement spectrum (audit, warn, approve, block) is identical to the one Unbound runs against Claude Code, Codex, and Cursor. A security team writes one policy. It applies everywhere.


## The feature-complete claim, examined


Feature completeness is a strong claim and worth defining carefully. Unbound's position is specific: across the four AI coding agents that account for the majority of enterprise usage (Claude Code, Codex, Cursor, and GitHub Copilot), Unbound now supports the full Discover, Assess, and Enforce loop with the same policy primitives on each.


Parity matters for two reasons.


**First** , security teams cannot manage a policy surface that looks different on every tool. If MCP governance works one way on Cursor and another on Copilot, the people writing policy have to remember which is which. They will not. They will write the lowest common denominator policy that works on the weakest hook surface, which is how governance erodes in production. Parity collapses that surface to one.


**Second** , the AI coding agent market is consolidating around a small set of tools that share a similar lifecycle. The vendors who run agents (Anthropic, OpenAI, GitHub) are converging on hook semantics that look approximately like the model Anthropic shipped first. The agents that do not converge will not be procurable inside enterprises with mature security functions. Unbound's bet has been that the hook surface stabilizes, and the platform that covers it consistently is the one that wins the AASB category. The Copilot release is evidence the bet is playing out.


## What this changes for an enterprise running Copilot today


If your organization has Copilot Business or Copilot Enterprise deployed and no enforcement layer on top, the practical implication is that controls you may have assumed required a Copilot replacement no longer do. The session-level governance that Unbound applies to Claude Code and Cursor now applies to Copilot, on the agent your developers are already using.


That removes a real friction point. Most security teams looking at AI coding governance have had to choose between blocking Copilot, building shadow infrastructure to inspect Copilot traffic, or accepting that Copilot is a partial-coverage tool. None of those are good options. With the hook release, a fourth option opens up: visibility, posture assessment, and active policy enforcement on the agent already in production.


## What to do now


Unbound AI is the first Agent Access Security Broker (AASB) platform. It provides discovery, assessment, and runtime enforcement across Cursor, Claude Code, GitHub Copilot, and their MCP server connections. If you would like to run Unbound against your deployment of GitHub Copilot, there are two ways to start:


**Start free.** Inventory your GitHub Copilot usage, analyze token use and MCP connections, and assess your risk surface. Sign up at[getunbound.ai/free](https://getunbound.ai/free) .


**Book a demo.** See policy-based mitigation running against real risk scenarios across GitHub Copilot at[getunbound.ai/book-demo](https://getunbound.ai/book-demo) .


---


## Related reading


- [What is an Agent Access Security Broker (AASB)?](https://getunbound.ai/blog/what-is-aasb)
- [Securing Cursor, Codex, and Claude Code: A Comparison of AI Coding Agent Risks](https://getunbound.ai/blog/securing-cursor-windsurf-claude-code)
- [Governing Claude Across Web, Desktop, and Code](https://getunbound.ai/blog/governing-claude-across-surfaces)
- [Every Known MCP Attack Pattern, Mapped](https://getunbound.ai/blog/every-known-mcp-attack-pattern-mapped)
