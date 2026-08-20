---
schema_version: "1.0.0"
document_id: "f090d4cd4a51015345cca1dcafb07f7490554f918f785a982b57d96b9f8afdf6"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-contracts-serve-many-audiences-with-a-unified-graph"
published_at: "2021-11-10T10:45:05+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:d0f88cd3424ed8af527bbfd406422ac5d56b86b963315b043459b1eb9ee6ddaa"
---

# Introducing Contracts: serve many audiences with a unified graph

At Apollo, we talk a lot about building a unified graph, and for good reason. A unified graph provides incredible benefits – a single source of truth for all services, faster apps, faster delivery, less maintenance overhead, and more.


But unification introduces new challenges, especially at scale;


*“How do I support different experiences and levels of access for different personas or types of users like customers, employees, suppliers, and partners?”*


*“How do I provide my partners and public API users with a schema scoped for their integration and app development needs?”*


*“How do I provide client-specific BFFs and experience-driven schemas alongside a canonical domain-driven schema?”*


*“How do I restrict access to experimental or non-stable parts of the schema, but still allow access for testing and early adopters?”*


These common challenges boil down to one question: **“how do I serve multiple different audiences with a single unified graph?”** Today, we’re excited to announce Contracts, a new feature in preview on Apollo’s Enterprise plan that allows you to create tailored graphs for different audiences by applying filters to a single unified graph.


## Modularity when consuming the graph


We often discuss consolidating APIs, but there are benefits to separation as well. Separate APIs can be managed by independent backend teams with less coordination. They can also be tailored to whoever will be consuming them and individually governed for granular access control.


We want to unify our services into a single graph to reduce manual API maintenance and improve delivery velocity, but we don’t want to lose the benefits of separate APIs. Apollo Federation accomplishes this for the backend by modularizing the graph into subgraphs that can be modified asynchronously, allowing service teams to build a unified graph while still operating independently. Contracts deliver the same modularity for the frontend.


With Contracts, graph platform teams can create tailored experiences for multiple audiences by creating contract graphs that contain a filtered subset of the unified graph. Each contract can align with a particular developer audience to streamline the graph experience, rather than providing the entire graph to every internal and external developer.


Using contracts you can create separate graphs for partner developers, specific client applications, public APIs, experimental features, and more without losing any of the benefits of the unified graph.


## Creating contracts


Contracts work with a simple but powerful new` @tag` directive which you can apply to fields, interfaces, objects, and unions in your subgraph schemas. You then create filter rules which include or exclude certain tags. Anything not included in the contract will be removed from the client-facing schema for that contract but will still be included in the internal supergraph schema to be used for Federation and routing.


## Documentation contracts vs deployed contracts


There are two ways you can use a contract schema – as documentation or as a running endpoint.


### Documentation contracts


You might use a contract schema just for documentation if you simply want to streamline the graph experience for internal developers by only showing them the fields they need from the graph. For example, you could create a mobile contract schema that only includes parts of the graph that are relevant to mobile developers and removes unnecessary parts that may only be useful for web or IoT applications.


*Documentation contract*


With Apollo Studio you can point consumers at the tailored contract schema so that they get the full Studio experience with a changelog, comprehensive schema reference, and interactive Explorer scoped to the relevant subset of the graph.


### Deployed contracts


There may be times, however, when we want to restrict not only what appears in the documentation, but also what can actually be queried by a particular audience. To accomplish this, we can deploy a dedicated Apollo Gateway or Router to run the contract schema as a separate endpoint.


*Deployed contract*


Developers accessing the deployed contract endpoint will only be able to query parts of the graph that exist in the contract schema and will encounter an error if they try to query for fields that are not included in the contract. This can be particularly useful for building endpoints for external partners or for a public-facing API.


## Managed Federation, managed contracts


BFFs and other customized APIs for different audiences are fantastic for providing a streamlined developer experience, but they also create overhead for whoever maintains them. One of the best things about contracts is that they automatically remain up-to-date without any manual work when combined with Apollo Managed Federation.


With Managed Federation in Apollo Studio, independent backend service teams publish subgraph schema updates into Apollo, including any tags to be used for contracts. The subgraph schemas are composed together into a supergraph schema which is then used to power a graph router like the Apollo Gateway or Apollo Router. Using the included tags, contract filters are run on the unified supergraph schema automatically deriving subset contract schemas.


## Get started


Contracts are available in preview today on Apollo’s Enterprise plan! If you’d like to learn more about Contracts, check out our[documentation](https://www.apollographql.com/docs/studio/contracts/) or join us virtually today at[GraphQL Summit](https://summit.graphql.com/) where we’ll be doing a deep dive and demo on contracts during the “How to Serve Many Audiences with One Graph” talk! If you aren’t on Apollo Enterprise and would like to try it out,[talk to us](https://www.apollographql.com/contact-sales/) about an Enterprise trial.


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
