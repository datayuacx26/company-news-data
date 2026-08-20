---
schema_version: "1.0.0"
document_id: "82c0438b8b6b350c3804807e77dd4e9ed2045fb7e9d750fa1151cb0cc6cbab84"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/4-simple-ways-to-call-a-graphql-api"
published_at: "2020-11-01T00:25:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:7a7ea80e5bd0be3b3540165c9fdbb20598d688f35cae345a339b4a15c12a918e"
---

# 4 Simple Ways to Call a GraphQL API

On this blog, we spend a lot of time pushing the limits of GraphQL and talking about some of the most advanced parts of the technology. However, GraphQL is inherently quite simple, and works just fine without any specialized tooling. So let’s demonstrate that by showing several very simple ways of calling a GraphQL API over HTTP.


## The example API


For this post, we’ll rely on the simple` todos` API from[apollographql/ac3-state-management-examples](https://github.com/apollographql/ac3-state-management-examples) .


Let’s clone the repo, install the dependencies, and start the server locally.


```text
git clone git@github.com:apollographql/ac3-state-management-examples.git
cd ac3-state-management-examples/apollo-remote-state/server
npm install
npm run start
```


If all goes well, you should see a message letting you know that your GraphQL server is up and running.


```text
🚀 Server ready at http://localhost:4000/
```


## 1. Using GraphQL IDEs (Apollo Explorer)


GraphQL IDEs let you build queries, browse schemas, and test out GraphQL APIs.


[Apollo Explorer](http://studio.apollographql.com/dev?referrer=blog) is a free cloud-based GraphQL IDE that comes with one-click query building, intelligent search, and a multitude of other productivity features to solve common pain-points we’ve heard from developers building apps with GraphQL.


Getting started should be faster than making a pot of coffee.


To use the explorer, we’ll head to[studio.apollographql.com/dev](http://studio.apollographql.com/dev?referrer=blog) and create an account (using either GitHub or your email).


Finally, choose a name for your graph, select the “Development” graph type, add your localhost endpoint (Apollo Server’s default is[http://localhost:4000](http://localhost:4000/) ), and click “Create Graph”.


And that’s it! You’re ready to start querying.


Here’s a query you can use to get started. Paste this into the *Operations* window and click the blue button to run the query.


```text
query AllTodos {
todos {
edges {
node {
completed
id
text
}
}
}
}
```


The Explorer is really useful! Not only is it easy to use, but we can see how long it took for the query to execute, download the responses, and keep track of all the operations you ran over time.


I don’t want to give you the impression that you need a specialized tool to access data from your GraphQL API. You don’t 😇. Let’s go way back to basics.


By the way, some other GraphQL IDEs you can check out are: Graph *i* QL, GraphQL Playground, and Insomnia.


## 2. Curl


curl is one of the most popular tools for accessing HTTP endpoints from the command line. It’s often used as a lowest-common-denominator example for how to access an API without any programming language or specialized tooling. So let’s use it to fetch GraphQL data!


Since curl is a command line tool, it’s easier to operate with queries that are just one line, so we’ll use the short one from above. Without further ado, here’s a curl command that will fetch a GraphQL query from our API:


```text
curl \
-X POST \
-H "Content-Type: application/json" \
--data '{ "query": "{ todos { edges { node { id completed text } } } }" }' \
http://localhost:4000/
```


It specifies four things about our request:


1. **The**` POST` **HTTP verb** , which is the most common one used by GraphQL APIs (although usually[GET is also supported](http://dev.apollodata.com/tools/apollo-server/requests.html) )
2. **The content type of**` application/json` , because we’re specifying the query as part of a JSON object
3. **The data sent** , which is a JSON document that looks like:` { "query": "{ todos { edges { node { id completed text } } } }" }` . This JSON wrapper is helpful because you can also specify other options, like dynamic variables as part of the JSON object.
4. **The actual URL of the GraphQL endpoint** , in this case` http://locahost:4000`


And that’s all! Using the above options, you can query nearly any GraphQL API, for example,[Trevor Blades](https://twitter.com/trevorblades) ‘[countries API](http://countries.trevorblades.com/) , which I love to use to introduce people to GraphQL for the first time:


```text
curl \
-X POST \
-H "Content-Type: application/json" \
--data '{ "query": "{ countries { code name } } " }' \
https://countries.trevorblades.com/
```


Alright great, so we’ve found a completely vanilla way to send requests. For more details about exactly how GraphQL queries work, read[Sashko Stubailo](https://twitter.com/stubailo) ‘s post,[“The Anatomy of a GraphQL Query.](https://dev-blog.apollodata.com/the-anatomy-of-a-graphql-query-6dffa9e9e747)


## 3. Fetch


You can make a GraphQL HTTP request in literally any programming language, as long as you can set the above 4 parts correctly. So let’s convert that curl request into some JavaScript code with` fetch` , the new standard for getting HTTP results with promises. This will work in modern browsers with no libraries, but will require a polyfill in Node and[some browsers](http://caniuse.com/#search=Fetch) . Let’s check out the code:


```text
require('isomorphic-fetch');


fetch('http://localhost:4000', {
method: 'POST',
headers: { 'Content-Type': 'application/json' },
body: JSON.stringify({ query: `
query {
todos {
edges {
node {
completed
id
text
}
}
}
}`
}),
})
.then(res => res.json())
.then(res => console.log(res.data));
```


If you run this code Chrome’s DevTools, you’ll notice that the following output is printed to the console.


Neat, right? As you can see, the way we request GraphQL data using Fetch is pretty similar to the request we constructed with curl, but there are a few things here that would become pretty repetitive if we had to type them out every time. Not only that, but to effectively work with GraphQL data in most applications, you’ll likely want to cache it, display the data somehow, and re-render your view when it changes.


That sounds like something that a GraphQL client can help us with.


## 4. Using GraphQL clients (Apollo Client)


GraphQL client libraries are meant to help us work with more effectively build fully-fledged applications with GraphQL data.


[Apollo Client](https://www.apollographql.com/docs/react/) handles the entire data-fetching and state management part of working with GraphQL data- we just need to hook it up and write the queries!


For example, if you’re building a React application, one of the most popular ways to query data using Apollo Client is to use the` useQuery` hook. Let’s demonstrate how we could fetch all the` todos` and display them in a list.


```text
import React from 'react'
import { gql, useQuery } from '@apollo/client'


const GET_ALL_TODOS = gql`
query GetAllTodos {
todos {
edges {
node {
completed
id
text
}
}
}
`;


export const Todos = () => {
const { loading, error, data } = useQuery(GET_ALL_TODOS);


if (loading) return 'Loading...';
if (error) return `Error! ${error.message}`;


return (
<ul>
{ data.todos.edges.map((edge, i) => (
<li key={edge.node.id}>{edge.node.text}</li>
))}
</ul>
)
}
```


Passing the GraphQL query to the` useQuery` hook gives us an object that can be de-structured into` loading` ,` error` , and` data` objects. If the query successfully executes and returns the GraphQL data from the server, we can map over the response` data` and display the elements in a list. Sweet!


To learn more about Apollo Client and how to set it up in your project, check out the[getting started docs](https://www.apollographql.com/docs/react/get-started/) .


If you’re an iOS or Android developer, you can use the[Apollo iOS](https://www.apollographql.com/docs/ios/) and[Apollo Android](https://github.com/apollographql/apollo-android) GraphQL clients which give you type-safe APIs to work with GraphQL servers.


## Conclusion


Because GraphQL operates over HTTP, you can use any language, client library, or tool to fetch GraphQL data so long as you format the HTTP request properly. If you’re just getting started with GraphQL, I recommend using a tool like[Apollo Explorer](http://studio.apollographql.com/dev?referrer=blog) that’ll help you understand how to structure requests and explore your schema. And if you’re getting ready to build a web application that relies on GraphQL data, try out a GraphQL client library like[Apollo Client](https://www.apollographql.com/docs/react/) ; it saves you time by handling the mundane parts of app development, and lets you focus on coding the parts that makes your app special.


---


Special thanks to[Sashko Stubailo](https://twitter.com/stubailo) who wrote the first version of this blog post nearly three years ago! Follow him on Twitter at[@stubailo](https://twitter.com/stubailo) .


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
