---
schema_version: "1.0.0"
document_id: "fe8294465cabb32292b29a5838f0a1373ddf395d8ba3b760916d81b6be3480c2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/build-centralized-graphql-authorization-with-apollo-graphos"
published_at: "2023-09-05T13:45:09+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:9e6d78655c1d0ef37cdca48bddd91a89a0ead6b4b38d058089eb4a4961300d8e"
---

# Build centralized GraphQL authorization with Apollo GraphOS

*December 2023 update: GraphOS authorization enforcement is **now generally available** , including the` @requiresScopes` ,` @authenticated` , and **new**` @policy` directives.[Read the GA announcement](https://www.apollographql.com/blog/announcement/backend/centrally-enforce-policy-as-code-for-graphql-apis/) .*


---


Implementing a durable access control system for APIs is critical to prevent data breaches, monetary losses, and potential denial of services. GraphQL presents API owners with a unique challenge in this regard because it is designed for maximum flexibility.


In general, there are two choices for enforcing access control for GraphQL APIs, neither of which scale particularly well. You can write custom logic to centralize enforcement — which must be kept in lock-step coordination with changes to the graph — or you can distribute enforcement across multiple GraphQL endpoints — eliminating much of the flexibility GraphQL provides to app developers.


Apollo GraphOS and[supergraph architecture](https://www.apollographql.com/blog/announcement/backend/the-supergraph-a-new-way-to-think-about-graphql/) are eliminating this trade-off. Starting today, graph owners can enforce centralized authorization across any number of connected services in the graph.


[Authz enforcement](https://www.apollographql.com/docs/router/configuration/authorization/) is available **in preview today** for all customers on the GraphOS Enterprise plan running self-hosted instances of Apollo Router.


## Durable, graph-native authz


When you operate a supergraph with GraphOS, you can now use the` @requiresScopes` and` @authenticated`[directives](https://www.apollographql.com/docs/router/configuration/authorization/#authorization-directives) to define the authorization model for individual types and fields in each of your services declaratively. Rather than relying on custom logic or per-service access control, GraphOS uses these directives to compose a single federated authorization model. It then enforces that model in your[graph router](https://www.apollographql.com/docs/router/) at the edge of the API stack to optimize query execution and improve defense-in-depth.


### Limit access to certain scopes


The **` @requiresScopes`** directive is useful in cases where certain data or experiences should be available to a subset of users, but not all. It allows you to define granular access control by specifying an array of scopes that are required to access the type or field. If GraphOS receives a request that is missing the required scopes for certain fields, it will automatically filter out those fields before executing the operation. \[[Example use case](https://www.apollographql.com/docs/router/configuration/authorization/#example-requiresscopes-use-case) \]


### Require authentication


The **` @authenticated`** directive is useful when you need to limit access to unauthenticated users (or when you want to cut off unauthenticated access entirely). It provides binary access control, limiting marked types and fields to authenticated requests. When GraphOS receives an unauthenticated request, it filters out all fields marked with` @authenticated` before execution. \[[Example use case](https://www.apollographql.com/docs/router/configuration/authorization/#example-authenticated-use-case) \]


## Optimize query execution


The best way to combat DoS attacks is to enforce access control well before traffic ever reaches your mission-critical services. With an access control model defined in your schema, the GraphOS runtime plane acts as a central control point that does just that. In the simplest case, GraphOS will immediately terminate requests lacking the proper authorization to access any fields, preventing them from ever reaching your services. However, GraphOS can also optimize the execution of partially authenticated requests and deduplicate requests to your services.


### Eliminate unauthorized subqueries


Let’s say we’re building the profile page for a social media app that shows profile information like a name and profile image to all users, but only shows the posts associated with the profile to authenticated users. We have one service in our federated graph that handles user information — the Users API — and another that handles post information — the Posts API. We can restrict access to data resolved by the Posts API to authenticated users with` @authenticated` .


Now, when GraphOS receives the query used to populate the profile page, it will first check if the user has the proper claims to access fields resolved by the Posts API. Here’s the fun part – if the request doesn’t have the proper claims, GraphOS will eliminate the entire subquery to the Posts API, preventing that service from ever seeing the traffic.


### Deduplicate subqueries


GraphOS also reduces load on your services by accounting for authorization when[deduplicating simultaneous subqueries](https://www.apollographql.com/docs/router/configuration/traffic-shaping/#query-deduplication) . If your router is simultaneously processing two or more requests that after authz enforcement require identical subqueries, GraphOS will fulfill both requests using a single subquery.


## Getting started


Implementing centralized GraphQL authorization with GraphOS eliminates the need for maintaining custom gateway code or bespoke authorization models for every one of your services. On top of that, it optimizes the performance of your services by eliminating unnecessary load from unauthorized requests.


Starting today, all customers on the GraphOS Enterprise plan can start using` @requiresScopes` and` @authenticated` to enforce declarative, graph-native authorization in the GraphOS runtime plane. To get started, head over to the[GraphOS documentation](https://www.apollographql.com/docs/router/configuration/authorization/) .


If you aren’t a GraphOS Enterprise customer and you want to start building authz enforcement with GraphOS, you can[try it out for free](https://studio.apollographql.com/signup?type=enterprise-trial) for 28 days.


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
