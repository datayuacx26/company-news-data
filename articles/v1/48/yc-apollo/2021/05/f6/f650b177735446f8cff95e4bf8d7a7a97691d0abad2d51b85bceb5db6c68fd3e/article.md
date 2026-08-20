---
schema_version: "1.0.0"
document_id: "f650b177735446f8cff95e4bf8d7a7a97691d0abad2d51b85bceb5db6c68fd3e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/why-you-should-disable-graphql-introspection-in-production"
published_at: "2021-05-07T11:48:47+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:f334a073c3afe853156c96f22e0af7d23d6c6acca0e13af9900b99b0441942c2"
---

# Why You Should Disable GraphQL Introspection In Production – GraphQL Security

Once your graph is up and running in production, like anything else on the internet, it’s a good idea to implement precautions to prevent it from being compromised by bad actors. Disabling introspection in production is a widely debated topic, but we believe it’s one of the first things you can do to harden your GraphQL API in production. In this post, we’ll discuss why we believe you should disable GraphQL introspection in production, how to do it, and present a way to get the same benefits of introspection in production using a schema registry instead.


## Introspection


### What is it?


GraphQL introspection enables you to query a GraphQL server for information about the underlying schema. This includes data like types, fields, queries, mutations, and even the field-level descriptions.


### What do we need introspection for?


We believe that **introspection should primarily be used as a discovery and diagnostic tool** when we’re in the **development phase** of building out GraphQL APIs. While we don’t often use introspection *directly* , it’s important for tooling and GraphQL IDEs like Apollo Studio, GraphiQL, and Postman. Behind the scenes, GraphQL IDEs use introspection queries to power the clean user experience helpful for testing and diagnosing your graph during development.


## Introspection in production


Let’s consider the utility of introspection outside of the context of development, in a production environment. First, we need to make a distinction between public and private GraphQL APIs.


### Public vs. private APIs


A public GraphQL API is one made primarily for consumption by developers *outside* of your organization (like the Shopify or GitHub APIs). Whereas a private GraphQL API is one built to serve the client-side experiences for products built by developers *within* your organization. **The vast majority of us are building private GraphQL APIs** .


### Use case: Learn all possible GraphQL operations


In the context of a public GraphQL API, to learn all possible GraphQL operations, you certainly could leave introspection on in production, but our principled belief is that clear and expressive documentation (API references) is the better **discoverability** tool for a public GraphQL API. On the other hand, to learn all possible operations in the private context, security is the prime concern. At first glance, it may make sense to leave introspection on in production so that developers on your team could merely point their GraphQL IDEs to the URL of the production graph, look at the structure, and see what’s possible. However, we should consider the risks of leaving an API *not intended for use by anyone other than the developers in your org (* with auto-generated documentation on how to perform every operation *)* out in the open on the internet.


### To utilize production data


Once we know what we can do with the API, we can write queries and mutations. For a public GraphQL API, querying and mutating your personally owned data is likely the very reason why the API exists. You’d also likely need to authenticate yourself, then accompany all of your requests with a valid auth token, but again — this is a constraint typically communicated better with *words* — through API documentation. We can do this without introspection. Now, considering the private GraphQL API context again — most of the time, you don’t want just *anybody* to learn how to run queries against your private data. It’s usually a small set of people. The developers within your org.


## Problems with introspection in production


### Revealing sensitive information


As you invest in building a graph at your company, you’ll want to ensure that details that should only be known *inside* your org, stay *inside* . If you use the **description feature** in GraphQL, it should be known that this is a type of detail that can be queried using introspection queries. For example, the following field containing helpful (yet sensitive) details can be queried by any party on a GraphQL API in production with introspection enabled.


```text
type CreditProfile {


"""
This field is computed based on the user's current
credit score + their history over the last 30
days using our product
"""
ranking: Float


...
}
```


### Easy to discover potentially malicious operations


Leaving introspection on in production is like serving a meal with the very recipe used to create it. While it’s still possible for bad actors to learn how to write malicious queries by reverse engineering your GraphQL API through a lot of trial and error, disabling introspection is a form of *security by obscurity* . It’s not the best form of security, but paired with other techniques like[size, depth, amount limiting and operation whitelisting](https://www.apollographql.com/blog/securing-your-graphql-api-from-malicious-queries-16130a324a6b/) , it can make a substantial difference.


## Turning off introspection in production


You can turn off introspection in production by setting the value of the` introspection` config key on your Apollo Server instance.


```text
const server = new ApolloServer({
typeDefs,
resolvers,
introspection: process.env.NODE_ENV !== 'production'
});
```


We recommend using environment variables so that you can enable introspection for GraphQL tooling in your` development` and` staging` environments but not for your` production` one. You can[read the docs](https://www.apollographql.com/docs/apollo-server/api/apollo-server/#introspection) about introspection here.


## Use a schema registry to update, browse, and maintain your production Graph


Now that we’ve turned off introspection in production, how do we:


- Enable new developers to explore the current schema and its capabilities
- Query production data
- Let other team members learn our graph


### Register your schema


First step is to **register your schema** to the Apollo Studio schema registry. You’ll want to see the docs on “[Registering schemas using schema reporting](https://www.apollographql.com/docs/studio/schema/schema-reporting/) ” to get started.


### Exploring the schema’s shape and data


Once your schema is registered, you can log into[studio.apollographql.com](http://studio.apollographql.com/dev) and check out the schema in the SDL tab.


You can also keep track of how your schema changes over time.


And when you want to dive deeper into your production environment, you can head over to the Apollo Explorer to write queries and view data.


### Invite team members to explore your graph


Apollo Studio also has **unlimited, free read-only consumer seats** . That means if you have non-developers on your team that want to safely and securely explore production data, you can generate a sharable link to invite them to your graph.


We think schema registration is much safer than having your graph sit out on the internet with introspection on. Happy querying!


## Conclusion


In this article, we learned that:


- GraphQL introspection lets you inspect all the types and fields of a schema
- GraphQL introspection is primarily for GraphQL developer tooling
- Leaving introspection on in production exposes potential issues like exposing sensitive information and enables malicious parties to more easily discover graph vulnerabilities
- Registering your graph to a schema registry is a safer, more secure way to enable access to your graph and your data


To register your schema, head to[studio.apollographql.com](http://studio.apollographql.com/dev?referrer=blog) .


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
