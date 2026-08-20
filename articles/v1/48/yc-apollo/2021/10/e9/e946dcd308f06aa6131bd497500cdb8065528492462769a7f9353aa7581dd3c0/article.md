---
schema_version: "1.0.0"
document_id: "e946dcd308f06aa6131bd497500cdb8065528492462769a7f9353aa7581dd3c0"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/demystifying-graphql-misconceptions-top-five-graphql-myths-debunked"
published_at: "2021-10-19T09:18:15+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:473e0cc91d1dabe32daaecd5078cf2e09b6fae40b8af02aebac477268460f902"
---

# Demystifying GraphQL Misconceptions | Top Five GraphQL Myths Debunked

Trying to get buy-in to adopt GraphQL can be challenging when voices in the room provide inaccurate information about the technology. FUD (fear, uncertainty, and doubt) about things like security and caching can grind decision-making to a halt. These topics can be challenging to address without having access to and gathering the proper resources.


That’s why we’ve compiled a list of the five most common GraphQL misconceptions you might encounter, along with resources to help address any related concerns! In this article, we’ll learn about securing, caching, and scaling GraphQL APIs, what teams benefit the most from adopting it, and a bit of how GraphQL fits into the stack.


If you’re looking for a comparison of REST vs. GraphQL, check out[this article](https://www.apollographql.com/blog/graphql/basics/graphql-vs-rest/) . Unfamiliar with a lot of GraphQL vocabulary? Be sure to check out the[GraphQL Glossary](https://www.apollographql.com/docs/resources/graphql-glossary/) .


## 1. GraphQL isn’t secure.


It’s true. A GraphQL API isn’t secure in the same way that a REST API isn’t secure until you take the necessary steps to ensure it.


When securing a GraphQL API, there are three main areas you’ll need to consider, **authentication/authorization** , **reducing attack surface** , and **operations and governance** .


### Auth


GraphQL APIs still use` HTTP` as the transport protocol and, as such, can use the same authentication and authorization patterns used for REST APIs. The implementation details change, but GraphQL API developers can rely on the exact authentication strategies you use in REST APIs, like JWTs, and authorization strategies you would use in REST APIs, such as role or action-based authorization.


### Reducing Attack Surface


One of the benefits of using GraphQL is the powerful query language for data access and manipulation. However, this DSL opens up new attack vectors — such as query depth, query complexity, and batch query attacks — that malicious actors can use to target your APIs data or performance. However, these attacks and other common attack vectors faced by all APIs — such as rate-limiting — can all be prevented with existing solutions.


### Operations & Governance


Beyond protecting your GraphQL API from bad actors and locking down private data, you also need a window into how your API is being used and by whom. GraphQL’s declarative query language combined with tracing makes it possible to identify anomalies — performance drops, error spikes, and traffic spikes — and trace them back to the actual operation and client.


Check out our[GraphQL Security Checklist](https://www.apollographql.com/blog/graphql/security/9-ways-to-secure-your-graphql-api-security-checklist/) for an in-depth guide on securing your graph.


## 2. You can’t cache GraphQL.


Generally, when you hear this statement, what folks are referring to is **edge caching** . But, more specifically, they are discussing the difficulty of implementing a drop-in edge caching solution like a CDN.


By default, GraphQL uses the` POST` method for all requests. Most CDNs ignore` POST/PUT/DELETE` requests because they’re used for mutating data in REST APIs. However, we can still get edge caching in GraphQL by using **cache hints** and **automatic persisted queries** .


We should always use a strategic approach to decide what to cache and how long to ensure security, data accuracy, and timeliness.


### Cache hints


Apollo Server enables you to define cache-control settings (` maxAge` and` scope` ) for each field in your schema with the` @cacheControl` directive.


When Apollo Server resolves an operation, it calculates the correct cache behavior based on the most restrictive settings among the results’ fields. Whenever Apollo Server sends an operation response with a non-zero` maxAge` , it includes a` Cache-Control` HTTP header describing the response’s cache policy.


If you run Apollo Server behind a CDN or another caching proxy, you can configure it to use this header’s value to cache responses appropriately.


### Automatic Persisted Queries


Suppose you need to send` GET` requests because you can’t configure your edge caching to support cache hints for` POST` requests. In that case, you can also use automatic persisted queries (APQ) to create a unique hash for each query after the first request and then use` GET` requests with the hash for all future requests, allowing them to be cached by a CDN service.


Automatic persisted queries are supported by both Apollo Server and Apollo Client and are enabled through the` persisted-queries` link on the client instance.


Check out the[Apollo server documentation](https://www.apollographql.com/docs/apollo-server/performance/caching/) for more on all things edge caching.


## 3. GraphQL exposes your underlying database.


GraphQL uses something called a **schema** to define the shape of the API. A schema is like a contract between the server and the client. The schema defines what data the clients can request or change, and the server returns that data to the client.


There are some approaches to building GraphQL APIs that closely tie the functionality of the API to the underlying data source, such as managed GraphQL solutions. They reduce the complexity of managing your API and data layer infrastructure, but often at the cost of auto-generating a schema.


However, by not using a managed solution and applying a schema-first design approach, you can structure your schema based on precisely the data the client application needs and not based on the underlying data source.


For a more in-depth look at how GraphQL works under the hood, check out[The Journey of a GraphQL Query](https://www.apollographql.com/blog/graphql/basics/journey-of-a-graphql-query/) .


## 4. GraphQL doesn’t scale.


Typically when we think of scale, we think of requests per minute and response times, but performance is only one part of scaling a technology. With the emergence of cloud computing and cloud providers offering various solutions for scaling servers horizontally and vertically, scale becomes more of a conversation about scaling the communication and collaboration around what the API should provide.


We’ve seen companies like Netflix, Walmart, PayPal, and others across every industry adopt GraphQL, but why?


Because while the technical benefits like performance gains from fewer requests and over-fetching of data initially drive GraphQL adoption, it quickly becomes clear that the developer experience and business intelligence benefits are the actual value of GraphQL.


As Mark Stuart states in their article “[Scaling GraphQL at PayPal](https://medium.com/paypal-tech/scaling-graphql-at-paypal-b5b5ac098810) ,” GraphQL is friendly to humans. With GraphQL, people think in terms of fields, entities, and relationships, not in endpoints and complex joins. The focus on client needs also ensures the API directly reflects business needs as client needs equal business needs.


GraphQL, despite the name, isn’t simply a query language. Instead, it’s a comprehensive solution to the problem of connecting modern apps to services in the cloud. It forms the basis for a new and vital layer in the modern application development stack: **the graph** .


The graph brings all of a company’s data and services together with one consistent, secure, and discoverable interface so that anyone can draw upon it with minimal friction.


If you’d like to know more about scaling the graph, check out our graph best practices guide,[Principled GraphQL](https://principledgraphql.com/) , and this post about[The Evolution of GraphQL at Scale](https://www.apollographql.com/blog/backend/architecture/the-evolution-of-graphql-at-scale/) .


## 5. GraphQL is only for large companies.


Inversely, GraphQL provides a lot of developer experience improvements that benefit any team, no matter the size. For example, we all know that iteration speed is equally important whether you’re one person building your first app or part of a team working on a massive API. Sunil Pai[recently tweeted](https://twitter.com/threepointone/status/1449736965537075201) asking which vectors people prioritize and more than half chose “Iterable”.


GraphQL enables many new tools, communication channels, and operational checks that allow teams of all sizes to benefit from adopting it. Here is a non-exhaustive list of benefits of adopting GraphQL that apply to a team or teams of any size:


- Complexity moves back to the server, which allows clients to be slimmer and loosely coupled.
- Data is more discoverable to developers and Product Managers, QA, or anyone else who might want to understand the business’s full digital capabilities.
- Deeper insights into API usage allow for better observability and tight integration into CI/CD workflows by adding operational checks, preventing breaking changes.
- Federated architectures allow backing services to scale independently based on usage and encourage fault tolerance on subsystems.
- Smoother API evolution. Rather than releasing a new “version” of the graph periodically — such as every 6 or 12 months — you make changes to it many times a day if necessary. Contributors can add new fields at any time. To remove a field, it is first deprecated with the` @deprecated` directive and then removed when no consumers use it.
- You can adopt GraphQL gradually. Many companies start by only running a few operations through the new GraphQL API, adding new functionality as they get more buy-in. This allows your graph to evolve naturally.


Check out the docs for more detail on the[benefits and trade-offs of adopting GraphQL](https://www.apollographql.com/docs/intro/benefits/) .


## Conclusion


GraphQL is a production-ready technology that offers many developer experience benefits to single developers and large companies alike. However, while GraphQL has many benefits, it differs fundamentally from REST and can cause FUD among developers over specific topics, especially security and caching.


When adopting GraphQL, it’s best to use a client-driven approach to designing your API and remember that a schema is a contract between the client and server. The schema defines what data the clients can request or change, and the server returns that data to the client.


The graph brings all of a company’s data and services together, with one consistent, secure, and discoverable interface so that anyone can draw upon it with minimal friction.


Written by


Kurt Kemple


[Read more by Kurt Kemple](https://www.apollographql.com/blog/author/kurtkemple)
