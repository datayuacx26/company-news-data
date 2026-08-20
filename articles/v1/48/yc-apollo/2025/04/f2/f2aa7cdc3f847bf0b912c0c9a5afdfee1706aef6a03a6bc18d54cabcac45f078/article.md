---
schema_version: "1.0.0"
document_id: "f2aa7cdc3f847bf0b912c0c9a5afdfee1706aef6a03a6bc18d54cabcac45f078"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-graphos-free-plan-unlock-the-power-of-api-orchestration"
published_at: "2025-04-22T13:46:26+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:91d275cc11eee6a5311285fdeabb8fa5c934ee830046f72f77555ee3f7c7d5e3"
---

# Apollo GraphOS Free Plan: Unlock the Power of API Orchestration

Apollo’s GraphOS API orchestration platform now offers a forever-free plan. Whether you’re already using GraphQL or are just getting started, this free plan is designed for anyone learning, exploring, or prototyping with Apollo GraphQL. You can begin immediately without needing a credit card. In this article, you’ll learn:


- What the GraphOS API orchestration platform is
- What is included in the free plan (and its limitations)
- How to get started with your first GraphQL query using the free plan


## What is the GraphOS API orchestration platform?


The GraphOS API orchestration platform from Apollo GraphQL is a collection of services and tools that help you build, manage, and orchestrate APIs at any scale. Apollo Studio is the UI for the control plane and includes:


- **GraphOS Explorer IDE** for interactive query development
- **GraphOS Registry** for schema management and versioning
- **GraphOS Insights** for performance monitoring and analytics


Figure 1: GraphOS control and runtime planes


The runtime plane contains the GraphOS Router (Router) and Apollo Server. These components fulfill GraphQL query requests for both single-server graphs and federated supergraph deployments. For example, in a federated use case, clients send queries to the Router, which intelligently determines which back-end subgraphs contain the necessary data, orchestrates the data exchange, and returns precisely what the client requested.


GraphOS is a complete API orchestration solution that enables you to:


- Filter and transform data
- Execute operations synchronously and in parallel
- Manage authentication and authorization
- Integrate diverse data sources seamlessly


## What is the Apollo GraphOS free plan?


The free plan replaced the enterprise trial plan that was limited to 90 days. This free plan has no time restrictions, making it easy to get started with GraphQL and prototype your next project.


It also offers access to Apollo Connectors, which integrates REST API data into a graph without writing complex procedural code. The free plan includes the same functionalities as the enterprise plan, with the following limitations:


- Self-hosted routers are rate-limited to 60 requests per minute
- Organizations are limited to 3 team members
- Insights, checks, and traces for graphs of all types (not just those using GraphOS Router), with data retention limited to one day


Support is available through the[Apollo Community](https://community.apollographql.com/) .


The free plan is ideal for personal projects and exploring GraphOS, providing all the essentials to learn, experiment, and create at no cost. When your API orchestration needs expand, Apollo’s paid plans immediately remove the free plan’s rate limits, allowing you to scale your workload to any level of request volume. Depending on your paid plan, you may also use features such as:


- **Single Sign-On (SSO)** support
- **GraphOS Router extensibility** , such as external coprocessors
- **Advanced collaboration tools** , such as schema proposals or configurable linting
- Comprehensive **Audit History**


## Get started with your free plan


Anyone can create an organization on the free plan by:


1. Signing up for[GraphOS](https://studio.apollographql.com/signup?utm_campaign=Unlock%20the%20Power%20of%20API%20Orchestration&utm_medium=blog&utm_source=website) , or
2. Logging into Apollo Studio with your existing account and creating a new organization.


Figure 2: Apollo Studio showing free plan organization


Self-hosted routers can be hosted on the cloud provider of your choice. The Free plan limits requests to 60 per minute. Exceeding this limit will return an HTTP 503 (Service Unavailable) error, and you’ll receive upgrade instructions via email if this occurs.


**Important Note** : If you have an existing enterprise trial, you’ll need to create a new organization to migrate to the free plan, then[transfer](https://www.apollographql.com/docs/graphos/platform/graph-management/transfers) your existing GraphQL APIs and schemas to that new organization.


For more information, check out the[https://www.apollographql.com/pricing](https://www.apollographql.com/pricing) page and[Apollo GraphOS Free plan FAQs](https://support.apollographql.com/space/ETKB/1297809476/Free+plan+FAQs) .


Written by


Robert Walters


[Read more by Robert Walters](https://www.apollographql.com/blog/author/robertwalters)
