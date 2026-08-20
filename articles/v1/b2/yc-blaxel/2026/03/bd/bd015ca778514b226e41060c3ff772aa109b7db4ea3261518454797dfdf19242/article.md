---
schema_version: "1.0.0"
document_id: "bd015ca778514b226e41060c3ff772aa109b7db4ea3261518454797dfdf19242"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/what-is-an-mcp-server"
published_at: "2026-03-30T07:50:51+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T21:58:04.130568+00:00"
content_hash: "sha256:c56b988f670b6e2ad62872952753bbbfbb090fdb1c6caeb24e2bbe3ecc280ee7"
---

# What is an MCP server: how AI agents connect to tools and APIs

# **What is an MCP server: how AI agents connect to tools and APIs + state of MCP in 2026**


You've built an agent that generates code, queries databases, and interacts with APIs using OpenAI's function calling. Each integration required custom glue code specific to OpenAI's API format. Now you want to migrate to Claude or support multiple LLM providers. You face a choice: rewrite all your tool integrations for each platform, or adopt a standardized approach.


The Model Context Protocol (MCP) offers a way out. It defines a standard interface between AI agents and external tools. Instead of rebuilding integrations per provider, you build once and connect everywhere.


This guide covers how MCP servers work at the protocol level. It explains benefits, challenges, and best practices for secure deployment.


## **What is an MCP server?**


