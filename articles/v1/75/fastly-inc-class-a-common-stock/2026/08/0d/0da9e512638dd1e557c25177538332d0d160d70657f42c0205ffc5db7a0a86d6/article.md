---
schema_version: "1.0.0"
document_id: "0da9e512638dd1e557c25177538332d0d160d70657f42c0205ffc5db7a0a86d6"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/mcp-at-the-edge/"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T16:31:56.014844+00:00"
fetched_at: "2026-08-19T16:31:57.744876+00:00"
content_hash: "sha256:6c1452ed0d812e70c7a6ca00958065b4aeb2172d7b01a52a0ff5917a995e6878"
---

# MCP at the Edge: What Changes When MCP Runs Securely in Every POP

[Model Context Protocol](https://modelcontextprotocol.io/) (MCP) servers give AI agents a standard way to call tools, read resources, and pull in prompts in bulk.


Most MCP servers run in a legacy cloud architecture: one region, behind a load balancer, holding a session per client. That's reasonable until an agent on the other side of the world is making forty tool calls to finish one task, and each call pays a transcontinental round trip inside its reasoning loop. Those long-distance round trips compound fast and kill your app's performance.


Running your MCP server on[Fastly Compute](https://developer.fastly.com/learning/compute/) as a WebAssembly binary across all of our datacenters, powered and secured by our 620+ TbPS global platform, changes the nature of what MCP can do for agents and the humans who control them.[We just shipped an example project that does exactly that](https://github.com/fastly/edge-mcp) and is ready for your coding agents to reference as they modernize your own MCP stack.


You no longer need to pay for niche hosting or isolate your server in a single geo region. Also, you can test the entire project locally against[Viceroy](https://github.com/fastly/Viceroy) , Fastly's local Compute runtime, before deploying it to the network. Below, we'll break down how running stateless MCP at the edge works under the hood.


## Resolving latency where it matters: inside the agent loop


Agents don't make one call and stop. They discover tools, call one, read the result, call another, sometimes loop a dozen times before they answer. Every one of those hops is in the critical path of the agent’s workflow. If you’re hosting your MCP server in a traditional architecture, an end user developer relying on your MCP server across the world might experience up to 500ms of total delay for each call – and their agent might make dozens of calls per action. That delay adds up quickly and produces a terrible end user experience.


At the edge, the server is milliseconds from wherever any request originates. There’s no single origin region limitation and no backhaul to a home data center. The tool catalog, the call dispatch, the auth check: all handled at the nearest POP. For a chatty agent workload, shaving hundreds of milliseconds off *each* call compounds into a noticeably faster agentic loop.


## Stateless by design — so any POP can answer


The[newest MCP revision (2026-07-28)](https://modelcontextprotocol.io/specification/2026-07-28) is built to be stateless. This is more in line with HTTP best practices, and we're grateful to support the[Agentic AI Foundation](https://aaif.io/) in their research around this and future breakthroughs.


The notable difference in this revision is that there's no initialization handshake. Every request carries its own identity and negotiated capabilities in a _meta block, and any state that needs to survive between calls travels *with the request* rather than living on the server.


Since there's no session pinned to a machine, a stateless edge deployment is now much more compelling than in previous specifications. With this implementation on a modern platform like Fastly’s:


-


Your MCP server is instantly deployed everywhere. A request can land on any Fastly datacenter worldwide and be handled correctly.


-


Scaling is horizontal and automatic with no cold start lag. Fastly spins up your MCP server in less than 50 microseconds.


-


Each request is securely isolated within our WebAssembly-based runtime, eliminating any noisy neighbor security risk.


You get the operational simplicity of a globally distributed platform with a single deploy.


## Long-running and multi-step work without holding a connection


"Stateless" usually sounds incompatible with "long-running." The spec's two continuation mechanisms make it work, and our implementation leans on both:


-


**Multi-Round-Trip Requests (MRTR):** a tool can pause mid-call to ask the client for input and resume later. The in-flight state is sealed into an opaque token the client echoes back on the next call. No connection is held open; any instance picks the work back up.


-


**The Tasks extension:** a tool that can trigger long-running work and return a task handle. The client polls for completion instead of holding a socket for the duration. This fits Fastly’s request model well.


Both continuation tokens are cryptographically sealed with authenticated encryption and bound to the caller's identity, so the "state travels with the request" model doesn't become a way to forge or hijack work.


## The tool catalog, served from cache


Discovery and listing calls —` server/discover, tools/list, prompts/list, resources/list` — return the same answer to every caller and rarely change. They're the perfect thing to[cache at the edge](https://developer.fastly.com/learning/concepts/edge-caching/) via Fastly’s native caching functionality.


The server marks these responses with freshness and cache-scope hints, so the cache can serve the tool catalog to agents worldwide without re-running the handler each time. Anything that depends on *who's* asking is automatically excluded from the cache and never leaks across requests.


## Security enforced before anything reaches your origin


Because Fastly sits in front of whatever your tools actually talk to, it's the right place to enforce access. We made sure to build the following functionality into the prototype:


-


**Authentication is fail-closed by default** : a misconfiguration results in a locked-down or unavailable endpoint, never a silently open one.


-


**Bearer tokens are verified at the edge** :[ES256](https://datatracker.ietf.org/doc/html/rfc7518#section-3.1)[JWTs](https://datatracker.ietf.org/doc/html/rfc7519) validated against a cached[JWKS](https://datatracker.ietf.org/doc/html/rfc7517) so unauthenticated or malformed requests are rejected before they cost you anything downstream.


-


**Authorization is default-deny and scoped** : a caller must hold the specific scopes a tool declares; a tool that declares none isn't callable under auth.


-


**Request bodies are capped before parsing** : JWKS fetches are[SSRF](https://owasp.org/www-community/attacks/Server_Side_Request_Forgery) -guarded, and internal errors are redacted with correlation IDs for debugging.


An unauthorized or abusive request is stopped within Fastly and never reaches your backend.


## Boosted by the full power of the Fastly Platform


The MCP prototype runs within Fastly compute. This means that it gets all the resilience and capabilities of the platform as tailwinds, such as:


-


sub-50 microsecond cold-starts


-


secure isolation via our WebAssembly runtime


-


DDoS protection with one-click


-


620+ TbPS of capacity when you need it


-


request collapsing to prevent a thundering herd problem when your product goes viral


The stateless MCP spec and Fastly’s runtime reinforce each other: the protocol was designed so state can travel with the request, and the edge is where that design thrives. What was previously turning what's normally a single-region, session-bound service into something that's *everywhere* , close to every agent that calls it.


To try it out,[check out the project on GitHub.](https://github.com/fastly/edge-mcp)
