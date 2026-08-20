---
schema_version: "1.0.0"
document_id: "a959f97d6ed2562eed2c26597522e32f6f91b552b75349c8f55d60d2bd0d3ab4"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/schema-services-transitioning-towards-a-federated-graphql-architecture"
published_at: "2020-11-04T15:09:30+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:413e7e14a5eb3531f761baf2e7ca66cb7ebab109591e678e34d2c7248cf894b4"
---

# Schema Services: Transitioning Towards a Federated Architecture

The reality for most large organizations adopting Federation is that the architecture sitting behind it is… messy. In this article, I will introduce the concept of *schema services* and demonstrate how they can provide an easier transition towards a federated architecture.


From a jumble of modern and legacy services, different tech stacks, data sources, platforms, etc. most enterprise architecture diagrams look more like a *grab bag* of tech from the last couple of decades than anything else. It most definitely doesn’t look like this:


It’s all very well seeing diagrams that show the perfect end state of a gateway backed by a set of federated services (like the one above), but, while that makes for a great target, it begs the question: what does a transitionary architecture look like?


Further complicating matters is another reality that most organizations face: a lack of GraphQL skills in their engineering teams. GraphQL is still a relatively new technology, and many engineers have little to no experience of GraphQL yet. How do you rapidly rollout Federation when you have a significant skills gap?


I believe the answer is a *transitionary* architecture.


## Focus on a Transitionary Architecture


While implementing Apollo Federation at[RS Components](https://www.rs-online.com/) , we had to develop a transitionary architecture that could overcome both the messy architectural landscape and the GraphQL skills gap in our engineering teams.


We started our implementation a year or so into a multi-year project to migrate our monolithic application (which is 20+ years old) to a cloud-based microservices architecture. At the start of the project, only about 20% of the data was available from these new services; the rest coming from legacy systems.


Across both the old and new services, the APIs were more data-centric than product-centric and contained lots of internal language that external consumers would not understand. In order to create a product-focused schema, we would either have to update the services or transform the data being returned.


Further complicating the situation was the mix of protocols being used across the various services: most were REST based, with a sprinkling of GraphQL-native ones along with some which were… unique to RS. Whilst our intent was (and still is) to shift all services over time to be GraphQL-native, for the foreseeable future we were going to be left with a variety of service types.


Unsurprisingly there was neither capacity nor appetite for updating every service, so we had to come up with a solution that would not only allow us to create the product-focussed federated schema we wanted, but that would also act as a stepping stone towards the clean target architecture above.


## Introducing Schema Services


Our solution to this problem was to introduce a set of services in between the gateway and our underlying services: we call these *schema services* .


A **schema service** deals with a single *concern* , combining and transforming several microservice responses into a single, product-focused schema. Taking and updating the simple diagram from earlier, we now have:


In the above we have introduced two **schema services** :` Product` and` Customer` . These sit between our federated gateway and the underlying services, responding to incoming operations from the gateway for their given concern.


Schema services are kept intentionally slim and are typically nothing more than an[Apollo Server](https://www.apollographql.com/docs/apollo-server/) instance containing a schema that is backed by simple resolvers that make use of custom[data sources](https://www.apollographql.com/docs/apollo-server/data/data-sources/) to call the underlying services. These data sources bake in standardized logging, caching and transformation hooks in order to reduce duplication of code between services.


With the introduction of schema services, underlying services can now be made available through the federated gateway with no changes required to the services at all. Rather than facing an uphill battle to convert all our services, we now only have to introduce a handful of schema services in order to create our federated data graph – we currently have eight schema services at RS.


### Baby Steps Towards the Target


The` Review` service in the diagram above is an example of how schema services can be phased out of our transitionary architecture. When the team that owns a service is able to take on the work of federating their service, as in the case of the` Review` service, then they can be integrated into the gateway directly.


This gradual move to directly federated services will slowly transition us towards our target architecture and eventually mean that the schema services are redundant.


### Advantages of This Approach


- There is no impact on existing services, the schema services will call them like any other client.
- The federated schema can start small and grow as more data requirements are included.
- Schema services are relatively simple services with no business logic. They should be quick to develop, test and release.
- If desirable, the initial schema service development can be centralized in one team meaning there are no inter-team dependencies to complicate matters.
- Further more, schema services can remain centralized if so desired to allow for a highly controlled schema.
- Federatable schemas can be designed from scratch without having to accommodate legacy or data-centric language.
- Schema service caching can be used to limit traffic to the underlying services, especially helpful when protecting legacy systems.


### Disadvantages of This Approach


- There is an additional hop in all requests for data; this will add latency to requests and complicate the overall architecture.
- No matter which way you look at it, you are introducing additional services into your architecture and thus cost.
- Synchronising changes in the schema services with underlying service changes mean that there is coupling between the two layers. Depending on the rate of change, this can be a big headache.
- Ownership of schema services can become complex. If you start centralised and then distribute ownership later, how is that transition managed? What’s the governance model and how do you maintain schema consistency.


## Summary


Are schema services perfect? No, not by a long shot, but they do turn what at first seems a daunting task of federating a messy collection of services, legacy systems and whatever other dragons are hiding in your enterprise into a manageable project that can be delivered in a surprisingly short amount of time.


My only advice, no matter what approach you take, is to start small.


Most enterprises have a lot of data and a lot of services, so pick a subset of both and deliver Federation for a single page or feature to start with. Keep a narrow focus.


Thanks for reading; if you’d like to talk about schema services, GraphQL or tech in general, give me a follow on[Twitter](https://twitter.com/andyroberts_io) and send me a DM. As a bonus, you’ll also get to see any future blogs I write!


Written by


Andy Roberts


[Read more by Andy Roberts](https://www.apollographql.com/blog/author/andyroberts)
