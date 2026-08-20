---
schema_version: "1.0.0"
document_id: "6d1361a0704c04b54523125440076d82d9b4c444a3381090c1b8478cb9b705b9"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/our-journey-to-apollo-connectors"
published_at: "2025-02-19T08:40:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:d95468b67863c945b38e3078710c53eebcd744cf0bce1a647e56800e2fa4610a"
---

# Our Journey to Apollo Connectors

At Apollo, we’ve always envisioned a world where APIs are truly declarative—from the client to the schema, all the way through to implementation. A world where orchestrating your data and services isn’t just powerful, but also easier to develop and maintain. With the general availability of[Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors) , that vision is now a reality.


One of GraphQL’s key strengths is its declarative nature. Rather than writing code and business logic to query various APIs, you *declare* your data requirements—typically alongside a declarative React, Compose, or SwiftUI component—and let another system determine how to fulfill them. This approach aligns GraphQL with other declarative, cloud-native products like Kubernetes and Docker, as well as established technologies like SQL.


Apollo Connectors extend this declarative approach even further. They enable declaring the data and capabilities of existing APIs so that[Apollo Federation](https://www.apollographql.com/federation) and[GraphOS Router](https://www.apollographql.com/graphos-router) can automatically orchestrate requests. As you will see, this approach makes your APIs even more useful without all the costs associated with BFFs (backends-for-frontend.)


```text
type Query {
products: [Product]
@connect(
source: "ecomm"
http: { GET: "/products" }
selection: """
$.products {
id
name
description
}
"""
)
}
```


Our journey to Apollo Connectors has been a winding one, and I’d like to tell that story from my perspective. By the end, you’ll understand the technical decisions and architectural principles that shaped Apollo Connectors—and hopefully share our excitement about their potential.


Build your first Apollo Connector in minutes with our new free tier.[Try it today](https://studio.apollographql.com/signup?referrer=blog) .


## 💡#1: My introduction to Federation


My first 💡lightbulb💡moment happened way before the idea of Connectors, during Apollo’s Federation announcement at GraphQL Summit 2018. As I watched[Martijn Walraven’s fantastic presentation](https://www.google.com/url?q=https://www.youtube.com/watch?v%3DOFT9bSv3aYA&sa=D&source=docs&ust=1739556560278911&usg=AOvVaw28vPZo0oO2VXCfo6IGAP7O) , I immediately recognized a compelling solution for a problem I faced at Square, where I was a Staff Software Engineer.


We were managing a massive RDBMS with billions of rows and needed to implement a much better search than the database could provide. As soon as Federation became available, I tried this out:


```text
# ElasticSearch schema
type Query {
searchCustomers(query: String): [Customer]
}


type Customer @key(fields: "id") {
id: ID!
}
```


```text
# RDBMS schema
type Customer @key(fields: "id") {
id: ID!
name: String
profile: Profile
}
```


My test worked on the first try—I was thrilled! Even better, I immediately understood how it worked when I saw the[query plan](https://www.apollographql.com/docs/graphos/reference/federation/query-plans) :


```text
QueryPlan {
Sequence {
Fetch(service: "elasticsearch") {
{
searchCustomers { __typename id }
}
}
Flatten(path: "searchCustomers.@") {
Fetch(service: "rdbms") {
{ ... on Customer { __typename id } } =>
{
name
profile {
email
}
}
}
}
}
}
```


By declaring a simple relationship between the two systems—Customers are[entity types](https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/entities/intro#defining-an-entity) identified by their` id` —Apollo Federation automatically orchestrated calls to the services. Client applications wouldn’t even know there are multiple systems behind the scenes.


This approach fit perfectly to the problems my team and I were facing: creating unified and consistent end-user experiences on top of massive distributed systems. This led me to dedicate my career to Apollo Federation and eventually join Apollo as a Solutions Architect.


## 💡#2: My first attempt at Connectors


Working with[customers](https://www.apollographql.com/customers) implementing Apollo Federation at scale, I witnessed the organizational and technical challenges that arose when adopting both GraphQL and Federation. I saw an opportunity to improve the process, leading to a proof-of-concept declarative system for implementing a GraphQL API with gRPC (because that’s what I used at Square). The implementation used[GraphQL directives](https://www.apollographql.com/docs/apollo-server/schema/directives) to implement fields:


```text
type Query {
post(id: ID!): Post!
@grpc__fetch(service: BLOG_SERVICE, rpc: "GetPost")
}
```


It wasn’t particularly original, and it never gained traction, but it taught me some valuable lessons that would come in handy later:


1. **Redundant architecture:** My approach involved a separate service running alongside the federated graph—essentially a black box that called gRPC methods in various combinations. However, this was redundant since GraphOS Router already does something similar; we call it “query planning.” Running two overlapping services added unnecessary complexity.
2. **Schema-first:** I prioritized GraphQL schema design, allowing users to craft schemas optimized for their clients, using directives to map fields to upstream RPC methods. This choice received positive feedback: it was easy to understand, and it enabled users to elegantly adapt RPC patterns to fit GraphQL idioms. And working in the GraphQL schema definition language is more enjoyable than using generated gRPC clients in languages like Java.
3. **Reinventing the wheel:** I had to write complex code to compare the GraphQL schema with the RPC service. It was a frustrating experience, both because it was technically difficult, and it always felt eerily similar to code in Federation (foreshadowing!)


Though I shelved the prototype, these insights became invaluable when Apollo began seriously exploring Connectors.


## 💡#3: Sometimes we’re working with not-so-great APIs


We started the Connectors project with user research, hoping to learn what kinds of APIs users wanted to integrate into GraphQL. The results were surprising! Many teams were grappling with legacy systems that provided, let’s just say, *not-so-great* APIs.


- They were frequently inconsistent in structure, naming conventions, and format.
- If they had any documentation at all, it was often incomplete or inaccurate.
- They did too much or not enough; either you received more data than necessary, or you had to call multiple endpoints to get all the information needed for a task.


While it was tempting to optimize for modern, well-structured APIs with reliable specifications, we recognized that the greatest need came from teams with legacy systems. We needed a product with the flexibility to work with APIs of all qualities.


With a clearer understanding of the kinds of APIs we needed to support, we started exploring the space—looking at how others had tackled these challenges before.


## 💡#4: There’s room for improvement


In addition to user research, we also looked at existing solutions for automating GraphQL APIs. While we found a lot of interesting ideas, we knew from customer conversations that they had major downsides due to being either autogenerated from data sources or black-box implementations.


1. **The autogenerated approach** made it easy to get started but resulted in APIs that didn’t follow best practices. Tools for adapting the results were cumbersome. In addition, this approach does not provide proper abstraction over the upstream schemas. Changes to the data source could easily cause breaking API changes.
2. **Black-box implementations** , similar to my prototype, prioritized schema design, but were redundant with Router and Federation. Adding another service increased operational complexity and maintenance burden for teams.


We knew there had to be a better way. We spent a lot of time discussing alternative approaches, defining our ideal developer experience, and determining what was technically feasible.


## 💡#5: We can just use Federation!


Sometimes the best solution is hiding in plain sight. During discussions with our engineers focused on Federation, we had our first big breakthrough: Federation wasn’t just a system for combining GraphQL APIs—it was fundamentally a system for orchestrating API calls. If you think about it, what is a GraphQL API but an HTTP endpoint with a specific protocol? This insight unlocked everything.


We sketched out an early version of Connectors using some directives pointed to REST-like endpoints. I started with my initial search use case, calling a search appliance and then filling in the rest of the data with a` GET` endpoint. In keeping with previous efforts, we designed a` @connect` directive to declare the implementation of a field.


```text
type Query {
searchCustomers(query: String!): [Customer]
@connect(GET: "http://search.api/q={query}")


customer(id: ID!): Customer
@connect(GET: "http://customer.api/customers/{id}")
}


type Customer {
id: ID!
name: String
profile: Profile
}
```


To test the concept, we manually converted this single schema with directives into separate schemas, each matching the capabilities of its respective API endpoint. It was a tedious but straightforward translation:


```text
# This is the schema for the http://search.api/q={query} connector.
# It provides a root field, the Customer type, but only the
# identifying fields.
type Query {
searchCustomers(query: String): [Customer]
}


type Customer @key(fields: "id") {
id: ID!
}
```


```text
# This is the schema for the http://customer.api/customers/{id}
# connector. It provides a way to fetch the rest of the
# Customer data when given an ID.
type Query {
customer(id: ID!): Customer
}


type Customer @key(fields: "id") {
id: ID!
name: String
profile: Profile
}
```


This matched my first use case for Federation, so naturally, it just worked! We didn’t need another system for orchestrating these requests—Federation handled it out of the box.


Using various public APIs, we sketched out more examples using these early directives before manually converting them to subgraphs. We discovered[a number of patterns](https://www.apollographql.com/docs/graphos/schema-design/connectors/guides) we could accomplish, revealing the power of[entities](https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/entities/intro) . Using Connectors, we could reference and extend entities to create a proper *graph* out of disparate endpoints, exposing the true data model hidden in these APIs.


Learn more about[thinking in entities](https://www.apollographql.com/blog/api-orchestration-with-connectors-thinking-in-entities) here.


Federation is so adept at managing entities that it automatically handled half of the[dataloader](https://www.apollographql.com/docs/graphos/schema-design/guides/handling-n-plus-one#the-dataloader-pattern-solution) problem for us: the query planner deduplicates entity references, ensuring that we don’t make redundant API calls when fetching relationships in a list.


We quickly noticed another advantage of this approach: Connectors would work seamlessly with existing subgraphs. Since Federation can already call GraphQL APIs, this means:


- Users could adopt Connectors inside their existing federated GraphQL APIs with no additional work.
- If Connectors didn’t work for a use case, traditional resolver-based[subgraphs](https://www.apollographql.com/docs/graphos/reference/federation/compatible-subgraphs) are a reliable fallback that work great alongside Connectors.


So with a working prototype based on manually translating connector directives, we’ve proven that Federation was more than capable of orchestrating APIs calls. We moved on to the next problem: what tools and directives do we need to automate converting our Connector schemas into something the[query planner](https://www.apollographql.com/docs/graphos/routing/about-router#subgraph-query-planner) can use?


## 💡#6: We’re making a query language for APIs


A tool for converting directives to data sources actually has three jobs:


1. **Declaring which parts of the schema an endpoint can satisfy.** For example, the search endpoint might provide` Query.searchCustomers: \[Customer\]` and` Customer.id` , but it can’t provide` Customer.name` . The query planner must know this so that it can plan for two requests—one that searches for customers, and another for fetching their names.
2. **Mapping API responses to the GraphQL schema.** REST APIs use different conventions for names and structure. For example, the API might use` customer_id: 1` but we want a` customer: { id: 1 }` in GraphQL.
3. **Ensuring the mapping is statically analyzable.** To guarantee that the Connector will reliably perform the first two jobs at runtime, we must be able to analyze and validate the mapping during development.


We started with the simplest thing that worked: a syntax similar to the GraphQL query language:


```text
@connect(
GET: "http://search.api/q={query}"
selection: "id: customer_id" # alias customer_id to id
)


@connect(
GET: "http://customer.api/customers/{id}"
selection: "id name profile { email }" # this endpoint provides more fields!
)
```


This approach turned out to be a great choice. It’s easy to write and read for common cases, familiar to users with GraphQL experience, and extensible for more complex mappings.


Early testers provided invaluable feedback on the mapping language. We added syntax for literal values (common in complex request bodies) and methods for transforming values based on their feedback. We’ve started a[standard library of methods](https://www.apollographql.com/docs/graphos/schema-design/connectors/mapping#methods) and have plans for many more, declaratively replacing the logic that previously required resolvers.


```text
data->first.attributes {
id: sku
name
description
status: status->match(
["in stock", "IN_STOCK"],
["discontinued", "DISCONTINUED"],
[@, "UNKNOWN"]
)
}
```


To learn more about the mapping language in Connectors, we have an[Odyssey Course](https://www.apollographql.com/tutorials/connectors-mapping-and-transforms) , as well as[documentation](https://www.apollographql.com/docs/graphos/schema-design/connectors/mapping) and a[playground](https://www.apollographql.com/connectors-mapping-playground) with examples.


## 💡#7: GraphOS Router is *fast*


Once we had an initial implementation of the directives and the mapping language, it was thrilling to see it in action! Data was flowing and query plans confirmed that we were orchestrating calls effectively.


But the real excitement came when we ran load tests using Connectors. Without any optimization, Connectors were blazing fast! Given that we’re doing more work in the Router request pipeline, like transforming responses using the mapping language, the performance was a testament to the engineering behind the Router. It validated our decision to build Connectors into the Router and Federation rather than adding another layer.


Load testing GraphOS Router is an interesting challenge. Generally speaking, its throughput greatly exceeds that of its upstream services. Many of our tests use unrealistically optimized subgraphs and APIs that return instantly, allowing us to focus on raw Router performance. In these tests, Connectors and subgraphs performed equivalently—exactly as we hoped.


Luckily, the Connectors public preview landed with the[new native query planner](https://www.youtube.com/watch?v=sGWS1U08IMI) , giving us the opportunity to test the scalability of Connectors. We stress-tested the planner with a schema of one thousand Connectors providing overlapping types—truly pathological, as it causes the planner to consider a combinatorial explosion of plan options. The new native planner handled this scenario with ease!


Beyond the raw speed of the Router, Connectors simplify the request pipeline, eliminating network hops and JSON serialization:


The usual development mantra is “make it work, make it right, make it fast”, but thanks to the Router’s speed, we could focus on “make it right” and refining the developer experience. Fortunately, Federation’s[composition step](https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/composition) gave us a powerful way to enforce correctness early in the development process, catching problems way before they reach production.


## 💡#8: Zen and the art of finding errors early


Orchestrating API calls in real-world applications is inherently complex. For Connectors to be truly valuable, they must simplify that complexity—otherwise, they’re not worth using. That’s why we didn’t just implement Connectors in the Router runtime and call it a day. Instead, we built a robust validation system to catch potential issues as early as possible. This is especially critical when working with *not-so-great* APIs.


One of the easiest yet most effective validations is ensuring that request inputs match the schema. Typos and mismatches are common, but we can catch them automatically with basic validation rules.


In this case, the directive references` customerId` , but the schema defines the argument as` id` . A simple validation can surface this mistake immediately, saving developers from frustrating debugging later.


When mixing and matching many API endpoints in a single schema, things get trickier. Consider a “list” endpoint and “get” endpoint, each returning different subsets of the same type:


```text
type Query {
customers: [Customer]
@connect(
http: { GET: "http://customer.api/customers" }
# Note how this compares to the other connector
selection: "id email"
)


customer(id: ID!): Customer
@connect(
http: { GET: "http://customer.api/customers" }
# A different subset of fields
selection: "id name phone"
entity: true
)
}
```


Can you spot the problem? If a client sends this query:


```text
{ customer(id: "1") { name email } }
```


There’s no possible combination of requests that can resolve both` name` and` email` . This might be obvious in a small example, but imagine trying to find the problem in a subgraph made of dozens of endpoints.


Fortunately, we didn’t need to invent a solution: it’s built into Apollo Federation as the[“satisfiability” rule](https://www.apollographql.com/docs/graphos/reference/federation/composition-rules#solutions-for-unresolvable-fields) . Since Connectors are built on Federation, we get this validation for free!


These validations help immensely when trying to build a graph of Connectors. But the real game-changer was[integrating the validations directly into IDEs](https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/ide-support) . With real-time feedback on connector directives, developers immediately see errors as they type, rather than discovering them later during execution. This creates a smooth, intuitive experience—one that feels almost magical.


## 💡#9: To err is human, to debug is divine


Validations prevent many kinds of mistakes, but as you’ll recall, we’re often dealing with *not-so-great* APIs that misbehave in confounding ways. And you can’t stick` console.log` statements in declarative systems like Connectors to debug a problem.


To solve this, we added a new section to[Apollo Sandbox](https://www.apollographql.com/docs/graphos/schema-design/connectors/troubleshooting#return-debug-information-in-graphql-responses) that provides complete visibility into the requests and responses of your Connectors. Whether you forgot to set a header, or you made a mistake in your response mapping, you can see exactly what’s happening under the hood.


You can also get mapping warnings in[logs and traces](https://www.apollographql.com/docs/graphos/schema-design/connectors/router#telemetry) , so you have similar visibility into the production behavior of your Connectors.


## 💡#10: Connectors work with lots of things!


As we started experimenting with Connectors, we realized that the pattern of “querying” an API and mapping it to schema opens the door to all sorts of integrations.


- Many products have HTTP/JSON APIs. They may not be “REST” but they can still work great with Connectors. ElasticSearch is one example: you can map ES documents directly to types in your schema.
- Some RPC implementations have a JSON bridge, allowing you to use RPC methods in your schema. I can finally retire my original gRPC prototype!
- Hypercloud features like AWS Lambdas and Step Functions have HTTP APIs, allowing you to compose arbitrary capabilities into your schema.


In general, if you have a capability exposed as an HTTP/JSON API, you can probably incorporate it into your graph using Connectors. And when you combine many APIs into a single graph, paying attention to endpoints that can resolve entities, you end up with a graph that’s greater than the sum of its parts.


## 💡#11: We’re onto something


We’ve been testing Connectors with preview customers for the better part of a year. The response to Connectors was remarkably positive even in the early stages! More than a few customers surprised us with connector prototypes we didn’t even know they were working on. At least once, a customer built an API specifically for use with Connectors—in their case, adding an endpoint to an existing REST service to use with Connectors was more effective than writing a single resolver!


Cox Auto was an incredible partner during the preview phase of Connectors. Their REST APIs are critical to the business, but they immediately understood the benefit of orchestrating them in GraphQL and Connectors. You can read more about their[journey with Connectors here](https://www.apollographql.com/customers/cox-automotive) .


Also, what could be better than seeing your CEO trying out your product and posting messages like this in Slack:


Wow, this is really cool!!


Update: this is really really cool 😛


OK this is now my preferred way to call REST APIs


## Connectors in practice


As someone who has assisted in dozens of complex GraphQL rollouts, I’m thrilled with how Connectors unlock Federation’s potential to orchestrate all kinds of APIs. Not only that, but developing, deploying, and maintaining them is even easier:


That being said, we know Connectors aren’t done. We’re planning to expand Connector features to cover more kinds of APIs, more use cases, and reduce the amount of code you need to write and maintain even more. Here are some topics that are top-of-mind for me, but we’d love to hear about your requirements.


- The explicitness of the declarative mapping can be repetitive, so we have plans to support something like fragments in the mapping language.
- The query planner fetches entities in batches, which leads to the infamous “N+1” problem. We can solve this with a “declarative dataloader” pattern that’s in the works.
- The Router’s entity caching system is an effective way to improve performance, even for private data. We can integrate` cache-control` headers from REST APIs into that system.
- I’m excited to apply GraphQL best practices like the[errors as data](https://www.apollographql.com/docs/graphos/schema-design/guides/errors-as-data-explained) pattern to REST APIs, turning a` 400 Bad Request` into type-safe schema.
- We started with a system that supports arbitrary, unspecified APIs, but we can provide even more confidence with validations against specifications like OpenAPI.


We’re also paying close attention to the efforts of the GraphQL Working Group to[standardize GraphQL federation](https://github.com/graphql/composite-schemas-wg) . The abstractions that power Connectors are the very same for federated GraphQL subgraphs, so it’s critical that they always work well together. We’re optimistic that there will be opportunities to share work and improve the capabilities of the whole system, benefiting GraphQL- and connector-based implementations alike.


## Get Started Today


[Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors) are now generally available and ready for production use. You can get started for free, because we want every team to experience how much easier API development can be with Connectors.


Whether you’re building a new API or maintaining an existing one:


- Join our[community forum](https://community.apollographql.com/c/connectors/29) to share your experiences
- Take the[Intro to Connectors Odyssey course](https://www.apollographql.com/tutorials/connectors-intro-rest)
- Check out our[documentation for detailed guides](https://go.apollo.dev/connectors)
- Let us know what features you need next via[Apollo Support](https://support.apollographql.com/)


Getting to this point was a monumental effort from dozens of talented Apollonauts. We all shared in 💡moments over the last year realizing this dream. Thank you so much to the Connectors, Router, and Federation teams for all your hard work!


Written by


Lenny Burdette


[Read more by Lenny Burdette](https://www.apollographql.com/blog/author/lenny)
