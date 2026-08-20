---
schema_version: "1.0.0"
document_id: "4fac1f83a5687e11c543e2e33d2d42a0a85eb0926aa388f1cf7f83c911b0bd84"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-response-caching"
published_at: "2026-01-15T09:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:23:35.329584+00:00"
content_hash: "sha256:7e1c9b79d3d12a87046b4a2402981740ca46f12f47c85b400c198df53cc436d0"
---

# Introducing Response Caching: GraphQL‑aware performance for your graph

[Response caching](https://www.apollographql.com/docs/graphos/routing/performance/caching/response-caching/overview) is now available in the[GraphOS Router](https://www.apollographql.com/graphos-router) , bringing GraphQL‑aware caching that reduces load on your services and improves end‑to‑end latency across your[graph](https://www.apollographql.com/docs/graphos/resources/glossary?docs%5Bquery%5D=supergraph) . This release introduces a router-native response cache, a complete re-write and replacement for the previous “entity caching” capability. The new response cache is purpose-built for federated GraphQL workloads and offers a straightforward migration path from entity caching. While entity caching remains available in preview for now, it will be deprecated in a future release.


## What is response caching?


Response caching enables the router to cache origin responses and reuse them across queries, caching both root‑level query results and granular entity representations so multiple requests can share the same data without redundant subgraph calls. It uses Redis as the backing store and includes a cache debugger to help you inspect what’s stored during development.


At a glance, the router caches:


- **Root query fields** as complete units (the entire response for that root field).
- **Entity representations** independently per origin, so each origin’s contribution to an entity can be reused across different queries and users, when appropriate.


## Why GraphQL needs a different approach to caching


GraphQL responses often combine data with mixed freshness requirements and visibility rules—e.g., long‑lived catalog details, fast‑moving inventory counts, and personalized user context in one operation. Traditional full‑response caches force the shortest TTL across the whole payload and can’t safely share personalized results across users, leading to poor hit rates and duplication.


response caching addresses these challenges:


- **Mixed TTLs without compromise.** The router derives TTLs from HTTP Cache‑Control headers (which in turn come from your schema’s @cacheControl directives), applying the minimum TTL when a payload includes multiple entity representations.
- **Safe sharing for public data, privacy for personalized data.** Shared cache entries serve stable, non‑personalized data broadly, while private data remains isolated as needed.


**Less duplication, fewer subgraph calls.** Common entity fields are cached once and reused across different queries, users, and pages for higher hit rates and lower latency.


### How it works in your graph


Consider a retail schema where a Product’s descriptive fields come from one subgraph and its inventory comes from another. Response caching lets you cache product details with a longer TTL while keeping fast‑moving inventory short‑lived—or not cached at all—so the router can often serve most of the response from cache and only fetch the fresh parts when needed.


The result:


- **Lower service load** across subgraphs, thanks to shared entity representations and root‑field cache hits (reduce subgraph request volume by 60-90%).
- **Faster end‑to‑end latency** for clients, especially on repeated or high‑traffic entities and lists (improve P95 latency by 50% or more).
- **Handle traffic spikes** without scaling backend infrastructure


## Customer Story: Dow Jones drives performance and reduces load


We’ve seen the power of response caching in action with our early adopters. A great example is **Dow Jones** , which faced the common challenge of optimizing performance and managing load across its backend services while efficiently distributing load across backend services and supporting a sophisticated, high-traffic GraphQL layer.


By enabling response caching, Dow Jones achieved immediate, measurable results:


“We rolled out response caching to production and are observing a **20%-25% decrease in median response time** . The even more significant improvement is how much less traffic we send to our subgraphs. We’ve seen an **8-10x decrease** in some cases which has helped us reduce load on our backend services.”


– Nimit Barochia, Senior Staff Software Engineer, Dow Jones


The ability to cache shared entity representations, combined with the flexible TTL management inherent in the GraphQL-aware approach, allowed Dow Jones to drastically cut down on redundant subgraph calls, leading to a significant decrease in backend service load and a noticeable improvement in end-user latency.


## Availability and requirements


Response caching is a feature that requires GraphOS Router v2.10.0 or later. It’s available on Free, Developer, Standard, and Enterprise plans, with rate limits on Free and performance pricing on Developer and Standard; Developer and Standard plans require Router v2.10.0 or later. The cache uses Redis as the backing store.


## Get started


It’s easy to try response caching in your environment:


- Read the[docs overview](https://www.apollographql.com/docs/graphos/routing/performance/caching/response-caching/overview) to understand concepts, TTL behavior, cache structure, and development tooling like the cache debugger.
- Ensure your router version meets the requirement (v2.10.0+ for this preview) and configure Redis as the cache store.
- Use **@cacheControl** (or Cache‑Control response headers) in your schema to express freshness, and let the router handle minimal TTL selection across mixed payloads.
- Use **@cacheTag** directive in your subgraph schemas to add a tag on the data you would like to invalidate via this tag.
- Start with high‑value entities and root fields (found with the metrics provided by the Router), confirm hits with the cache debugger, and iterate on TTLs and invalidation for optimal performance.


Response caching requires a Redis instance and minimal router configuration. Here’s a basic setup:


```text
response_cache:
enabled: true
debug: true # This should not be enabled in production, it enables debugger
invalidation:
# address of the invalidation endpoint
# this should only be exposed to internal networks
listen: "127.0.0.1:3000"
path: "/invalidation"
subgraph:
all: # enabled for all subgraphs
enabled: true
ttl: 60s # Default TTL if no max-age is returned by the subgraphs
invalidation:
enabled: true
shared_key: ${env.INVALIDATION_SHARED_KEY}
redis:
urls: ["redis://localhost:6379"]
```


The router determines cache TTLs from Cache-Control HTTP headers returned by your subgraphs. If you’re using Apollo Server, you can control these headers using the[@cacheControl directive](https://www.apollographql.com/docs/apollo-server/performance/caching) in your schema:


```text
type Post @key(fields: "id") @cacheControl(maxAge: 3600) {
id: ID!
title: String!
content: String!
}
```


Apollo Server automatically translates these directives into appropriate Cache-Control headers ( cache-control: public, max-age=3600), and the router handles the rest. Apollo Server is not a requirement and if you don’t use it, you just set the right value in the Cache-Control response header.


### **Active Cache Invalidation**


While TTL-based caching works well for many scenarios, sometimes you know exactly when data changes and want to invalidate cached entries immediately. Response caching supports active invalidation through cache tags.


Tag your cached data using the @cacheTag directive (introduced in Federation v2.12):


```text
type Query {
topPosts: [Post!]! @cacheTag(format: "topPosts") @cacheControl(maxAge: 3600)
}


type Post @key(fields: "id")
@cacheControl(maxAge: 3600)
@cacheTag(format: "post-{$key.id}") {
id: ID!
title: String!
content: String!
}


```


This tags each cached product with a unique identifier like post-42. When a product changes, send an HTTP request to the router’s invalidation endpoint:


```text
curl --request POST \
--header "authorization: $INVALIDATION_SHARED_KEY" \
--header "content-type: application/json" \
--url http://localhost:4000/invalidation \
--data '[{"kind":"cache_tag","subgraphs":["posts"],"cache_tag":"post-42"}]'


```


The router immediately removes all cache entries matching that tag, ensuring clients receive fresh data on the next query. This approach mirrors CDN surrogate key systems but operates at the GraphQL entity level. There are also other builtin invalidation requests to invalidate per entity type or per subgraph.


### Observability


In this new version, we focused on making it easier for you to verify that your subgraphs and router are correctly configured. To achieve this, we introduced tooling and significantly improved the observability and debuggability of the caching feature.


Caching can be challenging because it’s hard to know whether it’s working or what impact it’s having. From the data alone, caching is nearly invisible, you simply receive the same data and there’s no direct way to confirm its effectiveness except by checking if the latency is lower. However, even faster responses don’t tell the full story. Response caching can result in partial hits, where, for a single query, you might have two cache hits from two subgraphs and one cache miss from another. This can give the impression that caching is working because latency improves, but if your goal is to cache all three requests, it’s impossible to know from latency alone whether everything is working as intended.


That’s why, in addition to introducing new metrics, traces, and logs, we’ve developed the[caching debugger](https://www.apollographql.com/docs/graphos/routing/performance/caching/response-caching/observability#cache-debugger) in Apollo Sandbox. This tool lets you inspect your responses and see exactly which parts came from the cache and which did not.


So for example if we take the example of products subgraph above and add another subgraph for users without any cache enabled. We will execute this query:


```text
query TopPosts {
topPosts {
author { # Coming from subgraph users
bio
}
title
content
}
}
```


If we keep caching enabled on the posts subgraph, the root field topPosts will be cached and not the user entity coming from subgraph users.


Here is what we can see from the caching debugger in Apollo Sandbox:


You can see the generated cache tags topPosts for the root field query, you can see it’s coming from cache and other data coming from subgraph users, the User entity is uncacheable and if we click on one entry we will have warnings explaining why it’s not cached.


In this screenshot we can also see the data sent to the subgraph in the Request tab, the data returned by subgraph and the parsed cache control header to be able to debug properly and know why it hasn’t been cached.


In addition, once you know it works as expected you would like to know if caching has an impact on your latency and subgraph load, to know that we have plenty of metrics that will help you to discover if you have a lot of cache hits or not and how you could improve the impact of caching on your infrastructure.


This is the overall graph request duration, so from client to the router, we can see at the moment we enabled caching we started to have lower latency.


And looking at these metrics we can see we have less load on the posts subgraph.


A lot of other useful metrics have been added to know the number of cache hits we have per subgraph and also per type if needed. You can also have metrics to help you to know before enabling caching which subgraphs and types can highly benefit from caching to improve your caching strategy. Check the[documentation](https://www.apollographql.com/docs/graphos/routing/performance/caching/response-caching/observability) to know more.


## What’s new vs. earlier entity caching announcements


You may have seen us talk about “entity caching” in earlier product updates; that work paved the way for today’s response caching design and nomenclature, integrating granular entity‑level reuse with router‑native caching semantics and invalidation flows. Here is what’s new:


- **Cache debugger** : See exactly what’s being cached during development with detailed cache key inspection
- **Redis cluster support** : Scale your cache with Redis cluster deployments and benefit from read replicas for improved performance and availability
- **Comprehensive metrics** : Monitor cache performance with detailed Redis-specific metrics including connection health, command execution, and latency
- **Granular Redis timeout** settings (fetch/insert/invalidate)
- **Better controls** over private data stored in the cache


## What’s next ?


We’re excited to hear about what you’re building with response caching and are eager for your feedback on how we can make the caching experience even better. To learn more, check out[our docs](https://www.apollographql.com/docs/graphos/routing/performance/caching/response-caching/overview) and start optimizing your caching strategy today. We also invite you to join the discussion and share your thoughts in our[dedicated community topic](https://community.apollographql.com/t/now-ga-response-caching-for-faster-more-efficient-supergraphs/9592) .


Written by


Benjamin Coenen


[Read more by Benjamin Coenen](https://www.apollographql.com/blog/author/bcoenen)
