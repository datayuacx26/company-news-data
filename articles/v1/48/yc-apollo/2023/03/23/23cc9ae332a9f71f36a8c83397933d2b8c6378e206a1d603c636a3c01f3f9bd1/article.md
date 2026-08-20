---
schema_version: "1.0.0"
document_id: "23cc9ae332a9f71f36a8c83397933d2b8c6378e206a1d603c636a3c01f3f9bd1"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/adopt-a-customer-360-approach-to-prevent-fraud-with-graphql"
published_at: "2023-03-27T16:10:33+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:1a4c5cde1d8ade9be890eb195c5f586c5ceb1a06a23db6562780daf3425cd859"
---

# Adopt a Customer 360 approach to prevent fraud with GraphQL

This post is a part of our[“How to power modern financial services apps with Apollo GraphOS”](https://www.apollographql.com/blog/platform/financial-services/how-to-power-modern-financial-services-apps-with-apollo-graphos/) series. Also in this series:


- [Deliver a personalized customer experience with GraphQL](https://www.apollographql.com/blog/platform/financial-services/deliver-a-personalized-customer-experience-with-graphql/)
- [Securing APIs declaratively with GraphQL](https://www.apollographql.com/blog/platform/financial-services/directive-based-authorization-for-financial-services/)
- [Ensure compliant and secure third-party access using GraphOS](https://www.apollographql.com/blog/platform/financial-services/ensure-compliant-and-secure-third-party-access-using-graphos/)


---


Customers are the cornerstone of any business, but many companies don’t truly know who their customers are. KYC, or Know Your Customer, is an important process for financial institutions and businesses to verify their customers’ identities and assess the associated risk factors. As part of a larger Anti-Money Laundering (AML) program, KYC helps prevent financial crimes.


A single customer view (SCV) provides a complete picture of a customer’s data, including personal and financial information, transaction history, and behavior patterns. SCV can help businesses comply with KYC and AML regulations; however, one of the common challenges associated with implementing SCV is data integration. Aggregating data from multiple sources, such as disparate internal systems and third-party services, can be complex and time-consuming. But GraphQL and Apollo GraphOS can help.


## Assigning a risk rating to customers


A financial institution often needs to assess the risk associated with each customer. A KYC risk rating is a calculation based on a number of factors, including the customer’s identity. In an organization, basic customer metadata might be owned by the Identity team, while the ownership of risk assessments might be under the remit of the Compliance team. How do you create a holistic view of a customer?


In a federated graph, an entity is an object type that can resolve its fields across multiple subgraphs. Consider a users subgraph owned by the Identity team that contains the following type definition for` User` :


```text
# Users subgraph


type User @key(fields: “id”) {
id: ID!
name: String
}
```


By setting the` id` as the key field using the` @key` directive, we have turned the` User` object type into an entity so any other subgraph can now add fields to the` User` type.


We now need to add a` riskRating` field to the` User` type. To accommodate this, the Compliance team can create a new subgraph and extend the` User` like so:


```text
# Risk subgraph


type User @key(fields: “id”) {
id: ID!
riskRating: Int
}
```


Once this new subgraph is composed into a GraphOS-powered supergraph, any developer will be able to retrieve the needed customer metadata, including their risk rating. The Compliance team can continue to make improvements to risk assessment without impacting the Identity team or other using the supergraph.


## Simplifying AML checks


By creating a unified data experience, financial institutions can simplify execution of AML procedures. To create a comprehensive view of the customer’s financial behavior, data such as transactions, credit reports, and public records must be gathered and examined. This data set can then provide insights into the customer’s spending patterns and financial history. Based on the results, appropriate actions can be taken to mitigate risks.


Using GraphQL, we are able to craft a single query that retrieves the needed data from three discrete subgraphs (i.e. users, accounts, transactions). Each subgraph is powered by a[Federation-compatible](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs) GraphQL server which in turn can fetch data from any underlying source, such as a legacy REST API or a database. For example:


```text
query GetAllUserAccountsTransactions {
user {
id
name
accounts {
id
type
balance
transactions {
id
description
amount
}
}
}
}
```


One of the biggest advantages of GraphQL is that it allows developers to specify exactly the data they need. This means that clients avoid overfetching (receiving more data than they need) and underfetching (not receiving enough data to fulfill their needs). The result is increased developer velocity and improvement of network usage.


## Get started with a financial services supergraph today


Beyond onboarding third-party partners to supergraph, the best way to see the possibilities of a supergraph is to try one out.[You can explore a financial services supergraph schema and run real queries against it here.](https://studio.apollographql.com/public/apollo-financial-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgwQBmEARgJyYC0ArCwIwZMAs8ET9DDADsTDAyoAmdsIDMcYXAwgANOGjxkaTLgIBRfFgBOAT2JkQtbhgAckmPW5MADHG42eLDPSYQYVKiZZSUlaCAA2WRsbdl4QAF8gA&referrer=operation_collections&variant=prod)


We also have additional posts in this[series of financial services best practices](https://www.apollographql.com/blog/platform/financial-services/how-to-power-modern-financial-services-apps-with-apollo-graphos/) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern financial applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your financial services experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=finserv-solutions-page) .


Written by


Val Polouchkine


[Read more by Val Polouchkine](https://www.apollographql.com/blog/author/val)
