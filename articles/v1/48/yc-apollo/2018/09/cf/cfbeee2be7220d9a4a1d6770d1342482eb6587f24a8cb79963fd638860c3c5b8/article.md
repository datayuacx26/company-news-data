---
schema_version: "1.0.0"
document_id: "cfbeee2be7220d9a4a1d6770d1342482eb6587f24a8cb79963fd638860c3c5b8"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/batching-client-graphql-queries"
published_at: "2018-09-19T23:33:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:3e057cbe0aeb9d708195572ff9eb7c924ba33996f13d1dbc94694a517ad0cfbc"
---

# Batching Client GraphQL Queries

Modern apps are chatty—they require a lot of data, and thus make a lot of requests to underlying services to fulfill those needs. Layering GraphQL over your services solves this issue, since it encapsulates multiple requests into a single operation, avoiding the cost of multiple round trips.


While GraphQL encourages this style of data fetching, React encourages developers to split up data fetching logic to be colocated with the components that use it. This pattern is extremely useful, but can lead to many of the same issues of an app making too many network requests for a server to handle at scale. The idea of batching client operations can be applied to GraphQL clients to solve many of these issues, but sometimes batching can hurt more than help.


To batch, or not to batch, that is the question.


By the end of this post, you should be able to answer the following questions about batching client operations with Apollo:


- How does batching work?
- What are the tradeoffs with batching?
- Is batching necessary?
- Can batching be done manually?
- Can I fix automatic batching?


## How does batching work?


Batching is the process of taking a group of requests, combining them into one, and making a single request with the same data that all of the other queries would have made. This is usually done with a timing threshold. For example, with a threshold of 50ms, if a query is made from a component, instead of making the query immediately, the client waits 50ms. If any other queries are requested in that 50ms, all of those additional queries are requested at the same time, rather than separately.


In GraphQL apps, batching usually takes one of two forms. The first form takes all operations and combines them into a single operation using the alias feature in GraphQL. This approach is not recommended, however, since this removes the ease of tracking metrics on a per-operation basis and adds additional complexity to the client.


Instead, we recommend sending an array of operations to a GraphQL server, having the server recognize the request as an array of operations instead of a single one, and handle each operation separately. This method still requires only a single round-trip, while retaining the ability to track single operation performance. Apollo Client handles batching like this using` apollo-link-batch-http` . It’s also worth noting that this method requires the server to be able to receive an array of operations as well as single operations. Luckily,` apollo-server` supports this out of the box.


How Apollo batches operations with an array


