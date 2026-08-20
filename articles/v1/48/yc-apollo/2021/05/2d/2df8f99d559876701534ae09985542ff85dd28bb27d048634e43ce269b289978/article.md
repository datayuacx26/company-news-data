---
schema_version: "1.0.0"
document_id: "2df8f99d559876701534ae09985542ff85dd28bb27d048634e43ce269b289978"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-83888f119c8c"
canonical_url: "https://www.apollographql.com/blog/9-ways-to-secure-your-graphql-api-security-checklist"
published_at: "2021-05-26T11:03:58+00:00"
first_seen_at: "2026-07-24T16:23:53.794779+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:15a539b2c111de9c24c5fa8b95e1affdc8554bc30a87380e34dcfd98431b3920"
---

# 9 Ways To Secure your GraphQL API — GraphQL Security Checklist

So you’ve built out your GraphQL API, and you’re nearly ready to deploy it to production. Fantastic. Let’s talk security: a significant part of every stable application on the internet. It’s worth taking some time to mitigate the most common issues.


In this article, we will explore three main areas for how to secure your GraphQL API. We’ll learn how to set up a reasonable auth strategy and limit the GraphQL attack surface area. Finally, we’ll cover how to use Apollo Studio to enhance your application performance monitoring and provide more secure internal access to your production data.


## Auth


### 1. Authentication


Authentication is determining whether a given user is logged in and subsequently remembering who they are. Authentication can provide context to a session and personalize the type of data that a user sees.


### 2. Authorization


Authorization is then determining what a given user has *permission* to do or see. In GraphQL, we’d use this to manage access to particular queries and mutations based on identity, role, or permissions.


### Getting started with auth


