---
schema_version: "1.0.0"
document_id: "e9c41cae5f6ce24538b757b8fb773d86cf2ed7a6714cbd0ee16b8c6b4fce510d"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/code-execution-with-mcp"
published_at: "2026-04-23T18:48:17+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:52:14.556861+00:00"
content_hash: "sha256:60f03fcc5a3e8748b56048327d5b90f55c0c756cf05dd22ff9defd2a341433a7"
---

# MCP: Why production agents need both standardized tools and dynamic sandboxes

The demo goes perfectly. An AI agent queries a database, summarizes the results, and pushes a Slack notification. Leadership greenlights production. Then a real user asks the agent to normalize date formats across three inconsistent CSVs, compute a weighted average, and merge the output into a report. No predefined tool matches. The agent stalls.


That's the execution gap. Model Context Protocol (MCP) standardizes how agents discover and call tools, but it doesn't run code. It routes requests to capabilities exposed by servers. When a task falls outside those capabilities, the agent has nowhere to go.


Production agents that need dynamic computation need both layers: MCP for tool orchestration and sandboxes for runtime code execution. This article covers what each layer does, where each breaks down alone, and how the two combine into an architecture that handles enterprise workloads.


## **The execution gap in production agents**


Agents that execute code in production face a structural problem: the tasks they encounter can't always be mapped to a fixed set of tools.


### **From copilots to autonomous agents**


Copilots operate within a single interaction loop. They receive a prompt, generate a response, and terminate. Agents move beyond that loop. They plan, decide, and act across multi-step workflows.


