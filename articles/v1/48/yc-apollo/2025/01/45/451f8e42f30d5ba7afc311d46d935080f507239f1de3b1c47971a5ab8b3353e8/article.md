---
schema_version: "1.0.0"
document_id: "451f8e42f30d5ba7afc311d46d935080f507239f1de3b1c47971a5ab8b3353e8"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/migrate-apollo-federation-1-0-supergraphs-to-apollo-federation-2-0"
published_at: "2025-01-21T16:34:29+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:aa508542a7f9816099f75f0177dbab9a909d06e9f866118711af90b708d6f389"
---

# Migrate Apollo Federation 1.0 Supergraphs to Apollo Federation 2.0

We’re announcing an important update: starting with the 1.60 release of[Apollo GraphOS Router](https://www.apollographql.com/graphos-router) and Apollo Router Core coming in the next few weeks,[Apollo Federation](https://www.apollographql.com/federation) 1.0 supergraphs will no longer be supported. This update accompanies the introduction of a[new native query planner](https://www.apollographql.com/docs/graphos/routing/query-planning/native-query-planner) in these Apollo Router versions, marking a significant advancement in our technology.[Upgrading to Apollo Federation 2.0](https://www.apollographql.com/docs/graphos/reference/migration/to-federation-version-2) is essential to leverage these improvements fully, and as Federation and Apollo Router are mission-critical infrastructure for your API workloads, we are here to guide you through every step of this transition to confidently operating your GraphQL platform.


**Understanding the importance of upgrading**


The next Apollo Router versions, 1.60 and beyond, include a new native query planner with significant performance enhancements. Our extensive testing of over 620 million operations showed a median 10x improvement in query planning time, a 2.9x reduction in CPU utilization, and a 2.2x reduction in memory consumption. Designed specifically for Federation 2.0 supergraphs, this new query planner simplifies maintenance and enhances security by eliminating dependencies, including the need for a JavaScript execution engine within the Apollo Router process.


**Timeline and planning**


Federation 1.0 supergraphs have been marked as deprecated in our[GraphOS Studio](https://www.apollographql.com/graphos/studio) UI and users have not been able to create them since Q3 2023. With the upcoming new Apollo Router versions, support for Federation 1.0 supergraphs will end across all Apollo products, including Apollo Gateway. We strongly recommend you upgrade to Apollo Router and Federation 2.0 to ensure continued compatibility with our latest innovations.


If you are still operating your GraphQL deployment without having yet migrated to Federation 2.0, upgrading to the new query planner in Apollo Router 1.60 and beyond will introduce a breaking change.


Migration to Federation 2.0 is simple and focuses on the composition layer, without requiring changes to existing subgraphs in most cases. You only need to compose a supergraph with Federation 2.0 and typically your subgraphs can continue using Federation 1.0 until they need 2.0 features. Our[migration guide](https://www.apollographql.com/docs/graphos/reference/migration/to-federation-version-2) details how you can update just your Federation composition step to compile a compatible supergraph.


**Available resources and ongoing support**


We recognize that transitioning from Federation 1.0 to Federation 2.0 is a significant step for some of our users. This decision reflects our commitment to eliminating the complexities of outdated technology and ensuring our offerings meet the evolving needs of modern API infrastructures. We are dedicated to transparent communication and improving our deprecation processes to help maintain the robustness, efficiency, and security of your systems.


Apollo values your partnership and trust as we continue to innovate. We are here to support you through this transition to keep your API infrastructure powerful and future-proof. For additional support, please visit our[Apollo Community](https://community.apollographql.com/t/migrate-apollo-federation-1-0-supergraphs-to-apollo-federation-2-0/8125) or, for GraphOS users, contact our Technical Support team atsupport@apollographql.com .


Written by


Josh Lambert


[Read more by Josh Lambert](https://www.apollographql.com/blog/author/jlambert)
