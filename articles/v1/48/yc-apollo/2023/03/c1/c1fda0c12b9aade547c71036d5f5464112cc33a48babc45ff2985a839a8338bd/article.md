---
schema_version: "1.0.0"
document_id: "c1fda0c12b9aade547c71036d5f5464112cc33a48babc45ff2985a839a8338bd"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/supercharge-your-graphql-stack-with-graphos-cloud"
published_at: "2023-03-06T10:56:18+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:5f8ba035cf8307cdc2a511ec31e44a442c8dd6ae3ddb305e430e6cb9d0ddcc06"
---

# Supercharge your GraphQL stack with GraphOS Cloud

You’ve heard it a million times: *everything you need, and nothing more, all in a single query* . It’s the core promise of GraphQL, but it presents a challenge. To truly get everything in a single query, you need to put everything you have ** into a single graph.


At Apollo, we’ve spent the better part of three years designing a GraphQL architecture to address that challenge — we call it[supergraph architecture](https://www.apollographql.com/blog/announcement/backend/the-supergraph-a-new-way-to-think-about-graphql/) . Its main component is a graph router, which sits in front of your GraphQL servers and has a query planner to execute queries across multiple connected graphs. Just over a year ago, we released Apollo Router, our graph router binary, which is now used by Fortune 500 companies to run billions of operations each month for mission-critical applications.


But Apollo Router enables much more than just scale. It enables faster, more reliable, and more intelligent query execution for any GraphQL API by layering in support for directives like` @defer` and entity types. We want to bring these benefits and more to every developer in the world that uses GraphQL, not just those in a massive company.


That’s why today, we’re excited to announce general availability of **Apollo GraphOS Cloud** — a cloud-hosted platform with instant-on access to blazing-fast graph routing for any GraphQL API and integrated tools for rapid query development, collaboration, and schema design. Anyone with a GraphQL API can[start using GraphOS Cloud](https://studio.apollographql.com/signup) today with 10 million free queries per month our Serverless plan. Serverless is a great way for developers and teams to evaluate GraphOS or even GraphQL. From startups to proofs-of-concept for your enterprise, Serverless is an easy way to try all that Apollo GraphOS has to offer.


## Faster apps with hosted graph routing


At the heart of GraphOS Cloud is an edge graph router (built on Apollo Router) that enhances your existing GraphQL API with the latest runtime features of GraphOS. The router in GraphOS Cloud is so much more than just a GraphQL API gateway. It’s a runtime that is fully aware of the structure of data available to your connected GraphQL server, and it can manipulate how queries are executed to improve performance.


### ` @defer` for any server


With GraphOS Cloud, you can use` @defer` with any existing GraphQL server to improve time-to-interactive. When writing a query, you can simply add the` @defer` directive to slower fields, and GraphOS will automatically return those fields incrementally.


### Subscriptions, caching, and more coming soon


` @defer` is just the first of many features coming to GraphOS Cloud that will enhance performance for your GraphQL API. Support for subscriptions, response caching, and more are coming up soon on the roadmap!


## Develop, save, and test queries


GraphOS Cloud automatically registers the schema from your GraphQL API and integrates your hosted router with Explorer — our GraphQL IDE with IntelliSense. With Explorer, you can create queries for your graph with just a click, save queries to reuse later, and even write scripts that chain scripts together for automated testing workflows! Try it out on our[demo graph](https://studio.apollographql.com/public/apollo-retail-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgCccEADAEYQAsdAtAIwBMcbLjMdrABwUBLOvEZM4jAOwA2AGYKQAGnDR4yNJlwEAoviwAnAJ7EyIAMwBWOh34YKLDBArXeM6wpaDGlmSwygnLcgmwYgnAcNCAAvkA&referrer=operation_collections&variant=prod) .


## Scale and collaborate with ease


Most graphs start out small, but they can be a headache to scale across teams when the time comes. GraphOS Cloud has built-in support for Apollo Federation, so you can easily add to your graph modularly with subgraphs whenever you need to. It also automatically keeps your hosted router up-to-date with the latest version of Apollo Router, so you never need to worry about new version rollout.


GraphOS Cloud also integrates your router with a delivery pipeline to prevent breaking changes when collaborating on a graph with multiple subgraphs and client apps. Whenever GraphOS detects a change to one of your schemas, it will automatically recompose a supergraph schema for your router, check it against historic operations from registered client apps, and update your router with the new supergraph schema.


## Get started with GraphOS Cloud for free


Anyone with a GraphQL API can[start using GraphOS Cloud for free](https://studio.apollographql.com/signup?referrer=blog) today on our Serverless plan. On the Serverless plan, you can run up to 10 million operations per month through GraphOS Cloud for free! If 10 million isn’t enough, you can pay as you go for additional operations.


## Help us build the roadmap for GraphOS Cloud


We’re actively building the next generation of GraphOS Cloud to improve governance and throughput for production graphs. We would love to hear feedback, feature requests, and questions you have. Reach out atcloud@apollographql.com .


Written by


Jack Sellwood


[Read more by Jack Sellwood](https://www.apollographql.com/blog/author/jacksellwood)
