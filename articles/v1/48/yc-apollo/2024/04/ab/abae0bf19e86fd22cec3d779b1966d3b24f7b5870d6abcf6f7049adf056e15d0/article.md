---
schema_version: "1.0.0"
document_id: "abae0bf19e86fd22cec3d779b1966d3b24f7b5870d6abcf6f7049adf056e15d0"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/whats-new-in-apollo-client-3-10"
published_at: "2024-04-25T12:26:16+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:fe007bdc2046dd9481b80ced3770e0c248fdda561e6d31d88ff112149f853928"
---

# What’s new in Apollo Client 3.10

Apollo Client 3.10 has just been released, and we want to provide you with an overview of what’s new:


- Introducing new experimental schema-based testing utilities
- New API:` client.watchFragment`
- ` createQueryPreloader` reaches “stable” status


Let’s take a look:


## Introducing new experimental schema-based testing utilities


In Apollo Client 3.10, we are introducing new schema-based testing utilities under the` @apollo/client/testing/experimental` import.


This adds two new tools:


**` createTestSchema`** allows you to enhance a` GraphQLSchema` class with mock scalars and mock resolvers.


You can modify the resolvers inside your tests to simulate changing results.


This approach is an alternative to the current testing approach with` MockProvider` , allowing you to write tests similar to how you would implement your backend – applying skills you already have and can use in other situations.


You can then use this schema with[Mock Service Worker](https://mswjs.io/) to simulate a GraphQL server, or you can use our new **` createSchemaFetch`** function to create a` fetch` function to be used inside of your tests.


Both of these approaches allow you to use your real link chain instead of a mocked one.


You can read more on this testing approach in[the schema-driven testing RFC](https://github.com/apollographql/apollo-client/issues/11705) and[the documentation](https://www.apollographql.com/docs/react/development-testing/schema-driven-testing) .


## New API:` client.watchFragment`


We also add a new function to the` ApolloClient` class:` watchFragment` .


This function enables the functionality of the` useFragment` hook to be used in Vanilla JavaScript or other frameworks.


A PR to add a` watchFragment` functionality to` apollo-angular` has already been opened, and more will follow soon!


## ` createQueryPreloader` reaches “stable” status


In Apollo Client 3.9, we introduced the` createQueryPreloader` API as “alpha”, and since we’ve heard your positive feedback, with 3.10, we’re releasing it as “stable.”


` createQueryPreloader` is used to start a query outside of React (e.g. in a Router action) and then later attach to the generated` queryRef` with the` useReadQuery` hook.


You can read more about it in the[documentation](https://www.apollographql.com/docs/react/data/suspense/#initiating-queries-outside-react) .


Written by


Lenz Weber-Tronic


[Read more by Lenz Weber-Tronic](https://www.apollographql.com/blog/author/lenz)
