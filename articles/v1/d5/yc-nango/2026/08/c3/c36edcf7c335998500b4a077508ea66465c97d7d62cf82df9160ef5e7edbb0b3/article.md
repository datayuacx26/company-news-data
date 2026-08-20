---
schema_version: "1.0.0"
document_id: "c36edcf7c335998500b4a077508ea66465c97d7d62cf82df9160ef5e7edbb0b3"
company_key: "yc-nango"
company: "Nango"
source_id: "yc-nango-news-import-e2ee807d6673"
canonical_url: "https://nango.dev/blog/stateless-mcp-how-it-changes-the-way-agents-call-tools/"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-14T03:55:51.535992+00:00"
fetched_at: "2026-08-14T03:55:52.866823+00:00"
content_hash: "sha256:d377d3537cbe01403ba9f171b96e434d5780d05a2905b8536d16f845e193a352"
---

# Stateless MCP: how it changes the way agents call tools

If your product’s AI agent calls MCP servers as part of its API integrations, the protocol underneath changed on July 28, 2026. The[MCP 2026-07-28 specification](https://blog.modelcontextprotocol.io/posts/2026-07-28/) removed the` initialize` handshake and the` Mcp-Session-Id` header, making the protocol stateless by default.


## What changed in the MCP 2026-07-28 spec


The short version: every MCP request is now self-contained. A server can process any request without remembering anything from previous ones.


Before (2025-11-25 and earlier) After (2026-07-28)


Handshake \`initialize\` and \`notifications/initialized\` required before any call Removed. Version and capabilities travel in \`_meta\` on every request


Sessions Server may issue \`Mcp-Session-Id\`; client must echo it on every request Removed. Cross-call state uses explicit handles in tool arguments


Server capabilities Returned once in the \`initialize\` response \`server/discover\` RPC, optional to call


Server-to-client requests (sampling, elicitation) Sent as JSON-RPC requests on open SSE streams Embedded in results as input requests (MRTR pattern)


Unsolicited notifications Pushed on a long-lived HTTP GET stream Opt-in \`subscriptions/listen\` stream per notification type


Broken response streams Resumable via \`Last-Event-ID\` Cannot resume with \`Last-Event-ID\`. Re-issue with a new request ID, tools may need app-level idempotency or deduplication


Routing metadata None \`Mcp-Method\` and \`Mcp-Name\` HTTP headers required


List caching Unsafe (results could vary per session) Safe. Results carry \`ttlMs\` and \`cacheScope\`


Stateless transport does not make every request safe to retry. If a response stream drops after a side-effecting tool commits, the client may not know whether the operation completed. Automatically retry only idempotent operations. For writes, use an idempotency key, server-side deduplication, or a way to check the original operation’s status.


The full list of changes is in the[changelog](https://modelcontextprotocol.io/specification/2026-07-28/changelog) .


The[motivation behind the change](https://modelcontextprotocol.io/seps/2575-stateless-mcp) was to simplify MCP infrastructure. A stateful MCP server needs sticky sessions: every request from a client has to reach the specific instance holding its session state. That rules out plain load balancers, serverless platforms, and most edge runtimes. A stateless server is an ordinary HTTP workload, so any request can hit any instance.


## The Streamable HTTP tool call flow, before and after


Before the change, an agent needed a full MCP handshake before its first tool call.` initialize` negotiated the protocol version and capabilities, the server could return an` Mcp-Session-Id` , and the client had to send` notifications/initialized` plus the session header on everything that followed:


```text
sequenceDiagram
participant Agent as Agent (MCP client)
participant Server as MCP server (one specific instance)


Agent->>Server: POST initialize
Server-->>Agent: capabilities, protocol version, Mcp-Session-Id
Agent->>Server: POST notifications/initialized (Mcp-Session-Id)
Agent->>Server: POST tools/list (Mcp-Session-Id)
Server-->>Agent: tool definitions
Agent->>Server: POST tools/call (Mcp-Session-Id)
Server-->>Agent: result
```


That is four HTTP requests before the first result, all pinned to one server instance.


Under the new revision, the agent goes straight to` tools/list` and` tools/call` . Each request carries its protocol version, client identity, and capabilities in` _meta` , so any instance behind a load balancer can serve it:


```text
sequenceDiagram
participant Agent as Agent (MCP client)
participant LB as Load balancer
participant A as Instance A
participant B as Instance B


Agent->>LB: POST tools/list (version + capabilities in _meta)
LB->>A: any instance works
A-->>Agent: tool definitions (with ttlMs cache hint)
Agent->>LB: POST tools/call (self-contained)
LB->>B: any instance works
B-->>Agent: result
```


The new` tools/call` request looks like this:


#####


```text
POST   /mcp   HTTP  /  1.1
Content-Type  :   application/json
Accept  :   application/json, text/event-stream
MCP-Protocol-Version  :   2026-07-28
Mcp-Method  :   tools/call
Mcp-Name  :   get_customer


{
"jsonrpc"  :   "  2.0  "  ,
"id"  :   1  ,
"method"  :   "  tools/call  "  ,
"params"  : {
"name"  :   "  get_customer  "  ,
"arguments"  : {   "email"  :   "  jo@example.com  "   },
"_meta"  : {
"io.modelcontextprotocol/protocolVersion"  :   "  2026-07-28  "  ,
"io.modelcontextprotocol/clientInfo"  : {   "name"  :   "  my-agent  "  ,   "version"  :   "  1.0.0  "   },
"io.modelcontextprotocol/clientCapabilities"  : {}
}
}
}
```


Three things to notice:


- **` Mcp-Method` and` Mcp-Name` mirror body fields into headers** so gateways and load balancers can route or rate-limit tool calls without parsing the JSON-RPC body.
- **The` MCP-Protocol-Version` header is mandatory** and must match the` io.modelcontextprotocol/protocolVersion` field in` _meta` . A mismatch returns a` 400 Bad Request` with a` HeaderMismatch` error (code` -32020` ).
- **Capability discovery is now optional.** Servers must implement a` server/discover` RPC that returns their supported versions and capabilities, but clients are free to skip it and call` tools/list` directly. If the server does not support your version, it returns an` UnsupportedProtocolVersionError` (code` -32022` ) listing versions it does support, and you retry with one of those.


For an agent, this reduces latency: the first tool call drops from four HTTP round trips to two. If you cache the tool list (which is safe to do), it drops to one.


## How to talk to servers that still use an MCP session ID


Your agent will face a mix of old and new servers while implementations migrate. A production MCP client needs to support both types.


The[MCP spec](https://modelcontextprotocol.io/specification/2026-07-28/basic/versioning) defines the detection flow for HTTP: send a modern request first, and inspect the response before falling back.


```text
flowchart TD
A["POST a modern request<br/>(MCP-Protocol-Version: 2026-07-28)"] --> B{Response}
B -->|2xx| C[Modern server.<br/>Continue statelessly]
B -->|"400 / -32022"| D[Modern server.<br/>Select a mutually supported version]
B -->|"400 / -32020 or -32021"| E[Modern server.<br/>Correct the request or capability]
B -->|"Other 400"| F[Legacy server.<br/>Fall back to initialize]
B -->|"Other status or network error"| X[Surface the failure.<br/>Do not downgrade]
F --> G{initialize response<br/>includes Mcp-Session-Id?}
G -->|Yes| H[Send the header on every request.<br/>Required, not optional]
G -->|No| I[Continue on the legacy version<br/>without sessions]
```


In code:


#####


```text
const   MODERN_ERRORS   =   new   Set  ([  -  32020  ,   -  32021  ,   -  32022  ]);
//   HeaderMismatch, MissingRequiredClientCapability, UnsupportedProtocolVersion


async   function   detectMCPEra  (  mcpUrl  :   string  )  :   Promise  <  '  modern  '   |   '  legacy  '  > {
const   res   =   await   postModernRequest  (  mcpUrl  ,   '  tools/list  '  );


if   (res  .  ok  )   return   '  modern  '  ;


const   body   =   await   res  .  json  ()  .  catch  (  ()   =>   null  );


if   (
res  .  status   ===   400   &&
body  ?.  error   &&
MODERN_ERRORS  .  has  (body  .  error  .  code  )
) {
//   The server understands modern MCP.
//   -32020: correct the headers
//   -32021: handle the required client capability
//   -32022: select a version from error.data.supported
return   '  modern  '  ;
}


if   (res  .  status   ===   400  ) {
//   An unrecognized 400 is the HTTP legacy-server signal.
return   '  legacy  '  ;
}


throw   new   Error  (
`  MCP probe failed with HTTP   ${  res  .  status  }  ; refusing to downgrade  `
);
}
```


The three recognized error codes prove that the server understands modern MCP. Only` -32022` is a version-negotiation response: select a mutually supported version from` error.data.supported` and retry. For` -32020` , correct the routing or version headers. For` -32021` , declare the required client capability if your client supports it, or fail the call. Authentication failures, rate limits, timeouts, and server errors are not signals to downgrade.


Important details:


- **The session header is not optional on legacy servers.** For MCP servers built on older[revisions](https://modelcontextprotocol.io/specification/2025-11-25/basic/transports) , if the` initialize` response includes an` Mcp-Session-Id` , the client must include it on every subsequent request. Servers are allowed to reject requests without it.
- **Cache the era per server.** Era is a property of the server, not of a request. The spec recommends caching the detection result for the lifetime of the origin and re-probing only if the cached assumption fails.
- **Expect` 405 Method Not Allowed` from new servers.** A 2026-07-28-only server answers legacy traffic predictably: GET and DELETE on the MCP endpoint get a 405, and any` Mcp-Session-Id` header is ignored.
- **On stdio, probe with` server/discover` first.** There is no HTTP status code to inspect, so the spec recommends calling` server/discover` and falling back to` initialize` on any error that is not a recognized modern one.


## Sessions were used to store info: where did that go?


Sessions used to be the implicit container for anything a server kept between calls: a database connection, a shopping cart, a browser instance. The replacement is explicit handles, a tool-design pattern documented in[SEP-2567](https://modelcontextprotocol.io/seps/2567-sessionless-mcp) : a creation tool returns an ID, and the model passes that ID back as an ordinary argument on later calls.


#####


```text
//   tools/call: create_basket -> returns a handle
{   "structuredContent"  : {   "basket_id"  :   "  bsk_a1b2c3  "   } }


//   tools/call: add_item -> the model threads the handle through
{   "name"  :   "  add_item  "  ,   "arguments"  : {   "basket_id"  :   "  bsk_a1b2c3  "  ,   "sku"  :   "  shoes  "   } }
```


Well-designed remote MCP servers already worked this way: GitHub’s server returns a PR number that later calls operate on, Stripe’s returns customer IDs.


For agent builders, two consequences:


- **Handles live in the conversation, so protect them from compaction.** If your agent summarizes history mid-task and the handle was only in the discarded portion, the state is orphaned. Keep tool results with handles across compaction boundaries.
- **Long-running operations get a real primitive.** Connection drops now cancel in-flight requests (resumable streams are gone), so work that outlives a request should use the[Tasks extension](https://modelcontextprotocol.io/extensions/tasks/overview) (` io.modelcontextprotocol/tasks` ). The server returns a durable` taskId` instead of blocking, and your client polls` tasks/get` until the task completes, even across reconnects.


## Why stateless MCP is good for agent builders


The changes cost some migration work, but it’s worth it:


- **Servers get cheaper and more reliable.** A stateless MCP server runs on serverless platforms, edge runtimes, and plain load balancers without sticky-session configuration. Session affinity was the main blocker to running MCP at scale. Expect the remote MCP servers your agent depends on to fail less and scale further.
- **Transport recovery gets simpler.** A retry no longer depends on recovering an MCP session or reaching the same server instance. This does not make the tool operation itself idempotent. Clients can automatically retry reads and idempotent calls, but write operations need an idempotency key, server-side deduplication, or a way to check whether the original operation completed.
- **Tool lists are finally cacheable.** Results from` tools/list` carry a` ttlMs` freshness hint and a` cacheScope` , and servers should return tools in deterministic order. For an orchestrator spawning subagents, this turns` tools/list` traffic from one fetch per subagent per server into one fetch per server, and stable ordering improves LLM prompt cache hit rates on the tool definitions themselves.
- **Less client code.** No session tracking, no handshake state machine, no reconnection-and-resync logic. A minimal MCP client is now close to a well-formed` fetch` call.


## Stateless MCP simplifies transport, not API integrations


The 2026 revision removes protocol-level sessions, but it does not remove the provider-specific work around tool calls. Agents still need to authorize each user, store and refresh credentials, implement API operations, handle rate limits, and trace failures.


[Nango](https://nango.dev/) supports two ways of connecting agents to external systems:


- **Call external MCP servers with managed authorization.**[Nango MCP Auth](https://nango.dev/docs/guides/auth/mcp-auth) lets users authorize servers such as Notion, Linear, and HubSpot through your application. Nango handles the OAuth registration flow,[using CIMD when supported and DCR as a fallback](https://nango.dev/docs/updates/changelog#cimd-support-for-mcp-auth) , stores the credentials, and proxies MCP requests with the correct user’s authorization. Your MCP client remains responsible for protocol-version negotiation and legacy session handling.
- **Expose API integrations through one controlled tool surface.** Nango offers 6,000+ customizable tools across[900+ APIs](https://nango.dev/api-integrations) . Tools can be exposed over REST or through Nango’s[hosted MCP server](https://nango.dev/docs/guides/functions/tool-calling#mcp-server) .


Stateless MCP makes the transport easier to operate. Nango handles the integration lifecycle around it: authorization, credentials, tool execution, and observability.


## FAQ


### What is stateless MCP?


Stateless MCP means every request is self-contained and does not rely on protocol-level session state from previous requests. Servers can still maintain application state behind explicit handles. The` initialize` handshake and the` Mcp-Session-Id` header were removed. Any request can be served by any server instance, which makes MCP servers load-balanceable like ordinary HTTP services.


### How do agents keep state across MCP tool calls without sessions?


Similar to well-designed REST APIs: a creation tool call returns an ID (e.g.` create_basket` returning` basket_id` ), and the model passes that ID as an argument on subsequent calls. The server owns the state and checks authorization on every call.


### Does MCP still use SSE?


Yes, but only per request. A server can answer any POST with an SSE stream carrying progress notifications before the final result, and` subscriptions/listen` responses are long-lived SSE streams.


## Conclusion


The 2026-07-28 MCP revision makes MCP behave like the rest of the web: self-contained requests, explicit state, cacheable responses. MCP clients (agent frameworks) will have to adopt the standard, but the payoff is remote MCP servers that scale on ordinary infrastructure.


*Related reading:*


- [MCP vs tool calls for AI agents: which is better?](https://nango.dev/blog/mcp-vs-tool-calls-for-ai-agents)
- [MCP gateway vs MCP proxy: Do you need one?](https://nango.dev/blog/mcp-gateway-vs-mcp-proxy)
- [Best MCP servers for agent API integrations in 2026](https://nango.dev/blog/best-mcp-servers-for-agent-api-integrations)
- [How to build reliable tool calls for AI agents integrating with external APIs](https://nango.dev/blog/build-reliable-tool-calls-for-ai-agents-integrating-with-external-apis)
- [A complete guide to securing AI agent API authentications](https://nango.dev/blog/guide-to-secure-ai-agent-api-authentication)
