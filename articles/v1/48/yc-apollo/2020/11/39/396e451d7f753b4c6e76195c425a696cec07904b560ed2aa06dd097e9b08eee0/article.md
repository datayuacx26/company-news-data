---
schema_version: "1.0.0"
document_id: "396e451d7f753b4c6e76195c425a696cec07904b560ed2aa06dd097e9b08eee0"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/the-evolution-of-graphql-at-scale"
published_at: "2020-11-12T11:25:01+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:0e3cc08f15e5d75f68462b0d2b35e04ead203362638b74e7a0940de0bb3e3133"
---

# The Evolution of GraphQL at Scale

Most organizations’ adventures with GraphQL start with one team looking for a way to solve one of the most significant frontend development problems in a microservice architecture: how to get all the data your app needs without making a million and one service calls.


For a frontend engineer, that promise of getting all the data they need, in the *exact shape they envision using it* (and with only a single request) is often too hard to resist.


It’s here where a proof of concept comes to fruition. The POC proves the hypothesis that it 1) simplifies the codebase and 2) increases team velocity. Before you know it, GraphQL is in production.


The organization now has its first GraphQL API, but where does it go from there? The answer to that question depends on the organization: how they’ve structured their product engineering teams, the desired level of self-sufficiency of teams, their adherence to the ideals of a distributed architecture, etc.


In this blog post, I will walk you through your options for evolving GraphQL at your organization, from monoliths to BFFs to Federation.


## A Fork in the Road


With a working GraphQL API in place, it won’t be long before other teams want to use it and get all the benefits of using GraphQL.


Things tend to go in one of two directions: teams either start to adopt and add to the existing implementation (monolith) or duplicate what the initial team has done (BFF).


### The Glorious Monolith


Rather than reinvent the wheel, other teams want to build onto the efforts of that first team. Initially, things start great with new teams adding their unique data concerns to the graph and reusing the rest’s pre-existing schema.


Product development times decrease, there’s less duplication of effort, frontend engineers have a consistent API tailored to them to work with, etc. Nirvana, right? Not so quick.


As with any other type of monolithic application, as they grow, the cracks start to appear. The more teams there are making changes to it, the harder it is to release change, and, before you know it, the dream becomes a nightmare and our monolithic GraphQL API becomes a hated beast.


Also, the odds are that the initial implementation only considered the **implementing team’s data needs** and *their product.* This means the initial types and schemas are likely to have been structured around the initial team’s access patterns, which are likely to differ from others’.


Take an eCommerce application as an example. The team building product listings want to access product information by requesting products with a specific SKU, but the team building the basket will want product details for only those product SKUs contained within the basket.


This is a very simplistic example and easily fixable, but, in a monolithic implementation, it’s likely that both teams would write their own resolvers, probably duplicating the fetching logic for the back service that provides the product data.


Suddenly we start getting duplication of effort again, and things start to get messier by the day.


### Backend for Frontend


Rather than building on the initial implementation, another common approach is to follow the backend for frontend (BFF) pattern and introduce a GraphQL server per experience (either an app, channel, team, page, etc.) that only handles the data needs of that experience.


With BFF, we tailor each BFF’s schema around the experience’s access patterns and mirror the frontend application’s needs. It feels like we are getting closer to that nirvana state, right?


In some cases, yes, but consider what we are doing here: we are creating a GraphQL BFF *per experience* with the potential for a lot of duplication amongst BFFs.


In an organization with teams that have a very narrow focus (say where an experience is equivalent to a page on a site), there will be **a lot** of duplicated functionality. Not only that, but infrastructure costs will go up, and the number of attack vectors increases, among other things.


That said, BFFs can be a highly effective way of introducing and using GraphQL within organizations. Depending on the app you are building and your organizational structure, this may indeed be your end game.


