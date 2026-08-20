---
schema_version: "1.0.0"
document_id: "6af1c9950c62f4c7a6526f094c714d14eafb6c45c704b0f48a1fddd4034d9d44"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/from-monolith-to-federation"
published_at: "2023-08-09T11:33:08+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:bc619ccc83e142a27948fd0803cae68b2ed0afabe1b40caec34ea80003ae66cf"
---

# From Monolith to Federation

## Introduction


Many organizations own a Monolith (GraphQL or Non-GraphQL) and migrating away always seems an impossible task, most organizations fall into a seemingly endless amount of effort with very little progress to show for it.


At Apollo we have seen many different patterns of migrating away from a monolith. In this blog we will discuss the pattern which we’ve seen have the greatest success, and one which iteratively provides actual progress to the business investing in this change, all made possible by the careful application of Federation to the problem.


## The digital transformation


Our wide reaching experiences has led us to recommend the[Strangler fig](https://martinfowler.com/bliki/StranglerFigApplication.html) pattern, which basically means placing a facade in front of your monolith (with every call to that monolith now passing through the facade), allowing you to start to dice up and remove/replace the monolith – critically – without affecting your consumers.


Using this pattern allows you to move away from the Monolith whilst critically showing incremental progress to the business, regarding removing the tech debt which currently burdens them.


The first thing to ask – often the hardest to answer – is how do you split the monolith up? Most companies choose to split up the monolith by domain.


Lets start with an example scenario of a typical e-commerce application and some of its possible domains.


- Product
- Inventory
- Cart
- Order
- Payment
- Customer account
- Marketing


Using the domains discussed above we can see how we could view a monolith in segments relating to specific domains – in figure below


figure 1. Your Monolith


If we look at figure 2 of a simplified and typical monolith setup, you can see how clients fetch data and how tightly bound clients are to the system. To move past this, we use Federation as the critical enabler.


figure 2. Bound to the Monolith


The key technical aspect to manage here is bringing about productive change as fast as possible, wasting as little time and money during the digital transformation.


The approach is to decouple the existing clients from the monolith and onto an established Interface which is afforded by the Federation layer. We are now actively removing risk of back end change from the client, thus allowing the organization to go ahead with the transformation.


figure 3. First successful subgraph migration


The API Gateway here will now allow us to fetch a data response (previously monolith) from the newly Federation architecture. Make sure to record lessons here and apply them going forward to the next subgraph.


Do note, that some client work *may* be required here to bind newly created or improved client operations/queries.


Now that you have gained the experience of implementing one subgraph, you are ready to transfer this knowledge to implement the second subgraph. You will find it gets easier and easier, because every new subgraph implementation will build on the learning of the last.


It is important to note that this pattern keeps the monolith in play purely as a data source, because you are 100% certain the data is correct. Changing everything at once forcefully cascades change across (and activating) nearly every team, which will become impossible to manage. The creation of a contract (GraphQL Schema) de-risks the migration hugely because the org can be 100% certain they can serve the original data response structures reliably.


figure 4. Learning from the first and implementing second subgraph


Note however, it is not necessary to connect to the monolith **if** you already have the new replacement backing microservices already available. If this is the case, you can bypass the monolith immediately and connect to that microservice.


figure 5. Strangler Fig fully applied to Monolith


Using the Strangler pattern, we will arrive at a point where all traffic for each client is actually being facaded by the supergraph itself.We are now officially in a *decoupled state* , whereby the client has no idea where the data is coming from, freeing the org to start the decommissioning phase of this pattern; shutting down the monolith section by section.


figure 6. Strangler Fig worked. New modern services implemented.


Figure 2 above demonstrates two domains which were previously served by the monolith, now being served by the new modern microservices.


It is critical to note, this pattern actively hides deep architectural implementational details away from the client, yet *still* fulfils the historically agreed data response, enabled entirely by contract (Schema) of our new Supergraph.


figure 7. The migration is complete. The Monolith has been successfully removed.


The end state of the transformation is achieved once all the domains in the monolith have been migrated, thus allowing the org to successfully rid themselves of the monolith.


## Conclusion


Although moving away from a deeply entrenched monolith can seem like an impossible task, using the steps as described in this pattern and also compartmentalizing away any un-required complexity – what seems an impossible task, can be achieved in a manageable fashion.


Leverage the supergraph contract to decouple your system, allowing you to **safely and predictably** migrate away from the monolith, without impacting your clients.


Using Federation as a tool will greatly increase your ability to deliver this digital transformation on time and on budget; as it provides the missing link to decouple the change process of systems modernisation, between your clients and internal architecture. Head over to our[documentation](https://www.apollographql.com/docs/federation/#break-up-monolithic-code) to learn more of how you can break up your monolith and incrementally adopt Federation 🙌


Written by


Daljit Summan


[Read more by Daljit Summan](https://www.apollographql.com/blog/author/daljit)
