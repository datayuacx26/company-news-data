---
schema_version: "1.0.0"
document_id: "924536a886b30dfaca0888bfcfdecea0800f5896893ba9e2fe9410a4f426d250"
company_key: "yc-openphone"
company: "Quo (fka OpenPhone)"
source_id: "yc-openphone-rss-7c9664a9f202"
canonical_url: "https://www.quo.com/blog/scaling-graphql/"
published_at: "2026-07-21T17:45:17+00:00"
first_seen_at: "2026-07-21T18:39:06.493014+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:a08100cd7312232c85c880348c80e6ed8c96199614fcdb93af252bd0e5f8d6e3"
---

# Lessons from scaling with GraphQL

Over the years, Quo has grown to support thousands of small and medium-sized businesses. As we’ve scaled, architectural decisions that once made sense have become increasingly difficult to sustain. Our migration to GraphQL is still underway, but the benefits have already become clear. This article focuses on the scaling challenges we encountered with our existing architecture, and why we chose GraphQL as a solution.


##


**The challenge of an offline first approach**


Quo has always been offline first. Even without an active internet connection, users can still view their contacts, messages, and call history. To support this behavior, Quo performs a series of REST API calls on launch to synchronize the local database with any data that was missed while the device was offline. In practice, each device maintains a local cache that contains nearly all the data associated with that user.


While this architecture provides a fast and resilient user experience, its tradeoffs have become more apparent with our growth. It’s become more difficult to fit and synchronize all of a user’s conversation data on a single device. The synchronization process itself also places significant strain on both our backend infrastructure and our users’ devices.


##


**A failure to communicate**


These scaling challenges weren’t limited to infrastructure alone. As our engineering organization grew, we hit team collaboration challenges around our APIs.


Traditionally, engineers at Quo worked within isolated technology domains. Engineers worked in specific teams: backend, web, Android, and iOS.. Expertise across multiple domains was limited.


Teams would agree on the API contracts necessary to support a feature, but those contracts were rarely persisted or maintained in a centralized way. Over time, knowledge of the intended API behavior would fade and usage would gradually drift from the original implementation.


As a result, our application clients often began relying on backend behavior that was never intentionally designed as part of the public API surface. A backend engineer might add a field for internal use, only for another client team to discover it in a network response and begin depending on it to implement a feature or workaround. Other teams would remain entirely unaware of the field’s existence or intended purpose. It was like a combination of engineering telephone and unintentional scavenger hunts.


This isolated development process, combined with loosely defined API contracts, resulted in a fragile system. Mobile and web implementations diverged significantly, often relying on entirely different fields or endpoints to support similar functionality. Backend changes could easily introduce unintended downstream effects because there was no clear and centralized understanding of how clients consumed the underlying data model.


##


**A distributed monolith**


Initially, the backend was built as a monolith: a single codebase deployed as one application serving every feature Quo supported. As the company scaled, the monolith was split into a series of independently deployed microservices. This allowed infrastructure to scale according to the needs of each feature area.


However, the services themselves remained deeply interconnected. Through a thin abstraction layer, microservices were still able to make requests to each other for both reads and writes. Clients also needed to aggregate data from multiple microservices in order to render a single screen.


The result was a highly coupled system where failures in one area could easily cascade into widespread outages. While the microservice-based approach successfully isolated deployments, it didn’t isolate functionality itself. Over time, the architecture evolved into a distributed monolith: an interwoven application spread across multiple independently deployed services.


##


**Fixing the schema**


These architectural and organizational challenges ultimately stemmed from the same underlying issue: the structure of the backend systems didn’t align with the shape of the data clients actually needed.


One solution we considered was to remove aggregation from the backend entirely. Microservices would return the data they own, and the clients would be responsible for aggregating it together to render the UI. This approach would allow backend services to become more independent by pushing aggregation responsibility entirely to the clients. However, each client platform would need to implement the required data aggregation, leading to duplicated efforts and inconsistency.


Another solution we considered was to create a specific REST based Backend For Frontend, or BFF, aggregation layer on top of the existing microservices. This would allow the microservices to gradually decouple from each other, leaving the aggregation of multiple data sources to a single place. We decided not to go this route due to performance concerns of the aggregation layer. Existing microservices were already experiencing significant resource usage from aggregation and processing workloads.


No matter how you slice it, the data has to be aggregated somewhere. Whether it happens in a backend service or on each client platform, the result is still inefficient and difficult to scale. This is as much an organizational problem as it’s a technical one. Any solution we picked not only had to fix the existing problems we had but also provide a framework for defining APIs that are performant and scalable.


### **Why we chose GraphQL**


GraphQL allows clients to define the shape of the data they need while exposing a strongly typed, centralized schema shared across the organization. This changes the relationship between frontend and backend teams. Instead of API contracts existing implicitly through tribal knowledge and undocumented REST responses, the schema itself becomes the source of truth. Fields become discoverable, ownership becomes clearer, and changes become easier to reason about across teams.


This also eliminates both overfetching and underfetching, giving client teams exactly what they need to render UI efficiently. Instead of stitching together multiple REST responses and duplicating transformation logic on every platform, each client can request precisely the data it needs in a single query.


### **GraphQL Federation**


Federation takes this a step further. Rather than centralizing all schema definitions in one monolithic service, each backend microservice is responsible for its own slice of the overall graph. Teams can independently define and evolve the portion of the schema they own. A lightweight gateway service handles composing those individual schemas into a single unified graph. The result is a distributed, modular system that still provides a coherent API surface to the clients.


This allows teams to evolve their services independently without fragmenting the API surface exposed to clients. Ownership remains decentralized, while the graph itself remains unified.


##


**What’s next**


GraphQL hasn’t eliminated complexity from the system, but it wasn’t expected to either. Distributed systems remain difficult to reason about at scale, and aggregation still has to happen somewhere. GraphQL provides a clearer contract between teams and a more sustainable foundation for evolving both the backend architecture and the client experience over time.


Our migration is still underway, but the improvements in API discoverability, cross-team collaboration, and frontend development velocity have already validated the direction.


If building systems like this excites you, we’re always looking for talented engineers to join us at Quo. Check out our[Careers](https://www.quo.com/careers) page or reach out to connect!
