---
schema_version: "1.0.0"
document_id: "f428cf6ff592212ede94d8c2c2116fb8b4d5cb4d7bd91d13d5c9ae1c57a29aed"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/three-ways-to-represent-your-graphql-schema"
published_at: "2018-05-16T19:19:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:0c9d2f682d53ec890e4044c5c1964401210ae61acf9b454d266b7ff27d036baf"
---

# Three ways to represent your GraphQL schema

Three formats representing the same data schema in slightly different ways.


One of the best parts of GraphQL is that it provides a clean definition of the data available in your API through a schema. The schema describes the types and fields you can query, and also includes any relationships between those types.


The schema is what enables GraphQL to have such a variety of useful developer tools including auto-completing query explorers like GraphiQL and GraphQL playground, editor and IDE integrations, query validation tools, code generation, automatic mocking and more. But in order to use these tools, you first need to acquire the schema from your API and convert it to the appropriate format for the tool.


Today, we’re going to learn about the three most common formats for representing a GraphQL schema, when they are used, and how to convert between them. These formats are:


1. The GraphQL Schema Definition Language, or SDL
2. The GraphQL introspection query result
3. The GraphQL.js GraphQLSchema object


First, let’s dive in to what each format looks like. To do that, we’re going to write out the same schema — a basic API with authors and posts — using each method.


### GraphQL Schema Definition Language


