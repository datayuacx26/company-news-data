---
schema_version: "1.0.0"
document_id: "30295a495986d6c38df774c8fea18237294f6edef9e8f139d17d80c4c09bdfd7"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/unblocking-teams-to-go-faster-with-apollo-federation"
published_at: "2021-04-06T07:28:53+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:af95d3ce6dfad3ef4e2e18b0e400760271cc1d24619df3bdcd7efd352543d3ee"
---

# Unblocking teams to go faster with Apollo Federation

Building software for the internet in 2021 is hard. It’s no longer just HTML, CSS, and JavaScript. We now have to deliver our products beyond just websites to mobile, voice, in-store, chat, TV, and more—while integrating data from many services and delivering on time. One of the biggest things that slow down developers is waiting to align work with other teams. It is common to get blocked while waiting to coordinate or waiting for their backlog to free up.


We launched Apollo because we believe GraphQL transforms how product engineering teams build new experiences by unblocking teams and allowing them to work in parallel. Front-end teams can build new experiences faster by directly querying the graph and not waiting for new REST endpoints to be built. As each new capability is added to the graph, it’s available for all other teams to take advantage of it.


As we worked with early GraphQL adopters, we realized they needed a solution that could serve the needs of engineering organizations that were running at scale. They wanted the power of a single endpoint coupled with the flexibility of distributed ownership. So in 2019, we launched[Apollo Federation](https://www.apollographql.com/blog/apollo-federation-f260cf525d21/) and Apollo Gateway, a Federation-compatible graph router, to help teams use GraphQL in distributed architectures. With Federation, each service or domain team can own their part of the graph schema, known as the subgraph, while building a unified common graph for their developers.


Next, we launched Apollo’s[Managed Federation,](https://www.apollographql.com/blog/announcing-managed-federation-265c9f0bc88e/)) a hosted solution for scaling changes to your federated graph pipelines across many teams. Managed federation helps you reliably deploy schema changes to your Apollo Gateway, including checks for composition integrity. It prevents shipping breaking change by using your live traffic as an automated test suite. We’re thrilled by the growth and acceptance of Managed Federation by hundreds of the world’s most innovative companies. Apollo customers now run more than 2 billion queries a day through Apollo federated data graphs, representing a 400% annual growth.


Equally validating is the industry adoption of the Federation Specification. In addition to Apollo’s Server, there are now more than a dozen[GraphQL servers](https://www.apollographql.com/docs/federation/other-servers/) that support federation. Netflix open-sourced their[DGS](https://netflix.github.io/dgs/) GraphQL Server that was built for SpringBoot and they are[betting their entire graph strategy](https://www.infoq.com/presentations/netflix-api-graphql-federation/) on Federation. Companies including[Adobe](https://medium.com/adobetech/graphql-making-sense-of-enterprise-microservices-for-the-ui-46fc8f5a5301) ,[Walmart](https://medium.com/walmartglobaltech/federated-graphql-walmart-bfc85c2553de) , and[StockX](https://www.apollographql.com/blog/9-lessons-from-a-year-of-apollo-federation/) are all choosing Federation. Database vendors are also adopting Apollo Federation as a graph integration strategy. The latest to join is DataStax whose engineers are adding Apollo Federation to the open-source data gateway[Stargate](https://stargate.io/) , delivering Federation-ready GraphQL APIs to access Apache Cassandra™ stored data.


The most important signal that Federation is solving real-world problems are the stories we’re hearing from our customers:


## **Fast-moving startups powering their product development with Federation**


[Modern Health](http://modernhealth.com/) , a startup founded in 2017, chose Apollo Federation to help power their service-oriented architecture strategy and fulfill their mission of democratizing access to mental health.


*“We chose Apollo so we can continue to focus on our company mission – destigmatizing and making mental health more accessible for employees across the world. Apollo’s Federation empowers our engineering teams to work independently while collaborating on a rapidly growing graph that is becoming central to how we deliver industry-leading products and features to our growing global customer base.”* – Nick DiRienzo, Software Engineer, Modern Health


## **Looking to the future**


We’re excited that Federation has become the industry-standard choice for distributed GraphQL and today our teams are announcing several new integrated Federation tools including composition validation and webhooks in Apollo Studio as well as:


- **[Workbench](https://github.com/apollographql/apollo-workbench-vscode)** is a VS Code plugin that helps developers collaborate on schema design in a federated graph
- **[Rover CLI](https://www.apollographql.com/docs/rover/)** is a new tool to access all the power of Federation from the command-line and integrates well in all languages and CI environments.


You can learn more about these tools[here](https://www.apollographql.com/blog/powerful-new-graphql-tools-for-apollo-federation) and at[GraphQL Summit Worldwide](https://summit.graphql.com/) on April 7th where we will be talking about some of the new enhancements to our platform, and[GraphQL Summit Scale](https://summit.graphql.com/webinars/graphql-summit-scale-presented-by-apollo/) on April 8th where we will talk about the technical, cultural and organizational best practices for scaling GraphQL.


Written by


Matt DeBergalis


[Read more by Matt DeBergalis](https://www.apollographql.com/blog/author/matt)
