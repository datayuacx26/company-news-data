---
schema_version: "1.0.0"
document_id: "d8a8d73c823fe21900a92300c106b0c09ef74e573adf8a19a54d62f51230a14f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/stop-passing-the-hot-potato-leveraging-api-orchestration-to-drive-developer-efficiency"
published_at: "2025-05-09T19:19:58+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:ff0de7919e436e039cb4739e42ea9b2faaf543f18e5d71b232aca8147eb945a2"
---

# Stop Passing the Hot Potato: Leveraging API Orchestration to drive developer efficiency

Consider the application architecture of an online pet supply store. There might be a product catalog that would need to pull data from multiple services, including inventory, reviews, and prices. A user profile would require APIs to access user information such as demographics, loyalty program information, and previous orders. With all these APIs, it’s easy to see how API responses must be orchestrated. For instance, let’s take a look at processing an order. First, the app would need to confirm product availability, get product SKUs & prices, calculate shipping, calculate taxes, and then initiate payment. The information coming from the APIs might also require transformation. External payment systems such as Stripe return verbose, deeply nested JSON responses. The client does not require the complete Stripe response, so a transformed version of the data is returned, including specific information such as transaction ID, payment status, and customer name, all normalized in a format ready for client consumption. Let’s not forget about data protection. Managing the authentication and authorization credentials of all the different APIs introduces risk and is cumbersome and time-consuming. As you can see, complex web applications often integrate numerous APIs, leading to engineering challenges, increasing engineering costs, and slowing innovation.


Apollo, a leader in graph-based API orchestration, recognizes these challenges and has created the[GraphOS platform](https://www.apollographql.com/why-graphql) to address them. Apollo leverages GraphQL, a query language for APIs, in combination with its Apollo Router to abstract the underlying data sources from the clients. This abstraction makes it easy for client apps to simply request the exact data they need and have it returned from the back end, regardless of the number of API calls required to satisfy the query.


Apollo’s approach is to view APIs as a graph of objects rather than a collection of endpoints. Data from APIs is structured within a GraphQL schema, which defines the available data types and their organization for querying or mutation. This schema serves as a contract between the server and the client. It reduces the necessity for procedural code, allowing clients to request data declaratively by specifying what data they need rather than how to retrieve it.


Using schema and API sources, Apollo effectively manages API calls, optimizing the query process to produce only the necessary results, thus preventing over-fetching and under-fetching.


Apollo users have seen significant enhancements in engineering efficiency, enabling developers to concentrate on solving business issues rather than generating boilerplate orchestration code.[RS Group](https://www.apollographql.com/customers/rs-group) , a prominent provider of industrial and electronic solutions, has developed numerous APIs over time, leading to a complex and fragmented technology environment. By leveraging Apollo and graph-based orchestration, they adopted a declarative strategy for their APIs, which simplified the orchestration, minimized complexity, and increased the flexibility of their system. This resulted in a fivefold increase in throughput, allowing them to expand their organization without additional financing.


## Join us for a Deep Dive on API orchestration


RS Group is just one example of the many that have increased developer efficiency using Apollo. Join us **May 29th, 2025** at 9:00am PT, for a deep dive into the origins of the API orchestration problem and learn how traditional solutions like BFFs and gateways fall short, and how Apollo delivers a purpose-built approach for scalable, efficient, and future-ready API orchestration — even in the era of AI. If you can’t make the time, sign up and receive a recording.


[Register for the webinar](https://www.apollographql.com/events/stop-passing-the-hot-potato-leveraging-api-orchestration-to-drive-developer-effic)


## Discover ways to enhance efficiency at your own company


Are you curious about how API orchestration will work with your application? Sign up for a **free, no-obligation 1-hour working session** with an Apollo Architect.


[Sign up for an individual working session with an Apollo architect](https://www.apollographql.com/free-consultation)


Written by


Robert Walters


[Read more by Robert Walters](https://www.apollographql.com/blog/author/robertwalters)
