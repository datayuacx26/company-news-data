---
schema_version: "1.0.0"
document_id: "6be8391707caa6846971a7b3b6a09d8702426dabffa2b6e0ce15024f9246852a"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/caching-graphql-results-in-your-cdn"
published_at: "2018-04-06T21:05:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:f7abde937bfb49e428840c9c573215213df80cd007d514ac85742f109826a4cd"
---

# Caching GraphQL results in your CDN

At first glance, it might seem like GraphQL is so different from REST and other HTTP API technologies that you have to rethink all of your infrastructure from scratch. But it turns out that it’s not hard to add GraphQL to your existing setup and start getting the benefits you want right away, without having to rebuild anything you already have.


1. You can keep your existing backends and business logic, since you can put GraphQL over REST APIs.
2. You can add GraphQL data loading to your existing React app, one component at a time.
3. You can easily integrate GraphQL tools into your existing application infrastructure, since they run anywhere your server code does.


In this case, we’ll talk about Content Delivery Networks — CDNs. At first, it might seem like GraphQL isn’t easy to use with your existing CDN setup like Fastly, CloudFlare, or Akamai, but it turns out you can get it running in just a few easy steps.


I’m writing this post because we just launched a new feature in Engine that enables all of these things to work together more easily than ever before, and I’m really excited to tell you about it! To get started, make sure you’ve[set up Apollo Engine](https://www.apollographql.com/docs/engine/setup-node.html) and head on over to the[Engine CDN integration docs](https://www.apollographql.com/docs/engine/cdn.html) .


If you’re looking to learn more about what CDNs do and what questions people usually have about using them with GraphQL, read on!


---


### How does a CDN help your API?


If your application has a lot of public data that doesn’t change very frequently, and it’s important for it to load quickly, you’ll probably benefit from using a CDN to cache your API results. This can be particularly important for media or content companies like news sites and blogs.


A CDN will store your API result close to the “edge” of the network — that is, close to the region the user is in — and deliver a cached result much faster than it would have required to do a full roundtrip to your actual server. As an added benefit, you get to save on server load since that query doesn’t actually hit your API.


Edge caching doesn’t help as much if your application has a lot of frequently-changing or private data, but it’s a must-have optimization for publishing or media applications with infrequently-changing, public data.


## Misconceptions: GraphQL and CDNs


One question we hear pretty frequently is whether this type of infrastructure can be used with a GraphQL API. On the surface, GraphQL might appear to make CDN caching harder, but it turns out that it’s not much more effort than with a REST API.


Let’s go through some common misconceptions, and see how they are easily mitigated.


### 1. GraphQL requests are too diverse


*Answer: In most apps, they aren’t more diverse than hand-coded REST endpoints.*


Because GraphQL is a much more flexible query language than the URLs you use with REST, it might seem that whole query results would be harder to cache because there are so many different queries that could be sent to the server. Indeed, it is possible that some apps will have completely dynamic queries, but it turns out that when following modern GraphQL best practices, most frontends have a very finite set of queries they send to your server.


Before GraphQL, people would often use a backend-for-frontend pattern that involved creating customized endpoints that matched the needs of a particular UI feature. This meant that you could end up with dozens or even hundreds of endpoints that served similar data in different ways, to optimize delivery for a particular part of the app.


With GraphQL, you get those same efficiency benefits without having to write and maintain those endpoints. So at the end of the day, even though you didn’t handcode a set of URLs, your application makes a similar number of distinct API calls. So caching the entire GraphQL result for certain views based on the query and variables turns out to be just as reasonable as caching the entire REST API result was in the past.


This is thanks to something that has been a best practice in GraphQL for a long time — writing[static queries in your UI code](https://dev-blog.apollodata.com/5-benefits-of-static-graphql-queries-b7fa90b0b69a) with Apollo Client or Relay Modern rather than using a tool that automatically generates unpredictable queries, like Lokka or Relay Classic. Issues with how those older clients generated and sent queries are a big part of why that misconception exists today.


### 2. GraphQL requests are too large, and use POST instead of GET


*Answer: You can easily switch to smaller requests and use GET by combining Apollo Link and Apollo Engine.*


A central principle of REST is that you use a URL to identify a piece of data, and use the` GET` verb in your HTTP requests to indicate that you’re doing a data read and not a write. That tells a CDN that it’s OK to cache that result, since it’s not expecting to modify something on the backend.


In contrast to that, historically, most GraphQL tools sent HTTP requests using POST, and instead of a URL they uses a complicated request body that contains a query and variables. As an added complication, in some browsers there’s a relatively small URL size limit that means you can’t fit the entire query and variables in the URL for the GET request.


But by combining Apollo Link, our modular network interface for the client, and Apollo Engine’s “Automatic Persisted Queries” feature, we can address both concerns at once. After[setting up Engine](https://www.apollographql.com/docs/engine/setup-node.html) , you can easily add` apollo-link-persisted-queries` to your client code:


```text
import { createPersistedQueryLink }
from "apollo-link-persisted-queries";
import { createHttpLink } from "apollo-link-http";
import { InMemoryCache } from "apollo-cache-inmemory";
import ApolloClient from "apollo-client";


ApolloLink.from([
createPersistedQueryLink({ useGETForHashedQueries: true }),
createHttpLink({ uri: "/graphql" })
]);


const client = new ApolloClient({
cache: new InMemoryCache(),
link: link
});
```


This will do two things for you:


1. Send queries over HTTP GET instead of POST, while still using POST for mutations
2. Use a shortened persisted query ID in the GET URL, so that the cache key for CDNs is shorter and you don’t hit URL size limits


This brings GraphQL much closer to the regular HTTP GET requests that CDNs are designed to handle.[Read more about Automatic Persisted Queries in Engine in the docs.](https://www.apollographql.com/docs/engine/auto-persisted-queries.html)


### 3. It’s harder to specify cache hints for a GraphQL query


*Answer: With Apollo Cache Control, you don’t have to specify hints for each query separately — you can do it based on GraphQL types and fields.*


With your REST API, you can simply return a` Cache-Control` header from a specific endpoint, and until you write a new endpoint it will remain the same. But with GraphQL, you will constantly be improving the queries on the frontend, adding and removing fields based on what different versions of the UI need. So how do you make sure that your cache control hints stay up to date with the shape of the query, even as the data included in the result change over time?


That’s exactly what[Apollo Cache Control](https://github.com/apollographql/apollo-cache-control) is designed to solve. This is a spec for how a GraphQL server should return cache hints on a per-field level. It comes with a reference implementation for JavaScript that shows how you could specify cache hints with different levels of specificity:


*For the entire schema:*


```text
app.use('/graphql', bodyParser.json(), graphqlExpress({
schema,
context: {},
tracing: true,  // Set a max age of 5 seconds for the whole schema
cacheControl: {
defaultMaxAge: 5,
},
}));
```


*For a GraphQL type or field:*


```text
type Post @cacheControl(maxAge: 240) {
id: Int!
title: String
author: Author
votes: Int @cacheControl(maxAge: 30)
readByCurrentUser: Boolean! @cacheControl(scope: PRIVATE)
}
```


*Or even for a single execution of a resolver:*


```text
const resolvers = {
Query: {
post: (_, { id }, _, { cacheControl }) => {
cacheControl.setCacheHint({ maxAge: 60 });
return find(posts, { id });
}
}
}
```


This is important because this allows the API to specify the expiration of different pieces of data, while maintaining the freedom of the frontend code to specify whatever queries it needs.


At the end of the day, Engine combines all of these hints into one convenient` Cache-Control` header that your CDN can understand.


*Note: If you’re not using a CDN, you can use cache control to power the caching feature of Apollo Engine.*[Read the caching docs here.](https://www.apollographql.com/docs/engine/caching.html)


## Putting it all together


So, if you have some GraphQL data that you think would benefit from CDN caching at the edge, it’s actually really simple to get everything working well. This is a great example of the interplay between several tools we’ve been working on for a while:


1. Automatic Persisted Queries with Apollo Link lets queries use GET while mutations still use POST
2. Apollo Cache Control lets you specify cache control information in a fine-grained, schema oriented way
3. Apollo Engine generates small query IDs you can use in those GET requests to limit the cache key size, and sets the Cache-Control header for the CDN


Then, when you put it all together, you can see those results getting cached in your favorite CDN service:


A screenshot of a demo app with the Fastly dashboard.


Read all of the details for how to set it up yourself[in the Engine CDN docs](https://www.apollographql.com/docs/engine/cdn.html) !


---


Last year at GraphQL Summit, I gave a talk about how the power of GraphQL will become clearer as more and more tools work together across the stack — in the client, the API gateway, the server, and more. It’s really exciting to see these cooperating techniques combine to tell an even more compelling and coherent story, and we can’t wait to see what’s coming next!


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
