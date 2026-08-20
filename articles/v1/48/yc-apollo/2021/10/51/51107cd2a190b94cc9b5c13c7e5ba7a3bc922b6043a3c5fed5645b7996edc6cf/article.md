---
schema_version: "1.0.0"
document_id: "51107cd2a190b94cc9b5c13c7e5ba7a3bc922b6043a3c5fed5645b7996edc6cf"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/where-does-graphql-fit-in-the-stack-modern-app-development-with-graphql"
published_at: "2021-10-26T10:15:53+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:25d95141645c80467a9a64327defa5d63c9b2e16d489dc8eae338e4ee7ed3548"
---

# Where Does GraphQL Fit In the Stack? – Modern App Development with GraphQL

GraphQL is a popular language, and it’s getting more popular by the day. We hear front-end developers, mobile developers, and back-end developers talking about GraphQL. So it’s clear that developers use GraphQL across the stack. But where exactly does GraphQL fit in the stack, and what benefits do you gain by using it?


In this article, we’ll first do a quick recap of what “the stack” looks like without GraphQL, and then we’ll dive into how adding GraphQL changes it and what that means for building applications.


## The Stack Without GraphQL


An architecture without GraphQL will usually consist of three layers – the UI layer, the service layer, and the data layer. The specific structure of those three layers will look different depending on the size and needs of the company.


### UI Layer


The UI layer consists of all clients that interact with any backing APIs (the service layer). Including web apps, mobile apps, streaming devices, kiosks, etc.


### Data Layer


The data layer consists of all the data stores accessed by the service layer. Depending on the architecture of the service layer, the data layer could take on many forms.


### Service Layer


The service layer exposes one or more APIs to the UI layer. It can take on many different shapes, but for now, we’ll cover three common architectures:


1. **Client-to-service –** The service layer consists of internal and external APIs exposed to the UI layer, creating a direct client-to-service relationship.
2. **API Gateway –** The service layer consists of a single API that sits in front of other backing services. Clients interact only with the API Gateway.
3. **Backend-for-frontend** – The service layer consists of multiple smaller APIs responsible for one or more clients that sit in front of other backing services.


While these architectures are functional, they fail to expose more capabilities to the UI layer without forcing more complexity on the clients. The constantly increasing number of endpoints makes managing communication with the service layer difficult. As the number of endpoints increases, the client’s ability to iterate decreases.


The service layer often evolves into architectures like the API gateway and BFF patterns to prevent the clients from managing many endpoints. However, even with APIs explicitly built for a few or even a single client, they still have the problem of an increasing number of endpoints as the API surface grows.


Adding GraphQL into the stack solves this problem. Let’s dive into what this means and closely examine how GraphQL changes the stack by introducing a new layer called **the graph** .


## The Stack With GraphQL


GraphQL serves as the doorway into the graph layer. The graph layer fits in between the UI layer and the service layer. It brings all of a company’s data and services together into one consistent, secure, and discoverable interface so that anyone can access it through a single endpoint.


Because the graph layer only exposes a single endpoint through GraphQL, it decouples the UI from changes further up the stack, and it will always remain a single endpoint, no matter how large the service layer and graph layer may get.


Clients can now focus on building the product instead of building the infrastructure needed to manage all the endpoints from the service layer. Inversely, the service layer can evolve freely, without worrying about affecting clients.


## Conclusion


Let’s return to the original question: where does GraphQL fit in the stack? We’ve uncovered that using GraphQL changes application architecture by adding the graph layer to the stack. So GraphQL does more than fit into an existing layer in the stack – it forms the foundation for a completely new layer.


With the graph layer in place, clients can access what they need through a single endpoint. In addition, the reduced surface area allows developers on both the client and server sides to build independently, creating a more flexible architecture. If you’re interested in learning more about the benefits of using GraphQL instead of REST, take a look at this[GraphQL vs. REST article](https://www.apollographql.com/blog/graphql/basics/graphql-vs-rest/) .


Written by


Ceora Ford


[Read more by Ceora Ford](https://www.apollographql.com/blog/author/ceora)
