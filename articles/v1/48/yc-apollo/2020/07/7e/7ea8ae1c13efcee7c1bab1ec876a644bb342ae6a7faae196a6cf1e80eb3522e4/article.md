---
schema_version: "1.0.0"
document_id: "7ea8ae1c13efcee7c1bab1ec876a644bb342ae6a7faae196a6cf1e80eb3522e4"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-the-best-practices-series"
published_at: "2020-07-17T10:56:17+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:d684f3ba746d9846ccfe503a97a91aba9db124d8527c92683d991c7cf2038b99"
---

# Introducing the Apollo Client Best Practices Series

If you’ve been entrusted to build something important, it’s always good to know you have resources to rely on. Maybe a team member. Maybe a book or a course. Walking into a project with a set of new tools can be pretty scary.


That’s where I think *best practices* come in.


As a part of the Developer Experience team at Apollo, and someone who personally uses Apollo’s tools, I totally know what it feels like to not know if I’m doing things correctly. That’s why I’ve decided to work with the Apollo Client team to help put together the **Apollo Client Best Practices** series.


## Our advice


Apollo Client is, first and foremost, a GraphQL client library. You can cache *subsets* of your *backend data graph* on the client-side and work with it the same way you’d work with` global` state using tools like Redux or React Context. This means Apollo Client is also a[state management library](https://www.apollographql.com/blog/dispatch-this-using-apollo-client-3-as-a-state-management-solution/) *and* a data-fetching tool. State management and data fetching are two massive pieces of the puzzle building client-apps, but there are other problems to solve too.


What about architecture? How do we handle the various types of state (component, local, remote)? What about testing, pagination, server-side rendering, or security?


We like[principles](https://principledgraphql.com/) . This series is a curation of our advice for how to use Apollo Client to build testable, flexible, and maintainable production applications.


## Where should I start?


If you’re rusty or unfamiliar with Apollo Client and GraphQL, I’d recommend first walking through the[getting started guide,](https://www.apollographql.com/docs/react/v3.0-beta/get-started/) learning about[queries](https://www.apollographql.com/docs/react/v3.0-beta/data/queries/) and[mutations](https://www.apollographql.com/docs/react/v3.0-beta/data/mutations/) , then trying out[the full-stack tutorial](https://www.apollographql.com/docs/tutorial/introduction/) .


After that, check out the posts from the series in order.


## Start reading


Here are the current best practices guides.


1. [Apollo Client & Client-side Architecture Basics](https://www.apollographql.com/blog/apollo-client-client-side-architecture-basics)
2. [Local State Management with Reactive Variables](https://www.apollographql.com/blog/local-state-management-with-reactive-variables)
3. [Demystifying Cache Normalization](https://www.apollographql.com/blog/demystifying-cache-normalization)
4. [When to Use Refetch Queries in Apollo Client](https://www.apollographql.com/blog/when-to-use-refetch-queries-in-apollo-client/)
5. [Redux to Apollo: Data Access Patterns](https://www.apollographql.com/blog/redux-to-apollo-data-access-patterns/)


Have an idea for another guide you’d like to see? Use the feedback form below to suggest another topic!


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
