---
schema_version: "1.0.0"
document_id: "94d1efe9eed34d3b6864771123a5e000c7f6b5a0ba82eee12cf0ad9f4a6219dc"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/centrally-enforce-policy-as-code-for-graphql-apis"
published_at: "2023-12-12T14:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:b02b3c2768663d55a5f2557173cb1c93a1fb9aeabefe497e07e4a1b66a42ae5f"
---

# Centrally enforce policy as code for GraphQL APIs

Although many in the industry consider GraphQL a replacement for existing API technologies, it is at its most powerful when used as a central orchestration layer between apps and existing services. This layer, implemented as a federated graph (or[supergraph](https://www.apollographql.com/blog/announcement/backend/the-supergraph-a-new-way-to-think-about-graphql/) , for short), acts as a self-service API platform that accelerates development velocity for both frontend and backend teams. However, speed of delivery can’t come at the expense of a strong authorization model that protects personal identifiable information, financial data, and other sensitive information.


[Apollo GraphOS](https://www.apollographql.com/graphos) provides GraphQL-native workflows to implement access control declaratively across an entire supergraph. Rather than relying only on distributed enforcement or fragile custom gateway logic to secure the graph, teams using GraphOS can enforce policies at the type or field level in their router by decorating schemas with the` @requiresScopes` ,` @authenticated` , and new` @policy` directives. These directives provide the freedom to integrate with any authorization system, including policy engines like[Casbin](https://casbin.org/) or[Open Policy Agent](https://www.openpolicyagent.org/) (OPA). By proactively enforcing policies as code, API platform teams can ensure compliance at all times without sacrificing developer velocity.


These directives are generally available for all GraphOS Enterprise customers starting today.


## Durable, auditable authorization for supergraphs


Supergraphs expose a unified, versionless GraphQL endpoint for all connected services. While this offers a host of benefits for frontend development, it makes it tricky to centralize authorization with traditional API gateways designed to enforce policies at the endpoint level. These gateways are unaware of the shape of the supergraph, so policies need to be mapped to individual types and fields with custom logic that must be kept in lockstep coordination with any schema changes.


An authorization model that is instead integrated directly into a supergraph’s schema is far more durable and can still be centrally audited and enforced in the router.` @authenticated` and` @requiresScopes` ,[which we launched in preview](https://www.apollographql.com/blog/announcement/backend/build-centralized-graphql-authorization-with-apollo-graphos/) earlier this year, allow you to define access control policies for types and fields directly in the schema, restricting access to only authenticated requests or requests with certain scopes, respectively. Subgraph developers simply add the directives to the parts of the schema they want to enforce.


For example, let’s say you’re building a simple social media app. The app should allow unauthenticated users to view a post’s title, author, and content, but only authenticated users to see the number of views a post has received, and only certain users with required permissions to view email addresses. The relevant part of your schema may look something like this:


```text
type Query {
me: User @authenticated
post(id: ID!): Post
}


type User {
id: ID!
username: String
email: String @requiresScopes(scopes: [["read:email"]])
posts: [Post!]!
}


type Post {
id: ID!
author: User!
title: String!
content: String!
views: Int @authenticated
}
```


However, many organizations may require more complex policy evaluation and often leverage external tools like OPA or Casbin. For these more complex scenarios, GraphOS now supports a third authorization directive:` @policy` .


## Offload policy definition and resolution with @policy


The new` @policy` directive allows you to integrate your predefined policies from policy engines like OPA and Casbin into your supergraph schema and enforce them in your router. For example, let’s say in your example social media app, you only want users to have access to their own personal information and credit card information. Validating this requires more than the scopes of an access token, so you can use` @policy` to offload policy resolution for the relevant parts of your schema:


```text
type Query {
me: User @authenticated @policy(policies: [["read_profile"]])
post(id: ID!): Post
}


type User {
id: ID!
username: String
email: String @requiresScopes(scopes: [["read:email"]])
posts: [Post!]!
credit_card: String @policy(policies: [["read_credit_card"]])
}


type Post {
id: ID!
author: User!
title: String!
content: String!
views: Int @authenticated
}
```


The` @policy` directive allows you to use your supergraph as a central enforcement point for any services you’ve connected to your graph without refactoring the authorization model that you’ve already defined.


` @policy` loosely couples policy enforcement in your GraphOS router and the policy definition in your existing policy engine, giving you the flexibility to evolve how your application resolves policies without altering your GraphQL schema or infrastructure. For example, if your application needs to perform complex authorization validation, such as inspecting specific values in headers,` @policy` can be used to validate in a coprocessor or Rhai script.


## Get started


Schema-driven authorization with` @authenticated` ,` @requiresScopes` , and` @policy` is generally available for all GraphOS Enterprise customers today. To start centralizing authorization enforcement with GraphOS, head over to the[Authorization in the Apollo Router documentation](https://www.apollographql.com/docs/router/configuration/authorization) .


If you’re new to GraphOS, you can[try out GraphOS Enterprise](https://studio.apollographql.com/signup?type=enterprise-trial&_gl=1*1czaoty*_ga*NjU4MjMwOTQ0LjE2MzIxNTUyNDc.*_ga_0BGG5V2W2K*MTcwMTk3NjAwMy4xMDU4LjEuMTcwMTk3NjEwNy4wLjAuMA..) free for 28 days.


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
