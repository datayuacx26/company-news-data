---
schema_version: "1.0.0"
document_id: "f9d7a6f575580553bf7cd6e65851593904b183d892c99a42d3940d242e0704d3"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/using-nullability-in-graphql"
published_at: "2018-06-05T18:17:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:8fe1b1eed3948300f64b27dfe0f2c233f55abc8909758d73c517568b20c64063"
---

# Using nullability in GraphQL

Like the hole in a donut, what isn’t there can be just as important as what is. Photo credit Tommy Chheng: https://www.flickr.com/photos/tommychheng/3226662528/


When you’re working with a REST API, you don’t always know exactly what fields you’re going to get back when you call an endpoint. GraphQL improves on that by having a complete schema of your available data, and giving you back exactly the fields you asked for. But what if a field is simply not available, for example if the record you’re retrieving never got that value assigned, or there was an error fetching a piece of data?


Even GraphQL can’t get you a piece of data that doesn’t exist, but it has the next best thing: a built in concept of **nullability** . A field can either be nullable or non-null, and this tells you whether or not you could receive a null value when you ask for it. By default, every field in GraphQL is nullable, and you can opt in to mark it **non-null.**


In this article, we’ll go over two things:


1. How non-null in GraphQL works, and what it does
2. When and why you should use it in your schema


## How non-null works


Let’s look at an example. In the following schema, we have a Restaurant type with a location field that refers to a Location type:


```text
type Restaurant {
name: String!
rating: Int!
location: Location!
}


type Location {
address: String!
}


type LatLng {
lat: Int!
lng: Int!
}


type Query {
restaurants(query: String): [Restaurant!]
}
```


The fields whose types have an exclamation mark,` !` , next to them are **non-null fields** . These are fields that won’t return a null value when you query them. So for the following query:


```text
const query = gql`{
restaurants {
name
location {
address
}
}
}`;
```


You can get the following results:


```text
// no restaurants exist
{ restaurants: null }


// all data present
{
restaurants: [{
name: "McDonald's",
location: {
address: "55 Example Street, San Francisco, CA"
}
}]
}
```


But you can’t get the following results:


```text
// restaurant name or location can't be null
// this would be an invalid result
{
restaurants: [{
name: null,
location: null
}]
}


// items in the restaurant array can't be null
{
restaurants: [null]
}
```


This means that in addition to guaranteeing the shape of the result, GraphQL can provide a guarantee for which fields must be present when queried.


### Why are non-null fields useful?


The usefulness of such a guarantee becomes apparent when you think about the frontend code that might consume the result of such a query. Imagine the following code in JavaScript:


```text
// Using the same query as above
const result = await client.query({ query: query });


result.data.restaurants.forEach(restaurant => {
console.log(restaurant.name);
console.log(restaurant.location.address);
});
```


At first, this might seem like a reasonable piece of code to write based on the query, since you’re expecting to get back a list of restaurants that each have a name to be displayed. But, once we look more closely at our schema, it turns out that there are two potential errors hidden in this code:


1. ` data.restaurants` could be null
2. ` restaurant` could be null (since, by default, GraphQL allows null items in lists)


So a more correct version will look like:


```text
// Using the same query as above
const result = await client.query({ query: query });


if (result.data.restaurants) {
result.data.restaurants.forEach(restaurant => {
if (restaurant) {
console.log(restaurant.name);
console.log(restaurant.location.address);
}
});
}
```


But the great thing is that we don’t have to worry about` restaurant.name` being null, because in our schema we’ve declared that if we have a restaurant object at all, it’s guaranteed to have a value for the` name` field. That’s the value of declaring a field as non-null in GraphQL.


### What happens if you try to return null for a non-null field?


So we can declare some fields to be non-null in our GraphQL schema, but that doesn’t guarantee we won’t have some code in our resolvers that still returns null for that field. For example:


```text
const resolvers = {
Query: {
restaurants: (parent) => {
// Let's say this is what our database happens
// to return
return [{
name: "All Star Cafe",
rating: 5,
location: null // oh no!
}]
}
}
};
```


In this case, let’s say that we’re using a schemaless database like MongoDB and someone forgot to store a location field for one of our restaurants. So when we return that query result through GraphQL, we’ll be returning a` null` value for a non-null field. What happens?


**When you try to return a null value in a resolver for a non-null field, the null result bubbles up to the nearest nullable parent.**


So in this case, we’d get` null` for the entire array item, plus an error notifying us about the null:


```text
{
"data": {
"restaurants": [ null ]
},
"errors": [{
"message": "Cannot return null for non-nullable field Restaurant.location."
}]
}
```


