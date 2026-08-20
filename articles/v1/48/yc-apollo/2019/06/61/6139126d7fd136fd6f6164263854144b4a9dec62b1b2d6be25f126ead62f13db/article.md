---
schema_version: "1.0.0"
document_id: "6139126d7fd136fd6f6164263854144b4a9dec62b1b2d6be25f126ead62f13db"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-caching-the-elephant-in-the-room"
published_at: "2019-06-25T22:43:06+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:909655871e5baff348f7b26fe67fb64c69ba541dc54c3dcb0d62bedcd04c8bad"
---

# GraphQL & Caching: The Elephant in the Room

This post is a preview of my[upcoming book on GraphQL](https://book.productionreadygraphql.com/) . It is an introduction to a caching & performance chapter, taking a look at the whole caching & GraphQL situation to remove confusion and show a more nuanced way of looking at it. If you’re interested in learning more about the book, I would really appreciate if you signed up for the[newsletter](https://book.productionreadygraphql.com/) .


[https://knowyourphrase.com/elephant-in-the-room](https://knowyourphrase.com/elephant-in-the-room)


If you’ve followed the discussions around whether GraphQL is a good idea or not, you might have heard things like *“GraphQL breaks caching”* , or *“GraphQL is not cacheable”* . If not, I guarantee you’ll be hearing similar things when you start displaying interest for building a GraphQL API. This is something I see some companies starting to use GraphQL being scared of and for which they don’t have a clear answer on. Before we dive into the world of caching and GraphQL, it might be a good idea to address these common concerns, and understand where they come from.


Comments like *“GraphQL breaks caching”* lack the nuance required to actually have a proper discussion about caching and GraphQL. What kind of caching? Client side? Server side? HTTP caching? Application side caching? To have a proper discussion and end with a better understanding of GraphQL’s limitations in terms of caching, we must be more nuanced.


## GraphQL breaks server-side caching?


This is a common thing to see thrown around when talking about GraphQL. The first thing to understand is that *“server-side caching”* is already vague. At this point, we know that GraphQL can actually be a thin layer over our existing servers, and that in no way GraphQL prevents us to cache on the server-side, sometimes referred as **Application Caching** . We will dive deeper into some concepts that can be applied at the application level later on in this chapter.


Next, if you’re familiar with popular GraphQL clients, you know one of their major feature is a denormalized cache that allows client side application to avoid refetching data they already possess, using it to optimistically update an UI, and to keep a consistent version of the world across components.


So if we can actually cache things both at the server and client layers, why are we hearing so much about GraphQL “breaking”, or making caching really hard? This is where it becomes more nuanced.


## HTTP Caching


While certain API styles like REST make great use the powerful HTTP semantics, GraphQL does not really, at least, by default. Since GraphQL is transport agnostic, most server implementations out there use HTTP as a “dumb pipe”, rather than using it to its full potential. This causes issues around certain things, like HTTP caching. There are multiple parts to HTTP caching that are important to understand before we go further.


First, there are many different cache entities that can be involved in HTTP caching. **Client side caches** , such as browser caches, use HTTP caching to avoid refetching data that is still fresh. **Gateway caches** are usually deployed along with a server, to avoid requests from always hitting servers if the information is still up to date at the cache level.


There are two concepts that are particularly important to understand when it comes HTTP caching: **freshness** and **validation** . Freshness lets the server transmit, through` Cache-Control` and` Expires` HTTP headers, the time for which a resource should be considered fresh. For example, a server returning this` Cache-Control` header is telling clients not to bother fetching this resource again until it has been at least one hour (3600 seconds):


` Cache-Control: max-age=3600`


This is especially great for data that doesn’t change often, such as browser assets. Whenever the age of the resource we fetched will be greater than this` max-age` , the client will emit a request instead of using the value in its cache. However, it doesn’t mean that it actually changed on the server. This is where **validation** comes in.


Validation is a way for clients to avoid refetching data when they’re not sure if the data is still fresh or not. There are two common HTTP headers to achieve this. The first one is` Last-Modified` . When an HTTP cache on the server has a value for` Last-Modified` , a client can send a` If-Modified-Since` to avoid downloading the data if the data hasn’t changed since last time it downloaded it.


The other common way of validating caches is using` ETag` .` ETags` are server generated identifiers for representations that change when the representation does. This lets the client track which “version” of the representation it has and avoid re-downloading a representation for which the` ETag` is the same as the one the client has.


Together, freshness and validation are a powerful way to control client and gateway caches.


To get a deeper understanding of HTTP caching, I highly recommend[this article, by the great Mark Nottingham](https://www.mnot.net/cache_docs) .


## GraphQL & HTTP Caching


When we dig deeper into the issues with GraphQL & Caching, we discover some of these issues are purely related **HTTP Caching** . It is an important distinction to make since **server-side caching** could mean just as well an HTTP Gateway cache, or application side caching on the server.


One of the first things that could influence how HTTP caching works with GraphQL is the HTTP verb used to send GraphQL queries. There is a lot of misinformation out there, that has lead to some people believing using` POST` on a GraphQL endpoint is the only way to make it work. HTTP caches will not caches` POST` requests, which means GraphQL is simply not cacheable at the HTTP level. However,` GET` is indeed a valid way to query a GraphQL server over HTTP. This means that caches could indeed cache GraphQL responses.


The only issue with` GET` is with the size of the query string. For example, almost each browser has different limits for these. If this becomes an issue,[persisted queries](https://blog.apollographql.com/persisted-graphql-queries-with-apollo-client-119fd7e6bba5) become very useful. We’ll cover those later on, but they let you store query strings on the server instead of the client, meaning a client could execute queries like this:


` GET /graphql/my_query`


There’s one last blocker. Since most GraphQL implementations don’t use much of HTTP semantics, most GraphQL servers will currently let you use` GET` along with a` mutation` operation. **This will not play well with caches.** One way to address this issue would be to design your server to reject mutations using` GET` , and require mutation operations to be run on` POST` only.


At this point, we’ve got all the basic elements to have HTTP caching and GraphQL working together. In fact, as we talked about earlier in the book, if we see GraphQL queries as way to dynamically create a server side client specific representation, each query is in fact, something that could be cached. Can we apply HTTP concepts to GraphQL queries? Let’s start with freshness. With freshness, what we would want is for a server to be able to tell a client how long the query can be considered as fresh, and when to request for this data again. The unfortunate thing here is that HTTP semantics operate on whole responses/representations, and doesn’t care or understand GraphQL queries, meaning we don’t have a way to do per-field freshness for example. Still, nothing could stop us from adding a freshness to a whole query: we could say that a GraphQL query’s max-age is equal to the field in the query with the lowest max-age.


Validation is similar. While we can’t use HTTP to revalidate only parts of the query, we could set` Last-Modified` to the value of the field with oldest` Last-Modified` value, and we could also generate an` ETag` based on a combination of all data loaded within the query.


While these are **possible** , they’re not ideal. Since GraphQL queries possibly span multiple entities that could change, and that they need to be represented as one representation on the GraphQL side, the amount of invalidations would be quite high. A single field being invalidated would invalidate the entire query, even if the rest of it was still fresh.


## Customization vs Optimization, Again


Remember the continuum of customizability we covered earlier in this book? Well it turns out this also affect how cacheable GraphQL really is. The invalidation issue we discussed above is not something very specific to GraphQL. In fact, it is specific to highly customizable APIs.


Take for example a typical HTTP endpoint for a web API:


` GET /user/1`


This particular endpoint accepts no particular query parameters and simply returns the user associated to this URI. As a public API especially, this endpoint is highly cacheable across all API clients. Now imagine a more customizable version of this endpoint:


```text
GET /user/1?partial=completeGET /user/1?partial=compact
```


This API uses a` partial` query parameter to change the level of detail of the response. An even more customizable API, just as we saw in the introduction could look like this:


```text
GET /user/1?fields=name,friends
```


The more versions of an HTTP endpoint we have, the more we dilute the cache. Meaning someone requesting` fields=name` only can’t actually use a cache, even though someone requested` fields=name,friends` . We’ve got the same issue happening with GraphQL, remove a field, change anything to a query in fact, and we lose the benefit of all queries that were cached with a superset or subset of the data.


As you see however, this is not something specific to GraphQL, and can be found in any API over HTTP that decides to opt for a more customizable API. With endpoint based APIs, the API designer is in charge of building the API and making these tradeoffs. By choosing GraphQL, we implicitly take the customizability road. Hopefully, that tradeoff was deliberate and the cache invalidation issues were worth it on the long run. **Instead of *“GraphQL is not cacheable”* , how about “ *Highly customizable APIs benefit less from HTTP caching”* ?**


If you’d like to hear more about customizability and HTTP caching, I recommend you read[this article by Phil Sturgeon](https://phil.tech/api/2017/08/13/a-happy-compromise-between-customization-and-cacheability/) .


## How Important is HTTP Caching to you?


There’s no doubt HTTP caching is a wonderful mechanism for data that doesn’t change often, and can be shared across multiple users, especially when talking about gateway caches. For authenticated, web APIs, the eternal debate is on how useful HTTP caching really is. It is a debate which I won’t solve here, but that is still interesting to discuss.


An interesting fact is that shared caches actually **should not** cache any request with an` Authorization` header. If your API is authenticated, the *“GraphQL breaks HTTP network/gateway/shared caches”* argument simply does not apply.


Private caches, such as browser caches and client side caches could still gain a lot from using HTTP caching. As we saw, it is **not out of question** with GraphQL, it is simply not as powerful as for highly optimized/one-size-fits-all APIs because of how often a query can be invalidated and how little can be shared.


Another thing to keep in mind is that lot of web APIs actually can’t have stale data for very long and freshness headers become less useful.


Validators such as` ETag` and` Last-Modified` usually require the server to retrieve all necessary data and run business logic to be computed. This usually is the major part of the work, savings being mainly on serialization, and bandwidth since no data needs to be transmitted. If bandwidth or serialization is an issue, again, nothing could stop you to implement` Etag` or` Last-Modified` generation for a GraphQL query.


GraphQL definitely made tradeoffs where it is much more suited to authenticated APIs and realtime data that changes often, versus serving long lived data as a public API. If your use case is the latter one, and it is the only thing your API does, considering using an API architecture that uses HTTP in a more meaningful way could be a better choice.


## Ways Forward


HTTP Caching could benefit GraphQL in good ways. The lack of **GraphQL over HTTP** specification is something that makes things a bit harder. The mutations over` GET` is an example of something that could be solved by such specification. However, there are many other ways to cache GraphQL, be it at the client level, the whole response level, the individual resolver level, etc.[In fact, certain vendors, like Apollo, are exploring caching semantics right into GraphQL](https://www.apollographql.com/docs/apollo-server/features/caching/#adding-cache-hints-statically-in-your-schema) . In this chapter, we will mainly cover GraphQL specific approaches since these are the most used tools at the moment and can be more powerful in the long run since they understand GraphQL semantics.


---


Thanks for reading 💚 I’m doubling down on the writing, really hope to finish the whole thing in the next few months. In the mean time, I’ll keep sharing snippets like this one when I can 🙇‍♂️ It’s cool to see vendors and products like[Apollo Server](https://www.apollographql.com/docs/apollo-server/features/caching) trying to address the issue, and I think the future might be found in a mix of GraphQL specific caching implementations, and making GraphQL over HTTP a bit more standardized. It would be great to add a simple HTTP caching middleware to GraphQL clients and get some caching by default.


[Marc](https://twitter.com/__xuorig__)


Written by


Marc-André Giroux


[Read more by Marc-André Giroux](https://www.apollographql.com/blog/author/xuorig)