An MCP server is a lightweight service. It exposes tools, data sources, and prompt templates to AI applications through the Model Context Protocol. MCP provides a platform-agnostic interface layer. You build tools once as MCP servers. Then you connect them to any compliant AI platform without rewriting integration code. All four major AI platforms[adopted MCP](https://modelcontextprotocol.io/) by 2025. This standardized how agents interface with external tools and data sources.


The protocol uses JSON-RPC 2.0 for communication. Transport mechanisms include stdio for local processes and HTTP with Server-Sent Events (SSE) for remote connections.


The architecture has three participants. The MCP Host is the AI application: Claude Desktop, a coding IDE, or your custom agent. The MCP Client manages protocol-level communication for each server connection. The MCP Server exposes the actual capabilities. Each host creates a dedicated client instance per server. This maintains isolated 1:1 connections.


A concrete example: GitHub Copilot's coding agent uses MCP servers as a core architectural component. Its default configuration includes a GitHub MCP server for repository operations and a Playwright MCP server for web automation.


The agent decides which tools to invoke based on descriptions and context. It calls them through the standard protocol. No custom integration code required.


## **How MCP changes your agent architecture**


Without MCP, tool logic lives inside the agent. Database queries, API calls, and file operations get embedded directly in the agent's codebase as provider-specific function definitions. The agent, its tools, and its execution environment share the same process.


Adding a tool means modifying the agent. Removing a tool means redeploying the agent. Each agent maintains its own copy of every integration.


MCP decouples tools from agents entirely. Tool logic is moved to independent MCP servers. The agent's codebase shrinks to reasoning logic and a protocol client. Three architectural shifts follow from this separation.


First, tools become shared infrastructure. A single GitHub MCP server handles repository operations for every agent in your stack. Your coding agent, PR review agent, and deployment agent all connect to the same server instance. When you update the GitHub integration, every agent gets the change without redeployment.


Second, each tool runs in its own isolation boundary. In the embedded model, a vulnerability in your file system code exposes the entire agent process. With MCP, each server operates independently. It has its own filesystem mounts, network rules, and permissions. Compromising the Git server doesn't give an attacker access to database credentials on the query server.


Third, agents discover tools dynamically instead of loading them at build time. An agent sends a` tools/list` request and receives the current inventory with schemas and descriptions. You can add, remove, or update MCP servers without touching agent code. This makes tool routing a deployment decision rather than a development task.


## **Benefits of adopting MCP servers**


The value of MCP depends on your agent architecture and scale. Here's where each benefit applies and where it doesn't:


- **Build once, deploy everywhere:** Tools work across all four major AI platforms without rewriting integration code. This matters most for teams running agents on multiple providers or planning a migration. If you're locked to a single provider with no plans to switch, custom function calling avoids the protocol overhead.
- **Dynamic tool discovery:** Agents find and use tools at runtime via` tools/list` without hardcoded integrations. This becomes valuable in multi-agent systems where specialized agents share a common tool pool. For a single agent with a fixed toolset, discovery adds a round-trip without meaningful flexibility gains.
- **Per-tool isolation boundaries:** Agent reasoning runs separately from tool execution, each in its own process or microVM. This matters for coding agents and any workflow where untrusted code runs alongside sensitive data. Agents that only call read-only APIs gain less from this separation.
- **Ecosystem access:** Over 10,000 pre-built servers exist for common integrations like GitHub, Slack, and databases. These accelerate prototyping. Production deployments typically fork or rebuild these servers. They need authentication, logging, and permission scoping that community versions lack.


These benefits compound as your agent stack grows. Each new agent inherits access to every existing tool server without additional integration work.


## **Challenges of deploying MCP servers in production**


The protocol handles communication. Everything else falls on your team. Production MCP deployments face five categories of challenges:


- **Security is your responsibility:** The MCP specification defines authorization as optional. OAuth 2.1 is recommended for HTTP transports but isn't enforced. All access control happens at the host application level. You must implement authentication, token management, and permission scoping yourself.
- **MCP-specific attack vectors:** An[academic analysis](https://arxiv.org/pdf/2506.13538) of 1,899 MCP servers identified unique vulnerabilities. These include tool poisoning attacks that exploit dynamic tool discovery. The Cloud Security Alliance flags improper authentication as a critical risk.
- **Isolation requirements for code execution:** MCP servers that execute code need strong isolation boundaries. While some guidance recommends container-based sandboxes, containers share the host kernel and create potential escape vectors. MicroVM-based isolation provides hardware-enforced boundaries where each workload runs its own kernel. This is the approach used by platforms like AWS Lambda and Blaxel.
- **Infrastructure overhead:** Production MCP deployments need gateway orchestration, multitenancy isolation, observability systems, and version management. This requires dedicated DevOps resources and infrastructure investment.
- **Missing operational benchmarks:** Public data covers ecosystem growth (10,000+ servers,[97 million monthly SDK downloads](https://www.npmjs.com/package/@modelcontextprotocol/sdk) ). That adoption rate confirms the protocol has traction, but it doesn't tell you what latency or cost-per-operation to expect. Teams should plan for internal performance testing.


The best practices section below addresses each of these challenges with specific implementation guidance.


## **How does an MCP server work?**


Every MCP interaction follows a structured lifecycle defined in the[official protocol specification](https://modelcontextprotocol.io/docs/learn/architecture) . The client opens a transport connection using either stdio or HTTP with SSE. It then performs a capability negotiation handshake. Both sides declare supported features and agree on the intersection.


With the handshake complete, the client sends a` tools/list` request. The server responds with every available tool. Each entry includes names, descriptions, and JSON Schema parameter definitions. The host presents these tools to the language model. The LLM decides which tools to invoke based on descriptions and context. The client then sends` tools/call` requests with validated arguments. The server executes the logic and returns structured results.


The connection stays open for as many tool calls as needed. On shutdown, the server completes in-flight requests and cleans up resources.


## **Production deployment topology**


A typical production coding agent connects to 3 to 8 MCP servers. A code generation agent might use a filesystem server for file operations and a Git server for version control. It could also connect to a code execution server for running tests and a database server for schema lookups. Each server runs as a separate service in its own isolated environment.


The agent's MCP client maintains a persistent connection to each server. When the LLM selects a tool, the client routes the` tools/call` request to the correct server based on tool name. All servers share a centralized secrets store for credentials and a centralized observability stack for tracing and alerting.


Three failure modes require handling at the client level. If a server is unreachable, the client should remove its tools from the available set and inform the LLM. If a server times out mid-execution, the client returns a structured error.


The LLM can then retry or choose an alternative approach. If a server returns malformed results, the` isError` flag in the response lets the agent handle the failure gracefully. Building retry logic and timeout thresholds into the MCP client prevents one degraded server from blocking the entire agent.


## **Best practices for deploying MCP servers with autonomous coding agents**


Production MCP deployments require deliberate security and infrastructure decisions. These practices address the risks and operational requirements of running MCP servers with autonomous coding agents.


### **Implement token exchange for agent authentication**


Use OAuth 2.1 with PKCE for remote MCP servers over HTTP transports. Issue short-lived, scoped tokens per session to limit exposure windows. Validate tokens on every request at the host level. MCP's authorization model is optional by specification. All enforcement is your responsibility. A database query server should receive tokens scoped only to SELECT operations on specific tables, expiring after 15 minutes.


### **Run each MCP server in an isolated environment**


Compromising one MCP server can cascade to others if they share resources. Deploy each server in its own[isolated environment](https://blaxel.ai/blog/ai-sandbox) . Use separate filesystem mounts and network segmentation. For coding agents that execute untrusted code, microVM-based isolation provides stronger boundaries than containers.


Containers share the host kernel, which means a kernel exploit in one workload can reach others on the same host. MicroVMs run a separate kernel per workload, enforcing hardware-level boundaries. Perpetual sandbox platforms like[Blaxel](https://blaxel.ai/) provide MCP Servers Hosting with microVM-based isolation. Each instance resumes in under 25ms from standby with built-in credential management.


### **Scope permissions to the minimum required operations**


Define allowlists of permitted operations for each MCP server. A filesystem server for a code review agent should have read access to the workspace directory only. No write access. No access outside the project root. Policy engines like Open Policy Agent, Styra DAS, or Cedar can handle dynamic authorization decisions.


### **Adopt code execution patterns over upfront tool loading**


Anthropic's engineering team documented that loading all tool definitions into the context window creates waste. Their[code execution architecture](https://www.anthropic.com/engineering/code-execution-with-mcp) lets agents write code that calls MCP tools programmatically. Agents perform filtering and aggregation locally. This approach achieved up to 98.7% token savings compared to traditional tool loading. Perpetual sandbox platforms like Blaxel can help you set up code execution with MCP for any OpenAPI spec through a “[code mode](https://blaxel.ai/blog/efficiently-connect-agents-and-apis-with-code-mode-on-blaxel) ” feature.


This reduction comes from the code execution pattern, not from MCP as a protocol. The trade-off: code execution patterns require stronger sandboxing. MicroVM-based isolation prevents injection attacks by enforcing hardware-level boundaries.


### **Centralize secrets with automated rotation**


Store all API keys, database credentials, and tokens in a centralized secret management system. Use dynamic secrets with short time-to-live values. Automate rotation to limit exposure windows. Implement fine-grained permission scoping at the individual MCP server level. Rotate database credentials every 24 hours and API keys every 30 to 90 days. Revoke immediately on server decommission.


### **Deploy structured logging with distributed tracing**


Production MCP deployments need structured logging. Capture correlation IDs, tool invocations, latency, and security context. Use OpenTelemetry for end-to-end request tracing across the full agent-to-tool flow. Forward logs and traces to centralized observability platforms like Splunk, Datadog, or Elastic.


This gives you real-time alerting, performance analysis, and security monitoring. Trace a user request from the chat interface through tool selection to the MCP server's API call. Capture latency at each hop.


### **Monitor for MCP-specific threat patterns**


Production MCP deployments require monitoring designed to detect MCP-specific attacks.[Checkmarx's threat taxonomy](https://checkmarx.com/zero-post/11-emerging-ai-security-risks-with-mcp-model-context-protocol) identifies risks including tool poisoning, prompt injection, and confused deputy attacks. Deploy real-time alerting on suspicious patterns. Establish behavioral baselines for normal agent operation. Integrate monitoring data with SIEM systems for automated incident response.


### **Design tools as small, composable operations**


MCP implementations demonstrate a pattern of exposing discrete operations. A filesystem server provides separate` read_file` ,` list_directory` , and` get_file_info` tools. Each handles one operation. This design lets agents compose complex workflows by chaining small tools. It also gives you granular control over permissions per operation.


## **Start deploying MCP servers for your AI agents**


MCP moved from experimental protocol to industry standard in roughly a year. All four major AI platforms support it. The Linux Foundation governs it. Over 10,000 public servers exist in the ecosystem.


The critical takeaway: MCP gives you the protocol. Production deployment requires substantial infrastructure investment. Because MCP's security model is optional at the protocol level, your team must implement host-level security enforcement. This includes microVM sandboxes, OAuth token exchange, fine-grained permission scoping, and threat detection systems.


Teams should evaluate their isolation strategy early in production planning. Containers share the host kernel, which limits the security boundary they can enforce. MicroVMs provide hardware-enforced isolation at the hypervisor level. This choice has cascading effects on security posture, performance, and operational complexity.


Perpetual sandbox platforms like Blaxel address these isolation and infrastructure challenges with MCP Servers Hosting built on microVM isolation, SOC 2 Type II and ISO 27001 certification, and SDKs for[Python, TypeScript, and Go](https://docs.blaxel.ai/) .


Deploy MCP servers with microVM isolation


100+ pre-built integrations, sub-25ms resume, built-in credential management, and SOC 2 Type II certified. Custom servers in Python or TypeScript.


[Start free](https://app.blaxel.ai/)


## **FAQs about MCP servers**


### **How is an MCP server different from a REST API?**


A REST API exposes endpoints that developers integrate manually. An MCP server exposes tools through a standardized protocol that AI agents discover dynamically at runtime. Agents send a` tools/list` request to enumerate available capabilities. They receive structured descriptions with JSON Schema parameter definitions. The LLM reads these descriptions and decides which tools to call. REST APIs require developers to write integration code for each endpoint. MCP servers let agents find and use tools without custom code.


### **Do all major AI platforms support MCP?**


Yes. Anthropic launched MCP in November 2024 with native support in Claude Desktop and Claude Code. OpenAI and Microsoft both adopted MCP in May 2025. Google integrated MCP across its Gemini ecosystem. In December 2025, Anthropic donated the protocol to the Linux Foundation's[Agentic AI Foundation](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation) . This established vendor-neutral governance for the standard.


### **What security risks should I plan for with MCP servers?**


MCP's authorization model is optional by specification. Your team must implement OAuth token exchange, per-server permission scoping, and isolation. MCP introduces unique attack vectors documented by the Cloud Security Alliance. These include tool poisoning (malicious tool metadata) and confused deputy attacks. Prompt injection can also target tool selection. Servers handling code execution require microVM-based sandbox isolation or equivalent hardware-enforced boundaries.


### **Can I build a custom MCP server or do I need to use existing ones?**


You can build custom MCP servers in any language supporting JSON-RPC 2.0. Official SDKs exist for Python and TypeScript. The MCP Build Server Tutorial provides canonical implementations in six languages. The ecosystem also includes over 10,000 pre-built servers. Most production deployments combine custom servers for internal tools with pre-built servers for standard integrations.


### **How does MCP compare to OpenAI function calling?**


MCP is a JSON-RPC 2.0 protocol layer that works across AI providers. It supports multiple transports (stdio and HTTP/SSE) and stateful connections with capability negotiation. OpenAI function calling is an API-level feature locked to OpenAI models. It defines tools in each API request with no session state. Choose MCP for multi-provider support and persistent connections. Choose a function calling for OpenAI-only deployments with stateless operations.
