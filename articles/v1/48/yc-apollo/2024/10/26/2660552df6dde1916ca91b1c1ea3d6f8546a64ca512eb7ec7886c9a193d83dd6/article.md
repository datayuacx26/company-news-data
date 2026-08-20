---
schema_version: "1.0.0"
document_id: "2660552df6dde1916ca91b1c1ea3d6f8546a64ca512eb7ec7886c9a193d83dd6"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-connectors-for-rest-apis"
published_at: "2024-10-09T08:31:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:817d44386a8aed25dfbbc40eab262913540a46bff01fc78560b458534b94fb2a"
---

# Apollo Connectors: Blazing-Fast Integration with REST APIs

Apollo Connectors make it easier than ever to create a GraphQL API leveraging non-GraphQL services. Developers can use a declarative syntax, right in their schema files, to connect GraphQL fields to HTTP APIs. GraphOS Router uses that information to talk *directly* to the API, no subgraph needed.


## Reducing complexity


Before today, when developers wanted to add a REST service to their federated GraphQL API, they had to take several steps:


1. Identify a programming language and a Federation-compatible GraphQL framework
2. Deploy a new subgraph service, including monitoring, auto-scaling, and load-balancing, which will sit in between the router and the REST API
3. For each change to the REST API:


1. Design the subgraph schema for the underlying REST data
2. Write bindings to the REST API in the chosen programming language
3. Write resolvers that leverage those bindings
4. Deploy the subgraph service
5. Compose and publish the schema to update the router


Apollo Connectors eliminate the need for the intermediate service, saving developer time and infrastructure cost. With connectors, each iteration is reduced to:


1. Design the subgraph schema for the underlying REST data
2. Compose and publish the schema to update the router


The router then speaks directly to the REST APIs:


## Delighting developers


Apollo Connectors are designed with a singular goal in mind: make it as easy as possible for developers to build high-quality GraphQL APIs. Every aspect was built from the ground up to achieve this goal, starting with the declarative syntax:


```text
type Query {
post(id: ID!): Post
@connect(
source: "rest_api_v1"
http: { GET: "/posts/{$args.id}" }
selection: """
id
title
body
author: { id: userId }
"""
)
}
```


Data sources are connected right in your schema, so there’s no need to hop back and forth between multiple files. The selection format, for transforming the data, is designed to feel like the GraphQL query language to reduce context switching. It’s also designed to provide as much feedback as possible early in the development lifecycle, especially when combined with our[Visual Studio Code extension](http://go.apollo.dev/connectors/guides#visual-studio-code) . No more catching typos after deploy:


Apollo’s Visual Studio Code extension underlines errors in your Connectors definition


Our focus on developers extends through the testing phase, with powerful new debugging tools built into[Apollo Sandbox](https://www.apollographql.com/docs/graphos/explorer/sandbox/) :


And, of course, you get the production-ready features you’ve come to expect from GraphOS Router, like in-depth observability, built on the OpenTelemetry standard:


Apollo Studio’s insights page


## Get connected


Apollo is on a journey to make developing, operating, and consuming APIs as delightful as possible. GraphQL is a developer-friendly abstraction over all the moving pieces of modern applications. Apollo Connectors make the process of tying those pieces together into a cohesive, well-crafted schema, better than ever. You can[try out connectors today](https://go.apollo.dev/connectors) with your existing Enterprise plan or by getting[started with GraphOS](https://studio.apollographql.com/signup?referrer=blog) for free. Send us your feedback and ask questions in our[forums](https://community.apollographql.com/tag/connectors) or[Discord server](https://discord.gg/graphos) . We can’t wait to see what you create!


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
