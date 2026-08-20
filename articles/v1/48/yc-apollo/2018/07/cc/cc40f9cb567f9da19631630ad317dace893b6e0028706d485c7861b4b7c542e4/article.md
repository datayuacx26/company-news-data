---
schema_version: "1.0.0"
document_id: "cc40f9cb567f9da19631630ad317dace893b6e0028706d485c7861b4b7c542e4"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/announcing-apollo-server-2-2b69fb4702ce"
published_at: "2018-07-19T17:45:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:e1e4152a47cc6e684b6bc05bd00ff3895b50c35aae53bbae7f77d78a92ffce6d"
---

# Announcing Apollo Server 2

As we watch teams adopt Apollo, one lesson we’ve learned is how important it is for product engineering teams that use GraphQL to be able to work on their schema as a natural part of their development process. So we think it’s vital that there be a full-featured GraphQL layer in JavaScript with all the latest GraphQL innovations and techniques, all wired up with the machinery and tooling that makes it a complete solution.


The GraphQL API at the center of your architecture


So today, after long and productive collaboration with hundreds of Apollo developers and teams in production, we’re excited to announce a new major release of the Apollo GraphQL Server that addresses the biggest needs of product engineering teams and sets them up for long-term success.


Based on our experience working with teams building scalable, performant GraphQL APIs, we focused on three areas for Apollo Server 2:


