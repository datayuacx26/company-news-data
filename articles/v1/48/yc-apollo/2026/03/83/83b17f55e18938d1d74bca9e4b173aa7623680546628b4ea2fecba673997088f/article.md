---
schema_version: "1.0.0"
document_id: "83b17f55e18938d1d74bca9e4b173aa7623680546628b4ea2fecba673997088f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/the-new-apollo-kotlin-normalized-cache"
published_at: "2026-03-09T01:00:13+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:53:30.609843+00:00"
content_hash: "sha256:ea73342e6d06cf815167ec3ca2a6e5698f9fbdd987fc4d4317bc71c8a2627015"
---

# The New Apollo Kotlin Normalized Cache

Today, we’re releasing[the new cache library](https://github.com/apollographql/apollo-kotlin-normalized-cache) for[Apollo Kotlin](https://github.com/apollographql/apollo-kotlin) , the GraphQL client library for Android and Kotlin.


Historically, caching has generated the most questions, tickets, and feedback, and we knew it was an area that deserved significant attention.


To do that efficiently, we decided early on to create a[new repository](https://github.com/apollographql/apollo-kotlin-normalized-cache) with its own independent name, artifacts, and versioning. This allowed us to iterate on this new version without disrupting the existing stable library.


After more than a year of development and invaluable community feedback (thank you!),[v1 is now available](https://github.com/apollographql/apollo-kotlin-normalized-cache/releases) , and we are excited to share it with you.


## What’s new?


Compared to the previous iteration, this new normalized cache brings:


### Pagination support


Caching paginated queries has always been a bit tricky, and required a lot of manual work to merge the results. With this new version, Relay-style pagination is supported out of the box, and the cache will take care of merging the results for you.


### Expiration (a.k.a. Time to live)


The new cache allows you to specify a maximum age (TTL) for your types and fields, and will automatically expire them when they are too old. By default, expired fields are treated as a cache miss, which results in a network request to fetch the fresh data.


### Partial results from the cache


Partial cache reads are now possible, similar to how GraphQL supports partial responses. This means that even if *some* fields from your query are missing from the cache, the available ones can still be returned and displayed.


### Garbage collection and trimming


A cache can grow indefinitely, and previously the only thing you could do is to clear it entirely. We now have APIs to:


- remove unused data from the cache to reduce its size,
- trim the cache to a maximum size by removing the oldest records.


### Key scope support


Specifying the cache keys for your types is vital for a normalized cache to work efficiently. The cache used to always prefix keys with the type name (e.g.` User:42` ). If your keys are unique across all types, this isn’t always necessary and adds overhead. This is now configurable.


### Improved performance


The SQL cache now stores normalized records in a more efficient way, resulting in better performance, and a smaller on-disk footprint.


### And more!


There are also many smaller improvements along with API polish, and simplifications.


## Try it out!


If you’re already using the normalized cache, follow the[migration guide](https://apollographql.github.io/apollo-kotlin-normalized-cache/migration-guide.html) .


Otherwise, hop over to the[documentation](https://apollographql.github.io/apollo-kotlin-normalized-cache/welcome.html) to get started.


Either way, let us know what you think – via[GitHub issues](https://github.com/apollographql/apollo-kotlin-normalized-cache/issues) , the[Apollo Community](https://discord.gg/graphos) , or the[Kotlin Slack](https://slack.kotl.in/) !


If you’re thinking about caching, GraphOS also supports[response caching](https://www.apollographql.com/docs/graphos/routing/performance/caching/response-caching/overview) at the router level.


Written by


Benoit Lubek


[Read more by Benoit Lubek](https://www.apollographql.com/blog/author/blubek)
