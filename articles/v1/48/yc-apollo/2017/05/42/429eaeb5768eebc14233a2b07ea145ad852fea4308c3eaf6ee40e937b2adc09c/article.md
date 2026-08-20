---
schema_version: "1.0.0"
document_id: "429eaeb5768eebc14233a2b07ea145ad852fea4308c3eaf6ee40e937b2adc09c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-tools-a-simpler-way-to-create-graphql-apis-eadf018f3766"
published_at: "2017-05-30T00:34:13+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:e045ae4b8979c40be8e0adda5f94b076818db58cb303c6e8a3327b0251038bb2"
---

# graphql-tools: A simpler way to create GraphQL APIs

UI developers love GraphQL because it lets them fetch data quickly and efficiently, without having to make multiple API calls or trying to make sense of outdated documentation. With so many benefits on the client, it might seem like implementing a server to get all of these features might be very difficult. And a year and a half ago, when GraphQL had just been open sourced, it certainly looked that way! There weren’t many examples, and the syntax for defining types and fields looked pretty complicated at first glance.


Back in January of 2016,[Jonas Helfer](https://medium.com/u/39cbd38c57c8?source=post_page-----eadf018f3766----------------------) and I were looking for ways to make working with GraphQL simpler, and there was one thing that caught our eye — the schema definition language:


```text
type Person {
name: String
age: Int
picture: Url
}
```


It showed up everywhere in the[specification](https://facebook.github.io/graphql/#sec-Objects) , but it was just used as a nice shorthand. You couldn’t actually use it to create a GraphQL server. But… what if you could? Thankfully, as we were thinking about this, GraphQL.js was quickly improving its experimental support for the schema language in the parser. There weren’t enough features yet to build a complete server with modular parts, interfaces, and more, but we were on a mission to make developing GraphQL servers as easy as possible. So, Jonas built a prototype of Apollo’s first GraphQL library,[graphql-tools](https://github.com/apollographql/graphql-tools) , which lets you create a GraphQL API using just the schema language.


### graphql-tools 1.0 🎉


Today, a year and a half after the project started, Jonas and I are pleased to announce the 1.0 release of` graphql-tools` , with special thanks to long-time contributor[Hagai Cohen](https://github.com/dxcx) !` graphql-tools` is production-ready and has been battle-tested by dozens of companies in their production applications.


Here are some of the things you can do with` graphql-tools` :


1. [Create a schema](http://dev.apollodata.com/tools/graphql-tools/generate-schema.html) using the GraphQL Schema Definition Language, while taking advantage of[all of the features](http://dev.apollodata.com/tools/graphql-tools/resolvers.html) of GraphQL.js including resolvers, interfaces, and unions
2. Import[custom scalar types](http://dev.apollodata.com/tools/graphql-tools/scalars.html) from npm and also create your own
3. Completely[mock your API](http://dev.apollodata.com/tools/graphql-tools/mocking.html) based on a schema definition and customize those mocks
4. Handle some of the[common errors](http://dev.apollodata.com/tools/graphql-tools/errors.html) that come up when developing a server


At the end, you get a regular GraphQL.js schema object, just like you would with the reference implementation directly. You can use that with your favorite GraphQL server middleware, such as[graphql-server-express](http://dev.apollodata.com/tools/graphql-server/index.html) or[express-graphql](https://github.com/graphql/express-graphql) . If there’s something you think is missing, please file an issue or submit a PR — we want to keep` graphql-tools` very lightweight and un-opinionated, but we’re always looking for potential improvements!


## Building a simple GraphQL server


If you’re new to GraphQL,` graphql-tools` is a great way to build your first server. In this post, we’ll go over some of the most basic usage, including:


1. A hello world schema (and an intro to Launchpad, a tool for learning about GraphQL servers).
2. A simple read-only schema with authors and posts.
3. The same schema with a mutation added.


Let’s get to it, and hopefully by the end of the post you see that prototyping a simple GraphQL schema can be super easy! In future posts, we’ll cover some more advanced topics such as mocking and accessing databases and APIs.


### Hello world with graphql-tools


Here’s how you write a hello world GraphQL schema with` graphql-tools` :


```text
import { makeExecutableSchema } from 'graphql-tools';// Construct a schema using the GraphQL schema language
const typeDefs = `
type Query {
hello: String
}
`;// Provide resolver functions for your schema fields
const resolvers = {
Query: {
hello: (root, args, context) => {
return 'Hello world!';
},
},
};// Get a GraphQL.js Schema object
export const schema = makeExecutableSchema({
typeDefs,
resolvers,
});
```


[Click here to see a live demo and edit the code in your browser!](https://launchpad.graphql.com/new)


In this blog post, with every code snippet we’re going to include a link to a live example on[Launchpad](https://dev-blog.apollodata.com/introducing-launchpad-the-graphql-server-demo-platform-cc4e7481fcba) , a new GraphQL server demo platform. Log in and click “Fork” to edit the code, or “Download” to get a Node.js app you can run locally!


There are two parts that every GraphQL schema needs:


1. A schema that defines the fields and types in the API; this is kind of like the route definition in a REST API.
2. Resolvers, which are functions called during field execution, similar to controller methods in REST.


Now let’s make it a bit more interesting…


### Nested types and queries


One of the biggest benefits of a GraphQL API is that it isn’t just a flat list of endpoints — it’s actually a “graph” of types that can reference each other. Let’s create a simple API with two types that reference each other. Check out the[full example on Launchpad](https://launchpad.graphql.com/890l4w5wq) , and we’ll highlight the important parts below:


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
}type Query {
posts: [Post]
author(id: Int!): Author
}
```


You can see we have both one-to-many and one-to-one relationships here: Authors have multiple posts, but each post has exactly one author.


The schema language is quite intuitive — the` type` keyword specifies an “object type” which can have multiple fields, and you can use` \[ \]` to specify a list and` !` to specify a non-null field (all items must have an` id` in this schema). The` Query` type is special, since it defines the set of fields available at the first level of the query.


Let’s take a look at the resolvers here:


```text
const resolvers = {
Query: {
posts: () => posts,
author: (_, args) => find(authors, { id: args.id }),
},
Author: {
posts: (author) => filter(posts, { authorId: author.id }),
},
Post: {
author: (post) => find(authors, { id: post.authorId }),
},
};
```


These resolvers are simple functions, and in this example they read from arrays of` authors` and` posts` right in the code, since we don’t want to get into data fetching yet. Let’s take a look at that fake data:


```text
const authors = [
{ id: 1, firstName: 'Tom', lastName: 'Coleman' },
{ id: 2, firstName: 'Sashko', lastName: 'Stubailo' },
{ id: 3, firstName: 'Mikhail', lastName: 'Novikov' },
];const posts = [
{ id: 1, authorId: 1, title: 'Introduction to GraphQL' },
{ id: 2, authorId: 2, title: 'GraphQL Rocks' },
{ id: 3, authorId: 2, title: 'Advanced GraphQL' },
{ id: 4, authorId: 3, title: 'Launchpad is Cool' },
];
```


Note that we don’t have to specify resolvers for every field, GraphQL.js has a concept of “default resolvers”, which means it will pick up the` firstName` ,` lastName` , and` title` fields on the objects for us. Let’s check out work by[running the query below](https://launchpad.graphql.com/890l4w5wq) in GraphiQL on Launchpad:


```text
{
posts {
title
author {
firstName
}
}
}
```


We got some results, awesome! We can also fetch from the other perspective, starting with an author:


```text
{
author(id: 2) {
firstName
posts {
title
}
}
}
```


Great, now we’re starting to see some of the power of relationships between types in GraphQL.


### Adding a mutation


For now we’ve only worked with read-only data. What if we want to update the data as well? GraphQL has a concept for this, called “mutations”. Like queries are used to fetch data, mutations are used to update something and then fetch new results.


First, let’s add a new type to our schema:


```text
type Mutation {
addPost(authorId: Int!, title: String!): Post
}
```


Now, all we have to do is add a resolver for this field:


```text
const resolvers = {
// ... existing resolvers
Mutation: {
addPost: (_, args) => {
const post = {
id: posts.length,
authorId: args.authorId,
title: args.title,
};      posts.push(post);


return post;
}
}
};
```


After you’re done, it should look[like this](https://launchpad.graphql.com/vkqrnvvp3) . Now we can run the mutation in Graph *i* QL:


```text
mutation AddPost {
addPost(authorId: 1, title: "Added via launchpad") {
id
title
author {
firstName
}
}
}
```


Note that in a GraphQL mutation, we can refetch as much data as we want. Then when we query all posts, we will get the new post we just added in the list! Didn’t even break a sweat, and we can already see some of the work GraphQL is doing for us with fetching related data after a mutation. To make sure you’ve followed along, check out the[complete example](https://launchpad.graphql.com/vkqrnvvp3) .


Note: Since we’re just storing in an in-memory array, when our demo container restarts we’ll lose the data. That’s fine for a demo, but you might want to check out[one of the examples](https://github.com/apollographql/awesome-launchpad/) about how to read and write to a database.


Now that you have a basic setup with a few types, you can fork the project and keep working inside Launchpad, or click “Download” to edit the code locally.


---


If you’re as excited about advancing GraphQL technology and enabling developers as we are, join us! We’re actively hiring for[open source](https://www.meteor.io/jobs/#5e11e6cf-5303-4c12-a3e7-11e5f8da4be1) ,[backend](https://www.meteor.io/jobs/#b8ec842e-e79a-455e-a665-b312892d946e) , and other roles to work on next-generation API technology.


The Apollo and Meteor team.


---


A screenshot of Hello World for Medium’s preview image!


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
