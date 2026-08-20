---
schema_version: "1.0.0"
document_id: "3290a6d794a5c3c5808e8e0bccea11fd1146c9b119ad468ffd7b4c4d2ad8cd7a"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/directive-based-authorization-for-financial-services"
published_at: "2023-03-10T20:42:46+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:f49e3e1cbc00e57aafb0e3be9c1ed8a25518a17a0966796a7dbc8f3042d78abb"
---

# Securing APIs declaratively with GraphQL

This post is a part of our[“How to power modern financial services apps with Apollo GraphOS”](https://www.apollographql.com/blog/platform/financial-services/how-to-power-modern-financial-services-apps-with-apollo-graphos/) series. Also in this series:


- [Deliver a personalized customer experience with GraphQL](https://www.apollographql.com/blog/platform/financial-services/deliver-a-personalized-customer-experience-with-graphql/)
- [Adopt a Customer 360 approach to prevent fraud with GraphQL](https://www.apollographql.com/blog/platform/financial-services/adopt-a-customer-360-approach-to-prevent-fraud-with-graphql/)
- [Ensure compliant and secure third-party access using GraphOS](https://www.apollographql.com/blog/platform/financial-services/ensure-compliant-and-secure-third-party-access-using-graphos/)


---


## Update: Auth now available in Apollo Router


Apollo has shipped our new authentication and authorization features as built-in Router features. Checkout the launch post:[https://www.apollographql.com/blog/graphql/security/enforcing-graphql-security-best-practices-with-graphos/](https://www.apollographql.com/blog/graphql/security/enforcing-graphql-security-best-practices-with-graphos/)


In the financial services industry, securing access to APIs and data is of the utmost importance. These are not just best practices but regulatory standards enforced by governments across the world. GraphQL helps developers create flexible APIs that allow clients to query for exactly what they need, which may lead to the question of whether GraphQL can be just as secure as other API technologies?


The good news is that you can absolutely secure your data using a declarative approach with GraphQL directives. What’s more, this approach can be self documenting for API clients and run any at scale to serve all types of FinTech customers.


## What are Directives in GraphQL?


[GraphQL schema directives](https://www.apollographql.com/docs/apollo-server/schema/directives/) are special annotations that can be added to a GraphQL schema to modify or add additional logic to the runtime behavior. Directives can be used to control things like data retrieval, schema metadata, and even security. For example:


```text
directive @myDirective(arg1: String = ‘foo’) on FIELD_DEFINITION


type Query {
hello: String @myDirective
}
```


By defining` @myDirective` in the schema with a valid location of` FIELD_DEFINITION` , this directive can be applied to an object field like the` hello` field on the root` Query` type. At runtime, custom logic can be applied to the` hello` field now to alter its output. And just like field definitions, directives can accept typed arguments with optional defaults (like` arg1` above) to further alter the runtime output of any field where it’s applied.


## Authentication


Authentication is the process of verifying that a user is who they claim to be. A schema might have some fields that require authentication and some that do not, so GraphQL directives can be used to enforce authentication for specific fields in the schema.


A common approach to doing this requires the use of a custom type system directive that checks for the presence of a valid token in the request header, typically a[JWT](https://jwt.io/) . If a token is present, it can be decoded to determine the user’s identity, and the request can be processed as normal. If no token is present, the directive can return an error, indicating that an authentication header is required.


```text
directive @authenticated repeatable on OBJECT | FIELD_DEFINITION


type Query {
bankAccounts: BankAccountsResponse @authenticated
}


```


## Authorization


Authorization is the process of determining whether a specific user is allowed to access a specific set of data. GraphQL directives can also be used to enforce authorization for specific fields or types in the schema.


Similar to authentication, a custom schema-defined directive can be used to check the user’s role and permissions from the user token before executing the field resolver. If the current user’s role matches the one specified in the directive’s` role` argument, then the request will be processed normally, otherwise, the directive can throw an error and indicate that the user is not authorized to access the specific field or type. For example:


```text
directive @hasRole(role: Role) on FIELD_DEFINITION


enum Role {
ADMIN
USER
}


type Query {
bankAccounts: [BankAccount] @authenticated @hasRole(role: ADMIN)
}
```


In this example, the` @hasRole` directive is added to the accounts field and the required role is set to` ADMIN` . This means that any request to this field must include a valid token in the request header *AND* the user must have the` ADMIN` role. If the user is authenticated but their role is less privileged than an` ADMIN` , then the directive can throw an error.


These directive names are just examples. You can find many other similar implementations or create your own, but if you want to see a working example, check out the Apollo Solutions[simple-auth-directive GitHub repository](https://github.com/apollosolutions/simple-auth-directive) .


## Benefits of a Directive-Based AuthN/Z


By using directives to implement authentication and authorization, the security logic is centralized in the same repeatable directive, making it easier to manage, maintain, and enforce across your entire API surface area. Because the logic is abstracted into reusable directives, there’s no need to replicate it field-by-field where escalated privileges are required to view the resolved data. Securing your API is as simple as annotating the fields that require AuthN/Z.


Implementing a standard directive library in conjunction with a[Federated architecture](https://www.apollographql.com/docs/federation) allows teams to implement the same logic across fields in multiple subgraph services that run behind Apollo Router. This means that bug fixes can be shipped faster when security incidents are identified and new capabilities can be added centrally while allowing subgraph teams to incrementally adopt the new requirements when they are ready. This pattern is also generic enough that you can abstract the validation logic to any existing 3rd-party identity provider (IdP) but keep the schema-based annotations as a common abstraction layer.


## Self-Documenting Policies


Using a declarative, schema-based authentication flow also facilitates sharing that policy with other teams. Using the[@composeDirective](https://www.apollographql.com/docs/federation/federated-types/federated-directives/#composedirective) , we can expose this metadata to our supergraph so it is visible in Apollo Studio and instead of doing an in-depth code audit, you can bring in other internal teams from Security, IT, or DevOps to view the currently defined rules and invite them to review new additions with much less effort. From the client developer’s perspective, it also empowers them to understand API access requirements as they work with the schema and without relying on additional internal documentation or meetings.


---


## Get started with a financial services supergraph today


Beyond securing your data, the best way to see the possibilities of a supergraph is to try one out.[You can explore a financial services supergraph schema and run real queries against it here.](https://studio.apollographql.com/public/apollo-financial-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgwQBmEARgJyYC0ArCwIwZMAs8ET9DDADsTDAyoAmdsIDMcYXAwgANOGjxkaTLgIBRfFgBOAT2JkQtbhgAckmPW5MADHG42eLDPSYQYVKiZZSUlaCAA2WRsbdl4QAF8gA&referrer=operation_collections&variant=prod)


We also have additional posts in this[series of financial services best practices](https://www.apollographql.com/blog/platform/financial-services/how-to-power-modern-financial-services-apps-with-apollo-graphos/) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern financial applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your financial services experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=finserv-solutions-page) .


Written by


Shane Myrick


[Read more by Shane Myrick](https://www.apollographql.com/blog/author/shane)
