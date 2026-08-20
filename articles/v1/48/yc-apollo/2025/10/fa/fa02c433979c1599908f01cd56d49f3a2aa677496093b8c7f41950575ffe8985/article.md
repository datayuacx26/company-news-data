---
schema_version: "1.0.0"
document_id: "fa02c433979c1599908f01cd56d49f3a2aa677496093b8c7f41950575ffe8985"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-mcp-server-1-0-is-generally-available"
published_at: "2025-10-07T07:00:57+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:7723c7839216de14d6fac9d2d792eecc361d72ee488b07aa4ea18c708d722751"
---

# Apollo MCP Server 1.0 is Generally Available

Today we’re announcing the general availability of Apollo MCP Server 1.0!


If you’re building agentic applications, you know the challenge: LLMs need reliable, efficient access to your APIs. MCP provides the standard for this connection, and Apollo MCP Server provides a standard-based way to connect any GraphQL API to LLMs. As teams race from experiments to production in this emerging agentic world, the Apollo MCP Server is the powerful, supported, and reliable tool they need.


## **What Apollo MCP Server Does**


At its core, Apollo MCP Server turns your GraphQL API into MCP tools. In turn, any MCP client, like Claude Desktop, Cursor, Goose, or your custom agent, can discover and use those tools.


### **Core Functionality**


**Your GraphQL operations become MCP tools.** Save a query like GetCustomerOrders in your operations file or the GraphOS operation collection, and it’s instantly available as an MCP tool. No wrapper code, no manual tool definitions. The operation you test in the GraphOS Studio Explorer is exactly what your agent executes.


**Hot-reloading accelerates development.** Change your GraphQL operations, and they’re immediately available to your agent. No restart, no rebuild. This tight feedback loop is essential when you’re iterating on agent behaviors and discovering what operations actually work.


**Agents can explore your schema intelligently.** In addition to exposing GraphQL operations, the built-in introspection and execute tools let agents interact with your API dynamically, enabling a wide range of agentic workflows. This means your agent can not only execute predefined operations, but a user can also ask an agent questions like “what fields are available on the Customer type?” and receive structured answers. These tools can be disabled if an available agents’ actions need to be more narrowly scoped.


