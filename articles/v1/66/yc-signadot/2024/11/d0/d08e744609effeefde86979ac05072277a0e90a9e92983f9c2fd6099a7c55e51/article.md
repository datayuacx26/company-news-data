---
schema_version: "1.0.0"
document_id: "d08e744609effeefde86979ac05072277a0e90a9e92983f9c2fd6099a7c55e51"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/kafka-step-by-step-tutorial/"
published_at: "2024-11-06T18:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:45882d4641fce0f820287c7b1fa996a41b5db48090782cd8e93be445286bb3c2"
---

# Tutorial: How to Do End-To-End Testing of Asynchronous Kafka Flows Using Sandboxes

Imagine you are a developer in an engineering team building microservices that communicate asynchronously via Kafka. Setting up all microservices and Kafka infrastructure locally can be challenging, so integration and end-to-end testing are done in a shared staging environment. This setup, however, often means that one developer’s tests interfere with others’ tests. To avoid this, we need a way to test microservices in isolation; including those that produce and consume messages from Kafka. This tutorial will walk through how Signadot Sandboxes enables this.


**Components of the Kafka Demo App**


This demo app is designed to showcase how Kafka can facilitate selective message consumption in a microservices architecture. Each component of the app plays a specific role:


**Producer** : Sends messages to a Kafka topic. We will attach a routing key to each message header to allow sandbox-specific message consumption. Using OpenTelemetry, we’ll propagate the incoming request headers (which include the sandbox routing key) into the Kafka messages, allowing for sandbox-specific message consumption.


**Consumer** : Receives messages from Kafka. Each sandboxed consumer uses the routing key to decide whether to process or ignore messages, hence the selective consumption of message.


**Signadot Operator** : Manages the sandbox environment and ensures messages with specific routing keys are routed to the matching sandboxed consumers. The operator’s ‘Routes’ API also helps Kafka consumers selectively consume messages meant only for their designated sandbox.


**Frontend** : Provides a user interface to send messages and view message logs for both baseline and sandboxed consumer processing.


## **Request Flow**


**Baseline Flow (No matching Routing Key):**


- The producer sends messages to Kafka without a routing key.
- Since there is no routing key (or a routing key that does not have an association with any of the Sandboxes for this consumer), only the baseline consumer, with its own dedicated consumer group, processes the message.
- The baseline consumer logs the message, while sandbox consumers ignore it due to the absence of a matching routing key.


**Sandbox Flow (With Routing Key):**


- The producer sends a message with a routing key added to the Kafka message header.
- The sandbox consumer processes the message if its routing key matches, whereas the baseline consumer ignores it due to the unmatched routing key and group ID.


Now that we understand the flow, let’s get our hands dirty with the detailed steps.


## **Step 1: Deploy the Demo App**


This step will deploy the Kafka demo app (producer, consumer, frontend) to set up a baseline Kafka workflow.


**1.) Clone the Demo App Repository:**


**2.) Deploy the Kafka Demo:**


This will deploy:


- Kafka cluster
- Redis server
- Frontend (A)
- Producer (P)
- Consumer (C)


Let’s check the status of all the pods to ensure everything is up and running.


**3.) Forward frontend for testing:**


This will expose the fronte_nd at localhost:4000_ for interacting with the Kafka demo.


## **Step 2: Test Baseline Behavior Without Signadot Sandboxes**


