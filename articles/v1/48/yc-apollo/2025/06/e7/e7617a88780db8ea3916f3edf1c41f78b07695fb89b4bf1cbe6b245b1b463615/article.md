---
schema_version: "1.0.0"
document_id: "e7617a88780db8ea3916f3edf1c41f78b07695fb89b4bf1cbe6b245b1b463615"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/handling-the-n1-problem-declaratively-with-apollo-connectors"
published_at: "2025-06-24T08:53:29+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:25a6daf83ce58267c68b302676a4fc6d8ea4dd5b8e3b49f33b81d1c1504d0833"
---

# Handling the N+1 Problem Declaratively with Apollo Connectors

With the release of Apollo Router 2.3 and Federation 2.11, Apollo Connectors can now avoid the dreaded N+1 query problem! You can make your API faster and more efficient by changing your Connectors to use the` $batch` variable instead of the` $this` variable.


```text
type User
@connect(
http: { GET: "https://my.api/users?ids={$batch.id->joinNotNull(',')}" }
selection: "id name profilePic: profile_pic"
)
{
id: ID!
name: String!
profilePic: String!
}
```


If you want to learn more about the N+1 problem and how Connectors batching works internally, please read on!


---


The N+1 query problem is a classic performance killer that sneaks up on developers. Picture fetching a list of blog posts, then looping through each post to grab its author details with a separate API call. Your API client makes this feel natural in code, but your server is getting hammered with dozens or hundreds of unnecessary round trips. The result? Page load times that frustrate users and servers that buckle under load.


Like any API solution, Apollo Connectors are susceptible to the N+1 query problem. Here’s a schema that supplied posts and author details using Connectors.


```text
type Query {
posts: [Post]
@connect(
http: { GET: "https://my.api/posts" }
selection: "id title body author: { id: author_id }"
)
}


type Post {
id: ID!
title: String!
body: String!
author: User!
}


type User
@connect(
http: { GET: "https://my.api/users/{$this.id}" }
selection: "id name profilePic: profile_pic"
)
{
id: ID!
name: String!
profilePic: String!
}
```


You can see the N+1 requests in the Connectors Debugger in Apollo Sandbox.


The request table in the Connectors Debugger showing one request for a list of posts and requests for each related author.


## Whither DataLoaders?


The tried-and-true solution to the N+1 query problem for GraphQL is the DataLoader. DataLoaders are libraries for various programming languages, but Connectors is a declarative system in GraphQL, so we don’t have access to those libraries. Let’s look at what DataLoaders do and see what functionality we can recreate in a declarative system.


Under the hood, a DataLoader is three things:


1. A way to create a single request from a list of parameters.
2. A scheduler that determines the appropriate time to make the request.
3. A[map](https://en.wikipedia.org/wiki/Associative_array) (or associative array) where the keys are request parameters and the values are the requested entities.


The fact that DataLoaders use a map is a crucial detail. By using a map, we can enforce two important requirements:


1. We request each entity only once: the map automatically deduplicates request parameters.
2. We can correctly associate each entity with the request parameters, ensuring data integrity in our joins across endpoints.


Before we can figure out how to recreate the functionality of a DataLoader declaratively, let’s peek under the hood of Apollo Connectors.


## Connectors and Query Plans


When Apollo Router receives a client request like this:


```text
query PostsAndAuthors {
posts {
id
title
body
author {
id
name
profilePic
}
}
}
```


it first generates a query plan. The steps in a plan that do work, called “fetches”, match up with the Connectors in our example.


```text
Sequence


# Connector on Query.posts
Fetch GET: https://my.api/posts
=> { posts { id title body author { id } } }


# Connector on User
Fetch GET: https://my.api/users/{$this.id}
=> ... on Author { name profilePic }


Flatten posts.[].author
```


The first “fetch” is a single request to our /posts endpoint that returns a list of posts with foreign keys for authors:


```text
[
{
"id": "post4",
"title": "Recent updates",
"body": "Lorem ipsum dolar sit amet",
"author_id": "user2"
},
{
"id": "post3",
"title": "First post after a break",
"body": "Lorem ipsum dolar sit amet",
"author_id": "user3"
},
{
"id": "post2",
"title": "Second post!",
"body": "Lorem ipsum dolar sit amet",
"author_id": "user1"
},
{
"id": "post1",
"title": "First post!",
"body": "Lorem ipsum dolar sit amet",
"author_id": "user3"
}
]
```


From the` @connect(selection:)` argument, the query planner knows exactly which fields each Connector provides.


```text
# -- snip --
posts: [Post]
@connect(
http: { GET: "https://my.api/posts" }
selection: "id title body author: { id: author_id }"
)
# -- snip --
```


It sees that the` /posts` endpoint provides the` Author.id` field, but not` name` or` profilePic` , so it knows it will need to make more requests for that data.


Fortunately, the Connector on the` User` type can provide` name` and` profilePic` . Even better, it depends on the` User.id` field to make these requests via the` {$this.id}` expression.


```text
# -- snip --
type User
@connect(
http: { GET: "https://my.api/users/{$this.id}" }
selection: "id name profilePic: profile_pic"
)
# -- snip --
```


The expression` $this.id` denotes a *dependency* of this Connector. In order to execute this request, the query planner must first fetch the` id` of the` User` type from another Connector or subgraph.


Between the two fetches in the query plan, the planner collects the` author { id }` values from the first Connector into a list that is passes to the second Connector. We call this a list of “entity references”:


```text
[
{ id: "user2" },
{ id: "user3" },
{ id: "user1" },
]
```


Here we see the query planner already satisfying one of the DataLoader requirements. While the` /posts` response had four posts, with` user3` appearing twice, the planner already deduplicated the entity references.


To complete the rest of the picture, let’s look at how Apollo Connectors uses fetches from query plans to call our APIs.


## How Fetches Turn into Requests


When a fetch in the query plan contains a list of entity references and a Connector uses the` $this` variable, the router knows to turn each item in the list into a separate request.


```text
[
{ method: "GET", url: "https://my.api/users/user2" },
{ method: "GET", url: "https://my.api/users/user3" },
{ method: "GET", url: "https://my.api/users/user1" }
]
```


It then fires off the requests in parallel and returns a list of mapped responses in the same order.


```text
[
{ "id": "user2", "name": "Bobby", "profilePic": "https://profile.pic/abcd.jpg" },
{ "id": "user3", "name": "Carol", "profilePic": "https://profile.pic/efgh.jpg" },
{ "id": "user1", "name": "Alice", "profilePic": "https://profile.pic/ihjl.jpg" }
]
```


Order matters! The query planner expects the entities in the same order as the entity references. If the result was out of order, we could end up merging Alice’s name and profile pic with posts written by Carol.


## Batch Requests


To reduce the number of requests from four (one to` /posts` plus three to` /users/:id` ) to two, we need an endpoint that accepts a batch of user IDs and returns a matching batch of users.


If you don’t have an endpoint for that, you’re stuck with N+1. This might be fine! Maybe you’re migrating your client applications from REST to GraphQL. It’s probably faster to have Apollo Router make N+1 queries to your API within the same data center than making those same requests from the client over the Internet.


But maybe you do have a batch endpoint. These take many forms:


- Comma separated values like` ?ids=1,2,3`
- Repeated query parameters like` ?id=1&id=2&id=3`
- JSON POST bodies like` { “filter”: { “id”: { “$in”: \[1,2,3\] } } }`


Regardless of how you structure your batch requests, the key to batching with Apollo Connectors is switching from the` $this` variable to the` $batch` variable.


```text
type User
@connect(
http: { GET: "https://my.api/users?ids={$batch.id->joinNotNull(',')}" }
selection: "id name profilePic: profile_pic"
)
```


When using` $batch` , the router makes a single request of the entire list of entity references instead of one request per entity reference. You can use the entire array in the Connectors mapping language to construct different kinds of requests:


- ` ?ids={$batch.id->joinNotNull(',')}` becomes` ?ids=user2,user3,user1`
- ` http: { queryParams: "$batch { id }" }` becomes` ?id=user2&id=user3&id=user1`
- ` http: { body: "filter: { id: { '$in': $batch.id } }"` becomes` {"filter": { "id": { "$in": \["user2","user3","user1"\] } } }`


Once you’ve updated your Connector to make a batch request, you’ll quickly see the results in Apollo Sandbox!


The request table in the Connectors Debugger showing one request for a list of posts and a single request for all related authors.


A screenshot of the query plan with a` \[BATCH\]` label for the related authors.


### Completing the DataLoader Pattern


Revisiting the elements of a DataLoader:


1. We now have a way to create a single request from a list of parameters with` $batch` expressions.
2. The query planner handles scheduling requests with “fetch” steps in the query plan.
3. The query planner handles half the responsibilities of the DataLoader’s internal map by deduplicating entity references automatically.


The last requirement we need to complete the picture is making sure we’re associating the entities from the responses with the entity references from the query planner. This is especially important because we can make no guarantees about the order of entities in a JSON API. Fortunately, Connectors handle this for us by using a map internally (same as a DataLoader!)


Here’s some pseudocode describing how Connectors ensure data integrity using a map:


```text
# this is the order expected by the planner
entity_references = [{ id: "user2" }, { id: "user3" }, { id: "user1" }]


# this is the list from the API (notice the order!)
response = [
{ id: "user1", name: "Alice" },
{ id: "user2", name: "Bobby" },
{ id: "user3", name: "Carol" }
]


# store each response object by the key derived from "$batch.id"
let map = {}
for each entity in response:
let key = get_key("$batch.id", entity) # returns `{"id":"user1"}`
map[key] = entity


# use the list from the planner to put the response objects in the correct order
let ordered = []
for each reference in entity_references:
ordered.push(map[reference] or null)


return ordered
```


This pattern not only works like DataLoaders, but it also lets us support complex keys like` $batch { upc store { locationId } }` with no extra work on your end.


With that, we have the declarative equivalent of a DataLoader in Connectors!


### Embracing Declarative Systems


We weren’t sure how we’d handle the N+1 query problem when we started designing Connectors. We waited to get information from users on the kinds of batch API endpoints they had or were willing to build to make their Connectors better. Armed with that feedback and years of experience with various implementations of DataLoader libraries, the design for batching Connectors fell into place surprisingly quick. Things get easier when standing on the shoulders of giants!


[Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors) and batching are ready to use in production today! You can get started for free, because we want every team to experience how much easier API development can be with Connectors.


Whether you’re building a new API or maintaining an existing one:


- Join our[community forum](https://community.apollographql.com/c/connectors/29) to share your experiences
- Take the[Intro to Connectors Odyssey course](https://www.apollographql.com/tutorials/connectors-intro-rest)
- Check out our[documentation for detailed guides](https://go.apollo.dev/connectors)
- Let us know what features you need next via[Apollo Support](https://support.apollographql.com/)


We’re so thrilled to be launching this update with features that provide even more flexibility and possibilities for Connectors. We’re so excited for you to get hands on with these updates and we can’t wait to see what you build!


Happy building!


Written by


Lenny Burdette


[Read more by Lenny Burdette](https://www.apollographql.com/blog/author/lenny)