We recommend using[JSON Web Tokens (or JWTs)](https://jwt.io/introduction) to manage user auth. JWTs provide a mechanism that enables us to determine whether a token is valid, and thus — if a user is authenticated. JWTs also allow us to encode permissions about a user; we use permissions to restrict or permit GraphQL operations.


To learn how to set up authentication and authorization in an Apollo Server instance, read the official[authentication & authorization docs](https://www.apollographql.com/docs/apollo-server/security/authentication/) .


Setting up auth in the context of a federated data graph requires some special considerations to receive and verify access tokens at the gateway level of the API and forward them to implementing services to manage access to its queries. Read “[Setting Up Authentication and Authorization with Apollo Federation](https://www.apollographql.com/blog/backend/auth/setting-up-authentication-and-authorization-apollo-federation/) ” on the Apollo blog to learn more about this.


## Reducing your GraphQL API attack surface area


Even with authentication and authorization, the attack surface area is still sufficiently large. In this section, we’ll cover techniques to protect both the performance of your graph and the data behind it.


### 3. Mitigate malicious queries


**Limit query depth**


GraphQL gives clients the ability to ask for data in a variety of different ways. Because of the various entry-points available to request data, it’s possible to write exceptionally large nested queries like the following.


```text
query {
author(id: 42) {
posts {
author {
posts {
author {
posts {
author {
# and so on...
}
}
}
}
}
}
}
}
```


Queries like this are dangerous because they’re expensive to compute. They could crash our API and take up all available resources.


We recommend using a library like[graphql-depth-limit](https://github.com/stems/graphql-depth-limit) to specify the max depth across your queries to mitigate this problem.


**Paginate list fields where appropriate**


Query *depth* isn’t the only thing to worry about. We should also be conscious of how query *amount* could affect the performance of our API.


In the following example, if there were 10 authors, each with 100 posts, this query would attempt to return 100,000 nodes.


```text
query {
authors(first: 1000) {
name
posts(last: 100) {
title
content
}
}
}
```


Running a query like this would undoubtedly slow down (if it doesn’t DoS) your server.


To prevent this from happening, we recommend using pagination when appropriate, capping the input number in your resolver, or using a library like[graphql-input-number](https://github.com/joonhocho/graphql-input-number) to limit the possible input size.


**Improve validation and sanitization**


Validation and sanitization are standard web application security practices. When you accept data from a user, one should always expect that user-provided data could be malicious.


There are two especially malicious techniques in this area: *data exfiltration* and *data destruction* .


Data exfiltration is when a client writes a malicious query containing SQL or NoSQL code that tricks the database into returning more data than originally intended. For example, the following un-sanitized query could trick a SQL-based data source into returning all of the fields for a user, including their email and hashed password.


```text
query User {
user (id: "User*") {
email
id
}
}
```


The other technique (data destruction) is when a client writes a malicious query — again containing some database-layer code — that can destroy production data when executed.


There are various ways to prevent this vulnerability. We recommend following the usual rules for web application sanitization in addition to the[OSWAP GraphQL-specific recommendations](https://cheatsheetseries.owasp.org/cheatsheets/GraphQL_Cheat_Sheet.html#general-practices) like:


- Reject invalid input without giving away too many details
- Leverage the GraphQL schema to support validation
- Beware of using JSON scalars (they are a lot more prone to malicious queries if not properly sanitized —[read here](http://www.petecorey.com/blog/2017/06/12/graphql-nosql-injection-through-json-types/) )


**Use timeouts**


When we request data from downstream services or data sources, there are various reasons why it may take a long time to respond. The services may be down, queries may be expensive, or something else might be going on. Regardless of the reason, we don’t want our GraphQL API to hang for too long, waiting for a response.


To prevent this, we recommend using timeouts to keep from slow or unresponsive services impacting performance for subsequent queries.


In a federated context, you could use a fetcher pattern as follows:


```text
const gateway = new ApolloGateway({
// ...
buildService({ name, url }) {
// Sets a 3 second timeout on requests
// to subgraph
const fetcher = (input, init) => {
if (init) {
init.timeout = 3000;
} else {
init = { timeout: 3000 };
}
return fetch(input, init);
};
return new RemoteGraphQLDataSource({ url, fetcher });
}
});
```


It’s worthwhile to explore other places to use timeouts as well:


- On requests to the gateway’s Node HTTP server
- On requests to the subgraphs services
- On resolver functions (and using REST data sources)


**Rate limit APIs**


Rate limiting is when you dictate how many requests a client can make per some time. Often, we use rate-limiting to prevent brute-forcing login details, scraping data, or denial of service attacks.


To implement this, we recommend two approaches:


- GitHub’s approach: Maximum node limit and a rate limit score based on the total number of requests in a query ([read about it here](https://docs.github.com/en/graphql/overview/resourcelimitations) )
- Shopify’s approach: Query cost points and the leaky bucket algorithm ([read about it here](https://shopify.dev/concepts/about-apis/rate-limits) )


**Query cost analysis**


Despite our best efforts using query depth and amount limiting techniques, it’s still possible to overload the server with semantically expensive queries. Sometimes we can’t just look at the depth or potential amount of nodes.


For example, at Spectrum, the following query would be expensive:


```text
query evilQuery {
thread(id: "54887141-57a9-4386-807c-ed950c4d5132") {
messageConnection(first: 100) { ... }
participants(first: 100) {
threadConnection(first: 100) { ... }
communityConnection { ... }
channelConnection { ... }
everything(first: 100) { ... }
}
}
}
```


We recommend the[query-cost-analysis GitHub package](https://github.com/pa-bru/graphql-cost-analysis) as a way to analyze queries for complexity and block them if they’re too expensive.


To learn more on this topic, read “[Securing Your GraphQL API from Malicious Queries](https://www.apollographql.com/blog/graphql/security/securing-your-graphql-api-from-malicious-queries/) ” and “A Principled Approach to GraphQL Query Cost Analysis ([paper/video](https://arxiv.org/pdf/2009.05632.pdf) ).”


**Safelist operations**


A catchall approach for preventing unwanted queries is to maintain a list of approved queries allowed in your application.


Automatic persisted queries are a feature of Apollo Server 2 that enables query whitelisting and persisted queries.


To learn more, read the[automatic persisted queries docs](https://www.apollographql.com/docs/apollo-server/performance/apq/) .


### 4. Limit API discoverability


This next section is about security by obscurity — a way to reduce the attack surface by making it harder for malicious parties to discover API capabilities.


**Turn off introspection in production**


Introspection is a technique to provide detailed information about a GraphQL API’s schema.


While introspection is primarily helpful for diagnostic and GraphQL tooling in development, we recommend turning it off in production. Doing so prevents accidentally sharing business secrets and vastly reduces the ability of a malicious actor to discover how to abuse a GraphQL API.


For a more in-depth discussion and to learn how to turn introspection off in production, read “[Why You Should Disable GraphQL Introspection In Production – GraphQL Security](https://www.apollographql.com/blog/graphql/security/why-you-should-disable-graphql-introspection-in-production/) “.


**Mask errors**


When server or downstream service errors occur, it’s a good idea to withhold the exact specifics of what went wrong from the client.


Informing the client about error details in the server exposes the current server vulnerabilities and opens the door for more concentrated attacks.


For example, the following error reveals information about the source code and even, potentially, the type of databases we may be using.


```text
"data": {
"astronaut": null
},
"errors": [
{
"message": "Database Error: Astronaut does not exist",
"extensions": {
"code": "INTERNAL_SERVER_ERROR",
// ...
"exception": {
"stacktrace": [
"Database Error: User does not exist",
" at __resolveReference (../services/vehicles/index.js:29:13)”,
// ...
],
// …
}
}
}
]
}
```


To prevent this issue, swallow errors before they get to the client. You can use the formatError API in Apollo Server to implement this.


```text
const server = new ApolloServer({
typeDefs,
resolvers,
formatError: (err) => {
// Don't give the specific errors to the client
if (err.message.startsWith('Database Error: ')) {
return new Error('Internal server error');
}
// Otherwise return the original error
return err;
},
});
```


Read more about this in “[Masking and Logging Errors](https://www.apollographql.com/docs/apollo-server/data/errors/#masking-and-logging-errors) ” from the Apollo docs.


**Avoid schema autogeneration**


Many tools will autogenerate a GraphQL schema based on database tables, etc.


While these tools tend to speed you up in the short run, they make it very easy to guess fields on the root operation types based on CRUD patterns.


We recommend following the principle of a *demand-oriented schema* (see[PrincipledGraphQL](https://principledgraphql.com/agility#4-abstract-demand-oriented-schema) ) — that is, to build your schema tailored to what the client needs are and to do so by hand. Because a demand-oriented schema is custom-built, it benefits from reducing the predictability of the graph structure, thus reducing the attack surface area.


**Query subgraphs only (in Apollo federation)**


Federated schemas will have _entities and _service queries to assist with composition and query planning• The SDL field on the _service root query field fetches the subgraph schema SDL.


As a best practice, only allow an Apollo Gateway to query subgraph services directly, not the clients.


Read more about[subgraphs](https://www.apollographql.com/docs/federation/subgraphs/) and the[federation specification](https://www.apollographql.com/docs/federation/federation-spec/#federation-schema-specification) here.


### 5. Batch requests


**Limit query breadth**


Here’s one more particularly GraphQL-specific vulnerability. Clients can use aliases to write batch queries like the following:


```text
query MaliciousQuery {
alias1: fieldName { subField1 subField2 ...}
alias2: fieldName { subField1 subField2 ...}
...
alias10: fieldName { subField1 subField2 ...}
...
alias100: fieldName { subField1 subField2 ...
...
alias1000: fieldName { subField1 subField2 ...}
...
}
```


Someone may write a query like this to hurt performance purposefully, scrape as much data as fast as possible, or attempt to mitigate rate-limiting. For example, consider the scenario of brute-forcing login credentials:


```text
query Mutation (
$input1: LoginInput,
$input2: LoginInput,
$input3: LoginInput
# ... And more
) {
first: login (input: $input1) {
token
}


second: login (input: $input2) {
token
}


third: login (input: $input3) {
token
}


# .. And so on
}
```


To prevent this, we can use a combination of techniques we’ve previously discussed. Rate-limiting and query complexity analysis will work.


**Use data loaders to prevent DoS-ing yourself**


If you’re resolving data from backing data sources (like a REST API or a subgraph), you’ll want to make efficient use of the network to prevent DoS-ing yourself.


A great technique is to use data loaders to minimize the number of requests to backing data sources from resolvers:


Read about[data loaders here](https://github.com/graphql/dataloader) . Also, consider caching as an approach to mitigating the number of necessary requests. You can implement caching at various levels like the gateway or the subgraph level in the context of a federated architecture. Read “[Using Memcached/Redis as a cache storage backend](https://www.apollographql.com/docs/apollo-server/data/data-sources/#using-memcachedredis-as-a-cache-storage-backend) ” to learn more.


## Observability, monitoring, alerting & access


Beyond protecting your GraphQL API from bad actors and locking down private data, to improve your GraphQL security posture, you also need a window into how your API is being used and by whom. You’ll also want to know when there are performance anomalies and how to manage access to your graph safely. That’s where[Apollo Studio](http://studio.apollographql.com/dev) comes into play.


### 6. Observability


With observability, we can figure out what’s happening with our data graph in production and get a detailed understanding of who is using our graph, which clients called operations, and how long they take to execute.


To get this enhanced API understandability, we consistently name operations and force clients to identify themselves using our graph.


We can set this up with[Apollo Studio’s](https://studio.apollographql.com/dev) Client Awareness feature. For more info and to get started, read “Who’s[Using My Graph? — Apollo Studio Client Awareness](https://www.apollographql.com/blog/announcement/platform/how-apollo-studio-client-awareness-improved-our-lives/) “.


### 7. Monitoring


Apollo Studio also makes it easy to leverage field and operation-level tracing data to monitor API performance and errors.


### 8. Performance alerts


And you can also configure alerts to push notifications to you when something goes wrong, whether it’s an increase in requests per minute, changes in your p50, 95, or 99 response times, or errors in operations run against your graph.


Read more about “[Performance alerts](https://www.apollographql.com/docs/studio/performance-alerts/) ” in Apollo Studio on the Apollo docs.


### 9. Managing graph access


Just as important as it is to set limitations around how the outside world interacts with your data graph, you also want to manage access to different aspects of your graph internally too.


Apollo Studio provides both graph API keys and personal API keys to restrict access to the data graphs within your organization.


Apollo Studio also has unlimited, free read-only consumer seats. That means if you have non-developers on your team that want to safely and securely explore production data, you can generate a sharable link to invite them to your graph.


Studio gives your team members access to specific variants of your graphs.


Read more about user management in “[Managing organization members](https://www.apollographql.com/docs/studio/org/members/) ” in the Apollo Studio docs.


## Conclusion


In this article, we covered various techniques for securing your GraphQL API in production. Authentication and authorization are the first challenges to address. Beyond that, we learned how to reduce the attack surface area for many common GraphQL-related vulnerabilities and how we can use Apollo Studio to set up observability, monitoring, alerts, and user management for your production graph.


To get started with Apollo Studio, head to[studio.apollographql.com/dev](http://studio.apollographql.com/dev) .


Special thanks to the brilliant[Mandi Wise](https://twitter.com/mandiwise) for her experience securing GraphQL APIs.


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
