---
schema_version: "1.0.0"
document_id: "ffc5fdb2766341f81d8842c08c11ef34915458496bce15001cd8ce4d549e39f0"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-engine-and-graphql-error-tracking-e7dd3ce8b99d"
published_at: "2017-10-11T23:47:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:1de813f1dac7741360858c369d5236cd2ce63b09ef8349ee61947f69b6cef0d7"
---

# Apollo Engine and GraphQL error tracking

Today we’re excited to release a technical preview of the next version of Optics, called Apollo Engine. Engine lets you visualize, monitor and scale your GraphQL services, and is built on a new architecture that supports more languages and environments.


A complete log of a GraphQL server’s errors.


## New feature: Tracking errors in GraphQL


GraphQL’s flexibility brings some big advantages, like allowing clients to query just the exact data they need instead of depending on a set of rigid REST endpoints. It also creates a need for new tools like[Optics](http://apollodata.com/optics) that help us understand the behavior and performance of a GraphQL API. Errors are a great example. Unlike an REST API call that completely succeeds or fails, a GraphQL server that encounters one or more errors while executing a query can return a partial result with some fields missing, along with a list of the thrown errors. All the information about errors[is contained in the response](http://facebook.github.io/graphql/October2016/#sec-Errors-and-Non-Nullability)%20(http:/facebook.github.io/graphql/October2016/#sec-Errors-and-Non-NullabilityAll) , so without a GraphQL-aware tool, it can be difficult to recognize a misbehaving server or diagnose the underlying problem.


A GraphQL response with multiple errors


Here are the kinds of problems we wanted to address:


- GraphQL operations can throw **multiple errors** , so we wanted an accurate and complete log that captures each error in the context it was thrown.
- Because resolver errors may depend on the particular way a query was executed, we needed a way to zero in on **errors by field, by path, or by operation** .
- Errors can affect query performance(including fields that didn’t throw an error), so all traces in Optics **highlight failed resolvers** and include their execution timing data.
- To help zero in on **changes and trends** in server errors, Engine charts error rates alongside GraphQL query volume.
- To catch cases where something failed in the GraphQL server or its execution layer, we added **support for “500”-style errors** that occur outside of resolver code.


GraphQL’s error model also offers some interesting opportunities, like a more straightforward way to build UIs that gracefully degrade when a non-essential microservice is unavailable. We’re really excited to build tools like these across the stack that help everyone in the community take advantage of the new options made possible by GraphQL.


## A new architecture and Apollo Tracing


Engine is built on a new architecture that uses an embedded GraphQL proxy instead of a per-language agent package. The most immediate benefit is support for more languages and environments. Engine uses[Apollo Tracing](https://github.com/apollographql/apollo-tracing) , a GraphQL extension for exposing trace data, so it works with any spec-compliant GraphQL server that includes tracing support. The[list of servers that support Apollo Tracing](https://github.com/apollographql/apollo-tracing#supported-graphql-servers) today includes Node, Ruby, Scala, Java, and Elixir, with more being developed by the community. This approach has other advantages as well. The Engine proxy isn’t affected by a misbehaving server, so it accurately tracks errors even in cases where the GraphQL server crashes, or where a bug corrupts the server’s internal state. The new architecture also gives us a path forward to collecting more complete timing data for GraphQL operations, including time spent outside the resolver tree. Finally, keeping Engine’s trace sampling and analysis code out of the GraphQL server’s main execution thread improves request latency and server performance. This is just the beginning. We’ve already started exploring new features that the Engine architecture makes possible, like GraphQL query caching (take another look at the screenshot above!) and request rate limiting.


## Try it out today


To get started, visit our[getting started guide](http://engine-docs.apollographql.com/setup-node.html) . If you’re already on Optics today, please review our[upgrade instructions](https://www.apollographql.com/docs/engine/upgrade-optics.html) . The Engine preview is completely free to use today — we’ll announce our pricing in the coming weeks.


We’re excited to hear what you think of it! Join us on[Apollo Slack](http://apollodata.com/slack) in the **#engine** channel. Or, meet the engineers working on Apollo Engine in person at[GraphQL Summit](http://summit.graphql.com/) , October 25–26 in San Francisco (register with code` APOLLOENGINE` for 25 % off).


Written by


Matt DeBergalis


[Read more by Matt DeBergalis](https://www.apollographql.com/blog/author/matt)