Gartner projects that[40% of enterprise applications](https://www.gartner.com/en/newsroom/press-releases/2025-08-26-gartner-predicts-40-percent-of-enterprise-apps-will-feature-task-specific-ai-agents-by-2026-up-from-less-than-5-percent-in-2025) will feature task-specific AI agents by 2026, up from less than 5% in 2025. That growth rate signals a category shift, not incremental adoption. IDC's maturity model shows why tool use is non-optional at the agent tier.


Moving from "AI provides insights" to "AI executes workflows" requires writing to external systems. An agent processing a refund must interact with external APIs and databases. Text generation alone can't do that.


### **Where tool-only architectures break down**


Several failure modes show up repeatedly when agents rely on fixed tool catalogs.


The first is tool hallucination. When no matching tool exists, agents don't abstain. They fabricate tool names and invoke them confidently. Benchmark research measured this: in scenarios where no tool was available, models hallucinated a tool[34.8% of the time at baseline](https://arxiv.org/html/2510.22977v2) , and 90.2% with enhanced reasoning. Enhanced reasoning made the problem worse. For enterprise teams, a fabricated tool call against a production database creates compliance incidents and data corruption.


The second is brittle API integration. Tools break when the systems they connect to change. One production post-mortem describes agents that[worked during development but failed when deployed live](https://thenewstack.io/ai-agents-in-legacy-systems-the-problem-no-one-talks-about/) because batch processing cycles meant the data the agent needed was hours stale. Undocumented legacy modules contained hardcoded business logic never exposed as APIs. Stalled deployments follow.


The third is runtime inflexibility. Agents with broad tool access but no invocation-level enforcement generate runaway API call loops and insecure access patterns. The tool catalog is static. When a workflow requires logic that wasn't anticipated at design time, the agent either hallucinates a path forward or stops.


## **What MCP solves, and where it stops**


MCP addresses integration fragmentation. The computation problem still requires a separate execution layer.


### **How MCP standardizes tool use**


MCP is an open protocol that standardizes how AI applications share contextual information with language models and expose tools to AI systems. The[official specification](https://modelcontextprotocol.io/docs/learn/architecture) bounds its scope explicitly: MCP focuses on the protocol for context exchange. It doesn't dictate how AI applications use LLMs or manage the provided context.


The protocol uses JSON-RPC 2.0 as its message format and follows a client-host-server architecture. Servers are self-describing, announcing their own capabilities so agents don't need custom client code to communicate with them. The structural analogy is the Language Server Protocol. LSP standardized how programming language support integrates across IDEs. MCP does the same for how tools and context integrate across AI applications.


Before MCP, connecting agents to tools required a custom integration for each pairing. Every model vendor had its own function-calling format. MCP reduces that M×N fragmentation into M+N modularity: implement MCP once in your agent and connect to an ecosystem of integrations.


Adoption reflects this value. Anthropic reported[more than 10,000 active public MCP servers](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation) as of December 2025, with 97 million monthly SDK downloads. ChatGPT, Gemini, Microsoft Copilot, Visual Studio Code, Cursor, and Claude all support MCP as clients.


### **MCP as control plane, not execution layer**


MCP orchestrates what to call. Computation inside a tool still has to run somewhere else. MCP servers expose discoverable tools with metadata and input schemas. The` tools/list` method enumerates available tools at session initialization.


Agents invoke them via` tools/call` . The spec includes` listChanged` notifications that let servers surface updated tool availability at runtime. What MCP does not provide is a way for the agent to invent arbitrary new tools and rely on the protocol itself to make them executable.


That boundary matters for architecture decisions. MCP supports capability negotiation and discovery, but it doesn't provision compute, enforce resource limits, or isolate execution. You still need somewhere for code to actually run.


### **The limits of predefined tools in dynamic workflows**


MCP's predeclared tool model falls short in several common scenarios.


Exploratory data work is iterative. Each step's parameters depend on the previous step's output. One-off transformations resist pre-modeling. A user asks an agent to parse three CSVs with different schemas, normalize dates, and produce a merged report. No reasonable tool catalog anticipates every schema combination.


Chained computations consume context. MCP provides no native mechanism for piping the output of one tool as the input to another without the LLM mediating each step. Every intermediate result passes through the model, consuming tokens and adding latency.


Anthropic's engineering[blog](https://www.anthropic.com/engineering/code-execution-with-mcp) documents this: direct tool calls consume context for each definition and result, and agents scale better by writing code to call tools. These are the tasks that push teams toward runtime code execution alongside MCP.


## **Why production agents need dynamic execution sandboxes**


The tasks that defeat static tool catalogs share a common requirement: the agent must generate and execute code at runtime.


### **Tasks that can't be pre-modeled as MCP tools**


Data wrangling across inconsistent schemas is the most common. An agent ingests financial data from three vendor APIs, each with different date formats, currency representations, and nested structures. The normalization logic changes with every new data source. Pre-building a tool for each permutation is impractical because the schema space is unbounded. The agent needs to inspect the data, generate transformation code, and execute it.


API stitching, where the glue code varies by request, is another pattern. An agent orchestrating a procurement workflow might call an inventory API, a pricing API, and an approval system. The conditional logic connecting these calls depends on the specific request. Writing one tool per combination creates a combinatorial explosion.


Custom computation that shifts with user intent is another. A data analyst agent asked "which product categories grew fastest in Q3 relative to their Q2 baseline, excluding returns" needs to compose a statistical computation that no pre-built tool anticipates.


### **Requirements for enterprise execution sandboxes**


Running agent-generated code in production requires more than a Python subprocess. Several requirements are non-negotiable for enterprise teams:


- **Hardware-enforced isolation:** Containers provide some guarantee, microVMs provide stronger isolation, and full VMs the strongest. For[agents executing untrusted code](https://blaxel.ai/blog/ai-sandbox) , microVMs running a separate kernel per workload prevent an exploit in one sandbox from reaching the host or neighboring sandboxes.
- **Fast resume from idle:** Tool execution accounts for roughly 30-80% of first-token-response latency in production agent pipelines. Multi-second cold starts on top of that push agents past the[100-millisecond threshold for perceived instantaneous response](https://www.nngroup.com/articles/response-times-3-important-limits/) .
- **Compliance coverage:** HIPAA requires audit controls, access controls, integrity controls, and transmission security. SOC 2 Type II requires evidence of operational effectiveness over time. ISO 27001:2022 covers organizational, people, physical, and technological controls. Sandboxes that can't produce compliance artifacts stall enterprise procurement.
- **Observability and audit logs:** Every code execution must be traceable: what ran, when, under what permissions, and what it accessed.
- **Policy-gated execution:** Sensitive operations need approval workflows, resource limits, and permission boundaries at the execution layer.


[Blaxel](https://blaxel.ai/products/sandboxes) is the perpetual sandbox platform built for AI agents that execute code in production. Sandboxes use microVM isolation, create from template in roughly 200-600ms, and resume from standby in under 25ms with native Zero Data Retention support.


They remain in standby indefinitely with compute scaled to zero while idle, then resume with previous state restored when an agent needs them. Sandboxes return to standby after 15 seconds of network inactivity. Standby preserves state for resume but doesn't guarantee durable long-term persistence; use volumes for guaranteed storage. ZDR prevents perpetual standby mode.


### **The risks of naive execution approaches**


Teams that skip isolation requirements pay the cost in production incidents and stalled enterprise deals.


Shared-kernel containers expose a well-documented attack surface.[NIST SP 800-190 states](https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-190.pdf) that because containers share the same kernel, the degree of segmentation between them is far less than that provided to VMs by a hypervisor. The CVE history confirms this.[CVE-2024-21626](https://nvd.nist.gov/vuln/detail/cve-2024-21626) enabled container escape through a runc file descriptor leak.


[CVE-2022-0492](https://nvd.nist.gov/vuln/detail/cve-2022-0492) enabled escape through cgroup release_agent abuse. A widely reported runc binary overwrite vulnerability followed the same pattern. Containers are standard and appropriate for trusted first-party code. For agent-generated code that should be treated as untrusted output,[microVM platforms](https://blaxel.ai/blog/container-escape) provide the boundary containers that can't.


[NVIDIA's AI Red Team makes the point directly](https://developer.nvidia.com/blog/how-code-execution-drives-key-risks-in-agentic-ai-systems/) : LLM-generated code must be treated as untrusted output, and sandboxing is essential to contain its execution. Local runtimes without isolation create data exfiltration paths. Unbounded compute from uncontrolled code execution creates cost exposure.


The absence of compliance artifacts blocks enterprise sales cycles. Start by auditing where agent-generated code currently runs in your stack and whether that execution environment provides hardware-enforced isolation, audit logging, and resource limits.


## **A reference architecture for MCP and sandboxes**


MCP and sandboxes address different layers of the same problem.


### **Orchestration versus execution**


MCP is the control plane. The sandbox is the execution plane.


MCP handles tool discovery via` tools/list` , invocation routing via` tools/call` , schema negotiation through capability exchange, and audit surface through structured JSON-RPC request IDs. Every` tools/call` request carries a tool name, arguments, and a unique identifier that can feed an audit log.


The sandbox handles compute isolation, resource limits, syscall restriction, and runtime containment. It receives code, executes it in a hardware-isolated environment, and returns results. Intermediate data stays within the sandbox and never re-enters the agent's context window.


This separation means you can swap MCP implementations without touching the sandbox, or swap sandbox providers without touching MCP orchestration. Cloudflare's internal MCP platform[demonstrates this](https://blog.cloudflare.com/enterprise-mcp/) . They built a shared MCP layer with default-deny write controls, audit logging, and auto-generated CI/CD pipelines, while execution happens in a separate layer.


### **The hybrid execution flow**


The agent decides what needs to happen. MCP resolves which capability to invoke by matching the request against registered tool schemas. If the task maps to a pre-built tool, the tool executes and returns results. If the task requires dynamic computation, the agent generates code and sends it to a sandbox for execution. Results return to the agent's context for the next step.


Anthropic's engineering team notes that the code execution path is often preferable even when tools exist. LLMs are generally better at writing code than using tools directly. Code generation is often more token-efficient for complex operations, though direct tool calls are typically more reliably executed.


Blaxel co-locates[MCP Servers Hosting](https://blaxel.ai/mcp) and Sandboxes on the same infrastructure, eliminating network hops between the two layers. When both layers run in different data centers, every request adds network round-trip latency. Every Blaxel sandbox also includes a built-in MCP server, so agents can operate the sandbox through standard MCP tool calls.


### **Integration patterns**


A few patterns cover the most common workloads:


- **Sandbox as MCP tool:** Register the sandbox's` execute` capability as a standard MCP tool. The agent discovers it through normal` tools/list` enumeration and invokes it through` tools/call` . This works well when code execution is one of several capabilities the agent needs.
- **Policy-gated execution endpoint:** Add an authorization layer between MCP resolution and sandbox execution. The MCP gateway validates permissions, checks resource limits, and logs the request before forwarding to the sandbox. This fits regulated environments where certain computations require approval workflows.
- **Chained sandbox workflows:** For multi-step computations, the agent generates a sequence of code blocks that execute in the same sandbox, preserving state between steps. This fits exploratory data work where each step depends on the previous step's output.


### **Governance and audit**


Production architectures require governance that spans both layers. Tracing execution across MCP calls and sandbox runs requires a unified observability layer. Every MCP` tools/call` request carries a JSON-RPC ID. The sandbox execution produces its own logs, process traces, and resource consumption records. Correlating these across layers gives the engineering team a complete picture of each agent interaction.


Approval workflows for sensitive operations sit at the MCP gateway layer. Before a sandbox executes code that modifies production data, the gateway checks the request against policy rules and, if required, routes it through a human approval step. Access control restricts which agents can invoke which tools and execute which types of code.


Fine-grained permission sets, defined at deployment time, prevent the broad-access patterns that lead to runaway API loops. Audit logs capture the evidence SOC 2 Type II, and HIPAA auditors need: what ran, when, under what identity, and what data it accessed. Without that evidence, compliance gaps stall deals and extend sales cycles.


## **Strategic implications for engineering leaders**


The architecture above has implications for platform strategy and resource allocation.


### **Tooling as programmable infrastructure**


The mental model for AI tooling is shifting. Tools are programmable execution environments where agents compose computation at runtime, not static integrations that map one-to-one with API endpoints.


This changes platform strategy. Instead of building tool catalogs that try to anticipate every task, teams build execution infrastructure that can handle any task requiring dynamic computation. The tool catalog becomes smaller and more focused on stable, well-defined operations. Dynamic code execution handles the long tail.


The "tool builder" role evolves from API wrapper author to platform engineer who maintains the execution environment, policy rules, and observability pipeline. Start by reviewing your current tool catalog and identifying which tools exist because no dynamic execution option was available.


### **Build versus buy**


Building MCP orchestration and secure sandboxing in-house requires dedicated infrastructure engineers and significant ramp time. Firecracker microVMs, seccomp hardening, compliance-aligned audit logging, and sub-second standby resume are each individually complex. A small infrastructure team represents a substantial annual salary cost before benefits and recruiting.


Managed platforms absorb that cost and compress the timeline. The decision comes down to capital allocation and time-to-production: teams with dedicated specialists and enough runway for infrastructure work can build. Teams that need production execution soon should evaluate managed options and redirect engineering capacity to agent logic.


### **Standardizing execution across teams**


As organizations scale from one agent to dozens, fragmented execution patterns create day-to-day problems. Different teams use different sandboxing approaches, different policy enforcement, and different observability pipelines. Debugging across teams becomes an exercise in translation.


Standardizing on shared execution primitives solves this. A common sandbox runtime, consistent policy rules, and unified tracing let teams share infrastructure without coordinating on implementation details. When every team's agents run in the same type of sandbox under the same policy rules, organizations can add agent capabilities without multiplying operational complexity.


## **Building production-ready agent execution with MCP and sandboxes**


MCP and sandboxes are complementary layers. MCP standardizes tool discovery and invocation. Sandboxes provide the isolated runtime where computation actually happens. Agents that execute untrusted code or need dynamic computation beyond pre-registered tools need both.


Blaxel is the perpetual sandbox platform built for this architecture.[Sandboxes](https://docs.blaxel.ai/Sandboxes/Overview) deliver dynamic code execution with microVM isolation, initial creation in roughly 200-600ms, and sub-25ms resume from standby.


MCP Servers Hosting provides standardized tool access with built-in observability, co-located with sandboxes on the same infrastructure to eliminate network hops.[Agents Hosting](https://blaxel.ai/agents-hosting) lets teams deploy agent logic alongside both layers. Blaxel's SDKs span Python, TypeScript, and Go for platform interaction, while hosted agents and MCP servers support Python and TypeScript.


Teams building agents that execute code in production can request a demo at[blaxel.ai/contact](https://blaxel.ai/contact) or start with free credits at[app.blaxel.ai](https://app.blaxel.ai/) .


MCP orchestration + sandbox execution in one stack


Co-located MCP Servers Hosting and microVM sandboxes with sub-25ms resume. Built-in MCP server per sandbox. Up to $200 in free credits.


[Start free](https://app.blaxel.ai/)


## **FAQ**


### **What is the difference between MCP and a code execution sandbox?**


MCP is a protocol that standardizes how agents discover and invoke tools, handling routing, schema negotiation, and capability exchange. A code execution sandbox is an isolated compute environment where code actually runs. MCP handles the decision and routing layer while the sandbox runs the computation. They operate at different architectural layers and serve different purposes.


### **Can MCP handle dynamic code execution on its own?**


No. MCP servers expose discoverable tools with schemas, and` tools/list` enumerates what is available while` tools/call` invokes those tools. Servers can update tool availability at runtime and notify clients to rediscover it. What MCP does not do is let an agent invent arbitrary new tools and turn that into an execution environment. Dynamic code execution still requires a separate sandbox layer.


### **What security standards should an enterprise AI execution sandbox meet?**


At minimum: hardware-enforced isolation via microVMs rather than shared-kernel containers, SOC 2 Type II certification, and audit logging for every execution. Organizations handling health data need HIPAA compliance with a Business Associate Agreement. ISO 27001 covers organizational and technological security controls. The sandbox should also enforce resource limits, support policy-gated execution, and provide zero data retention options.


### **How do MCP servers and sandboxes fit together in an agent architecture?**


MCP servers handle the control plane: tool discovery, invocation routing, and schema management. Sandboxes handle the execution plane: isolated code execution, resource containment, and state management. The agent decides what to do, MCP resolves which tool to call, and the sandbox runs the code that tool requires.[Co-locating both layers on shared infrastructure](https://blaxel.ai/blog/codesandbox-alternatives) reduces network latency between them.


**Are there standards built with code execution through MCP**


“Code mode” was popularized by Anthropic and Cloudflare and enables a more efficient way to execute tool calls over MCP. Essentially, code mode turns any OpenAPI specification into a two-tool MCP server: \`search\` lets an AI agent explore the API spec, and \`execute\` runs JavaScript code in a sandbox to call the actual API. Blaxel supports “Code mode” natively.
