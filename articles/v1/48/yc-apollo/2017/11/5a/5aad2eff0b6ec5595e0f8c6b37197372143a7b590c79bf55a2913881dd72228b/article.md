---
schema_version: "1.0.0"
document_id: "5aad2eff0b6ec5595e0f8c6b37197372143a7b590c79bf55a2913881dd72228b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/the-graphql-stack-how-everything-fits-together-35f8bf34f841"
published_at: "2017-11-07T23:17:24+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:24.699163+00:00"
content_hash: "sha256:20f39232e4db37fe8b21462b400c2e16ba147911abb0a66702ec9ca75af22045"
---

# The GraphQL stack: How everything fits together

It’s been over 2 years since GraphQL was released as an open source technology by Facebook. Since then, the community has grown exponentially, and there are now thousands of companies using GraphQL in production. At this October’s GraphQL Summit 2017, I had the privilege of giving the opening keynote on the second day. You can watch the full talk on YouTube, or read this post to get a quick overview.


First, I’ll take a look at what GraphQL is today, then examine how its main benefits might evolve in the near future. In particular we’ll go over three examples of full-stack GraphQL integration: Caching, performance tracing, and schema stitching. Let’s get into it!


---


## What makes GraphQL special?


There are 3 main factors that make GraphQL stand out from all other API technologies:


1. GraphQL has a well-specified query language, which is a great way to describe **data requirements** , and a well-defined schema, which exposes **API capabilities** . It’s the only mainstream technology that specifies both sides of the equation, and all of its benefits stem from the interplay of these two concepts.
2. GraphQL helps you decouple API providers from consumers. In an endpoint-based API like REST, the shape of returned data is determined by the server. In GraphQL, the shape of the result lives with the UI code that uses it, which turns out to be much more natural. This allows you to focus on **separation of concerns, not technologies.**
3. Since a GraphQL query is attached to the code that uses it, you can consider that query to be a **unit of data fetching** . GraphQL knows all of the data requirements for a UI component up front, enabling new types of server functionality. For example, batching and caching underlying API calls within a single query, which represents the data needed for a single part of your UI becomes easy with GraphQL.


Now, let’s take a look at three aspects of data fetching that people frequently ask about, and how GraphQL improves each of them by taking advantage of the properties above.


Note that while a lot of the functionality I’m going to talk about below is something you can do today, some of it is aspirational for the future. If this stuff is as exciting for you as it is for me, scroll to the bottom to get involved.


## 1. Caching across requests


One of the first things people always ask about is — how do I do cross-request caching with my GraphQL API? There are some issues that come up when trying to apply regular HTTP caching to GraphQL:


- HTTP caching often doesn’t support POST requests or long cache keys
- Greater diversity of requests could mean fewer cache hits
- GraphQL is transport independent, so HTTP caching doesn’t always work


However, GraphQL also brings many new opportunities:


- The possibility to declare cache control information alongside your schema and resolvers, where you access your backend
- Automatic fine-grained cache control from the schema, rather than having to think about hints for every request


How can we make caching work well with GraphQL, and how can we take advantage of these new opportunities?


### Where should caching actually happen?


First, we have to decide where the caching functionality should live. One initial intuition could be that caching logic should be inside the GraphQL server itself. Unfortunately, simple tools like DataLoader don’t work well across multiple GraphQL requests, and putting caching functionality in our server code runs the risk of making our implementation very complicated. So we should put it somewhere else.


It turns out that, just like in REST, it makes sense to do caching on both sides of the API layer:


1. Cache entire responses outside of the GraphQL API, in an infrastructure layer.
2. Cache underlying fetches to databases and microservices below the GraphQL server.


For the second part, your existing caching infrastructure works just fine. For the first, we need a layer that lives outside your API and is able to do things like caching in a GraphQL-aware way. Essentially, this architecture enables you to pull complexity outside the GraphQL server:


Move complexity into a new layer in between the client and server.


I call this component a GraphQL gateway. On the Apollo team, we think this new gateway layer is really important, and everyone will need one as part of their GraphQL infrastructure.


