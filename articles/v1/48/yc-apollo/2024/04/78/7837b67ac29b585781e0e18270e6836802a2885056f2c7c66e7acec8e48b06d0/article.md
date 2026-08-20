---
schema_version: "1.0.0"
document_id: "7837b67ac29b585781e0e18270e6836802a2885056f2c7c66e7acec8e48b06d0"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/federated-subscriptions-with-routers-http-callback-protocol"
published_at: "2024-04-30T09:17:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:6b988c8b2d36a077be2d534650837ce71e3aed0ec5d84471569c1fd7edf3d74f"
---

# Federated Subscriptions with Router’s HTTP Callback Protocol

Federated subscriptions through GraphOS make the path to real-time data much smoother: a single query delivers continuous updates while the Apollo Router coordinates behind-the-scenes operations to subgraphs.


In addition to enabling subscriptions via WebSockets, the router supports an HTTP callback-based protocol. With callbacks, the router can communicate with subgraphs without needing to maintain multiple persistent connections for each new subscription event.


This article explores how the router orchestrates subscriptions over HTTP and uses a single subscription operation to query fresh data from two services.


*Note: If you’d like to follow along,[check out the repository for the demo code](https://github.com/apollographql-education/subscriptions-demo/tree/main) and complete the steps outlined in the README.*


## Real-time data: stock prices


Consider an example graph with two subgraphs:` stocks` and` prices` .


The` stocks` service provides basic data for a` Stock` type. A stock has three fields:` id` ,` name` , and` latestNews` , which returns a recent news headline about the stock.


```text
​​type Stock @key(fields: "id") {
id: ID!
name: String!
latestNews: String
}
```


Note that the` Stock` type is an entity, which means it has fields defined in multiple subgraphs. The` @key` on the` Stock` type tells the router which field to use when syncing up data about a single` Stock` instance from multiple subgraphs.


*To learn more about Apollo Federation and building a graph from the ground up, check out[Federation with TypeScript & Apollo Server](https://www.apollographql.com/tutorials/federation-typescript) on Odyssey, Apollo’s official learning platform.*


Because` Stock` is an entity type, the` prices` subgraph can contribute its own field,` currentPrice` , to the` Stock` type.


```text
type Stock @key(fields: "id") {
id: ID!
currentPrice: Int
}
```


To subscribe to changes in a stock’s price, we’ll use the` stockPriceChange` subscription field from the` prices` service.


```text
type Subscription {
stockPriceChange(id: ID!): Stock
}
```


This field accepts an` id` argument for a particular stock, and returns a` Stock` type. This means we can build our subscription operations to include any of the` Stock` type fields—not just the` currentPrice` field that we know will change.


Here’s an example of a subscription operation that requests data about a stock from both subgraphs.


```text
subscription Subscription($stockId: ID!) {
stockPriceChange(id: $stockId) {
name                 # provided by the `stocks` service
latestNews           # provided by the `stocks` service
currentPrice         # provided by the `prices` service
}
}
```


Before walking through how the router resolves this operation, let’s dive into how subscriptions with HTTP callbacks work.


## The HTTP callback protocol


With HTTP callbacks, subgraph servers use a callback URL provided by the router to send new subscription data as soon as it becomes available. This process occurs without maintaining the long-lived, resource-intensive connections common with the WebSocket approach.


You can picture the process a bit like setting up channels for radio communication: the participants note where to reach each other and first make sure everything’s working (“Do you read me?”). They check in periodically with each other (“Are you still there?”) and, with any luck, share updates as they happen (“I have news!”). When the subscription is terminated or one of the parties stops responding, the channel shuts down (“Over and out”).


Metaphors aside, let’s dive into the technical details. Three primary roles make the process possible: the **router** , the **subgraph** , and the **emitter** .


When it receives a subscription operation, the **router** sends it onward to the responsible **subgraph** , attaching instructions for how the subgraph can “call back.” The **emitter** acts as the mechanism that checks in with the router periodically and “emits” new subscription data. For the purposes of this article, the subgraph plays both **subgraph** and **emitter** roles, though they can be separate systems as needed.


### Configuring the router


The process starts in the router’s` router.yaml` file. This is the configuration file where we can provide all kinds of details about how the router should run—including how it should handle subscription events and which protocol to use for which subgraph.


```text
subscription:
enabled: true
mode:
callback:
public_url: http://127.0.0.1:4000/callback
listen: 127.0.0.1:4000
path: /callback
heartbeat_interval: 15s
subgraphs:
- prices
```


*To explore all possible configuration options in the` router.yaml` file, check out[the official documentation](https://www.apollographql.com/docs/router/configuration/overview/#yaml-config-file) .*


The first four lines in the configuration snippet above enable subscriptions in the router and set the mode as` callback` . All the callback-specific configurations are below the` callback` key. Let’s break them down, one by one.


#### listen


The` listen` property specifies the IP address and port the router will listen on for subscription callbacks. We haven’t changed it from the default here, but note that for security reasons it’s likely best to expose a separate port that’s available only through your internal network.


#### path


The` path` property states the specific path of our router’s callback endpoint. It should match the value appended to the` public_url` . If you don’t specify a` path` , it takes the value of` /callback` by default.


#### heartbeat_interval


` heartbeat_interval` is optional. By default, the router asks the subgraphs to send a “check” message every 5 seconds. Here we’ve lengthened the intervals to 15 seconds for demo purposes.


#### subgraphs


Lastly, under` subgraphs` , we specify the names of the subgraphs that use the HTTP callback protocol to provide subscription data. We have just one:` prices` .


### Starting the subscription


Let’s look at an example operation the router might receive from a client.


```text
subscription GetStockPriceUpdates {
stockPriceChange(id: "1") {
currentPrice
}
}
```


The` prices` subgraph is responsible for the` stockPriceChange` subscription field, so the router sends it an initial data object. This object contains the subscription operation itself and all the information the subgraph will need to be able to “call back.”


This information includes the` callbackUrl` ,` subscriptionId` , and` heartbeatIntervalMs` , along with a new property called` verifier` .


```text
{
"query": "subscription GetStockPriceUpdates { stockPriceChange(id: "1") { currentPrice }}",
"extensions": {
"subscription": {
"callbackUrl": "http://127:0.0.1:4000/callback/c4a9d1b8-dc57-44ab-9e5a-6e6189b2b945",
"subscriptionId": "c4a9d1b8-dc57-44ab-9e5a-6e6189b2b945",
"verifier": "XXX",
"heartbeatIntervalMs": 15000
}
}
}
```


The` verifier` property verifies the subgraph’s identity with the router; each response the subgraph sends to the router needs to include this property. When it receives this object, the subgraph server uses the` verifier` property right away and sends the router its first message, establishing that it can make contact. (“Do you read me?”)


When the router confirms (“Copy,` prices` subgraph, I read you”), the subgraph sends an empty GraphQL response object. With that, setup is complete—we’re ready to receive subscription updates!


### The loop


The subscription operation has been dispatched, and the router’s waiting to receive some new data. What’s happening under the hood? A few things:


1. Every fifteen seconds, (or whatever we set as the subscription’s` heartbeat_interval` ), the subgraph acting in its emitter role sends a “check” message to the router. (“Just checking in!”)
2. When the subgraph detects a subscription event, it sends the router a “next” message containing the new data. (“Here’s the latest…”)
3. If something goes wrong or the subscription should be terminated, the subgraph sends a “complete” message, and the router ends the subscription. (“Over and out!”)


Without any changes to the stock’s current price, this loop continues with the subgraph sending its regular “check” message to the router.


When we trigger a mutation that changes the stock’s price, however, the router sends a response containing the new data.


Updating the stock’s price caused our` prices` service to emit a “next” message to the router. The router packages up the new data and serves it back to the client. Then, the main loop continues: the subgraph will continue to check in at regular intervals, sending data when it becomes available.


## Subscriptions and the query plan


In[GraphOS Explorer](https://www.apollographql.com/docs/graphos/explorer/) , we get a sneak peek at the router’s plan of action to fulfill a query. By selecting *Query Plan Preview* from the *Response* panel dropdown, we see precisely where the router needs to fetch data from.


With a subscription operation that asks for a single field,` currentPrice` , the query plan is simple: the router just needs to fetch data from` prices` .


But what happens when our subscription response includes more fields from the` Stock` type?


Let’s update our query to include the` latestNews` and` name` fields from the` stocks` subgraph.


```text
subscription Subscription($stockId: ID!) {
stockPriceChange(id: $stockId) {
name                 # provided by the `stocks` service
latestNews           # provided by the `stocks` service
currentPrice         # provided by the `prices` service
}
}
```


When we check out the *Query Plan Preview* , we’ll see that the router has planned another stop in its itinerary. After fetching the` currentPrice` field from the` prices` subgraph, it then retrieves the remaining fields (` name` and` latestNews` ) from the` stocks` subgraph.


Though our subscription operation is primarily concerned with a single field—namely, the new value of` currentPrice` after a price-changing event—the router ensures that the remaining fields queried for the` Stock` type come along for the ride.


We’ll see this in action by triggering another` changePrice` mutation. We’ll see data for all three of our` Stock` fields appear in the *Response* panel.


We’ll run one more mutation, changing the price a final time.


And we’ll see not only a new value for` currentPrice` , but a new` latestNews` value as well!


Here’s what’s happening:


1. The` prices` subgraph sends the router an update that the stock’s price has changed, along with the value of` currentPrice` .
2. The router has all of the data it needs to return from the` prices` subgraph, but the subscription operation it’s executing includes two fields from the` stocks` subgraph as well:` name` and` latestNews` .
3. The router executes a regular GraphQL query to the` stocks` subgraph to get the value of the` name` and` latestNews` fields.
4. Now equipped with all the data from *both* subgraphs, the router bundles it up and sends it back to the client.


Notice that we haven’t explicitly subscribed to the` latestNews` field from the` stocks` subgraph, yet the router still *refetches* this field when a stock’s price changes. As a result, we get the latest data from both subgraphs—all from a single subscription operation!


#### A note on deduplication


By default, the router will not open a new subscription when it’s already handling an identical operation. By deduplicating both subscription operations from clients, and the requests to the subgraphs that fulfill the queried data, the router keeps stress to a minimum.


## TL;DR


The router supports subscriptions using the HTTP callback protocol. This allows the router and subgraphs to communicate relevant updates without the need for long-lived, persistent connections. Currently this feature is available out of the box with Apollo Server. To take advantage of this new feature, other subgraph libraries or frameworks should implement the HTTP callback protocol such that it can do the following:


1. Establish a connection with the router using a` callbackUrl` and a` verifier` .
2. Send regular “check” messages to the router at a predefined interval.
3. Provide the router with subscription updates via a “next” message.
4. Close a subscription connection with the router.


Taken altogether, we get the power of federated subscriptions across the graph without the weight of multiple, long-term WebSocket connections. We can send the router a subscription operation that requests data from multiple subgraphs, and the router takes care of fetching those updates when anything changes. This allowed us to subscribe to stock price changes in one subgraph, and receive updates for all the fields in our operation.


Check out our[demo code](https://github.com/apollographql-education/subscriptions-demo/tree/main) to give subscriptions with HTTP callbacks a try!


Written by


Liz Hennessy


[Read more by Liz Hennessy](https://www.apollographql.com/blog/author/liz)
