---
schema_version: "1.0.0"
document_id: "af4ee48045154e477aab087c51a88603234ace52c5c74fca4022a0aecd98fe58"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-summit-2024-whats-new-in-apollo"
published_at: "2024-10-09T08:29:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:6844cf516eb765c98926398ad5aa477247d7c9b37a932b253d942dd2dc9ff812"
---

# What’s new in Apollo GraphOS

Since our last[Apollo GraphOS update](https://www.apollographql.com/blog/graphos-summer-2024-launch-recap) , we’ve been hard at work to deliver some powerful features and improvements in[Apollo GraphOS](https://www.apollographql.com/graphos) that enhance developer experience, boost performance, and improve security and we’re excited for everyone to get their hands on it.


This major development was announced at[GraphQL Summit 2024](https://summit.graphql.com/) , but you can get the highlights of this — plus an overview of everything else that’s new with Apollo GraphOS — below.


## Design, build and operate at scale with GraphQL and Apollo


GraphQL has evolved from a tool for improving mobile app performance to becoming the backbone of modern API platforms, powering critical applications for major companies including the New York Times, Wayfair, Intuit and others. In fact a[recent report](https://www.apollographql.com/resources/gartner-when-to-use-graphql-to-accelerate-api-delivery) from Gartner® predicts that by 2027, more than 60% of enterprises will use GraphQL in production, up from less than 30% in 2024.


At its core, GraphQL makes your APIs better. It’s not about replacing your existing APIs, but about maximizing their value and potential. With[Apollo GraphOS](https://www.apollographql.com/graphos) powered by its[federated GraphQL](https://www.apollographql.com/federation) architecture, GraphQL can be leveraged as a complementary platform that sits atop existing services, helping magnify an organization’s API investments. By providing a single semantic layer, the GraphOS platform provides a unified way to access the data from any API in an organization and optimizes developer experience, accelerates product development, and prepares APIs for both the experiences we’re building today and the intelligent apps to come.


To further empower organizations in this API-driven future, we’re thrilled to announce several groundbreaking features and enhancements to our flagship product, Apollo GraphOS.


## Introducing Apollo Connectors for REST APIs


We’re excited to announce[Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors) , a new way to streamline GraphQL adoption by simplifying the integration of REST APIs into a federated GraphQL environment. Available now in public preview, Apollo Connectors eliminate the need to write redundant resolver code or manage GraphQL servers, allowing developers to easily map REST API endpoints to their GraphQL schema using a declarative syntax. This eliminates dependencies, boosts developer productivity, and helps teams unlock the full potential of GraphQL without overhauling existing systems.


Figure 1: Apollo Connectors for REST APIs


Our goal is to make Apollo Connectors effective for organizations of all sizes. For smaller teams it removes the complexity of setting up and maintaining GraphQL infrastructure, allowing developers to focus on building business logic rather than managing proxy resolver services. At the same time, Apollo Connectors scales for larger enterprises, enabling teams supporting existing REST APIs to provide GraphQL interfaces quickly and easily, ultimately accelerating the pace of innovation across the organization. Apollo GraphOS provides standardized tooling for using Apollo Connectors, providing easy onboarding for engineering leaders. Its context-aware smart editor and Visual Studio Code extension provides auto-completion, linting, and in-line validations, further enhancing developer productivity.


Get started with Apollo Connectors for[free](https://studio.apollographql.com/signup?referrer=www-new&_gl=1*cfb2hb*_ga*MTE2NDkyMzIxLjE3MjI5Njk2MDY.*_ga_0BGG5V2W2K*MTcyNzM4OTAwNy4xNjYuMS4xNzI3Mzg5MDA5LjAuMC4w%E2%80%9D%20with%20%E2%80%9Chttps://studio.apollographql.com/signup?referrer=blog) today and check out our[announcement blog](https://www.apollographql.com/blog/apollo-connectors-for-rest-apis) to learn more.


## Custom schema checks


Apollo GraphOS now supports custom schema checks, a powerful new feature that addresses API platform teams’ demand for enhanced schema validation capabilities. This functionality allows you to integrate your own business logic and governance rules into Apollo’s existing schema check workflow, ensuring that every schema change adheres to company-specific standards. The result is a more robust, flexible, and tailored approach to maintaining schema integrity, allowing platform teams to uphold organizational standards in an automated manner.


Refer to our[documentation](https://www.apollographql.com/docs/graphos/platform/schema-management/checks/custom) to learn more about custom schema checks.


## Local workflow enhancements


We’re always looking for ways to improve your day-to-day development workflows, and that’s why we’ve made some exciting updates to[Rover](https://www.apollographql.com/docs/rover/) , Apollo’s command-line interface for GraphOS. The new improvements in Rover integrates it more closely with[GraphOS Studio](https://www.apollographql.com/graphos/studio) and brings under-the-hood improvements that make it up to 10x faster when composing large graphs. By shifting to parallel and asynchronous execution of commands like compose and fetch, Rover now delivers significant improvements in both speed and reliability.


Beyond performance improvements, Rover now supports[subgraph mirroring](https://www.apollographql.com/blog/accelerating-graph-development-with-subgraph-mirroring-in-the-rover-cli) , an automated workflow that allows developers to fetch and test subgraph configurations on their local machines. This new enhancement eliminates the time-consuming manual setup previously required, enabling developers to quickly test new subgraphs and router configurations in a controlled environment, significantly boosting developer productivity.


And of course, Apollo Connectors are supported in Rover. Download the[latest Rover release](https://www.apollographql.com/docs/rover/getting-started/) today to get started.


## Apollo’s latest router updates enhances performance and stability


We’ve also made significant performance improvements to our supergraph runtime,[GraphOS Router](https://www.apollographql.com/graphos-router) . These enhancements are driven by features such as new native query planner, entity caching with invalidation API, and demand control.


### Native query planner for faster Router execution


A query plan is a strategy for executing an incoming GraphQL operation efficiently. It serves as a blueprint for dividing a single incoming operation into one or more operations that are each resolvable by a single subgraph. As your schema grows larger and more distributed, query planning becomes increasingly complex. For larger, more complex schemas, creating an optimal query plan is crucial to ensure that operations are executed quickly and with minimal resource consumption.


Figure 2: Query plan


We’re excited to announce a new native query planner in Apollo Router, designed to significantly enhance performance and reduce resource usage. Implemented in Rust, the native query planner processes and plans incoming client requests faster and more efficiently than ever before. Compared to the previous version, the new native query planner delivers a **10x** median performance improvement, with a notable **7x** improvement in p99 performance in query planning time. Refer to our[documentation](https://www.apollographql.com/docs/graphos/routing/query-planning/native-query-planner) to get started and explore how the native query planner can transform your query handling.


### Entity caching with invalidation API


You can also significantly improve application performance and reduce infrastructure cost across your GraphQL APIs with[entity caching](https://www.apollographql.com/docs/graphos/routing/performance/caching/entity) , which now includes comprehensive support for invalidating entries based on time to live, subgraph, type, and more.


Entity caching solves a lot of the[complexities that GraphQL](https://www.apollographql.com/blog/graphql-caching-the-elephant-in-the-room) brings to caching. Configuring response caching in a supergraph can be challenging due to the varied refresh rates of different fields. Consider a retail company as an example: their Product entity might have semi-static data like the catalog field, which can have a long time-to-live (TTL), alongside frequently changing data like the inventory field, which requires a short TTL.


This is where entity caching shines. It provides you granular caching control over different parts of the response. You can set longer TTLs for semi-static fields and shorter TTLs for frequently changing data. This fine-grained approach optimizes performance by reducing unnecessary subgraph queries, while still ensuring data accuracy.


### Demand control


As Apollo customers continue to scale their graphs to support more clients and more complex use cases, they need to ensure that these complex GraphQL operations don’t slow performance or create denial of service. To address this, we’re introducing demand control, now generally available, providing a way to secure your supergraph from overly complex operations.


To protect your supergraph infrastructure from overly complex or expensive operations, with demand control enabled, the GraphOS Router calculates the cost of each incoming request. If the operation exceeds your set cost limit, it’s automatically rejected, preventing system overload.


Apollo includes a default cost calculation algorithm, based on[IBM’s GraphQL Cost Directive specification](https://ibm.github.io/graphql-specs/cost-spec.html) , but users can also customize how the router calculates query costs using the @cost directive. This allows you to assign larger or smaller weights to operations based on their resource intensity, optimizing performance and resource management.


Figure 3: Customizing cost


To learn more about demand control, check out our[documentation](https://www.apollographql.com/docs/router/executing-operations/demand-control/) and[watch the demo](https://www.youtube.com/watch?v=ms8yCYFuVUo) .


## Clearer policy for Router versioning and support


Alongside all the improvements above, we’ve also made a change to give you more clarity on how we[manage and support router releases](https://www.apollographql.com/docs/graphos/reference/router-release-lifecycle) , including a long term support commitment of thirty months for new GraphOS Router releases from the time of initial release. We know that GraphOS Router is mission-critical infrastructure for your API workloads, and our goal is to give you, our customers, the predictability and support framework you need to confidently operate the router.


## Get started with Apollo GraphOS


Get hands-on with all of these features and more by starting your Apollo GraphOS[free trial](https://studio.apollographql.com/signup?referrer=www-new&_gl=1*cfb2hb*_ga*MTE2NDkyMzIxLjE3MjI5Njk2MDY.*_ga_0BGG5V2W2K*MTcyNzM4OTAwNy4xNjYuMS4xNzI3Mzg5MDA5LjAuMC4w%E2%80%9D%20with%20%E2%80%9Chttps://studio.apollographql.com/signup?referrer=blog) today and check out our[documentation](https://www.apollographql.com/docs/intro/platform) .


Want to learn more? It’s not too late to register for the[GraphQL Summit](http://www.graphqlsummit.com/virtual) where we’ll discuss all things Apollo Connectors, entity caching, native query planner and more.


Further reading:


- [Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors)
- [Press Release](https://www.apollographql.com/newsroom/press-releases/apollo-graphql-unlocks-the-value-of-enterprise-apis-with-new-innovations)


Written by


Rob Brazier


[Read more by Rob Brazier](https://www.apollographql.com/blog/author/rbrazier)
