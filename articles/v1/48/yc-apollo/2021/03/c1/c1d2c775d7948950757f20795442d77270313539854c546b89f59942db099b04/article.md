---
schema_version: "1.0.0"
document_id: "c1d2c775d7948950757f20795442d77270313539854c546b89f59942db099b04"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-use-graphql-with-javascript-graphql-js-tutorial"
published_at: "2021-03-25T12:01:03+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:26f93b657204cf5cf4cd25bd02a4503cd9822c33a4736ca534e709240171b730"
---

# How to use GraphQL with Javascript – GraphQL.js tutorial

GraphQL is first and foremost: a query language. It’s a more flexible and robust way to ask for data. You can use JavaScript to execute GraphQL anywhere. In this short post, we’ll take a look at the underlying JavaScript GraphQL implementation used by many popular GraphQL libraries and frameworks:` graphql-js` .


## Prerequisites


- You[understand what GraphQL is](https://www.apollographql.com/blog/what-is-graphql-graphql-introduction/)
- You’re familiar with the command line
- You have Node.js (8+) installed


## What is GraphQL.js


[GraphQL.js](https://github.com/graphql/graphql-js) is the *reference implementation* of GraphQL for JavaScript. By *reference implementation* , we mean that it adheres to the rules declared in the official[GraphQL specification](https://spec.graphql.org/June2018/) .


Other languages have also implemented the GraphQL specification, like[Java](https://github.com/graphql-java/graphql-java) , .[NET](https://github.com/graphql-dotnet/graphql-dotnet) , and[Go](https://github.com/graphql-go/graphql) .


## Getting started with GraphQL.js


Let’s install GraphQL.js and execute a query with it.


### Create a new project


We’ll start by creating a new folder for our project. Run the following command to create a directory and then` cd` into it.


```text
mkdir graphql-js
cd graphql-js
```


Use` npm` (or Yarn) to initialize a new Node.js project.


```text
npm init --yes
```


### Installation


To install` graphql-js` , run the following command:


```text
npm install --save graphql
```


This should create a` package.json` file and a` node_modules` folder. Next, we’ll create the file that we’re going to work within.


### Create an index.js file


We’ll write our code in a single file. Create a new` index.js` file in the current directory.


```text
touch index.js
```


## Writing a query in GraphQL.js


The following code constructs a schema with the root` Query` type containing a` hello` field. Then, we create a resolver to resolve the value for the` hello` field and run a query by passing in the schema, the query *itself* , and graph resolvers.


```text
const { graphql, buildSchema } = require('graphql');


// Builds a schema using the GraphQL schema language
const schema = buildSchema(`
type Query {
hello: String
}
`);


// The root of our graph gives us access to resolvers for each type and field
const resolversRoot = {
hello: () => {
return 'Hello world!';
},
};


// Run a simple graphql query '{ hello }' and then print the response
graphql(schema, '{ hello }', resolversRoot).then((response) => {
console.log(JSON.stringify(response.data));
});
```


### Running the query


To run this code, in your console — type the following command:


```text
node index.js
```


If all goes well, you should see this:


```text
{ "hello": "Hello world!" }
```


This is how we can use plain JavaScript to write GraphQL queries with GraphQL.js.


## Next steps


### Serve GraphQL over HTTP with a Server Library


GraphQL.js is the definitive JavaScript GraphQL implementation, but most of us are using GraphQL to build backend APIs. For that to work, we need to serve GraphQL over HTTP.


We covered how GraphQL.js works (under the hood of other GraphQL JavaScript libraries like Apollo Server), but I recommend reading “[Using Express with GraphQL – How to create a GraphQL server with Node.js/Express](https://www.apollographql.com/blog/using-express-with-graphql-server-node-js/) ” to learn how to connect GraphQL to a web server.


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
