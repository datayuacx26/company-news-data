---
schema_version: "1.0.0"
document_id: "7fac92229dc50effd5410e944c4eb38b48797919dab2310d391c518e0ac8d7a1"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/deliver-a-personalized-customer-experience-with-graphql"
published_at: "2023-03-27T16:18:40+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:d243cd1a3f1c705b26bd28201a8a820e5df3300b6b12e2a2621c5d7ff5f8c06a"
---

# Deliver a personalized customer experience with GraphQL

This post is a part of our[“How to power modern financial services apps with Apollo GraphOS”](https://www.apollographql.com/blog/platform/financial-services/how-to-power-modern-financial-services-apps-with-apollo-graphos/) series. Also in this series:


- [Securing APIs declaratively with GraphQL](https://www.apollographql.com/blog/platform/financial-services/directive-based-authorization-for-financial-services/)
- [Adopt a Customer 360 approach to prevent fraud with GraphQL](https://www.apollographql.com/blog/platform/financial-services/adopt-a-customer-360-approach-to-prevent-fraud-with-graphql/)
- [Ensure compliant and secure third-party access using GraphOS](https://www.apollographql.com/blog/platform/financial-services/ensure-compliant-and-secure-third-party-access-using-graphos/)


---


Modern customers are increasingly knowledgeable about finance—they demand faster and more personalized services that require financial services organizations to reconsider how they leverage data and technology to offer a holistic customer experience across all digital touch points. Unfortunately, many companies and institutions face obstacles to scaling developer velocity due to legacy architecture decisions and inefficient development processes.


A GraphOS-powered supergraph can be used to aggregate customer data from multiple sources to provide a singular, strongly-typed API that can power all client applications that deliver personalized experiences to customers.


## A unified interface


Mobile clients—often developed as follow-ups to the original web offering—may require data that are scattered across a complex array of services. These services may expose their data via rigid APIs that, more often than not, return additional data that the clients do not need. By using GraphQL’s flexible querying capabilities, teams can easily retrieve and combine customer data from different systems and present it in a way that is tailored to specific users’ needs.


Consider the need to show the user all of their accounts on iOS, Android, and web. One approach would be to call all of the necessary backend services to build the UI:


```text
GET /api/users?userID=123
GET /api/accounts?userID=123
…
```


Using this approach, we will likely generate many requests, get data that we did not ask for, and ultimately waste time and compute resources. Another approach could be to introduce facades (also called backends-for-frontends or BFFs) in front of the backend services for each client so that a consolidate call can be made like this:


```text
GET /api/getUserAccounts?client=iOS&userID=123
```


But, this too introduces challenges, such as the need to maintain duplicate code between discrete facades.


Let’s take a look at the GraphQL alternative. The` GetAllUserAccounts` query retrieves specific user and account data. Behind the scenes, the` user` and` accounts` objects live in two subgraphs thus encouraging the design principle of separation of concerns.


```text
query GetAllUserAccounts {
user {
id
name
accounts {
id
name
}
}
}
```


With GraphQL, developers can use a single query to retrieve only the necessary data for a feature, which can significantly reduce the number of requests required and improve the performance of mobile applications. This can help to ensure that all users, regardless of the device they are using, have a consistent and reliable experience.


## Smart resolvers


In order to respond to client requests, GraphQL servers need to retrieve the relevant data. To achieve this, they leverage[resolvers](https://www.apollographql.com/docs/apollo-server/data/resolvers/) that are defined for each field in the schema and are executed in a specific order based on the relationships between fields. A resolver can retrieve data from a database, a REST API, or any other data source.


Resolvers can provide personalized results as well. For example, in this` user` query, the user ID is accessed via a` context` object that’s available as a parameter in all field resolver functions. This ID value would have been exposed in an HTTP header and manually applied to the` context` object when the GraphQL request initially reached the server. In a real production application, this would come from an auth provider and would be extracted from a JWT or a similar mechanism.


The ID value can then be used to retrieve the user from the relevant data source (or throw and error if the ID is invalid) and a client developer can use the data in the GraphQL response to tailor that user’s experience in a front-end application:


```text
export const resolvers = {
Query: {
user(_, __, context) {
const userId = context.headers["x-user-id"];
const user = getUserById(userId);


if (!user) {
throw new GraphQLError("Could not locate user by id. Please specify a valid `x-user-id` header like `user:1`");
}


return user;
},
},
};
```


GraphQL resolvers provide a flexible and efficient way to retrieve and aggregate customer data from multiple sources—they don’t always have to return the same data and can be customized with additional functionality.


## Get started with a financial services supergraph today


Beyond onboarding third-party partners to supergraph, the best way to see the possibilities of a supergraph is to try one out.[You can explore a financial services supergraph schema and run real queries against it here.](https://studio.apollographql.com/public/apollo-financial-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgwQBmEARgJyYC0ArCwIwZMAs8ET9DDADsTDAyoAmdsIDMcYXAwgANOGjxkaTLgIBRfFgBOAT2JkQtbhgAckmPW5MADHG42eLDPSYQYVKiZZSUlaCAA2WRsbdl4QAF8gA&referrer=operation_collections&variant=prod)


We also have additional posts in this[series of financial services best practices](https://www.apollographql.com/blog/platform/financial-services/how-to-power-modern-financial-services-apps-with-apollo-graphos/) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern financial applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your financial services experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=finserv-solutions-page) .


Written by


Val Polouchkine


[Read more by Val Polouchkine](https://www.apollographql.com/blog/author/val)
