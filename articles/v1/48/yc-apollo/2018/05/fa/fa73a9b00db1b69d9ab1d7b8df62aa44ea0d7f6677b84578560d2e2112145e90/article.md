---
schema_version: "1.0.0"
document_id: "fa73a9b00db1b69d9ab1d7b8df62aa44ea0d7f6677b84578560d2e2112145e90"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-server-2-0-30c9bbb4ab5e"
published_at: "2018-05-03T19:52:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:f38ea13353a5cffec78f4e9dff7af219f8ad65ef6a45c7a2be08c61ca1aaa02c"
---

# Try out the Apollo Server 2.0 beta!

It’s no secret that we think GraphQL is the best way to manage data in modern applications. Not only does GraphQL eliminate overfetching and unnecessary round trips to the client, it also dramatically increases developer velocity. We want to empower all developers to take advantage of these benefits, even if the thought of spinning up a GraphQL server seems intimidating at first.


Today, we are thrilled to announce the beta release of **Apollo Server 2.0.** It is designed to lower the barrier to entry for getting started with GraphQL while providing all the features teams need to run their server in production with confidence. With a schema and resolvers, you can have a production-ready GraphQL server running in less than five minutes. Let’s learn how! 🚀


## A faster path to adoption


Once teams decide to make the transition to GraphQL, they are confronted with several choices they must make before they can start writing their first schema.


- What language do I use?
- Which server framework is best?
- How do I build my schema?


While important, these decisions slow teams down on the path to adoption. Based on years of experience building scalable, performant GraphQL applications, we’ve answered the hard questions for you with Apollo Server 2.0, so you can start running queries as quickly as possible. If you desire more flexibility, we also provide integrations for all of your favorite server frameworks, including serverless variants.


To get started with GraphQL, install the Apollo Server beta:


```text
npm install apollo-server@beta graphql --save
```


Next, create an instance of` ApolloServer` by passing in` typeDefs` (your schema) and` resolvers` (the functions that implement your schema). Finally, start your server by calling` server.listen()` .


```text
const { ApolloServer, gql } = require('apollo-server');


// Construct a schema, using GraphQL schema language
const typeDefs = gql`
type Query {
hello: String
}
`;


// Provide resolver functions for your schema fields
const resolvers = {
Query: {
hello: () => "world"
}
};


const server = new ApolloServer({ typeDefs, resolvers });


server.listen().then(({ url }) => {
console.log(`🚀 Server ready at ${url}`)
});
```


Out of the box, Apollo Server 2.0 sets up an Express server, which hosts your GraphQL API and[GraphQL Playground](https://github.com/graphcool/graphql-playground) , an IDE that allows you to inspect your schema and run queries. If you’d like to play around with the new Apollo Server, check out our example project hosted on[Glitch](https://glitch.com/edit/#!/apollo-server-2-0-post) **.**


## Run in production with confidence


Although we’ve simplified the Apollo Server API, we haven’t compromised on features! Apollo Server 2.0 introduces built-in support for GraphQL subscriptions, powerful error handling tools, schema stitching, and schema directives.


We’ve also made it easier to gain visibility into what’s happening with your GraphQL API with opt-in[Apollo Engine](https://engine.apollographql.com/) integration. Engine is a GraphQL gateway that sits in between the client and your GraphQL server. It unlocks necessary features for running GraphQL in production such as performance monitoring, caching, persisted queries, and schema analytics.


The Apollo Engine tracing view helps you understand your GraphQL API


Setting up Apollo Engine has never been faster — just install` apollo-engine` , pass` engine: true` to your server configuration, and provide an` ENGINE_API_KEY` as an environment variable. If you’d like to try it out,[create an Engine service](https://engine.apollographql.com/) to receive an API key,[remix this sample project on Glitch](https://glitch.com/edit/#!/dog-graphql-api) , and add your API key to the` .env` file.


## A bright future 🚀


After setting up a server, what’s next? We want to guide you along your GraphQL journey by providing all of the advice and best practices you need to architect your schema. Look no further than the revamped Apollo Server docs, which now support[multiple versions!](https://www.apollographql.com/docs/apollo-server/)


If you’re new to GraphQL, check out our improved[getting started guide](https://www.apollographql.com/docs/apollo-server/v2/getting-started.html) . We’ve also added new essentials guides explaining important GraphQL concepts such as[building a schema](https://www.apollographql.com/docs/apollo-server/v2/essentials/schema.html) and[fetching data with resolvers](https://www.apollographql.com/docs/apollo-server/v2/essentials/data.html) . If you’re an advanced user, be on the lookout for pages on deployment, schema stitching, and schema directives coming soon. Expect the new docs to improve even more in the weeks leading up to the official release.


👀 it looks like the next version of Apollo Server will be able to actually run inside[@Cloudflare](https://twitter.com/Cloudflare?ref_src=twsrc%5Etfw) workers at the edge![#GraphQL](https://twitter.com/hashtag/GraphQL?src=hash&ref_src=twsrc%5Etfw)[pic.twitter.com/rGSDigIztm](https://t.co/rGSDigIztm)


— Apollo (@apollographql)[May 2, 2018](https://twitter.com/apollographql/status/991499774984507393?ref_src=twsrc%5Etfw)


This is just the beginning for how we develop and deploy GraphQL servers. We think there are amazing new paradigms to explore in how data can be cached, queried, and distributed all around the world with Apollo Server. We’ve even made progress on experimental features such as running GraphQL on the edge via[Cloudflare workers](https://github.com/jbaxleyiii/apollo-server-cloudflare) .


With tighter integrations between Apollo Client and Apollo Server in the works, teams will be able to take advantage of more efficient transports, integrated caching from schema to UI components, and even long talked about features like` @defer` support. The new Apollo Server 2.0 beta brings us closer to making these features a reality.


We would love for teams to try out the Apollo Server 2.0 beta and give us feedback! If you’re currently using Apollo Server 1.0, you don’t have to worry about any breaking changes when you upgrade. If you would like to switch to the new API, be sure to read our comprehensive[migration guide](https://www.apollographql.com/docs/apollo-server/v2/migration-two-dot.html) . If you’re new to Apollo Server, our[getting started guide](https://www.apollographql.com/docs/apollo-server/v2/getting-started.html) will help you go from zero to GraphQL quickly. As always, please get in touch on[Twitter](https://twitter.com/apollographql) or[Apollo Slack](https://www.apollographql.com/slack) if you’d like to help build the next generation of GraphQL servers.


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
