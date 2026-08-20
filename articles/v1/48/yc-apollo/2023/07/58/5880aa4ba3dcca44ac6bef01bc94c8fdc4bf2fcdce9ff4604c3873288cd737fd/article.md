---
schema_version: "1.0.0"
document_id: "5880aa4ba3dcca44ac6bef01bc94c8fdc4bf2fcdce9ff4604c3873288cd737fd"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/provide-real-time-data-for-consumers"
published_at: "2023-07-14T10:11:59+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:b1ed55b5f546abd129c1cd3d8ac39b7c674e802b1682aa091df2ba08e1f08904"
---

# Provide real-time data for consumers

This post is a part of our “[How to power modern media apps with Apollo GraphOS](https://www.apollographql.com/blog/platform/media/how-to-power-modern-media-apps-with-apollo-graphos/) ” series. Also in this series:


- [Render frontends faster](https://www.apollographql.com/blog/platform/media/render-frontends-faster)
- [Enable omnichannel experiences](https://www.apollographql.com/blog/platform/media/enable-omnichannel-experiences)
- [Manage versions of your API for third-parties](https://www.apollographql.com/blog/platform/media/manage-versions-of-your-api-for-third-parties)


---


To meet consumer demand, media companies continuously strive to provide immersive and dynamic experiences across their platforms. A crucial aspect of powering these types of experiences is being able to stream real time updates to users.


There are almost always interesting technical and architectural implications at play when dealing with real-time data, particularly regarding how that functionality will be implemented at scale.


For instance, WebSocket connections are often the de-facto solution for simple use cases and often been used for implementing subscriptions operations in GraphQL APIs. However, simply implementing traditional WebSockets can also create limitations and architectural issues when you then need to provide the experience to millions of end users. What then happens in the common scenario where the subscription operation needs to return data from numerous data sources? It is exactly these challenges that federated subscriptions in Apollo GraphOS are able to solve for.


## Federated subscriptions with GraphOS


Subscriptions with GraphOS offer an alternative to traditional WebSocket connections by leveraging a new transport protocol based on HTTP between the client and Apollo Router. Additionally, federated subscriptions provide WebSocket deduplication to enable the solution to scale, as well as improved query planning enabling the subscription to provide data from multiple sources.


The ability to provide federated subscriptions can unlock new opportunities for media companies that wish to leverage a single subscription call from a client that needs to receive data in real-time from a large number of data sources. One of the most powerful features unique to Apollo federated subscriptions is that an individual subgraph can initiate real time updates, and after which, the Apollo Router can then contribute additional fields from other services to the payload returned to the client. This approach allows you to reap all of the benefits of Apollo Federation that have been available for query and mutation operations while streaming real-time data that can provide more responsive and personalized experiences for media consumers.


The transport protocol used by federated subscriptions is the new[multipart-subscriptions protocol](https://github.com/graphql/graphql-over-http/blob/main/rfcs/IncrementalDelivery.md) . This protocol provides similar end results to subscriptions over WebSockets without being limited to a low number of open connections as it is based on the[Incremental delivery over HTTP](https://github.com/graphql/graphql-over-http/blob/main/rfcs/IncrementalDelivery.md) spec. Real-time communication between Apollo Router and any applicable subgraphs is carefully implemented and performed over WebSockets because it enables maximum compatibility between existing subgraph servers that use subscriptions via protocols such as[graph-ws](https://github.com/enisdenjo/graphql-ws) (currently the default for Apollo Router).


In order to help operate at scale, Apollo Router also provides a deduplication mechanism that allows multiple subscription requests from multiple clients to a single Router instance to be serviced by a single WebSocket connection to the required subgraph(s), instead of opening a new websocket to serve the same data to individual clients.


## Getting Started with Federated Subscriptions


The experience for front-end developers that consume subscription operations that span multiple subgraphs will be much the same as consuming them through any GraphQL API. Subscription operations can be tested in Apollo Studio using Explorer just as queries and mutations can:


It’s important to note though that from Explorer, subscriptions can technically be configured for both WebSockets and multi-part HTTP for testing purposes. However, they can only be retrieved over HTTP for connections to Apollo Router. This can be easily configured via Apollo Studio, demonstrated in the following screenshot:


When initiating an Apollo Router instance with subscription support, there are a number of options that can be set in the YAML configuration file. Below is an example Apollo Router configuration for subscriptions in passthrough-mode, which uses long-lived WebSocket connections between Router and Subgraphs that expose a subscriptions endpoint of /subscription.


```text
subscriptions:
mode:
passthrough:
all:
path: /subscription
```


For more information on all of the configuration options available for subscriptions in Apollo Router, along with all of the pre-requisites, please visit[our documentation](https://www.apollographql.com/docs/apollo-server/data/subscriptions) .


## A new step for real-time APIs


Federated subscriptions allow media companies to serve huge numbers of clients with dynamic, personalized real time data originating from any of the data sources that back their supergraphs. This ability to scale and maintain both resilience and performance unlocks huge opportunities for richer experiences for customers and to offer even more bespoke real-time APIs for external users and third parties.


An example media supergraph is also available to explore on[Github](https://github.com/apollosolutions/media-supergraph) with a Federated subscription in place to demonstrate its capabilities and the new potential for serving real time data.


## Get started with a media supergraph today


The best way to see the possibilities of a supergraph is to try one out.[You can explore a media supergraph and run real queries against it here.](https://studio.apollographql.com/graph/Media-Demo-Graph/variant/current/explorer)


We also have a series of case studies and blogs that dive into different elements of a typical media organization’s schema to illustrate how Apollo GraphOS help power essential features of modern media applications:


If you’d like to talk to an Apollo expert about how a supergraph can power your media services,[please reach out to us.](https://www.apollographql.com/contact-sales/?referrer=media-solutions-page)


Written by


Joe Devine


[Read more by Joe Devine](https://www.apollographql.com/blog/author/joe)
