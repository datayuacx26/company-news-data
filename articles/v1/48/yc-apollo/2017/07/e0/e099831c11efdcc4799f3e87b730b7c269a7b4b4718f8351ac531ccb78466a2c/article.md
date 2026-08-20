---
schema_version: "1.0.0"
document_id: "e099831c11efdcc4799f3e87b730b7c269a7b4b4718f8351ac531ccb78466a2c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/tutorial-graphql-subscriptions-client-side"
published_at: "2017-07-07T18:28:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:1d97feb1111c149eb2687a1339c75f63a471cf5c552d39147205807568e4415a"
---

# Tutorial: GraphQL subscriptions with Apollo Client

This is part 7 of our full-stack GraphQL + React Tutorial that guides you through creating a messaging application. Each part is self-contained and focuses on a few new topics, so you can jump directly into a part that interests you or do the whole series. Here’s what we have covered so far:


- [Part 1: Setting up a simple client](https://dev-blog.apollodata.com/full-stack-react-graphql-tutorial-582ac8d24e3b)
- [Part 2: Setting up a simple server](https://dev-blog.apollodata.com/react-graphql-tutorial-part-2-server-99d0528c7928)
- [Part 3: Writing mutations and keeping the client in sync](https://dev-blog.apollodata.com/react-graphql-tutorial-mutations-764d7ec23c15)
- [Part 4: Optimistic UI and client side store updates](https://dev-blog.apollodata.com/tutorial-graphql-mutations-optimistic-ui-and-store-updates-f7b6b66bf0e2)
- [Part 5: Input Types and Custom Resolvers](https://dev-blog.apollodata.com/tutorial-graphql-input-types-and-client-caching-f11fa0421cfd)
- [Part 6: GraphQL Subscriptions on the Server](https://dev-blog.apollodata.com/tutorial-graphql-subscriptions-server-side-e51c32dc2951)
- Part 7: Client subscriptions (This part!)
- [Part 8: Pagination](https://dev-blog.apollodata.com/tutorial-pagination-d1c3b3ee2823)


---


In part 6, we implemented the server-side portion of GraphQL Subscriptions for our message channels. Clients can use these subscriptions to be notified whenever a specific event happens — in this case the creation of a message in a specified channel. In this tutorial, we will add GraphQL Subscriptions to our client so that instances of the client can see live updates to messages in a channel.


---


Let’s get started by cloning the Git repo and installing the dependencies:


```text
git clone https://github.com/apollographql/graphql-tutorial.git
cd graphql-tutorial
git checkout t7-start
cd server && npm install
cd ../client && npm install
```


First, let’s make sure everything works by starting the server and client.


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


The current state of the application


When you navigate your browser to[http://localhost:3000](http://localhost:3000/) , you should enter the homepage of our messaging app, which has a list of channels that the user has created. Click on one of the channels, and you’ll be taken to the detail view that we created in[Part 5](https://dev-blog.apollodata.com/tutorial-graphql-input-types-and-client-caching-f11fa0421cfd) , where you can add new messages in the channel. You’ll notice that if you open the same channel in multiple windows, messages added in one window don’t show up in the other. By the end of this tutorial, client syncing will allow multiple users to see each-other’s changes!


## GraphQL Subscriptions Transport


The first step to adding subscriptions to our client is to set up the WebSocket connection that the client and server will communicate over. Forming and maintaining the WebSocket connection will be the job of the Apollo network interface, defined in` client/src/App.js` . To add WebSocket support to our existing interface, we will construct a GraphQL Subscription client and merge it with our existing network interface to create a new interface that performs normal GraphQL queries over HTTP and subscription queries over WebSockets.


First, let’s add the necessary imports at the top of` client/src/App.js`


```text
import { SubscriptionClient, addGraphQLSubscriptions } from 'subscriptions-transport-ws';
```


Next, we construct the WebSocket-based subscription client and merge it with our existing network interface


```text
const networkInterface = createNetworkInterface({ uri:
'http://localhost:4000/graphql' });networkInterface.use([{
applyMiddleware(req, next) {
setTimeout(next, 500);
},
}]); const wsClient = new SubscriptionClient(`ws://localhost:4000/subscriptions`, {
reconnect: true,
});  const networkInterfaceWithSubscriptions = addGraphQLSubscriptions(
networkInterface,
wsClient,
);
```


Now all we have to do to enable subscriptions throughout our application is use` networkInterfaceWithSubscriptions` as the Apollo Client’s network interface


```text
const client = new ApolloClient({
networkInterface:  networkInterfaceWithSubscriptions  ,
...
});
```


If you load up the client and look in the “Network” tab of the developer tools (right-click and “Inspect element”), you should see that the client has established a WebSocket connection to the server.


We can see our subscription connection being established!


## Listening for Messages


Now that we can use GraphQL Subscriptions in our client, the next step is to use subscriptions to detect the creation of messages. Our goal here is to use subscriptions to update our React views to see new messages in a channel as they are added.


Before we start, we have to refactor our` client/src/components/ChannelDetails.js` component to be a full ES6 class component instead of just a function, so that we can use the React lifecycle events to set up the subscription.


First, we update our import statement to include the` Component` class.


```text
import React , { Component }   from 'react';
```


Then, we refactor our function component into an ES6 class


```text
class ChannelDetails extends Component {
render() {
const { data: {loading, error, channel }, match } = this.props;    if (loading) {
return <ChannelPreview channelId={match.params.channelId}/>;
}
if (error) {
return <p>{error.message}</p>;
}
if(channel === null){
return <NotFound />
}    return (
<div>
<div className="channelName">
{channel.name}
</div>
<MessageList messages={channel.messages}/>
</div>);
}
}
```


Now that our component is ready to handle subscriptions, we can write out the subscriptions query:


```text
const messagesSubscription = gql`
subscription messageAdded($channelId: ID!) {
messageAdded(channelId: $channelId) {
id
text
}
}
`
```


To make the subscription request, we will use Apollo Client’s` <a href="http://dev.apollodata.com/react/subscriptions.html#subscribe-to-more" target="_blank" rel="noreferrer noopener">subscribeToMore</a>` function, which lets us update the store when we receive new data. First, we define a` componentWillMount` in our component, which is where we will start the subscription.


```text
class ChannelDetails extends Component {
componentWillMount() {
}    render() {
...
}
}
```


Inside this React lifecycle function, we set up our subscription to listen for new messages and add them to our local store as they come. Because the` updateQuery` function is supposed to produce a new instance of a store state based on` prev` , the previous store state, we use the` Object.assign` method to create a copy of the store with modifications to add the new message.


Also, because we are manually managing our store of messages, it is possible to have duplicate messages. A message can be added once as the result of performing the mutation and again when we receive the subscription notification. To prevent duplicates, we add an extra check to verify that we did not already add the message to our store with a previous mutation.


```text
componentWillMount() {
this.props.data.subscribeToMore({
document: messagesSubscription,
variables: {
channelId: this.props.match.params.channelId,
},
updateQuery: (prev, {subscriptionData}) => {
if (!subscriptionData.data) {
return prev;
}      const newMessage = subscriptionData.data.messageAdded;
// don't double add the message
if (!prev.channel.messages.find((msg) => msg.id === newMessage.id)) {
return Object.assign({}, prev, {
channel: Object.assign({}, prev.channel, {
messages: [...prev.channel.messages, newMessage],
})
});
} else {
return prev;
}
}
});
}
```


We’re almost done now! All we have to do is perform the same de-duplication check in the` AddMessage` component, because when we create a new message we might be notified of creation through the WebSocket before the query returns data. In` client/src/components/AddMessage.js` , replace` data.channel.messages.push(addMessage);` with the same statement wrapped in a condition that checks for duplication


```text
if (!data.channel.messages.find((msg) => msg.id === addMessage.id))
{
// Add our Message from the mutation to the end.
data.channel.messages.push(addMessage);
}
```


Now we’re ready to test out our subscription-based live updating messages view! Open two windows of the client and select the same channel in both. When you add a message in one client, you should see the same message show up in the other client!


It works! The clients sync with each other!


## Conclusion


Congrats! You’ve now wired up the server-side implementation of GraphQL Subscriptions to your client through Apollo so that users can see live updates of message additions from other clients. With a couple more changes such as pagination, covered in the next[tutorial](https://dev-blog.apollodata.com/tutorial-pagination-d1c3b3ee2823) , and auth your application will be ready for real use!


If you liked this tutorial and want to keep learning about Apollo and GraphQL, make sure to click the “Follow” button below, and follow us on Twitter at[@apollographql](https://twitter.com/apollographql) and the author at[@ShadajL](http://twitter.com/shadajl) .


Thanks to my mentor,[Jonas Helfer](https://medium.com/u/39cbd38c57c8?source=post_page-----40e185e4be76----------------------) , for his support as I wrote this post!


Written by


Shadaj Laddad


[Read more by Shadaj Laddad](https://www.apollographql.com/blog/author/shadaj)
