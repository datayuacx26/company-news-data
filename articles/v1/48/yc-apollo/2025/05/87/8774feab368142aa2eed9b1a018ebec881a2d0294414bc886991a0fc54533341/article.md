---
schema_version: "1.0.0"
document_id: "8774feab368142aa2eed9b1a018ebec881a2d0294414bc886991a0fc54533341"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/the-future-of-mcp-is-graphql"
published_at: "2025-05-15T09:04:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:fe1d711730eccd8cdae347f626092d909a10dd2a971b8c85c177075df609c048"
---

# The Future of MCP is GraphQL

Today we’re introducing[Apollo MCP Server,](https://github.com/apollographql/apollo-mcp-server) enabling AI systems like Claude and GPT to interact with your organization’s APIs through GraphQL. While you can learn more about how to get started here, I wanted to share why we think GraphQL enabled by Model Context Protocol (MCP) will be an essential part of a modern AI stack.


## Connecting AIs to APIs


MCP – a new protocol that standardizes how LLMs interface with external systems – opens the door wide open to agentic applications. APIs are the entry point to capabilities: adding items to carts, checking order status, scheduling appointments, updating inventory. When AI can reliably interact with these systems, we unlock an entirely more capable kind of software where natural language becomes the interface to complex business operations. MCP makes this possible by providing the connective tissue between AI’s language understanding and your API infrastructure.


One approach to MCP is to build an MCP server for each API you want AI to access. Your payment API gets its own MCP server, your inventory system gets another, customer data gets a third. It’s a straightforward way to get started. Most teams can build an initial MCP for a service reasonably quickly and LLMs support multiple MCP tools. It makes for a compelling experience.


As we’ve worked with teams pushing from prototype to product, we’re discovering that complex AI systems need more than what is possible by directly connecting individual APIs through MCP. We’ve found four particular requirements that show up consistently:


**Deterministic execution** : An AI will often have to call multiple APIs in sequence and filter the results to just the appropriate fields for the user. This execution pattern needs to work exactly the same way every time for consistent, correct user experiences. Consider an AI returning purchase history – you can’t have one customer seeing five past orders while another sees three, or some results including shipping details while others don’t. We’ve also found that the more focused you keep the AI’s context, the better results you get. When APIs return large objects with hundreds of fields, the extra data can lead AI down unintended paths. Limiting data to exactly what’s needed keeps the AI on track and improves overall response quality.


**Policy enforcement** : Giving AI direct access to individual APIs makes it difficult to enforce appropriate limits and access controls. A rule like “only loyalty members have access to the detailed shipping tracking information for their outstanding orders” can’t easily be written in the API layer, as it relates to a specific combination of data spread across multiple systems.


**Efficiency** : Even simple customer queries can generate thousands of tokens when APIs return complete objects. Processing all of this and directly orchestrating multiple tool invocations from the AI becomes costly, both in terms of tokens and milliseconds.


**Agility** : The pace of AI is unprecedented. The technology and techniques are progressing furiously. Just as interesting to me is that the freeform nature of AI makes it possible to deliver new functionality faster than ever before: most users wouldn’t tolerate a mobile app that changed every day, but continuous improvement of an AI agent is nothing but good news. So an AI stack needs to accommodate and facilitate a tempo we’ve never seen before in software development. This requirement rules out strategies that aren’t aligned to the modern platform engineering discipline of self-service tools and workflows.


## GraphQL solves these needs well


What these requirements point to is the need for an abstraction layer between the AI and APIs. This[orchestration layer](https://www.apollographql.com/guides/api-orchestration) exposes a set of MCP tools, where each tool provides a specific capability backed by some number of orchestrated API calls. The AI’s job is to reason about which MCP tools to invoke; the orchestration layer’s job is deterministic, efficient execution and policy enforcement.


GraphQL is an ideal foundation of this orchestration layer.


- It’s declarative: MCP tools are defined in terms of schema and queries, rather than code.
- It’s performant: one GraphQL query can traverse your entire graph, gathering exactly what’s needed (and nothing that’s not) in an optimized way.
- It’s self-documenting: LLMs can reason about the meaning of the graph and its objects.
- It’s standards-based: a mature ecosystem with proven scalability.
- It’s self-service: on a GraphQL-based platform like Apollo, everyone with access to the graph can wire up MCP tools with the appropriate level of governance.


The characteristics of GraphQL align perfectly with what AI needs to access APIs: a clean, understandable interface to AI while handling the messy reality of existing systems and services.


Consider the earlier example of retrieving purchase history with shipping updates. A GraphQL query defines an MCP tool that the AI system can use. Under the hood, that query orchestrates multiple API calls into the order, shipping and loyalty systems and enforces the necessary policy check. That query can be authored and deployed by anyone on the platform, with the support of design and governance tools provided by the graph platform.


The challenges an abstraction layer solves may seem like tomorrow’s problems when just getting started with MCP. It’s always worth planning for success, but a good declarative architecture also saves time and effort in the earliest stages of development. A GraphQL-based approach avoids having to build, debug and deploy custom MCP servers. Apollo Connectors let you bring REST APIs to GraphQL (and now MCP) without any modification. And the benefits of having a semantic API layer are truly remarkable – watching an LLM traverse a large GraphQL schema as it builds a new query is an exciting, eye-opening moment.


## Introducing Apollo MCP Server


The Apollo MCP Server represents our continued investment in making GraphQL the best way to build modern API infrastructure. We’ve spent years developing tools that help teams build, manage, and scale GraphQL – from[Apollo Client](https://www.apollographql.com/docs/react) to[Apollo Federation](https://www.apollographql.com/federation) to our most recent innovation,[Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors) . Now we’re extending that toolkit to address AI integration.


By implementing MCP for GraphQL, we’re making it simple to connect AI to your existing graph. If you’re already using Apollo, this fits naturally into your stack. If you’re new to GraphQL, the MCP server provides a compelling entry point that solves immediate AI challenges while setting you up for future growth, all on top of the APIs you already have.


We believe the architectural patterns emerging around AI – the need for precision, policy, efficiency, and agility – align perfectly with what GraphQL was designed to do. This MCP server is our way of helping teams realize those benefits.


As you explore connecting AI to your services, consider not just immediate integration but your longer-term strategy. The architectural decisions you make today will determine how quickly and safely you can evolve AI capabilities tomorrow.


Ready to get started? Connect your REST and GraphQL APIs to the[Apollo MCP server](https://github.com/apollographql/apollo-mcp-server) today, explore our Odyssey training course, or join us at our upcoming[event](https://www.apollographql.com/events/get-started-with-mcp-tools-for-your-graphql-api-today) to learn more. The future of APIs is here – powered by[graph-based API orchestration](https://www.apollographql.com/guides/api-orchestration) that makes it easier than ever to build applications.


​​


Written by


Matt DeBergalis


[Read more by Matt DeBergalis](https://www.apollographql.com/blog/author/matt)
