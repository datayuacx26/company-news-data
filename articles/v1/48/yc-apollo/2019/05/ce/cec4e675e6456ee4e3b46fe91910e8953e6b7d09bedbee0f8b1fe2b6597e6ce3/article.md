---
schema_version: "1.0.0"
document_id: "cec4e675e6456ee4e3b46fe91910e8953e6b7d09bedbee0f8b1fe2b6597e6ce3"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-federation-f260cf525d21"
published_at: "2019-05-01T22:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:6267e263ed019ec1d501ec7016912e879e1c0f10b1380c7481afda3ab58932b6"
---

# Apollo Federation

What if you could access all of your organization’s data by typing a single GraphQL query, even if that data lived in separate places? Up until now, this goal has been difficult to achieve without committing to a monolithic architecture or writing fragile schema stitching code.


Ideally, we want to expose[one graph](https://principledgraphql.com/integrity#1-one-graph) for all of our organization’s data without experiencing the pitfalls of a monolith. What if we could have the best of both worlds: a complete schema to connect all of our data with a[distributed architecture](https://principledgraphql.com/integrity#2-federated-implementation) so teams can own their portion of the graph?


That dream is finally a reality with the launch of[Apollo Federation](https://www.apollographql.com/docs/apollo-server/federation/introduction/) , an architecture for building a distributed graph. Best of all, it’s open source! 🚀[Try it out now](https://github.com/apollographql/federation-demo) or keep reading to learn more.


## Introducing Apollo Federation


[Apollo Federation](https://www.apollographql.com/docs/apollo-server/federation/introduction/) is our answer for implementing GraphQL in a microservice architecture. It’s designed to replace schema stitching and solve pain points such as coordination, separation of concerns, and brittle gateway code.


Federation is based on these core principles:


- **Building a graph should be declarative.** With federation, you compose a graph declaratively from within your schema instead of writing imperative schema stitching code.
- **Code should be separated by concern, not by types.** Often no single team controls every aspect of an important type like a User or Product, so the definition of these types should be distributed across teams and codebases, rather than centralized.
- **The graph should be simple for clients to consume.** Together, federated services can form a complete, product-focused graph that accurately reflects how it’s being consumed on the client.
- **It’s just GraphQL, using only spec-compliant features of the language.** Any language, not just JavaScript, can implement federation.


Apollo Federation is inspired by years of conversations with teams that use GraphQL to power their most important projects. Time and time again, we’ve heard from developers that they need an efficient way to connect data across services. Let’s learn how in the next section!


## Core concepts


Connecting data between multiple services is where federation truly shines. The two essential techniques for connecting data are type references and type extensions. A type that can be connected to a different service in the graph is called an[entity](https://www.apollographql.com/docs/apollo-server/federation/core-concepts/#entities-and-keys) , and we specify directives on it to indicate how both services should connect.


### [Type references](https://www.apollographql.com/docs/apollo-server/federation/core-concepts/#referencing-external-types)


With Apollo Federation, we can declaratively reference types that live in different schemas. Let’s look at an example where we have two separate services for accounts and reviews. We want to create a` User` type that is referenced by both services.


In the accounts service, we use the` @key` directive to convert the` User` type to an entity. With only one line of code, we’ve indicated that the` User` type can be connected from other services in the graph through its` id` field.


```text
type User @key(fields: "id") {
id: ID!
username: String
}
```


In the reviews service, we can return the` User` type even though it’s defined in a different service. Thanks to type references, we can seamlessly connect the accounts and reviews service in one complete graph.


```text
type Review {
author: User
}
```


On the client, you can query the data as if both types were defined in the same schema. You no longer have to create extra field names like` authorId` or make separate round trips to a different service.


```text
query GetReviews {
reviews {
author {
username
}
}
}
```


### [Type extensions](https://www.apollographql.com/docs/apollo-server/federation/core-concepts/#extending-external-types)


To enable organizing services by concern, a service can` extend` a type defined in a different service to add new fields.


In the reviews service, we extend the` User` entity from the accounts service. First, we use the` @key` directive to specify that these two services should connect together on the` id` field defined in the` @external` accounts service. Then, we add the functionality for a user to fetch their reviews.


```text
extend type User @key(fields: "id") {
id: ID! @external
reviews: [Review]
}
```


The client only sees the final` User` type, which includes all of the fields merged together. Type extensions for Apollo Federation will feel familiar if you’ve used Apollo Client for[local state management](https://blog.apollographql.com/announcing-apollo-client-2-5-c12230cabbb7) because they share the same programming model.


### Production-ready features


We designed Apollo Federation to be approachable to start with, yet capable of handling the most complex real-world use cases:


- [Multiple primary keys](https://www.apollographql.com/docs/apollo-server/federation/advanced-features/#multiple-primary-keys) **:** It’s not always practical to reference an entity with a single primary key. With Apollo Federation, we can reference types with multiple primary keys by adding several` @key` directives.
- [Compound primary keys](https://www.apollographql.com/docs/apollo-server/federation/advanced-features/#compound-and-nested-keys) **:** Keys aren’t limited to a single field. Rather, they can be any valid selection set, including those with nested fields.
- [Shortcuts for faster data fetching](https://www.apollographql.com/docs/apollo-server/federation/advanced-features/#using-denormalized-data) **:** In large organizations, it’s common to denormalize data across multiple services. With the` @provides` directive, services can use that denormalization to their advantage to provide shortcuts for fetching data faster.


## Architecture


An example of a federated architecture


To implement Apollo Federation, you need two components:


- **Federated services** , which are standalone GraphQL APIs. You can connect data between them by extending types and creating references with the` @key` directive.
- A **gateway** to compose the complete graph and execute federated queries


Setting up federation with Apollo Server doesn’t require any manual stitching code. Once you have your services, you can spin up a gateway simply by passing them as a list:


```text
const { ApolloGateway } = require("@apollo/gateway");


const gateway = new ApolloGateway({
serviceList: [
{ name: "accounts", url: "https://pw678w138q.sse.codesandbox.io/graphql" },
{ name: "reviews", url: "https://0yo165yq9v.sse.codesandbox.io/graphql" },
{ name: "products", url: "https://x7jn4y20pp.sse.codesandbox.io/graphql" },
{ name: "inventory", url: "https://o5oxqmn7j9.sse.codesandbox.io/graphql" }
]
});
```


The gateway crunches all of this into a single schema, indistinguishable from a hand-written monolith. To any client consuming a federated graph, it’s just regular GraphQL.


We didn’t want the gateway to be a black box where federated schemas and queries magically turn into data. To give teams full visibility into what’s happening with their composed graph, Apollo Server provides a query plan inspector out of the box:


The query plan inspector


## Try Apollo Federation today! 🚀


We’re excited to open source Apollo Federation and get feedback from the community. To get started, we’ve created an[example repo](https://github.com/apollographql/federation-demo) that demonstrates how to build a federated graph with four services and a gateway. If you want to play with the example in the browser,[check out this CodeSandbox](https://codesandbox.io/s/v368r9ml47) .


Along with the example app, we added a new section to the Apollo Server docs about federation.[Read the docs](https://www.apollographql.com/docs/apollo-server/federation/introduction/) to get up to speed quickly and[migrate over from schema stitching](https://www.apollographql.com/docs/apollo-server/federation/migrating-from-stitching/) .


## What’s next


This release is the first chapter of distributed GraphQL. We can’t wait to hear about the many exciting ideas that stem from federation and see the amazing things you build. In the future, we’re eager to work with the GraphQL community to support new features and tools such as:


- Additional languages (e.g. Java, Scala, Python) for federated services. If you’re a server maintainer, please read our[federation spec](https://www.apollographql.com/docs/apollo-server/federation/federation-spec/) to learn about adding support and reach out to us with any questions on[Spectrum](https://spectrum.chat/apollo/apollo-federation?tab=posts) .
- More federation-aware tooling, such as adding federation directive support to the[Apollo VS Code](https://marketplace.visualstudio.com/items?itemName=apollographql.vscode-apollo) extension
- Service extensions for passing additional metadata back to the gateway
- Support for subscriptions and` @defer`


We’d love to hear your feedback on Apollo Federation.[Join the conversation on Spectrum](https://spectrum.chat/apollo/apollo-federation?tab=posts) and let us know what you think!


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
