---
schema_version: "1.0.0"
document_id: "5761597d1a61633b5454411efa5bd8f575626a52b5b8ca2ca7e606dc182ae9d5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/ensure-compliant-and-secure-third-party-access-using-graphos"
published_at: "2023-03-20T09:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:d12a9a5a570f73724594a91b35d1d01f6b51f5e91b1996aede1416fb9283d5ff"
---

# Ensure compliant and secure third-party access using GraphOS

This post is a part of our[“How to power modern financial services apps with Apollo GraphOS”](https://www.apollographql.com/blog/platform/financial-services/how-to-power-modern-financial-services-apps-with-apollo-graphos/) series. Also in this series:


- [Deliver a personalized customer experience with GraphQL](https://www.apollographql.com/blog/platform/financial-services/deliver-a-personalized-customer-experience-with-graphql/)
- [Securing APIs declaratively with GraphQL](https://www.apollographql.com/blog/platform/financial-services/directive-based-authorization-for-financial-services/)
- [Adopt a Customer 360 approach to prevent fraud with GraphQL](https://www.apollographql.com/blog/platform/financial-services/adopt-a-customer-360-approach-to-prevent-fraud-with-graphql/)


---


In recent years, GraphQL has gained popularity as a powerful alternative to REST APIs for building modern web applications. One area where GraphQL shines is in providing compliant and secure third-party access to data.


Financial services companies gather extremely sensitive information from their customers and must comply with data privacy regulations such as the[General Data Protection Regulation (GDPR)](https://gdpr-info.eu/) , the[California Consumer Privacy Act (CCPA)](https://oag.ca.gov/privacy/ccpa) and other federal and state privacy laws. In addition, they must protect data from unauthorized access and can do so ensuring compliant and secure third-party access with the help of[Apollo GraphOS](https://www.apollographql.com/docs/graphos/) .


## Compliance


GraphOS provides out of the box features to help protect against unintended data leaks and can help to narrow the scope of data which can be exposed through the use of schema contracts.


[Schema contracts](https://www.apollographql.com/docs/graphos/delivery/contracts/) allow developers to specify exactly which fields and types within a schema that applications are allowed to access. By using the` @tag` directive provided by Apollo Federation, developers can expose specific capabilities from the supergraph to individual use cases.


Most often financial companies use contracts to separate functionality between their customer use cases such as web and mobile, internal applications, and external partners.


```text
type Account {
id: ID!
balance: Int @tag(name: "mobile")
transactions: [Transaction] @tag(name: "mobile") @tag(name: "partners")
user: User
}
```


The flexibility of schema contracts allows for fine-grained control over data exposure, which is crucial for compliance. With GraphOS, a schema can be defined that specifies the exact fields and types of data that can be queried or mutated. This means that third-party applications can only access data that is explicitly allowed by the contract, making it easier to comply with regulations such as GDPR’s “data minimization” principle.


GraphOS further enhances the security which can be applied to a schema with Apollo Studio by enforcing user roles, restricting access to protected variants and limiting the visibility of graphs within the organization.


## GraphOS Security


Security is at the core of GraphOS and leverages built-in enterprise authentication features in Apollo Router and Apollo Studio.


Apollo Router supports multiple identity providers, and JWT (JSON Web Token) verification to ensure that only authenticated clients can access the supergraph. With built-in support for JWT claim extraction which removes the need for subgraphs to be extracting, parsing and validating claims on each subgraph request.


Schema contracts are derived from a source[schema variant](https://www.apollographql.com/docs/graphos/graphs/overview/#variants) and enforce inclusion/exclusion policies on the fields and types which are exposed through the contracts. Different authentication configurations can be deployed for each schema contract through its own instance of Apollo Router, which makes GraphOS capable of supporting many different use cases all powered by a single unified schema.


Enterprise users of GraphOS can enforce user roles within Apollo Studio both at the organization and graph levels which can be used to restrict write access to different graphs by role. While graph variants can also be protected to ensure only authorized users can make changes to the schema. GraphOS Enterprise also enables additional roles for billing managers, technical document writers, and other members of the organization. These features can be used together to provide multiple security layers to your graph for development, management and operations.


GraphOS provides a secure, compliant solution for financial companies to enable third-party access to customer data while remaining compliant with regulations like GDPR and CCPA. GraphOS Enterprise features provide fine-grained control over data exposure through schema contracts and built-in authentication features to protect sensitive data.


## Get started with a financial services supergraph today


Beyond onboarding third-party partners to supergraph, the best way to see the possibilities of a supergraph is to try one out.[You can explore a financial services supergraph schema and run real queries against it here.](https://studio.apollographql.com/public/apollo-financial-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgwQBmEARgJyYC0ArCwIwZMAs8ET9DDADsTDAyoAmdsIDMcYXAwgANOGjxkaTLgIBRfFgBOAT2JkQtbhgAckmPW5MADHG42eLDPSYQYVKiZZSUlaCAA2WRsbdl4QAF8gA&referrer=operation_collections&variant=prod)


We also have additional posts in this[series of financial services best practices](https://www.apollographql.com/blog/platform/financial-services/how-to-power-modern-financial-services-apps-with-apollo-graphos/) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern financial applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your financial services experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=finserv-solutions-page) .


Written by


Matthew Ratzke


[Read more by Matthew Ratzke](https://www.apollographql.com/blog/author/mratzke)
