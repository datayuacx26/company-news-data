---
schema_version: "1.0.0"
document_id: "9108e21ba95f5d48b2a5b66d711084e25d7b5e32666aba36bfa349b12de05ef9"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/the-supergraph-a-new-way-to-think-about-graphql"
published_at: "2022-05-18T09:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:751885b9e58444c968772ded7777b85c2995f4a1f0ae12e79c94d6917dd7f900"
---

# The supergraph: a new way to think about GraphQL

We’ve had a busy month at Apollo. Over the last several weeks we’ve released some significant updates to the Apollo stack. Last month we shipped[Federation 2](https://www.apollographql.com/blog/announcement/backend/apollo-federation-2-is-now-generally-available/) , a sophisticated and more flexible approach to GraphQL composition. And last week we made[Contracts](https://www.apollographql.com/blog/announcement/platform/contracts-are-now-generally-available/) generally available, a way to tailor your graph to specific use cases like mobile development or creating public GraphQL APIs.


Today we’re launching updates to two more components of our stack: the[GA release of Apollo Router](https://apollographql.com/blog/apollo-router-is-now-generally-available) – our new high-performance runtime for federated GraphQL APIs – and a[new cloud-hosted build pipeline](https://apollographql.com/blog/schema-checks-and-launches-are-now-available-for-free-in-apollo-studio/) for delivering changes to a graph.


We’ve been building these tools together to help teams implement a more advanced GraphQL architecture – one that delivers essential capabilities for operating mission-critical graphs in organizations with many teams, programming languages, and technologies. That architecture is now at the heart of API strategy at companies like Netflix, Walmart, Expedia, Zillow, RetailMeNot, Paypal, Major League Baseball, and many more.


We think it’s time to give this architecture a name, and after talking about it with the developers that have pioneered this approach in their companies, we’re calling it **the supergraph** .


## The principles of supergraph architecture


The essential ideas of the supergraph are going to be[pretty familiar to those who’ve been following Apollo for a while](https://principledgraphql.com/) . It comes down to three core principles.


### 1: a unified composition layer


The supergraph is a new way to think about implementing GraphQL that goes far beyond the technical benefits of replacing data fetching and BFF code with schema and queries. It’s a unified network of a company’s data, microservices, and digital capabilities that serves as the “composition layer” for the whole organization.


The supergraph empowers everyone to create richer, more engaging experiences for their users and customers by bringing everything into a single point of collaboration.


### 2: a modular architecture


Supergraph architecture is declarative and modular. Just as React and Kubernetes leverage modularity to scale UI development and service deployment, the supergraph leverages modularity to scale the API layer as it grows beyond a single team.


Apollo Federation has always been about bringing modularity to the API layer – letting you scale a graph without bottlenecking developers on a single monolith or creating a single point of failure.


Of course, Apollo Federation offers a lot of additional advantages along the way: programmable configuration, standardized tooling, analyzable query plans, strict validation at build-time instead of run-time, great support for heterogeneous environments, a structured on-ramp for new developers, and many more.


### 3: an agile approach


The supergraph enables an agile bottom-up approach. Traditionally, APIs tend to be fairly static. Because it’s so hard to change an API, the tendency is to waterfall a design that anticipates future requirements. But products change constantly, driven by evolving user requirements and feedback.


The supergraph works harmoniously with rapidly changing products because it empowers developers to ship schema changes swiftly and safely to support their own initiatives, causing the schema to naturally evolve based on actual usage and the needs of the business.


## On to the supergraph


We’ve seen case after case of the supergraph architecture – a unified layer, made from modules, continuously evolving – emerging organically at companies we respect and admire. So we’re focused on this idea of the supergraph and our ambition is to help every team have one. Here’s how all that we’ve been building helps with that goal.


### Apollo Federation: the spec


Apollo Federation is our open specification for composing subgraphs – modular GraphQL services – together into a single supergraph. Each subgraph is a self-contained component with its own schema and server, so it can be developed, tested, and deployed independently.


Rather than relying on custom code to combine subgraphs, subgraphs reference and extend each other through a principled set of primitives. Thanks to our incredible developer community, Federation supports subgraphs written in over[20 programming languages](https://www.apollographql.com/docs/federation/other-servers) and a growing number of GraphQL-native services and products.


[Federation 2](https://www.apollographql.com/blog/announcement/backend/apollo-federation-2-is-now-generally-available/) builds on what we’ve learned from our community. It adds more complete support for the GraphQL spec including interfaces and new directives which can eliminate much of the boilerplate code that came with Federation 1.


Federation 2 also streamlines cross-team collaboration with a flexible shared ownership model that allows multiple subgraphs to implement overlapping, complementary parts of types and schemas. New governance directives like` @inaccessible` ,` @shareable` , and` @override` give teams fine-grained control over the composition process and resulting supergraph schema.


TLDR; Apollo Federation is now more approachable and incrementally adoptable for anyone interested in building a supergraph, and it scales seamlessly all the way into enterprises with thousands of developers collaborating on the largest rapidly-evolving supergraphs. Check out[the Federation quickstart](https://www.apollographql.com/docs/federation/quickstart/setup/) to learn more.


### Apollo Router: the runtime


So you’ve created a federated schema, but how do you run it? At a minimum, the runtime for a supergraph has to receive client requests, build an execution plan, route requests to the proper subgraphs, assemble the results back together, and return it back to the client. We launched Apollo Gateway — an add-on to Apollo Server — alongside Apollo Federation to serve this purpose, but we quickly realized that we needed to build a more elegant solution that was purpose-built for the task. We decided to call it a **graph router** .


A graph router does far more than a traditional API gateway. Like a relational database, it requires a sophisticated implementation that shields users from unwanted details of execution while still providing a principled way to analyze and control the system when necessary. Critical infrastructure like this is better written in a lower-level systems language that offers a smaller operational footprint, so we opted for Rust instead of Node.js.


At the end of 2021, we released the world’s first and only graph router: Apollo Router. The performance advantages are significant: Apollo Router processes requests with **10x lower latency** , **10x higher throughput** , and **12x lower variance** compared to Apollo Gateway, its JavaScript predecessor. Take a look at the benchmark below to get a sense of how much better it is.


Response latency vs request throughput for Gateway, Router, and direct-to-subgraph


There are other benefits too, like an[extension facility](https://www.apollographql.com/docs/router/customizations/rhai) that lets you replace custom gateway code with configuration (this should sound familiar!) and comprehensive support for[OpenTelemetry](https://www.apollographql.com/docs/router/configuration/tracing) .


As of today,[Apollo Router is generally available](https://apollographql.com/blog/apollo-router-is-now-generally-available) , free for anyone to use. Check out the[Router docs](https://www.apollographql.com/docs/router/) to learn more.


### GraphOS Studio: the control plane


GraphOS Studio is the final piece of the puzzle for building and operating a supergraph. Its delivery pipeline provides the validation, automation, and observability you need to deliver changes rapidly and safely, making it possible to constantly evolve the supergraph schema, particularly when teams leverage and extend each other’s contributions.


[Schema Checks](https://www.apollographql.com/docs/studio/schema-checks) and[Launches](https://www.apollographql.com/docs/studio/launches) prevent breaking changes before they happen by automatically validating your supergraph schema in development and giving visibility into past and in-progress changes to the supergraph.


Previously only available on our Enterprise plan,[today we are making Studio’s delivery pipeline available to everyone](https://apollographql.com/blog/schema-checks-and-launches-are-now-available-for-free-in-apollo-studio/) .


## What’s next for the supergraph


We’re excited to get the core components of the supergraph architecture in your hands. We can’t wait to build with you and we are eager to work together on what’s next.


Written by


Matt DeBergalis


[Read more by Matt DeBergalis](https://www.apollographql.com/blog/author/matt)
