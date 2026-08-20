---
schema_version: "1.0.0"
document_id: "46aebbcbf3a486a2aa80b89259a718a9723e22826f27b43a1402de5688ed8f39"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-83888f119c8c"
canonical_url: "https://www.apollographql.com/blog/securing-apollo-federation-subgraphs-context-and-best-practices"
published_at: "2026-01-14T15:12:21+00:00"
first_seen_at: "2026-07-24T16:23:53.794779+00:00"
fetched_at: "2026-07-28T22:23:37.214552+00:00"
content_hash: "sha256:73f22b257b4565342ea8fcca05df7ed53f1c323bd2460f32833bc1ead15d1e63"
---

# Securing Apollo Federation Subgraphs: Context and Best Practices

Recent security research has reinforced important considerations around Federated subgraph exposure. This work is increasingly critical as AI-powered tools make it easier than ever to discover and exploit publicly accessible services that were never intended to be exposed.


**Apollo Federation subgraphs are intended to remain internal services, accessible only through the router.** This architectural requirement for isolation is fundamental to Federation’s security model. It also reflects a broader API strategy principle: centralized governance and observability through a unified access layer are essential to avoid API sprawl. This is not a vulnerability in Federation itself, but a deployment and trust-boundary issue that arises when internal services are exposed outside their intended architecture.


The router is designed to be the centralized security enforcement point, providing critical protections like access control, demand control, and operation safelisting. When subgraphs are inadvertently made public, they can bypass these protections, exposing internal coordination mechanisms that are intended only for router-to-subgraph communication.


We will continue to make these security patterns prominent in our documentation and welcome ongoing collaboration with security researchers to help the Federation community build more secure GraphQL architectures.


## **Understanding Federation Architecture**


Apollo Federation uses internal coordination fields to enable distributed graph functionality, allowing the router to query schemas and fetch entities across subgraph boundaries. GraphQL libraries serving Federated subgraphs automatically expose these coordination fields to maintain Federation compatibility, even when standard introspection is disabled for clients. These mechanisms exist by design as internal protocols that require a critical trust boundary: **only the router should communicate with subgraphs** . For this reason, subgraphs should operate behind the router with protections like network isolation and authentication preventing direct subgraph access. This further centralizes governance and security into the router.


When subgraphs are directly accessible, you bypass the router’s security enforcement entirely and internal coordination mechanisms become information disclosure vectors equivalent to leaving introspection enabled.


## **Recommended Security Practices**


If you’re running Apollo Federation, follow these practices to ensure proper security:


**API Discoverability**


- **Keep subgraphs internal** – **** Subgraphs should only be accessible from your router, never directly from the public internet, enabling you to leverage its centralized governance and security enforcement.
- **Turn off introspection in production** –[GraphQL’s built-in introspection query](https://www.apollographql.com/blog/why-you-should-disable-graphql-introspection-in-production#turning-off-introspection-in-production) reveals your entire schema to bad actors, but note this does not replace the need to keep subgraphs internal.
- **Obfuscate error details** –[Remove verbose error information](https://www.apollographql.com/docs/apollo-server/data/errors#omitting-or-including-stacktrace) like stack traces from your production server


**Defend Against Malicious Operations**


- **Validate and sanitize data** – Apply standard input validation; use enums, custom scalars, and directives to codify validation in your schema
- **Paginate fields** – Set maximum limits on how many items can be requested at once
- **Implement authentication/authorization in the router** – Enforce auth at the entry point to drop invalid requests before they reach downstream services
- **Set operation limits** – Configure max depth, max root fields, and other request limits to prevent resource exhaustion
- **Use operation cost analysis** – Calculate operation costs and configure demand control for rate limiting based on query complexity
- **Safelist with persisted queries** –[Register trusted operations](https://www.apollographql.com/blog/secure-your-graphql-platform-by-safelisting-known-queries) in a[persisted query list](https://www.apollographql.com/docs/kotlin/advanced/persisted-queries) (PQL); router rejects unregistered operations
- **Control batched requests** – Limit query breadth and use cost analysis to prevent enumeration attacks through batching
- **Set timeouts** – Apply timeouts at HTTP server, router-to-subgraph, and individual operation levels


**Additional Best Practices**


- **Client identification** – Require clients to identify themselves so you can monitor schema usage. These are[configurable headers](https://www.apollographql.com/docs/graphos/routing/observability/router-telemetry-otel/enabling-telemetry/usage-guides/client-id-enforcement) in the Apollo Router.
- **Monitor with observability** – Use tracing, alerts, and OpenTelemetry integration to track performance and errors
- **Restrict write access** – Use graph API keys, personal API keys, SSO, member roles, and protected variants to control who can modify schemas


For comprehensive implementation guidance, see our[GraphOS Security documentation](https://www.apollographql.com/docs/graphos/platform/security/overview) ,[microservices security best practices](https://www.apollographql.com/blog/secure-your-graphql-microservices) , and the[Security section of the Supergraph Architecture Framework](https://www.apollographql.com/docs/graphos/resources/saf/security) .


## **Conclusion**


Apollo Federation’s security model is built on a fundamental principle: subgraphs are internal services that must only be accessed through the router. This isn’t a limitation – it’s the foundation that enables the router to enforce access control, demand control, and operation safelisting across your entire graph. As AI-powered tools make automated discovery and exploitation easier, maintaining this architectural boundary and implementing centralized governance through Apollo GraphOS isn’t just a best practice – it’s essential to protecting your API infrastructure.


Written by


David Walter


[Read more by David Walter](https://www.apollographql.com/blog/author/dwalter)
