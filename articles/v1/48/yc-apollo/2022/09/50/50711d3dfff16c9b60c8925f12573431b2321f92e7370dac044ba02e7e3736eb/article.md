---
schema_version: "1.0.0"
document_id: "50711d3dfff16c9b60c8925f12573431b2321f92e7370dac044ba02e7e3736eb"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/generated-graphql-schemas-and-schema-design"
published_at: "2022-09-01T09:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:d38fe767668e4e696f669bfe3f4c5fd6ce707729fd478715955a0023a8393400"
---

# Generated GraphQL Schemas and Schema Design

GraphQL is a type-safe language for APIs. A common misunderstanding is that[GraphQL is another complete query language](https://www.apollographql.com/docs/resources/faq/#what-is-graphql) , like SQL, which allows you to search, filter, and select all the objects in a database. Instead, GraphQL should be thought of as a way to specify the types of data available and the specific operations users can perform over an API. It allows backend services and the clients consuming them to have a shared understanding of an API, and gives clients more control in selecting what parts of the data they would like to get back for a given operation. All of this is accomplished with the[GraphQL schema](https://www.apollographql.com/docs/tutorial/schema) .


A GraphQL schema is a machine and human-readable document that declares what operations are available and what types those operations return. The schema is the contract between a GraphQL server and its clients, which means that, inevitably, there are various opinions on what is good schema design. At Apollo, we have the unique opportunity to work with various GraphQL users as they build their schemas and we have documented much of what we have seen in[Principled GraphQL](https://principledgraphql.com/) and our[Enterprise Guide for Federation](https://www.apollographql.com/docs/federation/enterprise-guide/introduction/) . However, there is a common question that comes up when we start talking about schema design: “Should I manually craft my schema or generate it from my existing data types?”


## **Writing Types vs Generated Schemas**


Before we dive into the question, I need to clarify some common terms used in the GraphQL ecosystem.


** There are two ways you can write your GraphQL schema. They are with a[schema-first pattern or using a code-only pattern](https://www.apollographql.com/blog/backend/architecture/schema-first-vs-code-only-graphql/) . These methods differ in how the schemas are defined, but both allow API developers to declare their own types that are separate from the underlying data sources. Each of these patterns has its trade-offs but is outside the scope of this post.


The term generated schema is separate from schema-first and code-only. This refers to tools that can take some external data types or non-GraphQL schema and generate a GraphQL schema and/or the resolvers with exactly the same types. These tools allow you to take existing data sources, like REST APIs or SQL databases, and quickly add a GraphQL API on top of them with the same types. Clients can then start taking advantage of the benefits of GraphQL, like being able to customize the response selection or merge data sources together, with minimal upfront cost.


## **Client Oriented Schemas**


One of the core tenants of Apollo’s Enterprise Guide for schema design is having a[Demand (Client) Oriented Schema](https://www.apollographql.com/docs/federation/enterprise-guide/federated-schema-design/#best-practice-2-design-schemas-in-a-demand-oriented-abstract-way) . This means that the schema should act as an abstraction layer that provides flexibility to consumers while hiding service implementation details. A client-oriented schema can be built with a schema-first or code-only approach. In both patterns, we have the opportunity to communicate beforehand with all the users of our API, understand their needs, and design and build an API schema that can serve them all, without requiring every client to write additional transformation logic on top of our data. This is also called product-driven schema development, because the end goal is to be able to ship new features to our product with a minimal amount of effort, for all teams involved.


You will notice though, that generated schemas are not included. This is because generated schemas do not allow us to make changes to the types. They expose types that exist in other data sources in a single schema and possibly connect them in some way, but the data that our clients get back is still the same if they had called the underlying data sources themselves. Any transformations or additional merging of data must be done by all the clients, which means duplication of code shipped to your client apps.


If these tools are counter to one of the core principles of GraphQL schema design, you may be asking yourself: When should I use a generated schema?


## **Instant Gratification**


The primary benefit of generating a GraphQL API is the upfront cost to get started. If you have clients today that have to talk to 5 different databases, GraphQL can be a great tool to merge all those data sources into one schema and give some flexibility to your clients to select specific parts of the return data. If you have a small team and you don’t have time to build a new GraphQL layer or learn about how to deploy a new server to your infrastructure, the ability to get something up in running a matter of hours, or even minutes, can be incredibly powerful.


## **Real-world Insights**


Apollo has a recurring meeting with some of our customers and their[Graph Champions](https://www.apollographql.com/graph-champions/) , and we posed this question to them to hopefully get more insights from real developers in the enterprise and deploying GraphQL at scale: “Have you used or created a Generated GraphQL API and would you recommend other companies to use one?”. Their feedback echoed many of the points we have made already but brought up some interesting ideas.


(A generated schema tool) infers all the fields from the database and then it exposes GraphQL in a really incredibly powerful query language where you can get stuff from the DB, and I know a lot of projects that have gotten up really quickly with that insanely fast.


The way I like to phrase this when I am talking to teams that are wanting to bring this on is, instead of dictating what they should be doing, I explain to them what clean architecture should look like and then frame it in the context of exactly what you said, but say ‘What are your expectations with being able to change this? If you remove a field from the database, what does that do? Or if you need to pull something out into a completely different service because it has independent scaling needs, what does that look like?’. If they are ok with not changing anything then maybe this is ok.


Say you are exposing` first_name` from the DB directly, what happens when you need to migrate to expose that with[Elasticsearch](https://www.elastic.co/) ?


Staff Software Engineer – Medical Tech Company


This brings up the idea of what a migration strategy might look like in a generated schema when you want to move your database from a SQL format to a document-based or graph-based DB. Since the API is mapped 1-to-1 on the types from your tables, the clients are making requests against field names that map to rows and columns. If there doesn’t exist the same pattern in your new storage format, it could be difficult to have a completely-backward compatible schema, which means your clients would have to migrate to a new API or different types when you want to change your database.


---


We started off using a v1 of (a generating tool). You could define the key and table you connect to, and you would think in types, but then (the tool) would say ‘You can not do this change’. I know the database and I know in SQL it could work, but (the tool) would say: ‘This is going to break your relationship’. So you would go and start changing the first layer of schema, and then update the relationships. So you are changing the schema which you shouldn’t be doing (often) in GraphQL. You won’t see that in the beginning though, the more schema you add and the more database tables you keep generating, is when you start seeing issues.


Engineer Lead – Food & Beverage Retail Company


The team at this company ended up rewriting the GraphQL layer because their clients were constantly having to update their queries, and having to coordinate changes with the backend team. The generating tools helped them get going quickly, but the saved time upfront was not proportional to the added time they had to spend doing schema updates, client changes, and another code rewrite when their graph started to scale. This is not unique to one company though. When teams start adopting GraphQL, regardless of how it is built, it naturally becomes a new central place for cross-team collaboration. For those that spend the extra time in the beginning thinking about how their graph will scale in the future when more clients are using it and more teams are publishing schema changes, it leads to a longer supported and healthier platform.


---


This problem is not unique to the graph…For those of us that have been around a while, I’m sure we all remember back in the day when Microsoft was saying: ‘Just drag your database table onto your Visual Basic form and you’re done!’…There are tons of common historical questions. I would actually ask a fellow team: ‘Would you generate your REST endpoint directly from the database?’. If they say no, then the next question is why do we want to generate the GraphQL schema?


Principal Software Engineer – Business Expense Management Company


For teams thinking of GraphQL as a query language first, it can be seen as the best tool to solve your data merging needs. However, they are missing the concept that GraphQL is designed to be an API tool, not a data tool. In theory, we could build a similar REST API that merges multiple database calls and caches the results for clients to quickly query later. The reason you would build this is to solve a client problem though, not a database problem. Our databases should remain separate so they can scale independently and solve the storage problems they are designed for. We build abstractions on top of them, either in REST or GraphQL, to hide the details from clients who don’t need to be concerned about them.


This is why historical trends in tech have moved us towards more and more micro-services, but this has now reached a scale for our fellow clients and product developers that has become unmanageable. Having to juggle all the services needed to power a great web or mobile app experience and following updates and breaking changes, is too much for one team. This is where GraphQL has grown the most since its release, because having the ability to see all the services and their capabilities in one schema, and also inform the service owners of what operations clients are performing, gives the power back to client teams.


## **Choosing the right tools for the job**


With any problem in tech, there are going to be tradeoffs for any solution you decide to go with. GraphQL has been around for a while, but it is still an emerging ecosystem, so we are just now starting to hear more from developers and teams who have been using the tech for multiple years and some of the decisions they made.


If you are lucky to have the opportunity to create a new GraphQL API, conduct some market research and talk with the end users of your product: your client teams. Ask them how they use the existing products today and what frustrations they have. Ask how you might be able to help reduce those frustrations and remove any work needed from them. For every hour of effort you put in to help your clients, know that you are multiplying your efforts across all the clients you support. With a client-first GraphQL schema in mind, you can then choose the best tool that will help you achieve this goal and keep it running for years to come.


Written by


Shane Myrick


[Read more by Shane Myrick](https://www.apollographql.com/blog/author/shane)
