---
schema_version: "1.0.0"
document_id: "d90475b7ff2aa479a6f3f79126739be7f0d7193b2f66630be4e14de83b2ce63d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/mutation-vs-query-when-to-use-graphql-mutation"
published_at: "2021-02-23T17:12:20+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:bb4556532cc377b740233db5ef75512e801f2b24d539a3e5b43a97079a61b33d"
---

# GraphQL Mutation vs Query – When to use a GraphQL Mutation

This post is part of the Introduction to GraphQL series. You may want to read the previous posts: “[What is GraphQL? GraphQL introduction](https://www.apollographql.com/blog/what-is-graphql-graphql-introduction/) ” and “[What is a GraphQL Query? GraphQL query examples using Apollo Explorer](https://www.apollographql.com/blog/what-is-a-graphql-query-graphql-query-examples-using-apollo-explorer/) “


So far, we’ve covered what GraphQL is: a **query language** and a **server-side runtime** . But more importantly, GraphQL is the modern way to build declarative, efficient and performant APIs that developers love to work with.


In[the last post](https://www.apollographql.com/blog/what-is-a-graphql-query-graphql-query-examples-using-apollo-explorer/) , we learned about GraphQL queries and how to test querying your API with[Apollo Explorer](https://studio.apollographql.com/dev?referrer=blog) .


In this post, we’re going to spend some more time on the *other* GraphQL operation: mutations. We’ll cover what mutations are, how they compare to queries, and when to use them.


## About GraphQL mutations


In GraphQL, there are only two types of operations you can perform: queries and **mutations** .


While we use queries to fetch data, we use **mutations to modify server-side data** .


If queries are the GraphQL equivalent to` GET` calls in REST, then mutations represent the state-changing methods in REST (like` DELETE` ,` PUT` ,` PATCH` , etc).


## Mutation examples


Consider a *pet shop* GraphQL API.


A` query` to fetch all the pets from the app might look like this:


```text
query GetAllPets {
pets {
name
petType
}
}
```


And then a` mutation` that adds a new pet might look a little something like this:


```text
mutation AddNewPet ($name: String!, $petType: PetType) {
addPet(name: $name, petType: $petType) {
id
name
petType
}
}
```


The` AddNewPet`` mutation` expects values for the` name` and` petType` variables. The request data for this` mutation` holds the following shape.


```text
{
"name": "Rover",
"petType": "DOG"
}
```


In the` mutation` response, you could expect to see the following result.


```text
{
"data": {
"addPet": {
"id": 1
"name": "Rover",
"petType": "DOG"
}
}
}
```


## Similarities & differences


### Structure


Mutations look very similar to queries. If you’ll recall, the general structure of a GraphQL operation looks like this.


One *true* difference between a query and a mutation, at least *structurally* , is the **operation type** . We use the word` query` for queries and` mutation` for mutations.


Another important thing to note is that we signal the` mutation` we want to invoke by naming it exactly as it occurs within our server-side GraphQL API.


In the` AddNewPet` example, the name of the mutation is` addPet` .


```text
mutation AddNewPet ($name: String!, $petType: PetType) {
addPet(name: $name, petType: $petType) { # mutation name
name
petType
}
}
```


This means that within our GraphQL API type definitions, we’d have a field on the root Mutation object that looks like the following:


```text
type Mutation {
addPet (name: String!, petType: PetType): AddPetResult!
}
```


### Server-side config (resolvers)


Server-side resolvers for` mutations` involve code that changes state in some way.


Here’s an example of what it might look like to add a new pet using a connection to a` petsAPI`[data source](https://www.apollographql.com/docs/apollo-server/data/data-sources/) .


```text
const resolvers = {
...
Mutation: {
addPet: async (root, args, context) => {
const { name, petType } = args;
const newPet = await context.dataSources.petsAPI.addPet({ name, petType })
return {
id: newPet.id,
name,
petType
}
}
}
};
```


Compare this to a resolver in a` query` where the only responsibility is to fetch the requested resource.


Again, like RESTful requests, there’s nothing stopping us from writing API code here that makes the` mutation` resolver behave more like a` query` . But by convention, we ensure` mutation` operations change state, and` query` operations fetch data.


### Return data


You may have noticed that the` mutation` also returns data; this is another similarity between queries and mutations. However, it’s conventional to only return the relevant new data that was created.


## Conclusion: Determining whether to use query or a mutation


The best way to determine whether you want to write a query or a mutation is to ask yourself if you’re **fetching data** (` query` ) or if **modifying state in the server** (` mutation` ).


More query examples:


- ` GetPetByPetId` ,` GetMyPets` ,` GetPetStores`


More mutation examples:


- ` EditPet` ,` RemovePet` ,` GiveTreat` ,` RemoveAllPets` ,` Login` ,` Logout`


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
