---
schema_version: "1.0.0"
document_id: "78b8bef899de3d54c80c9aa0158b42aca8564d4a14d3d00d5cc41a4b47aea0c7"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/typescript-graphql-code-generator-generate-graphql-types"
published_at: "2021-04-01T11:00:04+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:79e9529471bc325b8c7b958d1159c705519bb1c0732578e63d54920d54d4435b"
---

# TypeScript GraphQL Code Generator – Generate GraphQL Types with Apollo Codegen Tutorial

Don Norman, the author of “The Design of Everyday Things” has a philosophy for what he believes constitutes good design. Don says that good design is primarily *discoverability* and *feedback* .


For example, when you encounter something new (like, a new codebase), how quickly can you discover what you can do (discoverability)? And then after you’ve taken some action, how immediately do you get to know if what you did was correct (feedback)?


Typed languages like TypeScript give us compile-time types, and **types act as an excellent form of feedback** . They let us know if what we’re attempting to do is going to work — and they let us know *immediately* .


For developers building client-side GraphQL applications using Apollo Client and React, compiling it with TypeScript is a good idea. But without compile-time types on our GraphQL operations, we have to wait until we execute our queries and mutations to tell if we’re doing something wrong.


In this post, you’ll learn how to use Apollo’s GraphQL codegen to generate TypeScript types for GraphQL operations in your Apollo Client project.


## Apollo’s GraphQL Codegen


