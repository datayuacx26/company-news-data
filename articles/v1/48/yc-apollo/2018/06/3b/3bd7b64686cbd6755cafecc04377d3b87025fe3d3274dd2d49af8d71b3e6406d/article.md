---
schema_version: "1.0.0"
document_id: "3bd7b64686cbd6755cafecc04377d3b87025fe3d3274dd2d49af8d71b3e6406d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/automatic-persisted-queries-and-cdn-caching-with-apollo-server-2-0"
published_at: "2018-06-28T17:50:01+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:162efc7cd2f680524ffa0b3bf61761c04a2aee7d51c0fe385d2f65670742d8ce"
---

# Automatic Persisted Queries and CDN caching with Apollo Server 2.0

A common challenge that developers experience as they build products is how quickly their apps grow in complexity. For GraphQL services, your request sizes and query strings fatten over time, which in turn leads to extra weight transferred over the network. We solved this challenge by shipping **Automatic Persisted Queries** — a tool to that improves GraphQL network performance by reducing request size.


In Apollo Server 1.x, Automatic Persisted Queries and CDN caching depended on a closed-source Go component called the “Engine Proxy”. But with[Apollo Server 2.0](https://www.apollographql.com/docs/apollo-server/v2/whats-new.html) , our goal is that it’s the only tool you need to implement a production-ready GraphQL API. So how could we take care of this issue?


As part of Apollo Server 2.0, we’re open sourcing all of the features of the Engine proxy!


Apollo Server 2.0 now has Automatic Persisted Queries (APQ) and CDN caching built right in, to greatly enhance your network performance. In the rest of the post, I’ll show you how to use them.


Performance will be a first-class citizen in your GraphQL services with Automatic Persisted Queries and CDN caching.


## The cost of GraphQL queries


The size of individual queries in your app can be a bottleneck for smooth performance. The larger the queries, the more time it takes to send them over the network.


We’ve seen GraphQL query sizes in production ranging well above 10 KB, *just for the query text* . This is can be a significant overhead when compared with a simple REST URL of 50–100 characters. When paired with the fact that the uplink speed from the client is typically the most bandwidth-constrained part of the chain, large queries can become bottlenecks for client performance.


The slower a user’s connection is, the more costly those extra bytes become, and the longer it takes for the client to transmit its request. This is exactly what **Automatic Persisted Queries** solves.


## What are Automatic Persisted Queries?


With **Automatic Persisted Queries** , the client has the ability to ask the server if it needs the full query by sharing a small, 64-byte hashed representation of it, and only sending the full query if the server hasn’t seen that request before.


### How it works


Apollo Server checks if a request coming from the client contains a persisted query hash. If a hash is detected, Apollo Server looks up its registry for the corresponding query. Upon finding a match, Apollo Server expands the request with the full text of the query and executes it.


Optimal Performance Path


On the other hand, if no query is found for a hash, the server will notify the client to resend the request with the full query so that it can store the query / hash mapping in the registry for subsequent requests to profit from.


New Performance Path


### How to configure the cache store


By default, Apollo Server is configured to use an in-memory cache to store the query hashes. While this works great for many projects, highly-trafficked deployments or deployments where the processes are short-lived (like Amazon Lambda or Google Cloud Functions) will benefit from using a shared cache store, such as Memcached or Redis. These stores can be configured via the Apollo Server constructor:


```text
const { MemcachedCache } = require('apollo-server-memcached');
const { ApolloServe } = require('apollo-server');


const server = new ApolloServer({
...
persistedQueries: {
cache: new MemcachedCache(
['memcached-server-1', 'memcached-server-2', 'memcached-server-3'],
{ retries: 10, retry: 10000 }, // Options
),
},
});
```


### Client Setup


Finally, you need to turn on APQ in your client to start sending hashes. You can set it up in your client app with Apollo Link, like so:


```text
import { createPersistedQueryLink } from "apollo-link-persisted-queries";
import { createHttpLink } from "apollo-link-http";
import { InMemoryCache } from "apollo-cache-inmemory";
import ApolloClient from "apollo-client";


const link = createPersistedQueryLink().concat(createHttpLink({ uri: "/graphql" }));


const client = new ApolloClient({
cache: new InMemoryCache(),
link: link,
});
```


Ready to give it a try? Check out[the documentation here](https://www.apollographql.com/docs/guides/performance.html#automatic-persisted-queries) to get your app set up and running with Automatic Persisted Queries.


## Bring data closer to clients with CDN caching!


Another important optimization that can be really helpful for commonly-executed queries is CDN caching. Placing your server behind a CDN allows you to deliver data faster to the clients your users interact with.


Most GraphQL requests are large POST requests. Therefore, improving network performance with CDN integration will involve you following these steps:


1. [Set up a CDN and configure it to send requests to Apollo Server.](https://www.apollographql.com/docs/guides/performance.html#setup-cdn)
2. [Add cache hints to your schema so that Apollo Server knows which fields and types are cacheable and for how long.](https://www.apollographql.com/docs/guides/performance.html#cache-hints)


```text
type Dog @cacheControl(maxAge: 120) {
id: String!
breed: String!
displayImage: String @cacheControl(maxAge: 240)
images: [Image]
subbreeds: [String]
}
```


For example, the schema above indicates that all fields that return a` Dog` type should be cached for 120 seconds, and that the` displayImage` field itself should be cached for 240 seconds.


[3. Finally, enable automatic persisted queries.](https://www.apollographql.com/docs/guides/performance.html#enable-apq)


Apollo Server makes it straightforward to use CDNs with GraphQL queries to cache full responses while still executing more dynamic queries as usual.


## One More thing!


Over the past few months, we’ve worked very hard in making Apollo Server 2.0 your go-to API server in your GraphQL toolbox. And to help you get over your fear of missing out, we’re[constantly updating the docs](https://www.apollographql.com/docs/apollo-server/v2/whats-new.html) to give you a healthy pathway in learning to consume[Data Sources, File uploads, metrics handling, subscriptions and a plethora of features.](https://www.apollographql.com/docs/apollo-server/v2/)


Furthermore, feel free to check out our[new best practices guides about topics like schema design, versioning, access control, and more.](https://www.apollographql.com/docs/)


---


Finally, I hope you’ll also[join us at the 3rd annual GraphQL Summit on Nov 7–8 in San Francisco](https://summit.graphql.com/) . With over 800 attendees expected, it’s the largest GraphQL developer event in the world. Super early bird tickets are selling fast, so[register today](https://www.eventbrite.com/e/graphql-summit-2018-tickets-46601841362) to reserve your spot!


Written by


Prosper Otemuyiwa


[Read more by Prosper Otemuyiwa](https://www.apollographql.com/blog/author/prosper)
