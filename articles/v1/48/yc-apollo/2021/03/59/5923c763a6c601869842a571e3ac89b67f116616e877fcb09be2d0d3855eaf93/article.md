---
schema_version: "1.0.0"
document_id: "5923c763a6c601869842a571e3ac89b67f116616e877fcb09be2d0d3855eaf93"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/using-express-with-graphql-server-node-js"
published_at: "2021-03-02T11:39:13+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:f6fcf9e5bab93a9dc1cc42e446dc8b78b8090fb2c98d052aea9301b40f4c0979"
---

# Using Express with GraphQL – How to create a GraphQL server with Node.js/Express

One of the fastest ways to get up and running with GraphQL is to install Apollo Server as *middleware* on your new or existing HTTP server.


In this short post, we demonstrate how to use Apollo Server to create a GraphQL server with Express.js using the[apollo-server-express](https://www.npmjs.com/package/apollo-server-express) package. At the end, we’ll discuss the tradeoffs of this approach.


## Using Apollo Server with Express


Let’s start with the dependencies.


### Install dependencies


You’ll need the` apollo-server-express` and` graphql` packages.


```text
npm install apollo-server-express graphql --save
```


### Example


The way we set up a GraphQL server using` apollo-server-express` is very similar to the barebones approach of using the` apollo-server` package.


The only difference is that we apply the Apollo Server instance as middleware to an Express HTTP instance with` server.applyMiddleware({ app })` .


```text
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');


// Construct a schema, using GraphQL schema language
const typeDefs = gql`
type Query {
hello: String
}
`;


// Provide resolver functions for your schema fields
const resolvers = {
Query: {
hello: () => 'Hello world!',
},
};


const server = new ApolloServer({ typeDefs, resolvers });


const app = express();
server.applyMiddleware({ app });


app.listen({ port: 4000 }, () =>
console.log(`🚀 Server ready at http://localhost:4000${server.graphqlPath}`)
);
﻿
```


Your GraphQL API should be running at` http://localhost:4000/graphql` .


## Tradeoffs of using Apollo Server as GraphQL middleware


### Advantages


One neat thing about using` apollo-server-express` instead of` apollo-server` is that we can serve both REST and GraphQL at the same time using Express.


Serving a GraphQL server *within* Express also maintains our ability to use Node.js middleware for common problems like rate-limiting, security, and authentication.


### Downsides


Using` apollo-server-express` involves a little bit more boilerplate than merely using` apollo-server` . However, since` apollo-server` is just a wrapper around` apollo-server-express` , there shouldn’t be any performance concerns.


## Conclusion


We learned how to set up a GraphQL server with` apollo-server-express` and discussed some of the tradeoffs. If you’re using a different middleware library like Restify, Hapi, Koa, or Lambdas,[check out our docs](https://www.apollographql.com/docs/apollo-server/v1/servers/express/) for details on the other GraphQL API integrations.


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
