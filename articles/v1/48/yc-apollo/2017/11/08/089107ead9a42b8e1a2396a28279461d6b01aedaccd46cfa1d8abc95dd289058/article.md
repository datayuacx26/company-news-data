---
schema_version: "1.0.0"
document_id: "089107ead9a42b8e1a2396a28279461d6b01aedaccd46cfa1d8abc95dd289058"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/aws-appsync-powered-by-apollo"
published_at: "2017-11-29T23:06:11+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:54.348132+00:00"
content_hash: "sha256:9b7409f78d155347a773213f69f36bc9e8579ecb18bd6e8577c155307493b300"
---

# AWS AppSync, built with Apollo

Yesterday at reInvent,[Amazon launched AppSync](https://aws.amazon.com/blogs/aws/introducing-amazon-appsync/) , a new GraphQL service featuring real-time updates and built-in offline support. Managed in the AWS console, AppSync’s GraphQL layer aggregates data from other AWS products such as ElasticSearch and DynamoDB. In your application, you query your GraphQL API with the AppSync Client, available for web and native.


The AppSync announcement got a lot of love from the developer community, and it’s easy to see why people are excited about GraphQL! Architecting your application with GraphQL enables you to build complex features quickly by simply describing your data requirements and letting your GraphQL layer take care of the rest. Companies like[the New York Times](https://open.nytimes.com/the-new-york-times-now-on-apollo-b9a78a5038c) ,[Coursera](https://dev-blog.apollodata.com/courseras-journey-to-graphql-a5ad3b77f39a?_ga=2.74751445.653517459.1511971836-1820713705.1508389092) , and[Express](https://dev-blog.apollodata.com/changing-the-architecture-of-express-com-23c950d43323) have led the way by powering their most important applications with GraphQL.


Naturally, we were curious to learn more about AppSync. Once we dug into the open-source code, we were excited to see that the AppSync Client implementations are based on the same MIT-licensed Apollo technology you know and love:[Apollo Client 2.0](https://github.com/apollographql/apollo-client) ,[Apollo iOS](https://github.com/apollographql/apollo-ios) , and[Apollo Codegen](https://github.com/apollographql/apollo-codegen) ! Read on to learn how different components of these Apollo projects power some of the coolest features of the AppSync platform.


## Leveraging Apollo Client’s extensibility


Last month, we officially released[Apollo Client 2.0](https://dev-blog.apollodata.com/apollo-client-2-0-5c8d0affcec7) at GraphQL Summit 2017. In addition to making it smaller and faster, one of our main goals was to also make it much more modular. You now have the ability to plug in your own cache and tailor your network stack to your application’s requirements with[Apollo Link](https://www.apollographql.com/docs/link/) .


It turns out that Apollo Client 2.0’s modular architecture was a huge win for developers looking to customize their GraphQL client, because that’s exactly how the AWS team was able to build[AppSync Client](https://github.com/awslabs/aws-mobile-appsync-sdk-js/blob/254930e3de1211c20bbac3efadf571008826bc32/packages/aws-appsync/src/client.js) !


AWS AppSync leverages the modular architecture of Apollo Client


In only a few weeks, the AWS team was able to make complex features like offline support and authentication a reality by extending the Apollo` InMemoryCache` and using Apollo Link to customize their network interface. We think this is a testament to the extensibility of Apollo Client and hope their example inspires more teams to experiment with personalizing their GraphQL client. Let’s dive into how AWS accomplished this.


### Apollo Link enables offline support


To achieve offline support, the AWS team created a[custom Apollo Link](https://github.com/awslabs/aws-mobile-appsync-sdk-js/blob/254930e3de1211c20bbac3efadf571008826bc32/packages/aws-appsync/src/link/offline-link.js) that takes a Redux store augmented by[redux-offline](https://github.com/redux-offline/redux-offline) for persistence. This link also queues up any pending mutations and calls to refetch queries when the device is offline and stores them in a Redux subtree to be executed when the device goes back online. Since the AppSync Client is based on Apollo Client, you still get the benefit of optimistic UI responses even if the device is offline when the mutation is fired.


Using AWS SDK for Mobile development will give you Automatic offline support out of the box[@saligramkarthik](https://twitter.com/saligramkarthik?ref_src=twsrc%5Etfw)[#reInvent](https://twitter.com/hashtag/reInvent?src=hash&ref_src=twsrc%5Etfw)[pic.twitter.com/D9okawhVbj](https://t.co/D9okawhVbj)


— AWS Events (@AWSEvents)[November 28, 2017](https://twitter.com/AWSEvents/status/935630189312143361?ref_src=twsrc%5Etfw)


We’re also working on improving the offline experience to create easy persistence that isn’t dependent on Redux and advanced features such as applying a timeout to the offline mutation queue to avoid potential conflicts. If you’d like to learn more about these initiatives, come[join the party](https://github.com/apollographql/apollo-cache-asyncstorage/issues/6) on the` apollo-cache-asyncstorage` repo! 🎉 We’d love to hear your input and welcome any contributions.


### Real-time data with GraphQL subscriptions


One AppSync feature that generated a lot of buzz is its emphasis on real-time data. Under the hood, AppSync’s real-time feature is powered by GraphQL subscriptions. While Apollo bases its subscriptions on WebSockets via[subscriptions-transport-ws](https://github.com/apollographql/subscriptions-transport-ws) , subscriptions in GraphQL are actually flexible enough for you to base them on another messaging technology. Instead of WebSockets, AppSync’s subscriptions use MQTT as the transport layer.


The AWS team even[wrote their own Apollo Link](https://github.com/awslabs/aws-mobile-appsync-sdk-js/blob/254930e3de1211c20bbac3efadf571008826bc32/packages/aws-appsync/src/link/subscription-handshake-link.js) to execute subscriptions over MQTT, which we think is a really cool use case. If you’d like to integrate real-time features into your application in only a few lines of code with Apollo Link,[please check out our example](https://www.apollographql.com/docs/react/features/subscriptions.html#subscriptions-client) which serves queries & mutations over HTTP and subscriptions over WebSockets. We’d also like to hear from you on the[subscriptions-transport-ws](https://github.com/apollographql/subscriptions-transport-ws) repo if you have any suggestions on how to improve our transport layer.


## Native client with Apollo iOS & Apollo Codegen


Another cool feature of AWS AppSync is that it’s compatible with different platforms. It works with JavaScript and also with native platforms written in Swift. One of our main goals for Apollo is that you can use it with any type of architecture, which is why we collaborated with the community on[Apollo iOS](https://github.com/apollographql/apollo-ios) and[Apollo Android](https://github.com/apollographql/apollo-android) . It’s exciting to see AppSync builds on that foundation to power their[native client](https://github.com/awslabs/aws-mobile-appsync-sdk-ios/tree/3fbd248440115d1b913fabc9290eb9d7d4bc9a9b/AWSAppSyncClient/Apollo/Sources/Apollo) !


AppSync’s iOS client is based on Apollo iOS


To generate Swift types from AppSync’s GraphQL layer, AWS uses a[customized version of Apollo Codegen](https://github.com/awslabs/aws-appsync-codegen/tree/6c5ddf4e589813695d4bdcca665776d4614205c7) .[Apollo Codegen](https://github.com/apollographql/apollo-codegen) is a build tool that uses your GraphQL schema and queries to output types for a variety of different language targets: Flow, TypeScript, Java, Scala, and Swift.


We’re constantly working with the community to continuously improve the language targets; for example,[Lewis Chung](https://medium.com/u/2ab2eb7f698c?source=post_page-----df61eb706183----------------------) from Coursera[published a revision](https://github.com/apollographql/apollo-codegen/pull/296) to the Flow & TypeScript targets just this week! If you’re interested in generating static types from GraphQL and would like to strengthen the generated code or support additional targets like Reason, please reach out! We’d love to welcome you on board.


---


## A bright future 🚀


AWS AppSync is an excellent example of how teams can create innovative products quickly by using the Apollo family of tools. We’re so excited to see that Apollo works well as an extensible platform for developing customized GraphQL client tooling.


With our solid foundation in place, we can’t wait to see more people build upon the Apollo platform to push the GraphQL ecosystem forward! If you’re interested in collaborating with us to create the next generation of GraphQL tooling,[learn more](https://www.apollographql.com/docs/community/) on our contributor site. We’d love to hear from you! 🤗


Written by


Peggy Rayzis


[Read more by Peggy Rayzis](https://www.apollographql.com/blog/author/peggy)
