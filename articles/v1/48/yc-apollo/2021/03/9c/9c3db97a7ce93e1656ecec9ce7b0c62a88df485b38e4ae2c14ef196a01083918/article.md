---
schema_version: "1.0.0"
document_id: "9c3db97a7ce93e1656ecec9ce7b0c62a88df485b38e4ae2c14ef196a01083918"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/building-a-graphql-api"
published_at: "2021-03-04T12:16:08+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:8169fdc8f3bae923612725a4eb331f3bd06a8e2303d49eb479aa9ca0e2d66342"
---

# Building a GraphQL API – GraphQL API example

GraphQL is the modern way to create flexible, robust, and strictly-typed APIs.


In this post, we’ll walk through creating a *books* API from scratch. At the end, we’ll discuss the most common concerns and next steps as you build out your GraphQL API.


## Prerequisites


- You[understand what GraphQL is](https://www.apollographql.com/blog/what-is-graphql-graphql-introduction/)
- You know the difference between[GraphQL queries](https://www.apollographql.com/blog/what-is-a-graphql-query-graphql-query-examples-using-apollo-explorer/) and[mutations](https://www.apollographql.com/blog/graphql-mutation-vs-query-when-to-use-a-graphql-mutation/)
- You’re familiar with the command line
- You have Node.js (8+) installed


## Getting started


### Create a new project


Let’s start by creating a new folder for our project. Run the following command to create a directory and then` cd` into it.


```text
mkdir graphql-api-example
cd graphql-api-example
```


Use` npm` (or Yarn) to initialize a new Node.js project.


```text
npm init --yes
```


### Install the dependencies


We only need two dependencies for this project:` apollo-server` and` graphql` .


```text
npm install --save apollo-server graphql
```


At this point, we should have a` package.json` file and a` node_modules` folder. Let’s continue by creating the file we’ll do our work in.


### Create an index.js file


We’ll keep things simple by writing all of GraphQL API code in a single file *for now* . Create a new` index.js` file at the root of your project directory.


```text
touch index.js
```


## Building, running, and querying our GraphQL API


Now that we’ve got our project setup, let’s build and run our GraphQL API.


### Creating a schema


The schema is the *structure* of the data that clients can query for. Let’s create our *books* API. Clients can query for all books, and each book contains the title and the author of the book.


```text
const { ApolloServer, gql } = require('apollo-server');


// A schema is a collection of type definitions (hence "typeDefs")
// that together define the "shape" of queries that are executed against
// your data.
const typeDefs = gql`
# Comments in GraphQL strings (such as this one) start with the hash (#) symbol.


# This "Book" type defines the queryable fields: 'title' and 'author'.
type Book {
title: String
author: String
}


# The "Query" type is special: it lists all of the available queries that
# clients can execute, along with the return type for each. In this
# case, the "books" query returns an array of zero or more Books (defined above).
type Query {
books: [Book]
}
`;
```


### Writing resolvers


While the *schema* defines the structure of our API, resolvers define where the data comes from.


Apollo Server is what we call *data-source agnostic —* this means that you can fetch data from any source you like (SQL & NoSQL databases, REST APIs, other GraphQL APIs, or even just static JSON).


In this example, we’ll hardcode some data.


Add the following to the end of your` index.js` file.


```text
const books = [
{
title: 'The Great Gatsby',
author: 'F. Scott Fitzgerald',
},
{
title: 'Wuthering Heights',
author: 'Emily Brontë',
},
];
```


This is the data we’ll resolve! Notice that the structure matches the structure of our GraphQL schema?


Now, we’ll create the resolvers object and connect it to our *books* data.


```text
// Resolvers define the technique for fetching the types defined in the
// schema. This resolver retrieves books from the "books" array above.
const resolvers = {
Query: {
books: () => books,
},
};
```


### Creating the server instance


Now that we have our schema, data, and resolver, we’re ready to initialize an` ApolloServer` instance.


Place the following at the very end of your` index.js` file.


```text
// The ApolloServer constructor requires two parameters: your schema
// definition and your set of resolvers.
const server = new ApolloServer({ typeDefs, resolvers });


// The `listen` method launches a web server.
server.listen().then(({ url }) => {
console.log(`🚀  Server ready at ${url}`);
});
```


### Running


We’re ready to run our server. In the root directory of your project, run the following command to start your GraphQL API.


```text
node index.js
```


You should now see:


```text
🚀 Server ready at <http://localhost:4000/>
```


### Querying your API with a GraphQL IDE


There are lots of ways to query your API (read “[4 Simple Ways to Call a GraphQL API](https://www.apollographql.com/blog/4-simple-ways-to-call-a-graphql-api-a6807bcdb355/) “), but we recommend starting with a GraphQL IDE.


GraphQL IDEs let you build queries, browse schemas, and test out GraphQL APIs.


Apollo Explorer is a free cloud-based GraphQL IDE that comes with a ton of productivity features to solve common pain-points we’ve heard from developers building apps with GraphQL.


To use the explorer, we’ll head to[studio.apollographql.com/dev](https://studio.apollographql.com/dev?referrer=blog) and create an account (using either GitHub or your email).


Finally, choose a name for your graph, select the “Development” graph type, add your localhost endpoint (Apollo Server’s default is[http://localhost:4000](http://localhost:4000/) ), and click “Create Graph”.


And that’s it! You’re ready to start querying.


In the *Operations* window, paste the following query and then click the **blue “Run” button** query our *Books* API.


```text
{
books {
title
author
}
}
```


You should see our data on the right-hand side in the *Response* window!


Congrats! You’ve just created your first GraphQL API.


## What next?


### Continue building out your GraphQL API


Let’s not stop here. Chances are, your GraphQL API has a lot more than just one query. Check out our[Apollo Server docs](https://www.apollographql.com/docs/apollo-server/) to learn more about the[schema](https://www.apollographql.com/docs/apollo-server/schema/schema/) ,[resolvers](https://www.apollographql.com/docs/apollo-server/data/resolvers/) , and connecting your API to real data with[data sources](https://www.apollographql.com/docs/apollo-server/data/data-sources/) .


### Authentication & authorization


Determining who is using your API and what permissions they have is a common part of API design. I recommend starting with the[Apollo Server Security docs](https://www.apollographql.com/docs/apollo-server/security/authentication/) to learn[how to put user info in context](https://www.apollographql.com/docs/apollo-server/security/authentication/#putting-user-info-on-the-context) and[how to implement authorization in resolvers](https://www.apollographql.com/docs/apollo-server/security/authentication/#authorization-in-resolvers) .


### Security


With great power comes great responsibility. GraphQL’s flexibility is superb, but before we deploy to production, we should take care that we protect our API from malicious requests.


I recommend starting with our blog post titled “[Securing Your GraphQL API from Malicious Queries](https://www.apollographql.com/blog/securing-your-graphql-api-from-malicious-queries-16130a324a6b/) ” for an overview of how to mitigate common security concerns with DoS protection, query whitelisting, size, depth, and amount limiting.


### Schema splitting


In this post, we wrote all of our server code in a single file. That works when we’re just getting started, but things get unwieldy when our project grows in size.


Read this post, “[Modularizing your GraphQL schema code](https://www.apollographql.com/blog/modularizing-your-graphql-schema-code-d7f71d5ed5f2/) “, to learn how to better structure your project as it grows.


### Client-side GraphQL


Client-side GraphQL libraries like Apollo Client pair nicely with React and can be used to create robust web and mobile applications using GraphQL.


Check out the “[Apollo Client Best Practices Series](https://www.apollographql.com/blog/introducing-the-apollo-client-best-practices-series/) ” to learn how to use Apollo Client as the state management layer in your next client application.


### Deployment


When you’re ready to deploy your GraphQL API to production, read our[docs on Deployment](https://www.apollographql.com/docs/apollo-server/deployment/heroku/) . We have guides on deploying to Heroku, Lambda, Netlify, and Azure!


## Conclusion


Got 30 minutes? Want to take our *completely free* GraphQL course?


You’ll learn the core components of a full-stack GraphQL app with Apollo through an interactive tutorial.


You can[get started here](https://odyssey.apollographql.com/?referrer=blog) . Enjoy!


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
