---
schema_version: "1.0.0"
document_id: "877d2d2057ac96c700498bed3838c6c49c7d917dd51e611f35e244815dd4d0f7"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/testing-event-driven-architectures-with-opentelemetry/"
published_at: "2024-08-09T15:45:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:b73debbfa68c00aa3c570203912b02aba34b4840349519cefce1f19155889876"
---

# Testing Event-Driven Architectures with OpenTelemetry

*Originally posted on*[The New Stack](https://thenewstack.io/testing-event-driven-architectures-with-opentelemetry/) *.*


In cloud native environments, asynchronous architectures are pivotal for decoupling services, enhancing scalability and bolstering system reliability. A message queue forms the basis of an asynchronous architecture, and you can choose one from myriad options ranging from open source tools like Kafka and RabbitMQ to managed systems like Google Cloud Pub/Sub and AWS SQS. However, testing queued asynchronous workflows presents unique challenges. This article explores pragmatic approaches to testing these workflows using[OpenTelemetry](https://thenewstack.io/opentelemetry-gaining-traction-from-companies-and-vendors/) , focusing on cost-effectiveness, resource optimization and operational simplicity.


## **Challenges in Testing Event-Driven Workflows with a Queue**


Adding a queue like[Kafka](https://thenewstack.io/decoding-kafka-why-its-worth-the-complexity/) to your environment involves complex setups with multiple brokers, producers and consumers. Trying to[replicate an environment](https://www.signadot.com/blog/environment-replication-doesnt-work-for-microservices/) with a queue for testing involves a large amount of operational lift, with considerations like security, replication, partitioning and keeping a replica updated. The complexity escalates when trying to consume messages with services across various languages and frameworks, making isolated end-to-end testing a demanding task.


Note that for these various models and in the example that follows, “tenant” has a specific meaning.


By “tenant,” we mean a developer or team that needs to run a test scenario in isolation. If two teams are working very closely together and collaborating on a release, they may be a single tenant. But generally, it will mean one team that wants to test some changes without having those changes affect anyone else.


## **Strategies for Testing Event-Driven Workflows**


When using a large and complex queue with many publishers and subscribers, two methods are the most common solution to creating a testing environment. With isolated infrastructure, the whole cluster is copied with all related services, publishers and subscribers for each tenant. With isolated topics, the queue is configured to use a special channel for test publishers and subscribers. Both of these approaches have their disadvantages, including maintenance and setup costs, and the final (sometimes dubious) accuracy of these new testing environments to production. For now, we’ll consider a solution that offers a highly scalable solution for multiple tenants, with an environment that very closely resembles production.


### **Isolated Messages Using a Shared Queue**


Rather than replicating components that shouldn’t be changed by a tenant, we can focus on the parts of the cluster we do want to isolate: the messages passing between services. With message isolation, we can share any resources we need to and even let our services under test communicate with the “baseline” version of other services.


This starts by establishing a baseline environment shared safely across tenants, using dynamic routing for requests and messages by adding context propagation via OpenTelemetry. This approach minimizes infrastructure setup, operational overhead and costs while ensuring up-to-date environments through continuous integration. There’s also no lift to including an additional testing tenant.


## **Implementing Message Isolation-Based Testing**


In this model, each tenant is assigned a unique ID, with mappings to specific service versions. For most services, any tenant will expect to interact with a baseline version, with only a select few services synched to a newer version. These modified services can communicate with each other or the other services in the cluster as normal.


Tenant IDs are used for routing in both synchronous (HTTP, gRPC) and asynchronous (queued) communications. That is, both messages to and from individual services and messages into and out of the queue will need specialized routing instructions. One of the methods to achieve this is by using a[service mesh.](https://www.signadot.com/docs/guides/request-routing)


There is support in any queuing system to add arbitrary headers to affect routing. In Apache Kafka, producers include tenant IDs in message headers, and consumers use these IDs for selective message processing. This setup requires modifying Kafka consumers and leveraging OpenTelemetry for context propagation. The process is quite similar when using RabbitMQ, which can also embed each message with a tenant ID.


### **Operational Architecture**


The implementation of message isolation for request isolation-based testing and experimentation has a few necessary components.


- **OpenTelemetry for context propagation:** Use OpenTelemetry for tenant ID propagation across services and your queue, ensuring coherent message routing. To instrument Kafka producers and consumers to propagate context through Kafka, you can refer to the concrete example provided in the OpenTelemetry documentation. This[example](https://opentelemetry.io/blog/2022/instrument-kafka-clients/) shows how you could propagate tenantID from publishers through Kafka to the consumers. There is also support for[context propagation for RabbitMQ](https://opentelemetry.io/docs/specs/semconv/messaging/rabbitmq/) , and similar support exists for public cloud queuing services on Google Cloud and[AWS](https://aws.amazon.com/?utm_content=inline+mention) .
- **Selective message consumption:** Implement logic in queue consumers for message filtering based on tenant IDs, with each consumer operating in its own group.
- **Service mesh or other routing systems:** For tenants to configure their cluster to only send test messages to their systems, while routing all other requests normally, configure a service mesh or its equivalent to route traffic based on request headers.


*In this example, a tenant can stand up new versions of services (B” and C”) and add them as producers and consumers without interfering with other teams’ testing process.*


A straightforward naming convention for these new consumer groups would be the service’s original consumer groups with “-\[sandbox name\]” appended.


### **Non-Request Scoped Flows**


Some consideration should be given when implementing this system for flows that don’t start with a single request. If, for example, a cron job is reading rows from a table, processing them and publishing each as a message to your queue, you would need to issue a tenant ID as each row is read, requiring you to design the system for your goals. Once there is clarity on how the baseline and “under-test” versions of consumers will partition the messages that originate from the database, the system needs to be designed accordingly.


## **Conclusion**


The[message isolation approach offers a scalable](https://www.signadot.com/blog/transforming-kubernetes-developer-environments-the-shift-to-request-level-isolation/) , cost-effective solution for testing Kafka-based asynchronous workflows. It reduces the need for extensive infrastructure while maintaining high isolation and flexibility. This method, extendable to other message queues, is a strategic choice for modern asynchronous applications.


A follow-up to this article will cover the specifics of implementing message isolation with an asynchronous workflow using Signadot.


For further insights and detailed implementation advice on testing with internal queues, join the discussion on Signadot’s community[Slack channel](https://signadotcommunity.slack.com/join/shared_invite/zt-1estxm8pv-qfiaNfiFFCaW~eUlXsVoEQ#/shared-invite/email) .
