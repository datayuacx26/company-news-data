---
schema_version: "1.0.0"
document_id: "e925654962f8f7110a3311e8f3f14889bafc5a53799cfdefe1595c2f6d9a8df2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/its-here-introducing-apollo-server-3"
published_at: "2021-07-15T10:01:04+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:68e0d6137313a97974a9a85a4192cae075e0528c28fbbacb08b08c47d87946e5"
---

# It’s here! Introducing Apollo Server 3

When we released Apollo Server 2 in 2018, our goal was to make it the best-in-class GraphQL server by integrating many of the most useful GraphQL libraries available at the time. However, for backward compatibility reasons, this meant that Apollo Server 2 continued to include the 2018 versions of these libraries three years later. Apollo Server 3.0.0 changes that with a modernized codebase designed to serve as a scalable foundation for Apollo Server users well into the future.


### A more modern, more flexible Apollo Server


The many hardcoded dependencies in Apollo Server 2 made it increasingly difficult over time to build the new features that our users needed most. Because of that, our main focus when designing the initial release of Apollo Server 3 was to make it more flexible by removing as many hardcoded dependencies as possible. At the same time, we also prioritized increasing extensibility so that any capabilities lost in removing dependencies could easily be added back in and, unlike in v2, be easily upgraded over time.


Because of the approach we took, you may notice that most of the changes in 3.0.0 are under-the-hood improvements rather than new features which build upon recent 2.x versions. But, while it’s not packed with new features quite yet, what 3.0.0 does provide is a new foundation that will allow us to vastly improve our feature delivery velocity in the coming months. Here’s a quick look at some of the new things you may notice in 3.0.0 and a little sneak peek of what’s coming to Apollo Server 3 in the near future:


### New in 3.0.0


One of the main things you’ll notice in 3.0.0 is that[Apollo Sandbox](http://studio.apollographql.com/sandbox) is now suggested as the main tool for querying your server. Apollo Server 2 shipped with GraphQL Playground, but that project was[retired last year](https://github.com/graphql/graphql-playground/issues/1143) . Instead of only supporting a single frontend, Apollo Server 3 has a “landing page” API. You can keep using the retired GraphQL Playground by adding a single line of code to your app, but the default landing page helps you and your users use the actively maintained Apollo Sandbox tool. Sandbox provides a number of UX improvements over GraphQL Playground like one-click query building, intelligent field and path search, and even more, which you can read about in our[Sandbox announcement blog](https://www.apollographql.com/blog/announcement/platform/apollo-sandbox-an-open-graphql-ide-for-local-development/) .


3.0.0 also includes a number of less visible changes which improve the UX and extensibility of Apollo Server:


- Apollo Server 3 now supports the latest version of Fastify (v3) and Hapi. We’ve made it easier to understand what web framework versions are supported by using peer dependencies more consistently.
- Apollo Server’s AWS Lambda and Google Cloud Functions integrations are now built on top of our Express integration rather than reimplementing a subset of its logic from scratch. This means we support new Lambda integrations such as Application Load Balancers, and it will be much easier for new Apollo Server functionality to work in Lambda and Cloud Functions without having to write the same code three times. Additionally, because it now uses Express internally, you can extend your Lambda or Cloud Functions server’s HTTP functionality via standard Express middleware by using the new` expressAppFromMiddleware` option to` createHandler.`
- Bad request errors are handled in a more consistent manner across integrations and consistently return HTTP responses with a 4xx status
- We’ve dropped support for Node 8 and 10 (which are no longer in LTS), which lets the TypeScript compiler output more efficient code that takes advantage of newer Node features.
- Apollo Server 2 supported releases of graphql-js dating back to 2017’s 0.12.0. Apollo Server 3 requires the more modern 15.3.0 release of graphql-js, allowing us to rely on the last four years of added functionality.
- We’ve refreshed all of our documentation. For example, check out our[new page which directly compares the different “flavors” of Apollo Server and helps you decide which one to use](https://www.apollographql.com/docs/apollo-server/integrations/middleware/) .


### What’s coming next?


Now that we’ve modernized the Apollo Server codebase, we’re in a great position to start making big improvements. Here’s some of what we’re working on next.


**Response-shape-based usage reporting** . Currently, getting insights into your Apollo Server’s usage and performance in Apollo Studio requires creating a detailed execution trace of every operation. While this gives you a very complete and detailed understanding of what’s going on in your graph, it can have a performance impact. Additionally, not every language’s GraphQL library has all the hooks required to make these traces, so if your federated subgraph doesn’t use one of the supported libraries you won’t get as much clarity in Studio. In a future release of Apollo Server 3, we plan to implement “response-shape-based reporting”, which enables most of Studio’s features without detailed per-field execution traces. This will improve your graph’s performance and let you use any GraphQL server for your subgraph without loss of Studio functionality.


**Additional directive support to improve response latency** . One of the disadvantages of the GraphQL response/request model is that the response is not returned to the clients until the entire request has finished processing. We are excited to get started on adding support for additional directives such as` @defer` which allow responses to return in multiple payloads, prioritizing the most important data first.


### Getting started with Apollo Server 3


We’re excited to get Apollo Server 3 in the hands of our community, and we would love to hear what you think as you start to use it! If you’re starting from scratch, our[getting started docs](https://www.apollographql.com/docs/apollo-server/getting-started/) will walk you through setting up Apollo Server for the first time, or, if you’re already using Apollo Server 2, our[migration guide](https://www.apollographql.com/docs/apollo-server/migration/) will help you upgrade. If you’d like to leave us any feedback on Apollo Server 3, we encourage you to do so by reaching out on[https://community.apollographql.com](https://community.apollographql.com/) or[GitHub](https://github.com/apollographql/apollo-server) .


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
