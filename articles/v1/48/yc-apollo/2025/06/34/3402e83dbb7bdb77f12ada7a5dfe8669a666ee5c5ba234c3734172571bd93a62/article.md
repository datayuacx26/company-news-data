---
schema_version: "1.0.0"
document_id: "3402e83dbb7bdb77f12ada7a5dfe8669a666ee5c5ba234c3734172571bd93a62"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/whats-new-in-graphos-apollo-summer-25-release-ai-ready-api-orchestration-with-enhanced-mcp-server-pricing-plans-and-performance-upgrades"
published_at: "2025-06-24T08:55:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:2cff610dd6862a27c45e8c2a8d1c5716e94957ef4dbe8f1737e0e1c09533bd58"
---

# What’s New in GraphOS: Apollo Summer ’25 Release – AI-Ready API Orchestration with Enhanced MCP Server, Pricing Plans, and Performance Upgrades

Today, we’re excited to announce a range of new improvements to the Apollo platform, all intended to help you build and operate powerful APIs for your team’s AI agents and applications. A quick summary of what we announced:


- **Apollo MCP Server** keeps getting better, now with **streamable HTTP support** , and an improved tool definition workflow to accelerate your agentic AI development
- **Apollo Connectors** got even more powerful, with **support for batch loading** and other enhancements
- We’ve **improved error reporting** from Apollo Router and added **error categorization** to Insights in Apollo Studio, helping you quickly get a better understanding of errors happening in your environment, and drill-down in specific issues
- We simplified deployment of both Apollo Router and Apollo MCP with a new all-in-one **Apollo Runtime container** distribution that includes everything you need to get Apollo’s self-hosted components up and running quickly in your cloud
- We introduced new **usage-based pricing plans** for Apollo GraphOS, making it easy to get started with Apollo for any GraphQL project: no up-front commitment required, and you only pay for what you use


## **Building APIs for Agentic AI: Apollo MCP Server Enhancements**


