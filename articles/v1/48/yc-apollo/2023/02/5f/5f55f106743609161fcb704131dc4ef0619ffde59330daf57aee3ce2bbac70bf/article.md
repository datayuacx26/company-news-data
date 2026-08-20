---
schema_version: "1.0.0"
document_id: "5f55f106743609161fcb704131dc4ef0619ffde59330daf57aee3ce2bbac70bf"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-federation-and-graphos-add-support-for-entity-interfaces-to-streamline-collaboration"
published_at: "2023-02-15T08:22:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:fa1d00b762e5e252a1146baecace32943f8d57321041d3188eeb80bbe89dff1a"
---

# Apollo Federation and GraphOS add support for entity interfaces to streamline collaboration

[Apollo Federation](https://apollographql.com/docs/federation) is widely adopted as the standard specification for building distributed GraphQL APIs, called[supergraphs](https://www.apollographql.com/blog/announcement/backend/the-supergraph-a-new-way-to-think-about-graphql/) , that run on top of one or more underlying subgraphs. Unlike a monolithic graph, a supergraph maintains clear[separation of concerns](https://www.apollographql.com/docs/federation/#separation-of-concerns) — each subgraph can be owned independently by separate developers or teams — allowing it to evolve rapidly as new use cases arrive.


Now generally available and fully supported by Apollo GraphOS, the Apollo Federation 2.3 spec further improves separation of concerns in a supergraph by adding support for *entity interfaces* and the new` @interfaceObject` directive. Entity interfaces (interfaces with a` @key` ) defined in one subgraph can now be abstracted as an object type in other subgraphs using` @interfaceObject` . Those subgraphs can then add and resolve fields directly on that interface in the supergraph.


To get started with entity interfaces and` @interfaceObject` , please see the[Apollo Federation documentation](https://www.apollographql.com/docs/federation/federated-types/interfaces) or read on. In the post below, we’ll discuss the challenges of reusing interfaces across multiple subgraphs, and walk through an example of using` @key` on interface definitions and` @interfaceObject` to solve these challenges.


## The challenges of reusing interfaces across subgraphs


In GraphQL,[interfaces](https://www.apollographql.com/docs/apollo-server/schema/unions-interfaces/#interface-type) specify a set of fields that all implementing object types must include. They are particularly helpful for the abstraction they provide — they allow you to return a set of objects of different types, without needing to know what those concrete types are. For example, if object types` Book` and` Movie` implement the` Product` interface, you can have the following query:


```text
type Query {
products: [Product!]!
}
```


This query will return all objects that implement the` Product` interface, making the graph more flexible over time. If new objects are added that implement the` Product` interface, we don’t need to change our query — all new objects will be returned.


Since interfaces were originally designed for a monolithic graph architecture, there were a few challenges in reusing interfaces across federated subgraphs prior to this release. Specifically, if an interface and all of its concrete implementations were defined in` subgraphA` ,` subgraphB` couldn’t easily add fields to the interface or resolve those fields without duplicating all of its concrete implementations. This can often negate the abstraction benefits of interfaces, since both subgraphs need to know everything about the interface.


Let’s consider an example:


### Example: adding a field to an interface in another subgraph *without*` @interfaceObject`


Let’s say we have a` products` subgraph that defines a` Product` interface. We then have two entity object types (` Book` and` Movie` ) that implement that interface:


```text
# products subgraph
type Query {
products: [Product!]!
}


interface Product {
id: ID!
description: String
price: Float
}


# Book entity with key
type Book implements Product @key(fields: "id") {
id: ID!
description: String
price: Float
pages: Int
}


# Movie entity with key
type Movie implements Product @key(fields: "id") {
id: ID!
description: String
price: Float
duration: Int
}
```


We also have another subgraph,` reviews` , which enables users to leave reviews on any product. In this example, we want to get all the best-reviewed products of any product type – this is achieved through the query` bestRatedProducts` that returns all objects implementing the` Product` interface.


Without` @interfaceObject` , our` reviews` subgraph would need to look like this:


```text
# reviews subgraph
type Query {
bestRatedProducts(limit: Int): [Product!]!
}


interface Product {
id: ID!
reviews: [Review!]!
}


type Review {
author: String
text: String
rating: Int
}


# Book entity with key
type Book implements Product @key(fields: "id") {
id: ID!
reviews: [Review!]!
}


# Movie entity with key
type Movie implements Product @key(fields: "id") {
id: ID!
reviews: [Review!]!
}
```


There are a few problems with the workflow presented above:


- **Lots of repetition** — the` reviews` subgraph needs to duplicate all the entity types that implement the` Product` interface (` Book` and` Movie` ).
- **Leaky abstraction** — we can’t truly abstract the` Products` interface across multiple subgraphs. If a subgraph defines an interface, it also needs to define all of its concrete implementations – when ideally, we shouldn’t have to know what concrete objects are implementing` Product` . However, we can’t remove the` Book` or` Movie` definitions in our` reviews` subgraph without making our subgraph an invalid GraphQL service.
- **Extensive coordination** — any time we want to add a new type that implements the` Product` interface, we need to add it to both the` products` subgraph and the` reviews` subgraph. If these subgraphs are owned by different teams, we need to make sure those teams are in lockstep coordination to make the change. This issue is compounded for any other subgraphs that need to contribute to the interface.


## Distributing interface definitions with entity interfaces and` @interfaceObject`


Ideally, our goal is to allow the` reviews` subgraph to do the following *without* duplicating each type that implements the` Product` interface:


- Add a` reviews` field to the` Product` interface defined in the` products` subgraph
- Add the` reviews` field to each type that implements the` Product` interface (including any future implementation types)
- Resolve the` reviews` field directly on the` Product` interface for all implementation types


In order to accomplish this, Federation 2.3 introduces two things:


1. Support for creating *entity interfaces* by adding the` @key` directive to interface definitions.
2. A new directive,` @interfaceObject` , which allows you to abstract an entity interface in isolation by declaring it locally as an object type. This enables you to contribute fields to the interface without needing to redefine all of the interface’s concrete implementation types.


Let’s apply these to our example.


### Create an entity interface with` @key`


Before we can use` @interfaceObject` in our` reviews` subgraph, we need to add a` @key` to our` Product` interface definition in the` products` subgraph. This enables the` products` subgraph to resolve the implementation type of the abstracted` Product` objects returned by the` reviews` subgraph.


If we try to use` @interfaceObject` to abstract an interface that doesn’t have a` @key` , we’ll get an error during composition.


Here’s what our` products` subgraph schema looks like with` Product` defined as an entity interface:


```text
# products subgraph
type Query {
products: [Product!]!
}


# Entity interface definition with key
interface Product @key(fields: "id") {
id: ID!
description: String
price: Float
}


# Book entity with key
type Book implements Product @key(fields: "id") {
id: ID!
description: String
price: Float
pages: Int
}


# Movie entity with key
type Movie implements Product @key(fields: "id") {
id: ID!
description: String
price: Float
duration: Int
}
```


### Abstract an entity interface locally with` @interfaceObject`


Now that we’ve made` Product` an entity interface, we can use the new` @interfaceObject` to declare` Product` locally as an object type in our` reviews` subgraph. Put simply,` @interfaceObject` essentially lets us “convert” an entity interface to an object type in our subgraph *only,* so that we can avoid duplicating all of its concrete implementations while still keeping our schema valid.


To illustrate, here’s what our` reviews` subgraph looks like when using` @interfaceObject` to abstract the` Product` entity interface and add a` reviews` field to it:


```text
# reviews subgraph


# Get the best-reviewed products regardless of type
type Query {
bestRatedProducts(limit: Int): [Product!]!
}


# Entity interface is declared as an object type locally
type Product @key(fields: "id") @interfaceObject {
id: ID!
# reviews field is automatically added to all objects that
# implement the Product interface
reviews: [Review!]!
}


type Review {
author: String
text: String
rating: Int
}
```


In the` reviews` subgraph, our` Product` entity interface is now an object type, and we’ve added the` reviews` field to the interface. However, since we’ve decorated` Product` with` @interfaceObject` , we’re telling composition (and any human readers) that “this object type is actually an interface type in the supergraph schema, but I’m abstracting it locally.”


During composition, the` reviews` field is automatically added to the interface definition and to the definition of each entity *type* that implements the` Product` entity *interface* —` Book` ,` Movie` , and any others added in the future. This allows us to treat the` Product` interface as an opaque abstraction so that we can evolve the graph over time without intensive cross-team coordination.


### Query the entity interface


Now that we’ve defined our` Product` entity interface and added the` reviews` field to it with` @interfaceObject` , let’s consider how our supergraph would handle the following query:


```text
query {
bestRatedProducts(limit: 10) {
__typename
price
}
}
```


With` @interfaceObject` , we can now query the` reviews` subgraph to get the ten best-rated products and their` id` . However, we also need to know if each` id` is a` Book` or` Movie` type. Our` reviews` subgraph doesn’t know the details of each implementation type, but since we have` @key` defined on the` Product` interface, the` products` subgraph is able to resolve the` __typename` for each` id` returned by the` reviews` subgraph.


## Getting started with entity interfaces


### If you are new to Apollo Federation…


Read the[Apollo Federation documentation](https://apollographql.com/docs/federation) to get started. Or, if you want a more structured tutorial for Federation, check out our hands-on[Odyssey Course on Federation](https://www.apollographql.com/tutorials/voyage-part1) .


### If you’re experienced with Federation and have a supergraph…


Great news! Apollo GraphOS fully supports the Federation 2.3 spec including` @interfaceObject` and` @key` on interfaces. This includes:


- GraphOS cloud routing and self-hosted Apollo Router/Gateway support
- Composition in GraphOS Studio


To learn more about requirements and rules for using entity interfaces in Federation, please see[this section](https://www.apollographql.com/docs/federation/federated-types/interfaces/#requirements) in our documentation.


* **Note** : If you’re using a self-hosted router or gateway, you’ll also need to ensure you are using a version that supports the Federation 2.3 spec (Apollo Router v1.10.2+ or Apollo Gateway v2.3+ ).


## Thank you!


Finally, we want to say a huge thank you to the Apollo Federation and GraphOS community for the continuous feedback which has been instrumental in the design of these new features. If you have any questions on Federation,` @interfaceObject` , or` @key` on interfaces, don’t hesitate to reach out on our[community forum](https://community.apollographql.com/) or our[Discord server](https://discord.gg/graphos) .


## Resources


- [Entity interfaces documentation (including @interfaceObject and support for @key on interfaces)](https://www.apollographql.com/docs/federation/federated-types/interfaces/)


Written by


Korinne Alpers


[Read more by Korinne Alpers](https://www.apollographql.com/blog/author/korinnealpers)
