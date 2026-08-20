---
schema_version: "1.0.0"
document_id: "326fd6ea9e278bf134dd8218d2e419584cb57c2402626ed8e104b389d79e6004"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/making-graphql-requests-using-http-methods"
published_at: "2021-08-20T11:30:05+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:9b47f3da6f7ffe840880bad80dd43ccfe5e68632614c4bb36caf235172e71810"
---

# Making GraphQL Requests using HTTP Methods

When I first started learning about GraphQL, I thought that I needed a special tool to make GraphQL requests. Luckily, you don’t!


While we recommend using Apollo Studio’s[Sandbox](https://www.apollographql.com/blog/announcement/platform/apollo-sandbox-an-open-graphql-ide-for-local-development/) , since GraphQL runs on top of HTTP, it’s possible to make GraphQL requests using any of the tools you were accustomed to using building out your apps with REST.


In this post, we’ll learn how to make GraphQL requests over HTTP with just GET and POST methods. We’ll also discuss the limitations of each approach.


## Prerequisites


- You know[what GraphQL is](https://www.apollographql.com/blog/graphql/basics/what-is-graphql-introduction/)
- You know[the difference between a query and a mutation](https://www.apollographql.com/blog/graphql/basics/mutation-vs-query-when-to-use-graphql-mutation/)
- You’re familiar with[the basics of Apollo Server](https://www.apollographql.com/docs/apollo-server/getting-started/) and have completed the getting started guide


## An Apollo Server example


Let’s assume we have a basic Apollo Server that returns a greeting with an optional` name` variable:


```text
const { ApolloServer, gql } = require('apollo-server');


const resolvers = {
Query: {
hello: (_, args) => {
let value;
if (args.name) {
value = `Hello ${args.name}, nice to meet you!`
} else {
value = `Hello, nice to meet you!`
}
return { value }
}
}
}


const typeDefs = gql`
type Hello {
value: String
}


type Query {
hello(name: String): Hello
}
`


const server = new ApolloServer({
resolvers,
typeDefs
})


server.listen().then(({ port }) => {
console.log(`Server started on port: ${port}`)
})
﻿
```


Apollo Server accepts either POST or GET HTTP methods as GraphQL requests. Let’s start with the POST method.


## Making GraphQL requests using the POST method


To make a GraphQL request using the POST HTTP method, we pass the following properties into the JSON body of the request.


- ` query` — the full[GraphQL query](https://www.apollographql.com/blog/graphql/basics/the-anatomy-of-a-graphql-query/) containing the operation type (can be either` query` or` mutation` ), the types & fields requested, and any variables included
- ` operationName` — optional, but if included, *must* be present in the` query`
- ` variables` — optional *if* there are no variables included in the` query`


Here’s an example of a GraphQL request including an` operationName` and` variables` .


```text
{
"query": "query GreetingQuery ($arg1: String) { hello (name: $arg1) { value } }",
"operationName": "GreetingQuery",
"variables": { "arg1": "Timothy" }
}
```


We’d expect to see a response like the following:


```text
{
"data": {
"hello": {
"value": "Hello Timothy, nice to meet you!"
}
}
}
```


## Making a GraphQL requests using the GET method


You can also make GraphQL requests using the GET HTTP method. The rules for making GraphQL requests this way are almost the same as the POST version — there are just slight differences:


- Each property is provided as an HTTP query parameter (values separated by an ampersand —` &` )
- **Limitation (variables):** Because they need to be parsed as JSON, GraphQL requests with variables don’t work in GET requests.
- **Limitation (operations)** : You can only execute` query` operations (` mutation` operations don’t work)


That said, the following request works nicely!


```text
GET http://localhost:4000/?query=query GreetingQuery { hello { value } }&operationName=GreetingQuery
```


Which, when encoded for the web, looks like:


```text
GET http://localhost:4000/?query=query%20GreetingQuery%20{%20hello%20{%20value%20}%20}&operationName=GreetingQuery
```


We could expect a response like the following.


```text
{
"data": {
"hello": {
"value": "Hello, nice to meet you!"
}
}
}
```


## Conclusion


In this post, we discussed how you can use HTTP GET and POST methods to send GraphQL requests. We also looked at some of the nuances and limitations of each approach.


If you’re just getting started with Apollo Server, you’ll see a screen similar to the following when you visit your running server in the browser.


Clicking “Query your server” takes you to Apollo Studio’s sandbox and gives you an environment where you can quickly navigate and test out your GraphQL server. While you can use other tools, we recommend Sandbox as the best way to get started.


---


For demonstrations of other ways to make GraphQL requests, read “[4 Simple ways to Call a GraphQL API](https://www.apollographql.com/blog/graphql/examples/4-simple-ways-to-call-a-graphql-api/) “.


For more on sending GraphQL requests with HTTP, read “[POST and GET format — How to send requests to Apollo Server over HTTP](https://www.apollographql.com/docs/apollo-server/requests/) ” from the official Apollo Server docs.


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
