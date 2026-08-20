---
schema_version: "1.0.0"
document_id: "9f78807be677c20ca53dced0e8b4dd6855ec1732d7ead5bdeca8b42c29672e3a"
company_key: "airbnb-inc-class-a-common-stock"
company: "Airbnb Inc."
source_id: "airbnb-inc-class-a-common-stock-rss-d1a7958eece3"
canonical_url: "https://medium.com/airbnb-engineering/viaduct-1-0-and-the-future-of-airbnbs-data-mesh-6bab4ec98b89"
published_at: "2026-05-13T17:01:01+00:00"
first_seen_at: "2026-07-20T04:35:41.377728+00:00"
fetched_at: "2026-07-28T22:13:13.812190+00:00"
content_hash: "sha256:688f8e215c0a4a6ecc9e3d756d0a3d8df92271484626fdb9fceebbe1cbc1d869"
---

# Viaduct 1.0 and the future of Airbnb’s data mesh

# **Viaduct 1.0 and the future of Airbnb’s data mesh**


[Adam Miskiewicz](https://medium.com/@skevy?source=post_page---byline--6bab4ec98b89---------------------------------------)


5 min read


·


May 13, 2026


--


Moving from an internal tool to a community-driven, production-ready data mesh.


Press enter or click to view image in full size


**By** :[Ryan Tanner](https://www.linkedin.com/in/ryanetanner/) ,[Raymie Stata](https://www.linkedin.com/in/rstata/) ,[Adam Miskiewicz](https://www.linkedin.com/in/adammiskiewicz/)


## Introduction


We’re excited to announce the 1.0 release of the Viaduct. This release marks a shift from Viaduct being an Airbnb-internal tool that happens to be open source to a true community-driven project with a stable public API. The 1.0 release includes substantial new features and enhancements which we describe in the[Viaduct blog](https://viaduct.airbnb.tech/blog/2026/05/13/viaduct-10-whats-new/) .


Viaduct is for platform engineers building a company-wide data API, service owners who want to contribute to a shared graph without spinning up their own server, or engineering organizations that have outgrown a single GraphQL service.


## What is Viaduct?


Viaduct is Airbnb’s data-oriented service mesh, a GraphQL-based system that provides a single interface for accessing and interacting with any data source. For years it has supported Airbnb’s data infrastructure, allowing product engineers to access data efficiently and safely, while enabling service owners to decouple implementation details from the API surface.


## What is a data-oriented service mesh?


A Viaduct service mesh is defined in terms of a GraphQL schema consisting of:


- *Types* (and interfaces) describing data managed within your service mesh
- *Queries* (and subscriptions) providing means to access that data, abstracted from the service entry points that provide the data
- *Mutations* providing ways to update data, again abstracted from service entry points


## Why Viaduct?


Viaduct was built to solve a specific problem faced by most organizations that adopt GraphQL strategically: **decentralized development of a central schema** .


## Why a central schema?


A central schema provides a single, consistent interface to the full range of an organization’s data and capabilities. Instead of every client needing to know which backend service to call, they interact with one unified graph that connects all of an organization’s domains. This makes APIs easier to discover, enables richer cross-domain queries, and provides a consistent place to enforce policies, observability, and schema governance.


## Why decentralized development?


A central schema only works if it can evolve quickly. The domain experts who understand each part of the business must be able to design and implement the parts of the schema they know best. A central team cannot own everything, and shouldn’t try to. The challenge is giving teams autonomy over their own domain contributions while preserving the coherence and stability of the shared schema.


Viaduct solves this through multi-tenancy. A shared multi-tenant runtime hosts independently developed and tested tenant modules, each owning a portion of the schema. A team wanting to contribute simply creates a directory for their module, defines their schema definition language (SDL) and resolvers, and they are ready to serve. There is no need to set up or operate a separate GraphQL service, manage router composition, or become experts in GraphQL infrastructure. Teams focus on domain logic; the platform handles execution, scaling, and integration.


## Viaduct and GraphQL Federation


We’re frequently asked how Viaduct compares with GraphQL Federation. Both address the same problem — decentralized development of a central schema — but they take different approaches.


Federation distributes development through services. Each team owns and operates its own GraphQL subgraph server; those subgraphs are composed by a federation router into a single unified graph. Viaduct distributes development through modules. A shared multi-tenant runtime hosts tenant modules that define and implement portions of the schema.


**Federation distributes development by distributing servers. Viaduct distributes development by distributing modules.**


We don’t see Viaduct as an alternative to federation, but as a complement to it. Viaduct can participate as a subgraph within a federated architecture. In a large organization where hundreds of teams contribute to the overall graph, a federated approach requires running hundreds of independent subgraph servers. With Viaduct, organizations can instead run a smaller number of Viaduct instances, each hosting many closely related tenant modules. Federation can then compose those instances into a larger enterprise graph.


## Community


The “1.0” designation for Viaduct is a commitment to stability. Until now, Viaduct has evolved rapidly to meet internal needs, often with breaking changes managed through our internal monorepo tooling. Public release required a different approach. We have applied @StableApi, @ExperimentalApi, and @InternalApi annotations across all public surfaces, and we run Kotlin’s binary compatibility validator in CI to catch breaking changes before they ship. Viaduct is now published to Maven Central with automated releases and Dokka-generated API documentation.


We are committed to developing Viaduct in the open. Our intent is to involve the community in major architectural decisions before code is written, not after. Our first public discussion is the[Connections RFC on GitHub](https://github.com/airbnb/viaduct/discussions/271) , and we plan to continue in that direction. Our goal going forward is to be a genuine community project, not simply an internal project that happens to be open source.


Whether you are looking to unify your data layer, contribute to the core engine, or build on top of the graph, now is the time to get involved.[Begin with Getting Started →](https://viaduct.airbnb.tech/getting_started/)


If this type of work interests you, check out some of our[open roles](https://careers.airbnb.com/) !


## GraphQLConf 2026


Heading to[GraphQLConf](https://graphql.org/conf/2026/) next week? Check out four Viaduct-powered conversations by Airbnb engineers on May 20th.


Speaker:[James Bellenger](https://graphql.org/conf/2026/schedule/0a67525d56d469af7df6fc4763e3f75e/?name=Brute+Force+Correctness+-+James+Bellenger%2C+Airbnb)


Time: 3:50–4:15 PM PST


This talk explains how probabilistic testing exposes hidden bugs in complex GraphQL systems — demonstrated by Airbnb’s launch of a new GraphQL engine — and shows how you can use the same approach to harden your own systems.


Speaker:[Vickey Yeh](https://graphql.org/conf/2026/schedule/d1bdf1a3eb90cb599c172cbdfa7fdd1c/?name=Observability+for+a+Multi-Tenant+GraphQL+Gateway+at+Scale+-+Vickey+Yeh%2C+Airbnb)


Time: 1:55–2:20 PM PM PST


A look at how Airbnb’s Viaduct system lets each team easily monitor and debug its own code — using built-in ownership tags, automatic alerts/dashboards, and cost-aware tracing — so everyone can treat their part of the shared service as if it were their own.


Speakers:[Linquan Zhang and Cetlin Sahin](https://graphql.org/conf/2026/schedule/a2d6aff0874a12a86810f7ffac23d12d/?name=Sharding+a+GraphQL+Gateway+for+Blast+Radius+Reduction+-+Linquan+Zhang+&+Cetin+Sahin%2C+Airbnb=)


Time: 2:30–2:55 PM PST


We will cover how we architected our sharding solution and how it improved our operational abilities. You will gain a clear understanding of how our implementation tradeoffs have fared over time, key production insights gathered since rollout, and strategies to evolve a GraphQL gateway towards greater isolation without fragmenting the API surface.


Speaker:[Michael Rebello](https://graphql.org/conf/2026/schedule/f95ae2cfb0b6a5e2e0dfa5971f531749/?name=GraphQL+Data+Mocking+at+Scale+With+LLMs+and+%40generateMock+-+Michael+Rebello%2C+Airbnb)


Time: 3:05–3:30 PM PST


Producing valid and realistic mock data for prototyping and testing has been an unsolved challenge for years. Mock data is tedious to write and maintain, but attempts to improve the process such as random value generation and field stubbing fall short as they lack essential domain context to make test data realistic and meaningful. In this talk, I’ll share how we’ve reimagined GraphQL mocking at Airbnb by combining existing GraphQL infrastructure, rich product and schema context, and LLMs to generate convincing, type-safe mock data simply by adding a directive (@generateMock) to a field or operation.


Whether you’re fine-tuning a single service or running a multi-tenant gateway, these sessions will equip you with practical strategies to build robust, observable, and developer-friendly GraphQL systems. See you on May 20th!


## Acknowledgments


Thanks to the entire Viaduct team, and especially Aileen Chen and Raymie Stata, for the tireless work on Viaduct Modern.


*All product names, logos, and brands are property of their respective owners. All company, product, and service names used in this website are for identification purposes only. Use of these names, logos, and brands does not imply endorsement.*
