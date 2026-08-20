---
schema_version: "1.0.0"
document_id: "35bf27e2b5476dbffdb1529833daee29aa8a78f6671a66de471d9992d525316d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-the-apollo-federation-best-practices-series"
published_at: "2022-04-15T09:06:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:7fb15bd94e121e5c530e8a48e21919236f880562d014ab19f0e10b552dfbdad1"
---

# Introducing the Apollo Federation Best Practices Series

Building a graph can feel daunting, regardless of whether you’re working on a new project or a large enterprise trying to bring it all together. Bringing everything into one graph brings up various questions: What APIs are made available for clients? How many services do existing clients talk to? How are these services related?


Apollo Federation provides the possibility of solving these concerns through a domain-oriented graph, which allows you to elevate your graph from just another API to a platform that exposes the value of your product.


Apollo Federation provides a developer-friendly path to GraphQL microservices for any project, whether you’re a single developer or a large organization. Since its inception, I’ve been helping customers with Apollo Federation, and that’s why I’ve started the Apollo Federation best practices series. To help you overcome the most common challenges devs face when designing, building, and scaling federation in the wild.


## Our Advice


At the heart of Apollo Federation is a thin[open-sourced specification](https://www.apollographql.com/docs/federation/federation-spec/) that makes it possible to modularize your graph into smaller parts using entities and subgraphs. Linking subgraphs together using the` @key` directive, we compose them into a **supergraph** . We believe the supergraph provides a new strategic advantage for devs building with GraphQL, but first, we should discuss the questions that lead us to this conclusion.


What is a supergraph? What is the architectural difference between a monolith and a supergraph? Why would we want to break down our current monolithic architecture? What are the advantages of a supergraph? How is designing a federated schema different from a single schema? What about deploying a federated graph and proper security for my subgraphs?


This series is a curation of our advice on why you should embrace a distributed architecture and how you can be successful with Apollo Federation.


## Where should I start?


If you’re rusty or unfamiliar with GraphQL, I’d recommend first walking through the getting started guide for Apollo Server and taking the[Lift-off course in Odyssey](https://odyssey.apollographql.com/lift-off-part1) .


After that, check out the posts from the series in order.


## Start reading


1. [Introduction to Apollo Federation](https://www.apollographql.com/blog/introduction-to-apollo-federation)
2. [Federation schema design principles](https://www.apollographql.com/blog/federated-schema-design/)


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
