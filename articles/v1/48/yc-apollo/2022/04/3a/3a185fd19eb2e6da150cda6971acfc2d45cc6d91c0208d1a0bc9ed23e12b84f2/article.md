---
schema_version: "1.0.0"
document_id: "3a185fd19eb2e6da150cda6971acfc2d45cc6d91c0208d1a0bc9ed23e12b84f2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introduction-to-apollo-federation"
published_at: "2022-04-15T09:06:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:7f8e02b40873ea98126bae9b8e1ddea6519972cafd5df06d92363eb26f572bd0"
---

# Introduction to Apollo Federation

Whether you are a large company or a single developer, we all want to know the best way to design an effective architecture – the schemas, services, folders, and so on. We find ourselves asking “Should we start with a modular monolith? Should we use microservices?”


These early architectural questions are important because they affect so many aspects of the way we work, making it easier or harder to divide work, understand the code, and scale-up in the future.


Before we can explore some of the more nuanced, technical topics, we need to learn some basics ways to think about a federated architecture.


In this post, we’re going to learn what federated architecture is and why we would want to use it.


## What is a federated architecture in GraphQL?


When developing a GraphQL monolith, the architecture is straightforward. Clients talk to your GraphQL monolith, and that monolith connects to the various data sources you have. The GraphQL server determines what data sources to interact with based on the incoming query and the necessary resolvers. We typically represent the details of some parent type or fields in a downstream resolver using some bit of code in the project.


Each GraphQL server provides a portion of the schema in the overall graph in a federated architecture. We call these GraphQL servers **subgraphs** . We can combine these subgraphs through metadata defined in their schemas to establish relationships in the graph:


A fundamental difference between a federated architecture and a monolith is that we define the relationships defined in our graph using metadata, not additional logic in our resolvers. Let’s take a simple Products and Reviews query as an example:


```text
query ProductsWithReviews {
products {
name
reviews {
content
}
}
}
```


In a monolith, we’ll need to ensure the resolver for “reviews” has access to the individual product id. This can get even more complicated as the monolith grows and a Review resolver is shared in more places in the schema.


In a federated architecture, the Product type would live in the products subgraph and be identified with metadata to identify the key fields for it. We would consider the Product type as an **Entity** and it would be denoted with the` @key` directive:


```text
# Products Subgraph
type Product @key(fields:”id”){
id: ID!
name: String
}
```


There would also be a reviews subgraph responsible for the reviews information. It would use the Product directly and add the appropriate fields for the graph:


```text
# Reviews Subgraph
type Product @key(fields:”id”){
id: ID! @external
reviews: [Review]
}
type Review {
content: String
}
```


Now in the reviews subgraph is responsible for providing the data it can without having to worry about any of the other details surrounding the Product type. Each subgraph represents the schema they can provide data for and these are combined to create the **supergraph** .


