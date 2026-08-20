---
schema_version: "1.0.0"
document_id: "2de185d7a58f758ab96cf5a44a7b99c98bdd764cfbe1af6ca1b2984d9b80c09b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/tutorial"
published_at: "2017-09-19T00:10:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:268f67641544413453dfaf7b227ffe1f4d8d9784278adea1fe313a7b87b817ef"
---

# Tutorial: Pagination

This is part 8 of our full-stack React + GraphQL tutorial series. Each part is self-contained and introduces one new key concept, so you can either do each part separately or follow the entire series — it’s up to you!


Here are the sections we’ve covered so far:


- [Part 1: The Frontend](https://dev-blog.apollodata.com/full-stack-react-graphql-tutorial-582ac8d24e3b)
- [Part 2: The Server](https://dev-blog.apollodata.com/react-graphql-tutorial-part-2-server-99d0528c7928)
- [Part 3: Basic GraphQL Mutations](https://dev-blog.apollodata.com/react-graphql-tutorial-mutations-764d7ec23c15)
- [Part 4: Optimistic UI and client side store updates](https://dev-blog.apollodata.com/tutorial-graphql-mutations-optimistic-ui-and-store-updates-f7b6b66bf0e2)
- [Part 5: Input Types and Custom Resolvers](https://medium.com/p/tutorial-graphql-input-types-and-client-caching-f11fa0421cfd)
- [Part 6: Subscriptions on the Server](https://dev-blog.apollodata.com/tutorial-graphql-subscriptions-server-side-e51c32dc2951)
- [Part 7: GraphQL Subscriptions on the Client](https://dev-blog.apollodata.com/tutorial-graphql-subscriptions-client-side-40e185e4be76)


In Parts 6 and 7 we went over how to add subscriptions to the server and hot-load changes to the server in your client. In this part, we’ll add pagination so your server can send data in smaller chunks, rather than everything at once. By the end, you’ll know how to:


- Use cursors and limits to control the amount of data returned from the server
- Use the Apollo Client` fetchMore` function
- Implement cursor-based pagination in response to a button click


To get started, let’s clone the git repo and install dependencies. We recommend you re-clone the repository even if you’ve finished previous parts of the tutorial, as we’ve made some useful changes to the file structure of the app from the previous step.


```text
git clone  https://github.com/apollographql/graphql-tutorial.git
cd graphql-tutorial
git checkout t8-start
cd server && npm install && cd ../client && npm install && cd ..
```


To make sure it worked, let’s start the server and the client, each in a separate terminal:


```text
cd server
npm start
```


In another terminal, type:


```text
cd client
npm start
```


When you navigate your browser to[http://localhost:3000](http://localhost:3000/) , you should see the homepage of our messaging app. When you click on the “faker” channel, you should see a long list of randomly generated messages. (We needed to generate a long list for this step to demonstrate how to avoid loading it all at once.)


Instead of pre-loading all of these messages, we can implement pagination and load only a page’s worth of data at once.


### Updating the Schema


Let’s start by making some changes to our server’s schema to expose an API to load smaller chunks of items.


```text
//server/src/schema.jsconst typeDefs = `
type Channel {
id: ID!                # "!" denotes a required field
name: String
messages: [Message]
# messages will be returned in a MessageFeed object wrapper
messageFeed(cursor: String): MessageFeed
}input MessageInput{
channelId: ID!
text: String
}# ...type Message {
id: ID!
text: String
createdAt: Int
} type MessageFeed {
# cursor specifies the place in the list where we left off
cursor: String!


# this is a chunk of messages to be returned
messages: [Message]!
}
```


In addition to returning the correct block of messages, the` messageFeed` type also returns a cursor, which tells the client where we left off in the list. Then, when we want to load more messages, we can pass this value back to the server to tell it which messages to give us next.


---


***What are cursors in pagination?***


*Cursors are pointers to the spot where we’ve left off in the data. More specifically, cursors are variables that can hold any value that the client can pass to the server to help the it find the point at which it should start returning data. It doesn’t matter what value you use, and in fact it should be opaque from the point of view of the client.*


*In this example, we’ll use a timestamp as a cursor. This is much better than pointing to an actual message ID or the index of the message in an array. If we use an ID or index, messages being deleted and inserted can cause problems: we might not be able to find that specific ID, or the index might now be referring to a different message. Read more in our*[previous article about understanding pagination](https://dev-blog.apollodata.com/understanding-pagination-rest-graphql-and-relay-b10f835549e7) *.*


---


Next, let’s add a new resolver for` messageFeed` in the resolvers file on the server. Let’s add the bolded code under the Query resolver:


```text
Query: {
channels: () => {
return channels;
},    channel: (root, { id }) => {
return getChannel(id);
},
},   // The new resolvers are under the Channel type
Channel: {
messageFeed: (channel, { cursor }) => {
// The cursor passed in by the client will be an
// integer timestamp. If no cursor is passed in,
// set the cursor equal to the time at which the
// last message in the channel was created.
if (!cursor) {
cursor =
channel.messages[channel.messages.length - 1].createdAt;
}


cursor = parseInt(cursor);        // limit is the number of messages we will return.
// We could pass it in as an argument but in this
// case let's use a static value.
const limit = 10;


const newestMessageIndex = channel.messages.findIndex(
message => message.createdAt === cursor
); // find index of message created at time held in cursor        // We need to return a new cursor to the client so that it
// can find the next page. Let's set newCursor to the
// createdAt time of the last message in this messageFeed:
const newCursor =
channel.messages[newestMessageIndex - limit].createdAt;


const messageFeed = {
messages: channel.messages.slice(
newestMessageIndex - limit,
newestMessageIndex
),
cursor: newCursor,
};


return messageFeed;
},
},
```


Notice that if a cursor is not passed to the query, we set the cursor equal to the time at which the most recent message was created, and the server will return the most recent messages. This way, the client can easily query the most recent messages on initial page load. We also use a limit of 10 to specify the number of messages to be fetched, and update the cursor returned by` messageFeed` accordingly so that the client can later fetch the next page of items.


### Test it out


At this point, you should be able to test your server to make sure all of the above code is running correctly. Navigate to[http://localhost:4000/graphiql](http://localhost:4000/graphiql) in your browser, and run the following query:


Retrieving a page of messages.


You’ll see that we can now retrieve messages via the` messageFeed` wrapper, which also gives us information about the cursor for the next page, and we only get 10 items at a time instead of the whole list. If we pass the cursor we got back as an argument, we get a new list of 10 messages that picks up where we left off:


Passing the cursor in to retrieve the next page.


Now, let’s use our new-found power to make the client more efficient by only loading the messages we are displaying.


## Updating the client


In` src/components/ChannelDetails.js` (in the client code), replace the channel details query with the following query, which includes the new` messageFeed` field and some information about pagination:


```text
export const channelDetailsQuery = gql`
query ChannelDetailsQuery($channelId: ID!, $cursor: String) {
channel(id: $channelId) {
id
name
messageFeed(cursor: $cursor) @connection(key: "messageFeed") {
cursor
messages {
id
text
}
}
}
}
`;
```


Since the messages we will display in our UI are now nested under` messageFeed` , we also need to update our` ChannelDetails` component. Make the bolded change to the render function to access the` messageFeed` property:


```text
return (
<div>
<div className="channelName">
{channel.name}
</div>
<MessageList messages={channel.messageFeed.messages}/>
</div>
);
```


The updated query will now only return 10 messages at a time, rather than returning all of the messages in the channel.


### Updating our mutation and subscription code


We’re now loading the messages nested under the` messageFeed` field, so that’s where they’ll be stored in the Apollo Client cache. You can confirm this by opening up the Chrome DevTools and checking out the store tab:


Therefore, we need to update our subscription and mutation code, which updates that list to be aware of that nesting, basically, replacing` channel.messages` with` channel.messageFeed.messages` .


**In src/components/AddMessage.js:**


```text
// don’t double add the message
if (!data.channel.messageFeed.messages.find(( msg  ) =>
msg.id === addMessage.id)) {
// Add our Message from the mutation to the end.
data.channel.messageFeed.messages.push(addMessage);
}
```


**Similarly, in src/components/ChannelDetails.js:**


```text
// don’t double add the message
if (!prev.channel.messageFeed.messages.find(( msg  ) =>
msg.id === newMessage.id)) {  return  Object  .assign({}, prev, {
channel:  Object  .assign({}, prev.channel, {
messageFeed: {
messages: […prev.channel.messageFeed.messages, newMessage],
}
})
});
} else {
return prev;
}
```


Now, go into the UI, open two tabs into the same channel, and test adding a message. It should appear correctly both on your screen and on the other screen via mutation support and subscriptions.


If you get stuck on this step, check out[the diff between the start and end of step 8](https://github.com/apollographql/graphql-tutorial/compare/t8-start...t8-end) to see all of the necessary changes.


---


***The connection directive***


*We’ve also added the*` <em>connection</em>` *directive to the*` <em>messageFeed</em>` *field in the query. This a special client-side only directive that controls how data under that field is cached in the Apollo Client store. Since fields that deal with pagination often have some extra arguments like*` <em>cursor</em>` *or*` <em>limit</em>` *, we want to make sure we have a clean cache key that doesn’t include those arguments.*


*In this case, we specify that data returned from this field should be stored under the key*` <em>messageFeed</em>` *, making it easier to append to this list from the mutation for adding a new message. If we didn’t use the*` <em>connection</em>` *directive on that field, our mutation*` <em>update</em>` *function would need to reproduce the exact set of arguments originally passed to that field.*


---


Now we are ready to actually add the` fetchMore` function, which is the primary way we add pagination in Apollo Client. We’ll define a function called` loadOlderMessages` , which can be accessed through the props passed to our` ChannelDetails` component. This function will make use of the` fetchMore` method, which Apollo Client attaches to the` data` prop.


Let’s add the bolded code to the` channelDetailsQuery` container, next to the` options` field:


```text
export default (graphql(channelDetailsQuery, {
options: (props) => ({
variables: {
channelId: props.match.params.channelId,
},
}),   props: (props) => {
return {
data: props.data,
loadOlderMessages: () => {
return props.data.fetchMore({
variables: {
channelId: props.data.channel.id,
cursor: props.data.channel.messageFeed.cursor,
},            updateQuery(previousResult, { fetchMoreResult }) {
const prevMessageFeed =
previousResult.channel.messageFeed;
const newMessageFeed =
fetchMoreResult.channel.messageFeed;              const newChannelData = {...previousResult.channel,
messageFeed: {
messages: [
...newMessageFeed.messages,
...prevMessageFeed.messages
],
cursor: newMessageFeed.cursor
}
}              const newData =  {
...previousResult,
channel: newChannelData
};              return newData;
}
});
}
};
}
})(ChannelDetails));
```


The` loadOlderMessages` function calls` fetchMore` with the id of the channel that the messages belong to and the cursor returned by our first` channelDetailsQuery` . The` fetchMore` function will use the original query by default (` channelDetailsQuery` , in this case), so we just pass in new variables. Note that we actually need to specify the cursor in the` loadOlderMessages` function, since we no longer want only the most recent messages. Once new data is returned from the server, we use Apollo Client’s` updateQuery` function to merge the new data with the existing data, which will cause a re-render of your UI component with an expanded list. As a final step, let’s add a ‘Load Older Messages’ button to the` ChannelDetails` component. Update the render function with the bolded code:


```text
return (
<div>
<button onClick={loadOlderMessages}>
Load Older Messages
</button>
<div>
<div className="channelName">
{channel.name}
</div>
<MessageList messages={channel.messageFeed.messages}/>
</div>
</div>
);
```


Now, if you visit[http://localhost:3000/channel/1](http://localhost:3000/channel/1) and click on the “Load Older Messages” button, a fresh of set of 10 messages will appear at the top of your screen and the already-displayed messages will get pushed down in the list! And with that, you’ve implemented cursor-based pagination.


## Conclusion


Congratulations, you’ve reached the end of part 8 in this tutorial series! You’ve learned how to use cursors, limits, and the` fetchMore` function to update your server and client and implement cursor-based pagination in your app. With a couple more changes like auth, which we’ll cover in later tutorials, your application will be ready for real use!


*Thanks to my mentor*[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----d1c3b3ee2823----------------------) *and fellow intern*[Klaire Tan](https://medium.com/u/df256b3f5a1d?source=post_page-----d1c3b3ee2823----------------------) *for helping me understand pagination and write this tutorial!*


Written by


Ramya Nagarajan


[Read more by Ramya Nagarajan](https://www.apollographql.com/blog/author/rrramyan)
