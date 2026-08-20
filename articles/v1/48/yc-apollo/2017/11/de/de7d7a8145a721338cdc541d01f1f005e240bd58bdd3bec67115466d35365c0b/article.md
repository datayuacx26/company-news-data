---
schema_version: "1.0.0"
document_id: "de7d7a8145a721338cdc541d01f1f005e240bd58bdd3bec67115466d35365c0b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-client-2-0-beyond-graphql-apis-888807b53afe"
published_at: "2017-11-14T23:11:46+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:54.348132+00:00"
content_hash: "sha256:744410d0ac667c7ad332a845bd214cfdfd5573b6f343b0491162261f425373a2"
---

# Apollo Client 2.0: Beyond GraphQL APIs

Apollo Client 1.0 was all about taking data from your GraphQL server and attaching it to your UI components. That’s a really important aspect of building an application that we’ve made faster, simpler, and more powerful in Apollo Client 2.0. But with the newly released version, you can actually use GraphQL to do much more than just load data from a server. You can use the same query language to manage your client-side data and even fetch from non-GraphQL APIs!


This is exactly what I explored in my talk at this year’s[GraphQL Summit](http://summit.graphql.com/) . Check out the video below, or if videos aren’t your jam, keep on reading to learn how Apollo Client 2.0 can simplify managing all of your application’s data, including local & REST data.


## A solid foundation


Over the past year,[Apollo Client](https://github.com/apollographql/apollo-client) has seen astounding growth, with over 2.5 million downloads on npm & nearly 5,000 stars on GitHub. I think a lot of this success can be attributed to the fact that Apollo removes a ton of front-end complexity for React, Angular, and Vue developers alike. All you have to do is describe exactly the data you want from your server with GraphQL, bind it to your component, and let Apollo Client take care of the rest.


The benefits of building your application with Apollo extend far beyond the ability to easily retrieve data from a GraphQL server. With Apollo Client, you no longer have to worry about fetching data, transforming the response, normalizing, and caching it. Features that normally require significant coding, such as optimistic UI updates, pagination, and prefetching data, become trivial to execute.


A diagram explaining the data flow in Apollo Client


## What’s new with Apollo Client 2.0


How did we improve upon an already solid foundation with Apollo Client 2.0? The newest version is 2–5x faster, 40% smaller, and most users can upgrade by changing[only 5 lines of code](https://www.apollographql.com/docs/react/2.0-migration.html#basic) . The main difference is our revamped modular architecture that paves the way for the future of GraphQL — things like streaming data, custom directives, and individualized caching.


An overview of Apollo Client 2.0’s new modular architecture


### Cache


We know caching is never one size fits all, so with Apollo Client 2.0 you can now easily swap out our default in-memory cache with your own implementation. We’ve already started to see community members build their own with great success, like the team at Convoy did with their[graph-based cache Hermes](https://github.com/convoyinc/apollo-cache-hermes) . I’m super excited to see the community get creative with their Apollo Client caches in the upcoming weeks; for example, I’d love to see an integration for React Native AsyncStorage or MobX. If you have an idea, please don’t hesitate to get in touch by pinging me on Twitter[@peggyrayzis](http://twitter.com/peggyrayzis) !


### Network stack


Our new interface for personalizing your network stack is called[Apollo Link](https://www.apollographql.com/docs/link/) . Links are powerful because they allow you to hook into the request cycle at any point to manipulate how you process & retrieve your data. They’re backed by Observables, instead of promises, so you can return multiple results from one operation.


With only a few lines of code, you can create your own links and compose them together to fully manage your data flow from beginning to end. Here, we’re using the` ApolloLink.split` utility method to branch our request by serving subscriptions over WebSockets and queries & mutations over HTTP. The first argument to a link gives you full access to your query, which enables you to determine how to retrieve your data and what happens with the result.


An example of Apollo Link composition using split


If we can hook into the request cycle at any point with Apollo Link, then we can start to get really creative with how we process and retrieve our data. The last half of my talk dives into some experimental features that are now possible because of Apollo Link. Let’s get weird! 😜


## Unlocking a universe of possibilities


Remember our diagram from earlier in the post where data was loaded from a GraphQL server? What if we could switch that GraphQL server out with *something else* ? Since Apollo Link allows you to determine where you’re retrieving your data from based on a GraphQL operation, **you can theoretically request data from anywhere** . As long as the data coming back is JSON or an object, you should be able to query your source using GraphQL and get back whatever you want.


Apollo Link opens up the door to unlimited possibilities. What if we could query local data in your application and store it in Apollo Client? This includes things like network status, routing, or anything you currently use Redux or MobX for. By integrating your client-side data into Apollo Client, you get the same awesome features like built-in caching and ease of querying with GraphQL, without having to set up and maintain a second store.


### Client-side state management


This is exactly what we’re proposing with[Apollo Link State](https://github.com/apollographql/apollo-link-state) , our solution for managing client-side data entirely in Apollo Client. Thanks to[James Baxley](https://medium.com/u/fd3d3dea3cd3?source=post_page-----888807b53afe----------------------) , Apollo Link State is already out, and although it’s in a very early stage, we’re excited about its potential! To set it up in your application, you will pass a resolver map to your state link that determines how you’ll read & write your local data to the cache. If you’ve worked with[graphql-tools](https://www.apollographql.com/docs/graphql-tools/resolvers.html#Resolver-map) before, the API will look familiar. The main difference here is that the cache is added to the context so you can manipulate it directly like you would a database on the server.


What a resolver map could look like in Apollo Link State


What would our query requesting local data look like? By annotating a client-side field with the` @client` directive, the state link will be able to process the operation and tell it to resolve from the cache.


We can also query data from a REST endpoint in a similar way. By marking a field with the` @rest` directive, we can hit our REST endpoint and store exactly the data we request in Apollo Client side by side with our local and GraphQL server data. A contributor has already started working on a[similar implementation](https://github.com/n1ru4l/graphql-schema-generator-rest) and we hope to have an official version of Apollo Link REST out soon. Eventually, you’ll be able to combine all of these directives and request from multiple data sources in one query! 😍


Using @client and @rest directives to request data from anywhere


Now that Apollo Link enables us to request data from anywhere, we can use Apollo Client to manage **all** of our application’s data. No more action creators, reducers, or maintaining a separate store! All you need to do is write queries with GraphQL and let Apollo Client take care of the rest.


Apollo Client 2.0 is your universal data solution


Whether you’re requesting data from a REST endpoint, your application, or a GraphQL server, Apollo Client 2.0 is truly a universal data solution. The new modular architecture is flexible and extensible, so you can tailor your network stack and cache implementation to your application’s needs. Since Apollo Client is decoupled from the view layer, React, Angular, and Vue developers can all benefit from these advancements.


## Get started 🚀


If you would like to test out Apollo Client 2.0, here are some example projects to get you started:


- **React:**[https://codesandbox.io/s/jvlrl98xw3](https://codesandbox.io/s/jvlrl98xw3)
- **Angular:**[https://github.com/kamilkisiela/apollo-angular-ssr](https://github.com/kamilkisiela/apollo-angular-ssr)
- **Vue:**[https://codesandbox.io/s/3vm8vq6kwq](https://codesandbox.io/s/3vm8vq6kwq)


We’d love to welcome you to our community and are eager to hear your feedback on the latest version of Apollo Client.[Here’s how to get involved](https://www.apollographql.com/docs/community/) !


---


*For more awesome GraphQL content, keep an eye on our YouTube channel to see more talks from GraphQL Summit 2017. Make sure to*[subscribe](http://youtube.com/apollographql) *so you don’t miss a beat!* 😊


Written by


Peggy Rayzis


[Read more by Peggy Rayzis](https://www.apollographql.com/blog/author/peggy)
