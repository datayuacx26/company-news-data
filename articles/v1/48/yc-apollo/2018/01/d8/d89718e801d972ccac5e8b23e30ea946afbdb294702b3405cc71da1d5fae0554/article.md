---
schema_version: "1.0.0"
document_id: "d89718e801d972ccac5e8b23e30ea946afbdb294702b3405cc71da1d5fae0554"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/2017-the-year-in-graphql-124a050d04c6"
published_at: "2018-01-03T22:12:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:119b6807a648a15cdb7836114af61be5371e27a23b077c7beb29db8c11ef3dd4"
---

# 2017: The year in GraphQL

GraphQL was announced and initially open-sourced in 2015, with relatively little fanfare. I think even[Lee](https://medium.com/u/d9b1a61823fa?source=post_page-----124a050d04c6----------------------) , Dan, and[Nick](https://medium.com/u/835451854df2?source=post_page-----124a050d04c6----------------------) , the creators of GraphQL, didn’t expect that they had created a pivotal technology that would be a global phenomenon just two years later.


On the Apollo team, our primary goal is the overall growth of GraphQL, which we see bringing transformative change to thousands of organizations. In 2017, we really saw GraphQL take off. It has grown from a niche technology with a tight-knit community to something that is now being used in production by thousands of serious companies.


## Companies made big bets on GraphQL


One of the most exciting things to see is when established teams with serious production workloads make big bets on GraphQL. One of the possible worries with a cool new technology is that it never moves beyond the sphere of small startups and hipsters, but thankfully GraphQL is especially useful at scale, since it can improve performance and team collaboration for large apps.


- **Walmart:** The team at Walmart Labs[published their Clojure GraphQL server implementation](https://medium.com/walmartlabs/open-sourcing-lacinia-our-graphql-library-for-clojure-96a4ce5fc7b8) for everyone to use!
- **IBM:**[Jason Lengstorf](https://medium.com/u/6a98eb806571?source=post_page-----124a050d04c6----------------------) from IBM gave a great presentation at GraphQL Summit about[how GraphQL is used to combine multiple microservices](https://www.youtube.com/watch?v=T3FbZsYXi50) .
- **New York Times:** As part of their new architecture, the New York Times is[moving to GraphQL](https://open.nytimes.com/react-relay-and-graphql-under-the-hood-of-the-times-website-redesign-22fb62ea9764) for their API layer, and recently posted about how they are going to be[using Apollo](https://open.nytimes.com/the-new-york-times-now-on-apollo-b9a78a5038c) as well.
- **KLM/Air France:** Ugur and Tom from KLM gave a great talk at GraphQL Summit about how they went about[building a new ticketing experience with GraphQL](https://www.youtube.com/watch?v=T2njjXHdKqw&list=PLpi1lPB6opQywks7yYYs5jJAIRI3faRTm&index=8) , that will need to serve over 90 million passengers a year.


For me, these kinds of developments have are the most important thing for the community, since this will give more confidence to developers at companies all around the world to switch to GraphQL for their APIs.


## Major advancements in GraphQL


This has been by far the most active year so far in the GraphQL community, with a lot of huge steps forward. These developments are paving the way for GraphQL to become one of the primary ways people work with their data.


With major additions to the specification, two world-class conferences, significant open source releases, and more, it’s starting to be hard to keep up with the whirlwind of announcements. So if you aren’t totally up to date, read below for our rundown of the top events in the GraphQL community in 2017, in chronological order.


### GraphQL subscriptions added to the spec in March


Last year we saw the addition of the biggest new feature to the GraphQL specification since its original release: Subscriptions. GraphQL subscriptions are a way to deliver realtime events from a GraphQL server, while letting clients specify the shape of data they want to receive using a GraphQL query.


From Lee Byron’s[talk at GraphQL SF](https://www.youtube.com/watch?v=bn8qsi8jVew&t=4s) about subscriptions


This feature was discussed in an[RFC](https://github.com/facebook/graphql/pull/267) , then added officially as part of[Rob Zhu’s PR](https://github.com/facebook/graphql/pull/305) to the spec. We’ve been excited about real time data in GraphQL and subscriptions specifically for[a long time](https://dev-blog.apollodata.com/a-proposal-for-graphql-subscriptions-1d89b1934c18) , and it was great to see this land in the core. To get the most up-to-date explanation of subscriptions, check out[Robert](https://medium.com/u/3e6b69951eef?source=post_page-----124a050d04c6----------------------) ’s[recent talk at GraphQL Summit 2017](https://www.youtube.com/watch?v=PsjiL6Yl1ag) .


### Apollo Client and other libraries reach 1.0 in April, then 2.0


One of our main focuses over the last few years has been creating a really productive experience for using GraphQL inside your frontend code.[Apollo Client](https://www.apollographql.com/client/) started in late 2015 as a small experiment, and is now the most popular way to integrate GraphQL data with React and other frontend platforms.


This year saw several major milestones in the project, as we shipped[Apollo Client 1.0](https://dev-blog.apollodata.com/apollo-client-1-0-a-flexible-community-focused-javascript-graphql-client-2253b90e6c84) in April to solidify the APIs that had been in use for almost a year, then[2.0](https://dev-blog.apollodata.com/apollo-client-2-0-5c8d0affcec7) which kept largely the same API but added tons of new extension points to unlock new ways of using GraphQL on the frontend. A lot of these are enabled by[Apollo Link](https://www.apollographql.com/docs/link/) , a new abstraction for composable networking. To get all the details about the new capabilities, watch[Peggy](https://medium.com/u/c827782c6410?source=post_page-----124a050d04c6----------------------) ’s[talk from GraphQL Summit](https://www.youtube.com/watch?v=eHjP2WFt0zU) .


One of the newest developments enabled by Apollo Client 2.0 is the ability to manage client-side state as well, and you can read about the first developments in that direction in our post from just two weeks ago:[The future of state managementManaging your local data in Apollo Client with apollo-link-statedev-blog.apollodata.com](https://dev-blog.apollodata.com/the-future-of-state-management-dd410864cae2)


Other Apollo libraries like[Apollo Server](https://dev-blog.apollodata.com/apollo-server-1-0-a-graphql-server-for-all-node-js-frameworks-2b37d3342f7c) and[graphql-tools](https://dev-blog.apollodata.com/graphql-tools-a-simpler-way-to-create-graphql-apis-eadf018f3766) also hit 1.0.


### Relay Modern released in April


When GraphQL was originally announced by Facebook, it was part of the release of[Relay](https://facebook.github.io/relay/) , a client library that aimed to make data loading and routing in React more declarative. Since GraphQL was open sourced, it’s been evolving rapidly, and so Relay has evolved as well. This year, we saw the release of a rewrite of Relay from the ground up, and a 1.0 release marking the new API as production-ready.


In particular, Relay Modern focuses on encouraging statically defined queries and using that to enable additional performance optimizations and a simpler developer experience. Check out the[official announcement post](https://code.facebook.com/posts/1362748677097871/relay-modern-simpler-faster-more-extensible/) , and[our blog post](https://dev-blog.apollodata.com/exploring-relay-modern-276f5965f827) covering some of the differences between the previous versions and the 1.0 release.


### GraphQL Europe in May


[GraphQL Europe](https://2017.graphql-europe.org/) , the first GraphQL conference in Europe organized by Graphcool and Honeypot, was a great opportunity for the European community to come together. It was a great one-day event in Berlin with talks about some of the most exciting GraphQL projects out there. Check out the[playlist on YouTube for the videos](https://www.youtube.com/playlist?list=PLn2e1F9Rfr6n_WFm9fPE-_wYPrYvSTySt) . GraphQL Europe is happening again in 2018, and[you can get tickets now](https://www.graphql-europe.org/) !


### Official launch of Graphcool in May


Graphcool has been one of the easiest ways to get a GraphQL backend up and running for a long time, and this year they[officially launched](https://www.producthunt.com/posts/graphcool-2) the first complete version of the platform. With Graphcool, you get the ease of setup that you would expect from a backend as a service like Firebase, but you can retain control over your data and build your app around the open GraphQL standard, so you’re not locked in to a proprietary set of technologies.


In October, Graphcool made a big step in making their platform more accessible to developers around the world,[open sourcing their core functionality](https://blog.graph.cool/introducing-the-graphcool-framework-d9edab2a7816) . This is one of the first steps towards a future where Graphcool works more like a[GraphQL database](https://blog.graph.cool/graphql-databases-a-preview-into-the-future-of-graphcool-c1d4981383d9) than a backend as a service, making it easier to start migrating your backend to GraphQL incrementally, and use Graphcool alongside existing data sources. Learn more about Graphcool and GraphQL-native backends from[Johannes](https://medium.com/u/ff75ba3095a6?source=post_page-----124a050d04c6----------------------) ’[talk at GraphQL Summit this year](https://www.youtube.com/watch?v=WhYjSFKNhBA&list=PLpi1lPB6opQywks7yYYs5jJAIRI3faRTm&index=30) .


### GraphQL Summit 2017 in October


This year was the second edition of[GraphQL Summit](https://summit.graphql.com/) , the GraphQL conference in the United States. This year’s event in San Francisco was expanded to two days with over 30 talks about the hottest topics in GraphQL.


[Check out the full playlist of talk videos here](https://www.youtube.com/playlist?list=PLpi1lPB6opQywks7yYYs5jJAIRI3faRTm) .


For me, GraphQL Summit was a great representation of all of the excitement about GraphQL that had been building up over the course of 2017. There were talks covering almost every major topic of interest from the community, and it was amazing to see how excited people from all kinds of different backgrounds and industries were about the technology.


### Apollo Engine launch in October


Around the time of GraphQL Summit, we launched a new product that’s the first of its kind:[Apollo Engine](https://www.apollographql.com/engine/) . Engine is something we’re calling a “GraphQL Gateway”, and it’s a component that lives between your client and your GraphQL server to give you advanced features like result caching, performance tracing, error tracking, persisted queries, and more.


We hope that putting more complex functionality into a reusable gateway component will help people make their GraphQL server simpler and easier to maintain, in the same way that technologies like caching for REST APIs lived in separate infrastructure layers rather than cluttering your server code.[Read the announcement post to get all of the details.](https://dev-blog.apollodata.com/introducing-apollo-engine-insights-error-reporting-and-caching-for-graphql-6a55147f63fc)


### AWS AppSync announced in November


In a big validation of GraphQL as a mainstream technology, AWS selected it as the API layer for their new service for building apps on top of AWS backend techologies like DynamoDB and Elasticsearch. I think it’s great that services like this can rely on a common API specification rather than having to invent their own, which also enables better interoperation between different tools.


Speaking of tools, when the service launched we were excited to see that[the frontend implementations were based on Apollo Client](https://dev-blog.apollodata.com/aws-appsync-powered-by-apollo-df61eb706183) ! It was exciting for us to see that the work we put into making Apollo Client 2.0 more flexible has enabled people to build new experiences on top of it.


## Going into 2018


This last year was absolutely incredible — compared to the initial excitement for GraphQL that we saw in 2016, 2017 was a huge leap forward. And based on what we’re seeing, 2018 is going to be just like that again. We’re going to see more and more core developer tools companies ship GraphQL products, more integrations and tools, and more content about how developers are finding GraphQL a valuable part of their workflow.


And we won’t be waiting around, either. I had the privilege of being the first developer working on what is now called Apollo, and today I couldn’t be more excited to see how the project has grown and how the community has come together to build tons of great tools to improve the lives of developers everywhere. I’m super excited about what we’re going to be doing in 2018, and you’ve already seen the start with the launch of[Apollo Engine](https://dev-blog.apollodata.com/introducing-apollo-engine-insights-error-reporting-and-caching-for-graphql-6a55147f63fc) and[client-side state management](https://dev-blog.apollodata.com/the-future-of-state-management-dd410864cae2) late last year.


If you want to get involved, please contribute to any Apollo project that catches your eye[on GitHub](https://github.com/apollographql) . 2018 is going to be great!


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
