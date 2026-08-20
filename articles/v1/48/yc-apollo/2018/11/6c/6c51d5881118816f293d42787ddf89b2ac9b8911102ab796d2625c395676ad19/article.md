---
schema_version: "1.0.0"
document_id: "6c51d5881118816f293d42787ddf89b2ac9b8911102ab796d2625c395676ad19"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-the-apollo-graphql-platform-8ef34bb269e5"
published_at: "2018-11-07T22:26:24+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:beb2f9264224a84d8eb36106a62a54a9b819d316b554a713ee628d7254b8cfb5"
---

# Introducing the Apollo GraphQL Platform

We started Apollo in 2016 with the objective of lowering the barriers to making high-quality, modern apps. Since then, we have seen tremendous growth in our community with nearly 500,000 downloads per week and companies like Audi, the New York Times, Airbnb, and CNBC all making strategic commitments to GraphQL. With 800+ attendees at our third annual GraphQL Summit this week and yesterday’s launch of an initiative to form a GraphQL standards body, GraphQL has emerged as the next big advance in app development.


Amidst this momentum, we’ve been fortunate to work with many companies who’ve taken Apollo into large-scale production. From that experience we’ve identified a set of best practices and techniques for doing GraphQL right, and we’re excited to share how we’ve brought those back into Apollo.


## A platform fit for every team


Today, we’re introducing the open-core[Apollo GraphQL Platform](https://www.apollographql.com/platform/) for product teams of all sizes to do GraphQL right. It builds on our core open source and Apollo Engine technology to give organizations at all stages of development the tools they need to adopt GraphQL incrementally, on top of the code and services they already have, all while baking in the best practices and workflows we’ve learned from working with teams moving to GraphQL.


An architecture diagram of the Apollo platform


Our important lesson we’ve learned is that GraphQL is really about collaboration, and the teams that get furthest are those who work closely together on their GraphQL API. To facilitate this collaboration, we built a **schema registry** that serves as the source of truth and a central collaboration point for GraphQL development, **client and** **operation registries** that track each known consumer of the data graph, and a **trace warehouse** that stores a structured log of each operation made against your schema. These registries all work together to give your team a precise understanding of your API, like knowing which clients are still accessing a deprecated field or determining which microservices a particular GraphQL query accesses.


On top of these, we built new tools and workflows for GraphQL developers. The new Apollo **VS Code plugin** puts valuable information about your schema — like the average latency of a specific field — right at your fingertips at development time. A plugin for Apollo Server implements **safelisting** , locking your API down to trusted, pre-registered operations. We also added **schema validation** , which tests a proposed schema against the known set of operations observed in production, returning a list of affected clients for each breaking change.


And finally, we added a **GraphQL gateway** , implemented as a commercial plugin for Apollo Server. The gateway delivers on a key enterprise need: a way for multiple teams to collaborate on a single, org-wide schema without mixing everyone’s code together in a monolithic single point of failure. Instead, the gateway composes separately deployed “micro-schemas” that reference each other into a single master schema, which looks to a client just like any regular GraphQL schema. To answer queries, the gateway builds a query plan, fetches data from each upstream GraphQL service, and assembles it all back together into a single result. Since the gateway is just an Apollo Server, the rest of the platform — safelists, schema change validation, and so on— is all tied in and works great.


## Open source, open-core


We’re an open source company. Hundreds of you have worked with us to make Apollo Client and Apollo Server the industry-leading implementations of GraphQL on both sides of the wire. They’re the centerpiece of the new platform. But, there’s still a ton more to do and we welcome (and need!) your help.


Our strategy is to make transformative and groundbreaking tools — built from open source, commercial plugins, and cloud services — and package them in a fair and transparent way that keeps us aligned with your interests. Building our business and funding our work by making and selling valuable software keeps us honest, hungry, and focused on what developers really need, and ultimately results in a better GraphQL for everyone.


Here’s our model for “what goes where:”


- Core technology that implements the GraphQL spec and the standard resolver-based programming model stays in open source, such as Apollo Client and Server. Additionally, our building blocks and low-level libraries, like CLI tooling for codegen and the Apollo language server, work best as common implementations shared across the community, so they will continue to be open source and free forever.
- Services (eg, the trace warehouse), features designed for large-scale enterprise environments (eg, distributed query execution), and workflows built on top of these (eg, schema change validation) are what we’ll sell commercially and build our business on.


## Pick your flavor


The platform comes in three flavors, based on that open-core model:


- **Community edition** is our free-forever, unlimited-use offering that includes all the core open source Apollo libraries and tools — including the new VS Code plugin — plus the complete schema registry to tie it all together. We also include parts of the trace warehouse here to enrich registered schemas with useful metrics.
- **Team edition** is for small and medium-sized companies that want all the Apollo tools and workflows. This comes with full use of the trace warehouse and operation registry. Team starts at just $49/month, with pay-for-what-you-use pricing up to 25 million monthly queries.
- **Enterprise edition** is for teams and organizations running mission-critical GraphQL APIs. It includes the enterprise Apollo GraphQL Gateway, and also 24x7x365 SLA-backed production support for the entire platform.


For more details on what’s in each package, please check out[our plans page](https://www.apollographql.com/plans) . If you’re currently using Apollo Engine, we’ll reach out with details on how to switch to a platform plan so you can take advantage of all the new features.


We’re excited to introduce the[Apollo GraphQL Platform](https://www.apollographql.com/platform) and show off some of the ways it’s already being used at[GraphQL Summit](https://summit.graphql.com/) this week. If you’re at the conference, we would love to hear what you think of the platform and learn how we can help your team be successful with GraphQL.


To get started today, visit our[new platform docs](https://www.apollographql.com/docs/) for detailed guides on all of the new features and a tutorial that ties all of the pieces together.


Written by


Matt DeBergalis


[Read more by Matt DeBergalis](https://www.apollographql.com/blog/author/matt)
