---
schema_version: "1.0.0"
document_id: "c35cf7a6c57ed05859246223d470c7881b37e282de1af5c961cd9ee159d336e4"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/federated-subscriptions-in-graphos-real-time-data-at-scale"
published_at: "2023-06-22T11:35:12+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:7f4eca3734719ad815b84d13f56fbdd4835c9e4ccc3fe8eb312c35955e1b5889"
---

# Federated subscriptions in GraphOS: real-time data at scale

Building an API that can serve real-time data to millions of clients is no small challenge. With traditional microservice architectures and REST, building real-time experiences meant excessive polling traffic, over-fetching, and constant coordination across backend and frontend teams to maintain the right endpoints for real-time services.


GraphQL subscriptions offer an elegant alternative to REST and polling, allowing clients to subscribe to real-time updates for just the data they need. However, while setting up subscriptions for a single service and a small number of concurrent clients is straightforward, developing subscriptions to support *millions* of clients with lockstep coordination across multiple backend teams and services, is not. Developers from every corner of the organization must contribute to a single schema with thousands of lines, added types become duplicative, and removed types lead to breaking changes. And if you[can’t efficiently scale WebSockets](https://thenewstack.io/the-challenge-of-scaling-websockets/) , you’re out of luck.


Federated GraphQL APIs and Apollo GraphOS provide the foundation for a better, more scalable experience. Today, we’re excited to announce support for **federated GraphQL subscriptions** in Apollo GraphOS: an advanced approach to handling GraphQL subscriptions in an enterprise-scale federated GraphQL API. Federated subscription support in GraphOS goes far beyond standard GraphQL subscriptions, introducing a new transport protocol for client-to-router communication, WebSocket deduplication to improve scalability, and updated query planning that allows a single subscription to serve data from multiple separate services.


## Subscriptions, meet Apollo Federation


Starting today, federated subscriptions are generally available for organizations with a GraphOS Enterprise plan running self-hosted instances of Apollo Router. For users on the GraphOS Serverless plan running cloud-hosted routers, federated subscription support is currently available in preview.


Support for subscriptions in a federated architecture has been long requested by our customers and community alike. We’re excited to finally get it into your hands. The key requirements brought to us by our customers were as follows:


**Self-service delivery for client teams** **** Client teams should be able to use a single subscription to access the appropriate data to build their apps.


**Flexibility for service teams** Backend teams should be able to use their existing services to contribute fields to a shared subscription type without tight coupling across teams.


**Performance and reliability for infrastructure teams** API platform teams should be confident in scaling subscriptions across millions of concurrent connections without sacrificing performance.


## One subscription, countless services


Federated subscriptions take full advantage of federated graph architecture by allowing a single subscription to request any appropriate fields in a federated graph. This self-service approach enables feature developers to build with data that lives across dozens or even hundreds of separate services using a single subscription. Once subscriptions are defined in subgraph schemas and an individual subgraph initiates the real-time update, the router can add additional fields from other services and return them back to the client as if they came from a single source.


Developers can quickly build multiservice subscription operations using the Explorer in GraphOS. Fields that can be subscribed to are automatically shown under a single subscription root type and can be added to an operation with just a click.


Requesting all necessary data in a single subscription operation allows teams to build more personalized applications. For example, we have customers using federated subscriptions to provide promotions based on where the user is located, even though the information required to do so is spread across disparate services.


## Decoupled service development


With federated subscriptions, backend developers can now independently contribute fields from real-time data sources to the federated graph. Rather than requiring backend teams to work in lockstep coordination across an organization, federated graph development is modularized across multiple subgraphs that can be owned and evolved independently.


Each subgraph defines its own subscription type along with associated fields and resolvers. Fields from the subscription type in each subgraph are automatically composed into the root subscription type for the federated graph.


To make it as easy as possible to use federated subscriptions with existing GraphQL services, GraphOS routers support both the[graphql-ws](https://github.com/enisdenjo/graphql-ws/blob/master/PROTOCOL.md) protocol and the deprecated[subscriptions-transport-ws](https://github.com/apollographql/subscriptions-transport-ws) protocol for communicating with subgraphs. In most cases, any GraphQL server with support for Apollo Federation that is already running GraphQL subscriptions will work seamlessly with federated subscriptions.


## Built for enterprise-scale


From supporting the Major League Baseball app during the post-season or Expedia flight data during the holiday season, Apollo has to make sure that federated subscriptions can perform at scale. Prior to launch, federated subscriptions were pressure tested by enterprises with some of the most demanding concurrent client use cases and have been **proven to deliver upwards of 9,000 subscription events per second** with only a few microseconds of latency.


Scaling GraphQL subscriptions typically requires overcoming two client-server communication challenges: (1) handling millions of client connections to the server at once and (2) managing stateful transports (such as WebSockets). In a federated graph with a router handling communication between clients and multiple servers, these challenges span two architectural areas, each with its own scaling needs: client-to-router communication and router-to-subgraph communication. Federated subscriptions mitigate these issues with redesigned and optimized transport protocols so that API teams can confidently deliver real-time API infrastructure at scale.


### A new protocol for client-to-router communication


For client-to-router communication, we designed a new` multipart-subscriptions` transport protocol that delivers a similar end result to subscriptions with WebSockets or Server-Sent-Events (SSE) without being limited to a low number of open connections. With` multipart-subscriptions` , which is based on the[Incremental Delivery over HTTP specification](https://github.com/graphql/graphql-over-http/blob/main/rfcs/IncrementalDelivery.md) also used by GraphOS for` @defer` , subscription requests are transmitted from clients to routers over HTTP, and events are streamed back to clients in a multi-chunked format.


To reduce the work required for app developers to use federated subscriptions, we’ve implemented support for` multipart-subscriptions` in our three Apollo clients (Web, iOS, and Kotlin), and we will be releasing low-level library implementations for other popular GraphQL clients shortly.


### Deduplication for router-to-subgraph communication


We chose to continue using WebSockets for router-to-subgraph communication given that they are the most commonly used subscriptions transport for the majority of GraphQL servers (specifically,[graphql-ws protocol](https://github.com/enisdenjo/graphql-ws/blob/master/PROTOCOL.md) and the deprecated[subscriptions-transport-ws protocol](https://github.com/apollographql/subscriptions-transport-ws) ).


To aid with some of the scalability challenges of maintaining WebSocket connections between the router and subgraphs, federated subscriptions provide a deduplication mechanism. With this mechanism, similar subscriptions coming from clients are fully de-duplicated between the router and subgraphs — meaning, if 1,000 clients send the same subscription request over HTTP to a router instance, the router will only open a single WebSocket connection against a subgraph server. For applications that expect many concurrent client connections or that have a particularly busy subscription subgraph, this can significantly reduce the number of open WebSocket connections.


## A principled architecture for real-time APIs


Federated subscriptions in Apollo GraphOS give feature developers, backend teams, and API owners a platform for delivering real-time user experiences at a scale never before possible. **Here’s how you can get started:**


### Federated subscriptions with GraphOS Cloud


***Note:** At time of posting, federated subscriptions are in preview for GraphOS Cloud.*


GraphOS Cloud provides you with a cloud-hosted instance of Apollo Router that automatically updates to the latest version.


To use subscriptions with a cloud-hosted router:


1. [Create a free GraphOS org](https://studio.apollographql.com/signup) on the Serverless plan.
2. Create a new cloud-hosted router endpoint in GraphOS by connecting an existing GraphQL server running subscriptions.
3. Send subscription requests from your client to your new router endpoint using[HTTP multipart](https://www.apollographql.com/docs/router/executing-operations/subscription-multipart-protocol/) . (Apollo Client[Web](https://www.apollographql.com/docs/react/data/subscriptions#http) ,[Kotlin](https://www.apollographql.com/docs/kotlin/essentials/subscriptions#configuring-http-subscriptions) , and[iOS](https://www.apollographql.com/docs/ios/fetching/subscriptions#http) have built-in support)


### Federated subscriptions with GraphOS Enterprise


Here’s what you’ll need to use federated subscriptions with self-hosted instances of Apollo Router:


1. An active Apollo GraphOS Enterprise plan
2. A federated GraphQL API with


- One or more[federation-compatible GraphQL servers](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs/) with subscription support using the` graphql-ws protocol` or` subscriptions-transport-ws protocol` )
- The[latest version](https://github.com/apollographql/router/releases) of the Apollo Router


3. The latest version of Apollo Client (Web, iOS, or Kotlin) with HTTP-based subscriptions support.


If you don’t have a GraphOS Enterprise plan and you need to self-host the Apollo Router for an enterprise-scale deployment,[try out GraphOS Enterprise](https://studio.apollographql.com/signup?type=enterprise-trial&referrer=blog) for free for 28 days.


## Let us know what you think!


We’re excited for you to try out federated subscriptions in GraphOS and would love to hear what you think. If you have any questions or feedback for the team, feel free to reach out to us on our[community forum](https://community.apollographql.com/) or in our[Discord](https://discord.com/invite/graphos) !


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
