---
schema_version: "1.0.0"
document_id: "c2aa135c2f575903f8d8d3e10f7e30e2e4b5339b3866228d874355b5e49782a6"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-priceline-com-manages-pci-compliance-with-graphql"
published_at: "2022-09-07T11:31:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:5ae72913a8ff703c06588cab1bc6d9509a0a4b1d405aed4f930f8142e36d96e3"
---

# How Priceline.com manages PCI compliance with GraphQL

Let’s consider the core promise of GraphQL:


**Get exactly what you need and nothing more with a single query.**


Does it sound amazing? Absolutely. But if you’ve ever tried to implement GraphQL for an entire engineering team, you’ve probably realized that it’s not quite as simple as it sounds to achieve. For any dev in the organization to be able to get everything they need in a single query, *everything* that anyone could need has to exist in a single graph.


The concept of a single graph for everything seems all but impossible when you think about it from a compliance perspective, and that was one of the biggest challenges we faced when we started to scale up our use of GraphQL at Priceline. As a travel booking platform that supports millions of customers around the world, our backend services contain quite a bit of PCI data that can’t be made accessible to our client apps.


One graph → one endpoint → client apps have access to everything, including cardholder information and other sensitive data. Right?


Well it turns out, not necessarily. With a[supergraph](https://www.apollographql.com/blog/announcement/backend/the-supergraph-a-new-way-to-think-about-graphql/) , we can have a single graph that unifies all of all of our data and services, including sensitive data, but we can selectively choose which parts of the graph are exposed to various audiences, like app devs. In this post I’ll walk you through how we did this for the Priceline supergraph and how you can set it up yourself using contracts in Apollo Studio.


## Is a supergraph worth it?


Let’s take a step back – is it even worth it to put everything into one graph? Couldn’t we just have entirely separate graphs for internal systems that require sensitive data and for client apps that don’t? We could, and at Priceline we actually did at first, but we would end up with a lot of duplicative maintenance work that could be avoided.


There’s always going to be overlap between what internal systems need and what client apps need. For example, our booking graph, which was not part of the supergraph, contained airline policy schema used for display when a customer was booking a flight. Our native apps teams wanted to display this information to a customer viewing flight information, earlier in the funnel. However, they did not want the overhead of multiple network requests. Normally this would result in duplicating the airline policy schema inside the supergraph. Not ideal.


And that’s just one example. To provide a specific subset of services for a certain audience or experience that differed from an already existing graph, we either had to use preexisting REST APIs or create an entirely new graph to support it. IT systems app? Separate graph. Partner API? Separate graph.


With a supergraph, we could create a single source of truth for definitions of every entity in our data model (hotel, flight, rental car, customer, etc), eliminating a huge amount of maintenance work associated with these disparate graphs. For us, it was worth it.


## Restricting access to sensitive data in a supergraph


As a single source of truth, our supergraph schema contains everything, including sensitive data. So we can’t just have every application query the endpoint for the entire supergraph directly. Instead, we need to define which parts of the supergraph certain applications are allowed to access. We can do this by creating contract graphs in Apollo Studio.


### Contract graphs: filtered representations of the supergraph


Contract graphs are variants of the overall supergraph that only contain a filtered subset of the fields and types defined in the supergraph schema. Deriving a contract graph’s schema from the supergraph schema requires two things:


1. Using the` @tag` directive to apply tags to fields, interfaces, objects, and unions in your subgraph schemas.
2. Creating a contract in Apollo Studio that defines which tags should be included or excluded from the contract schema


Once you’ve tagged your subgraph schemas and created your contract in Apollo Studio, the contract graph schema will automatically be derived from the supergraph schema whenever it’s composed in Studio. The contract schema can be used to power Apollo Router or Apollo Gateway to create a running contract graph with its own endpoint.


### Using contracts to filter out PCI data


At Priceline, we use a contract graph to prevent clients from sending and requesting cardholder data through the supergraph, to the booking subgraph. The fewer touchpoints with this sensitive information, the better. As I mentioned earlier, the booking subgraph also contains airline policy information which we do need client apps to have access to so that we can display it to customers on a variety of pages


Using the` @tag` directive, we tagged all of the sensitive entities in the Checkout subgraph with` PCI` . Then, we defined a contract in Apollo Studio that excludes anything tagged with` PCI` . The derived contract graph is what our client apps query rather than the supergraph itself.


**That’s it.**


Applying tags within your subgraph schemas and creating a contract in Studio is all you need to do to create contract graphs that remove access to sensitive data. And, because contract graphs derive their schema from the supergraph, they are automatically kept up-to-date with any changes in the supergraph schema. Modifying the entities included in a contract graph is as easy as editing tags in your subgraph schema or the filters in Apollo Studio.


## What’s next for GraphQL at Priceline


At Priceline, our supergraph already powers nearly all of our experiences, but we’re still just getting started. In the future, we’re planning on using contract graphs to improve our Travel Agents API, our internal customer support application, and more.


Written by


Mike Byrnes


[Read more by Mike Byrnes](https://www.apollographql.com/blog/author/mike-byrnes)
