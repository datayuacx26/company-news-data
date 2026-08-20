---
schema_version: "1.0.0"
document_id: "a52f669a9076b293a14c79fbb50b5342521a3581d50368d4f798fadaf276ce82"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-apollo-engine-insights-error-reporting-and-caching-for-graphql-6a55147f63fc"
published_at: "2017-10-24T23:38:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:54.348132+00:00"
content_hash: "sha256:d1f0c10f000ea6d24dcee5fd3ad14a01188d2c83f0fb2be422522efb16ba189f"
---

# Introducing Apollo Engine: insights, error reporting and caching for GraphQL

Today we’re excited to introduce[Apollo Engine](https://www.apollographql.com/engine/) , our new turnkey infrastructure that helps you take GraphQL services into production with confidence. The[successor](https://dev-blog.apollodata.com/apollo-engine-and-graphql-error-tracking-e7dd3ce8b99d) to Apollo Optics, Engine delivers essential capabilities like query caching, error tracking, and execution tracing on top of any spec-compliant GraphQL server including Apollo Server, GraphQL-Ruby, Sangria, and Absinthe. Adding Engine to your server is as easy as an \`npm install\` command.[Click here](https://www.apollographql.com/docs/engine/setup-node.html) to get started!


Performance latency, cache hit rates and error tracking for each GraphQL operation


## Unlock the full power of GraphQL


Engine implements the most important features you need to develop and run a production GraphQL API, including:


- **Integrated GraphQL query caching** : Engine caches pre-assembled query results at the edge of your GraphQL layer, minimizing page render times and workload on your backends.
- **Error tracking** : Engine tracks every error from your GraphQL server in the context it occurred, giving you a way to find and diagnose GraphQL service errors by field, by path, and by operation.
- **Query execution tracing:** Engine captures a precise trace of your GraphQL operations, including field-by-field details of resolver execution times, cache hit rates, and individual error events.
- **Schema analysis** : Engine gives you a complete understanding of exactly how — and how much — each field in your GraphQL schema is used.
- **API usage trends** : Engine shows you historical trends of all the data it captures, giving you a clear understanding of the impact on your API as your UI components, GraphQL queries, and user behavior change.


Each of these considerations is best addressed in separate infrastructure, rather than inside a GraphQL client or as part of a server process — and that’s why we architected Engine as a layer between the two. A thin proxy is deployed into your own cloud or datacenter, sitting in the request path right next to your GraphQL services. Most of Engine runs in our cloud and is centrally managed, monitored, and automatically scaled for you. We call this a “hybrid prem” architecture, and it has some big advantages:


- Low latency — because the proxy is next to your server, requests are handled in your own datacenter
- Highly available — your API uptime doesn’t depend on our services
- Security — your business data never leaves your premises.


Engine’s hybrid prem architecture


## Engine’s newest feature: GraphQL-aware caching


Engine’s hybrid prem architecture opens the door for performance optimization features that live in the query path. The first such capability we are shipping is an initial preview of GraphQL query caching. API caching is a standard best practice, both to reduce the load on your servers, and to accelerate API responses and decrease page render times. But because GraphQL requests are POSTed to a single endpoint, existing HTTP caching solutions don’t work well for GraphQL APIs. To bring caching to GraphQL, we developed[Apollo Cache Control](https://github.com/apollographql/apollo-cache-control) , an open standard that allows servers to specify exactly what parts of a response can be cached, and for how long. Apollo Engine implements Apollo Cache Control, caching whole query responses based on a computed cacheability of each new query type, without requiring any manual configuration. Engine also shows how cache policies impact each query type, making it easy to optimize your queries for cacheability, refine field-level cache policies, and pinpoint slow components on screen that could benefit from caching.


View per-field cache control for each query


We’ve added support for Apollo Cache Control in Apollo Server, and are looking forward to working with other GraphQL server authors as well.


This is just the start of what we believe can be delivered with GraphQL-aware caching. By having Engine take advantage of fine-grained cache control, different parts of a query response can be cached with different lifetimes and stitched together on demand. Stitching can also be used to augment publicly cached data with user-specific fields. Going beyond specifying cache lifetimes, Apollo Cache Control could be used to explicitly invalidate parts of your data, so both Engine and your client cache can be kept up-to-date automatically.


It’s easy to add cache control directives to your existing GraphQL server —[follow instructions on doing this](https://www.apollographql.com/docs/engine/caching.html) with Apollo Server.


## Free forever


We want everyone to be able to use Engine, so just like with Optics, the first million requests each month are completely free –– plenty for development and for many production applications. Beyond one million queries, Engine costs just $9 per each additional million queries, billed to the nearest penny. Engine is the successor to Optics, and we’ll work with existing Optics users to migrate them to Engine in the coming months. We’ll discontinue support for the Optics agent and shut down optics.apollodata.com in early 2018. Engine is the result of hundreds of customer conversations we’ve had over the past few months with companies taking GraphQL into production. This is just the beginning; we’ve already started exploring new capabilities that the Engine architecture makes possible, like rate limiting and schema stitching, and we look forward to building them together.


---


*To get started, visit our*[guide](https://www.apollographql.com/docs/engine/setup-node.html) *. If you’re already on Optics today, please review our*[upgrade instructions](https://www.apollographql.com/docs/engine/upgrade-optics.html) *.*


Written by


Rohit Bakhshi


[Read more by Rohit Bakhshi](https://www.apollographql.com/blog/author/rohit)