This is what most people use when they are writing down a GraphQL schema in a language-agnostic way. It’s also the notation used in the[GraphQL specification](http://facebook.github.io/graphql/) , and the notation itself was recently added to the spec.


Here’s what a basic schema looks like using the SDL:


```text
type Author {
id: Int!
firstName: String
lastName: String
posts: [Post]
}type Post {
id: Int!
title: String
author: Author
votes: Int
}type Query {
posts: [Post]
author(id: Int!): Author
}
```


It’s a convenient notation that is easy to write by hand and easy for humans to read, which is why it’s used so often. It’s usually just passed around as a string in a variable.


### GraphQL introspection query result


GraphQL APIs are required to be self-documenting. According to the specification, every GraphQL API needs to respond to queries about its own structure. This system is called[introspection](http://facebook.github.io/graphql/October2016/#sec-Introspection) , and an introspection query starts with the` __schema` field present on every GraphQL API that hasn’t intentionally disabled it.


For example, a simple query to get a list of types might look like this:


```text
# An example query that uses introspection,
# but not _the_ introspection query
{
__schema {
types {
kind
name
possibleTypes {
name
}
}
}
}
```


That’s *an* introspection query, but most often, people use a very specific query that is often referred to as *the* introspection query. That’s a particular query exported by the reference GraphQL.js implementation you can install from npm. You can either find it in the[graphql-js](https://github.com/graphql/graphql-js/blob/0276d685b51262686c841763ba0b6e71103f64f3/src/utilities/introspectionQuery.js) GitHub repository, or import it from the npm package directly:


```text
const { introspectionQuery } = require("graphql");
```


This is a pretty large query that extracts basically all of the information a GraphQL API provides via introspection. When you save the result of that query, you essentially have a JSON representation of the shape of your API, that is interchangeable with the Schema Definition Language above. Here’s a script that will fetch the introspection result from an API, and save it to a file:


```text
const fetch = require("node-fetch");
const { introspectionQuery } = require("graphql");
const fs = require("fs");


fetch("https://1jzxrj179.lp.gql.zone/graphql", {
method: "POST",
headers: { "Content-Type": "application/json" },
body: JSON.stringify({ query: introspectionQuery })
})
.then(res => res.json())
.then(res =>
fs.writeFileSync("result.json", JSON.stringify(res.data, null, 2))
);
```


The JSON result is very long — over 1000 lines — which is why it’s not a great representation when you need something human-readable. You can check it out[in this Gist](https://gist.github.com/stubailo/c542779b872ae56fe69b332c68732e69) . But it’s the most reliable way to query the schema of a GraphQL API with a program.


Later in the article, we’ll look at how easy it is to convert between these representations, so that you can introspect the schema programmatically then convert it to a different representation if needed.


### GraphQL.js GraphQLSchema object


Last but not least, we have the objects that the JavaScript reference implementation of GraphQL uses to store information about the schema. It’s a great intermediate representation, so it’s often the medium used to communicate between different JavaScript tools.


Here’s what part of the above schema above looks like when implemented using these objects directly:


```text
const {
GraphQLObjectType,
GraphQLSchema,
GraphQLNonNull,
GraphQLInt
} = require("graphql");


const queryType = new GraphQLObjectType({
name: "Query",
fields: {
posts: {
type: postType
},
author: {
name: "author",
type: authorType,
arguments: { id: { type: new GraphQLNonNull(GraphQLInt) } }
}
}
});


// ... postType and authorType defined similarly


const schema = new GraphQLSchema({
query: queryType
});
```


This representation of the schema makes it the easiest to manipulate the objects directly, and write functions that generate types for you, but it’s not as convenient as the SDL for writing schemas by hand.


Some JavaScript tools, such as the graphql-tools[addMockFunctionsToSchema](https://www.apollographql.com/docs/graphql-tools/mocking.html) and the[Schema Link](https://www.apollographql.com/docs/link/links/schema.html) for Apollo Client, accept this representation directly, so if you’re working with an SDL string or introspection result you’ll need to convert it to this format.


---


## Converting between all of the different types


Now comes the fun part! It turns out that all of these different ways to represent a GraphQL schema are equivalent, and we can actually convert between them directly. So if you need to pass a` GraphQLSchema` object to some utility, or you need to print out an SDL, but all you have is an introspection result, you’ve come to the right place.


### SDL to GraphQLSchema object


The method to use here depends on whether or not you need to add resolvers to your schema. If you’re just trying to get a representation of the types and fields in your schema definition, use` buildSchema` from GraphQL.js:


```text
const { buildSchema } = require('graphql');


const sdlSchema = `
type Author {
firstName: String
lastName: String
}
type Query {
author(id: Int!): Author
}
`;


const graphqlSchemaObj = buildSchema(sdlSchema);
```


If you want to also add resolvers, use` makeExecutableSchema` from[graphql-tools](https://www.apollographql.com/docs/graphql-tools/) :


```text
const { makeExecutableSchema } = require("graphql-tools");


const sdlSchema = `
type Author {
firstName: String
lastName: String
}
type Query {
author(id: Int!): Author
}
`;


const graphqlSchemaObj = makeExecutableSchema({
typeDefs: sdlSchema,
resolvers: {
Query: {
author: () => ({ firstName: "Ada", lastName: "Lovelace" })
}
}
});
```


You can use this method to implement your whole GraphQL API using the schema definition language. Just check out[the graphql-tools docs](https://www.apollographql.com/docs/graphql-tools/generate-schema.html) .


### GraphQLSchema object to SDL


It’s also easy to get a schema language string when you have a JavaScript GraphQL schema object. Use the accurately named` printSchema` function:


```text
const { printSchema } = require("graphql");


console.log(printSchema(graphqlSchemaObj));
```


### Introspection query result to GraphQLSchema object


If you are working with a remote server, you’ll probably need to use introspection to figure out its schema, as demonstrated in the code snippet with` fetch` above. You can convert the result of that into a GraphQLSchema object using the` buildClientSchema` function from GraphQL.js:


```text
const { buildClientSchema } = require("graphql");
const fs = require("fs");


const introspectionSchemaResult = JSON.parse(fs.readFileSync("result.json"));
const graphqlSchemaObj = buildClientSchema(introspectionSchemaResult);
```


### GraphQLSchema object to introspection query result


In this case, you can execute the introspection query against the schema object you have directly, instead of against a remote server:


```text
const { graphqlSync, introspectionQuery } = require("graphql");


const result = graphqlSync(graphqlSchemaObj, introspectionQuery).data;
```


` graphqlSync` is a relatively recent addition to GraphQL.js that lets you execute a query that you know is going to return synchronously and get the result right away, rather than getting a promise. Since we know that introspection won’t require calling any asynchronous resources, we can safely use it here.


The next two are just going to be combinations of the above, but we’ll reproduce them here for completeness:


### SDL to introspection query result


This combines the steps to go from the SDL to a GraphQLSchema object, and then introspects that schema. As you can see, the GraphQLSchema object is a useful intermediate representation.


```text
const { buildSchema, graphqlSync, introspectionQuery } = require("graphql");


const sdlString = `
type Query {
hello: String
}
`;


const graphqlSchemaObj = buildSchema(sdlString);
const result = graphqlSync(graphqlSchemaObj, introspectionQuery).data;
```


### Introspection query result to SDL


In this case, we first build a schema object, then print it:


```text
const { buildClientSchema, printSchema } = require("graphql");
const fs = require("fs");


const introspectionSchemaResult = JSON.parse(fs.readFileSync("result.json"));
const graphqlSchemaObj = buildClientSchema(introspectionSchemaResult);
const sdlString = printSchema(graphqlSchemaObj);
```


---


## The wonderful world of GraphQL.js


One thing you might learn from this post is that[GraphQL.js](https://github.com/graphql/graphql-js) , the reference implementation of GraphQL in JavaScript, is an incredibly useful set of tools for working with GraphQL schemas and queries. It’s not the most thoroughly documented, but if you’re looking for a generic utility for working with GraphQL in JavaScript, there’s a good chance GraphQL.js has something that will help.


One caveat is that it might not always be a good choice for client use cases, since the library is quite large; for this reason we have elected to carefully import only the query parser and printer in Apollo Client, and write our own smaller utilities for other functions. But if you’re building a CLI tool or a server library, look around inside and you don’t know what wonders you might find!


Let me know if you have other cool tips for using GraphQL.js to work with GraphQL concepts, especially if you want to blog about it!


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
