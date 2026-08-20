---
schema_version: "1.0.0"
document_id: "e89b19a4ccd181d60bb0662808f41ad9256e7fe36ba27010453165ad8a5157a3"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/schema-checks-and-launches-are-now-available-for-free-in-apollo-studio"
published_at: "2022-05-18T08:50:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:31e6bf4662c78ddcd48327d6f5dc4c0523e9aeddb212f6a01bb09ec973befa02"
---

# Schema Checks and Launches are now available for free in Apollo Studio

Today, we’re updating Apollo Studio to make its[supergraph](https://apollographql.com/blog/the-supergraph-a-new-way-to-think-about-graphql) delivery pipeline, including[Schema Checks](https://www.apollographql.com/docs/studio/schema-checks) and[Launches](https://www.apollographql.com/docs/studio/launches) , available for anyone to use for free.


We’ve made a lot of exciting announcements about supergraphs recently. In April, we announced the[general availability of Federation 2](https://apollographql.com/blog/apollo-federation-2-is-now-generally-available/) , an improved version of Federation that makes it easier for anyone to build a supergraph. And just today, we announced[general availability of Apollo Router](https://apollographql.com/blog/apollo-router-is-now-generally-available) , our next generation supergraph runtime written in Rust.


With Federation and Router, anyone can build and run a supergraph. But the needs of your users evolve rapidly, and like any other layer of your stack, your supergraph needs to evolve with them. That’s where Apollo Studio comes in.


## Optimized for iteration: the supergraph delivery pipeline


The days of lockstep deployments are over. Teams need the ability to deploy changes many times a day without worrying about breaking production.


We built Apollo Studio’s delivery pipeline to help you evolve and iterate on your supergraph schema with speed and confidence. It’s a single source of truth that provides the automation, validation, and observability you need to deliver changes rapidly and safely. After building a supergraph and adopting a Studio’s delivery pipeline,[RetailMeNot](http://apollographql.com/customers/retailmenot) saw a 7X reduction in time spent deploying schema changes and eliminated production outages. Schema Checks and Launches are two core delivery features that make this possible.


### Schema checks


Schema checks validate changes to your supergraph schema so that you can be confident that it’s ready for production. There are two types of schema checks that Apollo Studio runs for supergraphs: composition checks and operation checks.


Composition checks validate that subgraphs compose successfully into the overall Supergraph schema. If composition succeeds, operation checks then validate that your composed supergraph schema will serve all of your client applications by checking it against historical operations on your supergraph.


The best part: schema checks can be run within existing CI/CD tools using Apollo’s Rover CLI to create a seamless integration with your existing software delivery pipelines.


### Launches


One of the fantastic benefits of GraphQL that a supergraph inherits is that it’s versionless. But even without a version, it’s still extremely important to understand how your supergraph has evolved over time and if new changes are live. Launches in Apollo Studio are how you do that.


Launches give you a way to reason about the lifecycle of your supergraph. In Apollo Studio, a launch represents the complete process of an update to your deployed supergraph running through Studio’s delivery pipeline. An update to your supergraph could include:


- Adding, removing, or modifying types and fields in a subgraph schema
- Adding or removing entire subgraphs
- Migrating types or fields between subgraphs


The Launches page in Apollo Studio gives you visibility into in-progress and past launches for your supergraph. It’s a dashboard that summarizes how launches over time have evolved your supergraph, helping you track when new changes are reflected in production and debug when something goes wrong.


## The supergraph pipeline for everyone


Supergraphs are transformational for developers, and we believe that everyone should be able to start building one. To make that a reality, as of today Apollo Studio’s end-to-end delivery pipeline is now available to everyone.


You can start using Schema Checks and Launches for free in Apollo Studio today. For instructions on how to get started, head over to the[Schema Checks](https://www.apollographql.com/docs/studio/schema-checks) and[Launches](https://www.apollographql.com/docs/studio/launches) docs.


If you don’t have a supergraph yet, check out[this post](https://apollographql.com/blog/the-supergraph-a-new-way-to-think-about-graphql) to learn about Apollo’s supergraph stack and how to get started!


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
