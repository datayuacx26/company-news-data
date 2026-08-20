---
schema_version: "1.0.0"
document_id: "48c9a3df87be1b6d954f339b547cf00a0211c56f162fc70d41a99b7443bd110b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/9-lessons-from-a-year-of-apollo-federation"
published_at: "2020-11-06T11:23:47+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:1090297a87da2db9549e140bccb722f272cc56a9b4e9bf8b39289b549bfccab1"
---

# 9 Lessons From a Year of Apollo Federation

At StockX, we’ve been running a federated GraphQL implementation for over a year now. Like any distributed system, the rewards it comes with also comes with challenges to overcome. Overall, we’ve had a positive experience serving our APIs with GraphQL and Apollo Federation.


In this post, I’d like to highlight some of the most important lessons we’ve learned over the past year running a federated graph in production.


## 1. Build your schema as you go, don’t try to do it all at once.


When we first started using GraphQL, we initially tried to create as much of the schema as possible. That worked great for a proof of concept, but it wasn’t until we got the green light to build it for production that we realized that **upfront schema design** isn’t the best idea.


Our initial approach was to mirror the shape of the existing RESTful API calls, but this resulted in many extra fields that no one was using. A year later, we are *still* in the process of removing those unused fields.


We now believe in something we call *Just In Time Schema (JITS)* development. It’s essentially the term[YAGNI](https://martinfowler.com/bliki/Yagni.html) (you ain’t gonna need it) but applied to schema design; this means that it’s not until a consumer *truly* needs a particular field or a type that we expose it.


## 2. Documenting the Schema helps. A lot.


One thing that has been a massive help for consumers of our graph is the ability to document the schema and make that documentation easily consumable. Internally, we had an enormous lack of documentation, and our APIs’ usage relied a little too much upon tribal knowledge.


With[documentation strings](https://www.apollographql.com/docs/apollo-server/schema/schema/#documentation-strings) , the schema has become a large part of our documentation for consumers. People no longer have to guess what you can do with our API. It’s been a breath of fresh air, and we no longer have to have to track down which query params work and which ones don’t.


## 3. The performance improvement is real


At StockX, GraphQL brought massive performance increases. We’re talking SECONDS of improvement – yes, you read that right. Seconds!


The most critical performance improvement we enjoyed was the reduction of payload size; with GraphQL, payload sizes were up to 7 times smaller! We noticed that every metric we were tracking on mobile devices seemed to improve once we made the payload reduction.


Architecturally, GraphQL does become the choke point for requests. This led us to look into improving multiple parts of our edge layer. Before GraphQL, we used various caching strategies since our serverless handlers and servers did their own caching. The shift to GraphQL allowed us to unify our caching approach and increase the overall strategy.


With a more unified and easier-to-reason-about caching strategy, we were able to reduce the amount of requests to our backend services.


*Stay tuned for my other post titled “ **Caching Approaches in GraphQL Federation** ” to learn how to cache in a federated GraphQL architecture using memoization, data loader, and distributed caching techniques.*


## 4. There’s a learning curve.


Most people know how RESTful APIs work – it’s become something of a web standard. To me, the learning curve for GraphQL resembles a hockey stick. Unless you’ve integrated with GraphQL or used GraphQL before, there will undoubtedly be a learning curve because it’s a much different API style from REST. Since you’re no longer calling an API to get everything the server will give you back as a response, clients need to know what they want and request it.


Once you wrap your head around GraphQL, you’ll notice a mindset change from the RESTful world; and it’s a world where you can move much faster.


## 5. Faster development cycles.


After switching over to GraphQL, we’ve noticed that our tickets complete faster. Instead of “Team A” waiting for “Team B” to do something, then “Team C” to expose the data for “Team A”, teams can now more independently due to their ability to traverse through the graph to get the data they need.


For example, when “Team A” needs to use a part of the federated graph that they don’t necessarily own, they don’t have to wait for other teams to help them. This graphical nature of GraphQL solves more problems than we initially thought.


## 6. Don’t reinvent the wheel.


GraphQL is excellent, but it still faces the same problems as any other technology out in the wild. For example, scaling is a challenge you’ll want to tackle with well-known architectural best practices.


That means using Apollo Federation to implement a microservice-like architecture by decoupling our graph by *concern* ; this enables teams to manage their distinct parts of a single federated GraphQL API, scale traffic more effectively, and ship code faster.


We initially tried to build our own “pure” GraphQL server/schema. It wasn’t until we exposed it to production traffic and experienced common challenges serving a monolithic API that we realized we needed to follow trusty distributed computing best practices.


## 7. GraphQL federation scales well horizontally.


After putting our federated GraphQL implementation into production, we found that horizontal scaling was the best way for us to serve large traffic loads.


We use NodeJS- and with NodeJS being single-threaded, it allows for more requests to be handled simultaneously.


We still have to preemptively scale our systems before large traffic spikes like a push notification or other marketing events. But one of the beautiful parts of using Federation is that we can scale *only* the implementing services affected by the event.


For example, if we send out an email to clients about a fantastic blog post, we can scale the blog portion of our federated architecture and leave everything else the same. This is great when we look at the cost of running our services. Due to not having to scale everything, we save money.


## 8. Read. Then, keep reading.


We have had some stumbles while adopting GraphQL. The team at StockX has had no prior experience with GraphQL.


So we read, and then read, then read even more. The amount of knowledge out there has helped us make more informed decisions. Some of the articles we found helpful were:


- [GraphQL Best Practices from GraphQL.org](https://graphql.org/learn/best-practices/)
- [GraphQL Cursor Specification from Relay.dev](https://relay.dev/graphql/connections.htm)
- [How to Handle Pagination from GraphQL.org](https://graphql.org/learn/pagination/)


## 9. GraphQL forces domain conversations.


When we first started GraphQL, our types were a mess. Sure, our endpoints *structurally* returned the same payload as the RESTful API we had in place before, and that made the front-ends work, but it was a mess to work with.


Eventually, we had the opportunity to design the graph from scratch again, starting clean.


We used this opportunity to start conversations which led to a better understanding of the division of responsibilities and relationships between services and types. We discussed what fields belong in which type, what to do with our massive existing types, and how to break them into smaller, better namespaced types. We’re now at a point where our dedicated selection of types work much better for us.


## Conclusion


It was a lot of work to migrate to GraphQL, but looking back at all that work over the last year, I *still* think that migration was a major success and was incredibly impactful to the way we work today. GraphQL has enabled us to have faster development cycles, push smaller chunks to production with fewer problems, rethink our UX, among so many other improvements.


Written by


Kyle Schrade


[Read more by Kyle Schrade](https://www.apollographql.com/blog/author/kyleschrade)
