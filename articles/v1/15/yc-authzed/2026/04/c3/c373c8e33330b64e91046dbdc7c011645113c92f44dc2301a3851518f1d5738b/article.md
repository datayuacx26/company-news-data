---
schema_version: "1.0.0"
document_id: "c373c8e33330b64e91046dbdc7c011645113c92f44dc2301a3851518f1d5738b"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents"
published_at: "2026-04-08T14:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:52:48.201959+00:00"
content_hash: "sha256:5e7e2bf56b681139accef3ea10516cb685fcd9ffccb1d74fbe04e93088128f94"
---

# Introducing SpiceBox and spicedb-dev

Your coding agent needs better permissions, and so does the code it writes.


AI coding agents have quickly become a normal part of how software gets built. Developers run Claude Code, Cursor, Copilot, and similar tools with broad access to their codebases, letting them read files, execute commands, make network requests, and generate entire applications.


We see two problems emerging from this that we are well-positioned to address. After all, authorization is what we do at AuthZed. We built[SpiceDB](https://authzed.com/spicedb) , the open-source permissions database inspired by Google Zanzibar, and we have spent years helping organizations implement fine-grained access control. Thus, we are happy to announce the release of two open-source tools (SpiceBox and spicedb-dev) that bring that expertise to where developers spend their time right now: working with AI coding agents.


## The agent with too much access


Agents without explicit boundaries can[take actions outside their mandate](https://x.com/summeryue0/status/2025774069124399363) . A common approach to preventing this is letting the agent decide for itself what's safe. Anthropic analysis of Claude Code's[auto permissions mode](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode) reports a[17% false-negative rate on dangerous actions](https://www.anthropic.com/engineering/claude-code-auto-mode) (so roughly 1 in 6 risky commands go through) and is upfront about the tradeoff. Auto mode targets high autonomy at low maintenance cost, and users accept residual risk. It is even worse without auto mode (let alone` --dangerously-skip-permissions` ): Claude Code can make mistakes and misinterpret the user's intentions.


This is a fundamental limitation. The agent makes probabilistic judgments but authorization requires deterministic answers. As we wrote in[MCP is Not Secure](https://authzed.com/blog/mcp-is-not-secure) , AI cannot reliably police itself. The enforcement model that works for securing applications, default-deny with explicit grants, works for agents too.


So we built[SpiceBox](https://github.com/authzed/spicebox) .


### SpiceBox: fine-grained permissions for AI coding agents


SpiceBox wraps Claude Code (and any Claude Agent SDK agent) with permission enforcement powered by SpiceDB. You describe what the agent should be allowed to do in plain language:


1


```text
Read and write Go files under src/. Run go test and go build. Fetch from github.com. No subagents.


```


1


```text


```


SpiceBox translates that into structured SpiceDB permissions, presents them for your review, and enforces every permission on every tool call through three independent layers:


1.


**Hook server (application layer).** An out-of-process server checks every tool call against SpiceDB before execution. All file-access tools (Read, Write, Glob, Grep) resolve to the same SpiceDB permissions, so a file restriction applies uniformly regardless of which tool is used. This is the same cross-resource consistency model that SpiceDB provides in production applications.


2.


**macOS sandbox (OS layer).** A kernel-level sandbox restricts the agent process directly. If no write permissions were granted, the working directory is read-only.` echo > file` is blocked by the OS even if the hook server allows bash.


3.


**Network filtering proxy.** All outbound HTTP traffic routes through a localhost proxy that permits connections only to explicitly allowed domains.` curl evil.com` fails at the proxy because` evil.com` is absent from the allowed host list.


These layers are independent. A prompt injection that convinces the model to ignore its instructions still hits the hook server. A bash workaround that bypasses the hook server still hits the OS sandbox. A creative network request still hits the proxy.


SpiceBox also supports mid-session permission escalation through a cryptographic grant flow. If you want to grant access to the agent that it was not initially given, you can request it (except filesystem, which is a sandbox limitation). The request is HMAC-signed, shown for approval, and written to SpiceDB immediately. No session restart required.


SpiceDB runs embedded in the SpiceBox binary as a Go library, entirely in-memory. No separate server to install. The same authorization engine that powers permissions in production applications at scale runs locally in your coding session.


We've been using SpiceBox internally and it has allowed us to set up very specific development environments for our Claude Code sessions to run autonomously and uninterrupted by permission prompts. We've documented other[scenarios](https://github.com/authzed/spicebox/blob/main/SCENARIOS.md) well-suited for SpiceBox.


## The app that inherits bad habits


Now consider the other side. Even if your agent is properly constrained, what about the code it writes?


Ask Claude to scaffold a multi-tenant SaaS application. You get working routes, database models, and API handlers. You almost certainly do not get permission checks on those handlers. No verification that the requesting user can access the requested resource. No relationship tracking when resources are created or shared. No concept of ownership beyond a` user_id` column.


We have seen this pattern for years, long before AI agents entered the picture. Access control has been one of the most consistently under-implemented parts of application development: added after the fact, inconsistent across endpoints, with unintended gaps that become security incidents. AI coding agents trained on that code reproduce those patterns faithfully. The generated code reflects the state of the art in training data, and for authorization, the state of the art has historically been poor.


If you are generating an application, you should generate it with proper access control from the start. The coding agent needs context it does not have: what good authorization patterns look like, where permission checks belong, how to write relationship data alongside business logic, and how to audit coverage. We have spent years building that expertise into SpiceDB's schema language, client libraries, and best practices. The missing piece was getting that knowledge into the coding agent's workflow.


So we built[spicedb-dev](https://github.com/authzed/authzed-marketplace/tree/main/spicedb-dev) .


### spicedb-dev: authorization expertise for your coding agent


spicedb-dev is a Claude Code plugin that gives your coding agent authorization context it otherwise lacks. It provides commands, skills, and specialized agents covering the full authorization workflow:


-


**` /spicedb-dev:plan`** analyzes your project and produces an authorization implementation plan, then adds a snippet to your` CLAUDE.md` that makes authorization ambient. Permissions are considered automatically on every handler Claude generates or modifies, not as a separate step.


-


**` /spicedb-dev:implement-spicedb`** routes to the right implementation: relationship writes for create/delete handlers, permission checks for read/update handlers, or both. It generates code using SpiceDB client libraries with correct consistency models and error handling.


-


**` /spicedb-dev:audit-coverage`** identifies which schema permissions have code-level checks and which do not, catching gaps before they become vulnerabilities.


The most impactful feature is the ambient` CLAUDE.md` integration. Once added, Claude asks two questions every time it generates or modifies a handler: "Does this handler create or delete a resource? Add relationship writes." "Does this handler access a resource on behalf of a user? Add a permission check first." Authorization shifts from a dedicated task to a continuous concern, which is how we believe it should work.


The plugin also includes a schema validator agent that checks` .zed` files against SpiceDB best practices, and a checkpoint identifier agent that analyzes existing code to find where permission checks belong. For teams adopting SpiceDB into an existing codebase, this audit-and-implement workflow is where most of the value lands.


**A note on accuracy:** Better context produces better output, but AI-generated code still requires human review. This is especially true for authorization logic, where a missing check or incorrect relationship write can become a security gap. Treat spicedb-dev as a knowledgeable starting point, not a replacement for validating the generated code.


## Why these tools, why now


We use agentic coding tools every day at AuthZed, and so do most developers we talk to. These tools meet developers where they are right now. SpiceBox brings our authorization expertise to how you run your coding agent, and spicedb-dev brings it to the code your agent writes. Both are open source and we hope that they are useful tools to add to your AI coding environment.


If you are building AI applications or agents that need fine-grained permissions beyond coding tools,[we would like to hear about your use case](https://authzed.com/schedule-demo) .


On this page


- The agent with too much access
- SpiceBox: fine-grained permissions for AI coding agents
- The app that inherits bad habits
- spicedb-dev: authorization expertise for your coding agent
- Why these tools, why now


## Related


[Engineering Introducing the LangChain SpiceDB Integration We're announcing langchain-spicedb, a new library that brings SpiceDB's relationship-based access control into LangChain and LangGraph workflows. Build RAG pipelines that respect what users are allowed to see with post-retrieval filtering, LangGraph auth nodes, and agent permission tools. Mar 9, 2026 · 6 min](https://authzed.com/blog/langchain-spicedb-integration)[Engineering Introducing the LangChain SpiceDB Integration We're announcing langchain-spicedb, a new library that brings SpiceDB's relationship-based access control into LangChain and LangGraph workflows. Build RAG pipelines that respect what users are allowed to see with post-retrieval filtering, LangGraph auth nodes, and agent permission tools. Sohan Maheshwar · Mar 9, 2026 · 6 min](https://authzed.com/blog/langchain-spicedb-integration)


[Engineering Introducing AuthZed's MCP Servers We're launching two MCP servers to bring SpiceDB closer to your AI workflow. The AuthZed MCP Server provides instant access to documentation and examples, while the SpiceDB Dev MCP Server integrates with your development environment. Learn about our MCP journey from early prototypes to production, and discover how these tools can speed up your SpiceDB development. Sep 30, 2025 · 4 min](https://authzed.com/blog/introducing-authzeds-mcp-servers)[Engineering Introducing AuthZed's MCP Servers We're launching two MCP servers to bring SpiceDB closer to your AI workflow. The AuthZed MCP Server provides instant access to documentation and examples, while the SpiceDB Dev MCP Server integrates with your development environment. Learn about our MCP journey from early prototypes to production, and discover how these tools can speed up your SpiceDB development. Sam Kim · Sep 30, 2025 · 4 min](https://authzed.com/blog/introducing-authzeds-mcp-servers)


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)