[Semantic search makes discovery efficient](https://www.apollographql.com/blog/smart-schema-discovery-how-apollo-mcp-server-maximizes-ai-context-efficiency) **.** Instead of processing your entire schema (expensive in tokens), agents can search for relevant types and fields. Looking for inventory data? The search tool returns just the relevant parts: Product.inventory, Warehouse.stockLevels, Order.inventorySnapshot. This focused discovery keeps context windows lean.


**Efficient use of tokens.** GraphQL, out-of-the-box, can be very precise with data, saving on token costs and reducing the risk of hallucination through context pollution. For agents using \`introspect\` or \`search\`, Apollo MCP Server also supports minification of their results to help optimize the context window usage. When you’re paying per token and working within context limits, these savings add up quickly across thousands of agent interactions.


**Persisted queries provide production safety.** Instead of allowing any arbitrary queries, you can restrict them to a pre-approved set of persisted operations. Each query gets a unique ID, and agents can only execute operations from your manifest. This prevents malicious or expensive queries while maintaining the flexibility to update your operation set as needs evolve.


**Contracts shape what agents can see.** Using[Apollo Contracts](https://www.apollographql.com/docs/graphos/platform/schema-management/delivery/contracts/overview) , you can create different views of your schema for different agents. Your customer support agent sees customer data and order information, but not internal pricing margins. Your analytics agent accesses aggregated data but not personally identifiable information. Same underlying GraphQL API, different bounded contexts.


### **Operational Features**


**OAuth keeps your MCP Server secure.** We implemented the[MCP Spec](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization) so that your MCP Server can perform authorization with the same identity providers as the rest of your stack. JWT validation, audience restrictions, dynamic client registration, and scope enforcement all work as expected. Read more in our[authorization blog post](https://www.apollographql.com/blog/introducing-authorization-for-apollo-mcp-server) .


**Flexible transport options.** Use stdio transport for local development with tools like MCP Inspector. Switch to streamable HTTP for remote production MCP Servers behind load balancers. The same configuration works across environments with environment variable overrides.


**YAML configuration with environment variables.** Define your configuration in YAML, override with environment variables for different deployments. Standard DevOps practices, no surprises.


**Health monitoring enables reliable deployments.** Liveness and readiness probes with configurable thresholds integrate with Kubernetes, load balancers, and monitoring systems. Know when your MCP server is healthy and ready to handle agent requests.


### **What’s New in 1.0**


**OpenTelemetry integration brings observability.** AI agents create unpredictable usage patterns and complex request flows.The Apollo MCP Server’s OpenTelemetry integration provides the visibility you need to run a reliable service for AI agents. You can monitor agent behavior (which tools and operations are used most frequently), performance (response times and bottlenecks across tool executions), reliability (error rates and request success patterns), and distributed request flows (complete traces from agent request through your Apollo Router and subgraphs, with automatic trace context propagation). The server[exports metrics, traces](https://www.apollographql.com/docs/apollo-mcp-server/telemetry) , and events to any OTLP-compatible collector, integrating seamlessly with your existing observability stack.


**CORS support enables browser-based agents.** Web applications can now connect directly to Apollo MCP Server. Build agent interfaces that run entirely in the browser, or integrate AI capabilities into existing web apps. Configure allowed origins, handle credentials properly, maintain security.


**Production ready.** Version 1.0 marks Apollo MCP Server as stable and production-ready. You can continue to engage with us on[GitHub](https://github.com/apollographql/apollo-mcp-server) , the[Apollo Community](https://community.apollographql.com/c/mcp-server/41) , and via the standard Apollo channels.


## **The GraphQL Advantage for Agents**


We’ve written before about[why GraphQL makes sense for AI agents](https://www.apollographql.com/blog/the-future-of-mcp-is-graphql) and[how GraphQL optimizes token usage](https://www.apollographql.com/blog/building-efficient-ai-agents-with-graphql-and-apollo-mcp-server) . Here’s the practical reality:


**Single request for related data.** A REST-based agent might need five API calls to gather customer order details. With GraphQL, it’s one query that fetches exactly what’s needed. Lower latency, fewer tokens, simpler agent logic.


**Self-documenting schema.** GraphQL schemas include descriptions, types, and relationships. Agents don’t just see that a customerId field exists – they understand it’s an ID type that references the Customer object. This semantic understanding helps agents make better decisions about which operations to use.


**Type safety prevents errors.** Strong typing means agents know exactly what inputs each operation expects and what data they’ll receive back. No more runtime surprises when an API returns unexpected nulls or different data types.


**Declarative relationship handling.** This is where GraphQL really shines for agents. Instead of writing code to orchestrate multiple API calls, you express relationships declaratively in your query. The agent says what it needs, and GraphQL figures out how to get it efficiently.


Consider what happens without this orchestration layer. Either you write custom code for every possible API call sequence (brittle, time-consuming), or you let the LLM figure out how to chain REST APIs together (unpredictable, error-prone). GraphQL provides a principled middle ground – relationships are defined in the schema, traversal happens predictably.


Yes, building a GraphQL schema requires upfront investment. We’re working on making this easier with tools like Apollo Connectors. But once you have that schema, it becomes the foundation for reliable agent interactions. No more writing orchestration code for each new use case. No more hoping the LLM correctly sequences API calls.


## Looking Ahead


Apollo MCP Server 1.0 serves GraphQL APIs today, but we see a broader opportunity. What if you could connect AI agents to any API or data source – REST, gRPC, databases – and Apollo handled the orchestration? This vision extends beyond just GraphQL to become the declarative orchestration platform for all AI-application interactions.


We’re already exploring this with[Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors) , which bring REST APIs into GraphQL without modification. The primitives are falling into place: schema federation, contract-based boundaries, and now MCP integration. Each piece moves us closer to a universal orchestration layer – not just for GraphQL, but for the entire stack that AI agents need to access.


## Get Started Today


Ready to connect your AI agents to GraphQL APIs?[Apollo MCP Server 1.0](https://www.apollographql.com/docs/apollo-mcp-server) is available today. Install it, connect your supergraph, and see how declarative orchestration transforms your AI applications.


Have feedback or interesting use cases? File issues on[GitHub](https://github.com/apollographql/apollo-mcp-server) or share your experiences in our[community](https://community.apollographql.com/tag/mcp) – your input directly shapes our roadmap.


Written by


Kevin Chu


[Read more by Kevin Chu](https://www.apollographql.com/blog/author/kchu)