For a deeper dive into how batching works with Apollo, check out[this post](https://blog.apollographql.com/query-batching-in-apollo-63acfd859862) introducing batching in an earlier version of Apollo Client. Even though some of the implementation details have changed, the concepts are still relevant today.


## What are the tradeoffs with batching?


Batching may sound like the perfect solution to some network performance issues on the client, but it’s far from perfect. Request batching is prone to slow loading times on the client. For example, if five requests from separate components are made to the GraphQL endpoint, and one of the five requests take a long time, the client won’t get *any* of the results back until *all* operations have been resolved. In other words, batched operations are always as slow as the slowest operation in the batch.


Since GraphQL is often used to aggregate data from multiple data sources, this can be problematic. If one of the underlying services is slower than the rest, the effects may be felt across the whole client app.


Additionally, batching makes it much harder to debug network traffic. In most scenarios, if a specific operation was taking a long time to resolve, it would be obvious. The component making the query would be stuck in a loading state, and the rest of the app would function normally. Additionally, when using browser debugging tools, slow network requests can usually be found easily. Batching queries makes both of these debugging methods more difficult.


## Is batching necessary?


Before looking at solutions to the issues that batching introduces, it’s important to question whether batching is even needed in the first place. Since enabling batching is as easy as replacing one` apollo-link` with another, it’s tempting to introduce batching at the onset of a project, without any real evidence that batching is needed. In fact, because of its drawbacks, we don’t recommend batching unless performance issues are still present after all of the following steps have been taken:


- [Use automatic persisted queries](https://www.apollographql.com/docs/guides/performance.html#automatic-persisted-queries) (APQ) to reduce the request body size.
- [Cache requests with a CDN](https://www.apollographql.com/docs/guides/performance.html#get) to prevent operations from ever reaching the GraphQL endpoint.
- Enable partial query caching with[Apollo Server](https://www.apollographql.com/docs/apollo-server/features/data-sources.html) to make operation resolution significantly quicker in most cases.
- If possible, use HTTP/2 on the server (with Node.js 10), which allows multiplexing of requests, effectively batching them.


## Can batching be done manually?


Yes! Batching can be done manually. Often, for the cases where batching may still be a necessity, inexpensive operations can be manually batched together to prevent unnecessary requests. In GraphQL, this is done by combining smaller queries into one larger one. For example, if there was a page with four content blocks on it, rather than having each block fetch its own data, a` container` could fetch the data and pass it to the components manually. This is conceptually similar to the first implementation of batching described in the first section.


This may sound counterintuitive to the patterns that have been established, like colocating queries with the components that use their response, but there are ways around this.


This isn’t suggesting to write one large GraphQL query at the container-level. Instead, write queries normally, next to the components that use them. When you’re ready to optimize a section of an app, convert those queries to[fragments](https://www.apollographql.com/docs/react/advanced/fragments.html) and export them from the component file. You can then import these fragments in the container, let the container make the single, large query, and pass the fragment results back to the children. Using container components in this way can even allow you to control loading and error states at the container-level, rather than in each component.


Take a look at this CodeSandbox for a full example of this in action:


Even manual batching has issues, though. Since manually batched operations are much larger, **their ability to take advantage of whole-query caching is reduced** . Whole-query cache TTLs are based on the field in an operation with the shortest TTL. Increasing the number of fields in an operation increases the chances that a field that can’t be cached for a long time is included, reducing the ability to cache the whole operation. For more on whole-query caching, and how these TTLs are calculated,[read this doc](https://www.apollographql.com/docs/engine/proxy/guides.html#cache-hints) .


## Can I fix automatic batching?


There is no silver bullet for batching. If batching is enabled, there is always the potential for portions of the batch to run slower, and thus hold up the remaining portions of the batch. Sometimes, however, the trouble of manually batching operations outweighs the benefits. Manually batching may be too complicated, or too large of a refactor to reasonably undertake.


Some of the issues around batching can be solved by manually *debatching* expensive operations–that is, allowing most operations to be batched like normal, but preventing batching for ones that are known to cause issues. Doing this requires a few steps:


1. Build components as usual, with colocated queries.
2. Identify the most expensive operations using a tool like[Apollo Engine](https://www.apollographql.com/engine) (these are not necessarily just the largest queries).
3. Mark expensive operations on the client using an operation’s` context` . You can set the context by specifying a prop on your` Query` component.
4. Use` <a href="https://www.apollographql.com/docs/link/composition.html#directional" target="_blank" rel="noreferrer noopener">split</a>` to switch between` apollo-link-http` or` apollo-link-batch-http` depending on the context of the operation.


Take a look at this CodeSandbox for a full example of this in action:


## Conclusion


Batching is a tricky topic. There are plenty of reasons to use some form of client request batching, but many times these solutions just cause more problems than they solve.


Hopefully, armed with this information, you can feel confident when making a decision about how to boost performance on the client.


---


I hope you’ll also[join us at the 3rd annual GraphQL Summit on Nov 7–8 in San Francisco](https://summit.graphql.com/) . With over 800 attendees expected, it’s the largest GraphQL developer event in the world. Tickets are selling fast, so[register today](https://www.eventbrite.com/e/graphql-summit-2018-tickets-46601841362) to reserve your spot!


Written by


Jake Dawkins


[Read more by Jake Dawkins](https://www.apollographql.com/blog/author/jake)
