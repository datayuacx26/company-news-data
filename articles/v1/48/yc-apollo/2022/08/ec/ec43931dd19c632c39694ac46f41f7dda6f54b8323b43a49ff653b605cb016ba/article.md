---
schema_version: "1.0.0"
document_id: "ec43931dd19c632c39694ac46f41f7dda6f54b8323b43a49ff653b605cb016ba"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/schema-first-vs-code-only-graphql"
published_at: "2022-08-29T09:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:5ef01fa5677d0ebed1f596f9cc931506d039fb4668b863a2058cb635bacd6ca1"
---

# Schema-First vs Code-Only GraphQL

When creating a GraphQL server there are two artifacts required by the GraphQL engine: a schema, which defines all the types and fields, and the resolvers, which are the collection of functions that are called to return the data for those types. Since GraphQL can be implemented in many different programming languages, there are of course many tools and libraries you can use to create a server and execute requests. Whenever there are multiple options there will also be pros and cons for each choice and differing opinions on which are best.


## Schema-First


Schema-first, or SDL-first, is one of the more common approaches to creating a GraphQL server. It requires that you define the schema in the Schema Definition Language (SDL) and also write the resolvers that execute and return data at runtime. This pattern is popular in languages that do not have a type system, like JavaScript, because the GraphQL engine is providing this API type-safety for us. It is also the pattern used by many popular GraphQL servers, like[apollo-server](https://github.com/apollographql/apollo-server) , which adds to the exposure of the pattern, as new GraphQL developers will likely see this pattern first when learning GraphQL for the first time.


```text
# schema.graphql using schema-first
type Query {
hello: String
}
```


```text
// resolvers.js using schema-first
const resolvers = {
Query: {
hello: () => 'hello world'
}
};
```


## Code-Only


Code-only, or code-first, is an approach where you only need to write the resolvers for the GraphQL server and a build tool can compile the schema and SDL based on the types or annotations included in the code. This is more common in type-safe languages, such as[graphql-kotlin](https://github.com/ExpediaGroup/graphql-kotlin) , where you can run reflections against source code, however, it can be used by any GraphQL server language.


```text
// schema.kt using code-only
class Query {
fun hello(): String = "hello world"
}
```


## Generated Schemas


The term[generated schema](https://www.apollographql.com/blog/tooling/apollo-codegen/generated-graphql-schemas-and-schema-design/) is separate from schema-first and code-only. This refers to tools that can take some external data types or non-GraphQL schema and generate a GraphQL schema and/or the resolvers with the same types. These tools allow you to take existing data sources, like OpenAPI or SQL databases, and quickly add a GraphQL API on top of them with the same types. The result is an API server that you deploy and manage, but you don’t control the types or data that the API clients can ask for. Instead of comparing all GraphQL API patterns, we will only focus on the first two, which allow you to define types separate from your underlying data sources.


## Pattern Misconceptions


There are a few misconceptions that we often hear from the GraphQL community that we would like to address.


#### *“Rumor* : *Schema-first leads to better schema design”*


Schema-first is sometimes framed as a “better” pattern because it requires you to write out the schema types separate from the runtime code. The reasoning is that developers will have to think about the types they are returning because the types are defined in a separate location. Hopefully, this also makes your code more readable as code reviewers can easily go to one location to see only the API changes and not internal implementation details in the resolvers. While you do have to write the schema manually, schema-first does not guarantee your API schema will be better.


Often when first learning GraphQL, developers will try to map existing API patterns from REST or only think about the underlying data sources, rather than the users of their API. What defines “good” schema design is going to come from the clients of the API, not the server maintainers. If a client can get the data it needs with minimal effort and without the need to transform the data themselves, it will be an easy API to consume. There are many patterns for[good schema design](https://www.apollographql.com/blog/backend/federation/federated-schema-design) ; regardless of how you build the schema, try thinking about your end consumers. When building with code-only you still need to have the types defined, and they should be planned and thought of ahead of time the same way you do with schema-first.


#### *“ *Rumor** : *You can’t see the SDL with code-only”*


In all of the above patterns, the runtime artifacts of the GraphQL server are still the same. We need a schema, which defines all the types according to the[GraphQL spec](https://spec.graphql.org/) , and the resolvers to execute code. If developers have concerns about where the schema types can be found, using tooling like a build plugin, you can generate an SDL file at build time, like how[graphql-kotlin includes a Gradle plugin](https://opensource.expediagroup.com/graphql-kotlin/docs/plugins/gradle-plugin-tasks#graphqlgeneratesdl) . Another option is to generate the SDL file from introspection, by using a tool like[Rover CLI](https://www.apollographql.com/docs/rover/commands/graphs#graph-introspect) . Once you have the SDL file, you can check in the changes to source control or upload it to a schema registry, like[Apollo Studio](https://www.apollographql.com/studio) , to easily share schema changes and explore the types directly, or even make real queries against your data.


SDL files abstract our types from our code and allow all developers, both client-side and server-side, to design, discuss, and review API changes in a common language. Even with code-first as your implementation tool, SDL files can be used for everything else developers do that is not writing code, whether that is architecting APIs in design docs or offering review comments in line with code changes. Instead of focusing on tooling, consider adopting a[schema stewardship model](https://www.apollographql.com/blog/community/graphql-champions/10-best-practices-for-schema-stewardship/) that encourages collaboration and iteration that will allow your team to maintain your API design principles, regardless of the implementation details of the server.


#### *“ *Rumor** : *Code-only is easier because there is less code to maintain”*


In the schema-first approach you can have errors because the names did not match up exactly between the SDL and the resolvers. Most of the time this can be caught with builds or tests, but this still can lead to runtime errors. Code-only solves this problem by having you write the types essentially in line with the resolvers which in theory is less code to maintain.


For small examples, this pattern looks easier if you have a good reflection and type system, but not all code-only libraries are built the same. The reference GraphQL implementation,[graphql-js](https://github.com/graphql/graphql-js) , is actually code-only, and only through external libraries, initially created by Apollo, did it become easy to create a server through the schema-first approach. The original builder pattern required much more code to include types in a non-typed language.


```text
// server.js using code-only graphql-js
const schema = new GraphQLSchema({
query: new GraphQLObjectType({
name: 'Query',
fields: {
hello: {
type: GraphQLString,
resolve: () => ‘hello world’
}
}
})
});
```


If you are choosing schema-first, you also still have the option to generate code in the reverse. Because GraphQL schema is type-safe, you can generate code from GraphQL that can be used in the server and resolvers. Tools like[graphql-code-gen](https://www.graphql-code-generator.com/plugins/typescript/typescript-resolvers) allow you to generate the Typescript types so your resolvers have the guaranteed type safety, and[gqlgen](https://gqlgen.com/) takes it even further, allowing you to specify directives in the schema which will generate some of the resolver code.


#### *“ *Rumor** : *Schema-first makes it difficult to have a modular schema”*


Some schema-first libraries only support passing in a single schema definition. If you want to split up your GraphQL server into different modules, each of which contains its types and resolvers, this can be more difficult to accomplish with just one library. Code-only, by its nature, solves this issue by having the types inlined so sharing resolvers also share the types and they can be merged back together. While code-only might have been the best way to have a modular schema a few years ago, the GraphQL ecosystem has evolved as more developers use GraphQL and build more tooling, and schema-first libraries have been updated to support multiple type definitions.


For those looking to just split up a single server into smaller modules, using the built-in extend keyword from GraphQL spec can usually solve most problems.


```text
// reviews.js
const reviewsSDL = gql`
extend type Query {
allReviews: [Review]
}


extend type Product {
reviews: [Review]
}


# ...other types
`;


const reviewsResolvers = {
Query: {
allReviews: () => callReviewsDB()
},
Product: {
reviews: (parent) => reviewsForProduct(parent.id)
}
};


// products.js
const productsSDL = gql`
type Query {
allProducts: [Product]
}


type Product {
id: ID!
name: String!
}
`;


const productsResolvers = {
Query: {
allProducts: () => callProductsDB()
}
};


// “merging” types and resolvers
const server = new ApolloServer({
typeDefs: [reviewsSDL, productsSDL],
resolvers: [reviewsResolvers, productsResolvers]
});
```


## Conclusion


GraphQL is extremely flexible and a strong type system allows for lots of amazing tooling to be built. When you are choosing any library or coding pattern there will be trade-offs and decisions to be made, so taking the time to compare your tool set and your team’s needs will help you make an informed decision on what one to use. If you have any other misconceptions that you hear often about schema-first vs code-only share this article and tweet at us[@apollographql](https://twitter.com/apollographql) with how you explain it in your own words!


Written by


Shane Myrick


[Read more by Shane Myrick](https://www.apollographql.com/blog/author/shane)
