---
schema_version: "1.0.0"
document_id: "7b34d3547a720d56117735801ae046fb5ba8ff5f38cfae5df47def45085c1ce0"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-link-the-modular-graphql-network-stack-3b6d5fcf9244"
published_at: "2017-07-25T18:17:28+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:41f6931e929a00721b2e981f66311b5e2b6120d8c212621e211c9c67edcace8a"
---

# Apollo Link: The modular GraphQL network stack

When we started building Apollo Client, our goal was to empower people to use GraphQL in the way they want. This goal, and countless conversations with developers using GraphQL, drives all of our design decisions. (For example, we started by integrating with Redux because that’s what most React developers were familiar with.)


We found that there was a core set of data management features that people needed in a GraphQL client: caching, polling, pagination, batching and a few other things. One by one, the Apollo community added each of these to Apollo Client, culminating in the[1.0 release this April](https://dev-blog.apollodata.com/apollo-client-1-0-a-flexible-community-focused-javascript-graphql-client-2253b90e6c84) . Thanks to your feedback and contributions, Apollo Client is now the most widely used GraphQL client solution for every platform!


### Extensions and custom functionality


Along with the features baked into Apollo Client, there have always been hooks and extension points to change the functionality. The most important is the network interface, which controls how GraphQL operations are converted into network requests, and has became the focal point for adding custom functionality to Apollo Client.


We’re learning that everyone needs a different mix of features and tradeoffs in their GraphQL client library. Aside a set of core needs, such as making basic queries and mutations, developers require a slightly different set of surrounding functionality, including retries, live queries, diverse transports, alternative caching layers, and offline support. It’s clear that a single, monolithic library won’t be able to support all of these needs at once, and that adding all of these features to one codebase won’t scale.


## The solution:[Apollo Link](https://github.com/apollographql/apollo-link)


Today, we are introducing the first step towards a new GraphQL network management architecture that will allow you to compose modular parts to create the GraphQL client you want. In this architecture, you compose Links, which each implement one small part of a GraphQL network stack.


When you execute a GraphQL request, each Link’s functionality is applied one after the other. Links can perform polling, caching, retrying, queuing mutations, offline support and anything else by controlling the exact flow of each request.


Eventually, we plan to move more and more functionality that is currently in Apollo Client core into modular links, so you don’t have to rely on a single feature set built into the library. We believe this architecture will give you the best of both worlds — an easy get-started experience with a default set of features and the ability to add and remove functionality once you discover the specific needs of your app.


With Apollo Link, you will be able to access your GraphQL data exactly the way you want, completing our original goal.


### Try the prerelease today, and give us feedback!


Apollo Link is currently in[prerelease](https://github.com/apollographql/apollo-link/) , and you can use it today in place of the network interface on the latest Apollo Client, or as a standalone library. The first new feature we are introducing is the` RetryLink` , which enables you to retry GraphQL queries in the case of a transient network error.


The HTTP link implementation uses[apollo-fetch](https://github.com/apollographql/apollo-fetch) , which enables you to continue using middleware and afterware you’ve written for the current network interface.


### Using with Graph *i* QL


Apollo Link isn’t just a new network layer for Apollo Client. We’ve designed it so that it can be used in any situation where you need to make GraphQL requests.


The simplest way to see Apollo Link in action is to use it with GraphiQL. You can easily try the` RetryLink` by composing it with an` HttpLink` , and the result can be passed into Graph *i* QL:


Along with retries, the first version of Apollo Link also supports the ability to split your control flow depending on the GraphQL operation. This makes it easy to create a hybrid network stack that uses split to support subscriptions over websockets and single result GraphQL requests over HTTP.


Apollo Client with a visualized hybrid link ([actual implementation](https://github.com/apollographql/apollo-link/blob/master/docs/implementation.md) )


### Next steps


Now that we’ve put together the first version of the architecture, we’re excited to hear your feedback and hopefully see some of the first links built by the community. We’re going to follow up with some additional features, such as batching.


We’re excited by how many possibilities there are with the Apollo Link architecture: request deduplication, client-side execution, custom directives, simple offline support, pluggable caching, partial operation completion, sending a single request to multiple locations, and easy persisted queries.


Your contributions and suggestions are welcome in the` apollo-link`[repository](https://github.com/apollographql/apollo-link/) .


If you want to keep updated on Apollo Link, make sure to click the “Follow” button below, and follow us on Twitter at[@apollographql](https://twitter.com/apollographql) and the author at[@evanshauser](https://twitter.com/evanshauser) .


Thank you to[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----3b6d5fcf9244----------------------) ,[James Baxley](https://medium.com/u/fd3d3dea3cd3?source=post_page-----3b6d5fcf9244----------------------) , and[Jonas Helfer](https://medium.com/u/39cbd38c57c8?source=post_page-----3b6d5fcf9244----------------------) for their design help and incredible support!


Written by


Evans Hauser


[Read more by Evans Hauser](https://www.apollographql.com/blog/author/evans)
