---
schema_version: "1.0.0"
document_id: "01737d68bde9a2d413ce8160ec838dc980b1c5093af8eea2f7c9fc5f1ba1515e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/sdl-first-products-parallelizing-product-development-with-graphql-16f016e66af6"
published_at: "2018-03-05T21:36:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:d234dabd50de3a62ae409b327adcd999edb324e3e6fa56d26e4819ffce416002"
---

# SDL First Products: Parallelizing Product Development with GraphQL

Developing new products is difficult, especially by the dependencies involved when talking about frontend UI and server-side APIs. To be truly useful, a UI must display and manipulate real data (likely through APIs). This means that many times, UI is blocked on building a backend. Similarly, building REST APIs through a tool that generates endpoints for you can result in locking API endpoints to a specific database schema (thus breaking proper separation of concerns between the frontend and database schema).


Recently, I was able to talk about how the GraphQL SDL addresses these concerns at the[Apollo GraphQL meetup](https://www.youtube.com/watch?v=iW4il6wUlvs&list=PLpi1lPB6opQxp6zPDuwhHd8MLm_BhBgN9) in San Francisco.


In addition, I will be expanding on these ideas in my qcon talk “[Parallelizing Product Development with GraphQL](https://qconlondon.com/london2018/speakers/chris-biscardi) ”.


## The SDL


The[Schema Definition Language](https://blog.graph.cool/graphql-sdl-schema-definition-language-6755bcb9ce51) is a core piece of building GraphQL APIs. It is, I would argue, the primary place to define and structure the relationship between the backend and the frontend. Backends can use a schema definition file to abstract the makeup of backend services and frontends can use the SDL and introspection result to validate and pre-ordain queries that will be sent to the server before any resolvers are implemented.


## Working with the SDL


First, let’s talk a bit about what the SDL actually is and how to write it. The SDL is a typed language for specifying various objects that can be accessed through a GraphQL API.


Example of the Schema Definition Language


This example has two main types,` Post` and` Blog` . These types are made up of a number of fields that are either required (noted by an exclamation point) or optional. The structure is very similar to a JSON object with normal keys and weird looking values. In this case the values are the types of each field.` Post`` id` s are required` Strings` ,` likes` are required` Ints` , and the` blog` field refers to` Blog` type. You can define your own primitives similar to the way Ints and Strings are defined.


One use case for custom primitives (or Scalars in GraphQL parlance) would be for dealing with dates or with extremely large integers. When defining a new Scalar, you tell the GraphQL implementation how to parse and serialize the new Scalar. This means that you could define a` BigInt` which is treated as a string in the client side JavaScript, thus not losing any accuracy.


## Prototype the SDL for a New Product


So when building a new product, how do we best experiment with the SDL to create new GraphQL APIs? One example of an SDL first tool is[Prisma](https://github.com/graphcool/prisma) . Prisma turns a database into a GraphQL API by letting you design your schema using the GraphQL SDL. This means that there are two APIs, one for the database and one layered on top. Both APIs are able to generate a number of useful methods from the well typed SDL, such as querying for collections.


If we take the previous` Blog` schema example earlier in this post, we can easily generate a GraphQL API from it. First, we’ll run` prisma init` to bootstrap a new API using a full stack boilerplate.


```text
➜ prisma init full-03
? How to set up a new Prisma service?Minimal setup: database-only
❯ GraphQL server/fullstack boilerplate (recommended)
```


We will choose the basic node boilerplate for now, but could also start with a more interesting API that includes authentication.


```text
➜ prisma init full-03
? How to set up a new Prisma service?Running $ graphql create …
? Choose GraphQL boilerplate project:
❯ node-basic Basic GraphQL server (incl. database)
node-advanced GraphQL server (incl. database & authentication)
typescript-basic Basic GraphQL server (incl. database)
typescript-advanced GraphQL server (incl. database & authentication)
react-fullstack-basic React app + GraphQL server (incl. database )
```


Then we immediately have to choose where we want to deploy it. In our case, we will use a local Docker cluster (although we could use one of graph.cool’s clusters or deploy a kubernetes pod to handle it as well).


```text
[graphql create] Running boilerplate install script…
Running $ prisma deploy…? Please choose the cluster you want to deploy “full-03@dev” to  prisma-eu1 Public development cluster
prisma-us1 Public development cluster
❯ local Local cluster (requires Docker)
```


You may notice that Prisma is a collection of other tools in the ecosystem, so if you like a given piece of Prisma you can pull it out and use it without adopting wholesale.


Now that we’ve deployed it, we can see the migrations that happen as a result of our Post type. Notice the seed.graphql import.


```text
Please choose the cluster you want to deploy “full-03@dev” to
Added cluster: local to prisma.yml
Creating stage dev for service full-03 ✔
Deploying service `full-03` to stage `dev` on cluster `local` 201ms
Changes:Post (Type)
+ Created type `Post`
+ Created field `id` of type `GraphQLID!`
+ Created field `isPublished` of type `Boolean!`
+ Created field `title` of type `String!`
+ Created field `text` of type `String!`
+ Created field `updatedAt` of type `DateTime!`
+ Created field `createdAt` of type `DateTime!`Applying changes 1.1sHooks:
Importing seed dataset from `seed.graphql` 498msYour GraphQL database endpoint is live:HTTP:  http://localhost:4466/full-03/dev
WS: ws://localhost:4466/full-03/devChecking, if schema file changed 180ms
Writing database schema to `src/generated/prisma.graphql` 0ms
```


We can use the` seed.graphql` file to populate data in development (for example). The seed file operates on the API that results from our Post SDL so will use migrations to insert data.


```text
mutation {
first: createPost(data: {
title: "Hello World"
text: "This is my first blog post ever!"
isPublished: true
}) {
id
}second: createPost(data: {
title: "My Second Post"
text: "My first post was good, but this one is better!"
isPublished: true
}) {
id
}third: createPost(data: {
title: "Solving World Hunger"
text: "This is a draft..."
isPublished: false
}) {
id
}
}
```


Taking this SDL first approach also allows you to experiment with advanced features like[subscriptions](https://github.com/graphcool/prisma/tree/c9869a4a2135e3b31ca64702e899ac00f3caf71b/examples/subscriptions) , which can be a huge pain to implement and manage on the client side and server side without GraphQL.


## Using the SDL for Existing Products


Often times, our frontend depends on existing backends and we are unable to start a greenfield project. These existing backends, generally presented through REST API, were built before the SDL and force the unnecessary coupling of frontend and backend. To modernize these backends with the SDL you must place GraphQL over REST with tools such as[graphql-tools](https://dev-blog.apollodata.com/graphql-tools-a-simpler-way-to-create-graphql-apis-eadf018f3766) . graphql-tools allows you to think in an SDL first world by providing the ability use SDL directly,[extend](https://www.apollographql.com/docs/graphql-tools/schema-directives.html) the base SDL, and combine distinct SDL files through a technique called[schema stitching](https://dev-blog.apollodata.com/graphql-tools-2-0-with-schema-stitching-8944064904a5) .


## Extras


To finish off, I’d like to whet your appetite a little more by pointing out that you can use an SDL file to generate fake data with[graphql-faker](https://github.com/APIs-guru/graphql-faker) through an @fake directive. The introspection data can be used for exploration with[graphql-playground](https://www.graphqlbin.com/RVIn) . Finally, a client application can extend this approach with[apollo-link-state](https://www.apollographql.com/docs/link/links/state.html) to manage local data with Apollo Client, unifying the querying abstractions. As of today, link-state is works with your existing application’s GraphQL schema, rather than allowing a client-only SDL to define its behavior. I’d love to see it support a client-side SDL-based extension mechanism in the future.


## Fin


Hopefully I’ve piqued your interest around the GraphQL SDL and how it can be used to delegate resolver implementation and client side state management instead of requiring a hard dependency chain when building a new product.


---


If you’d like to write a guest post on our blog, email Sashko at sashko@apollographql.com!


Written by


Christopher Biscardi


[Read more by Christopher Biscardi](https://www.apollographql.com/blog/author/chrisbiscardi)
