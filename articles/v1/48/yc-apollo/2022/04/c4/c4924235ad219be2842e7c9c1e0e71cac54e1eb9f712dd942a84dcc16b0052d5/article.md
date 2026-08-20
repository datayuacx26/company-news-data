---
schema_version: "1.0.0"
document_id: "c4924235ad219be2842e7c9c1e0e71cac54e1eb9f712dd942a84dcc16b0052d5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/federated-schema-design"
published_at: "2022-04-20T09:23:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:2649f57c438a883b3925c3db44b52f5fd64cf30b3ee9eef07b3d0c009b0c50bd"
---

# Federated Schema Design

## Introduction


If you’ve been building with GraphQL, you likely know how essential good schema design is. As the entry-point to all your GraphQL operations, it’s desirable to design a schema that’s easy to understand and work with, serves the needs of our clients today, and can be changed to fit their needs tomorrow.


This article teaches about the supergraph, platform thinking, and why it’s key to designing your graph for extensibility. We also discuss considerations that make federated schema design different from the monolithic approach. Finally, we cover the macro-level principles that help us design extensible and maintainable federated graphs.


## Prerequisites


- You know[what GraphQL is](https://www.apollographql.com/blog/graphql/basics/what-is-graphql-introduction/)
- You’ve[built and deployed a GraphQL server](https://www.apollographql.com/docs/apollo-server/) before
- You’re familiar with[Apollo Federation](https://www.apollographql.com/docs/federation/)


## The supergraph: your graph is a platform


When Apollo started our journey into GraphQL, we dreamt of a world where we’d have one API endpoint to get any data we needed. The reality of that dream is different for everyone, but the supergraph can help you get there.


The way we see it, the graph isn’t just another API – it’s a platform – it’s a *supergraph* . A supergraph is a next-generation API gateway capable of encapsulating all the capabilities across *all your products* and services and can grow to whatever our needs are.


Traditionally, when we build client-server applications, we design them to conform to a 1-to-1 relationship — one frontend and one backend API. Instead, consider the architectural benefits of thinking about your supergraph as a platform instead.


The world is rapidly changing. And while it’s impossible to always be ready for the next “thing”, by designing your backend API as a platform, we can reuse various capabilities across services to build new products for multiple clients using the existing infrastructure of the graph.


Your supergraph should provide a consistent experience of the **capabilities** you have. Each client application may have a slightly different user interface, but the underlying capabilities are often the same if not similar.


For example, if the` checkoutCart` mutation exists for mobile apps, shouldn’t the website reuse that functionality? We should aim to leverage that same mutation instead of introducing additional work for all the clients; this is the power of the supergraph.


***Note: You may have some optional input values or expose additional fields on the*** ***response for a specific client use case, but everyone uses the same root checkout*** ***mutation.***


By thinking about our efforts as a platform, we create a consistent approach to executing operations (queries and mutations) against the supergraph.


## How designing a federated schema is different than designing a monolithic one


Designing a federated schema has similarities to a monolithic schema design but is a little different. The three main differences represent a shift in how we think of our schema:


- **Subgraphs:** We focus on what can be provided by subgraphs, not by the entire schema
- **Generic API design:** Stronger emphasis on designing interfaces generic enough for use by multiple clients
- **Explicit collaboration design:** We must document the unique identifiers (key fields) for entities


## Subgraphs: splitting or combining features


Adding a new feature to a large monolithic schema could be demanding. Often requiring complete knowledge of the schema, it may take time to discover the most appropriate place to place a feature.


In a federated schema, we primarily work against subgraphs; this means that for any new feature (combination of queries or mutations), you can design it as either:


- a part of an existing subgraph
- a new subgraph


This decoupling enables you to focus on only the relevant portion of the schema at a time, helping new devs get up to speed and contribute their first feature to the supergraph faster.


Additionally, after everything is working, if we’ve designed the feature as a part of an existing subgraph, we can always break it up into multiple subgraphs or combine it back together with an existing one later.


## Generic API design: designing for multiple clients


When designing a GraphQL schema for a single client app, schemas sometimes take on a more focused shape — distinctive of the client to which it belongs.


As we now know, platform thinking means designing a graph for multiple clients.


We want a flexible supergraph design so that clients get productive quickly. Still, we don’t ** want to concede to introducing separate “web” and “mobile” versions of types within our supergraph since to do so is an anti-pattern.


## Explicit collaboration design: context mapping


The way relationships work in a monolithic graph vs. a federated one is also different.


Implementation-wise, for monolithic graphs, we typically link relationships together through:


1. **The context pattern:** Writing code to map known values onto the request context for usage within child objects *or*
2. **Parent objects:** Some code checking parent objects in type resolvers


In a federated graph, we rely on *the*[graph router](https://www.apollographql.com/docs/router/) to generate a[query plan](https://www.apollographql.com/docs/federation/query-plans/) to figure out how to get all the needed data for a given query. As schema authors, we just annotate the relationships between entities within the schema.


The significant difference is that in a monolith, we think of the relationships in our resolvers. In contrast, in a federated design, we express relationships declaratively and explicitly within our subgraph schemas.


**Additionally** , depending on if you’re a single developer or multiple teams contributing to the supergraph, there are two different schools of thought for how you compose your types: (1) a single subgraph owns types vs. (2) shared type ownership across subgraphs.


In general, small projects rely on the (1) single subgraph school of thought; this means that a subgraph exists as the “owner” of a given type, and other subgraphs contribute fields to that entity through the use of the same key fields. Types marked with key fields are called[entities](https://www.apollographql.com/docs/federation/entities) and they often represent some shape of an object in the underlying data source.


Larger projects or complex use cases often lean more into the (2) shared type model where subgraphs collaboratively express the entities by putting forth the fields it contributes. This enables the flexibility for subgraphs to express any entity in a way it can provide data for it. For example, a product might actually change depending on if a locale is identified with it.


There are also times when subgraphs need to share a given type, but it doesn’t have a uniquely identifiable set of key fields. We could have a search functionality for both of the subgraphs above and they would want to share a` SearchFilter` type (there isn’t anything to “extend”). Apollo Federation provides the[@shareable directive](https://www.apollographql.com/docs/federation/federation-spec/#shareable) that can be used to indicate that a field can be resolved by multiple subgraphs.


Now let’s get into some of the design principles for building a platform!


## Principle #1: Think in Entities


Entities are a core building block to federated schema design. You can think of entities as the primary nodes in your supergraph, connected and extended to various subgraphs.


Represented by the` @key` directive, entities denote the primary key (expected values for an entity) by the fields listed in the directive.


Other subgraphs can build on top of the entity by providing additional fields and are responsible for resolving only the fields they contribute.


So how do we even identify what should be an entity? In general, if we can answer yes to any of these questions, then we probably want to define that type as an entity:


1. Can we uniquely identify the objects based on one or more of its fields?
2. Does the type map closely to the data source shape?
3. Are the inputs to resolve a given entity well defined?


## Examining a shopping cart query structure


To better understand thinking in entities, let’s look at an example where we’re exposing a user’s active shopping cart.


The query looks like this:


```text
query MyActiveCart {
me {
activeCart {
items {
name
price
}
}
}
}
```


Our query shape exposes a logged-in user and their active shopping cart with the products in the cart. Asking ourselves what types should be defined, thinking about the business, we realize that:


- **Users** are uniquely identified by their email addresses.
- **Users** can only have one active shopping cart at a time, but there is an order history of past completed **Carts** . The completed cart information is stored in our databases by cart id and connected to our users.
- Our product API is a highly optimized REST API with well-defined arguments. We use the **Product** type in multiple places in our graph.


For simplicity’s sake, **User, Cart, and Product are entities** , and we can put each one into its own subgraph.


Now we have three subgraphs: a` User` ,` Cart` , and` Product` . Try not to get too caught up in your early implementation. We recommend you just pick something and can easily migrate to the next thing in the future. Maybe we find out that our User has two flavors, internal employees and customers; the main difference is that employees have an internal id. No problem! We have both of those entities defined in our User subgraph. Using this information, we have the following subgraphs:


We expose each entity on the graph through the capabilities we decide. Looking at the User subgraph for this example, we’re going to have to expose a root` me` entry point that returns our User entity. Borrowing from classic object-oriented design, we think about the other capabilities of a` User` subgraph (role), like allowing employees and customers to update their profiles with things like a profile picture.


**Extensibility:** Our supergraph is a platform, and we’re beginning to provide an experience for an authenticated customer all in one place. The` me` entry point on the root Query is an excellent example of a platform functionality that would enable other client consumer applications to be developed independently of the paradigm. Maybe a user wants to check their activeCart on their watch? Or Google Home speaker? Or Alexa? The supergraph helps us create a ready platform for whatever is next.


**Using entities in other subgraphs:** As capabilities grow, sometimes they need information from multiple entities; this is one of the benefits of Apollo Federation — the ease of using an entity in another subgraph. For our example, we need to expose the` activeCart` capability so that a` Customer` can use it.


Now, the Cart subgraph *extends* the Product and Customer entities:


When this first feature is complete, you can imagine various other capabilities built across all subgraphs to provide that delightful experience we want in our platform. Ultimately, it is up to developers to decide what should be an entity and how to break down subgraphs within the supergraph.


## Principle #2: Use Domain-Oriented Thinking


As we build our supergraph, the subgraphs we define typically tie closely to our use case’s subdomains (an idea from DDD).


For example, while we may start with a web and mobile backend-for-frontend architecture for our subgraphs, we’ll incrementally reach a point where both the web and mobile client team rely on entities defined in their own subdomains. **For this reason, we recommend thinking of subgraphs as the subdomains in your supergraph** .


Domain-Driven Design (DDD) is a software design approach that focuses on modeling based on the reality of your use case. Domain-oriented thinking is essential in today’s technology space because the internal relationships made in the APIs we create are a big part of what makes a delightful experience.


To know that that new killer feature can be made possible by connecting a new API with existing data, we need to design our supergraph to retain that flexibility.


**Using the language of the subdomain:** As we zoom in from the entire domain to a single subgraph, we are thinking about the entities we define and use in that domain. The types and fields we define in subgraphs should use the language of that domain so that it makes sense to all the clients. It doesn’t matter what you’re working on; naming things is hard. Trying to use the domain language doesn’t answer the question of how you should name things consistently across subgraphs.


In general, you should give individual subgraphs the flexibility to build what they need but also provide some basic naming conventions for your supergraph. (i.e., are we using camel or snake case, let’s not do both 🙂).


*Note: If you’re interested in learning more about using Domain-Driven Design techniques to design a GraphQL schema, check out Khalil Stemmler’s blog post on*[Domain-Driven GraphQL Schema Design](https://khalilstemmler.com/articles/graphql/ddd/schema-design/) *.*


## Principle #3: Make Your Collaboration Strategy Explicit


Entities are a common point of collaboration in our subgraphs, and being explicit on your strategy here is helpful for the overall design.


When designing our federated schemas, remember that we have two options for how to represent entities: (1) single subgraph ownership vs. (2) shared type ownership.


Which one to use? That depends on your particular scenario. Here are a few.


**Greenfield projects and smaller team projects**


Small or greenfield projects have more flexibility because they are new and there isn’t the need to change what might already exist. In this case, we advise just picking a type ownership standard and sticking with it.


For example, if we decide on the (1) single subgraph ownership model, the product subgraph owns the Product type, and other subgraphs can merely extend the primary Product representation. We can safely repeat the single subgraph ownership pattern until the day arises when we need to introduce multiple representations of an entity used in multiple subgraphs. In the future, adopting a shared ownership model could help keep subgraphs cohesive and relationships explicit.


**Larger companies and enterprises**


The challenge in the enterprise space is that we typically have a lot of existing business service level agreements (SLAs). Since we can’t just rip those apart and modify the structure of existing teams, we must consider the shared approach. A shared-ownership model for entities is necessary for the enterprise to meet the business where it’s at. Subgraphs define entities and the fields they can provide data for based on the defined keys. One subgraph may use the` product` by its id, whereas another subgraph uses the product by its ISBN because it’s specific to books (and that totally works)!


Another approach we’ve seen some teams use successfully is to make every route of their REST API containing an id, an entity with canonical capabilities. We recommend growing the supergraph based on client use cases, but this isn’t necessarily a bad approach if you are just starting out. Each subgraph team has to decide on what’s best for them.


That leads us to our next principle, focus your workflow around subgraphs.


## Principle #4: Use a Subgraph-Focused Workflow


To perform testing and feature development, we should use a subgraph-focused workflow. If we’re a part of the` product` team, we shouldn’t need to spin up an instance of the` cart` or` user` subgraph to develop features.


Each subgraph is just a running GraphQL server, and the best workflow meets that effort where they are. Since Apollo Federation is a thin spec on top of GraphQL and can[support 20+ server implementations across 10+ languages](https://www.apollographql.com/docs/federation/other-servers/) , developers are free to set up workflows so subgraph teams can build their subgraphs however they like and can easily plug into the supergraph.


In schema design, one thing to consider is how we develop our schema for each subgraph. We have a variety of options.


- Schema-First
- Code-First
- Autogenerated (not recommended)


*Note: We don’t recommend autogenerating your schema because it often brings unused fields on to the graph and not the right shape. In general, how we store data is shaped very different than our client’s needs.*


Depending on the language and server implementation you use will play a factor in how you develop your schema from a design. For large organizations or multi-language projects, It is often easier to design in schema first and translate that into the necessary code if that is the preferred workflow. This enables other developers to contribute to the design without needing to know any of the language implementations.


## Conclusion


The supergraph helps us elevate our GraphQL efforts, and we should think about designing our schema as a platform. At the heart of that schema design are the individual subgraphs, and we should create those in a domain-oriented fashion. We defined some principles that you can use as we think about how we design our federated schemas.


Here is a quick reference to them:


Have an idea for another guide you’d like to see? Use the feedback form below to suggest another topic


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
