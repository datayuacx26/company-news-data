---
schema_version: "1.0.0"
document_id: "d0528e1efaf429c18d229f60564ed1430d00dcea2b31787b64c69a061f6bb222"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/tutorial-graphql-subscriptions"
published_at: "2017-06-20T22:47:49+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:211866c132258b51dd10525bc192cd6b1034b1548a9d0036a2cbbba042ddd57a"
---

# Tutorial: GraphQL Subscriptions on the Server

This is part 6 of our Full-Stack GraphQL + React Tutorial, which guides you through creating a messaging application. Each part is self-contained and focuses on a few new topics, so you can jump directly into a part that interests you or do the whole series. Here’s are the other parts of the tutorial:


- [Part 1: Setting up a simple client](https://dev-blog.apollodata.com/full-stack-react-graphql-tutorial-582ac8d24e3b)
- [Part 2: Setting up a simple server](https://dev-blog.apollodata.com/react-graphql-tutorial-part-2-server-99d0528c7928)
- [Part 3: Writing mutations and keeping the client in sync](https://dev-blog.apollodata.com/react-graphql-tutorial-mutations-764d7ec23c15)
- [Part 4: Optimistic UI and client side store updates](https://dev-blog.apollodata.com/tutorial-graphql-mutations-optimistic-ui-and-store-updates-f7b6b66bf0e2)
- [Part 5: Input Types and Custom Resolvers](https://dev-blog.apollodata.com/tutorial-graphql-input-types-and-client-caching-f11fa0421cfd)
- Part 6: Server-side Subscriptions (right now)
- [Part 7: GraphQL Subscriptions on the Client](https://dev-blog.apollodata.com/tutorial-graphql-subscriptions-client-side-40e185e4be76)
- [Part 8: Pagination](https://dev-blog.apollodata.com/tutorial-pagination-d1c3b3ee2823)


---


In this tutorial, we are going to go through the process of adding GraphQL Subscriptions to our server. In Part 5, we added the concept of a message and implemented a detailed channel view in the client to display messages in each channel. But right now, messages do not sync across clients, because there’s no way for the server to notify clients that new messages have been added.


Just like many other applications, our messaging app needs to have real-time updates for some features, so in this tutorial we’ll build out the server-side logic to enable our client to show new messages in real time thanks to GraphQL Subscriptions. If you want to dive into the details of how subscriptions work, check out[this blog post](https://dev-blog.apollodata.com/graphql-subscriptions-in-apollo-client-9a2457f015fb) introducing Apollo Client’s support for subscriptions and the[Request For C0mments](https://github.com/facebook/graphql/blob/master/rfcs/Subscriptions.md) to add subscriptions to the GraphQL specification.


---


Let’s get started by cloning the Git repo and installing the dependencies:


```text
git clone  https://github.com/apollographql/graphql-tutorial.git
cd graphql-tutorial
git checkout t6-start
cd server && npm install
cd ../client && npm install
```


First off, let’s make sure everything works by starting the server and client.


In one terminal session we launch the server, which will run on port 4000:


```text
cd server
npm start
```


And in another session we launch the client, which will run on port 3000:


```text
cd client
npm start
```


When you navigate your browser to[http://localhost:3000](http://localhost:3000/) , you should enter the homepage of our messaging app, which has a list of channels that the user has created. Click on one of the channels, and you’ll be taken to the detail view that we created in the last part, where you can add new messages in the channel.


What we’ve built so far


We won’t be writing any client-side code in this tutorial, but we’ll use the client UI to insert messages for testing our subscription implementation.


## GraphQL Subscriptions


To notify the client when messages are added to a channel, we’ll use GraphQL subscriptions, which allow the client to make a query and be notified of new results in the case of specific, server-side events. In our implementation of the server, we’ll use an Express server with WebSockets for pushing updates to the client.


In this tutorial, we’ll first add a subscription that notifies the client about new messages. Next, we’ll add a field to our GraphQL schema and implement a resolver for the subscription. Finally, we’ll use an in-memory pub-sub object that will handle passing along notifications about added messages. Together, the flow of message creations and subscription notifications will look something like this:


The data flow in an app using GraphQL subscriptions.


Let’s dive right in by adding a subscription for listening to message additions to our server-side schema! To the end of our GraphQL schema in` server/src/schema.js` we add a new root type of` Subscription` that sits on the same level as` Query` and` Mutation` . This root type contains` messageAdded(channelId: ID!)` , a field that represents a topic a client can listen to to be notified of messages added to a specific channel. All together, we’ve added:


```text
// server/src/schema.js
type Subscription {
messageAdded(channelId: ID!): Message
}
```


Our schema now gives us a solid reference to follow as we continue to implement subscriptions.


## Adding a Subscription Resolver


Now that we have a schema defining which subscriptions clients can ask for, the next step is to allow events to be sent to the subscription. To keep things simple, we’ll implement our subscription model with the in-memory` PubSub` system from the` graphql-subscriptions` package. We’ll also need to use the` withFilter` helper from the same package to split up events by which channel they are for. In` server/src/resolvers.js` we’ll start by adding the necessary import statements.


```text
// server/src/resolvers.js
import { PubSub, withFilter } from ‘graphql-subscriptions’;
```


Next, we’ll construct an instance of` PubSub` to handle the subscription topics for our application. We’ll add this instance right before we define` resolvers` , where we’ll need to use the instance to produce events when messages are created.


```text
const pubsub = new PubSub();  export const resolvers = {
```


Once we have our subscription manager set up, we need to publish messages into it! With the` PubSub` class, this is as simple as calling` pubsub.publish(topic, data)` . For our app, we’ll publish each new message to the` messageAdded` topic along with an additional property that carries the channel ID (this will become more important later in the tutorial). One thing to note: the topic name doesn’t have to match the subscription name; we just used the same name here to keep things simple.


```text
addMessage: (root, { message }) => {
const channel = channels.find(channel => channel.id ===
message.channelId);
if(!channel)
throw new Error(“Channel does not exist”);  const newMessage = { id: String(nextMessageId++), text: message.text };
channel.messages.push(newMessage);   pubsub.publish(‘messageAdded’, { messageAdded: newMessage, channelId: message.channelId });    return newMessage;
}
```


Next, let’s use the published events to resolve subscription queries! With the` graphql-subscriptions` package, this is super easy. We set up our nested objects in` resolvers` just like we normally would, instead of returning an object at the end, we return an` AsyncIterator` that will emit the messages to send over to the client. Since the` messageAdded` topic contains events for *all* channels, we also use the` withFilter` function we imported earlier to save resources. This will filter events to select only those for the channel specified in the query. When using` withFilter` , the first argument is a function that returns the` AsyncIterator` we are filtering. The second argument is a condition that specifies if an event should pass through the filter given the event data and query variables.


```text
Subscription: {
messageAdded: {
subscribe: withFilter(
() => pubsub.asyncIterator(‘messageAdded’),
(payload, variables) => {
return payload.channelId === variables.channelId;
}
)
}
}
```


And that’s all we need for resolving a GraphQL Subscription query!


## WebSocket Transport for Subscriptions


The last step in this tutorial is to add subscription support to our GraphQL server through WebSockets, since we can’t push frequent updates from the server to the client over HTTP. Thanks to` subscriptions-transport-ws` package, this is super simple! First, let’s start by adding the necessary import statements in` server/server.js`


```text
import { execute, subscribe } from ‘graphql’;
import { createServer } from ‘http’;
import { SubscriptionServer } from ‘subscriptions-transport-ws’;
```


Next, we can open the WebSocket in our GraphQL server. We do this in two steps: first wrapping the Express server with` createServer` , and then using the wrapped server to set up a WebSocket to listen to GraphQL subscriptions.


```text
// Wrap the Express server
const ws = createServer(server);ws.listen(PORT, () => {
console.log(`GraphQL Server is now running on http://localhost:${PORT}`);  // Set up the WebSocket for handling GraphQL subscriptions
new SubscriptionServer({
execute,
subscribe,
schema
}, {
server: ws,
path: '/subscriptions',
});
});
```


Next, we configure Graph *i* QL to use the subscriptions WebSocket we just set up.


```text
server.use('/graphiql', graphiqlExpress({
endpointURL: '/graphql',
subscriptionsEndpoint: `ws://localhost:4000/subscriptions`
}));
```


And with that, our server is ready for action! To try it out, we can open GraphiQL at[http://localhost:4000/graphiql](http://localhost:4000/graphiql) and run the following query


```text
subscription {
messageAdded(channelId: 1) {
id
text
}
}
```


When you run the query, you should see a message that looks like


```text
"Your subscription data will appear here after server publication!"
```


Graph *i* QL is now listening for the creation of a new message, which we can trigger by creating a message in the client. Because we are only listening to channel 1, make sure to navigate to the first channel in the client (or directly point your browser to[http://localhost:3000/channel/1](http://localhost:3000/channel/1) ). When you create a new message, you should see it instantly show up in your Graph *i* QL window!


It works!


## Conclusion


Hooray! You’ve now implemented the server-side portion of GraphQL Subscriptions by adding a subscription type to your schema and implementing a subscription transport through WebSockets! With a couple changes to our client, which will be explained in the[next tutorial](https://dev-blog.apollodata.com/tutorial-graphql-subscriptions-client-side-40e185e4be76) , our client will be able to see message additions in near real-time.


If you liked this tutorial and want to keep learning about Apollo and GraphQL, make sure to click the “Follow” button below, and follow us on Twitter at[@apollographql](https://twitter.com/apollographql) and the author at[@ShadajL](http://twitter.com/shadajl) .


Thanks to my mentor,[Jonas Helfer](https://medium.com/u/39cbd38c57c8?source=post_page-----e51c32dc2951----------------------) , for his support as I wrote this post!


Written by


Shadaj Laddad


[Read more by Shadaj Laddad](https://www.apollographql.com/blog/author/shadaj)
