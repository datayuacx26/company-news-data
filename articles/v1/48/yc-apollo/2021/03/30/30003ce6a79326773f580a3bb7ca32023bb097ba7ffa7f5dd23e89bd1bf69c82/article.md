---
schema_version: "1.0.0"
document_id: "30003ce6a79326773f580a3bb7ca32023bb097ba7ffa7f5dd23e89bd1bf69c82"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-search-and-filter-results-with-graphql"
published_at: "2021-03-23T11:30:04+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:a605bd8de408aef661a23898b9c89bb6559818c7bdb0feec42af306c8257c445"
---

# GraphQL Search and Filter – How to search and filter results with GraphQL

Searching and filtering is a standard part of any GraphQL API. In this article, we’ll learn how to add filtering capabilities to your API by walking through a few real-world examples.


To learn how to handle searching and filtering on the front-end using Apollo Client, read “[How to Filter and Search using Variables in Apollo Client](https://www.apollographql.com/blog/apollo-client/how-to-filter-and-search-using-variables-in-apollo-client/) “.


## Examples


**Searching *is* filtering** : Searching and filtering are two different ways to say the same thing. The goal is to find an item (or a list of items) that matches some sort of criteria. For the rest of this post, we’ll use the word *filter* instead of search.


For each filtering example, we’ll take a look at the originating schema, how to write a query within it, and a resolver implementation.


As a reminder, GraphQL is data source-agnostic. This means that you can use whatever database technology you like to fetch and save data; for instance — SQL, ORMs, NoSQL, JSON, or in-memory data all work just fine.


For these examples, we’ll assume we’ve stored the data *in-memory* .


### Filtering for a specific item


Consider the following GraphQL schema:


```text
type Query {
album(id: ID!): Album
}
```


The` album` field expects an` ID` to be provided as an argument, and if found — it’ll return the` Album` type.


To fetch this, we could write a query that passes in an` id` and asks for the` album` with the` title` ,` artist` and` genre` fields. The query could look like this:


```text
query GetAlbumById {
album (id: "1") {
title
genre
artist {
name
}
}
}
```


And in the[resolver](https://www.apollographql.com/docs/apollo-server/data/resolvers/) on the server, we’d pull the` id` value out from` args` and use it to filter our data.


```text
const resolvers = {
Query: {
album (parent, args, context, info) {
const { id } = args;
return context.db.Albums.find((a) => a.id == id)
}
}
}
```


### Filtering down a list


Now consider our GraphQL schema had an additional field on the root` Query` type to query for a *list* of albums:


```text
type Query {
album(id: ID!): Album
albums(genre: Genre): [Album]! # New
}
```


With this new field, we can query for the list — optionally filtering it down using the` Genre` argument.


Here’s what a query might look like:


```text
query GetRockAlbums {
albums (genre: "Rock") {
title
genre
artist {
name
}
}
}
```


And the resolver:


```text
const resolvers = {
Query: {
albums (parent, args, context, info) {
const { genre } = args;
return context.db.Albums
.filter((a) => a.genre == genre)
}
}
}
```


### Filtering for items matching a list


Let’s say you wanted to get a very specific set of items and you already knew their` ids` .


We could adjust our GraphQL schema to look more like this:


```text
type Query {
album(id: ID!): Album
albums(genre: Genre, ids: [ID!]): [Album]!
}
```


By the way, do you notice that the parameter list is starting to get a little bit lengthy? There’s a way to clean that up, and we’ll do that soon — but first, let’s see what a query for this might look like.


The following query asks for albums with the ids` 1` ,` 4` ,` 5` ,` 6` , and` 8` .


```text
query GetAlbumsByIds {
albums (ids: ["1", "4", "5", "6", "8"]) {
title
genre
artist {
name
}
}
}
```


To handle this sort of query in the resolver, we could write it this way:


```text
const resolvers = {
Query: {
album (parent, args, context, info) {
const { ids } = args;
return context.db.Albums.filter((a) => ids.includes(a.id))
}
}
}
```


### Complex filtering using an Input Type


In the last example, we started to see that the parameter list was getting long. A better approach is to design an *Input Type* containing all the possible filtering (or sorting) options necessary, and use that as a single parameter, encapsulating all of the arguments for the field.


Instead of this:


```text
type Query {
album(id: ID!): Album
albums(genre: Genre, ids: [ID!]): [Album]!
}
```


Let’s try this:


```text
type AlbumsFilters = {
genre: Genre
ids: [ID!]
}


type AlbumsInput = {
filter: AlbumsFilters
}


type Query {
album(id: ID!): Album
albums(input: AlbumsInput): [Album]!
}
```


To handle queries for` albums` in the resolver, we’d want to take care to handle both the cases of the` genre` filter being applied, the` ids` filter being applied, and both of them being applied.


```text
const resolvers = {
Query: {
album (parent, args, context, info) {
const { filter } = args;
const shouldApplyFilters = filter !== null;


let albums = context.db.Albums;


if (!shouldApplyFilters) {
return albums;
}


const shouldApplyGenreFilter = filter.genre !== null;
const shouldApplyIdsFilter = filter.ids;


if (shouldApplyGenreFilter) {
albums = albums
.filter((a) => a.genre === filter.genre)
}


if (shouldApplyIdsFilter) {
albums = albums.filter((a) => ids.includes(a.id))
}


return albums;
}
}
}
```


While this *does* add complexity to the resolver, it makes the querying experience much more robust.


Using the *Input Type* pattern, you can incrementally add more filtering (or sorting) options to your API like:


- **Filtering between ranges (greater than, less than, equal)**
- **Conditional filtering (and, or, not, exists, etc)**
- **Any other filtering criteria you can think of**


## Conclusion


We just walked through a few examples of how to add filter functionality to your GraphQL API. You should be all set to implement filtering in your own GraphQL API.


### Develop Your GraphQL API with Apollo Explorer


Apollo Explorer is a free cloud GraphQL IDE that we built from the ground up, **specifically for GraphQL developers** .


The Explorer comes with productivity-boosting features like one-click query building, intelligent search, and the ability to extract variables and fragments.


Apollo Explorer also comes with[development graphs](https://www.apollographql.com/docs/studio/dev-graphs/) , which enables you to build, test, and document your schema changes locally, as well as preview graph changes in local branches and PRs.


To use Apollo Explorer, head over to[studio.apollographql.com/dev](https://studio.apollographql.com/dev?referrer=blog) and get started with your first development graph.


Happy querying!


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
