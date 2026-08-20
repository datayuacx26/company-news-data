---
schema_version: "1.0.0"
document_id: "8766e3682502fd66005c1506dc58ac9888f1ad55358504ba5cf6508e773da0ba"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/input-types-and-client-caching"
published_at: "2017-06-06T00:15:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:0e5f56534c19d8ab4746556cf0de3a31c7046032df1413fee1389f321cfce765"
---

# Tutorial: GraphQL Input Types and Custom Resolvers

This is part 5 of our full-stack GraphQL + React Tutorial that guides you through creating a messaging application. Each part is self-contained and introduces new key concepts, which means you don’t have to do all the other parts before doing this one. But just in case you’re curious, here’s what we’ve covered so far:


- [Part 1: Setting up a simple client](https://dev-blog.apollodata.com/full-stack-react-graphql-tutorial-582ac8d24e3b)
- [Part 2: Setting up a simple server](https://dev-blog.apollodata.com/react-graphql-tutorial-part-2-server-99d0528c7928)
- [Part 3: Writing mutations and keeping the client in sync](https://dev-blog.apollodata.com/react-graphql-tutorial-mutations-764d7ec23c15)
- [Part 4: Optimistic UI and client side store updates](https://dev-blog.apollodata.com/tutorial-graphql-mutations-optimistic-ui-and-store-updates-f7b6b66bf0e2)
- Part 5: Input types and custom resolvers (you are here!)
- [Part 6: Server-side Subscriptions](https://dev-blog.apollodata.com/tutorial-graphql-subscriptions-server-side-e51c32dc2951)
- [Part 7: GraphQL Subscriptions on the Client](https://dev-blog.apollodata.com/tutorial-graphql-subscriptions-client-side-40e185e4be76)
- [Part 8: Pagination](https://dev-blog.apollodata.com/tutorial-pagination-d1c3b3ee2823)


---


In Part 4, we went over how to use store updates and Optimistic UI to deal with network latency in the channel list view of our sample app.


In this part, we’ll build a channel detail view that displays all the messages in a channel and allows you to post new messages. By the end, you’ll know how to:


- Use field arguments in your queries
- Make the most out of Apollo’s normalized cache
- Use GraphQL input types


---


To get started, let’s clone the git repo and install the dependencies:


```text
git clone  https://github.com/apollographql/graphql-tutorial.git
cd graphql-tutorial
git checkout t5-start
cd server && npm install
cd ../client && npm install
```


To make sure it worked, let’s start the server and the client, each in a separate terminal:


```text
cd server
npm start
```


In another terminal:


```text
cd client
npm start
```


You can now navigate to` localhost:3000` and explore the current state of the channel detail view. We’ve already done some work for you, so it should look something like this:


Initial state of the channel detail view


Since this tutorial focuses on GraphQL, we have already built the routing and template for the channel detail view for you. You don’t need to know how it works for this tutorial, but just in case you are curious, we use` react-router` (see the[react-router tutorial](https://medium.com/@pshrmn/a-simple-react-router-v4-tutorial-7f23ff27adf) and[documentation](https://reacttraining.com/react-router/web/guides/quick-start) ).


It may look like we’ve done all the work already, but the new view is only a stub so far. To make it actually work, you will need to write a GraphQL query to get the channel name and its messages from the server, and you’ll need to create a mutation to add new messages.


## Adding the Channel Detail View


The channel detail view should display the channel name, its messages, and a new message input. First, let’s modify the schema and write a query to display the messages currently in the channel.


In the schema (on the server), we need to create a` Message` type, add the` messages` field to the` Channel` type, and provide a way to fetch a single channel by adding a` channel` field to the root` Query` type. After making these changes,` typeDefs` in` schema.js` should look like this:


```text
//server/src/schema.jsconst typeDefs = `
type Channel {
id: ID!
name: String
messages  : [Message]!
}type  Message   {
id: ID!
text: String
}# This type specifies the entry points into our API
type Query {
channels: [Channel]
channel  (id: ID!): Channel
}# The mutation root type, used to define all mutations
type Mutation {
addChannel(name: String!): Channel
}
`;const schema = makeExecutableSchema({ typeDefs, resolvers });
export { schema };
```


Notice that the way we fetch a single` Channel` is by adding` id` as a field argument. This is a very common pattern in GraphQL and you will probably use it a lot in your applications. Arguments can be of any scalar or input type, which we’ll learn more about later in this tutorial.


Next, the new query needs to be backed by a resolver, which returns the proper channel. For this, add the bolded query to` resolvers.js:`


```text
//server/src/resolvers.jsconst channels = [{
id: '1',
name: 'baseball',
messages: [{
id: '2',
text: 'baseball is life',
}]
}];
let nextId = 3;export const resolvers = {
Query: {
channels: () => { ... },
channel: (root, { id }) => {
return channels.find(channel => channel.id === id);
},
},
};
```


*Note: We created an array with pre-populated messages for*` <em>channels</em>` *. If you didn’t check out the t5-start branch, you’ll have to create that array yourself.*


Now that the sever supports querying a specific channel, the client — specifically the` ChannelDetails` component — needs to perform the query. The best practice in GraphQL is to use query variables for arguments (` $channelId` for` id` in this case). The GraphQL spec requires that we define the variables we use after the` query` keyword. If we don’t do it, the server will complain that we used a variable without defining it. The definition has to match the type that the argument expects. In this case, it’s` ID` .


In` channelDetails.js` write the following query:


```text
//client/src/components/channelDetails.jsexport const channelDetailsQuery = gql`
query  ChannelDetailsQuery($channelId : ID!)   {
channel(id: $channelId) {
id
name
messages {
id
text
}
}
}
`;
```


In the` ChannelDetails` React component, replace the stub with code that renders the actual data. First check if the query is loading (` data.loading` ), then check to make sure that there is no error (` data.error` ), and finally render the channel name and` MessagesList` .


If you do all of that, you should end up with a component that looks like this:


```text
//client/src/components/ChannelDetails.jsconst ChannelDetails = ({ data: {loading, error, channel }, match }) => {
if (loading) {
return <p>Loading...</p>;
}
if (error) {
return <p>{error.message}</p>;
}
if( channel   === null){
return <NotFound />
}  return (<div>
<div className="channelName">
{channel.name}
</div>
<MessageList messages={channel.messages}/>
</div>);
}//export const channelDetailsQuery = gql`...`;// ...
```


Now all you have to do is wrap the component with the query we wrote earlier, and export it.


```text
//client/src/components/ChannelDetails.js (at the bottom)export default (graphql(channelDetailsQuery, {
options: (props) => ({
variables  : { channelId: props.match.params.channelId },
}),
})(ChannelDetails));
```


We’re well on our way to a functioning chat application, try it out for yourself! It should look like this:


Midpoint state of channel detail view


Now that we have a channel name and message stream, let’s add the message mutation to post new messages.


## Posting a New Message


Creating a functional` AddMessage` is very similar to adding a channel in[part 3](https://dev-blog.apollodata.com/react-graphql-tutorial-mutations-764d7ec23c15) , so first instinct suggests using a mutation with fields for the message text and a channel id. But in the future, we may want to associate a username, timestamp, text encoding, picture, mentioned users, or other meta-message information. Adding each of these to the Mutation’s signature quickly becomes unwieldy and inflexible. To keep things tidy, we’re going to use a GraphQL[input type](http://graphql.org/graphql-js/mutations-and-input-types/) , which is an object that can only contain basic scalar types, list types, and other input types. Input types allow client mutation signatures to stay constant and provide better readability in the schema.


Starting on the server, let’s define the` MessageInput` input type and include the mutation in` schema.js` as follows:


```text
//server/src/schema.jsinput  MessageInput  {
channelId: ID!
text: String
}type Mutation {
# A mutation to add a new channel to the list of channels
addChannel(name: String!): Channel
addMessage  (message: MessageInput!): Message
}
```


The resolver for` addMessage` in` resolvers.js` should check that the input


```text
//server/src/resolvers.jsMutation: {
addChannel: {...},
addMessage  : (root, { message }) => {
const channel = channels.find(channel => channel.id === message.channelId);
if(!channel)
throw new Error("Channel does not exist");    const newMessage = { id: String(nextMessageId++), text: message.text };
channel.messages.push(newMessage);
return newMessage;
},
},
```


Next on the client side, we need to complete` AddMessage.js` , starting with the query:


```text
//client/src/components/AddMessage.jsconst addMessageMutation = gql`
mutation addMessage($message: MessageInput!) {
addMessage(message: $message) {
id
text
}
}
`;
```


The` AddMessage` component body adds variables to` AddChannel` ’s base code, which includes the same optimistic UI functionality we used in the[last tutorial](https://dev-blog.apollodata.com/tutorial-graphql-mutations-optimistic-ui-and-store-updates-f7b6b66bf0e2) . The only part that’s different are the variables. I have highlighted the changes in bold below:


```text
//client/src/components/AddMessage.jsconst AddMessage = ({ mutate, match }) => {
const handleKeyUp = (evt) => {
if (evt.keyCode === 13) {
mutate({
variables  : {
message: {
channelId: match.params.channelId,
text: evt.target.value
}
},
optimisticResponse: {
addMessage: {
text: evt.target.value,
id: Math.round(Math.random() * -1000000),
__typename: 'Message',
},
},
update: (store, { data: { addMessage } }) => {
// Read the data from the cache for this query.
const data = store.readQuery({
query: channelDetailsQuery,
variables  : {
channelId: match.params.channelId,
}
});
// Add our Message from the mutation to the end.
data.channel.messages.push(addMessage);
// Write the data back to the cache.
store.writeQuery({
query: channelDetailsQuery,
variables  : {
channelId: match.params.channelId,
},
data
});
},
});
evt.target.value = '';
}
};  return (
...
);
};//const addMessageMutation = gql`...`const AddMessageWithMutation = graphql(
addMessageMutation,
)(withRouter(AddMessage));export default AddMessageWithMutation;
```


*Note:*[match](https://reacttraining.com/react-router/web/api/match) *is react-router’s interface to url properties provided by*[withRouter](https://reacttraining.com/react-router/web/api/withRouter)


Now we have a fully-functioning messaging channel! However, there’s a small problem: if the network is slow, the user has to wait for both the channel name and messages to be loaded from the server. Until all of the data is loaded, the user won’t even know which channel they are in, which is bad UX. Ideally, we’d want the user to see a good channel preview while messages are being loaded. That’s what we’re going to do in the last section of this tutorial.


## Reading the Channel Name from Cache


As you may have noticed, the client already knows the channel names because it loaded them with the` ChannelsListQuery` on the homepage. If there was a way for us to keep the channel name around, we could display it without making another request to the server!


Lucky for you, Apollo Client automatically stores each query result in its[normalized cache](https://dev-blog.apollodata.com/the-concepts-of-graphql-bc68bd819be3) , which means we can just query for the data we want and let Apollo Client figure out whether it can be loaded from the cache or not. However, there is a small catch:


By default, Apollo Client uses the query path (for example` /channel(id:5)/name` ) to determine if an object is cached.


Since the` channels` and` channel` queries result in different paths to the same object, Apollo Client doesn’t know that they are the same unless you explicitly tell it that the` channel` query might resolve to an object that was retrieved by the` channels` query. We can tell Apollo Client about this relationship by adding a custom resolver to the` ApolloClient` constructor in` App.js` . This custom resolver tells Apollo Client to check its cache for a` Channel` object with ID` $channelId` whenever we make a` channel` query. If it finds a channel with that ID in the cache, it will not make a request to the server.


The following custom resolver creates this mapping in` App.js` :


```text
//client/src/App.js//function dataIdFromObject (result) {...}const client = new ApolloClient({
networkInterface,
customResolvers  : {
Query: {
channel: (_, args) => {
return toIdValue(dataIdFromObject({ __typename: 'Channel', id: args['id'] }))
},
},
},
dataIdFromObject,
});
```


` <em>ApolloClient</em>` *uses*` <em>dataIdFromObject</em>` *to tag GraphQL objects in the cache and*` <em>toIdValue</em>` *ensures an ID type is returned.*


Now all you have to do is create the` ChannelPreview` component as you normally would:


```text
//client/src/components/ChannelPreview.jsconst ChannelPreview = ({ data: {loading, error, channel } }) => {
return (
<div>
<div className="channelName">
{channel ? channel.name : 'Loading...'}
</div>      <div>Loading Messages</div>
</div>
);
};export const channelQuery = gql`
query ChannelQuery($channelId : ID!) {
channel(id: $channelId) {
id
name
}
}
`;export default (graphql(channelQuery, {
options: (props) => ({
variables: { channelId: props.channelId },
}),
})(ChannelPreview));
```


Lastly, we need to replace the loading message of our` ChannelDetails` component with the` ChannelPreview` component:


```text
//client/src/components/ChannelDetails.jsconst ChannelDetails = (...) => {
if (loading) {
return <ChannelPreview channelId={match.params.channelId}/>;
}
```


By pulling data from the cache, we have created a channel detail view that displays the channel name immediately, while loading the messages in the background.


Final state of channel detail view


## Conclusion


Congratulations, you now have an application that provides channel-labelled messaging streams! The service is almost ready for production, after a couple enhancements: First, we’ll want a way to show messages in real time using GraphQL Subscriptions, start with the server side in the[next part](https://dev-blog.apollodata.com/tutorial-graphql-subscriptions-server-side-e51c32dc2951) . Second, we will want to paginate the messages, since loading all messages at one time could be slow. Finally, we’ll also want to add login and authentication to make sure we know who a message is from.


If you liked this tutorial and want to keep learning about Apollo and GraphQL, make sure to click the “Follow” button below, and follow us on Twitter at[@apollographql](https://twitter.com/apollographql) and the author at[@evanshauser](https://twitter.com/evanshauser) .


A huge thank you to[Jonas Helfer](https://medium.com/u/39cbd38c57c8?source=post_page-----f11fa0421cfd----------------------) for his guidance!


Written by


Evans Hauser


[Read more by Evans Hauser](https://www.apollographql.com/blog/author/evans)
