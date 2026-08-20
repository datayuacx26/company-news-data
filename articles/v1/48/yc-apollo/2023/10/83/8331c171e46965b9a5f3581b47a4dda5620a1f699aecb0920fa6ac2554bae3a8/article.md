---
schema_version: "1.0.0"
document_id: "8331c171e46965b9a5f3581b47a4dda5620a1f699aecb0920fa6ac2554bae3a8"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphos-persisted-queries-are-generally-available"
published_at: "2023-10-11T17:18:15+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:43fd9c977a93e48a430ec6ede64deed7d0bc506a04af007f37bfea2d6a62b6b1"
---

# GraphOS Persisted Queries are generally available

Unbounded operations are generally considered a benefit of GraphQL, giving client developers unprecedented flexibility when experimenting and developing apps. However, while this flexibility is greatly appreciated in development environments, allowing it in production adds unnecessary surface area to GraphQL endpoints which generally serve only first-party clients.


Instead, your production graph should adhere to the principle of least privilege. If your first-party apps only require` QueryA` ,` QueryB` , and` QueryC` to operate properly, your graph should only allow` QueryA` ,` QueryB` , and` QueryC` . Any tolerance for ad hoc operations creates unnecessary exposure.


If you have a GraphQL monolith serving just a few clients, building a custom safelisting system to enforce this behavior is relatively straightforward. However, as the graph scales to serve dozens of clients and takes on a supergraph architecture, the complexity of maintaining such a system becomes prohibitive.


Apollo now offers a scalable solution for mitigating the risks of unbounded operations in supergraphs: **GraphOS Persisted Queries** . Persisted queries enable your platform team to limit the attack surface area of your supergraph by centrally registering trusted GraphQL operations and safelisting them in any instance of the Apollo Router.


## A GraphQL platform approach to API security


A supergraph is a self-service API platform designed to empower frontend and backend teams to consume and build services independently, but equally important is its ability to systematically centralize access control policy enforcement. By choosing to leverage the supergraph for policy enforcement, API platform teams can protect any number of backend services with layers of access control without heavy-handed manual review.


GraphOS Persisted Queries is one such layer that centralizes policies defining which *operations* should be allowed access to the supergraph. Rather than relying on platform teams to create and maintain this policy manually, GraphOS provides a workflow for[discovering and registering known operations](https://www.apollographql.com/docs/graphos/operations/persisted-queries#implementation-steps) that can be integrated directly into client CI/CD pipelines.


## Flexibility in development, discretion in production


In most cases, your production API only needs to execute a finite number of GraphQL operations from a set pool of stable, first-party applications. In this environment, safelisting limits the exposure of your graph by blocking any unknown operations.


However, in development environments, it’s important to allow developers the flexibility to experiment with unbounded operations. To do so, you can configure your router to simply[log any unregistered operations](https://www.apollographql.com/docs/graphos/operations/persisted-queries#security-levels) rather than block them. This is also useful when you’re getting started with Persisted Queries, and you haven’t yet registered all known operations from existing clients. However, it can be useful to pair this mode with some[operation limits](https://www.apollographql.com/docs/router/configuration/operation-limits/) that prevent any intentional or unintentional misuse of the graph.


## Expanding security options for your GraphQL platform


GraphOS Persisted Queries are one of the many ways you can[use GraphOS to improve the security posture of your GraphQL platform](https://www.apollographql.com/blog/graphql/security/enforcing-graphql-security-best-practices-with-graphos/) . Persisted Queries are generally available for GraphOS Enterprise today and coming soon for GraphOS Cloud. To get started, head over to the[GraphOS documentation](https://www.apollographql.com/docs/graphos/operations/persisted-queries/) .


For platform teams looking to further leverage their supergraph to improve API security, stay tuned for additional GraphOS security features and look out for more GraphQL security content at[GraphQL Summit](https://summit.graphql.com/event/c51538f6-4b76-44e3-871e-54180c77cad8/summary) .


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
