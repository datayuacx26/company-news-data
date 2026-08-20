---
schema_version: "1.0.0"
document_id: "1c0896fec1d47fca60e8f92e2d7961802624f43399930960485baad245d9f424"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/seamlessly-integrate-with-partners-to-enhance-the-travel-experience"
published_at: "2023-07-14T12:48:06+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:a8a889ee29035bec4b425a1a8c777d1c61c664eeb0a8d6a50f07820dbd76b596"
---

# Seamlessly integrate with partners to enhance the travel experience

This post is a part of our “[How to build connected travel apps with Apollo GraphOS](https://www.apollographql.com/blog/platform/travel/how-to-build-connected-travel-apps-with-apollo-graphos/) ” series. Also in this series:


- [Delivering personalized travel experiences with GraphQL](https://www.apollographql.com/blog/platform/travel/delivering-personalized-travel-experiences-with-graphql/)
- [Ship products faster with SDUI](https://www.apollographql.com/blog/platform/travel/ship-products-faster-with-sdui/)
- [Mitigate scraping and bot attacks with GraphOS](https://www.apollographql.com/blog/platform/travel/mitigate-scraping-and-bot-attacks-with-graphos/)


---


Travel companies often strive to enhance their services by collaborating with third-party providers through API integrations. These integrations enable seamless interactions between different platforms, facilitating a more comprehensive and personalized travel experience for customers. For instance, airlines may partner with ride-share services to share loyalty points or collaborate with hotel booking sites to offer a wide range of accommodation options.


Traditionally, these integrations have been implemented using RESTful APIs. However, REST has limitations when it comes to the visibility of individual field usage and the clients accessing them. Conversely, traditional GraphQL approaches have their own challenges, such as the need to manage multiple GraphQL APIs for different use cases or maintaining a single GraphQL API for both internal and external users.


Fortunately, Apollo GraphOS comes to the rescue, streamlining the entire process of integrating third-party services in the travel industry. With GraphOS, travel companies can leverage the power of[Contracts](https://www.apollographql.com/blog/announcement/platform/contracts-are-now-generally-available/) , demand control features, and field-level metrics to create a seamless and secure integration experience.


## Supporting third-party clients with schema contracts


[In another post](https://www.apollographql.com/blog/platform/travel/delivering-personalized-travel-experiences-with-graphql/) we explored how Apollo Federation allows services to extend types to provide personalized recommendations for hotels and flights. While exposing certain parts of the schema to third-party APIs can be advantageous, there may be instances where we want to exclude specific details, such as user-specific details, from these integrations.


We can leverage Apollo Federation’s` @tag` directive and[GraphOS Contracts](https://www.apollographql.com/docs/graphos/delivery/contracts) to enable us to selectively exclude sensitive data from third-party APIs. However, before we discuss Contracts, it’s important to understand what[graph variants](https://www.apollographql.com/docs/graphos/graphs/#variants) are. A GraphQL service is typically deployed in multiple contexts, such as different deployment stages or configurations (production, development, etc.).


In GraphOS, each graph is designed to have one or more variants, where each variant represents a specific context or configuration of the service. Contracts are a special variant of the graph that’s derived from another variant, known as the source variant. The source variant is a supergraph and Contract variants can be used to create subsets of the overall supergraph. This provides several advantages such as being able to maintain a single source supergraph implementation while also serving different applicable versions of your supergraph to different clients.


Additionally, Apollo Studio’s observability tools such as client awareness and field level usage facilitate tracking and analysis of client usage for these integrations, providing deeper insights into each integration.


## Implementing a contract variant


Our supergraph source variant defines a` User` type with the following fields:


```text
type User @key(fields: "id") {
id: ID!
name: String
email: String
phoneNumber: Int
}
```


This` User` type is required to fetch a list of trips that the given user is on, however providing access to our supergraph to any third party users would enable them to not only fetch a user by their` id` they would be able to fetch any of the given user’s personal information such as their email or phone number.


Even if access to these fields are restricted by our underlying service layer, merely knowing the existence of these fields can constitute data leakage. Ideally, our third party integrations should only be able to fetch a user’s` id` in order to fetch a list of available trips for the user while being unaware of any other details. Without Contracts, you would need to maintain multiple supergraphs for each use case, which drastically complicates the management of your overall graph service.


A much more effective approach involving Contracts can alleviate all these issues. Contracts rely on the` @tag` directive which was introduced as part of a set of[Federation-specific directives](https://www.apollographql.com/docs/federation/federated-types/federated-directives/#tag) . This directive only takes in a single argument, the` name` field and can be used to tag any types and fields in the schema. An annotated version of the` User` type looks like this:


```text
type User @key(fields: "id") {
id: ID!
name: String @tag(name: "internal")
email: String @tag(name: "internal")
phoneNumber: Int @tag(name: "internal")
}
```


Now, we can leverage GraphOS contracts to automatically create a new variant from our source variant to selectively exclude any types marked with the tag “internal”.


Once done, our contract variant will have all of the sensitive fields removed. Comparing the API schemas between the supergraph schema on the left and the contract schema on the right we can see these changes applied to our contract variant:


```text
# Source Variant
type User @key(fields: "id") {
id: ID!
name: String
email: String
phoneNumber: Int
}


# Contract variant after filtering
type User @key(fields: "id") {
id: ID!
}
```


This process has several advantages. First, we retain our original supergraph schema in the source variant and can automatically generate any downstream variants when our source schema changes, providing a single management plane for all our graphs. Secondly, this gives us the ability to ensure that any changes to our supergraph are automatically checked against our Contract variant as well, providing a safety mechanism to prevent any service disruption to any external integrations.


We can also lean on GraphOS’s available observability tooling such as[field level usage](https://www.apollographql.com/docs/graphos/metrics/field-usage) and[client awareness](https://www.apollographql.com/docs/graphos/metrics/client-awareness) , to manage the third party consumers of Contract variant. This allows us to manage deprecations and schema updates with the same mechanisms used for the internal use case.


Finally, having created a partner-specific variant, we can scale it independently from the source variant to cater to specific consumer needs. This enables us to implement personalized authentication, rate limits, and telemetry, ensuring a tailored approach for our partner-specific operations.


## Get started with a travel supergraph today


Beyond integrating with third-party partners, the best way to see the possibilities of a supergraph is to try one out.[You can explore a travel supergraph schema and run real queries against it here](https://studio.apollographql.com/public/apollo-travel-supergraph/variant/prod/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgDMcAZgJwAsATBRALQQCsADB6-T7Va0OANhisAjBmqcA7ACNZMLhUYgANOGjxkaTLgIBRfFgBOAT2JkQMWRDhcMGCawwQYIvhAAcvCPW9PWThaWQ55DHlbRlkQAF8gA&referrer=operation_collections) .


We also have additional posts in this[series of travel best practices](https://www.apollographql.com/blog/platform/travel/how-to-build-connected-travel-apps-with-apollo-graphos/) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern travel applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your financial services experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=travel-solutions-page) .


Written by


Tushar Bhushan


[Read more by Tushar Bhushan](https://www.apollographql.com/blog/author/tushar)
