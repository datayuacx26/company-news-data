---
schema_version: "1.0.0"
document_id: "2b838fe553358a0a82f6b5d3f15e09b074d70dac81fefe6dc5f96b42cca77a85"
company_key: "varonis-systems-inc-common-stock"
company: "Varonis Systems Inc."
source_id: "varonis-systems-inc-common-stock-rss-915499d71e96"
canonical_url: "https://www.varonis.com/blog/claude-coverage"
published_at: "2026-07-14T12:55:01+00:00"
first_seen_at: "2026-07-24T05:47:29.345543+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:5f0fa42f5f066ac4b7cff41fcac937b20147aadcd1f1e6e67360a4bab1ad51c9"
---

# Varonis Atlas Extends Coverage Across the Claude Enterprise Suite — From Claude Cowork to Claude Code

[Varonis' Atlas AI Security Platform](https://www.varonis.com/platform/ai-security?hsLang=en) now secures the entire Claude enterprise suite — extending existing support for[Claude Enterprise and Claude Platform](https://www.varonis.com/blog/claude-compliance-api-integration?hsLang=en) to Claude Code and Claude Cowork.


With this release, Atlas becomes one of the few AI security platforms covering the breadth of the Anthropic agentic portfolio, from assistants like Claude Enterprise and Claude Cowork, to developer tools like Claude Code, to the infrastructure organizations use to build and deploy Claude agents, like Claude Platform, Microsoft Foundry, and AWS Bedrock.


As Anthropic's agents move from answering questions to taking actions, like writing to local files, running terminal commands, and editing code, security has to move as well. Atlas is the most complete way to secure the new agentic world Anthropic has created with Claude.


## Securing an agentic ecosystem, not a single app


A few years ago, "securing AI" meant putting guardrails around chatbots. That's no longer the case.


Anthropic's own product line illustrates how much our use of generative AI has changed and how much the surface area has expanded:


- **Claude Enterprise** handles knowledge work and RAG-style retrieval over corporate data.
- **Claude Cowork** can read, edit, and create files across a user's tools and devices, running multi-step tasks with minimal supervision.
- **Claude Code** operates inside the developer's terminal and IDE, with access to source repositories, credentials, and build systems.
- **Claude runs through infrastructure** , like Microsoft Foundry, AWS Bedrock, and Anthropic's own Claude Platform, where governance often lives with a completely different team than the one managing desktop or IDE tools.


Fragmented coverage and control create blind spots that attackers and careless or malicious prompts can walk right through. The risk doesn't stay contained to the surface where it started. A poorly scoped Cowork task can touch the same sensitive files that a Claude Enterprise chat pulled from moments earlier.


## Securing Claude Code: IDE-Native oversight for IDE-Native risk


Claude Code is powerful for developers. It runs in the terminal and the IDE, with access to source code, environment variables, package managers, and often direct shell execution. That access can also create risk, and security teams often have had the least visibility once code moves outside of a traditional CI/CD pipeline.


Atlas brings dev-cycle security principles to Claude Code activity, giving security teams visibility and control they didn't have before:


*Varonis Atlas provides AI runtime guardrails for Claude Code, including redacting sensitive*


*Varonis Atlas provides AI runtime guardrails for Claude Code, including redacting sensitive data, blocking exfiltration, and quarantining high-risk sessions.*


**AI Runtime Guardrails:** Atlas can monitor, block, modify, or alert on Claude Code activity in real time across every prompt, LLM call, tool call, and MCP interaction.


*Varonis Atlas provides full session visibility for Claude Code, every prompt, LLM, and tool, MCP activity, and more.*


**


**AI Detection & Response (AIDR):** Reconstruct exactly what happened during a Claude Code session, including prompts, LLM and tool calls, MCP server activity, command execution, and agent actions. Gain comprehensive attack-path views for prompt manipulation, tool abuse, and privilege escalation.


*Varonis Atlas monitors Claude Code activity to detect and prevent sensitive data exposure.*


**AI Security Posture Management (AI-SPM):** Discover the artifacts that shape Claude Code's behavior — skills, configs, embedded instructions — and flag attempts to manipulate that behavior, such as malicious skills designed to steer the agent off-course.


*Varonis Atlas discovers the artifacts that shape* *Claude Code’s* *behavior, helping identify attempts to manipulate agent behavior, such as malicious skills.*


**


**AI Activity Monitoring:** Detect and prevent exposure of source code, credentials, API keys, and regulated data across all agent activity, including unusual token usage.


## Securing Claude Cowork: Agentic file access and creation bring agentic risk


Claude Cowork is built to act, not just respond. Point it at a folder, and it can rename, sort, deduplicate, summarize, and rewrite files across a user's local environment, and coordinate multiple sub-agents to do so. That's the appeal, and it's also where new risks show up once an agent has read/write access to a folder.


Atlas now supports a dedicated Claude Cowork Runtime Integration — a plugin-based connection that sends Cowork activity to Atlas so runtime policies can evaluate it as it happens. Because Cowork is configured as its own Guardrail Integration resource, separate from Claude Code, security teams can apply distinct policies, logging, and audit trails to each surface without one governing the other.


*Varonis Atlas provides AI runtime guardrails for Claude Cowork to block, modify, alert, or log risky or malicious activity.*


**AI Runtime Guardrails:** Atlas evaluates Cowork activity and can block, modify, alert, or log depending on the event type. Prompt submissions and tool calls can be blocked outright or have their input/output modified before execution.


**AI Activity Monitoring & Investigations:** Every evaluated event carries user attribution automatically enabling activity to be tied back to the person who triggered it and reviewed— giving security teams an audit trail by user, project, resource, and source application, and the ability to catch an agent being redirected mid-session rather than just what it was originally asked to do.


## Complete coverage, not partial visibility


Coverage that stops at one Claude surface isn't coverage; it's a blind spot. With this release, comprehensive coverage means visibility that follows Anthropic's agents wherever an organization runs them, connected back to the same data context, permissions, and risk signals that power the rest of the Varonis Data Security Platform.


Varonis Atlas is available today. Watch the demo or start a free trial to see Atlas' AI inventory, posture management, security testing, runtime guardrails, and compliance reporting in action.
