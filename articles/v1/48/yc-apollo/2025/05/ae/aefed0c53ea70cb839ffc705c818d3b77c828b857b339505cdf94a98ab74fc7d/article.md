---
schema_version: "1.0.0"
document_id: "aefed0c53ea70cb839ffc705c818d3b77c828b857b339505cdf94a98ab74fc7d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-booking-com-orchestrated-their-service-architecture-with-apollo-federation"
published_at: "2025-05-07T13:45:42+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:9424f074b3bcdbb011445bf6aeb9491e18db137142061f808fda30c787ce8d04"
---

# How Booking.com Orchestrated Their Service Architecture with Apollo Federation

This post is part of our GraphQL Summit 2024 customer series, highlighting how leading teams are using Apollo to accelerate development and build world-class applications. Join hundreds of your peers this October as we return for[GraphQL Summit 2025.](https://www.apollographql.com/graphql-summit-2025)


---


[Booking.com](https://www.booking.com/) , a leader in the online travel industry, has significantly advanced its digital infrastructure by integrating[Apollo Federation](https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/federation) into its service architecture. At GraphQL Summit 2024, Sanver Tarmur, Senior Software Engineer at Booking.com discussed the company’s journey to improve user experience with GraphQL and how they transformed their approach to API orchestration – by moving to a distributed, federated model.


## Context and Challenges


Before Federation, Booking.com faced significant API orchestration challenges. With over 1,000 simultaneous experiments running across their platform and connections to multiple domains (accommodations, flights, attractions, and vehicle rentals), the company’s monolithic orchestration layer became increasingly complex.


“Our monograph setup was essentially a centralized API orchestration layer,” explained Sanver. “It was responsible for connecting clients with all our backend services, transforming data, handling errors, and coordinating the complex relationships between our various domains.”


This approach to API orchestration created several issues:


- Performance bottlenecks as all orchestration logic flowed through a single system
- Development bottlenecks as teams waited for changes to the central orchestration layer
- Difficulty maintaining consistent experiences across different client platforms
- Limited ability to optimize orchestration patterns for specific domains


## The Shift to Apollo Federation


Booking.com’s GraphQL journey began with a monograph setup, an initial schema stitching layer primarily focused on its highest-traffic business domain, accommodations. “We sell around one billion reservations per year, that is the most critical part of our business, we wanted to start with that part,” Sanver shared. The goal was to streamline client communication without embedding business logic into the monograph layer.


However, as adoption grew, so did the challenges. “Our initial setup became a bottleneck. Developer experience suffered, performance issues arose, and schema governance became difficult to maintain,” Sanver noted. At its peak, this setup supported 35 subgraphs, but the centralized team managing it became overwhelmed. Performance issues became particularly critical, with Sanver noting that a “20% latency increase causes a lot of booking loss for us”


## Apollo Federation Implementation


The migration to Apollo Federation enabled Booking.com to decouple subgraphs, improve performance, and enhance developer experience through improved independent delivery cadence. The monograph became the first subgraph in the federation gateway, gradually giving way to a more distributed architecture. The team was able to decentralize the ownership of GraphQL schemas which significantly enhanced developer experience and reduced schema governance challenges. As Sanver explained, “we helped people to migrate their schemas out of that single subgraph, interfaced by federation gateway, and we encouraged adaptation internally in the company.”


Later, during their cloud modernization, Booking.com undertook a strategic shift from their internal clusters to AWS. The team implemented edge routing for query optimization, enhanced traffic management using Envoy, and added resilience measures including schema caching and timeout management systems to overcome cross-cloud communication challenges.


To ensure reliable operations at scale, Booking.com implemented comprehensive monitoring and governance measures:


- Query-level latency monitoring to track and optimize query performance
- Timeout budgets dynamically adjusted based on query plans
- Schema caching to prevent issues during schema fetch failures
- Comprehensive alerting and monitoring dashboards to maintain reliability


## Measurable Results


### **Technical Scale**


“With Federation, we now support 140 service connections handling over 8 billion requests per day, with peaks of 100,000 requests per second,” Sanver shared. This represents one of the largest API orchestration implementations in the travel industry, coordinating data flows across accommodations, flights, attractions, and vehicle rentals to create seamless experiences for Booking.com’s 120 million active clients across iOS, Android, and Web.


### **Development Velocity**


The most significant business impact? Product delivery accelerated by 40%, allowing Booking.com to respond faster to market opportunities and customer needs. As Sanver shared, “developers don’t need to wait for release cycles anymore. They can ship quite faster compared to before.”


### **Internal Efficiency**


Beyond external benefits, Federation also solved internal challenges. Service-to-service communication, particularly for backend services that needed access to data, became significantly more efficient. “Instead of every service connecting directly to different databases, we used federation to streamline access, reduce latency, and improve performance,” Sanver explained.


### **Developer Experience**


Apollo Federation enabled developers to independently manage their services without the bottlenecks typically associated with monolithic architectures. This autonomy has made developers at Booking.com happier and more productive. The company has put in a lot of effort to educate its teams on GraphQL best practices, building internal tools to monitor schema quality, and create a supportive environment for ongoing learning and improvement.


## Future Outlook and Strategic Initiatives


Booking.com will continue to refine its use of API orchestration approach, with plans to migrate to[Apollo Router](https://www.apollographql.com/docs/graphos/routing) to further enhance performance and cost-efficiency. This next generation of orchestration technology promises significant performance improvements: complex orchestration use cases saw latency reduction from 200 milliseconds to 10 milliseconds, while property page interactions (which requires orchestrating hotel data from multiple services) saw overhead decrease from around 100 milliseconds to 5 milliseconds.


*“As we continue to evolve our API orchestration strategy,” Sanver noted, “we’re seeing not just technical benefits but business impacts as well. More efficient orchestration directly translates to faster page loads, better conversion rates, and ultimately, more bookings.”*


Booking.com’s experience with Apollo Federation shows how they eliminated API bottlenecks while future-proofing their platform. Through careful design, strong monitoring, and a focus on developer experience, they scaled to billions of daily requests with exceptional performance.


To learn more from Sanver Tarmur and Booking.com, watch his full session from GraphQL Summit 2024.


Written by


Maylee Jacob


[Read more by Maylee Jacob](https://www.apollographql.com/blog/author/mjacob)