- **Improving the developer experience** : making it easy and straightforward for product engineers to stand up a GraphQL API, with minimal boilerplate and configuration.
- **Baking in the best development patterns** : including common patterns for[error handling](https://blog.apollographql.com/full-stack-error-handling-with-graphql-apollo-5c12da407210) ,[testing and mocking](https://www.apollographql.com/docs/apollo-server/v2/features/mocking.html) ,[subscriptions](https://www.apollographql.com/docs/apollo-server/v2/features/subscriptions.html) ,[file uploads](https://blog.apollographql.com/file-uploads-with-apollo-server-2-0-5db2f3f60675) , as well as implementations of[automatic persisted queries](https://blog.apollographql.com/automatic-persisted-queries-and-cdn-caching-with-apollo-server-2-0-bf42b3a313de) ,[CDN integration](https://blog.apollographql.com/automatic-persisted-queries-and-cdn-caching-with-apollo-server-2-0-bf42b3a313de) , and[schema and query metrics](https://blog.apollographql.com/apollo-server-2-0-performance-and-error-reporting-built-in-baa6158f3c09) that are critical in production.
- **Layering GraphQL over existing APIs and services** : defining[Data sources](https://blog.apollographql.com/easy-and-performant-graphql-over-rest-e02796993b2b) , a new abstraction that manages connections to REST endpoints and implements standard patterns for cross-request partial query caching.


Apollo Server 2 is an easy transition for teams using Apollo Server 1.x or` express-graphql` . After a complete beta and RC cycle, many companies are already running Apollo Server 2 in production. Follow the[short migration guide](https://www.apollographql.com/docs/apollo-server/v2/migration-two-dot.html) to find out how.


Read on for a closer look at what’s inside.


## Improving the developer experience


Apollo Server 2 contains everything you need to build a GraphQL layer, all wired up for you. To get started, install the` apollo-server` package:


```text
npm install apollo-server
```


Using Apollo Server 2 is as simple as defining your GraphQL schema and passing resolvers to the constructor. This structured approach lets you and your team focus on the core application and schema design without needing to worry about the mechanics of a GraphQL API.


```text
const { ApolloServer, gql } = require('apollo-server');  // Construct a schema, using GraphQL schema language
const typeDefs = gql`
type Query {
hello: String
}
`;// Provide resolver functions for your schema fields
const resolvers = {
Query: {
hello: () => 'Hello world!'
},
};const server = new  ApolloServer  ({ typeDefs, resolvers });server. listen  ().then(({ url }) => {
console.log(`🚀 Server ready at ${url}`)
});
```


To see this in action, try out[a live example here](https://glitch.com/edit/#!/apollo-server?path=index.js) .


The idea to bundle all the features together into one constructor echoes Prisma’s approach with` graphql-yoga` . We really liked how it followed our goal of making GraphQL API development more approachable, so we decided that a similar API should be the entry point for Apollo Server 2. Thank you again to the Prisma team for the inspiration and for always pushing the GraphQL community forward.


## Production best practices


In addition to revamping the getting started experience, Apollo Server now includes the most common and important patterns we’ve seen in use.


- **Automatic persisted queries** — Apollo Server supports[automatic persisted queries](https://www.apollographql.com/docs/guides/performance.html#automatic-persisted-queries) , which lets clients send a calculated hash instead of a full query string over the network, falling back to the full query in cases where the server doesn’t recognize the hash. This technique reduces query latency and bandwidth requirements — especially for clients that have large or complex queries — and doesn’t require any build-time configuration on either client or server.
- **CDN integration** — Apollo Server implements[Apollo Cache Control](https://github.com/apollographql/apollo-cache-control) , a specification for computing cache control headers from field level schema directives. Paired with a CDN, this technique enables full query caching and can dramatically reduce TTI in many settings. For more on how CDN integration works, read our[post about improving full stack performance](https://blog.apollographql.com/automatic-persisted-queries-and-cdn-caching-with-apollo-server-2-0-bf42b3a313de) .


Apollo Server behind a CDN


- **Integrated metrics reporting** — Apollo Server includes a pure JavaScript implementation of our metrics reporting agent. We’ve eliminated the need for a separate in-band binary proxy and made it simpler than ever to enable Engine, our cloud service that helps teams manage schema changes, understand what parts of their schema are used by each client, and diagnose performance problems down to the field and resolver level. The new metrics collection system also works out of the box in modern serverless and edge environments, such[AWS Lambda](https://www.apollographql.com/docs/apollo-server/v2/deployment/lambda.html) and Cloudflare Workers, that are an excellent match for GraphQL.


[Request Rate View in Apollo Engine](https://engine.apollographql.com/)


- **Standard error handling** —Building on GraphQL’s flexible approach to errors, Apollo Server 2 includes standard full-stack patterns for categorizing errors that, in conjunction with Apollo Client, makes it easy to handle re-authentication and build error-aware UI components and interactions. Read more[here](https://blog.apollographql.com/full-stack-error-handling-with-graphql-apollo-5c12da407210) .


## Data sources — Integrating existing backends


Data sources provide a structured approach for fetching the data for your schema. They facilitate the best practice of keeping resolvers simple and delegating to a data model, replacing most uses of Dataloader with a purpose built solution, including deduplication and error handling.


Today’s Apollo Server 2 release comes with a REST data source that enables an important pattern, partial query caching. A REST data source can cache long lived data in an in-memory cache, Memcached, or Redis and stitch short lived personal data for low latency full responses. We hope to continue to expand this pattern with more data sources for backends like GRPC, Thrift, and databases.


Read more about data sources in[our introductory post](https://blog.apollographql.com/easy-and-performant-graphql-over-rest-e02796993b2b) .


## Get started now


To get started with Apollo Server 2, follow[the introductory guide](https://www.apollographql.com/docs/apollo-server/v2/getting-started.html) . Upgrades from Apollo Server 1.x or` express-graphql` are straightforward and documented in our[migration guide](https://www.apollographql.com/docs/apollo-server/v2/migration-two-dot.html) . We’re here to help and excited to work with you in[the future](https://github.com/apollographql/apollo-server/blob/defer-support/docs/source/defer-support.md) . Reach out to us on the[Apollo Slack](https://www.apollographql.com/slack) or[GitHub](https://github.com/apollographql/apollo-server) . For contributors and beta users who helped make Apollo Server 2 possible, thank you!


GraphQL Summit 2018 (Nov 7–8, San Francisco)


I hope you’ll also[join us at the 3rd annual GraphQL Summit on Nov 7–8 in San Francisco](https://summit.graphql.com/) . With over 800 attendees expected, it’s the largest GraphQL developer event in the world. To share your GraphQL API experience and best practices, submit a[proposal to speak](https://www.papercall.io/graphql-summit) . Early bird tickets are selling fast, so[register today](https://www.eventbrite.com/e/graphql-summit-2018-tickets-46601841362) to reserve your spot and hear about what’s next for Apollo Server 2!


Special thank you to:[Prosper Otemuyiwa](https://github.com/unicodeveloper) ,[James Baxley](https://github.com/jbaxleyiii) ,[Matías Olivera](https://github.com/olistic) ,[Clarence Ngoh](https://github.com/clarencenpy) ,[Jake Dawkins](https://github.com/JakeDawkins) ,[Alessio Dionisi](https://github.com/adnsio) ,[Matías Olivera](https://github.com/olistic) ,[Martijn Walraven](https://github.com/martijnwalraven) ,[Yichang Liu](https://github.com/Astray-git) ,[Jesse Rosenberger](https://github.com/abernix) ,[C. T. Lin](https://github.com/chentsulin) ,[Gauthier Rodaro](https://github.com/gauthierrodaro) ,[Radu Achim](https://github.com/raduachim) ,[Danilo Bürger](https://github.com/danilobuerger) ,[Vince Picone](https://github.com/vpicone) ,[Sasha Jolich](https://github.com/sasadjolic) ,[uosl](https://github.com/uosl) ,[Hugh Willson](https://github.com/hwillson) ,[Adam Zionts](https://github.com/zionts) ,[Nicola Molinari](https://github.com/emmenko) ,[Tejas Kumar](https://github.com/TejasQ) ,[Ben Iofel](https://github.com/benwaffle) ,[Fabien BERNARD](https://github.com/fabien0102) ,[Daniel Ferreira](https://github.com/danferreira) ,[Amit Portnoy](https://github.com/amitport) ,[Daniele Zurico](https://github.com/daniele-zurico) ,[Dave Wasmer](https://github.com/davewasmer) ,[Luke Barton](https://github.com/lukebarton) ,[Yihong](https://github.com/xcv58) ,[David Glasser](https://github.com/glasser) ,[Aditya Pratap Singh](https://github.com/addityasingh) ,[Giorgio Grasso](https://github.com/gragio) ,[Walter Barbagallo](https://github.com/bwlt) ,[Andy Brenneke](https://github.com/SneakyMax) ,[Vlad Shcherbin](https://github.com/vladshcherbin) ,[Nicolás Fantone](https://github.com/nfantone) ,[Sashko Stubailo](https://github.com/stubailo) ,[Eve Porcello](https://github.com/eveporcello) ,[Alex Banks](https://github.com/MoonTahoe) ,[Justin Hall](https://github.com/wKovacs64) ,[Abhi Aiyer](https://github.com/abhiaiyer91) ,[Steve Rice](https://github.com/steverice) , and[Matt Debergalis](https://github.com/debergalis) .


Written by


Evans Hauser


[Read more by Evans Hauser](https://www.apollographql.com/blog/author/evans)
