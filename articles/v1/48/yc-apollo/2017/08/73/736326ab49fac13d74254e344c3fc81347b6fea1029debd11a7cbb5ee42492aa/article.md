---
schema_version: "1.0.0"
document_id: "736326ab49fac13d74254e344c3fc81347b6fea1029debd11a7cbb5ee42492aa"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-link-creating-your-custom-graphql-client-c865be0ce059"
published_at: "2017-08-10T18:03:45+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:3a64b9bb3b6f2a28267cb91b922a649c7cca6ade430764b2d63a29e4776260ed"
---

# Apollo Link: Creating your custom GraphQL client

You’ve probably encountered a library before which serves all of your needs, except for a handful of unsupported corner cases. Even though they might not be widely needed features, they cause a huge pain in your app. That’s why we’re working on a library called Apollo Link, which addresses this issue for today’s GraphQL clients by providing a framework to give you complete control over GraphQL request control flow and result resolution.


To speed up development time, the Apollo Link package contains a collection of Links that each perform a discrete function commonly found in GraphQL network stacks. The behavior of these links ranges from simple HTTP requests, to subscriptions over websockets, to modifying request control flow with retrying, polling, batching, and deduplicating.


These basic links, together with any custom links you build for your app, can be composed together to combine their functionality, creating a GraphQL client with custom behavior. When a link is added to the middle of a chain, the link applies its modification to the request’s control flow, such as retrying. The resulting link can return a GraphQL` ExecutionResult` (data + errors). This can be anything from mocked data, local application state, or, more often, the result of a request to your GraphQL Server.


The HTTP Link


To support the diverse behavioral requirements of GraphQL applications, each GraphQL response is represented by an Observable. Observables expose a subscribe method that takes three callbacks: next, error, and complete. Next can be called zero, once, or many times, until either the error or complete callbacks are made. This structure of callbacks is ideal because it provides the power to represent current and planned GraphQL results, including queries, mutations, subscriptions, and even live queries.


## Creating your first Apollo Link


In this blog post, we will gradually build up a custom GraphQL client with Apollo Links that provides results to GraphiQL.


The foundation of all GraphQL clients, is fetching a GraphQL result. So we’ll start by building a link that can make an HTTP request. This link will be a helpful example for other custom transports, such as using a Firebase endpoint, XHR, a REST endpoint, or other GraphQL endpoint. All Links inherit from a base class that provides the methods for composition, and are required to implement the` request` method.


Check out the implementation in the CodeSandbox below:


## Creating an Apollo Link Composition


Now that we have our custom transport link, the next goal is to add some request control flow with links. To illustrate, we are going to create one link that catches errors and routes them to GraphQL data and another link that logs the operations and results. The logging link will be applied first, then the catch link; finally, the http link will fetch the request.


Ordering of Link Application


In order to make the chain work, the logging and catch link need access to the next link. To accomplish this, the` request` method has access to the` forward` argument, a function that pass the operation to the next link returns the observable result. Finally, we create a composed chain of the links with the static` from` method of` ApolloLink` .


See the implementation of the chain below:


## Modifying specific GraphQL requests


Great! Now we have the ability to control the flow of a request and response through our GraphQL client. What if we want different behavior for mutations and queries? This example uses a different endpoint for mutations, reroutes errors on queries only, and logs all requests. Additionally we might want to selectively modify an operation while using the same transport, for example adding another variable to a query with a certain operation name. ApolloLink provides the ability to specify a certain portion of a composition to run depending on the operation with the` split` method.


Look at the implementation below:


## Building your own GraphQL client


Awesome! Now we have the ability to create a completely custom GraphQL client that can run logic and fetch results on a per query basis. Apollo Link provides many basic links that can be combine and extended with your own links to create the behavior required by your application. You can find the currently offered links in the[Apollo Link repository](https://github.com/apollographql/apollo-link/) .


The Apollo Link ecosystem is constantly evolving, and entirely community driven. If you are interested in contributing, or shaping the future of GraphQL client customization, please open an issue or pull request. If you want some ideas of how to start contributing, join the[Apollo Slack](https://www.apollodata.com/#slack) and send me (evans) a private message!


In the future, look out for posts about communicating between links using the context field in the GraphQL operation, stateful vs stateless links, publishing your own link configuration, adding type safety, and more!


If you want to keep updated on Apollo Link, make sure to click the “Follow” button below, and follow us on Twitter at[@apollographql](https://twitter.com/apollographql) and the author at[@evanshauser](https://twitter.com/evanshauser) .


Thank you to[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----c865be0ce059----------------------) ,[James Baxley III](https://medium.com/u/fd3d3dea3cd3?source=post_page-----c865be0ce059----------------------) , and[Jonas Helfer](https://medium.com/u/39cbd38c57c8?source=post_page-----c865be0ce059----------------------) for greatly informing the Apollo Link design process and their continued support of the author!


Written by


Evans Hauser


[Read more by Evans Hauser](https://www.apollographql.com/blog/author/evans)
