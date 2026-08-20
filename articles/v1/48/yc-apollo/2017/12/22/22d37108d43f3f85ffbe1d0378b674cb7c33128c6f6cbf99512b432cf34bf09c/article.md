---
schema_version: "1.0.0"
document_id: "22d37108d43f3f85ffbe1d0378b674cb7c33128c6f6cbf99512b432cf34bf09c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/improve-graphql-performance-with-automatic-persisted-queries"
published_at: "2017-12-06T22:58:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:54.348132+00:00"
content_hash: "sha256:581570f3c9f80401e358b8b89b990bbae0620789a0dfd18219c7f099472fd0fd"
---

# Improve GraphQL Performance with Automatic Persisted Queries

So let’s say you and your team are hooked on GraphQL. You love the flexible API and reductions in roundtrips to the backend, and you’ve been adding new features at a breakneck pace. Amidst all the commotion, you might not have noticed something important: as your app has grown in complexity, your GraphQL query strings have been growing with it *,* resulting in request sizes that are now becoming a bit of “a problem”. Don’t worry, you’re not alone. In some cases, we’ve seen GraphQL query sizes ranging well above 10 KB, *just for the query text* . This can be extra concerning since the uplink speed from the client is typically the most bandwidth-constrained part of the system. We continue to hear feedback in the GraphQL community that the size of individual queries is a major pain point, and in some cases a blocker to switching from REST. That’s why today we’re excited to introduce **Automatic Persisted Queries** — a tool to greatly improve network performance for GraphQL, now shipping with support in Apollo Server and Apollo Client. The concept is simple: by sending a query ID or hash instead of an entire GraphQL query string, bandwidth utilization can be reduced, thus speeding up loading times for end-users. Previously, you would have needed some fairly sophisticated build steps in place in order to make this work (more on that below). Now there’s a solution that requires zero build-time configuration and is supported by Apollo Client + Apollo Server — simply add the[Automatic Persisted Queries Link](https://github.com/apollographql/apollo-link-persisted-queries) to your client, upgrade to the latest version of[Apollo Server](https://www.apollographql.com/docs/apollo-server/) , and you’re ready to go.


## How it works


The mechanism is based on a[lightweight protocol extension](https://github.com/apollographql/apollo-link-persisted-queries#protocol) between Apollo Client and Apollo Server, which sits in front of your GraphQL server. It works as follows:


- When the client makes a query, it will optimistically send a short (64-byte) cryptographic hash instead of the full query text.
- **Optimized Path** : Apollo Server observes the queries as they pass through to your resolvers. If a request containing a persisted query hash is detected, Apollo Server will look it up to find a corresponding query in its registry. Upon finding a match, Apollo Server will expand the request with the full text of the query and pass it to the resolvers for execution.
- **New Query Path** : In the unlikely event that the query is not already in the cached registry (this only happens the very first time that Apollo Server sees a query), it will ask the client to resend the request using the full text of the query. At that point Apollo Server will store the query / hash mapping in the registry for all subsequent requests to benefit from.


**Automatic Persisted Queries: Optimized Path**


**Automatic Persisted Queries: New Query Path**


Inside Apollo Server, the query registry is stored in a user-configurable cache. This can either be an in-memory store on the same machine as your Apollo Server, or an external memcache store that you operate in your infrastructure. Read more here about[how to configure Apollo Server with APQs](https://www.apollographql.com/docs/apollo-server/features/apq) .


## A brief history of (build) time


This isn’t our first take on persisted queries —[earlier this year](https://dev-blog.apollodata.com/persisted-graphql-queries-with-apollo-client-119fd7e6bba5) we published an alternative implementation in[persist-graphql](https://github.com/apollographql/persistgraphql) . The persist-graphql solution (and others like it) is based on doing all of the work to generate the query map at *build-time* . Those solutions tend to rely on this kind of workflow:


1. Use static analysis to gather all of the queries from your client codebase (or codebases) and generate a mapping of queries to IDs.
2. Ensure that clients only send the IDs from this mapping instead of the raw queries.
3. Ship this mapping with your GraphQL server so it can translate the IDs back into queries.


While this “static” version of persisted queries solves some of the same performance problems, it makes different trade-offs that many organizations who absolutely need persisted queries are not able to make. Notably, it requires a build step that tightly couples your client code to your server in order to work. Any time a new query is added, it must be included in the server’s mapping well before it gets executed in production. This might not be so hard if your client and server are sharing a codebase, or even if they are owned by the same team. In practice, though, even in the best-case scenario this can mean a lot of added complexity. Managing all of this can certainly balloon from a small task to improve performance into a rigid process that slows everyone down. In some cases, it’s not even possible to know ahead of time what queries the server will see in the wild. For those providing their GraphQL APIs to 3rd-party developers, part of the appeal is letting app developers come up with new and interesting ways to use their data. The same applies to larger organizations where the client and server teams operate mostly independently. Unless you can force all of the developers who use your API to also run a client-side build step whenever they ship their apps, build-time persisted queries are a complete non-starter.


## Automatic for the people


We’ve developed this new take on persisted queries to answer these frustrations. At its core, GraphQL was designed to improvedecoupling of clients and servers by empowering application developers to work with data in a more flexible way. With **Automatic Persisted Queries** , we fully embrace this decoupling: since the query-to-ID mappings are stored by the server at runtime, *any* query that passes through can automatically be turned into a persisted query. Client teams can ship new queries and features at will, and all of the complicated build-time machinery is eliminated. And everyone can stop worrying about how big their queries are getting. Ready to give it a try? Check out[the documentation here](https://www.apollographql.com/docs/apollo-server/features/apq) to get your app set up and running with Automatic Persisted Queries. Help us run some experiments and let us know what you learn about the performance impacts on your system!


Written by


Tim Hingston


[Read more by Tim Hingston](https://www.apollographql.com/blog/author/timbotnik)
