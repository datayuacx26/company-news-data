---
schema_version: "1.0.0"
document_id: "ae1e9b66d41d2fd47bab3c565978a5ea1a6adaf53ce8c355322e691cef929304"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphos-summer-2024-launch-recap"
published_at: "2024-06-17T09:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:ceff080bc90fcbf97d42964470e513cfb694363289815917c3828836c6dc61a3"
---

# GraphOS Summer 2024 Launch Recap

On, June 12, 2024, we hosted our[GraphOS summer launch event](https://www.apollographql.com/events/apollo-graphos-summer-update-operational-excellence-for-graphql-federation) , where we announced new features designed to improve the performance and operational excellence of GraphQL platforms. These new capabilities improve how API platform teams implement and optimize GraphQL federation, providing faster baseline performance and greater control over API request execution in a supergraph.


## GraphQL Federation: Usage on the rise


GraphQL adoption in the enterprise is at an inflection point, driven by the proliferation of federated GraphQL architectures – a pattern originally introduced by Apollo GraphQL in 2019 with the launch of Apollo Federation and[GraphOS](https://www.apollographql.com/graphos) . In a[recent report](https://www.apollographql.com/resources/gartner-when-to-use-graphql-to-accelerate-api-delivery) , Gartner® predicts that 30% of enterprises using GraphQL will utilize GraphQL federation by 2027, up from less than 5% in 2024. Apollo believes this trend suggests the number of enterprises using GraphQL federation as their API access layer will increase by 12x.


Apollo Federation is a specification for modularly and declaratively mapping existing application services into a unified GraphQL API access layer, known as a supergraph. This architecture empowers platform engineering teams to provide client applications with self-serve access to services through a single GraphQL endpoint, facilitating the rapid evolution of complex end-user experiences. By decoupling backend development from frontend complexity, teams can independently manage and contribute services to the supergraph via subgraphs, representing core business entities such as Product, User, or Cart.


In our summer launch, we focused on the Operational Excellence pillar of the[Supergraph Architecture Framework](https://saf.apollographql.com/) . The[Operational Excellence pillar](https://www.apollographql.com/docs/graphos/enterprise/supergraph-architecture-framework/operational-excellence) focuses on empowering subgraph and stewardship teams. A well-architected graph isn’t only technically excellent—it’s also operationally excellent. Processes and tools should be in place to ensure that the supergraph is easy to use, understand, and maintain. These principles help ensure that the graph is a successful tool for building new experiences quickly.


## Key Announcements


### Query Batching: GA


GraphOS now supports end-to-end (client-to-subgraph)[query batching](https://www.apollographql.com/docs/router/executing-operations/query-batching) , which is essential for ensuring consistency in component-based architecture where individual micro-frontends (MFE) make requests separately to fetch data relevant to them. By handling several queries in one request, this feature has the additional performance benefit of reduced network traffic and server load.


### Enhanced Telemetry and Insights: GA


The enhancements to[custom telemetry](https://www.apollographql.com/docs/router/configuration/telemetry/overview) in GraphOS Router provide platform teams with deeper visibility into the performance of their supergraphs. New support for custom[events](https://www.apollographql.com/docs/router/configuration/telemetry/instrumentation/events) gives you full customization over signalling when something of note happens in the Apollo Router’s request lifecycle.


New custom[instruments](https://www.apollographql.com/docs/router/configuration/telemetry/instrumentation/instruments) collects custom data and reports measurements to a metric backend.


Combined, enhanced telemetry allows client teams to investigate runtime errors and performance issues autonomously, reducing the debugging burden on core platform teams.


Additionally, GraphOS Studio now offers more[comprehensive checks and insights](https://www.apollographql.com/blog/new-graphql-observability-and-insights-in-graphos-studio) , giving teams greater control over supergraph operation validation to mitigate production errors.


### Entity Caching with Per-Subgraph TTL: Public Preview


Configuring response caching in a supergraph can be challenging due to the varied refresh rates of different fields.[Subgraph entity caching](https://www.apollographql.com/docs/router/configuration/entity-caching) addresses this by allowing platform teams to cache entities centrally while setting granular TTLs for each subgraph. For example, a Product entity can have a long TTL for its Name field and a short TTL for its Inventory field, so long as they are resolved by different subgraphs.


### Demand Control: Public Preview


Platform teams often need to protect their graphs against highly complex queries that can cause service disruption if left unchecked. Our new native cost calculation and limiting features make it easier for teams to observe the average cost of operations in their graph and implement cost controls centrally to avoid misuse of their GraphQL endpoints.


## Transforming API Platform Operations


These new features mark a significant step forward in how organizations can improve the operational excellence with which they manage federated GraphQL architectures. By providing advanced tools for caching, cost control, and query batching, Apollo GraphOS empowers platform teams to build, operate, and evolve supergraphs efficiently.


To learn more about these features and more, including schema visualization, context directives, and checks and insights, watch the[Summer Launch webinar](https://www.apollographql.com/events/apollo-graphos-summer-update-operational-excellence-for-graphql-federation?utm_campaign=2024-06-05_apollo-graphos-summer-update-operational-excellence-for-graphql-federation&utm_medium=blog) hosted by VP Product, Rob Brazier.


Written by


Andrew I. Carlson


[Read more by Andrew I. Carlson](https://www.apollographql.com/blog/author/andrewcarlson)
