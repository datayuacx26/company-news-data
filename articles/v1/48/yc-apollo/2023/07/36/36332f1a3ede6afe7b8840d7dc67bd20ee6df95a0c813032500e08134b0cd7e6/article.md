---
schema_version: "1.0.0"
document_id: "36332f1a3ede6afe7b8840d7dc67bd20ee6df95a0c813032500e08134b0cd7e6"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/manage-versions-of-your-api-for-third-parties"
published_at: "2023-07-14T11:33:45+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:acb6fb8a0fc0d61c60b17e3c8952892434cccfdadec35f471374d73188f99025"
---

# Manage versions of your API for third-parties

This post is a part of our “[How to power modern media apps with Apollo GraphOS](https://www.apollographql.com/blog/platform/media/how-to-power-modern-media-apps-with-apollo-graphos/) ” series. Also in this series:


- [Render frontends faster](https://www.apollographql.com/blog/platform/media/render-frontends-faster)
- [Enable omnichannel experiences](https://www.apollographql.com/blog/platform/media/enable-omnichannel-experiences)
- [Providing real-time data for consumers](https://www.apollographql.com/blog/platform/media/provide-real-time-data-for-consumers)


---


The ability to consume media on-demand from virtually any device with a screen is something that we often take for granted today. As a result, the volume of data that needs to be supplied by media companies providing this content has also increased dramatically.


As media companies have inevitably grown as enterprises, it has become increasingly apparent that having a unified understanding of their APIs and data sources is incredibly powerful, and it is exactly this that GraphQL enables by providing a singular entrypoint to all data within an organization.


However, building and scaling GraphQL schemas has also historically come with its own challenges as teams try to consolidate data from multiple services behind a single API endpoint for front-end developers to query. This situation can become even more complicated when you also need to expose a subset of that data to third-party client applications.


Luckily, with GraphOS, you can have the best of all worlds where you unify your data behind a single endpoint, allow service owners to manage relevant portions of GraphQL schema independently, and also expose the right amount of data to different kinds of clients.


## Benefits of federating your graph


Leveraging Apollo Federation with Apollo GraphOS helps alleviate these pain points as it enables independent teams to own and develop their subgraph – empowering engineering teams to have the autonomy to contribute to their Subgraphs independently, while also providing the toolsets and controls that enable well managed composition of the supergraph, which can then be exposed to clients via an instance of Apollo Router as pictured below:


Due to numerous disparate data sources, as well as the requirement for a consolidated entry point across channels, Apollo Federation is a compelling architectural approach for media companies that need to modernize their API strategy and increase developer efficiency.


However, despite the benefits that come with adopting Apollo Federation, it alone does not provide all of the answers when you need to provide multiple variations of your graph. For example, consider the need for an internal or externally facing subset of your schema. More specifically, as is often the case for media companies, the need to provide a subset of a supergraph schema to third parties while hiding possibly confidential internal fields.


Historically, the only way to accomplish this was to manage multiple graphs with all of the governance and deployment overheads that came with it. Thankfully, Apollo GraphOS provides an answer to this as well through[contract variants](https://www.apollographql.com/docs/graphos/delivery/contracts) .


## Contracts with GraphOS


Contracts provide the ability to pick and choose types, fields, and field arguments from your supergraph schema to create multiple different variations of your graph from a single source of truth, as pictured below:


As can be seen from the diagram above, contracts provide bespoke subsets of your schema driven from a single, shared supergraph.


Contracts enable you to create logically separated views of your single source of truth – the supergraph – which means that you only need to maintain a single unified graph. This powerful feature of GraphOS allows you to avoid huge additional maintenance overheads, while still being able to provide your known first-party clients or other third parties consumers with a tailored variation of the graph.


In addition to removing the need for duplication of effort across teams, deriving contract variants from a single supergraph helps prevent inconsistencies and divergence between versions that may have occurred as subgraph-owning teams iterate their portions of the overall schema over time.


## Applying tags for Third Parties


Creating a contract variant only requires the application of a special directive in a subgraph schema and a small amount of configuration in Apollo Studio.


In order to enable Apollo Studio to detect fields that should be either included or excluded from a new contract, you can apply the @tag directive where required in your schema. Consider the case where a subgraph has the following type definitions:


```text
type Media @key(fields: "id") {
id: ID!
title: String
description: String
mediaUrl: String
releaseDate: String
internalClassification: String!
}


type User @key(fields: "id") {
id: ID!
username: String
accountId: String
}
```


Here you can see we have the definition of an object type called Media and User. However, the Media type includes an internalClassification field and the User type also includes an accountId field, both of which shouldn’t be exposed to third party applications that consume the GraphQL API. Let’s introduce the tag directive as follows in order to label the fields appropriately:


```text
type Media @key(fields: "id") {
id: ID!
title: String
description: String
mediaUrl: String
releaseDate: String
internalClassification: String! @tag(name:"internal")
}


type User @key(fields: "id") {
id: ID!
username: String
accountId: String @tag(name:"internal")
}
```


As you can see above, the @tag directive has been applied to the field and given a name of \`internal\`. Once this updated schema has been published to Apollo Studio, the tag is detected and it is possible to create a contract that filters this field:


Here the contract has been given a name of “external-api” as the intention for this contract is to provide a subset of the source graph that excludes the internally tagged fields, thus making it suitable for external users. It is then possible to select the method of filtering, which in this case is an excludes filter, as the goal is to simply exclude the tagged fields:


As shown above, Apollo Studio then provides a preview of the proposed contract schema, which as shown above is successfully removing the two fields as intended. Once reviewed and created we will be able to see the resulting schema which has sensitive fields removed for that specific contract variant:


```text
type Media @key(fields: "id") {
id: ID!
title: String
description: String
mediaUrl: String
releaseDate: String
}


type User @key(fields: "id") {
id: ID!
username: String
}
```


As seen in the resulting schema, the contract has been created hiding the sensitive fields from third-party consumers. However, It is important to consider the method of filtering when creating contracts as, in practice, leveraging an “excludes” filter does come with the risk of possibly leaking fields because it requires very careful management of which fields external users can see and all fields are included in the resultant schema if no tag is provided.


Generally speaking an “includes” filter is the recommended approach as it requires an explicit “Allow” of fields to be included in externally facing APIs. Every use case will differ and the most appropriate filtering approach will be dependent on the intention for the contract. Note that both filtering approaches can be used simultaneously, so the nuances of your use case should be carefully evaluated when determining how to best implement contract variants.


It is also important to note that each contract requires a separate instance of Apollo Router that is connected to the specific contract variant. As a result, it is not possible for a client to query internal fields on the exposed contract as those fields are marked as inaccessible and are genuinely unavailable on that instance of Apollo Router, meaning they cannot be resolved.


## Deploying new Contracts


Because each contract variant will run on a unique instance of Apollo Router, you have the ability to separately manage the CI/CD workflows for that contract variant with independent checks. Any required updates to the schema are managed via the “source variant”, which is the variant of your graph that the contract filters were applied to.


This ensures that there is only a single source of truth that needs to be managed, removing the need to govern multiple graphs or any risk of divergence. Therefore, all changes are pushed to the source variant and Apollo Studio introduces the concept of “downstream variants.” Contracts are considered downstream variants of your graph, meaning that the updates are first applied to the source variant as it might be serving additional contracts or clients directly, and once that is complete, downstream checks are run to control any potential breaking changes to the contract specifically:


It’s worth noting that by default, issues with the downstream variants **don’t** block the source variant. This is important as source variants might be serving multiple other downstream variants or clients themselves.


However, it is possible to configure downstream checks to be blocked if required, which may be beneficial to ensure consistency across variants. Ultimately, Apollo Studio provides not only the ability to create contract variants in a simple way, but also the ability to test and check these new variants as part of the overall CI/CD process, providing increased governance, security and critically predictability to the graph.


## Summary


Overall, contract variants in GraphOS are a powerful way to manage multiple unrelated logical views of your graph from a single source of truth. Contracts help organizations easily provide an external API securely and simply with a low maintenance overhead.


It is crucial to consider the filtering approach used to create the contracts, whether include or exclude and the risks of possibly leaking fields, but of course, there is no right or wrong answer as the best approach will be dependent on use case and context.


Ultimately, contracts remove the need to manage multiple supergraphs tailored to a fixed experience requirements, and provide a simple method of creating and managing multiple variations which can be best hosted and provided via GraphOS.


## Get started with a media supergraph today


The best way to see the possibilities of a supergraph is to try one out.[You can explore a media supergraph and run real queries against it here.](https://studio.apollographql.com/graph/Media-Demo-Graph/variant/current/explorer)


We also have a series of case studies and blogs that dive into different elements of a typical media organization’s schema to illustrate how Apollo GraphOS help power essential features of modern media applications:


- [Provide real-time data for consumers](https://www.apollographql.com/blog/platform/media/provide-real-time-data-for-consumers)


If you’d like to talk to an Apollo expert about how a supergraph can power your media services,[please reach out to us.](https://www.apollographql.com/contact-sales/?referrer=media-solutions-page)


Written by


Joe Devine


[Read more by Joe Devine](https://www.apollographql.com/blog/author/joe)