The **supergraph** knows all of the data and operations across all of your schemas and where to find them, whether that’s in a remote URL or a file in a git repository. Using the[router](https://www.apollographql.com/docs/router) , the supergraph figures out the most efficient path to fetch the relevant data. The best part is that the supergraph doesn’t look any different than a regular GraphQL endpoint. It just combines all the data in a structured, easy-to-use way.


Now that we understand a little more about what a federated architecture is, why would we want to use it? We should start by looking at the start of our GraphQL journey, the GraphQL monolith.


## The journey of a GraphQL Monolith


Our first implementation of a monolithic graph was great, but as it grew, we started to see problems. The initial schema and use cases were simple, and we could create relationships in our schema that connect to various APIs by how we defined our field and type resolvers.


New use cases grew our schema along with our resolvers. The pattern of passing data through our resolvers becomes increasingly essential, and we often realize it once it’s too late. Maybe some teams have just thrown data on the request’s context, and the memory footprint of our instances runs out of control. Often, we introduce complexity into the resolvers, and they tend to cascade into each other.


Type resolvers are defined to help integrate into a specific API, but as we use a type in other places within our schema, a resolver could be used in more than one place. This could cause that resolver to grow with some code to check expected parent values. We just want some simple expected inputs to provide to the underlying data source for that type, and the mixed-use in our monolith can grow in a way that requires “tribal knowledge” of how the graph is designed.


**“Too big” monolith**


We should celebrate the success and growth of our GraphQL initiative. Whether you’re a single dev or a large organization, GraphQL is the future. We’ve seen this pain for monolithic graphs when they grow “too big”. “Too big” is differs by project, though, and it’s dependent on factors like the number of devs and its overall success.


When we had a monolith at Apollo, it became increasingly difficult to onboard new members to the project because there was an enormous scope of what you might encounter. Setup and running may be simple, but successfully navigating the code to implement a new feature is challenging. For a new developer trying to build their first new feature, starting from a templated server simplifies the scope of what they need to know.


**Backend for Frontend (BFF) Monoliths**


Some projects are large enough that the web and mobile teams have built separate GraphQL backends, each as a monolith. This could have naturally grown from the team’s desire to move quickly and separately from each other. These bodies of work often overlap with each other and don’t share learnings as easily. There probably is a good reason why one team did something differently, and instead of having that learning standardized into the graph, it gets siloed into one team’s BFF. The BFF pattern also has similar “too big” potential pitfalls for each team, but a need to standardize within the organization is typically the larger driver.


**GraphQL Microservices with handwritten gateway code**


Many projects felt these growing pains of a GraphQL monolith and ventured into GraphQL microservices. We did this by writing some code in a GraphQL gateway that would “stitch” together multiple GraphQL schemas: schema stitching. Schema stitching gave a new level of coordination that the monolith was missing. The web team and mobile team can combine their efforts by just trying to add their BFF schemas together. Gateway teams and committees started everywhere to provide a level of governance not out of desire but out of necessity.


Combining multiple schemas can result in numerous problems. You would think you could pull together any schema, but how do you handle things like naming conflicts or mismatches in shared types? What if both teams have the same type defined but drastically different representations? You have to decide how you’re going to combine all this together and make some decisions about collaboration in contributing to the graph. The gateway team would often help other new efforts join the graph.


Sometimes, a query would have an issue, and getting to the bottom of it could be an extremely painful exercise. A client team reports that their query is returning much slower now and they didn’t change anything on their end. How does the incoming query even execute? You probably have some performance monitoring and tracking information, but which GraphQL microservices were called and in what order? It could even be that the merging flow changed, which has an impact on the overall execution. I may have spent a day in the past tracking an error down to this exact problem 🤦.


We also had to think about how to handle updating our gateway when there were changes to any GraphQL microservice schema. This meant crafting a solution either built off Introspection queries or some cloud file storage solution. Eventually, the day comes when your graph has some downtime due to a team pushing a breaking change. The breaking change most commonly comes in the GraphQL microservice deployment succeeding only to have the gateway fail on startup because it had type merging errors.


## Why Apollo Federation?


We designed Apollo Federation to solve the common monolithic pitfalls and to prevent having to rely on brittle code to merge multiple schemas. Now, we can combine schemas into a supergraph in a declarative way. We can define relationships in our graph using nothing other than types, just like we expect to.


Separation of concerns is a core principle of Apollo Federation because it allows each team to deal with its own subgraph. Federation ultimately increases team productivity and collaboration because they can see the relationships defined in the schema and quickly understand points they can collaborate on. Each subgraph only has to deal with the schema it represents. Federation also cuts down on breaking changes and communication while maintaining the necessary collaboration based on the defined metadata. Federation makes the development process simpler by being able to focus on the portion of the schema you’re implementing.


Along with having a modularized graph, we also gained the ability to scale our subgraph instances independently. In a monolith, you could only scale the entire monolith. With Apollo Federation, we can scale subgraphs independently and even architect subgraphs for expected loads. For example, people look at products more than they are using the “checkout”. Running these pieces of functionality in their own subgraphs enables us to scale them independently. We can even define independent caching strategies for our subgraphs if needed.


## Conclusion


Eventually, the monolith outgrows what it was initially designed for, and we find ourselves trying out GraphQL microservices. Apollo Federation provides a flexible architecture that enables you to scale your graph and define relationships through metadata defined in your schema. Federation removes the need for any handwritten code to “glue” together multiple GraphQL microservices. With federation, you can responsibly share ownership of your graph across any number of subgraphs. No single subgraph controls every aspect of a type; instead, we distribute the definition across codebases rather than centralize it. And since it’s just GraphQL, you can[build your subgraphs in any language](https://www.apollographql.com/docs/federation/v2/federation-spec/) .


## Continue Reading


[Federated Schema Design Principles](https://www.apollographql.com/blog/federated-schema-design/)


Have an idea for another guide you’d like to see? Use the feedback form below to suggest another topic! Have an idea for another guide you’d like to see? Use the feedback form below to suggest another topic!


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
