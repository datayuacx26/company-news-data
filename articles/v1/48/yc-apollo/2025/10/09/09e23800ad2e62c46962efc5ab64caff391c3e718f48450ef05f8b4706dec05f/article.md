---
schema_version: "1.0.0"
document_id: "09e23800ad2e62c46962efc5ab64caff391c3e718f48450ef05f8b4706dec05f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-summit-2025-apollo-product-announcements"
published_at: "2025-10-07T07:00:24+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:3a7bdd0d615428b9e2a1597bfae3bb0dfa991ac2ce07cf108a738890f29ce453"
---

# GraphQL Summit 2025 Product Highlights: Building the future of AI and Apps

Software delivery isn’t slowing down, and neither is Apollo. At[GraphQL Summit 2025](https://summit.graphql.com/) , we’re showcasing a platform that’s now even easier to use, enables you to connect your APIs and development workflows to agents, and accelerates your graph development.


AI is exerting immense pressure on both businesses and platform teams. Organizations have the opportunity to build better experiences and to get them to customers faster, but platforms need to be ready to support them. With today’s announcements, Apollo GraphQL is powering the oncoming agentic world. Customer-facing agents can access APIs to provide new experiences, GraphOS integrates better into your deployment workflows, and it’s faster than ever to add new APIs to your platform.


Here’s what’s new.


## API Orchestration for the AI Era


Agents are radically transforming every application, every interface, and every task we perform online. Search boxes will become prompt boxes, content retrieval will be replaced by content generation, and endless hunting for the right thing will be supplanted by contextual, hyper-personalized results. Apollo will power the future of applications: with Apollo MCP Server, we bring the easy API access of the graph to agentic application experiences.


### Apollo MCP Server 1.0


MCP has quickly become the standard for giving LLM agents access to data and tools. Apollo MCP Server, now generally available, lets you connect LLMs to any GraphQL API in minutes. With the Apollo MCP Server, your GraphQL operations automatically become MCP tools, with no new wrapper code or manual tool definitions. Apollo MCP Server also includes optional built-in tools that enable agents to introspect and search your GraphQL schema to dynamically find the right information. MCP Server is also designed with security in mind: You can apply guardrails on what operations agents are allowed to perform by limiting MCP operations to a set of persisted queries, or give different agents access to different subsets of your schema with Apollo Contracts.


To run Apollo MCP Server confidently in production, this release includes OpenTelemetry integration for observability of MCP behavior, performance, reliability, and distributed request flows.[Apollo MCP Server also implements OAuth 2.1](https://www.apollographql.com/blog/introducing-authorization-for-apollo-mcp-server) , following the MCP Authorization Spec. Apollo MCP Server 1.0 is stable and production-ready.


For more information, read the Apollo MCP Server GA release[blog post](https://www.apollographql.com/blog/apollo-mcp-server-1-0-is-generally-available/) .


## Solid foundations for engineers… and agents


AI-powered development is fundamentally changing both the speed and expectations of software delivery. Both human engineers and agents need robust, self-service tools and platforms to maximize productivity. Apollo’s GraphOS platform already[delivers these foundations](https://www.apollographql.com/blog/the-future-of-mcp-is-graphql) and today, we’re taking it further with three new releases.


### Graph Runtime APM Templates


We’re always looking for ways to improve your experience and help you unlock insights faster for GraphOS Router. That’s why we’re introducing pre-built Application Performance Management (APM) templates, starting with support for Datadog, now available in public preview. These templates are designed to make it easier than ever to set up GraphOS Router monitoring in your APM tool of choice. The dashboard provides a clear, customized view of your router’s key performance metrics including incoming and outgoing HTTP requests, container and host metrics (CPU, memory, and pod counts), cache metrics, query planning, coprocessor usage, and additional diagnostic sentinel values.


To make setup effortless, we’ve included[step-by-step integration guides](https://www.apollographql.com/docs/graphos/routing/observability/router-telemetry-otel/apm-guides/datadog/observing-and-monitoring/dashboard-template) , so your teams can configure and tailor these dashboards in minutes. With these templates, you get immediate, actionable insight into your router’s health and performance – no custom engineering required.


Check out our[documentation](https://www.apollographql.com/docs/graphos/routing/observability/router-telemetry-otel/apm-guides/datadog) and[announcement blog](https://www.apollographql.com/blog/graphos-router-apm-dashboard-templates-for-datadog/) to learn more about APM templates.


Figure: Datadog dashboard with GraphOS Router metrics


### Apollo GraphOS Operator for Kubernetes is now GA


We introduced the[Apollo GraphOS Operator](https://www.apollographql.com/blog/announcing-the-apollo-graphos-operator-in-public-preview) in preview in August 2025, and we’re thrilled to announce it’s now generally available. Scaling and operating GraphQL infrastructure can be complex – provisioning runtimes, coordinating schema rollouts, managing progressive deployments, and ensuring safe rollbacks all take time and effort. The[Apollo GraphOS Operator](https://www.apollographql.com/graphos/apollo-graphos-operator) handles this complexity, helping your teams spend less time on managing infrastructure and more time delivering seamless user experiences.


This Kubernetes-native operator lets you declaratively deploy and manage GraphQL environments at scale. Using Kubernetes Custom Resource Definitions (CRDs), the operator powers declarative, GitOps-friendly workflows to provision GraphOS Runtime fleets, automate schema rollouts, and enable safe rollbacks. You can manage your environments through Kubernetes manifests or the GraphOS Studio interface – whatever fits your team’s workflow best. Designed for organizations of all sizes, the operator provides opinionated guardrails for autoscaling, real-time monitoring via Kubernetes events and statuses, and consistent runtime configuration across cloud native architectures.


[Install](https://www.apollographql.com/docs/apollo-operator/get-started/install-operator) the operator today and explore our[technical blog post](https://www.apollographql.com/blog/getting-started-with-apollo-graphos-operator) for a step-by-step guide to getting started.


### Graph Artifacts


Apollo GraphOS now supports graph artifacts, immutable, versioned packages of your supergraph schema, now available in public preview. Graph artifacts introduce a new layer of control and visibility, making it easier for you to manage which versions of your supergraph are running in different environments.


Until now, your GraphOS Routers were often set to auto-update to point to the latest schema. While this keeps deployments current, it can make it harder to lock to a known-good schema version, roll back safely, or audit exactly what was running in production. These capabilities are crucial, especially if you are operating in regulated industries or managing multiple environments.


With graph artifacts, every schema publish automatically recomposes and packages your supergraph as a versioned artifact, each tagged with a unique SHA-256 digest. You can deploy your GraphOS Routers with a specific artifact instead of always running the latest version, letting you achieve stable, repeatable deployments, fast rollbacks, and clear audit trails.


Refer to our[documentation](https://www.apollographql.com/docs/graphos/platform/schema-management/delivery/graph-artifacts) and[technical blog post](https://www.apollographql.com/blog/introducing-graph-artifacts/) to learn more and start using graph artifacts today.


Figure: Launches page with graph artifacts


## Accelerate your Graph Development


Last year at[Summit](https://www.apollographql.com/events/series/graphql-summit-2024) , we set a goal to cut the time it takes to unlock value from your graph. That mission is even more critical today as AI agents and the experiences they power demand fast, reliable access to data.[Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors) was our first big leap, enabling teams to connect REST APIs directly to a graph in minutes instead of days. This year, we’re pushing that momentum forward with new releases and enhancements designed to make building and scaling your graph faster than ever.


### Apollo Connectors Enhancements


Earlier this year, we announced the general availability of[Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors) to help you bring your existing REST APIs to the graph. To improve Connectors maintainability, we’re announcing a **Connectors Testing Framework** , available today in public preview. With the testing framework, you can easily build test cases for your connectors and run them from Rover or your CI tool.


We’re also making it easier to develop Connectors with new “type to run” functionality integrated into Rover and IDEs. Previously, the process of creating a new Connector required switching back and forth between IDE, Rover, and Sandbox as you iterated through development, testing, and debugging. Now, you can stay in a single environment, test running an in-progress Connector in the same IDE where you’re authoring it.


We want Connectors to handle the richness of all of your existing REST APIs. To that end, we’re expanding Connectors with enhancements to the mapping language, making it possible to bring more API functionality into your graph. Available today in public preview, new language features include more arrow functions in the Connectors selection syntax: array methods, logical operators, math methods, and type conversion. The mapping language now also supports nullable entity reference to handle optional fields.


Get started building and testing new Connectors today with the[documentation](https://www.apollographql.com/docs/graphos/connectors) .


### Subgraph and Connectors Insights


Today, we’re also excited to introduce subgraph and connector-level insights in[GraphOS Studio](https://www.apollographql.com/graphos/studio) , a new way to give your backend teams a fine-grained view into their service’s health and performance. With these insights, you can spot issues and proactively optimize your graphs with visibility into which clients and operations are driving the most traffic, as well as their services’ error patterns, latency trends, and request volume. You can measure your subgraph’s performance against the rest of the graph ecosystem by sorting subgraphs based on request rate, latency and error rates. Subgraph owner information is now visible, so you can quickly reach the right team when things go wrong, helping you resolve issues faster. With this new level of observability, you can now optimize, troubleshoot, and iterate on your services with confidence.


Subgraph insights is available as an opt-in feature for any federated graph running Apollo Router 2.7.0 or later. Read the[documentation](https://apollographql.com/docs/graphos/platform/insights/subgraphs) and explore the[deep-dive blog](https://www.apollographql.com/blog/subgraph-and-connector-insights) to get started.


Figure: Top Traffic Sources widgets


### Subgraph API Keys


We’re raising the bar on deployment security with the launch of subgraph API keys. Instead of granting broad, supergraph-wide permissions, these new keys follow the principle of least privilege. Purpose-built for deployment workflows, they let you scope deployment permissions down to just the subgraphs your team owns within the variants they should be operating. You can run checks against your supergraph and publish subgraph schema updates – the minimum permissions needed to change your subgraph schema. They also come with clear expiration dates and 256 bit encryption to align with security best practices and further prevent against brute force attacks.


Subgraph API keys are available today on[GraphOS Standard and Enterprise Plans](https://www.apollographql.com/pricing) , giving your teams the tools they need to strengthen security while empowering them to innovate faster.


Read the[documentation](https://apollographql.com/docs/graphos/platform/access-management/api-keys#subgraph-api-keys) and[technical blog post](https://www.apollographql.com/blog/subgraph-api-keys-and-safer-deployment-pipelines) to start using subgraph API keys today.


### GraphOS MCP Tools


To bring development with Apollo into the agentic era, we’re making available GraphOS MCP Tools in preview today. Increasingly, engineers expect to be able to rely on coding assistants and use natural language conversation to assist in development tasks. With this release, we’re making available a hosted MCP Server for interacting with GraphOS. Connect your preferred coding assistant to start it working with GraphOS.


Today, there are two categories of MCP tools available for GraphOS. The` apollo_connectors_spec` empowers the LLM with the specifications and instructions to build Apollo Connectors. The` apollo_docs_search` and` apollo_docs_read` searches for and adds to the agent context the relevant Apollo documentation. You can expect that we’ll add additional MCP tools to make it easier for agents to work with GraphOS.


To start working with the GraphOS MCP Tools today, visit the[documentation](https://www.apollographql.com/docs/graphos/platform/graphos-mcp-tools) .


### New Clients


Last month, we released[Apollo Client 4.0](https://www.apollographql.com/blog/announcing-apollo-client-4-0) . This release moves to an opt-in architecture for features that not everyone needs, which translates to a reduced bundle size. Most users will see a **20-30%** reduction, depending on what features they use. 4.0 also brings improved TypeScript support, better error handling, and more. To learn about Apollo Client 4.0 and how to migrate, read the[blog post](https://www.apollographql.com/blog/announcing-apollo-client-4-0) .


We’re also pleased to announce Apollo iOS 2.0, available now. 2.0 features modernized APIs that take full advantage of the Swift 6 concurrency model with async/await fetching and thread-safe response models. Read[Apollo iOS 2.0 blog post](https://www.apollographql.com/blog/announcing-apollo-ios-2-0) to learn more about what’s new and understand the migration path.


## Get Started with Apollo GraphOS


Apollo GraphOS continues evolving as the industry’s leading API orchestration platform for AI agents, web, and mobile apps. Whether your priorities are building agentic AI experiences, enhancing digital experiences, modernizing legacy systems, or accelerating application delivery, Apollo helps your team innovate faster.


To explore these new capabilities yourself, check out our[pricing plans](https://www.apollographql.com/pricing) to find the right fit for your team and follow our step-by-step[documentation](https://www.apollographql.com/docs) .


Want the full story? Watch the[GraphQL Summit 2025 Keynote](https://youtube.com/live/MoPYTN4piQc?feature=share) from Matt DeBergalis, CEO and cofounder at Apollo GraphQL.


Written by


Rob Brazier


[Read more by Rob Brazier](https://www.apollographql.com/blog/author/rbrazier)
