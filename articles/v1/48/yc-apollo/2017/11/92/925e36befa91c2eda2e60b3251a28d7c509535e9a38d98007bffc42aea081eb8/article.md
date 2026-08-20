---
schema_version: "1.0.0"
document_id: "925e36befa91c2eda2e60b3251a28d7c509535e9a38d98007bffc42aea081eb8"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/universal-graphql-5d4260a9a1b5"
published_at: "2017-11-30T23:02:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:24.699163+00:00"
content_hash: "sha256:fb59789f754f5bc858fa5d604a3f554370d5f066bf8042eac637e2d4347e8a88"
---

# Universal GraphQL

Managing data can be pretty hard. As a team grows, the different ways you add to manage local state, fetch remote data, and keep the two in sync can slow down your app and, more importantly, your team. This is a problem area where GraphQL shines — once you get started with GraphQL, you’ll want to use it for every bit of data that your product needs. I had the privilege of talking about how and why to expand the reach of GraphQL in products at GraphQL Summit 2017.


You can watch the full talk below, or read on to see how you can use GraphQL everywhere today!


## Building a client for the future


Most people think of GraphQL as a new way to send HTTP requests that improves on the current pattern of RESTful architectures. This certainly has been the way it’s been adopted and there is tremendous value in doing so. However, one of the key benefits of GraphQL is that it is a language itself! As you learn GraphQL, you are learning a better way to describe the data requirements that your application has, and expecting the “network” to carry out the action that you told it. Now, imagine if you could manage all of your application state that way…


GraphQL is about declaring the data your app needs


In order to take full advantage of GraphQL, we need a more flexible way to pair data with our application. One that:


1. Can vary the execution of a request based on what it actually needs,
2. Has a well defined cache API to explore new ways to store data, and
3. Is fast enough and small enough to use everywhere we need data.


Those were the guiding principles behind Apollo Client 2.0.


There is a lot to be said about the 2.0 upgrade, and luckily there are already a few ways to learn about all of the new things you can do with it. I highly recommend checking out my colleague[Peggy Rayzis](https://medium.com/u/c827782c6410?source=post_page-----5d4260a9a1b5----------------------) ’s talk from[GraphQL Summit on Apollo Client 2.0](https://dev-blog.apollodata.com/apollo-client-2-0-beyond-graphql-apis-888807b53afe) . You can also read the[announcement post](https://dev-blog.apollodata.com/apollo-client-2-0-5c8d0affcec7) or check out the[upgrade guide](https://www.apollographql.com/docs/react/2.0-migration.html) .


## Going universal


Equipped with a brand new GraphQL client, you can start to explore the different ways this can change how you work as a team, how incremental changes to our application can be made, and how our services can remain small but become smarter.


### Designing with real data


In 2016, the leading edge of design was pushed farther thanks to[Jon Gold](https://medium.com/u/5056df9f86a9?source=post_page-----5d4260a9a1b5----------------------) , who introduced us to[React Sketch.app](https://airbnb.design/painting-with-code/) . With it design and engineering teams could work together better than ever before by sharing actual UI code that would be used in production. The bigger value, though, is the ability to programmatically load data into your designs instead of using the folder of stock images you have so carefully cultivated over the years. Using data from your GraphQL endpoint, you can select the fields you need, apply the arguments to get just what you want, and design for real people using real data.


Using GraphQL with React Sketch.app


### Incremental adoption


Using GraphQL shouldn’t require throwing out all of the hard work you have already put into your application. Since it is a flexible language, it can be used to describe the data you already have in a better way. With Apollo Link, it is possible to adjust how the operation is executed to do some amazing new things. During my talk I introduced a couple of these possibilities, which are already being worked on!


The first is the ability to write GraphQL and have it fetch from your existing API endpoints. This makes for an easy migration path as your team implements a GraphQL endpoint. It also lets you start to experiment with schema design in an ad-hoc way before building out the full endpoint. This is a great project to get involved with if you are thinking about trying out GraphQL and already have a built API! Checkout the design discussions[here](https://github.com/apollographql/apollo-link-rest/pull/1) .


Using REST endpoints with GraphQL


The second possibility is a way to manage local application state using GraphQL. This is so exciting because it is one of the biggest points where teams have to learn multiple ways of managing data. Forget the application state libraries you use today and just write GraphQL instead! This project has already been started at the[apollo-link-state repo](https://github.com/apollographql/apollo-link-state) and I would love to get feedback on it!


## Architectural improvements


GraphQL has a ton of benefits, but many have been afraid of moving to a single monolith endpoint. All of the hard battles fought to move to microservices shouldn’t be in vain; it turns out GraphQL can actually be used to make it *easier* for these services to work together.


Using GraphQL’s type system, you can join microservices of GraphQL endpoints together into a single place for clients to query against. To quote my teammate[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----5d4260a9a1b5----------------------) :


Schema stitching is a simple concept: GraphQL makes it easy to combine multiple APIs into one, so you can implement different parts of your schema as independent services. These services can be deployed separately, written in different languages, or maybe even owned by different organizations


Building out the links between schemas can even be done by writing GraphQL fragments. This is shown in this[Apollo Launchpad](https://launchpad.graphql.com/8r11mk9jq) , where you can even try out the two merged schemas.


Schema stitching two API’s together


## GraphQL everywhere


During GraphQL Summit it was amazing to see how many incredible ways people were using GraphQL to build things. There was a ton of energy around new paradigms on the server, better transports layers, and smarter clients.[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----5d4260a9a1b5----------------------) gave an[incredible keynote about how the modern GraphQL stack fits together](https://dev-blog.apollodata.com/the-graphql-stack-how-everything-fits-together-35f8bf34f841) , but I’m also so excited about the growth of where GraphQL is used to manage data. One of the most fun things at Summit was that in the GraphQL Garage,[OK GROW!](https://medium.okgrow.com/) had an IOT golf ball that was powered by GraphQL mutations you could run. You could literally golf with GraphQL!


This is only the beginning in my mind. With tools like Apollo Client 2.0,[Apollo Engine](https://dev-blog.apollodata.com/introducing-apollo-engine-insights-error-reporting-and-caching-for-graphql-6a55147f63fc) , and Apollo Codegen we are already starting down a new path for managing data.


---


## Let’s build it together


GraphQL opens the door to a lot of new possibilities that will transform how we work with data in our applications. If you want to be a part of it, check out our[getting started page](https://www.apollographql.com/docs/community/) and join us in building what’s next!


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