Michelle Garrett gives a[great talk](https://youtu.be/qaQEioKovCg) on how Condé Nast makes effective use of BFFs that I’d highly recommend you check out as an example of running the BFF pattern at scale.


But it is not for everyone.


Let’s switch gears and talk about a different approach.


## One Gateway to Rule Them All


So what are the alternatives to a single monolith or numerous BFFs? For a long time, the only option available to teams was schema stitching.


[Schema stitching](https://www.apollographql.com/docs/federation/migrating-from-stitching/) describes the process of creating a single GraphQL schema from multiple underlying GraphQL schemas. It was a technique designed to solve some of the challenges faced by those who went down the monolithic GraphQL route.


It essentially allows you to split up and distribute a monolithic GraphQL schema into several underlying services. Each of these deals with one or more types of the original schema. In front of these, you place a gateway server that will use tooling to introspect and build up a singular schema from the individual parts.


When a request comes into the gateway, it acts as a proxy and distributes the query amongst the underlying services. So, requests for type A will be sent to service A, type B to service B, etc.


But schema stitching isn’t easy.


Using my simple example of Product and Basket types, what you want is the following schema where the` Basket` type contains an items field that returns an array of` Product` .


```text
# Product service
type Product {
id: ID!
name: String
description: String
}
​
# Basket service
type Basket {
id: ID!
items: [BasketItem]
}
​
type BasketItem {
id: ID!
quantity: Int
product: Product
}
​
# Query to fetch the basket
query {
getBasket(id: 12345) {
items {
id
quantity
product {
name
description
}
}
}
}
```


​But that won’t work. The` Basket` service has no knowledge of` Products` and probably only knows about the ID of the products in the basket.


Exposing the schema gets messy. You’d either have to:


- Use some glue code in the gateway. It will need to understand the relationship between the two services and do a behind-the-scenes fetch of all Products that match the IDs held in BasketItems.
- Or instead, make the` Basket` service call the` Product` service to get the` Product` information.


Of course, this would be invisible to the consumer of the stitched schema, but… don’t you feel just a little bit dirty having read the past few paragraphs? As a one-off, it is fine, but multiply this by however many interconnections you have between your types and… eek!


Schema stitching is full of these little solvable problems, but it is *death by a thousand cuts* . Throw into the mix questions on updating the stitched schema when one of the underlying schemas change or what happens when one of the underlying services is down at compose time. I guarantee you will quickly be banging your head against your desk.


## Federation to the Rescue


So what to do? A monolith is bad because monoliths are bad, BFFs are great, but the amount of duplication can get ridiculous at scale, and schema stitching is… a headache and often very fragile.


In an attempt to solve this dilemma, Apollo[introduced Apollo Federation in May 2019](https://www.youtube.com/watch?v=LKQKn1oFXJU&ab_channel=ApolloGraphQL) to give the ability to build a single, cohesive schema from multiple federated services.


Rather than splitting the schema up by type (which you end up doing with schema stitching), federation allows you to separate it by *concern* .​


I won’t go into too much detail here (I’ll save that for a later blog post), but it does this by allowing multiple federated services to extend a single type, with the gateway composing the type fragments together again.


The gateway’s inbuilt query planner then splits incoming queries into smaller queries (based on its understanding of which service resolves which part of the query) and sends them on to the federated services.


Refactoring our example from earlier:


```text
# Basket service
type Basket @key(fields: "id") {
id: ID!
items: [BasketItem]
}
​
type BasketItem {
id: ID
quantity: Int
}
​
# Product service
type Product @key(fields: "id") {
id: ID!
name: String
description: String
}
​
extend type BasketItem @key(fields: "id") {
id: ID! @external
product: Product
}
​
# Query to fetch the basket
query {
getBasket(id: 12345) {
items {
id
quantity
product {
name
description
}
}
}
}
```


The Basket service defines a` Basket` type with an items field of` BasketItem` that defines the quantity of a given item. But where are the products?


Well, the Product service now not only defines the` Product` type, but it also extends the` BasketItem` defined in the Basket service. It also adds a product field that resolves the related product for that given basket item.


How does it achieve this? At a high level, the` @key` directives after the various types define the externally referenceable fields used to link the types together.


One of the easiest ways I’ve found to reason about this is to think of the federated data graph as a relational database, with the keys being analogous with foreign keys in the database. Each federated service then needs to provide a standard resolver to handle queries referencing their foreign key(s).


In the above simple example, each federated service now only has to be *concerned* about one high-level data type. The Basket service handles all requests for` Basket` data and the Product service handles all requests for` Product` data, even when some of those requests are *actually* made to resolve the` Product` data for the items in the` Basket` .


This fits in nicely with the modern trend for microservice architectures with colocated data sources. There are no blurred lines between services or, say, the Basket service storing a subset of product data for items in the` Basket` .


Is it perfect? No, and running a large federated data graph comes with a whole host of distributed system challenges, but it is a giant step forward for GraphQL at scale.


Thanks for reading; if you’d like to talk GraphQL or tech in general, give me a follow on[Twitter](https://twitter.com/andyroberts_io) and send me a DM. As a bonus, you’ll also get to see any future blogs I write!


Written by


Andy Roberts


[Read more by Andy Roberts](https://www.apollographql.com/blog/author/andyroberts)
