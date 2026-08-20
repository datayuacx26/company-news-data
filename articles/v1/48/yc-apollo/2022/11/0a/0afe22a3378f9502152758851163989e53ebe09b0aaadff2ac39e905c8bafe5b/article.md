---
schema_version: "1.0.0"
document_id: "0afe22a3378f9502152758851163989e53ebe09b0aaadff2ac39e905c8bafe5b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-server-4-a-lightweight-and-easier-to-use-apollo-server"
published_at: "2022-11-09T12:23:17+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:677af51a07a8525074e707a26136dd69042ffe2fdc76afd3435cc33bc4e01554"
---

# Apollo Server 4: a lightweight and easier-to-use Apollo Server

Apollo Server 4 is a lightweight, open-source, spec-compliant GraphQL server that’s compatible with any GraphQL client. It’s a great choice for building a production-ready, self-documenting GraphQL API that can use data from any source. You can use Apollo Server 4 as a subgraph in a federated Supergraph or as a stand-alone GraphQL server.


Apollo Server 4 introduces the` @apollo/server` package which decouples plugins and frameworks from the core of Apollo Server. It offers a stable API for HTTP and serverless frameworks, enabling users to build Apollo Server integrations for their framework of choice. Apollo Server 4 also introduces a TypeScript-first development and documentation experience to help you build typesafe servers using a schema-first development process.


Apollo Server 4 is generally available! You can[get started from scratch](https://www.apollographql.com/docs/apollo-server/v4/getting-started) or[migrate from an older version](https://www.apollographql.com/docs/apollo-server/v4/migration) today!


## A dedicated API for HTTP and Serverless framework integrations


A strength and a challenge of the JavaScript/TypeScript ecosystem is how many different options there are available to solve any problem. For example, there are dozens of popular web frameworks to choose between when setting up an HTTP server. There are also many serverless environments like AWS Lambda and Google Cloud Functions that each have their own APIs for handling HTTP requests. We want Apollo Server to work with as many of them as possible.


In previous versions of Apollo Server, there wasn’t a well-defined API layer between the “core” of Apollo Server and the individual framework integrations. Many features of Apollo Server were implemented as logic which had to be repeated in each integration, but often in slightly different, framework-specific ways. Improving any of these features often meant changing framework-specific code in 9 different packages.


For Apollo Server 4, we defined simple portable data structures for HTTP requests and responses, and we rewrote Apollo Server to act on these representations. Apollo Server now has a stable API for building framework integrations, which now usually consist of a dozen or so lines of code that convert between the request and response objects of the framework and Apollo Server. AS4 also has native support for serverless framework lifecycles, and serverless integrations now use a dedicated API during the server startup sequence to execute operations.


### Using and contributing to integrations


The Apollo Server 4 core project ships with a core-maintained integration for the most popular Node web framework (Express) as well as a simple standalone web server for when you don’t need to configure much.


We’ve[set up infrastructure](https://github.com/apollo-server-integrations) to help our community build and maintain integrations, such as with[AWS Lambda](https://github.com/apollo-server-integrations/apollo-server-integration-aws-lambda) ,[Fastify](https://github.com/apollo-server-integrations/apollo-server-integration-fastify) , and[Koa](https://github.com/apollo-server-integrations/apollo-server-integration-koa) . Community members have also already built integrations with new environments such as[H3](https://github.com/apollo-server-integrations/apollo-server-integration-h3) and[Next.JS](https://github.com/apuyou/apollo-server-nextjs) that were never before supported by Apollo Server. Because building integrations require less domain-specific implementation, framework experts and integration authors can build integrations for their framework of choice and add those integrations to our[Apollo Server integrations GitHub organization](https://github.com/apollo-server-integrations) .


Additionally, because integrations are now maintained independently from Apollo Server, it will be much easier to update your framework to take advantage of new features in your web framework of choice. We are always looking to add new framework integrations, so if you don’t see[an integration](https://github.com/apollo-server-integrations) you’d like to use, create an issue on[GitHub](https://github.com/apollographql/apollo-server/issues) . If you specialize in any of[these frameworks](https://github.com/apollographql/apollo-server/labels/integration-collaborators) and are interested in building or maintaining an AS4 integration, please reach out!


## Simpler package structure and reduced bundle size


Apollo Server 3 was structured as a large number of distinct packages for each framework integration. For example, Express users used the` apollo-server-express` package while Fastify users used the` apollo-server-fastify` package. Other functionality involved depending on one of a wide array of smaller packages, and it was challenging to ensure you used a consistent version across these packages. Apollo Server 4 simplifies this situation by combining all of the core code into a single new` @apollo/server` package.


Unlike Apollo Server 3, the Apollo Server 4 package is also published in both CJS and ESM formats! The ECMAScript Module format improves upon the CommonJS (CJS) format pioneered by Node. Modern build tools can better optimize packages built as ESM, resulting in reduced bundle size. Smaller, optimized bundles allow serverless runtimes like AWS Lambda to start up faster.


Because AS4 is published in both formats, you get the improved efficiency of ESM if you use a newer build tool without losing compatibility with older build environments that only support CJS.To learn more about the difference between ESM and CJS and their interoperability, check out the[Node.js documentation](https://nodejs.org/api/esm.html#differences-between-es-modules-and-commonjs) .


## Avoid resolver type and schema mismatches


With the release of Apollo Server 4, we’ve updated all of[our documentation](https://www.apollographql.com/docs/apollo-server/) to show a TypeScript-first approach and recommend Typescript as the default way to build with Apollo Server.


When using Apollo Server 3, there was no way to specify the type of your context value when setting up your server. This led to typing inconsistencies because there was no compile-time check ensuring the type returned by your context function matched the value in your plugins and resolvers.


Apollo Server 4 enables you to write code that can statically check that your resolver types match their schema types, using a schema-first approach. To do this, Apollo Server documents the use of the` graphql-code-generator` package with typescript-resolvers to ensure server implementations are type-safe. To get started or learn more about generating types from your schema, check out[our documentation](https://www.apollographql.com/docs/apollo-server/workflow/generate-types) .


## **Getting started with Apollo Server 4**


We’re excited for you to start using Apollo Server 4 and all of its new features!


If you’re starting from scratch, our[getting started docs](https://www.apollographql.com/docs/apollo-server/v4/getting-started) will walk you through setting up Apollo Server for the first time, or if you’re already using Apollo Server 3, our[migration guide](https://www.apollographql.com/docs/apollo-server/v4/migration) will help you upgrade. We would love your feedback on Apollo Server 4 and its framework integrations! You can do so by reaching out on[our community forum](https://community.apollographql.com/) or[GitHub](https://github.com/apollographql/apollo-server) .


Written by


Mary Cutrali


[Read more by Mary Cutrali](https://www.apollographql.com/blog/author/mary-cutrali)
