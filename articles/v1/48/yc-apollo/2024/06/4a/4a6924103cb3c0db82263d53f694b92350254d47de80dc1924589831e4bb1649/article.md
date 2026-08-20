---
schema_version: "1.0.0"
document_id: "4a6924103cb3c0db82263d53f694b92350254d47de80dc1924589831e4bb1649"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-graphql-announces-major-graphos-update-enhancing-observability-and-performance-for-enterprise-scale-graphql-federation"
published_at: "2024-06-12T08:55:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:817f8b42c25264eb549175e65fc8b606ee60566f8585582d2efc6687677bb2b1"
---

# Apollo GraphQL Announces Major GraphOS Update, Enhancing Observability and Performance for Enterprise-Scale GraphQL Federation

*Caching, Native Cost Calculation and Limiting, and Telemetry Capabilities Revolutionize API Management and Streamline Building Cross-Platform Digital Application Experiences*


Today at PlatformCon,[Apollo GraphQL](https://apollographql.com/) , the creator of GraphQL federation, announced new features in[Apollo GraphOS](https://www.apollographql.com/graphos/) are now generally available to better leverage GraphQL’s developer productivity and performance benefits at any scale. This release provides API platform teams with visibility, usability, and performance benefits for GraphQL federation, enabling the engineering teams they serve to maximize the value of their APIs across their business.


As the leading GraphQL federation platform, GraphOS provides an abstraction layer for API complexity, enabling product engineering teams to rapidly build and iterate features based on customer demand.[Gartner® predicts that](https://www.apollographql.com/resources/gartner-when-to-use-graphql-to-accelerate-api-delivery) 30% of enterprises using GraphQL will utilize GraphQL federation by 2027, up from less than 5% in 2024.1 Apollo believes this trend suggests the number of enterprises using GraphQL federation as their API access layer will increase by 12x. GraphOS provides everything enterprise teams need to deliver GraphQL federation at any scale.


“GraphQL federation removes the friction between the frontend and backend that we see in distributed architectures,” said Matt DeBergalis, Chief Technology Officer and co-founder at Apollo GraphQL. “This latest GraphOS release enables API platform teams to operationalize GraphQL federation’s benefits faster, so they can focus less on building custom tooling and more on their customers’ needs.”


This latest GraphOS release includes significant improvements around observability, usability, and performance, enabling API platform teams to operate GraphQL federation more easily than ever before. “At enterprise-scale, every millisecond of latency in your consumer’s digital experiences impacts the bottom line,” said Rob Brazier, VP of Product at Apollo GraphQL. “With this release of Apollo GraphOS, we’ve made it even easier for API platform teams to observe and optimize the performance of federated graphs.”


**Apollo’s latest release provides operational excellence for GraphQL federation**


GraphQL federation is becoming a de facto architecture for platform engineering teams to simplify API governance across distributed architectures. Client teams can fetch all the data they need from a single endpoint, and this paradigm enables API platform teams to enforce better governance, provide better visibility into service usage, as well as use standardized tools for performance. GraphOS provides a complete solution for delivering GraphQL federation at any scale:


- **Better application performance** – Apollo introduces an improved and simplified form of API caching built for GraphQL federation. GraphOS’s caching layer spans any number of APIs and now, in public preview, supports entity caching. Entities are object types that are composed from multiple GraphQL APIs and are the business objects or “nouns of every business.” API platform teams can store entities such as their inventory, products, or user records in a Redis data store. API platform teams can set how long these entities are cached in the data store, enabling API platform teams to significantly improve performance across an entire fleet of applications. “We’ve rolled out entity caching in production and are observing a decrease in median response time,” said Nimit Barochia, Senior Principal Software Engineer at Dow Jones. “The major improvement is how much less traffic we send to our subgraphs. Like a 8-10x decrease in some cases.”


- **Cost control** – API platform teams need a means to ensure that costly GraphQL operations don’t slow performance or create a denial of service. Platform teams need a means to limit cost of GraphQL operations processed by the GraphOS Router. Apollo is adding in public preview native cost calculation and limiting to GraphOS to make it easy for platform teams to centrally implement this, rather than relying on underlying services.
- **Stronger support and improved performance** – Micro-frontends provide a frontend architecture that refines performance and composability across mobile and web applications. To improve performance, micro-frontend UIs typically batch multiple client operations, issued close together, into a single API request. GraphOS is adding better support for these use cases, enabling GraphOS to handle batched requests from clients to a supergraph. It also further augments performance by ensuring that batches are kept intact when processed and passed on to the required subgraphs.
- **Enhanced telemetry** – API platform teams need both standardized and custom metrics to better optimize federated graphs for performance and business objectives. GraphOS now provides unparalleled visibility to debug and optimize federated graphs, enabling these teams to import these metrics into their Application Performance Monitoring tool of choice for better decision-making.
- **Significant improvements to the GraphOS management plane** – In Apollo Studio, schema checks and insights enable API teams to build, test, and ship changes to their federated graph with confidence. This release features significant UX improvements and better insights, enabling API teams to ship changes faster without introducing breaking changes to downstream clients.


**Apollo customers to focus on innovation instead of API aggregation or orchestration**


AI is driving rapid innovation across industries, making the API layer critical for delivering competitive, personalized applications. Apollo aims to help API platform teams leverage GraphQL federation’s benefits across engineering organizations, empowering their teams to lead their organizations into the AI era.


**Learn more with Apollo**


- [Attend a webinar](https://www.apollographql.com/events/apollo-graphos-summer-update-operational-excellence-for-graphql-federation?utm_campaign=2024-06-12_Webinar_Apollo-GraphOS-Summer-Update-Operational-Excellence&utm_medium=email&utm_source=sales) on June 12 to learn more about what’s new in GraphOS and see new features demoed.
- Join Apollo[live on June 13](https://lu.ma/platformcon-watch-party-apollo) in NYC as it celebrates the largest conference for platform engineering, PlatformCon, and hear from customers COX Automotive, The New York Times, and Wayfair on how they’re finding success with Apollo’s federated GraphQL technology.
- And the largest gathering of GraphQL enthusiasts and API platform teams is coming. Join Apollo at[GraphQL Summit](https://summit.graphql.com/) , October 8-10, where engineering teams can learn from industry leaders spearheading successful GraphQL deployments at scale.


1Gartner, When to Use GraphQL to Accelerate API Delivery, By Shameen Pillai, et al, March 7, 2024. GARTNER is a registered trademark and service mark of Gartner, Inc. and/or its affiliates in the U.S. and internationally and is used herein with permission. All rights reserved.


## About Apollo GraphQL


[Apollo GraphQL](https://apollographql.com/) helps developers help the world by providing a better API platform across teams. Apollo’s open-source software is downloaded 25M times per month and its commercial GraphQL technologies power the most innovative brands today. Teams at Netflix, Snap, and the New York Times ship personalized, omnichannel experiences faster with a supergraph – a self-service GraphQL platform that spans any number of backend services. Serving over 5T requests in 2023, the Apollo GraphOSTM platform simplifies API development with workflows and infrastructure to build, test, and ship supergraphs at any scale. Based in San Francisco, Apollo is backed by Insight Partners, Andreessen Horowitz, Matrix Partners, and Trinity Ventures. Learn more at:[https://www.apollographql.com](https://www.apollographql.com/) .


Written by


Jennifer Tyrseck


[Read more by Jennifer Tyrseck](https://www.apollographql.com/blog/author/jennifer)
