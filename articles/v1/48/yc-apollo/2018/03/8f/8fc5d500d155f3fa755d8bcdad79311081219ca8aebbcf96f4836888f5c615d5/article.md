---
schema_version: "1.0.0"
document_id: "8fc5d500d155f3fa755d8bcdad79311081219ca8aebbcf96f4836888f5c615d5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-caching-and-performance-monitoring-in-minutes-with-apollo-engine-9b8e5da57bfb"
published_at: "2018-03-07T21:32:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:d1ff73b12a8316e25afa2323e15e704d361b404b138e6472ba8c1e5bc86a06aa"
---

# Set up GraphQL caching and performance monitoring in minutes

[Apollo Engine](https://www.apollographql.com/docs/engine/) is a GraphQL gateway that provides caching, performance monitoring, and other features specifically for GraphQL. It’s built from the ground up to empower product developers to feel confident about using GraphQL on top of existing infrastructure.


Today, we’re launching the 1.0 version of the` apollo-engine` npm package and our standalone Docker container. These new releases make it dramatically easier to add Engine to your server.


In the past, you had to add code in several places and attach middleware to your app in a specific order, but now it’s much clearer where everything needs to go for a production-ready setup.


### The new setup experience


Apollo Engine provides a ton of valuable features to help you work with GraphQL, and can handle millions or even billions of requests a day, but at the same time we want to make sure it’s a breeze to get started with.


If you want to use Engine with Apollo Server and Node.js, setup is now as simple as can be:


```text
// Install and import the apollo-engine package
const { ApolloEngine } = require('apollo-engine');
const { graphqlExpress } = require('apollo-server-express');


const app = express();


// Enable tracing and cacheControl in Apollo Server
app.use('/graphql', graphqlExpress({
tracing: true,
cacheControl: true,
schema: schema,
// ... other options
});


// Initialize Engine with an API key
const engine = new ApolloEngine({ apiKey: 'API_KEY' });


// Replace app.listen() with engine.listen()
engine.listen({
port: 3000,
expressApp: app,
});
```


You can get an API key by logging into the[Apollo Engine app](https://engine.apollographql.com/) . Once you put in the API key, run your server, and execute some queries, you’ll be able to see traces of your queries, track errors, set up caching, and more!


For all of the details, read the setup directions in the docs:


- [For Node.js](https://www.apollographql.com/docs/engine/setup-node.html)
- [For other languages](https://www.apollographql.com/docs/engine/setup-standalone.html)


## Improvements from previous versions


This new release comes with several improvements that make Engine easier to use while expanding support to more servers.


### Simpler API


We’ve updated the API and documentation to make it easier to set things up correctly. The previous API required code to be added in very specific places, and the new one is much easier to explain and use correctly.


If you are already using Engine, upgrading is easy:


- Instead of using a middleware like` engine.expressMiddleware()` , you replace your app’s` app.listen(port)` call with` engine.listen({ port, expressApp: app })` . This reduces code duplication and removes the need to worry about middleware ordering.
- Instead of the` Engine` constructor with an` engineConfig: {...}` option, you now import an` ApolloEngine` constructor which takes the Engine config directly. Any Node-specific options are passed into the` listen` function above. This helps avoid confusion about where configuration should be passed.


For all of the details,[read the upgrade guide in the docs](https://www.apollographql.com/docs/engine/1.0-migration.html) .


### Expanded Node web framework support


Because the new API relies on the standard Node` listen` interface instead of framework-specific middlewares, we’ve[expanded our support of Node server frameworks](https://www.apollographql.com/docs/engine/setup-node.html#not-express) . Engine now supports Express, Hapi, Koa, Micro, Restify, Meteor, and even the built in` http.Server` .


### Less overhead


Even thought it’s easy to install with npm, the part of Engine that you run inside your server is written in Go for better performance and stability. In the previous version, the web framework middleware you installed routed GraphQL requests to the Engine binary, and they were routed back to actually execute your GraphQL schema. With the new 1.0 API, all requests hit the Engine binary directly, and non-graphql requests are passed through. This new approach involves fewer roundtrips, making it even better than the previous approach while actually being easier to set up.


### Easier to use with non-Node servers


We’ve always distributed the Engine proxy binary both via the apollo-engine npm module (for use with Node GraphQL servers) and as a standalone Docker container (for other servers). We’ve made two improvements to how we support non-Node servers. First, we’re now using the same version numbers to tag the Docker container as we are for the npm module. So today is version 1.0.1 of both distributions, and the same[release notes](https://www.apollographql.com/docs/engine/proxy-release-notes.html) describe updates to both distributions. Secondly, because we know it’s not easy to run Docker containers in every hosting environment, we’ve added a[new ApolloEngineLauncher API to the npm module](https://www.apollographql.com/docs/engine/setup-standalone.html#apollo-engine-launcher) which you can use to run the Engine binary as a standalone process with no Node web framework integration.


## What you’ll see in the Engine UI


Once you get Engine set up, you’ll be able to look at[detailed traces of your GraphQL queries](https://www.apollographql.com/docs/engine/performance.html) :


Track your query performance distribution over time:


[Cache GraphQL results](https://www.apollographql.com/docs/engine/caching.html) to make common queries resolve in a matter of microseconds:


And more — there are a lot of other capabilities that you can learn about in the[Engine docs](https://www.apollographql.com/docs/engine/) . We’ve hand-picked this set of functionality over the last two years of talking to companies using GraphQL in production.


## Continuous improvement


In addition to overhauling the API you use to set up Apollo Engine in your app, we’ve also started a total rework of[our documentation](https://www.apollographql.com/docs/engine/) , beginning with the setup instructions. Keep an eye on our blog over the next few weeks to learn more about what Apollo Engine is, how it’s getting easier to use, and what features we’re working on next!


Written by


David Glasser


[Read more by David Glasser](https://www.apollographql.com/blog/author/glasser)
