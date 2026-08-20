---
schema_version: "1.0.0"
document_id: "9b67cbfb60e167fbdff9951c512e91561405a05c05a653a1c2e0359150d8cae2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/reduce-real-time-cloud-spend-with-federated-subscriptions-support-for-http-callbacks"
published_at: "2024-02-01T10:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:06955c79b4a331638dd9dde931fa3d62a02337586bbbda47da86876c20bd5e5b"
---

# Reduce real-time cloud spend with Federated Subscriptions support for HTTP Callbacks

Modern apps must support real-time use cases like flight changes, team collaboration, or live score updates for the big game. GraphQL subscriptions push events in a more efficient and performant way compared to traditional client-side polling of RESTful services that try to pull new events every few seconds. Instead of polling, clients can subscribe to the specific data they require, and the graph immediately delivers those events as they occur, reducing backend load and providing faster real-time updates.


Traditionally, GraphQL subscriptions have been implemented with WebSockets, an approach with many issues, including compression, debugging, scalability, and compatibility with firewalls, load-balancers, and other networking infrastructure.


We addressed the frontend portion of the WebSocket problem in June 2023, when we announced[Federated subscriptions in GraphOS](https://www.apollographql.com/blog/federated-subscriptions-in-graphos-real-time-data-at-scale) with support for a new[HTTP multipart protocol](https://www.apollographql.com/docs/react/data/subscriptions/#subscriptions-via-multipart-http) based on the underlying protocol for GraphQL @defer support in[Apollo Router](https://www.apollographql.com/docs/router/executing-operations/defer-support/) . This makes hosting a real-time API in any cloud environment easier, with all the benefits of a standard HTTP connection.


However, on the backend, Apollo Router supports legacy WebSocket protocols for compatibility with existing subgraph APIs, which is inefficient at scale. For example, if a subscription can’t be[deduplicated](https://www.apollographql.com/docs/router/executing-operations/subscription-support/#subscription-deduplication) , 1 million client subscriptions would require 1 million backend WebSockets connections.


Today, we are excited to announce the GA release of Apollo Router 1.37 with[HTTP callback support](https://www.apollographql.com/docs/router/executing-operations/subscription-callback-protocol/) , designed in partnership with the GraphQL community. HTTP callback support solves the backend WebSocket problem and dramatically reduces the number of open subgraph connections. This results in the same great performance but with more efficiency at scale and reduces real-time cloud spend for router and subgraph deployments.


Subscription callbacks offer new levels of flexibility and interoperability between the router and underlying services. By default, federated subscriptions use WebSockets, which requires two long-lived connections for every client: one between the client and the router and one between the router and the subgraph. Additionally, these long-lived connections aren’t well suited for serverless infrastructures.


With HTTP callbacks, on the other hand, the router no longer manages a WebSocket connection with subgraphs, and it’s flexible enough to be used with other Pub/Sub architectures that can support HTTP requests.


## HTTP Callbacks for Federated Subscriptions


Here’s what you’ll need to use federated subscriptions with self-hosted instances of Apollo Router:


1. An active Apollo GraphOS Enterprise plan
2. A federated GraphQL API with


1. One or more HTTP callback-compatible GraphQL servers, such as[Apollo Server](https://www.apollographql.com/docs/apollo-server/api/plugin/subscription-callback) >= 4.10.0 or[Spring GraphQL](https://github.com/apollographql/federation-jvm/tree/main/spring-subscription-callback#usage) >= 4.3.0
2. [Apollo Router v1.37](https://github.com/apollographql/router/releases/tag/v1.37.0) or greater


3. The latest version of Apollo Client (Web, iOS, or Kotlin) with HTTP-based subscriptions support.


If you don’t have a GraphOS Enterprise plan and you need to self-host the Apollo Router for an enterprise-scale deployment, try out[GraphOS Enterprise](https://studio.apollographql.com/signup?type=enterprise-trial&referrer=blog) for free for 28 days.


### Migrating from the preview to GA


If your application already uses the preview of subscription callbacks, there are a few things to change in your Router configuration to migrate to the GA:


1. Change the name of the option from` subscription.mode.preview_callback` to` subscription.mode.callback` . Failure to use the updated option name when running the router will result in an error, and the router won’t start.
2. Update the` public_url` field to include the full URL of your callback endpoint. Previously, in the preview, the public URL used by the router was the automatic concatenation of the` public_url` and` path` fields. In GA, the behavior has changed, and the router uses the value set in` public_url` . This enables you to configure your public URL, for example, if you have a proxy in front of the router and want to configure redirection with the public URL.
3. Configure the new` heartbeat_interval` field to set the period that a heartbeat must be sent to the callback endpoint for the subscription operation. The default heartbeat interval is 5 seconds. You can turn off heartbeats, for example, if you’re operating in a serverless architecture without long-lived processes, by setting` heartbeat_interval: disabled` .


## Try it today!


We’re excited for you to try[HTTP callbacks in GraphOS](https://www.apollographql.com/docs/router/executing-operations/subscription-callback-protocol/) and would love to hear your thoughts. If you have any questions or feedback for the team, feel free to reach out to us in our[Discord](https://discord.com/invite/graphos) .


Written by


Andrew I. Carlson


[Read more by Andrew I. Carlson](https://www.apollographql.com/blog/author/andrewcarlson)
