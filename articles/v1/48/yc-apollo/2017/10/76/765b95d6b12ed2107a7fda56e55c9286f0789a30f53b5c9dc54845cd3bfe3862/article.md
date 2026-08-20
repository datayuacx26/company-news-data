---
schema_version: "1.0.0"
document_id: "765b95d6b12ed2107a7fda56e55c9286f0789a30f53b5c9dc54845cd3bfe3862"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-tools-2-0-with-schema-stitching-8944064904a5"
published_at: "2017-10-04T23:53:19+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:f3521d41adad77bf8738d0f7a1306af13c9d35b0e270f6434711fd5ff2a7de6f"
---

# GraphQL-Tools 2.0 with Schema Stitching

**Note:** This post discusses Schema Stitching, an approach we explored that eventually led to the development of[Apollo Federation](https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/federation) . We recommend migrating to Federation for a more robust and scalable architecture. To get started, follow[this migration guide](https://www.apollographql.com/docs/graphos/schema-design/guides/migrating-from-stitching) and read[this blog post](https://www.apollographql.com/blog/announcement/expedia-improved-performance-by-moving-from-schema-stitching-to-apollo-federation/) for a real-world example of an organization benefiting from this migration.


Today, we are proud to release` graphql-tools` 2.0, the first stable release of a new feature we call “schema stitching”: the ability to create a single GraphQL schema from multiple underlying GraphQL APIs. We think this new approach to schema development will unlock a lot of new potential in GraphQL, and we’re very eager to see what people can do with it.


[Learn how to use schema stitching in the docs!](http://dev.apollodata.com/tools/graphql-tools/schema-stitching.html)


*Thanks to*[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----8944064904a5----------------------) *for co-authoring this post.*


### What is graphql-tools?


` graphql-tools` is actually the oldest library developed and maintained by the Apollo community. It started out as a toolbox of several functions related to GraphQL schema development, but today it’s the[most popular](https://npm-stat.com/charts.html?package=graphql-tools&package=graphql&from=2016-10-02&to=2017-10-02) library for creating and manipulating GraphQL schemas in JavaScript, aside from the GraphQL.js reference implementation it’s based on. It was the first library to pioneer the[GraphQL-first approach](http://dev.apollodata.com/tools/graphql-tools/index.html) of developing a GraphQL server by starting from a GraphQL schema definition string.


We are excited to see` graphql-tools` continue to drive innovative new ways to develop GraphQL schemas with the introduction of the new schema stitching feature in 2.0.


## The case for schema stitching


One of the main benefits of GraphQL is that you can query all of your data as part of one schema and get everything you need in one request. But as your schema grows, it might become cumbersome to manage it all as one codebase, and it starts to make sense to split it into parts. This separation can be both logical (split schemas into different modules in a codebase) as well as physical (schemas split between different services).


Plus, GraphQL is frequently used as an API gateway on top of REST APIs or other kinds of microservices. So it feels like you should get some big benefits if your backends are themselves implemented with GraphQL. Unfortunately, until now you didn’t — it was just as hard or harder to put a GraphQL gateway over multiple GraphQL services. Another similar idea is to combine multiple 3rd party APIs together with some local data, to create a mash-up GraphQL gateway.


## Schema stitching in the wild


One of our favorite aspects of the open source development and design process is getting feedback as soon as possible. So even during the earliest design phases of this new feature we were excited to learn as much as possible about the use cases involved in schema stitching.


Here’s what[Jason Lengstorf](https://medium.com/u/6a98eb806571?source=post_page-----8944064904a5----------------------) said about a GraphQL system he designed at IBM:


*We needed a way to let multiple teams create, test, and maintain GraphQL data sources in isolation, but ultimately expose them all as part of the same*` <em>/graphql</em>` *endpoint on our platform.*


And[Abhi Aiyer](https://medium.com/u/6d7ee7c0b225?source=post_page-----8944064904a5----------------------) from Workpop said this about their server:


*At Workpop our goal is to deconstruct our monolithic Meteor application into a set of microservices. When thinking about how our clients would interact with these services, GraphQL was a natural choice to provide a great query language for the data these services own. Thus, we set out to work on a solution that would combine our graphql services into one schema. This allows the client to query APIs across the system without worrying about the address of any given downstream service.*


Here’s how startup founder[Devan Beitel](https://medium.com/u/eedd44bc61fe?source=post_page-----8944064904a5----------------------) explained building an API that combines local data with Yelp:


*We are using the graphql-tools beta to stitch together two external services, Yelp and Graphcool.* *A query is sent to the Yelp API, and the response is a list of businesses. When that list is returned the ID is used as a query parameter sent to Graphcool. Graphcool returns any user specified data like if the business is operating and what hours it is open. Finally, the Yelp and Graphcool data is stitched together and returned to our React Native app.*


Armed with this conversations with hundreds of GraphQL developers like these, we were sure that schema stitching could be a huge boost in how GraphQL schemas are developed, and had a lot of information about the diversity of needs and use cases. For more information, check out[our previous post about schema stitching](https://dev-blog.apollodata.com/graphql-schema-stitching-8af23354ac37) .


## The` graphql-tools` schema stitching design


Initially, we viewed local code modularization and remote schema merging as separate situations, but it turns out that they are very similar. As a result, we came up with the` makeRemoteExecutableSchema` function that is very similar to the` makeExecutableSchema` function already present in` graphql-tools` . It produces a GraphQL schema instance from any remote GraphQL API using built-in GraphQL introspection. This way, we can write a schema stitching function that just operates on a local schema instance and avoid treating the local and remote cases separately.


The actual` mergeSchemas` function that implements the stitching, in its basic use case, just accepts list of schemas and produces a new schema that merges together all of the types and the Query and Mutation fields from each. That schema can then be queried as if it were just one schema, and handles delegating different parts of the query to the correct backend.


### Links and conflicts


Two additional features are avaliable with` mergeSchemas` : schema extensions and conflict resolution. First, it’s possible to link between any two types in your new schema (even across backends) by extending the schema and passing the custom` resolvers` option:


Second, you can manage conflicts between types with the` onTypeConflict` callback.` onTypeConflict` is very useful when using common types across multiple APIs. Normally, if a type with the same name appears in two schemas, they will cause type conflicts. With` onTypeConflict` , one can define custom logic to resolve that overlap, for example by using the first type of those two or merging them in some way.


## Examples


If you want to jump right in and see this new feature in action, we’ve got two complete examples on[Launchpad](http://launchpad.graphql.com/) . The first one uses two very simple schemas, one to describe a simple user system:


```text
type User {
id: ID!
email: String
}type Query {
userById(id: ID!): User
}
```


Then, there’s a simple schema for “Chirps”: small text snippets that can be posted by an author:


```text
type Chirp {
id: ID!
text: String
authorId: ID!
}type Query {
chirpById(id: ID!): Chirp
chirpsByAuthorId(authorId: ID!): [Chirp]
}
```


With just a few lines of code, we can merge them together:


```text
export const schema = mergeSchemas({
schemas: [chirpSchema, authorSchema],
});
```


For the complete code, as well as an example of how to add resolvers between these types, see the example on Launchpad:[Apollo LaunchpadView this code sample on Launchpad, the GraphQL server playground.launchpad.graphql.com](https://launchpad.graphql.com/8r11mk9jq)


We also have a more complex example with a real estate theme, which combines a[property schema](https://launchpad.graphql.com/v7l45qkw3) with a[booking schema](https://launchpad.graphql.com/41p4j4309) into a larger[merged schema](https://launchpad.graphql.com/q5kq9z15p) .


## Future work


This is the first stable release of this feature, so we focused on creating an API that is generic but also simple to use. However, we are sure that there are more possible extension points to merging schemas, as well as opportunities to make the API more declarative for common cases.


For now we’re excited to see more use cases and approaches from the community, and we can improve the API in the future to work better with those cases.


### Join the discussion


If you’d like to share your schema stitching use case or give us feedback on this feature, let us know in the the **#schema-stitching** channel on[Apollo Slack](http://apollodata.com/#slack) .


The team and contributors who have been working on schema stitching in graphql-tools will be attending and speaking at GraphQL Summit 2017, October 25–26 in San Francisco, along with GraphQL engineers from GitHub, Facebook, Twitter, Twitch, Yelp, and more. Use the discount code` APOLLOBFF` for 20% off your ticket if you’d like to join.


Written by


Mikhail Novikov


[Read more by Mikhail Novikov](https://www.apollographql.com/blog/author/freiksenet)
