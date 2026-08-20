---
schema_version: "1.0.0"
document_id: "ff5f129c5b4cac23425a77c1336d0745ff5511f433899e9cf180b52c6ff0ce63"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/why-duplicating-environments-for-microservices-backfires/"
published_at: "2024-11-15T21:55:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:aa89648061b2e9e5c5abda9cbf44e30528dda129946216a21dcad568857e67ad"
---

# Why Duplicating Environments for Microservices Backfires

**Typical ways of testing microservices are too slow and unsustainable as engineering teams grow and architectures grow more complex.**


In microservices development, how long it takes to test your code changes in a production-like environment is critical. Long microservices testing cycles can significantly hamper developer productivity and slow down the entire release cadence.


With developers needing to run these tests multiple times a day, even small delays can compound into major bottlenecks. As engineering teams and architectures grow, finding an efficient, scalable solution for[testing microservices](https://www.signadot.com/blog/microservices-testing-feature-flags-vs-preview-environments/) becomes paramount.


## Challenges of On-Demand Environments


Many teams turn to on-demand environments as a solution, spinning up separate instances for each developer or team. Various implementations of this method spin up an environment within virtual machines (VMs), Kubernetes namespaces or even separate Kubernetes clusters.


*On-demand environments using namespace isolation in Kubernetes.*


While this approach seems logical at first, it often leads to challenges, such as the following.


### High Management Burden


As system complexity increases, each environment requires a multitude of components: stateless services, load balancers, API gateways, databases, message queues and various cloud resources. Managing and updating these components across multiple environments becomes increasingly difficult.


### Divergence From Production


To manage costs and complexity, teams often resort to using mocks and emulators for certain components. This leads to a divergence from the production environment, potentially reducing the reliability of tests.


### Data Management Complexities


Maintaining and synchronizing data across multiple databases in numerous ephemeral environments is a significant challenge. This is especially problematic when dealing with large data sets or complex data relationships.


### Environment Staleness


As the main branch of each microservice is continuously updated, ephemeral environments can quickly become outdated. This leads to tests being run against old versions of dependent services, reducing their effectiveness.


### Increased Startup Times


As the complexity of these environments grows, so does the time required to spin them up. This delay directly impacts developer productivity and can slow the entire software development process.


### Cost Implications


The financial impact of running multiple full environments is significant. Consider this example:


> *For a system with 50 microservices, you might need an*[AWS](https://aws.amazon.com/?utm_content=inline+mention) *EC2 m6a.8xlarge instance (32 vCPUs, 128 GiB memory) that costs approximately $1.30 per hour. Running this 24/7 for a month costs $936, or $11,232 per year for a single environment. To run 50 instances of this, the annual cost skyrockets to $561,600 — and that’s just for compute, not including storage, data transfer or managed services.*


## Shared Environments and Sandboxes Instead


Shared environments with application-layer isolation, called “sandboxes,” have emerged as a way to address these challenges. This concept, similar to what[Uber has implemented for end-to-end testing](https://uber.com/en-TW/blog/shifting-e2e-testing-left/) , offers a more efficient and scalable solution.


In this model, instead of spinning up separate environments for each developer or team, you use a shared environment. Within this shared space, you provide “tunable isolation” for every test client by sandboxing services and resources as needed. The services within sandboxes are accessed by dynamically routing requests based on request headers.


*Sandboxes within a shared environment.*


This approach offers several advantages:


1. **Resource efficiency:** By sharing the underlying infrastructure, you significantly reduce resource usage and associated costs.
2. **Consistency:** All tests run against the same baseline environment, eliminating “it works on my machine” issues and providing more reliable results.
3. **Reduced maintenance overhead:** With a single shared environment to maintain, it’s more manageable to keep everything up to date.
4. **Faster startup time:** Sandboxes can be created almost instantaneously, allowing developers to start testing without delay.
5. **Production-like testing:** The shared environment can more closely mimic the production environment, improving the reliability and relevance of test cases.


## Implementation Considerations


While the shared environment approach offers significant benefits, there are several key considerations for implementation.


### Context Propagation


To ensure proper isolation within the shared environment, it’s crucial to propagate context through the services. This can be achieved efficiently using[OpenTelemetry](https://opentelemetry.io/) instrumentation. Its` baggage` and` tracecontext` standards are particularly useful for maintaining context across service boundaries.


### Data Isolation


Careful attention must be paid to data partitioning, especially for data being edited or deleted. A fundamental rule is that a test should not be able to mutate data it doesn’t create. This ensures that concurrent tests don’t interfere with each other’s data, maintaining the integrity of each sandbox.


### Message Queue Handling


Special consideration is needed for message queues to ensure that sandboxes don’t compete for the same messages. This might involve implementing custom routing logic or using separate queues for each sandbox. Refer to[Testing Kafka-based Asynchronous Workflows Using OpenTelemetry](https://www.signadot.com/blog/testing-kafka-based-asynchronous-workflows-using-opentelemetry/) for details on how to implement isolation with asynchronous message queues.


## Traditional Microservices Testing Approaches Are Unsustainable


As microservices architectures continue to grow in complexity, the traditional approach of[duplicating entire environments](https://www.signadot.com/blog/why-environment-replication-doesnt-work-for-microservices-testing/) for testing becomes increasingly unsustainable. The shared environment model, with its use of sandboxes for isolation, offers a more efficient, cost-effective and scalable solution.


This approach has already proven successful in several high-profile cases. Signadot has helped companies like[Brex](https://www.signadot.com/case-studies/brex-uses-signadot-to-scale-developer-testing-across-100s-of-engineers/) ,[Earnest](https://www.signadot.com/case-studies/how-earnest-empowers-developers-for-early-testing/) and[DoorDash](https://www.signadot.com/case-studies/how-developers-at-doordash-get-10x-faster-feedback/) streamline their microservices testing processes and improve developer productivity. Their experiences demonstrate the real-world applicability and benefits of this new approach to microservices testing.


## Related Articles


- [Why Environment Replication Doesn’t Work for Microservices Testing](https://www.signadot.com/blog/why-environment-replication-doesnt-work-for-microservices-testing/) - In the complex world of microservices, traditional testing strategies often fall short. Discover why environment replication fails and how sandbox approaches provide a superior alternative.
- [How to Diagnose Flaky Tests](https://www.signadot.com/blog/how-to-diagnose-flaky-tests/) - Flaky tests are a pervasive issue in software development, particularly as teams scale up and systems become more complex. Learn systematic approaches to identify and resolve test instability.
- [Cut Testing Costs 90% with Kubernetes Ephemeral Environments](https://www.signadot.com/articles/cut-testing-costs-90-with-kubernetes-ephemeral-environments/) - Explore how Signadot’s Kubernetes Sandboxes revolutionize microservices testing, offering rapid, cost-effective, and accurate ephemeral environments that reduce testing bottlenecks and costs by up to 90%.
- [The Million-Dollar Problem of Slow Microservices Testing](https://www.signadot.com/blog/the-million-dollar-problem-of-slow-microservices-testing/) - Discover how organizations lose significant money to inefficient testing processes and learn about modern approaches that transform both developer productivity and infrastructure costs.
