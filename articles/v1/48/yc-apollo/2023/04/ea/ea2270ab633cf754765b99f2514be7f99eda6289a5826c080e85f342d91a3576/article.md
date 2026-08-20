---
schema_version: "1.0.0"
document_id: "ea2270ab633cf754765b99f2514be7f99eda6289a5826c080e85f342d91a3576"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-changelog-april-3-2023"
published_at: "2023-04-03T17:10:07+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:cbee85cf2a45624f2c212aae3d6df81d5c3652953218e8929792f8642097a286"
---

# Apollo Changelog: April 3, 2023

## GraphQL Summit Call for Proposals – Last Week


This is the last week to[submit a talk](https://summit.graphql.com/event/c51538f6-4b76-44e3-871e-54180c77cad8/websitePage:ee8a1bc8-1286-4106-b34b-f28798e56f93) for this years GraphQL Summit! We will officially be closing down the submission form on 4/10 at 11:59 PM PST. If you have a talk that you are unsure of or want some help, we’re here to help! Jump into[our Discord server](https://discord.gg/graphos) and ping @admin – our Developer Advocates. We can start up a private thread with you and discuss your talk to help with the submission.


## New video! Testing out modularizing your schema using` rover dev`


Did you know you can quickly play around with any schema files (` .graphql` files) and run the Apollo Router locally with` rover dev` ? This is a great way to test out modularizing your schema, introduce entities and try out things like` @defer` !


## Apollo Router + Apollo Explorer with GraphOS


We introduced the` graph_ref` option to the` homepage` configuration of the Apollo Router which will use an Apollo Explorer version connected to GraphOS. Just add this to your routers config:


```text
homepage:
graph_ref: { graph_id }@{ variant }
```


This exposes the Apollo Studio Explorer URL to the end user on the default landing page:


## New Apollo Tutorial – GraphOS: Shipping your supergraph


The team just released[a new tutorial on shipping your supergraph in GraphOS](https://www.apollographql.com/tutorials/graphos-shipping-supergraph) . This tutorial includes using the Rover CLI to run schema checks, publish schema changes and launch our changes confidently. You’ll walk through an example of how to deprecate a field in your schema safely using helpful tools and metrics from GraphOS. The course is in beta and we’re looking for feedback. Jump into our[Discord server](https://discord.gg/graphos) and let us know what you think!


## Apollo Kotlin v3.8 Release 🎉


There is[a new release](https://github.com/apollographql/apollo-kotlin/releases/tag/v3.8.0) of Apollo Kotlin and it is jam packed with cool new things:


1. Multipart subscriptions support 🤯
2. 🧪 New Jetpack Compose artifacts in` apollo-compose-support` artifact
3. [New Gradle plugin: run codegen after gradle sync](https://github.com/apollographql/apollo-kotlin/pull/4796)
4. [Much more](https://github.com/apollographql/apollo-kotlin/releases/tag/v3.8.0)


## Rapid roundup


- [@apollo/client v3.7.11 released](https://github.com/apollographql/apollo-client/releases/tag/v3.7.11) which fixes a couple of tricky bugs in` useQuery` and` useLazyQuery` , making error message generation more reliable. It also improves WebSocket error handling!
- Last week[Danielle](https://twitter.com/danimman) wrote a[blog post](https://www.apollographql.com/blog/life-at-apollo/why-isnt-apollo-explorer-open-source/) about Apollo Explorer and her thoughts on how that project has developed over time. It’s a great read!
- [Stephen Spalding’s 2022 GraphQL Summit talk](https://www.youtube.com/watch?v=4JMgeRM-n7c) on sharing Netflix’s learnings on how to avoid GraphQL pitfalls was highlighted in[this weeks GraphQL weekly](https://www.graphqlweekly.com/issues/309#content) !


## Community spotlight


The GraphQL Working Group has the next[upcoming meeting on 4/6 at 10:30-12:00 PM PST](https://github.com/graphql/graphql-wg/blob/main/agendas/2023/04-Apr/06-wg-primary.md) . There is a block of time dedicated to field merging in incremental delivery ( *hint: they’re talking about` @stream` and` @defer`* ). This is an exciting area of the spec that is rapidly evolving 😍


[Matthew Wilson](https://dev.to/matthewwilson) wrote a great[blog post](https://dev.to/aws-builders/zero-to-serverless-car-insurance-part-1-cml) on tips for running GraphQL on AWS Lambda. The information is great for anyone interested in running[Apollo Server](https://www.apollographql.com/docs/apollo-server) on[AWS Lambda](https://github.com/apollo-server-integrations/apollo-server-integration-aws-lambda) 🙌


A big shoutout to our[Discord community](https://discord.gg/graphos) users and all of their great questions this last week. We’re seeing more engagement and we have more things planned that we’ll be dropping soon. We hope to see you there!


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
