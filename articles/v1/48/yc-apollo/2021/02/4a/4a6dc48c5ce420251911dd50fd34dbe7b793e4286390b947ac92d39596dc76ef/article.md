---
schema_version: "1.0.0"
document_id: "4a6dc48c5ce420251911dd50fd34dbe7b793e4286390b947ac92d39596dc76ef"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/what-is-a-graphql-query-graphql-query-using-apollo-explorer"
published_at: "2021-02-19T11:06:32+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:3a263cda2110eee2ad38e0b7418781242ca5529e3e7e4e5d1e4a8210f077eb7d"
---

# What is a GraphQL query? GraphQL query examples using Apollo Explorer

Queries are the one of the first things we learn about in GraphQL. Combined with great tooling, GraphQL’s declarative approach to asking for the data you want makes the query-writing experience quite enjoyable.


In this post, we’ll cover what GraphQL queries are and how they work. We’ll also learn how to write and execute GraphQL queries using[Apollo Explorer](https://studio.apollographql.com/dev?referrer=blog) : a powerful GraphQL IDE.


## Understanding GraphQL queries


### What is a GraphQL query?


In GraphQL, there are two primary operations we can perform: *queries* and *mutations* .


- Queries fetch data – like a` GET` in rest
- Mutations change data – like a` DELETE` or a` POST` in REST


Some people use the word *query* to refer to both GraphQL queries and mutations, but I prefer to use the word *operation* instead.


### What do GraphQL queries look like?


Here’s an example of a GraphQL query written three different ways.


```text
# GraphQL query with the query and name keywords
query GetPets {
pets {
name
petType
}
}


# GraphQL query with just the query keyword
query {
pets {
name
petType
}
}


# GraphQL query with neither the query or name keyword
{
pets {
name
petType
}
}
```


All three of these queries are functionally equivalent.


In GraphQL, the *query* and *name* keywords are optional. However, it’s recommended to use both of them anyway to:


1. Distinguish the operation type
2. Make it easier for your teammates to know what your query is for


The other interesting thing to note is that we use **nesting** to get related data. In the query examples above, we ask for the` name` and` petType` fields, which are sub-fields of the` pets` type.


### GraphQL query results


When we ask for data, we get a response back in a similar shape to the query, most commonly in JSON format.


If we executed our` GetPets` query above, our data would look like the following.


## How to execute GraphQL queries


We know what queries look like and we know what to expect in response when we write them. The next question is: how do we execute them?


It’s important to know that since GraphQL queries are just plain strings, we can use a variety of approaches to fetch data. Among the many options, some that come to mind are:


- curl
- fetch
- GraphQL client libraries (like Apollo Client)


And one other option: a **GraphQL IDE** .


When I’m building out a new GraphQL API, I typically recommend using a free GraphQL IDE like[Apollo Explorer](https://studio.apollographql.com/dev?referrer=blog) . Explorer connects to your GraphQL API running on your local machine and gives you a workspace to run build, run, and save queries.


The less time I spend manually writing queries, the better. For someone just getting started with GraphQL, it’s definitely recommend it for working with a local GraphQL API.


We’ll use Explorer to demonstrate writing and running GraphQL queries.


If you’re curious to learn about the other approaches, read “[4 Ways to Call a GraphQL API](https://www.apollographql.com/blog/4-simple-ways-to-call-a-graphql-api-a6807bcdb355/) “.


## Setup


We’re going to use a minimal` discography` GraphQL API example. It lives in the examples folder in the[Apollo Server Starter](https://github.com/apollographql/apollo-server-starter) on GitHub.


### 1. Clone the repo and start the server


Let’s clone the GitHub repo and start up the *initial* starter server.


```text
git clone https://github.com/apollographql/apollo-server-starter
cd apollo-server-starter
npm run start:dev
```


If all goes well, you should see a message telling you that your server is ready at` http://localhost:4000` .


### 2. Connect to your graph in Apollo Explorer


Next, we want to connect to our local graph from Explorer.


Head over to[Apollo Studio](https://studio.apollographql.com/dev?referrer=blog) . If you don’t already have an account, you can create one using your GitHub account.


When you’re in, you’ll want to click the “New graph” button at the **top right** .


Next, you’ll be prompted to give your new graph a **title** and choose a **graph type** .


- **Title:** You can name it whatever you want – I’m going to name mine` Discography`
- **Graph type** : Select *Development* under graph types. A dev graph lets you connect to a graph running locally on your machine.


Finally, select *Create Graph.*


You should now be in the Explorer, connected to our local graph running on` http://localhost:4000` .


### 3. Test our connection


Let’s test our connection. We’re currently hooked up to the default starter server which has the following server config.


```text
const { ApolloServer, gql } = require("apollo-server");


// Construct a schema, using GraphQL schema language
const typeDefs = gql`
type Query {
hello: String
}
`;


// Provide resolver functions for your schema fields
const resolvers = {
Query: {
hello: (root, args, context) => "Hello world!"
}
};


const server = new ApolloServer({
typeDefs,
resolvers
});


server.listen().then(({ url }) => {
console.log(`🚀 Server ready at ${url}`);
});
```


Our graph only has one thing to query: a` hello` field that returns a string. Let’s use that to test out our connection.


Ensure you have the following query in your *Operations* window.


```text
query ExampleQuery {
hello
}
```


And if we run the example by clicking the **blue play button** , we should get a JSON result back that says “Hello world!”


Boo-yah. There it is. We’re all set up and ready to go!


Let’s try some less trivial examples out.


## Examples


### Start the discography example graph


Let’s **stop the current server that we’re running by inserting a CTRL-Z** **into our console** , then run the following command to start up the` discography` GraphQL API example included in the starter repo.


```text
npm run start:example discography
```


Confirm that the server is running by checking the console.


In the Explorer, you should also notice that our` ExampleQuery` ‘s` hello` field has a red line through it now. That’s because our graph has changed from the *default* starter graph in the starter repo to the example` discography` one.


Let’s clear the existing query from the operation editor. Now we’re ready to rock & roll with some custom queries.


### Observe the GraphQL schema


The` discography` schema in[discography/examples/index.js](https://github.com/apollographql/apollo-server-starter/blob/main/examples/discography/index.js) has the following shape.


```text
const typeDefs = gql`


enum Genre {
Pop,
Rock,
Alternative
HipHop
}


type Track {
title: String!
number: Int!
}


type Artist {
name: String!
}


type Album {
title: String!
artist: Artist!
tracks: [Track!]!
genre: Genre!
}


type Query {
albums(genre: Genre): [Album!]!
album(title: String!): Album
}
`;
```


The key things to note about this GraphQL schema are:


- There are *two* top-level fields we can query:` albums` and` album`
- The` albums` field returns a list of albums and has an optional` Genre` variable.
- The` album` field returns either a single album or null. To query this, we need to pass in a value for the` title` variable.


### Example #1: Getting a list of data ━ GetAllAlbums


The first query we want to write is to fetch all albums.


We can compose the query by clicking the fields we want in the *Documentation* window on the left, starting with the top-level` albums` field. Then we can selected the nested fields we want beneath it.


Here’s a demonstration.


Pretty snappy, right? Much better than typing all of that out by hand.


If we press the **blue *Query* button** , you’ll see that it fires off the query and displays the result in the *Response* window on the right-hand side.


The composed GraphQL query looks like this:


```text
query Query($albumsGenre: Genre) {
albums(genre: $albumsGenre) {
title
tracks {
number
title
}
artist {
name
}
}
}
```


Explorer automatically lines up the possible variables you can use. Since we didn’t use them for this GraphQL query, we could simplify our query by writing it like this:


```text
query Query {
albums {
title
tracks {
number
title
}
artist {
name
}
}
}
```


Even better yet, we should give our query a name. As I mentioned before, it’s a good practice to name our queries so that other developers quickly understand what our query does.


```text
query GetAllAlbums {
albums {
title
tracks {
number
title
}
artist {
name
}
}
}
```


Perfect.


### Example #2: Getting a specific item ━ GetAlbumByTitle


Now we’re going to get a specific` album` , filtering in on the` title` field. This means we’re going to be using variables this time.


Let’s clear our operation window and click the *back arrow* to get ready to write our next query.


To write this query, I’m going to start by selecting the` album` field and then all the sub-fields I want to display (` title` ,` artist` ,` genre` ,` tracklist` ). After that, I’m going to to search for the album by typing its name in the *Variables* window towards the bottom-middle part of the screen.


See the demonstration below.


It worked!


Again, I recommend giving your query a good name. Your teammates will thank you. Here’s what the query looks like.


```text
query GetAlbumByTitle($albumTitle: String!) {
album(title: $albumTitle) {
title
artist {
name
}
genre
tracks {
title
number
}
}
}
```


## Summary


- A GraphQL operation can either be a *query* or a *mutation* . Queries fetch data and mutations change data (see here for[how to use mutations in a GraphQL client library](https://www.apollographql.com/docs/react/data/mutations/) ).
- We learned that we use *nesting* to ask for related data in a GraphQL query.
- We learned that the` query` and name keywords for a GraphQL operation are optional, but it’s recommended to use both to keep your queries readable and understandable.
- Since GraphQL queries are plain strings, we can use a variety of tools to write them.
- We learned how to use the[Explorer GraphQL IDE](https://studio.apollographql.com/dev?referrer=blog) to query a local GraphQL API with the dev graphs feature.


## Conclusion


Got 30 minutes? Want to take our *completely free* GraphQL course?


You’ll learn the core components of a full-stack GraphQL app with Apollo through an interactive tutorial.


You can[get started here](https://odyssey.apollographql.com/?referrer=blog) . Enjoy!


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
