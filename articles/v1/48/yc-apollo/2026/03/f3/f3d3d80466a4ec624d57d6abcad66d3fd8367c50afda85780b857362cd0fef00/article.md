---
schema_version: "1.0.0"
document_id: "f3d3d80466a4ec624d57d6abcad66d3fd8367c50afda85780b857362cd0fef00"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-orchestrate-multiple-apis-for-your-llm-agent"
published_at: "2026-03-26T10:00:28+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:00:16.885857+00:00"
content_hash: "sha256:e51cde6709c3e1c1b8919a42b0b2f988452c07286c21832858eb07e0939ac75a"
---

# How to Orchestrate Multiple APIs for Your LLM Agent

Most real-world applications don’t have one API. They have many: user services, product catalogs, inventory systems, third-party integrations, each with its own interface, authentication model, and response shape. When you start building AI agents on top of this landscape, you quickly run into a fundamental problem: how do you give an LLM reliable, governed access to all of that data without writing a custom integration layer from scratch?


Teams with an existing GraphQL API are well-positioned to connect AI agents to their data. GraphQL’s design makes it a natural fit for agents, and[Apollo MCP Server](https://www.apollographql.com/docs/graphos/mcp) turns your existing GraphQL operations into AI-callable tools without any custom integration code.


This post explains how these pieces fit together and what it looks like in practice.


## **The Orchestration Problem**


Imagine you’re building an AI agent that needs to answer questions about upcoming space launches, pulling data from mission records, astronaut profiles, celestial body catalogs, and launch schedules. Each of these may live in a different service with a different API contract.


You have a few options. You could write a custom connector for each service and hard-code the logic for combining results. Alternatively, you could expose everything through a generic REST API and hope the LLM figures out which endpoints to call and in what order. Or you could use a GraphQL endpoint that already orchestrates those services behind a single, introspectable interface, and then make that endpoint available to your AI agent through MCP.


The third option requires the least custom work and produces the most reliable agent behavior. Here’s why.


## **Why GraphQL Works Well for AI Agents**


GraphQL’s core design principles (declarative queries, strong typing, and schema introspection) turn out to be exactly what AI agents need to work reliably with APIs.


**Introspection makes the API self-describing.** GraphQL’s introspection system lets any client, including an AI agent, query the schema itself to discover all available operations at runtime. This is a structural advantage that REST APIs simply don’t have. Rather than relying on documentation that may be stale, agents get a live, machine-readable map of your domain that updates as your schema evolves.


**Strong typing eliminates guesswork.** REST APIs require agents to infer behavior from endpoint names and query parameters. GraphQL’s type system, by contrast, provides deterministic specs: every type, field, argument, and relationship is declared explicitly. As a result, the agent knows exactly what inputs a query requires and what shape the response will take before making the call.


**Declarative queries match how agents think.** Instead of navigating a sprawl of endpoints, agents describe what data they need in a single query. Given a high-level instruction, an agent can determine the required fields and request them directly, with no endpoint mapping required.


**A single entry point reduces complexity.** Rather than navigating dozens of REST endpoints with different conventions and response shapes, an AI agent interacts with one GraphQL endpoint. The underlying services can be as complex as they need to be; the agent, however, only sees a clean, unified interface.


**Precise field selection keeps responses clean.** Agents can request exactly the fields they need, nothing more. This approach reduces noise in the response and makes reasoning about the data more reliable.


## **How MCP Standardizes Agent-to-API Communication**


To understand the orchestration architecture, it helps to first understand what MCP is and what problem it solves.


Before MCP, connecting an AI agent to an external API meant writing custom integration code for each pairing: a connector for your GraphQL API, another for your database, another for your third-party services. Every new tool meant new code. Every new agent meant starting over.


MCP, an open standard developed by Anthropic and now adopted by major AI providers including Microsoft, Google, and OpenAI, solves this with a universal protocol. Think of it the way you think of HTTP for web pages or DNS for domain names: MCP defines *how* communication happens between agents and services, so developers can focus on *what* they’re communicating rather than the mechanics of connecting them. It defines three components:


- **Hosts** are the AI applications themselves (Claude, ChatGPT, or your own agent framework) that orchestrate everything: managing clients, presenting tools to LLMs, and executing tool calls
- **Servers** connect to external APIs and translate their operations into tools that AI systems can understand and invoke
- **Clients** are embedded in AI applications to connect to MCP servers, discover available tools, and invoke them when the LLM requests it


The key insight is that MCP decouples tool *implementation* from tool *consumption* . A service provider builds an MCP server once; any MCP-compatible AI application can use it immediately. The ecosystem of MCP servers now numbers in the thousands, and any GraphQL API with an Apollo MCP Server instance is automatically part of it.


## **How Apollo MCP Server Connects GraphQL to Your AI Agent**


Apollo MCP Server is Apollo’s implementation of the MCP standard for GraphQL APIs. It wraps any GraphQL endpoint and makes its operations available to AI systems as standardized tool calls, without requiring changes to your existing API, resolvers, or authorization logic. The result is a clean separation of concerns: your GraphQL API keeps serving applications unchanged, your agent focuses on reasoning, and Apollo MCP Server handles protocol translation, schema validation, and secure execution in between.


Here’s how the data flow works at runtime:


1. The AI host application connects to Apollo MCP Server on startup and fetches the available tool definitions
2. When the LLM needs data, it requests a specific tool call with the appropriate arguments
3. The AI system’s MCP client forwards that request to Apollo MCP Server
4. Apollo MCP Server validates the request against the schema, executes the GraphQL operation against your endpoint, and returns structured data through the MCP protocol
5. The LLM incorporates the result and can chain additional tool calls to complete complex tasks


This agentic loop (where an LLM reasons, calls a tool, gets a result, and reasons again) is what enables agents to handle multi-step workflows. As a result, Apollo MCP Server makes your GraphQL API a reliable participant in that loop.


### Controlling What Agents Can Access


You decide exactly which operations to expose as MCP tools. Apollo MCP Server supports several approaches:


- **Local operation files:** define` .graphql` operation files that map directly to tool definitions
- **Persisted query manifests:** use pre-approved query lists for tightly governed access
- **GraphOS-managed persisted queries:** centrally manage and hot-reload approved operations through Apollo GraphOS without restarting the server
- **Schema introspection:** enable dynamic tool discovery so agents can explore and execute operations across your entire schema


Each approach sits at a different point on the flexibility-versus-governance spectrum. Teams that need tight control over agent behavior use persisted queries; teams that want agents to explore a broader schema surface enable introspection with appropriate authentication.


## **A Concrete Example: The Space Devs API**


Let’s take Apollo MCP Server for a ride. We’ll use this[working example](https://github.com/apollographql/apollo-mcp-server/tree/main/graphql/TheSpaceDevs) built on The Space Devs API: a GraphQL endpoint covering rocket launches, astronaut profiles, and celestial body data.


### **Tools and Initial Setup**


To get started, install the[Apollo Rover CLI](https://www.apollographql.com/docs/rover) , then clone the repo:


```text
git clone https://github.com/apollographql/apollo-mcp-server.git
```


### **Start Local Services**


Running the example locally with the Rover CLI starts both the GraphQL server and the MCP server in a single command:


```text
% rover dev --supergraph-config ./graphql/TheSpaceDevs/supergraph.yaml \
--mcp ./graphql/TheSpaceDevs/config.yaml


merging supergraph schema files
supergraph config loaded successfully
starting a session with the 'thespacedevs' subgraph
==> Watching /Users/demouser/git/apollo/apollo-mcp-server/graphql/TheSpaceDevs/api.graphql for changes
composing supergraph with Federation 2.10.0
==> Attempting to start router at http://localhost:4000.
==> Health check exposed at http://127.0.0.1:8088/health
WARN: Connector debugging is enabled, this may expose sensitive information.
==> Your supergraph is running! head to http://localhost:4000 to query your supergraph
2025-08-24T05:25:15.010628Z  INFO Apollo MCP Server v0.7.2 // (c) Apollo Graph, Inc. // Licensed under MIT
2025-08-24T05:25:15.015315Z  INFO Tool SearchUpcomingLaunches loaded with a character count of 545. Estimated tokens: 136
2025-08-24T05:25:15.015461Z  INFO Tool ExploreCelestialBodies loaded with a character count of 759. Estimated tokens: 189
2025-08-24T05:25:15.015577Z  INFO Tool GetAstronautDetails loaded with a character count of 898. Estimated tokens: 224
2025-08-24T05:25:15.015668Z  INFO Tool GetAstronautsCurrentlyInSpace loaded with a character count of 501. Estimated tokens: 125
2025-08-24T05:25:15.020958Z  INFO Indexed 50 types in 5.15ms
2025-08-24T05:25:15.021362Z  INFO Starting MCP server in Streamable HTTP mode port=5000 address=127.0.0.1
```


The GraphQL server is running locally on port` 4000` , while the MCP server (which is what we’re focusing on) is running on` 5000` .


### **Review the Configuration**


The transport is streamable HTTP, which is typically used for remote transport, even when your MCP server is running locally. There is an` operations` dictionary that contains allowed operations, and` introspection` is also enabled.


```text
endpoint: https://thespacedevs-production.up.railway.app/
transport:
type: streamable_http
operations:
source: local
paths:
- ./graphql/TheSpaceDevs/operations
schema:
source: local
path: ./graphql/TheSpaceDevs/api.graphql
overrides:
mutation_mode: all
introspection:
execute:
enabled: true
introspect:
enabled: true
search:
enabled: true
```


This configuration exposes curated operations as tools while also enabling introspection, giving the agent both a curated set of tools and the ability to explore the schema dynamically when needed.


### **Demo: Add Apollo MCP Server to Claude Desktop**


For this demonstration, let’s see how we can extend[Claude Desktop](https://claude.ai/download) to query the Space Devs API with natural language. Claude Desktop serves as the AI system with built-in MCP support, meaning you can configure it to use any MCP server’s tools.


Adding MCP servers requires editing the config file for Claude Desktop. You can find this in the developer settings for Claude Desktop.


When you click **Edit Config** , you will see the location of` claude_desktop_config.json` . Open the file and add the following to the` mcpServers` dictionary:


```text
"thespacedevs": {
"command": "npx",
"args": [
"mcp-remote",
"http://127.0.0.1:5000/mcp"
]
}
```


Here is a complete config file, which also includes another MCP server called filesystem:


```text
{
"mcpServers": {
"filesystem": {
"command": "npx",
"args": [
"-y",
"@modelcontextprotocol/server-filesystem",
"/Users/demouser/Desktop",
"/Users/demouser/Downloads"
]
},
"thespacedevs": {
"command": "npx",
"args": [
"mcp-remote",
"http://127.0.0.1:5000/mcp"
]
}
}
}
```


You’ll need to restart Claude Desktop for the settings to take effect. Now, when you ask Claude about space-related information, it will use GraphQL to fetch it for you.


## **Extending MCP’s Reach with Apollo Connectors**


Not every backend you need to expose is already a GraphQL API. Many organizations, for instance, have a mix of GraphQL services, REST APIs, and other internal services that all need to be accessible to their agents.


[Apollo Connectors](https://www.apollographql.com/docs/graphos/connectors) let you wrap REST APIs in GraphQL without writing custom resolvers, bringing them into your GraphQL schema and, by extension, making them available through Apollo MCP Server. As a result, your AI agents can access a much broader slice of your service ecosystem through a single, governed interface. And if your organization has grown to the point of running multiple GraphQL services across different teams,[Apollo Federation](https://www.apollographql.com/docs/federation) can unify them all behind one endpoint, giving agents a single entry point to your entire data graph.


## **Wrapping Up**


Building AI agents that reliably work with your APIs is an architectural challenge as much as an implementation one. The right foundation, therefore, makes the difference between agents that work predictably and agents that require constant attention.


GraphQL gives AI agents a single, self-describing entry point to your data. MCP, in turn, gives those agents a standard way to discover and call tools. Apollo MCP Server connects the two, without custom integration code, without changes to your existing infrastructure, and with the governance controls that production deployments require.


If you’re ready to get started, clone the[Apollo MCP Server repository](https://www.apollographql.com/docs/apollo-mcp-server) and run the Space Devs example. It’s the fastest way to see the full architecture in action. When you’re ready to connect your own GraphQL API, the path forward is straightforward: define your operations, configure the server, and point your agent at it.


For a deeper dive into schema design decisions that make your GraphQL API more effective for AI agents, see[How To Make Your Existing GraphQL API AI-Ready With Apollo](https://claude.ai/chat/2f8e3be3-8cc1-406a-a7fa-b57386d49018#) . For a step-by-step implementation tutorial, see[How to Build AI Agents Using Your GraphQL Schema](https://claude.ai/chat/2f8e3be3-8cc1-406a-a7fa-b57386d49018#) .


Written by


Kaitlyn Barnard


[Read more by Kaitlyn Barnard](https://www.apollographql.com/blog/author/kbarnard)
