---
schema_version: "1.0.0"
document_id: "be9049ea43f04b62dc49fbd266995bf023b3d2c601826c3a994c9dae4962ea1c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/contracts-are-now-generally-available"
published_at: "2022-05-11T10:10:09+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:daaec2eb869fb4bcb0eaf178e7e4feaf7c17124118f47db0928bbb60179dcca0"
---

# Contracts are now generally available

At GraphQL Summit last year, we announced the[preview release of Contracts](https://www.apollographql.com/blog/announcement/platform/introducing-contracts-serve-many-audiences-with-a-unified-graph/) on Apollo’s Enterprise plan. Contracts allow you to create tailored graphs for different audiences by applying filters to a single unified graph. Today, we are excited to announce that Contracts are generally available!


## **One graph – many representations**


Building a unified graph is incredibly beneficial. It gives you a single source of truth for all your organization’s data, services, and business capabilities and allows developers to deliver better, faster apps with less overhead. However, unification can introduce new challenges at scale:


- How do you create varying levels of access for different personas or types of users like customers, employees, suppliers, or partners?
- How do you provide partners and public API users with a schema scoped for their integration and app development needs?
- How do you maintain client-specific BFFs and experience-driven schemas alongside a canonical domain-driven schema?
- How do you restrict access to experimental or non-stable parts of the graph, but still allow access for testing and early adopters?


In summary, **“how do I serve multiple different audiences with a single graph?”** The answer: **Contracts** !


Using Contracts, you can take your unified graph and create filtered variants of that graph that contain only a subset of its types and fields. Contracts can power separate graphs for partner developers, specific client applications, public APIs, experimental features, and more without losing any of the benefits of the unified graph.


## **What’s new in Contracts?**


Since announcing the preview, we’ve been hard at work making sure that Contracts integrates seamlessly with all of the other great features of Apollo Studio. We’ve even added in some net new functionality based on customer feedback! Here are some of the improvements we’ve made:


### Contract Launches


Earlier this year we added[Contract Launches](https://www.apollographql.com/blog/announcement/platform/introducing-contract-launches-see-the-lifecycle-of-contract-schemas-in-apollo-studio/) to the Launches dashboard in Apollo Studio. This gives you full visibility into the lifecycle of your contract schemas and helps you understand which contracts launched from a given source variant.


### Support for Federation 2 and tagging additional schema elements


In case you missed it,[Federation 2](https://www.apollographql.com/blog/announcement/backend/apollo-federation-2-is-now-generally-available/) – with a much improved shared ownership model, enhanced type merging, and cleaner syntax for a smoother developer experience – was made generally available recently. If you have migrated your graph variants to Federation 2, you can now create contracts from them.


Additionally, if your graph variants are on Federation 2, you can hide additional schema elements like input arguments, input types and fields and enums, which is currently not possible with Federation 1.


### Filter previews


We have also added the ability to see a *before* and *after* view of your contract schema once you have applied your filter configurations. This will allow you to quickly verify the changes you are making against the source variant schema or latest successful contract schema.


When creating or updating a contract schema, you can click on Generate Preview to see the number of types and fields the proposed contract schema will have. You can also see any errors that occur when generating the contract schema with the selected filter configuration.


On proceeding to Review, you can now see the changes that are being made based on the selected filter configuration. You can compare your proposed changes to the contract schema against the last successful contract schema build or to the source variant schema.


You can also select whether you want to compare API Schema or Supergraph Schema.


## **Getting Started**


Contracts are generally available today on Apollo’s Enterprise plan! To learn how to create your first contract, check out our documentation.


To get the most out of Contracts, we recommend migrating your source variants (from which contracts schemas are created) to Federation 2. Federation 1 supports Contracts as well, but it does not support tagging of input and enum types and fields.


If you aren’t on Apollo Enterprise and would like to try it out,[talk to us](https://www.apollographql.com/contact-sales/) about an Enterprise trial.


**⚠️ Note:** Make sure you are on the latest version of the UI. Click on the bell icon at the top right corner of Studio and if you see “Update available, click to relaunch” at the bottom left of the modal then click that to get the latest version.


*(Contracts are patent pending)*


Written by


Parul Schroff


[Read more by Parul Schroff](https://www.apollographql.com/blog/author/parul)