**That’s why, during the week of GraphQL Summit this year, we**[launched Apollo Engine](https://dev-blog.apollodata.com/introducing-apollo-engine-insights-error-reporting-and-caching-for-graphql-6a55147f63fc) **, the first ever GraphQL gateway.**


### A GraphQL response extension for cache control


As I mentioned in the introduction, one of the main benefits of GraphQL is that there’s a huge ecosystem of tools, which all work by leveraging GraphQL queries and schemas. I think functionality like caching should work the same way. That’s why we’re introducing[Apollo Cache Control](https://github.com/apollographql/apollo-cache-control) , which uses a feature built into the GraphQL spec called *extensions* to include cache control information right in the response.


With our[JavaScript reference implementation](https://github.com/apollographql/apollo-cache-control-js) , it’s easy to add cache control hints right in your schema:


Cache control hints on your schema with apollo-cache-control-js


I’m really excited about how this new cache control spec builds on the main strengths of GraphQL. It enables you to specify information about your data in a fine-grained way, and takes advantage of GraphQL execution to send the relevant cache control hints back to the consumer. And it does so in a totally language- and transport-independent way.


Since I presented this talk at GraphQL Summit,[Oleg Ilyenko](https://medium.com/u/93c50e4b5ebe?source=post_page-----35f8bf34f841----------------------) already posted about[a working version of cache control for Sangria](https://twitter.com/easyangel/status/925037659482976257) , the Scala GraphQL implementation he maintains.


### Caching with a gateway


Now that we can return cache control hints in the GraphQL server, we have a clear way to do caching in the gateway. Each piece of the stack plays its part:


Caching is a collaboration between all of the parts of the stack.


One cool thing to note is that most people already have a cache in their GraphQL stack: Libraries like Apollo Client and Relay cache your data inside the frontend. In future versions of Apollo Client, cache control information from the response will be used to automatically expire old data from the frontend. So, just like in other parts of GraphQL, the server describes its capabilities, the client specifies its data requirements, and everything works together nicely.


Now, let’s look at another example of GraphQL functionality that spans across the stack.


## 2. Tracing


With GraphQL, frontend developers have the capability to work with data in a much more fine-grained way than with endpoint-based systems. They can ask for exactly what they need, and skip fields they aren’t going to use. This creates an opportunity to surface detailed performance information and make it actionable in a way that’s never been possible before.


Don’t settle for an opaque total query time — GraphQL enables you to get detailed timings on a per-field level.


You could say that GraphQL is the first API technology with fine grained insights built in. And that’s not because of a specific tool — GraphQL is legitimately the first time it’s been possible for a frontend developer to get field-by-field execution timings, and then modify their query to work around issues.


### Tracing across the stack


It turns out that with tracing, just like with caching, coordination across the whole stack is useful.


Each part has its role in providing tracing data and making it actionable.


The server can provide information as part of the result, just like it provides cache hints, and the gateway can extract and aggregate that information. Once again, the gateway component is handling complex functionality that you don’t want to worry about inside your server process.


In this case, the primary role of the client is connecting queries with UI components. This is critical so that you can associate API layer performance with its impact on the frontend. For the first time, you can directly relate the performance of a backend fetch to the UI components it will affect on the page.


### GraphQL tracing extension


Much like caching, the above can be achieved in a server-agnostic way by leveraging GraphQL’s response extension functionality. The[Apollo Tracing](https://github.com/apollographql/apollo-tracing) specification, which already has implementations in[Node](https://github.com/graphql/graphql-js) ,[Ruby](https://github.com/rmosolgo/graphql-ruby) ,[Scala](https://github.com/sangria-graphql/sangria) ,[Java](https://github.com/graphql-java/graphql-java) , and[Elixir](https://github.com/absinthe-graphql/absinthe) , defines a way for GraphQL servers to return timing data for resolvers in a standardized way that any tool can consume.


Imagine a world where all of your GraphQL tools have access to performance data:


Shared abstractions allow tools to use information such as tracing data.


With Apollo Tracing, you can get performance data in GraphiQL, in your editor, or anywhere else.


So far, we’ve been investigating the interaction between one client and one server. For our last example, let’s take a look at how GraphQL can enable us to modularize our architecture.


## 3. Schema stitching


One of the best parts of GraphQL is having access to all of your data in one place. However, until recently, that has come with a cost: You needed to implement your whole GraphQL schema as one codebase to be able to query it all in one request. What if you could have a modular architecture, but at the same time retain the benefits of having a single universal GraphQL API?


Schema stitching is a simple concept: GraphQL makes it easy to combine multiple APIs into one, so you can implement different parts of your schema as independent services. These services can be deployed separately, written in different languages, or maybe even owned by different organizations.


Here’s[an example](https://launchpad.graphql.com/130rr3r49) :


Combining data from the GraphQL Summit ticketing system and a weather API in one query:[https://launchpad.graphql.com/130rr3r49](https://launchpad.graphql.com/130rr3r49)


In the screenshot above you can see how one query on a stitched API can combine two independent queries against different services, in a way that’s totally invisible to the client. With this approach you can combine GraphQL schemas like Lego bricks.


We’ve got a working implementation of this you can try today, as part of the Apollo graphql-tools library.[Read more in the docs.](https://www.apollographql.com/docs/graphql-tools/schema-stitching.html)


### Stitching in a gateway


The schema stitching concept also works well across the whole stack. We think the new gateway layer will be a really great place to do stitching in the long term, empowering you to build your schemas using whatever technologies you want, such as[Node.js](https://www.howtographql.com/graphql-js/1-getting-started/) ,[Graphcool](https://www.graph.cool/) , or[Neo4j](https://neo4j.com/developer/graphql/) .


It turns out, stitching is relevant in every part of the stack.


The client can join in on the fun too! Just like you can load data from multiple backends with one query, you can combine data sources on the client. The new client-side state management capabilities in the recently released[Apollo Client 2.0](https://dev-blog.apollodata.com/apollo-client-2-0-5c8d0affcec7) enable you to load data from client-side state and any number of backends in one query.


## Conclusion


If there’s one thing I hope you’ve gained from reading this post or watching the talk, it’s that even though GraphQL tooling today is already great, there’s so much more potential for the future. We’ve just scratched the surface of what the abstractions and capabilities of GraphQL can provide.


I’d like to finish this off with a todo list of just the concepts above:


There’s a lot of work to be done to integrate these new capabilities, especially in the area of developer tools and editors.


There’s a lot to be done to unlock the full potential of GraphQL. On the Apollo team, we’re working on this as hard as we can, but no one person, team, or organization can do it on their own. To reach the future, we’re all going to need to work together and collaboratively build out all of these solutions.


Wherever we look, one thing is clear: GraphQL has already been a transformative technology for thousands of companies, and it’s just the beginning! I can’t wait to see what it’s going to be like to build apps in the next 2, 5, and 10 years, because it will be incredible.


---


## Get involved


If you’re as pumped about the potential of GraphQL as we are at Apollo, consider getting involved in the community. We’ve put together a[helpful page](https://www.apollographql.com/docs/community/) to get you started.


Other talks from GraphQL Summit coming soon! Follow[@graphqlsummit](https://twitter.com/graphqlsummit) on Twitter or subscribe to[Apollo’s Youtube channel](http://youtube.com/apollographql) to get the latest.


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