This ensures that our frontend code never has to check for null when accessing the` location` field, even if it’s at the expense of not getting any part of the restaurant object. So there’s a tradeoff here:


**When using non-null, you need to decide if you’d rather have a partial result, or no result at all.**


This also brings up another common question about nullability: How does it work with lists?


### Nullability and lists


In GraphQL, you don’t define a new type when you are trying to return a list of items from a field; you simply apply a list *modifier* to that type, like so:


```text
type ObjectType {
singleRestaurant: Restaurant
multipleRestaurants: [Restaurant]
}
```


Non-null is also a modifier, and it turns out you can apply non-null and list modifiers in an arbitrarily nested way, especially since lists can be nested:


```text
type ObjectType {
first: Restaurant
second: [Restaurant]
third: [Restaurant!]
fourth: [Restaurant!]!
fifth: [[Restaurant!]]!
# ... etc
}
```


So what does it mean to have the non-null inside vs. outside of a list? Well, it decides whether the non-null applies to the *list item* vs the list itself.


For example, you can have a List of Non-Null Strings:


```text
drinkSizes: [String!]
```


This means that the *list itself* can be null, but it can’t have any null members. For example, in JSON:


```text
drinkSizes: null // valid
drinkSizes: [] // valid
drinkSizes: ["small", "large"] // valid
drinkSizes: ["small", null, "large"] // error
```


Now, let’s say we defined a Non-Null List of Strings:


```text
drinkSizes: [String]!
```


This means that the list itself cannot be null, but it can contain null values:


```text
drinkSizes: null // error
drinkSizes: [] // valid
drinkSizes: ["small", "large"] // valid
drinkSizes: ["small", null, "large"] // valid
```


Finally, we can combine the two:


```text
drinkSizes: [String!]!
```


This is the most restrictive:


```text
drinkSizes: null // error
drinkSizes: [] // valid
drinkSizes: ["small", "large"] // valid
drinkSizes: ["small", null, "large"] // error
```


One interesting conclusion here is that there’s no way to specify that the list can’t be empty — an empty list` \[\]` is always valid, regardless of whether the list or items are non-null.


### Non-null in arguments and input objects


So far, we’ve only talked about using non-null in the context of GraphQL results. But you can also use it to validate the inputs to your fields:


```text
type Query {
restaurants(query: String!): [Restaurant]
}
```


In this case, we’ve added the` !` to the` query: String!` argument definition. This means that we’ll get an error if we don’t pass that argument. Similarly, we can use non-null on any field of an input object type to ensure that field is passed whenever the field is called:


```text
input RestaurantFilters {
name: String
location: String!
distance: Int!
}


type Query {
restaurants(filters: RestaurantFilters!): [Restaurant]
}
```


This means that whenever we call the field, we need to pass it a` RestaurantFilters` object that must have at least` location` and` distance` fields.


**Important note:** Non-null in arguments and input object types has serious implications for backwards compatibility. If you change your API by adding a non-null argument to a previously existing field, existing frontend code that calls that field without passing that argument *will no longer work.* So watch out when adding new arguments or fields to your inputs.


## When to use non-null


Now that we’ve learned about how to actually use non-null fields and arguments and what they do, what’s the right way to use them?


