---
schema_version: "1.0.0"
document_id: "5c1b8556f92378d229c37644c00d07237b4523d4c0b87f45607c1fc969e81a53"
company_key: "acv-auctions-inc-class-a-common-stock"
company: "ACV Auctions Inc."
source_id: "acv-auctions-inc-class-a-common-stock-rss-d00f1f014d5a"
canonical_url: "https://acv.engineering/posts/graphQL-subscriptions-in-go/"
published_at: "2024-11-25T06:00:00+00:00"
first_seen_at: "2026-07-20T23:17:32.344046+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:7f08b78a37b78897fce4eec01d52b183dce7d4b107dad57a48b8c6ba405244c2"
---

# Building Scalable GraphQL Subscriptions in Go for Real-Time Applications

# Introduction


## What Are GraphQL Subscriptions?


GraphQL subscriptions are a feature of the GraphQL specification that enable real-time data transfer from a server to a client. While typical GraphQL queries and mutations operate on a request-response model, subscriptions are unique in allowing a server to push data to clients actively. This capability makes subscriptions ideal for use cases where clients need to receive continuous, real-time updates based on certain events or data changes.


### Key Characteristics of GraphQL Subscriptions:


- Event-Driven: Subscriptions are designed to respond to specific events. When an event occurs (like a new bid placed on auction or a stock price update), the server broadcasts the event data to all clients that have subscribed to this event.
- Persistent Connection: Unlike the traditional request-response model, subscriptions typically rely on persistent connections, often using WebSockets or Server-Sent Events (SSE), to maintain an open channel between the server and the client.
- Real-Time Communication: This persistent connection allows the server to instantly notify clients as soon as data changes, enabling a seamless real-time user experience.


### Common Use Cases:


- Real-Time Notifications: Subscriptions are perfect for notifying users about new activity like new followers, likes, auction bid activities — as soon as they happen.
- Live Chat Applications: As users send messages, each participant in the chat room receives updates instantly, ensuring that conversations are fluid and responsive.
- Collaborative Applications: In applications like collaborative documents, multiple users can edit simultaneously and see each other’s changes in real-time.
- Live Streaming Data: For applications such as stock trading or monitoring sensor data in real time, subscriptions allow users to receive constant updates, which is critical for decision-making.


## How Subscriptions Work in GraphQL


- GraphQL Subscriptions use a Publisher-Subscriber (PubSub) model: clients subscribe to certain data changes, the server monitors data sources, and when an event occurs, it broadcasts the update to all subscribed clients.
- Basic Flow and Subscription Lifecycle:
1. **Client Connection:** The client establishes a WebSocket connection with the server.
2. **Subscription Registration:** The client sends a subscription request, specifying the data or events they are interested in.
3. **Event Listening:** The server monitors for relevant data changes or events.
4. **Data Delivery:** When a matching event occurs, the server pushes data to the client via the open connection.
5. **Transport Protocols:** GraphQL subscriptions typically use WebSockets for two-way communication, but Server-Sent Events (SSE) can also be used, particularly for one-way updates.


# Why Golang for GraphQL Subscriptions?


Golang is a powerful choice for implementing scalable GraphQL subscriptions, especially for applications requiring real-time, high-frequency updates. One of Golang’s standout features is its high performance and efficient concurrency model. Unlike traditional threading, Go’s lightweight goroutines allow it to handle thousands of concurrent connections with minimal memory and CPU usage, making it well-suited for scenarios where multiple clients need real-time data updates with low latency. This efficiency in resource management ensures that applications can scale smoothly without sacrificing performance, even under heavy loads.


The Go runtime is specifically optimized for managing I/O-bound tasks, such as WebSocket connections essential for GraphQL subscriptions. Its scheduler efficiently allocates resources, maintaining high throughput and responsiveness even under significant traffic. This capability is crucial for delivering interactive, real-time user experiences in applications like chat systems, notifications, and live updates.


Moreover, Golang’s rich library ecosystem enhances its suitability for building robust GraphQL subscriptions. Key libraries include:


- **gqlgen** : simplify GraphQL schema creation and server implementation.
- **gorilla/websocket** : offers robust WebSocket management.
- **go-redis** : integrates Redis PubSub for reliable message broadcasting


# Redis PubSub as a Message Broker


In our GraphQL subscription server, Redis PubSub serves as the backbone for delivering real-time data updates to clients. As a lightweight and efficient message broadcasting system, Redis PubSub facilitates seamless communication between multiple applications by acting as a centralized message broker — ensuring that updates are reliably delivered in real time.


### How Redis PubSub Works


Redis PubSub operates on a classic Publisher-Subscriber model:


- Publisher: The publisher sends messages to a specific channel. In our application, this is the source of data updates — such as new auction bid events or auction ended events.
- Subscriber: Clients subscribe to one or more channels to receive relevant updates. In our application, each GraphQL subscription acts as a subscriber, waiting for updates on specific channel.


### Redis PubSub Advantages for GraphQL Subscriptions


Redis PubSub brings a range of benefits that make it a powerful choice for implementing GraphQL subscriptions:


-


Real-Time Data Delivery: Redis PubSub excels in low-latency message broadcasting, ensuring clients receive updates instantly as data changes. This makes it ideal for dynamic, real-time applications requiring responsive user experiences.


-


Scalability: Redis efficiently handles a high volume of publishers and subscribers without degrading performance. In large-scale deployments, Redis can distribute the workload across clusters, processing thousands of messages per second and supporting numerous concurrent clients.


-


Seamless Integration: With Golang libraries like go-redis, integrating Redis PubSub is straightforward. Developers can easily publish and subscribe to channels directly from Go applications, simplifying implementation.


-