The Apollo CLI (which lives on GitHub at[apollographql/apollo-tooling](https://github.com/apollographql/apollo-tooling) ) contains a collection of helpful utilities to assist us with maintaining GraphQL applications in production.


One of the tools from the CLI is the **GraphQL codegen feature** . It lets us:


1. Download any GraphQL schema using a GraphQL endpoint
2. Generate TypeScript types for queries written in an Apollo Client project (given a valid GraphQL schema)


And that’s exactly what we’ll do. Let’s get started.


## Prerequisites


- You’re familiar with the command line
- You have Node.js (8+) installed
- You know how to build a full-stack GraphQL application. If not,[check out this blog post for our recommended tutorials](https://www.apollographql.com/blog/learn-graphql-tutorial-for-full-stack-graphql) .


## Getting started


The project I’ll use to demonstrate how to generate TypeScript types for GraphQL operations is[apollographql/ac3-state-management-examples](https://github.com/apollographql/ac3-state-management-examples/tree/master/apollo-remote-state) (the *apollo-remote-state* example, to be specific).


You can fork, download, or browse the code online to see the full setup.


### Installation


Assuming we’re in the root directory of an Apollo Client project, we need to install two development dependencies:` apollo` and` @types/graphql` .


```text
npm install --save-dev apollo @types/graphql
```


The` apollo` dependency is the Apollo CLI while the` @types/graphql` dependency gives us better type completion for GraphQL operations.


### Downloading the schema


Next, we’d like to download the GraphQL schema that we’re working with. In the ac3-state-management-examples repo, the **apollo-remote-state** server lives in` /server` and runs at` http://localhost:4000/` .


With the server started, we’d like to download the server to a JSON file called` graphql-schema.json` .


From the root of your Apollo Client project, paste the following command:


```text
npx apollo service:download --endpoint=http://localhost:4000/ graphql-schema.json
```


Note: *npx apollo **schema:download** is an alias for this command and should also work if you swap it out for npx apollo **service:download** .*


You should now see a` graphql-schema.json` file in the root of your Apollo Client project directory. That’s good.


Next, we’ll use this schema + the GraphQL operations in our app to generate TypeScript types for them.


## Generating types for remote GraphQL operations


### A sample query


The ac3-state-management project has several queries in it already (see[here](https://github.com/apollographql/ac3-state-management-examples/tree/master/apollo-remote-state/client/src/operations) ). One of the queries is to *get all todos* from the backend GraphQL API. It looks like this:


```text
// apollo-remote-state/client/src/operations/queries/getAllTodos.tsx
import { gql } from "@apollo/client";


export const GET_ALL_TODOS = gql`
query GetAllTodos {
todos {
edges {
node {
id
text
completed
}
}
}
}
`
```


Let’s see if we can generate the appropriate TypeScript types for this query so that we can get type completion when we use it in our components.


### Generating types for all operations


The command to generate TypeScript types for all GraphQL operations written in our Apollo Client project is as follows:


```text
npx apollo codegen:generate --localSchemaFile=graphql-schema.json --target=typescript --tagName=gql
```


What we’re doing here is telling the Apollo CLI that we’d like to execute the GraphQL codegen feature (denoted by the` codegen:generate` option). To do this, we use the` localSchemaFile` , that is — our` graphql-schema.json` . The GraphQL codegen feature uses this schema file to verify that the queries we’re writing will compile against our graph, which we’ve pulled locally. The` target` flag indicates the output type (TypeScript), and the` tagName` is the *template* that the CLI looks for in our code in order to distinguish what *is* and what *isn’t* a GraphQL query.


If all goes well, you should see a message in your console like the following:


And if it doesn’t work — don’t panic; there’s likely a great reason why. We’ll discuss below. Keep reading!


You should now find` __generated__` folders within your project containing the generated type definitions for your GraphQL operations. Here’s an example of what the type defs for` GetAllTodos` looks like:


```text
/* tslint:disable */
/* eslint-disable */
// This file was automatically generated and should not be edited.


// ====================================================
// GraphQL query operation: GetAllTodos
// ====================================================


export interface GetAllTodos_todos_edges_node {
__typename: "Todo";
id: number;
text: string;
completed: boolean;
}


export interface GetAllTodos_todos_edges {
__typename: "TodosEdge";
node: GetAllTodos_todos_edges_node;
}


export interface GetAllTodos_todos {
__typename: "TodosConnection";
edges: (GetAllTodos_todos_edges | null)[];
}


export interface GetAllTodos {
todos: GetAllTodos_todos;
}
```


Fantastic!


### Using the generated GraphQL operation types


Now, we can utilize our types for the GraphQL operations in our code. Here’s a stripped down example of what a` TodoList` component might look like utilizing our new types.


```text
import React from 'react'
import TodoList from '../components/TodoList';
import { Todos } from '../models/Todos';
import { useQuery } from '@apollo/client';
import { GetAllTodos } from '../operations/__generated__/GetAllTodos'; // Import
import { GET_ALL_TODOS } from '../operations/queries/getAllTodos';
...


export default function VisibleTodoList () {
...
const {
loading: isTodosLoading,
data: todosConnection,
error: todosError
} = useQuery<GetAllTodos>( // Use the type here for type safety
GET_ALL_TODOS
);


if (isTodosLoading) return <div>Loading...</div>
if (todosError) return <div>An error occurred {JSON.stringify(todosError)}</div>
if (!todosConnection) return <div>None</div>;


const todos: Todos = todosConnection.todos.edges.map((t) => t?.node) as Todos;


return <TodoList
todos={todos}
/>;
}
```


## Generating types for client-only types


If the` codegen:generate` script *did not work for you* , a common reason is that we’re working with[client-only types](https://www.apollographql.com/blog/local-state-management-with-reactive-variables/) .


Client-only types are types that exist *only* on the client, but not in the remote GraphQL API. We can use them, along with[Apollo Client 3’s Reactive Variables API](https://www.apollographql.com/blog/local-state-management-with-reactive-variables/) to represent local state. Here’s an example of a query that uses the` @client` directive for a client-only type.


```text
import { gql } from "@apollo/client";


export const GET_VISIBILITY_FILTER = gql`
query GetVisibilityFilter {
visibilityFilter @client {
id
displayName
}
}
`
```


How do we remedy this? We extend the SDL of our graph with a local schema.


### Extending your graph with a local schema


Since the Apollo CLI relies on a GraphQL schema to determine what *is* and *what isn’t* in your graph, and which operations can be typed, and which ones can’t, we can *extend* the graph with our client-only types by creating a local schema file.


Create a new file in the root of your project called` local-schema.graphql` . Here’s where we’ll put the types that exist *locally* but not remotely. In this case, since` visibilityFilter` sits right on the root` Query` type, we’ll need to use the` extends` keyword to add our new type to` Query` .


```text
// local-schema.graphql
type VisibilityFilter {
id: String!
displayName: String!
}


extend type Query {
visibilityFilter: VisibilityFilter!
}
```


Perfect. Now, we need to use *this* new file **in addition** to the original` graphql-schema.json` file that we generated from backend GraphQL API. Paste the following script into your command line:


```text
apollo codegen:generate --localSchemaFile=graphql-schema.json,local-schema.graphql --target=typescript --tagName=gql
```


And now, we’ve got our TypeScript type definitions for both our remote GraphQL API *and* our local state *client-only* types as well!


## npm GraphQL codegen scripts


You may want to run these scripts periodically throughout the lifetime of your project. I recommend saving your codegen command as an NPM script. Here’s an example of what it might look like for you.


```text
"scripts": {
...
"download-schema": "apollo service:download --endpoint=http://localhost:4000/ graphql-schema.json",
"codegen": "npm run download-schema && apollo codegen:generate --localSchemaFile=graphql-schema.json,client-schema.graphql --target=typescript --tagName=gql"
},
```


## Conclusion


We just learned how to use the graphQL codegen feature from Apollo’s CLI tool to generate TypeScript types for GraphQL operations in an Apollo Client project! That should help you move a little faster and write less bugs.


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
