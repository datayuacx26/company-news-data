---
schema_version: "1.0.0"
document_id: "3448be64808a1fc7f4f8f302debeb82c17d233266a6fc9d98cf4fc82ce83cbea"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/subgraph-and-connector-insights"
published_at: "2025-10-07T07:00:20+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:f3b09e60db6a3009005f50cc73c687430caba34366d99da0b960089e6759706a"
---

# Subgraph and Connector Insights: Empowering Developers Through Endpoint Observability

Today, we’re introducing **Subgraph and Connector-level insights** in[GraphOS Studio](https://www.apollographql.com/graphos/studio) . This feature enables teams to quickly understand their services’ behavior in the context of a larger federated graph. Such insights help teams identify issues and proactively improve performance.


## **The Problem: Flying Blind in Your Own Graph**


In a federated GraphQL architecture, your subgraph or[connector](https://www.apollographql.com/graphos/apollo-connectors) doesn’t exist in isolation. It’s part of a larger system where client queries span multiple services, and performance bottlenecks can cascade across the entire graph. Until now, engineers responsible for specific subgraphs or connectors could not see how their services were operating in GraphOS. They could only look at aggregate performance of the larger supergraph, or detailed performance of specific objects and fields. Those numbers don’t tell you:


- Which clients are hitting your subgraph the hardest
- What operations are causing your service to slow down
- How your performance compares to other subgraphs in the system
- Whether that recent deployment had a positive impact


This lack of visibility made it difficult to proactively optimize performance or quickly triage incidents when they occurred.


## **What We Built: Your Subgraph Under the Microscope**


Subgraph-level insights provide backend developers with unprecedented visibility into their specific APIs’ performance. Think of it as a dedicated dashboard for your corner of the federated graph. Thanks to subgraph insights, you can now:


### **Find your biggest impact opportunities**


The Top Traffic Sources widgets show you exactly which clients or operations are generating the highest error rates and request volumes. No more guessing about where to focus your optimization efforts.


### **Discover your most common errors**


The subgraphs Errors dashboard highlights error distribution patterns as well as providing details around which operations failed. No matter the granularity, you can always see where your service is struggling.


### Spot patterns before they become problems


The Latency Heatmap, Error Rate, and Request Rate charts reveal trends over different time horizons. Whether it’s a daily traffic pattern or a weekly spike in errors, you’ll be able to catch it using your subgraph insights page.


### Sort graphs to see how you compare


Stack rank your subgraph’s request rate, latency, and error rates against other services using the “Sort subgraphs by” function. This context helps you understand when performance issues are localized to your subgraph or part of a broader system trend.


### Say something when you see something


The subgraph owner info (which is derived from any @contact directive tags in your schema), gives you the information you need to reach out to the appropriate team if something is wrong.


## How It Works: Data Collection for Service Owners


Apollo Router resolves a federated query by retrieving data from multiple services defined in the operation’s query plan. Each request the Router makes against a downstream service is called a fetch. Starting in Router 2.7.0, the Router measures the latency of every fetch made to a subgraph or connector. Apollo captures this data and uses it to present subgraph insights in our Studio UI.


Router will still report subgraph fetch latency data through the[standard OTel instrument](https://www.apollographql.com/docs/graphos/routing/observability/telemetry/instrumentation/instruments#opentelemetry-standard-instruments) , http.client.request.duration. Customers can export their subgraph fetch data to their teams’ APM tooling, as well as enrich the information with customized attributes.


## **Getting Started: Enable Insights in Minutes**


Subgraph insights is currently offered as a generally available, opt-in feature. If you’re already running a federated graph with Apollo Router 2.7.0+, enabling subgraph insights takes just a few lines of configuration:


yaml


```text
telemetry:
apollo:
preview_subgraph_metrics: true # default: false
```


**Note:** In Apollo Router 2.8.0+ the configuration will be renamed to telemetry.apollo.subgraph_metrics.


You’re now ready to see your subgraph insights! Simply return to your GraphOS Studio insights page, filter by subgraphs, and start exploring your performance data.


## **No Surprises: Pricing and Availability**


Subgraph-level insights is part of our continued commitment to empower developers with the tools they need to build and maintain high-performance GraphQL APIs. It is available to all GraphOS Studio customers running Router 2.7.0+, regardless of[plan](https://www.apollographql.com/pricing) . There are no additional costs, and metrics respect your existing data retention policies.


## **Looking Ahead: What’s Next**


Subgraph-level insights represent more than just new metrics. It’s about democratizing observability in federated architectures. When every backend developer can see how their service fits into the larger system, the entire graph gets better.


Visibility can only go so far though if it isn’t easily accessible. To ensure every developer can benefit from our subgraph insights we’ll be working to turn subgraph insights on by default in future Router versions. We’ll also provide subgraph insights data in the platform API for further extensibility.


Ready to get unprecedented visibility into your subgraph performance? Enable insights in your Router configuration and start exploring your data in GraphOS Studio today.


Want to learn more? Check out our[documentation](https://apollographql.com/docs/graphos/platform/insights/subgraphs) for detailed setup instructions and best practices.[Reach out and share your reactions](https://community.apollographql.com/t/subgraph-and-connector-insights/9484) as you start working with subgraph insights. Your feedback will shape how these capabilities evolve.


*Note: Ross Regitsky also contributed to this blog post.*


Written by


Leah Hurwich Adler


[Read more by Leah Hurwich Adler](https://www.apollographql.com/blog/author/ladler)