**1.)** Open[http://localhost:4000](http://localhost:4000/) in your browser to access the Kafka demo frontend and send a message. As there are no sandboxes created yet, the baseline consumer will process the message, which you can see in the frontend interface in below screenshot.


## **Step 3: Producer’s Code to Propagate Routing Key**


(This step can be automated without modifying the producer code by using OpenTelemetry auto-instrumentation, as described[here](https://opentelemetry.io/blog/2022/instrument-kafka-clients/) .)


So what does the producer currently do?


- It obtains the routing key from the incoming HTTP request’s *baggage* header.
- It also includes the routing key in the message headers when sending messages to Kafka.


Let’s walk through the code of producer app.js regarding how it incorporates above two points:


Then make sure the routing key is included when publishing the message to Kafka.


Although the producer includes the routing key, selective consumption based on this key is primarily handled by the consumer.. So it is consumer’s turn to factor in the presence of routing key and then decide whether to consume the message or not. Let’s make some changes to the consumer code.


## **Step 4: Modify the Consumer for Selective Message Consumption**


What exactly will be managed by the consumer?:


- **Create a New Consumer Group for Sandboxes** : That will make sure that sandboxed consumers use a unique consumer group to prevent interference with the baseline consumer’s offsets. Creating separate consumer groups isolates message consumption between baseline and sandboxed consumers. Note that we are following the standard Kafka messaging approach of “consumer group” mode and not the “stand-alone” mode.
- **Manage Offsets Appropriately** : Configure the consumer to start consuming messages from the latest offset, so sandboxed consumers focus on new messages instead of processing old messages.
- **Discard Messages Not Meant for the Sandbox** : Revise the consumer code to use the Signadot ‘Routes’ API, which maps routing keys to sandbox names, to ignore irrelevant messages. Platform teams often encapsulate this logic in a custom Kafka client library that engineering teams can easily use for selective message consumption..
- **Propagate the Routing Key to Maintain Context** : Just in case, If the consumer communicates with other services via synchronous APIs or asynchronously via Kafka messages, we need to ensure that the routing key is also included in the outgoing message/request headers.


Let’s go through important sections of the code one by one.


**Connecting to the Routes API** :


The *run* method in the consumer code triggers periodic calls to Signadot’s Routes API to retrieve routing keys. This enables the consumer to determine if a message is intended for its specific sandbox. Every 5 seconds, the *getRoutes* method sends an HTTP request to the Routes API ( *routeServerURL* ) to fetch the latest routing keys.


Within *getRoutes* , the response is parsed to extract routing keys, which are then cached. These cached keys are referenced by the *shouldProcess* function to decide if a message should be processed based on its routing key. This setup ensures that the consumer is continuously updated with the latest routing information. As a result, consumer is able to isolate messages dynamically for each sandbox environment.


**Creating Consumer Group on the Fly** :


In above code, consumer is making use of *SIGNADOT_SANDBOX* environment variable to create a unique group ID for sandboxed consumers. By using different consumer group IDs, the sandboxed consumers have their own offsets and do not interfere with the baseline consumer. Note that it is the Signadot Operator that automatically injects the *SIGNADOT_SANDBOX_NAME* environment variable into the pods running in a sandbox.


**In Sandbox:**


- The *SIGNADOT_SANDBOX_NAME* environment variable is set to the name of the sandbox.
- The consumer group ID becomes sandbox-consumer-\[SANDBOX_NAME\].


**In Baseline:**


- When running in the baseline environment, *SIGNADOT_SANDBOX_NAME* is not set or is empty.
- The consumer group ID defaults to ‘baseline-consumer’.


**Consuming Messages with the Specified Group ID** :


This will pass the *groupId* to the *consumeMessages* (Defined in *kafka.js)* function to ensure the consumer uses the correct consumer group.


**Extracting Headers from Kafka Messages** :


Above will retrieve the routing key from the message headers. This will determine whether the message is relevant to this consumer or not.


**Deciding Whether to Process the Message** :


The *shouldProcess()* function plays a crucial role in determining if this consumer should process a message based on its routing key. This function is part of the new code written in the custom kafka client wrapper, which matches the message routing key with the sandbox name as returned from the ‘Routes’ API.


The *shouldProcess* function determines if a message should be processed based on the presence of a routing key and whether the workload is a sandbox or a baseline. It retrieves a set of valid *routingKeys* from the cache. If the workload is sandboxed (indicated by *sandboxName* not being empty), it processes only messages with a routing key that matches the sandbox’s routing key set. For baseline workloads, it processes messages without a routing key, ignoring any messages designated for sandboxed workloads. This ensures that sandboxed messages are isolated from baseline processing.


Now that the consumer behavior is updated, the last thing to do is update the Kafka implementation to handle *groupId* and manage offsets appropriately by starting from the latest message. Let’s analyze the changes in Kakfa implementation.


## **Step 5: Modify Kafka Consumer Implementation for Offset Management**


What will be changed in Kafka?


- **Accept Consumer Group ID as a Parameter:** Modify the *consumeMessages* function to accept a groupId parameter, allowing the consumer to specify the consumer group ID.
- **Manage Offsets by Starting from the Latest Message** : Configure the Kafka consumer to begin consuming messages from the latest offset when a new consumer group is created.


Let’s go through some of the core sections of kafka.js:


**Modify the *consumeMessages* Function to Accept *groupId* :**


**Consumer Creation having groupID:**


**Manage Offsets by Starting from the Latest Message:**


## **Step 6: Create Sandboxes**


Let’s create the sandboxes for consumer and producer. Note that there is no change needed in the configuration YAML files for either producer or consumer. We will just use the standard Signadot commands to create these sandboxes.


**Create Producer Sandbox**


**Create Consumer Sandbox**


The sandboxes are created but the pods for consumer and producer are still running with old code. Let’s update the deployment.


## **Step 7: Test Sandbox Behavior with Routing Key**


Let’s verify that sandboxed consumers process messages with routing keys while the baseline consumer ignores them.


-


Use the Signadot Browser Extension to set the sandbox consumer.


-


Open[http://localhost:4000](http://localhost:4000/) in your browser.


-


Send a message through the frontend UI.


**Scenario 1 - Baseline Producer and Baseline Consumer**


‍


‍


**Scenario 2 - Baseline Producer and Sandbox Consumer**


Select the consumer sandbox after enabling Signadot Chrome extension.


Send a message to see whether the baseline consumer or the sandbox consumer consumes it.


As you can see, the sandbox consumer consumes this message. The separate consumer group ID in the sandboxed consumer ensures that messages intended for the sandbox do not interfere with or get processed by the baseline consumer, even if routing keys are used. As you can see, the sandboxed consumer processes the message since the routing key matches, while the baseline consumer ignores it due to its empty routing key set. If you add logs to display the consumer group ID, they will appear in the consumer pod’s logs, confirming our implementation. If you stop your consumer pod and send a message through the demo app, these messages will be ignored, and the consumer will only consume those messages that were sent after it resumed running.


**Scenario 3 - Sandbox Producer and Baseline Consumer**


This time, we will select “Sandbox Producer” when enabling Signadot Chrome extension.


Now let’s send the message and see the results.


So even though you can see there is a routing key, but still the baseline consumer picked this message. This is because the routing key sent by the producer is unmatched. The baseline consumer is programmed to process any message not specifically routed to a sandbox. In cases where a routing key is present but doesn’t match any active sandbox, the baseline consumer treats it as general traffic and processes it by default.


### **Conclusion**


This tutorial has demonstrated how to implement selective message consumption in a Kafka-based application, which enables isolated feature testing within a shared environment. By leveraging consumer groups, routing keys, and Signadot’s sandboxed environments, we were able to separate test traffic from production flows in a controlled manner.


As systems scale, Signadot’s sandboxing capabilities present an efficient way for teams to test feature-specific traffic within complex architectures, helping avoid the overhead associated with duplicating environments. For development teams seeking enhanced testing precision and speed without increased infrastructure costs, Signadot provides a powerful and scalable solution.