Fire-and-Forget Mechanism: Redis PubSub follows a “fire-and-forget” model where messages are delivered only to active subscribers. If no subscribers are present, messages are discarded without error. Subscribers can also listen on empty channels, waiting for future messages, ensuring flexibility and resilience in the messaging process.


# Setting Up GraphQL Subscriptions for ACV Marketplace


## Implementation Highlights


To enable real-time GraphQL subscriptions in Go, we leverage the[gqlgen](https://acv.engineering/posts/graphQL-subscriptions-in-go/github.com/99designs/gqlgen) library. This tool simplifies the process by defining GraphQL schemas and generating the essential server components, including resolvers. Combined with Redis PubSub for live data updates, this setup ensures a robust and efficient architecture for delivering dynamic real-time experiences.


### Step 1: Define the GraphQL Schema


Start by creating a schema.graphql file. Here, we define the Auction type and a subscription called auctionLiveUpdates. This subscription will push updates to clients whenever data for specified auction IDs changes.


```text
type Auction {
auction_id Int!
make: String!
model: String!
status: String!
}


type Subscription {
auctionLiveUpdates(auction_ids: [Int!]!): Auction!
}


```


The auctionLiveUpdates subscription accepts a list of auction IDs as input, allowing clients to subscribe to updates for specific auctions.


### Step 2: Generate Server Boilerplate Code


Once the schema is defined, run the following command to generate the necessary server boilerplate code:


```text
go run github.com/99designs/gqlgen generate


```


This command will create the Go code required to manage GraphQL queries and subscriptions. It generates types, resolvers, and setup files, streamlining the development process and saving time on manual configurations.


### Step 3: Implement the Subscription Resolver


With gqlgen, the resolvers are automatically scaffolded in resolver.go. This file is where you’ll implement custom logic to handle real-time data updates from Redis.


Below is an example of the subscription resolver for auctionLiveUpdates. Here, we listen to a Redis channel and push any received updates to subscribed clients.


```text
func (r *subscriptionResolver) AuctionUpdates(ctx context.Context, auctionIds []int ) (<-chan *model.Auction, error) {
auctionUpdateChannel := make(chan *model.Auction)


go func() {
pubsub := r.RDB.Subscribe(ctx, "auction:<auction_id>")
defer pubsub.Close()


for {
select {
case <-ctx.Done():
close(auctionUpdateChannel)
return
case msg := <-pubsub.Channel():
auction := &model.Auction{
ID:   msg.Payload
}
auctionUpdateChannel <- auction
}
}
}()


return auctionUpdateChannel, nil
}


```


#### Explanation of the Code:


- This resolver sets up an auctionUpdateChannel for sending notifications to clients.
- go func() starts a goroutine that listens for new messages and pushes them into auctionUpdateChannel whenever they’re received on a Redis PubSub channel.
- ctx.Done() check ensures that when the client unsubscribes, the goroutine is cleaned up to free resources.
- Message Handling: When a message is received on the channel, we parse it into an Auction model. The parsed message is then pushed to the auctionUpdateChannel, which the gqlgen library uses to send real-time updates over WebSockets to clients.


### Step 4: Start the GraphQL server with WebSocket support


```text
func main() {


srv := handler.NewDefaultServer(generated.NewExecutableSchema(generated.Config{Resolvers: &graph.Resolver{}}))


srv.AddTransport(&transport.Websocket{})


http.Handle("/", playground.Handler("GraphQL playground", "/query"))
http.Handle("/query", srv)


log.Fatal(http.ListenAndServe(":"+8080, nil))
}


```


# Benefits of Using GraphQL Subscriptions in Real-Time Applications


GraphQL subscriptions offer several distinct advantages in applications that require real-time interactivity. Below are some key benefits of using subscriptions, especially when building responsive, high-performance applications.


### Reduced Client Polling and Network Traffic


One of the primary benefits of GraphQL subscriptions is their ability to push updates only when data changes, eliminating the need for constant polling. Traditional methods often require clients to repeatedly query the server to check for updates, which consumes bandwidth and creates latency. With subscriptions, the server actively notifies clients as soon as an event occurs, delivering updates only when they’re relevant. This approach significantly reduces network traffic, making applications more efficient and responsive.


### Improved User Experience with Real-Time Data


Subscriptions enhance the user experience by making applications feel live and interactive. With instant data updates, users experience an app that is more responsive and engaging, creating a seamless interface where actions and data appear immediately. Real-time updates make applications feel faster, which can enhance user satisfaction and retention, especially in competitive environments where speed and interactivity are key.


# Conclusion and future work


Using GraphQL subscriptions in Golang with Redis PubSub is a powerful solution for building high-traffic, low-latency, real-time applications. However, several challenges remain that require careful attention and future work.


1. Network Reliability


- Issue: Although WebSockets provide reliable real-time communication, they are vulnerable to network disruptions, particularly in mobile environments where connections can be unstable.
- Strategy: To address this, we should consider implementing reconnection strategies with exponential backoff. This approach will help manage temporary network interruptions and improve the overall stability of real-time updates for end users.


2. Efficient Data Synchronization


- Issue: When updates are frequent and high in volume, the server must balance delivering real-time updates with minimizing excessive data transfer.
- Strategy: Implement throttling, batching or data aggregation mechanisms to minimize the volume of data pushed to clients.


# References:


- Gqlgen subscriptions - https://gqlgen.com/recipes/subscriptions/
- Gqlgen repo - github.com/99designs/gqlgen
- Apollo subscriptions - https://www.apollographql.com/docs/react/data/subscriptions


UPDATE (September 2025): check out our[follow-up post on queries here!](https://acv.engineering/posts/graphQL-queries-in-go/)


-
-
-
-