One point that almost everyone seems to agree on is that it’s important to think carefully about how you use nullability in GraphQL. For example,[Caleb Meredith](https://medium.com/u/d7533f31b8ab?source=post_page-----2254f84c4ed7----------------------) mentions that “Non-null fields make it hard to evolve your schema” in his article[“When To Use GraphQL Non-Null Fields”](https://medium.com/@calebmer/when-to-use-graphql-non-null-fields-4059337f6fc8) .


### Non-null and backwards compatibility


One of the first things to think about with your GraphQL schema is how to evolve it over time.[Marc-André Giroux](https://medium.com/u/ecb22831abfb?source=post_page-----2254f84c4ed7----------------------) recently wrote[a great article about evolving schemas](https://dev-blog.apollodata.com/graphql-schema-design-building-evolvable-schemas-1501f3c59ed5) . Here’s how you would apply those concepts to nullability:


1. For input arguments and fields, adding non-null is a breaking change.
2. For output fields, removing non-null from a field is a breaking change.


The first case is easy to understand: If an API consumer isn’t passing in an optional argument, then making that argument required means that request is no longer valid. So if we started with this schema with optional pagination:


```text
restaurants(offset: Int, limit: Int): [Restaurant]
```


And later moved to this schema, which requires a limit to be passed in:


```text
restaurants(offset: Int, limit: Int!): [Restaurant]
```


Then a query like the following would stop working:


```text
query { restaurants { name } }
```


This might be OK if you have a single web app, and it’s deployed at the same time as your backend, but definitely unacceptable if you’re using this API from a native app that the user has to update manually, or if it’s a public API.


The second case is a little harder to understand — why does making a non-null output field nullable potentially break clients? Well, let’s look into how a client might consume that API by revisiting an example above. Let’s start with this schema:


```text
# Schema version 1
type Restaurant {
name: String!
rating: Int!
location: Location!
}


type Location {
address: String!
}


type LatLng {
lat: Int!
lng: Int!
}


type Query {
restaurants(query: String): [Restaurant]
}
```


And we could have the following query:


```text
const query = gql`{
restaurants {
name
location {
address
}
}
}`;
```


Then, let’s say we consume the result with the following code:


```text
// Using the same query as above
const result = await client.query({ query: query });


if (result.data.restaurants) {
result.data.restaurants.forEach(restaurant => {
if (restaurant) {
console.log(restaurant.name);
console.log(restaurant.location.address);
}
});
}
```


Now, let’s say we iterate on the schema, and make the following change:


```text
# Schema version 2
type Restaurant {
name: String!
rating: Int!
-   location: Location!
+   location: Location     # We made this field nullable
}
```


The code we wrote above might now break, since` restaurant.location.address` could now give us something like` Type Error: restaurant.location is undefined` .


So there’s a general thought here: when we’re changing our API we don’t just have to worry about whether any of our queries break; we need to worry about whether the code consuming those query results could break.


Of course, we might be able to catch an error like the above with static type checking for our frontend code.


### Non-null with GraphQL query type generation


In addition to using non-null fields to simplify frontend code to reduce the amount of null cases we need to check, we can set up an even safer system by combining that with code generation.


Most statically typed languages offer a way to distinguish nullable and non-null fields in objects, so for example in TypeScript, the two schemas above would generate different code for the query:


```text
// Schema version 1, non-null location
export type GetRestaurantsQuery = {
restaurants: Array<{
__typename: "Restaurant",
name: string,
location: {
__typename: "Location",
address: string
}
} | null> | null
};


// Schema version 2, nullable location
export type GetRestaurantsQuery = {
restaurants: Array<{
__typename: "Restaurant",
name: string,
location: {
__typename: "Location",
address: string
} | null
} | null> | null
};
```


Note that the second one has the` | null` at the end for the` location` field.


Side note: I used this[neat online tool at transform.now.sh](https://transform.now.sh/graphql-to-typescript) that runs[apollo-codegen](https://github.com/apollographql/apollo-codegen) in the browser to generate the snippets above.


I would say static type code generation is one of the main benefits of taking advantage of the non-null feature in GraphQL, as long as you responsibly consider the backwards compatibility questions it brings.


### Recommendations for nullability


Here are some of the best practices we recommend concerning nullability in your GraphQL schemas.


**Use non-null** in the following cases:


1. In field arguments, where the field doesn’t make any sense if that argument is not passed. For example, a` getRestaurantById(id: ID!)` field should never be called without the` id` argument, so it’s helpful to mark that non-null.
2. On object fields that are guaranteed to be there, to simplify frontend code. For example, every single restaurant in our API had better have a` name` .


Here are some cases where we recommend **avoiding non-null** :


1. In any field arguments or input types that are added to a field. Given that this is a backwards-incompatible change, it’s clearer to simply add an entirely new field, since the new required argument probably represents a significant change in functionality.
2. In object type fields where the data is fetched from a separate data source. I’d suggest making any fields that have resolvers that fetch asynchronous data nullable, so that it’s easier to deal with errors that result from a service or database being unreachable. For example, if getting the` location` of a restaurant required hitting a separate locations database, then it should be nullable in case that lookup fails in some way.


Finally, I’d suggest thinking about nullability in lists. For example, if a list you’re returning just represents a set of items, it might not make sense for there to be any null items; therefore, I almost always recommend setting the items inside the list to be non-null:


```text
fieldName: [String!]
```


One case where using non-null for the items inside a list doesn’t make sense is when the list represents an ordered array of items that actually has gaps.


## Conclusion


Nullability is a somewhat advanced topic in GraphQL, and it’s not always clear on first look how it works or when you should use it. I think choosing nullable as the default for GraphQL fields was a good decision, and that making too much of your GraphQL schema non-null can be a risky proposition if you don’t think ahead.


Hopefully, after reading this article you have a clearer understanding of null and non-null in GraphQL. If you have any of your own ideas or experiences you’d like to share, please write a response or hit me up on Twitter!


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
