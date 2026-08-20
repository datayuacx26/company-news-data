---
schema_version: "1.0.0"
document_id: "a6a91f1419c472791f0422bcfd8a92f3867243115335a37f2dc4dadef8f64fdd"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/testing-event-driven-architectures-with-signadot/"
published_at: "2024-08-16T21:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:7cbd7b96f0ba63f2fdb71848c47e3d3a4f876844254eac2df9483cbbafabe7f8"
---

# Testing Event-Driven Architectures with Signadot

*Originally posted on*[The New Stack .](https://thenewstack.io/testing-event-driven-architectures-with-signadot/)


**Isolate messages within your queue to test and experiment with asynchronous architectures.**


When doing end-to-end or acceptance testing, there are many ways to allow developers to experiment in well-isolated environments. Asynchronous architectures present a different set of challenges without a single set of best practices for replication and testing, which makes implementing a testing solution more complex.


In my previous article on[testing event-driven architectures](https://www.signadot.com/blog/testing-event-driven-architectures-with-opentelemetry/) , I described why it is better to isolate testing on a message level rather than needing to duplicate Kafka queues or create new topics for testing. This article shows how to implement message isolation on an architecture that includes a Kafka message queue. The same approach can be applied to other queues like RabbitMQ and to managed systems like Google Cloud Pub/Sub and Amazon Web Services ([AWS](https://aws.amazon.com/?utm_content=inline+mention) ) Simple Queue Service (SQS).


## Why Testing Asynchronous Architectures Is Complex


As a testing platform, Signadot heavily relies on context propagation, to propagate the required information to perform routing through all the services, and on routing, to direct testing traffic to the right destination. With synchronous architectures, context propagation is a given, supported by multiple libraries across multiple languages and even standardized by the[OpenTelemetry](https://thenewstack.io/observability-in-2024-more-opentelemetry-less-confusion/) project. There are also several[service mesh](https://www.signadot.com/blog/scaling-environments-with-opentelemetry-and-service-mesh/) solutions, including Istio and Linkerd, that handle this type of routing perfectly.


But with asynchronous architectures, context propagation is not as well defined, and service mesh solutions simply do not apply — at least, not now: They operate at the request or connection level, but not at a message level.


This article shows how to implement message-level isolation using sandboxes combined with Signadot’s recently released[Routes API](https://github.com/signadot/routesapi) .


## Message Isolation


The idea behind this solution is that by performing a selective consumption of messages on consumer workloads, each message is consumed by one (and only one) version of the consumers. For example, if an application has a consumer` C1` (the baseline version of the workload) and I fork it within a sandbox` S1` to create` C1'` (a sandboxed version of the workload), both` C1` and` C1'` will subscribe to the same topic, but only one of them will process the message, depending on its context.


In this image, two messages are passed through the queue to a topic, but only one of the images has the routing key` rk1` , and the sandbox service` S1` is the only one that gets this request.


You may already know that[Kafka](https://www.signadot.com/blog/kafka-step-by-step-tutorial/) can pass an arbitrary header on any request, but it’s worth diving into how to implement the routing needed to make sure a service in a sandbox gets all the requests it needs, and that other services either don’t receive or ignore those test messages.


One of the key primitives within the Signadot Operator is the routing key, an opaque value assigned by the Signadot Service to each sandbox and route group that’s used to route requests within the system. Asynchronous applications also need to propagate routing keys within the message headers and use them to determine the workload version responsible for processing a message.


A sandboxed workload is responsible for responding to multiple routing keys, primarily the one assigned to the sandbox that created it, plus the routing keys assigned to each of the route groups containing that sandbox. Continuing with the previous example, if I have sandbox` S1` , which has been assigned routing key` rk1` , and route group` RG1` has been assigned routing key` rk2` , then the workload` C1'` should process traffic that includes routing keys` rk1` and` rk2` .


On the other hand, a baseline workload should process only the traffic without routing keys or with routing keys that don’t match the keys assigned to any of its sandboxed workloads.


This means the logic and the data required for each of these cases are similar but not the same.


Sandboxed workloads require:


1. The message routing key.
2. The set of routing keys assigned to the current sandboxed workload.


The consumption logic is: If the message routing key is included in condition 2, the set of routing keys assigned to the current sandboxed workload, then process the message; otherwise ignore it.


Baseline workloads require:


1. The message routing key (if any).
2. The set of routing keys assigned to all the sandboxed workloads of the given baseline.


The logic is: If the message routing key is included in condition 2, then ignore the message, as it will be processed by the corresponding sandboxed workload; otherwise process it.


To implement this logic in Kafka, you must make sure that each version of a consumer workload belongs to a different Kafka consumer group so that all workloads will have access to all messages in the topic. A simple solution for sandboxed workloads is appending the sandbox name to the consumer group (which is available as an environment variable), because each sandbox name in Signadot must be unique.


Signadot Operator` v0.15.0` introduced the[Routes API](https://github.com/signadot/routesapi) , a set of endpoints (gRPC and REST) implemented by the[route server](https://www.signadot.com/docs/concepts/architecture) that exposes sandbox routing information within the Kubernetes cluster. In the case of gRPC, the routing information can be accessed by pulling or by using streams (which works in an almost real-time manner). This API can collect the required information to perform the selective consumption logic described above for baseline and sandboxed workloads.


## Try It with a Demo Application


I developed a basic Node.js application to demonstrate how all of this works together. You can find the source code and installation instructions in[Signadot’s GitHub repository](https://github.com/signadot/examples/tree/main/selective-consumption-with-kafka) .


The application is formed by three[microservices](https://www.signadot.com/blog/how-microservices-communicate-with-each-other/) — the` frontend` , the` producer` and the` consumer` ; one Kafka cluster; and one Redis server.


1. The **` frontend`** exposes an HTTP server where static content (HTML, images) can be accessed, and a minimal REST API with two methods, one for publishing a message and another for getting log entries.
2. The **` producer`** exposes a REST API with a single method called from the **` frontend`** every time a user submits a new message.
3. Upon receiving a request, the producer publishes the message in **` Kafka`** with the provided information (by default in the topic **` kafka-demo`** ).
4. The **` consumer`** performs the selective consumption of messages from **` Kafka`** , implementing the logic described in the previous section, using the REST version of the Routes API to pull the required routing keys.
5. The three services implement context propagation through OpenTelemetry and, upon receiving requests or consuming messages from **` Kafka`** , they log those events in the **` Redis`** server.
6. Finally, the **` frontend`** reads those logs and displays them on the user interface (the browser pulls the **` frontend`** API every two seconds).


## Where Signadot Comes into Play


Signadot’s ability to create testing sandboxes is particularly advantageous in scenarios where updates to one service will directly affect several other services.


For example, imagine that I want to change the formatting of event data. This requires changes to both the` producer` and the` consumer` . For this simulation, I want to have messages passing in and out of the Kafka queue and see how logs are written to Redis. I also want to see how the frontend handles the log entries that the new version of the consumer sends to Redis, and whether the new object schema is displayed in the frontend. This level of interdependence of services isn’t unusual, and without the ability to run tests on a complete cluster, I really wouldn’t know if these changes are fully working until the final prerelease deployments to Staging.


This is where Signadot’s request isolation capability really shows its utility: This isn’t easily simulated with a unit test or stub, and duplicating an entire Kafka queue and Redis cache for each testing environment can create unacceptable overhead. Instead, Signadot lets you create a sandbox that doesn’t affect other services and can use the baseline version of any services not included in the test. The tests below won’t change the frontend service and can rely on the baseline version.


## Sending Test Requests


Detailed installation and configuration instructions are available[on the Signadot blog](https://www.signadot.com/blog/using-sandboxes-in-kafka-based-asynchronous-applications/) . Once your demo cluster is ready, you can test using the generated preview URLs (included in the output from the command line infrastructure) or by accessing the[Signadot Dashboard](https://www.signadot.com/docs/tutorials/quickstart/first-sandbox) .


I will use a different method here. Instead of using the preview URLs, I will use the[Signadot browser extension](https://www.signadot.com/docs/overview) to set the desired routing context for my requests. This will allow me to get better feedback from the application. To access the frontend service locally, I will execute a` port-forward` :


1 kubectl port-forward service/frontend -n kafka-demo 4000:4000


Now the` frontend` will be available at` http://localhost:4000/` . Here’s how it looks when I hit the baseline version of the services:


The log entries show that all the messages came from baseline versions of the workloads. Now, if I switch my routing context to the sandbox` consumer-sbx` , I can see that the request is processed by the baseline version of the producer but consumed by the forked consumer:


Switching now to the sandbox` producer-sbx` , my requests will hit the forked producer but be consumed by the baseline consumer:


Finally, if I point my routing context to the route group` kafka-demo` , my request will first hit the forked producer and be consumed by the forked consumer:


This final version shows that:


- Forked versions of the producer and consumer can interact correctly.
- Both are interacting correctly through the Kafka queue.
- Log messages are going to Redis as usual.
- The frontend is handling the new log messages and displaying them correctly.


This level of high testing accuracy isn’t possible without a highly accurate cluster for testing, and that includes replicating the Kafka queue.


## Conclusion


This approach can be applied, with some adaptations, to other queues for[event-based architectures](https://www.signadot.com/blog/testing-microservices-message-isolation-for-kafka-sqs-more/) . For architectures using Kafka, Signadot enables more complete end-to-end testing with minimal lift required to add more tenants or sandboxes for complex testing of interactions. Signadot’s new route groups enable forked versions of multiple services to be tested both individually and as they interact.


For further insights and advice on testing with internal queues, and to share your use case, join the discussion on Signadot’s community[Slack channel](https://signadotcommunity.slack.com/join/shared_invite/zt-1estxm8pv-qfiaNfiFFCaW~eUlXsVoEQ#/shared-invite/email) .
