---
schema_version: "1.0.0"
document_id: "5b1aed9738a8355c3977223405bdba320b31295006fb856c756fd53dd919cecf"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-use-graphql-with-postman"
published_at: "2021-03-18T00:01:12+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:5f3bb15fa556e1a25268e754c6fbc0885b201afd805bd59f779c356be002c4f4"
---

# How to use GraphQL with Postman – Postman testing with GraphQL

When building out APIs on the web, we’ll often find ourselves in the situation where we need to quickly verify that the route or API call we just created works the way we intend it to.


If you’re not using[Apollo Explorer](https://studio.apollographql.com/dev?referrer=blog) to test your GraphQL API (our free GraphQL IDE in Apollo Studio), another option is Postman. It works as a great tool for testing *any* HTTP endpoint – that means GraphQL too.


In this short post, we’ll walk through how to manually test your GraphQL API using Postman.


## Getting started


### Prerequistes


- You have Git installed
- You have NPM and Node.js (8+) installed
- You have the Postman Agent installed or a Postman account
- You understand the basics of GraphQL (you can read “[What is GraphQL? GraphQL Introduction](https://www.apollographql.com/blog/what-is-graphql-graphql-introduction/) ” to get up to speed)


### Clone, install, and start the server


We’re going to use the discography example from[apollo-server-starter](https://github.com/apollographql/apollo-server-starter) .


To pull the code from GitHub and start the server, paste the following command into a terminal on your machine.


```text
git clone https://github.com/apollographql/apollo-server-starter
cd apollo-server-starter
npm install && npm run start:example discography
```


If you see the following, your server is up and running:


```text
🚀 Server ready at http://localhost:4000/
```


### Review the schema


Let’s review our schema before we get into it. In the discography schema, you’ll find definitions for albums, tracks, and artists — and two queries that return either all albums, or a specific one.


```text
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
```


## Writing a GraphQL Request with Postman


The first query we’ll write is one that **fetches** **all albums** .


We begin by opening postman and selecting the option to write a new query.


Next, we begin the query by first writing the URL of the request. In our case, that’ll be @` http://localhost:4000/` .


Then, we’ll change the method to POST — this is necessary since all GraphQL operations work on top of the HTTP POST method.


And then we’ll formulate the body of the request.


Click the selection that says *Body* and then choose *GraphQL* in the selection list below it. You’ll be presented with a *Query* and a *GraphQL Variables* window.


Within this *Query* window, let’s write our query. Write or paste this code into it.


```text
query GetAllAlbums {
albums {
title
artist {
name
}
tracks {
title
number
}
genre
}
}
```


Finally, let’s click the blue *Send* button at the top right.


It all goes well, you should see the query results in the Response window at the bottom of the page.


That’s how you write GraphQL queries using Postman!


### Including variables


Just as often as we need to write queries that return data, we need to pass in variables to apply sorting and filtering through our data so that we can see relevant data.


To add GraphQL variables, we use the window of the same title on the right hand side.


Let’s use the` albums(genre: Genre!)` query and filter in on the list of albums by a particular genre.


First, we modify the query by giving it the the *Variable Definitions* necessary to make this query. Paste the following into the Query window.


```text
query GetAllAlbums ($genre: Genre) {
albums (genre: $genre) {
title
artist {
name
}
tracks {
title
number
}
genre
}
}
```


Then, in the GraphQL Variables window, we need to supply it with the data that we want to use as arguments.


Let’s filter in on albums with the` HipHop` genre. Paste the following into the GraphQL Variables window.


```text
{
"genre": "HipHop"
}
```


Your windows should look like this:


And when you’re ready, click the blue *Send* button again.


Looking at the results in the *Response* window at the bottom, it looks like we only got a single album back: “ *Paul’s Boutique* ” (good album 👍 ).


And that’s all there is to it.


## Conclusion


When you’re building out your GraphQL API, it’s a good idea to use a GraphQL IDE to test out your GraphQL operations. Keeping those feedback loops tight helps so that you can verify that things are working the way they should be. And since GraphQL works on top of HTTP, you can use Postman to manually test your API.


### Get started with Apollo Explorer


[Apollo Explorer](https://studio.apollographql.com/dev?referrer=blog) is a free cloud GraphQL IDE that we built from the ground up, **specifically for GraphQL developers** .


The Explorer comes with productivity-boosting features like one-click query building, intelligent search, and the ability to extract variables and fragments.


Apollo Explorer also comes with[development graphs](https://www.apollographql.com/docs/studio/dev-graphs/) , which enables you to build, test, and document your schema changes locally, as well as preview graph changes in local branches and PRs.


To use Apollo Explorer, head over to[studio.apollographql.com/dev](https://studio.apollographql.com/dev?referrer=blog) to get started with your first development graph.


Happy querying!


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