AI agents need to do more than just chat – they need to retrieve data, modify systems, and take real actions. This requires reliable[API orchestration](https://www.apollographql.com/why-apollo#api-orchestration) , the same challenge GraphQL solved for web and mobile applications. Model Context Protocol (MCP) standardizes how AI models like Claude and ChatGPT connect to your systems and APIs, and Apollo MCP Server gives you a simple, rapid solution for creating MCP tools from any new or existing GraphQL API.


Today we added a bunch of features that makes Apollo MCP Server even more powerful for building AI applications that need to interact with your existing APIs and data sources, or new GraphQL APIs you’re building to support agentic workflows:


- **Streamable HTTP Support** : In the context of an agentic application using MCP, streamable HTTP transport can offer several significant advantages over traditional HTTP request-response patterns: more efficient memory use, progressive loading of context, better support for long-running operations, faster feedback to users, and more
- **Enhanced Tool Definition** : Built-in Collections provide seamless workflows, allowing you to group related API operations and create more sophisticated AI interactions with minimal configuration.
- **Container Deployment Options** :[Production-ready deployment capabilities](https://www.apollographql.com/docs/apollo-mcp-server/guides#deploying-a-container) with built-in telemetry and advanced authentication, so you can confidently run MCP Server in your existing infrastructure.
- **Declarative Configuration** : Define your MCP tools declaratively rather than writing procedural code, ensuring consistent, maintainable AI integrations that scale with your organization.


On top of all this, Apollo MCP Server doesn’t just implement MCP: It’s integrated to Apollo’s GraphOS platform. Your MCP tools benefit from the same schema registry, insights, rate limiting, and governance that power your existing GraphQL API. When you define a query in Studio, it automatically becomes available as an MCP tool. When your AI agents call these tools, you see it all in your familiar GraphOS Insights dashboards.


If you’re already running GraphOS, deploying Apollo MCP makes your existing GraphQL and REST APIs AI-ready without modification, flowing through the same governance and security layer you already trust. You can learn more about building efficient AI agents using Apollo MCP Server in our[latest blog post](https://apollographql.com/blog/building-efficient-ai-agents-with-graphql-and-apollo-mcp-server/) .


## **Enhanced Apollo Connectors: Performance, Flexibility, and a growing community**


The N+1 problem has long posed a significant performance challenge when orchestrating REST APIs. When your application needs to fetch related data for multiple items, it traditionally requires making separate API calls for each item – turning what should be efficient queries into performance bottlenecks.


Now, Apollo Connectors include powerful batch loading capabilities that transform multiple individual API calls into a single, efficient request for REST APIs that support batched requests. Here’s how it works:


### **Before: N+1 Queries**


Let’s look at an example Connector:


```text
type Product
@connect(
http: { GET: "/products/{$this.id}" }
selection: "id name"
)
{
id: ID!
name: String!
}
```


Before batch loading, if I want to request details on multiple products, this pattern would result in one API call for each product – exactly the N+1 problem we want to avoid.


### **After: Batch Loading**


Now, we’ve added a new $batch variable to Connectors that allows you to pass a batch of parameters to your REST APIs that support batch requests. This enables you to write a Connector that takes advantage of your REST endpoint’s existing batch capability. Back to our example:


```text
type Product
"""
[{ "id": 1, "name": "Product 1" }, { "id": 2, "name": "Product 2" }]
"""
@connect(
http: {
GET: "/products"
queryParams: "ids: $batch.id->join(',')"
}
selection: "id name"
)
{
id: ID!
name: String!
}
```


Now, multiple products are fetched in a single request.


- The batch loading solution is extremely flexible, and supports a variety of API patterns such as comma-delimited parameters, repeated parameters, OData $filter syntax, and JSON body requests. You can learn more about this in our detailed blog on the new $batch feature[here](https://www.apollographql.com/blog/handling-the-n1-problem-declaratively-with-apollo-connectors/) .


### **Advanced URL Templating for Complex API Patterns**


Alongside batch loading, we’ve enhanced Connectors with more powerful URL templating capabilities. The new system provides:


**Explicit Component Definition** : Define separate components like path, queryParams, and body parameters for more maintainable configurations.


**Better Array and Object Handling** : Natural support for complex data structures in URL parameters.


**Enhanced Expressiveness** : More dynamic URL components without special syntax requirements.


For example, expressing /users?id=1&id=2&id=3 becomes:


```text
@connect(
http: {
GET: "/users"
queryParams: "id: $batch.id"
}
)
```


This approach is more explicit, safer, and provides better support for the complex URL patterns common in modern REST APIs.


### **20+ Ready-to-Use Connectors**


On top of all this, we’re excited to see the community around Connectors growing, and wanted to provide a starting point for your Connectors projects. We’ve built over 20 Connectors that you can grab off the shelf and start using immediately, including popular services like Stripe, Anthropic, JIRA, and AWS—the APIs your teams are already using every day.


Check out the[Apollo Connectors Community GitHub repo](https://github.com/apollographql/connectors-community) where you can clone existing connectors, build your own, and share them with the community.


### **Streamlined Error Diagnostics: Extended Error Categorization**


Debugging distributed GraphQL systems just got significantly easier. While REST developers have long relied on HTTP status codes for error classification, GraphQL has lacked this structured error taxonomy – until now.


Our new Extended Error Diagnostics brings standardized error categorization to GraphQL Federation, eliminating guesswork in root cause analysis. Traditional GraphQL error handling relied on parsing unique error messages, making it nearly impossible to group similar errors or identify trends.


Now, error rates can be measured using standardized extensions with error codes and service identifiers, providing instant visibility into error type and source service. When an error spike occurs, you can immediately see whether it’s isolated to a specific subgraph, affecting multiple services, or originating from client queries.


Extended Error Diagnostics is available now in Apollo Router 2.1.2+ as a preview feature, combining low-cardinality error codes with sampled traces for complete error context.


## **Get Started Instantly: Apollo Runtime Container**


Getting started with all these powerful features shouldn’t require weeks of setup. That’s why we’re introducing the[Apollo Runtime Container](https://github.com/apollographql/apollo-runtime?tab=readme-ov-file#apollo-graphos-runtime-container) – the easiest way to deploy the latest version of all the self-hosted Apollo components. This single container includes:


- Apollo Router
- Apollo MCP Server, ready to connect with AI models like Claude or OpenAI
- Dev mode with Apollo Sandbox for immediate experimentation
- Works everywhere – Docker, Kubernetes, any cloud environment


Whether you’re prototyping locally or deploying to production, the Apollo Runtime Container is now your fastest path to get Apollo up and running in your environment.


## **Improved GraphOS Pricing for Every Team: New Developer and Standard Plans**


And finally, we announced a big improvement to our pricing model in the form of our[new Developer and Standard plans](https://www.apollographql.com/pricing) . We’ve heard from many customers, particularly those at organizations early in their GraphQL journey, that their company needed a more flexible pricing model from Apollo – one that was a better fit for startups, single teams bringing GraphQL into their organization for the first time, and/or platform owners building out their very first Federated GraphQL platform. In these cases, API workload volumes are often speculative, making a usage-based pricing model a better fit. Further, depending on the workload, some of Apollo’s advanced features might not be needed. Both of these plans are intended to address these needs by offering a usage-based pricing model, eliminate usage caps, and give you more flexibility in which specific features you pay for.


A quick summary of these new plans follows:


Our Developer Plan offers simple, usage-based pricing with no commitment required, starting at $5 per 1 million GraphQL requests, with built-in volume discounts as your workload scales. This plan includes capacity for 10 GraphOS users, 7 days of data retention for Checks and Insights, and 8×5 support – all the basics you need to operate your first GraphQL workload in production. Further, signing up for the Developer Plan gives you $50 in usage credit, so you can begin ramping your API workload with no up-front cost whatsoever.


Our **Standard Plan** adds on more capabilities that teams need as they scale the size of the engineering organization contributing to the graph: Single Sign-On and IDP integration, basic user roles for managing developer access to the graph, and up to 30 GraphOS user accounts. This plan also adds schema proposals, to help growing teams collaborate on GraphQL schema changes as their graph grows. Usually at this stage, our customers have a stronger sense of their workload and a committed subscription makes sense for their organizations, so this plan is available with an annual commitment, which can be customized as you discuss your usage expectations with our sales team. Just like with the Developer plan, there’s no cap on usage, and volume discounts are available as your workload scales. If you think the Standard plan is right for your team, you can[contact our sales](https://www.apollographql.com/contact-sales) team today to learn more.


Both plans also provide flexible usage-based pricing for Apollo features like Entity Caching and Federated Subscriptions, metered independently of your total operations volume, allowing you to choose the features you need for the API workload in question, and only pay for what you use.


We’re always looking for ways to improve our pricing, and make it cost effective to power any and every API workload with Apollo. We look forward to introducing further improvements and new functionality to both of these plans in the future, so please continue sharing your feedback on how we can make these offerings even better.


## **Getting Started with the Summer ’25 Release**


**Missed our Apollo Summer ’25 Product Release webinar?** You can still catch all the exciting updates on-demand.


[Watch Now →](https://www.apollographql.com/events/scale-your-ai-stack-apollo-summer-25-release)


All the capabilities announced in our Summer ’25 release are available today:


**Try the Latest MCP Server Enhancements** : Get started with our[MCP Server documentation](https://www.apollographql.com/docs/apollo-mcp-server) and begin building AI-ready applications immediately.


**Explore New Plans** : Sign up for our[Developer](https://www.apollographql.com/pricing) plan, or[get in touch](https://www.apollographql.com/contact-sales?referrer=nav-contact-sales) with us to learn more about our Standard plan.


**Implement Batch Loading** : Upgrade to Apollo Router 2.3+ and Federation 2.11+ to take advantage of[Connectors batch loading capabilities](https://www.apollographql.com/docs/graphos/connectors/requests/batching) .


**Get Started with[Error Diagnostics](https://www.apollographql.com/blog/getting-started-with-error-diagnostics-in-graphos-studio/) in Studio**


We’re incredibly excited to see what you build with these new capabilities. The combination of enhanced MCP Server, accessible plans, and powerful batch loading creates new possibilities for both AI and traditional applications.


What will you create?[Share your projects with us](https://twitter.com/apollographql) and let us know how these new features are helping you build better applications faster.


Written by


Rob Brazier


[Read more by Rob Brazier](https://www.apollographql.com/blog/author/rbrazier)
