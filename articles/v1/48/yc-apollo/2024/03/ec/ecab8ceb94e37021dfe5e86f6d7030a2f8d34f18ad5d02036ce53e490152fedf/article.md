---
schema_version: "1.0.0"
document_id: "ecab8ceb94e37021dfe5e86f6d7030a2f8d34f18ad5d02036ce53e490152fedf"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/visualizing-federated-graphql-apis-with-graphos-studio"
published_at: "2024-03-26T12:20:22+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:22773af13762c02bc0d59ba4bb01188160881abcb0e9b81fbd3358fb5c7feaab"
---

# Visualizing federated GraphQL APIs with GraphOS Studio

GraphQL’s secret sauce is its ability to act as an abstraction layer for service-oriented backends, translating a sea of microservices into a more human-friendly representation of an organization’s data and capabilities. However, as a graph inevitably expands across more and more of the backend, it becomes increasingly difficult for the platform and backend teams that own it to fully understand its structure.


This pain is particularly acute at organizations deploying a federated GraphQL API (also known as a supergraph) at a massive scale with[GraphQL Federation](https://apollographql.com/docs/federation) . The distributed nature of developing supergraphs makes it possible to scale them across many independent backend teams and domains, but it also significantly increases the complexity of the relationships in the schema and makes it difficult to find value in existing schema visualization tools which are designed for traditional monolithic graphs.


As the creators of Federation, we want to give API developers a better way to visually explore their supergraph. Today, we’re excited to release native schema visualization in Apollo GraphOS Studio, **free** ( 🎉) for all users. API architects and developers can now use[Apollo GraphOS](https://www.apollographql.com/graphos) to visually explore the types, fields, and connections in their graph without running their graph locally, enabling introspection, or spinning up a separate visualization tool.


## Centrally explore your federated graph


Historically, generating a schema visualization for a federated graph required jumping through some hoops. You either had to run the graph with introspection enabled ([generally not recommended](https://www.apollographql.com/blog/why-you-should-disable-graphql-introspection-in-production) for production graphs) or copy-paste the supergraph SDL into a hosted visualization tool. For extra points, you could spin up your own visualization service and integrate it with your graph’s deployment pipeline, but even then you would probably experience limited returns as most out-of-the-box graph visualization libraries aren’t equipped to handle the scale and complexity of a supergraph.


With GraphOS Studio, you and your team can explore an always-up-to-date visualization of your graph with integrated capabilities like subgraph filtering and entity highlighting that simplify navigation for supergraphs. You can also copy and share a direct link to any view within the visualization to help other developers, product managers, and non-technical contributors understand the structure of the graph.


## Visualize graph performance


In addition to making large graphs more intelligible, the schema visualizer in GraphOS Studio also helps you uncover more actionable information by integrating metrics for your graph. From the Highlight drop-down in the UI, you can choose to highlight fields by error rate, request rate, and P95 latency, helping you understand which parts of the graph may need to be debugged or optimized.


To dig further into errors and performance, you can navigate to the insights page for any field directly from the schema visualization UI.


## Get started


Schema visualization is available today for all GraphOS users! To get started, log in or create a graph in[GraphOS Studio](https://studio.apollographql.com/) , and navigate to Schema > Visualization. As always, we would love to hear your feedback so we can improve our visualization tools in future iterations. Don’t hesitate to drop us a question or comment in our community[Discord](https://discord.gg/graphos) .


Happy visualizing!


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
