---
schema_version: "1.0.0"
document_id: "7492ce9a63e5f11e2351db0f94d75aeed3292c714fea2168120c912292c308fa"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/what-is-graphql-introduction"
published_at: "2021-02-16T14:01:05+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:cc40e6e22bfef4fc2db7efbed442c8bdc94a4fbfe852c024019f062e93fd6330"
---

# What is GraphQL? GraphQL introduction

In recent years, app development has become quite complex. Gone are the days of merely pulling some data from a database and displaying text on a page with a form.


Today, we call upon app developers to build rich, complex user interfaces and create memorable web experiences **from a variety of different sources** .


Unfortunately, traditional API styles like REST can become cumbersome to work with as complexity grows. Enter GraphQL.


What is GraphQL? GraphQL is the modern approach to client-server communication and aims to improve the way developers build applications on the web.


In this post, we’ll learn about what GraphQL is, where it came from, how it works, and what benefits it has over traditional approaches to building and maintaining an API.


## What is GraphQL


In essence,


GraphQL is a **query language** and a **server-side runtime** (typically served over HTTP).


Let’s break this down piece-by-piece.


### GraphQL is a query language


From[our docs](https://www.apollographql.com/docs/resources/faq/#what-is-graphql) ,


**GraphQL is a language for querying data.** Unlike most query languages (such as SQL), you *don’t* use GraphQL to query a particular type of data store (such as a MySQL database). Instead, **you use GraphQL to query data from any number of different sources.**


The structured way in which we make requests presents two types of operations: **queries** and **mutations** .


Queries that *fetch data* are synonymous to` GET` calls in REST, while mutations signal that we’d like to invoke a *change* the in system, similar to REST’s` POST` or` DELETE` methods.


Learn more about the “[Anatomy of a GraphQL Query](https://www.apollographql.com/blog/the-anatomy-of-a-graphql-query-6dffa9e9e747/) ” in this post by Sashko Stubailo.


### Runtime


The *runtime* part points to the fact that a GraphQL API runs on a server.


In a GraphQL API, we shape our data within a strictly typed schema, telling it how to *resolve* data when asked for. To GraphQL, where data comes from doesn’t matter. It could be from a database, micro-service, or even an underlying RESTful API.


GraphQL is data source *agnostic* .


### Served over HTTP


GraphQL is typically served over HTTP. This means operations are just plain ol’ strings, and we don’t need a special tool to query data from a GraphQL server.


## A brief history


Facebook created GraphQL in 2012. They needed a better data-fetching approach that they could use across the entirety of the company’s products and services. They also wanted something that was understandable by developers, designers, as well as non-technical folk. After using it internally for some time, Facebook[open-sourced GraphQL in 2015](https://engineering.fb.com/2015/09/14/core-data/graphql-a-data-query-language/) .


GraphQL makes it easier for app developers to get the data they need without needing to know which sources it’s coming from


## How does it work?


### Design your GraphQL schema


On the server, we create a GraphQL schema based on the data we need to build our app UIs. If we were building a job board, it’s here that we’d model out the` Job` ,` Company` , and` Location` types.


### Connect your resolvers to data sources


For each type, we write the[resolver functions](https://www.apollographql.com/docs/tutorial/resolvers/) , connecting the data to our graph. Perhaps we get the` Job` data from an external API. And maybe we get the` Location` data from the Google Maps API.


### Ask for exactly what you want


Finally, write your query, specifying precisely what you’d like to fetch. No more, no less.


Here’s an example of a query using[Apollo Studio’s Explorer](https://studio.apollographql.com/signup/?referrer=blog) . Like we used Postman for REST, Apollo Studio’s Explorer is a great way to build out and test your GraphQL APIs.


We recommend using a GraphQL client library like[Apollo Client](https://www.apollographql.com/docs/react/) if you’re building apps with GraphQL. To see other approaches, check out “[4](https://www.apollographql.com/blog/4-simple-ways-to-call-a-graphql-api-a6807bcdb355/)[Simple Ways to Call a GraphQL API](https://www.apollographql.com/blog/graphql/examples/4-simple-ways-to-call-a-graphql-api/) ”.


## Key features of GraphQL


### Performance


**Prevents over-fetching:** In traditional approaches like REST, you often end up *over-fetching* — asking for more data than you need. With GraphQL, you can ask for exactly what you need. The benefit is reduced bandwidth, which may be especially important on mobile and low-energy devices.


**Prevents under-fetching (multiple round-trips):** The other end of the fetching problem in REST is *not* getting all the data you need in a single round-trip. Consider fetching your friends list in a social media app and needing to batch individual queries to get each friend’s profile picture. GraphQL helps solve this problem.


### Developer experience


**Hierarchical & declarative:** GraphQL data is inherently *graphical* and declarative. This makes your queries easier to understand and your data easier to work with. By nesting, we can ask for related data, keeping queries cohesive, and spending zero time stitching together multiple responses like we sometimes have to do with REST.


**Strongly typed (stable API):** GraphQL is strongly typed. This means a more stable API with fewer bugs in development and introduces the possibility of more intelligent tooling.


**GraphQL versioning:** GraphQL favors the notion of a single, incrementally developed graph. You can give deprecation hints when you add, remove, and change fields on your existing graph. Compare this to the coarse-grained approach to versioning in a REST context with version numbers across an entire API.


### Architecture


**Decouples the client from the server:** UI developers don’t need to wait for backend teams to build an API first. A GraphQL schema acts as a *contract* for front-end app devs to start building (by mocking out the API calls) and leaves backend teams to deliver on the contract by building the underlying services that serves the graph.


**Single source of truth:** UI developers only need to know about to a single endpoint to access the entirety of the data behind a company’s graph.


**Scalable with Federation:** GraphQL fits nicely into a microservice architecture using an approach called Federation. Federation enables backend teams to maintain their own sub-graphs and compose them into a single graph for an entire organization.


**Introspection:** This key feature lets you ask the GraphQL system what types of queries it supports. Introspection provides the framework for all kinds of great tooling. There exist tools to auto-generate API docs and TypeScript types for your resolvers and queries.


### Community


**Great tooling** : We build tools for GraphQL developers. From GraphQL client libraries (like our very own[Apollo Client](https://www.apollographql.com/docs/react/) ), GraphQL IDEs ([Apollo Studio](https://studio.apollographql.com/?referrer=blog) ), and GraphQL servers (Apollo Server), you’re likely to modern tooling in a variety of languages. There are also a wide range of mature community-built tools for you to try out.


**Widespread adoption:** GraphQL is currently being used by a large number of notable companies including GitHub, PayPal, Glassdoor, Shopify, Twitter, The New York Times, Expedia, etc.


## Challenges & considerations


When GraphQL was first released, major topic areas like **caching, query complexity,** and **security** were major concerns in the community.


Over the years as GraphQL and the larger community around it matured, we’ve identified common solutions and best practices to the majority of these problem areas.


### FAQ


For frequently asked questions about GraphQL involving topics like proof of concepts, schema design, migrating to GraphQL, error handling and more, I recommend reading the[official Apollo GraphQL FAQ page](https://www.apollographql.com/docs/resources/faq/) in our docs.


## Conclusion


- GraphQL is a query language and a server runtime that typically runs over HTTP
- It was designed to be more efficient, performant, and provide a better developer experience for working with data in a client-server architecture
- GraphQL adoption has significantly grown since it was first open-sourced and it doesn’t look like it’s going anywhere soon.


### What next?


I’m a believer of “if we do it, we know it”. If you’re just getting started with GraphQL, I highly recommend you check out our *completely* *free* “Lift Off” GraphQL course.


In less than 30 minutes, you’ll learn the core components of a full-stack GraphQL app with Apollo through an interactive tutorial.


You can[get started here](https://odyssey.apollographql.com/?referrer=blog) . Enjoy!


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
