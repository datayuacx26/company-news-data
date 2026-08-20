---
schema_version: "1.0.0"
document_id: "437854ef69d2f4492653a79b092b3d6dc4cb62b9321d065ebc311245fd79c4ae"
company_key: "yc-redouble-ai"
company: "Redouble AI"
source_id: "yc-redouble-ai-rss-94bf26a72f2a"
canonical_url: "https://deconvoluteai.com/blog/nvidia-openshell-mcp-protocol-layer"
published_at: "2026-03-27T00:00:00+00:00"
first_seen_at: "2026-07-27T11:30:56.530208+00:00"
fetched_at: "2026-07-28T21:56:52.525405+00:00"
content_hash: "sha256:6bc1c8c21326f982154cc858d53a6f2d217d2a8380a443439b05b5dce8920ef2"
---

# NVIDIA OpenShell and MCP Protocol Security - What the Sandbox Cannot Enforce

At GTC 2026, NVIDIA announced[OpenShell](https://github.com/NVIDIA/OpenShell) as part of its[Agent Toolkit](https://nvidianews.nvidia.com/news/ai-agents) : an open-source runtime that addresses something the agent security space had been missing, namely a principled infrastructure-level sandbox for autonomous agents. OpenShell runs agents inside Docker containers governed by declarative YAML policies that control which binaries can reach which network endpoints, which filesystem paths are readable or writable, and what process identity the agent runs under. The per-binary egress model is particularly granular, more so than a traditional network firewall, and the[Landlock LSM](https://docs.kernel.org/security/landlock.html) enforcement reaches down to the kernel so that a compromised process inside the sandbox cannot escape filesystem constraints through low-level syscall tricks. The privacy router adds a further capability by keeping LLM API calls on local or self-hosted compute rather than routing inference traffic through external providers.


What OpenShell does not do is inspect the content of what travels inside those permitted connections. When, for example, the claude binary opens a TLS connection to an MCP server, OpenShell evaluates the policy and allows the connection. For REST endpoints with explicit L7 configuration it can additionally inspect the HTTP method and path of individual requests, see[documentation](https://github.com/NVIDIA/OpenShell/blob/main/architecture/sandbox.md) . What it cannot inspect is the request body, which is where MCP tool names, arguments, and schemas live. Whether a particular tool was called, what arguments were passed, and whether the schema has changed since session start are questions that sit below the HTTP envelope and outside what any network-layer control can answer.


That gap is what[Deconvolute](https://github.com/deconvolute-labs/deconvolute) addresses. Where OpenShell governs the environment an agent runs in, Deconvolute is a protocol-layer firewall that governs the MCP session itself, inspecting and enforcing policy on the tool calls happening inside those permitted connections.


## Two Tools, Two Layers


The[OSI model](https://en.wikipedia.org/wiki/OSI_model) provides a useful frame for understanding where OpenShell and Deconvolute each operate, as visualized in figure 1.


Figure 1: A simplified view of where each tool enforces policy. OpenShell operates at the OS, kernel, and transport layers. Deconvolute operates at the application layer on the MCP protocol itself.


OpenShell operates at the OS and network layers. Its policy engine controls which binaries can initiate which connections, enforces filesystem boundaries via Landlock LSM at the kernel level, and sets the process identity the agent runs under.


The MCP protocol sits at the application layer, above where OpenShell operates. An MCP session is a JSON-RPC conversation happening inside a permitted connection. The tool names being negotiated, the arguments being passed, the schemas the agent uses to decide what to call and how, none of this structure is visible at the network layer. A sandbox policy can permit binary X to reach host Y on port 443. It has no surface for specifying which tools binary X may call once that connection is open, or what those tools are allowed to do.


## What the Sandbox Sees


To make this concrete, consider what happens during a typical MCP session. The agent process starts, connects to an MCP server, and receives a list of available tools. It then begins calling those tools as it works through a task. From OpenShell's perspective, this entire session is a single permitted connection: binary X reached host Y on port 443, the policy matched, traffic flowed.


Inside that connection, however, a structured conversation is taking place. The server advertises tools with names, descriptions, and input schemas. The agent selects tools, passes arguments, and receives results. Each of these exchanges is carried inside HTTP request bodies as JSON-RPC messages. OpenShell's L7 inspection layer can see the HTTP method and path of a request. It cannot see inside the body, which means the tool name, the arguments, and the schema are invisible to it regardless of how the endpoint is configured, see[here](https://github.com/NVIDIA/OpenShell/blob/main/architecture/sandbox.md) .


## The Gaps That Remain


The question is what an attacker or a misbehaving agent can do within a permitted MCP connection that OpenShell cannot prevent. Two gaps are worth examining.


### Least Privilege at the Tool Call Surface


Even when an enterprise controls and fully trusts every MCP server its agents connect to, the agent itself remains an attack surface. An agent that only needs` read_file` to complete its task should not be able to invoke` execute_code` , not because the server is adversarial, but because the agent can be manipulated into requesting tools it was never intended to use. Prompt injection embedded in a retrieved document, a malicious instruction hidden in a file the agent reads mid-task, is enough to redirect an otherwise well-behaved agent toward tools outside its intended scope.


[Deconvolute](https://github.com/deconvolute-labs/deconvolute) addresses this by enforcing a least-privilege policy directly on the MCP tool call surface. Rules are evaluated before any request reaches the server, and the policy supports argument-level conditions written in[CEL](https://cel.dev/) , the same expression language used by Kubernetes and Envoy. A policy that allows` read_file` only for paths under` /tmp` , blocks` execute_code` entirely, and denies everything else by default looks like this:


```text
version  :     "2.0"
default_action  :     "block"


servers  :
filesystem  :
tools  :
-     name  :     "read_file"
action  :     "allow"
condition  :     "args.path.startswith('/tmp/')"
-     name  :     "execute_code"
action  :     "block"
-     name  :     "*"
action  :     "block"
```


This policy is evaluated on the MCP session before the tool call is forwarded to the server.


### Tool Visibility and Context


The second gap is more subtle. When an MCP server advertises its full tool catalog at session start, every tool in that list enters the agent's context window and becomes available for the agent to reason about and call. On servers with large catalogs, this inflates context with tools the agent should never touch, which increases token consumption and expands the surface available to prompt injection attacks.


Deconvolute filters the tool list at discovery time, before it reaches the agent. Tools that do not match an allow rule in the policy are dropped from the response entirely. The agent never sees them, never reasons about them, and cannot be manipulated into calling them.


Figure 2: At session start, a filesystem MCP server advertises its full tool catalog. Deconvolute evaluates each tool against the policy and drops blocked tools before the list reaches the agent. The agent context window contains only the tools it is permitted to call.


## A Complete Stack


Because OpenShell and Deconvolute operate at different layers, they compose cleanly without either tool needing to know the other exists. OpenShell governs the environment: which processes run, which connections they can open, which filesystem paths they can reach. Deconvolute governs the MCP session running inside that environment: which tools the agent sees, which calls are permitted, and whether the policy conditions on those calls are satisfied.


Figure 3: A tool call from an agent passes through two independent enforcement boundaries. OpenShell evaluates whether the agent process is permitted to reach the MCP server at the network layer. Deconvolute evaluates whether the requested tool and its arguments satisfy the policy at the application layer. A request blocked by either boundary never reaches the server.


In the current SDK form, this means wrapping the MCP session in the Python agent code with` mcp_guard()` , loading a policy file, and running the agent inside the sandbox as normal. The two controls are independently configured and independently enforced, which is exactly the property you want from a defense-in-depth stack. A failure or misconfiguration at one layer does not compromise the other.


## Getting Started


If you are already running agents inside OpenShell and want to add protocol-layer enforcement, the Deconvolute SDK is available on[PyPI](https://pypi.org/project/deconvolute/) and the source is on[GitHub](https://github.com/deconvolute-labs/deconvolute) . The[quickstart guide](https://docs.deconvolutelabs.com/docs/introduction/quickstart-guide) covers installation, policy generation, and wrapping your first MCP session in a few minutes. To see the protocol-layer attacks that infrastructure controls cannot prevent, the[mcp-deconvolute-demo](https://github.com/deconvolute-labs/mcp-deconvolute-demo) repository has runnable scenarios with and without the firewall active.


The next post will cover a proxy architecture that moves protocol enforcement out of application code entirely, making it deployable across any agent framework without instrumentation. If that is of interest, watch the[Deconvolute Labs blog](https://deconvolutelabs.com/blog) and follow along on[GitHub](https://github.com/deconvolute-labs/deconvolute) .


## Frequently Asked Questions


**Does NVIDIA OpenShell inspect MCP traffic?** No. OpenShell evaluates policy at the network layer, determining whether a given binary is permitted to open a connection to a given host and port. The MCP protocol content inside that connection, tool names, arguments, and schemas, is not visible to it.


**Can OpenShell enforce which MCP tools an agent is allowed to call?** No. Tool-level enforcement requires a control that parses the MCP JSON-RPC protocol. OpenShell operates two layers below where tool names and arguments exist. Deconvolute provides this enforcement at the application layer.


**Does OpenShell protect against MCP tool poisoning or rug pull attacks?** Not directly. These attacks occur at the MCP protocol level, within a connection that OpenShell has already permitted. Detecting a tampered tool schema requires stateful protocol inspection across the session lifecycle, which is outside the scope of a network and OS sandbox.


**What is the difference between OpenShell and an MCP firewall?** OpenShell is an infrastructure-layer sandbox: it controls which processes run, which network connections they can open, and which filesystem paths they can access. An MCP firewall like Deconvolute operates at the application layer, enforcing policy on the tool calls and schemas exchanged within an MCP session. The two controls address different threat surfaces and are designed to be used together.


**How do OpenShell and Deconvolute work together?** OpenShell wraps the container environment the agent runs in. Deconvolute wraps the MCP session inside that container. Each is configured independently and neither is aware of the other. A misconfiguration or failure at one layer does not affect the other.


**What is least privilege enforcement for MCP agents?** Least privilege for MCP agents means an agent can only invoke the tools its policy explicitly permits, with arguments that satisfy defined conditions. Deconvolute enforces this at the MCP tool call surface using a declarative YAML policy and CEL-based argument inspection, before any request reaches the server.
