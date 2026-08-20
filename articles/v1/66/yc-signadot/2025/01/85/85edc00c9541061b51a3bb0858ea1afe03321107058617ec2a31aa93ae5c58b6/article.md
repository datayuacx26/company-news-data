---
schema_version: "1.0.0"
document_id: "85edc00c9541061b51a3bb0858ea1afe03321107058617ec2a31aa93ae5c58b6"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/tutorial-how-to-do-end-to-end-testing-of-asynchronous-google-pub-sub-flows-using-sandboxes/"
published_at: "2025-01-13T09:10:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:0e3d6e10c5f671b88824470ba924d3a3c970980953a544a249398e210128fb2b"
---

# How to Test Asynchronous Google Pub/Sub Flows End to End Using Sandboxes

Imagine you are a developer in an engineering team building micro-services that communicate asynchronously via Pub/Sub. Setting up all micro-services locally with Google manage Pub/Sub infrastructure can be challenging, so integration and end-to-end testing are done in a shared staging environment. This setup, however, often means that one developer’s tests interfere with others’ tests. To avoid this, we need a way to test micro-services in isolation; including those that publisher and subscriber messages from Pub/Sub. This tutorial will walk through how Signadot Sandboxes enables this.


**Components of the Pub/Sub Demo App**


This demo app is designed to showcase how Pub/Sub can facilitate selective message consumption in a micro-services architecture. Each component of the app plays a specific role:


**Publisher** : Sends messages to a Pub/Sub topic. We will attach a routing key to each message header to allow sandbox-specific message consumption. Using Open-telemetry, we’ll propagate the incoming request headers (which include the sandbox routing key) into the Pub/Sub messages, allowing for sandbox-specific message consumption.


**Subscriber** : Receives messages from Pub/Sub. Each sandboxed subscriber uses the routing key to decide whether to process or ignore messages, hence the selective consumption of message.


**Signadot Operator** : Manages the sandbox environment and ensures messages with specific routing keys are routed to the matching sandboxed subscriber. The operator’s ‘Routes’ API also helps Pub/Sub subscriber selectively consume messages meant only for their designated sandbox.


**Frontend** : Provides a user interface to send messages and view message logs for both baseline and sandboxed subscriber processing


## **Request Flow**


**Baseline Flow (No matching Routing Key):**


- The publisher sends messages to Pub/Sub without a routing key.
- Since there is no routing key (or a routing key that does not have an association with any of the Sandboxes for this subscriber), only the baseline subscriber, with its own dedicated subscriber group, processes the message.
- The baseline subscriber logs the message, while sandbox subscriber ignore it due to the absence of a matching routing key.


‍


**Sandbox Flow (With Routing Key):**


- The publisher sends a message with a routing key added to the Pub/Sub message header.
- The sandbox subscriber processes the message if its routing key matches, whereas the baseline subscriber ignores it due to the unmatched routing key and group ID.


Now that we understand the flow, let’s get our hands dirty with the detailed steps.


## **Step 1: Deploy the Demo App**


This step will deploy the Pub/Sub demo app (Publisher, Subscriber, frontend) to set up a baseline Pub/Sub workflow. **‍**


**1.) Clone the Demo App Repository:**


**2.) Deploy the Pub/Sub Demo:**


This will deploy:


- Redis server
- Frontend (A)
- Publisher (P)
- Subscriber (C)


Let’s check the status of all the pods to ensure everything is up and running.


**3.) Forward frontend for testing:**


This will expose the fronte_nd at localhost:4000_ for interacting with the Pub/Sub demo.


## **Step 2: Test Baseline Behavior Without Signadot Sandboxes**


Open[http://localhost:4000](http://localhost:4000/) in your browser to access the Pub/Sub demo frontend and send a message. As there are no sandboxes created yet, the baseline consumer will process the message, which you can see in the frontend interface in below screenshot.


## **Step 3: Producer’s Code to Propagate Routing Key**


What does the producer currently do?


- It obtains the routing key from the incoming HTTP request’s *baggage* header.
- It also includes the routing key in the message headers when sending messages to Pub/Sub.


Let’s walk through the code of producer app.js regarding how it incorporates above two points:


Then make sure the routing key is included when publishing the message to Pub/Sub.


Although the producer includes the routing key, selective consumption based on this key is primarily handled by the consumer.. So it is consumer’s turn to factor in the presence of routing key and then decide whether to consume the message or not. Let’s make some changes to the consumer code.


## **Step 4: Modify the Consumer for Selective Message Consumption**


What exactly will be managed by the consumer?:


- **Create a New Subscriber for Sandboxes** : That will make sure that sandboxed subscribers use a unique subscriber to prevent interference with the baseline subscriber’s offsets. Creating separate subscriber isolates message consumption between baseline and sandboxed subscribers.
- **Discard Messages Not Meant for the Sandbox** : Revise the subscriber code to use the Signadot ‘Routes’ API, which maps routing keys to sandbox names, to ignore irrelevant messages. Platform teams often encapsulate this logic in a Pub/Sub client library that engineering teams can easily use for selective message consumption..
- **Propagate the Routing Key to Maintain Context** : Just in case, If the subscriber communicates with other services via synchronous APIs or asynchronously via Pub/Sub messages, we need to ensure that the routing key is also included in the outgoing message/request headers.


Let’s go through important sections of the code one by one.


**Connecting to the Routes API** :


The *run* method in the subscriber code triggers periodic calls to Signadot’s Routes API to retrieve routing keys. This enables the consumer to determine if a message is intended for its specific sandbox. Every 5 seconds, the *getRoutes* method sends an HTTP request to the Routes API ( *routeServerURL* ) to fetch the latest routing keys.


Within *getRoutes* , the response is parsed to extract routing keys, which are then cached. These cached keys are referenced by the *shouldProcess* function to decide if a message should be processed based on its routing key. This setup ensures that the subscriber is continuously updated with the latest routing information. As a result, subscriber is able to isolate messages dynamically for each sandbox environment.


**Creating Subscribers on the Fly** :


In above code, subscriber is making use of SIGNADOT_SANDBOX_NAME environment variable to create a unique subscriber for sandboxed subscribers. By using different subscribers, the sandboxed subscribers have their own subscriber and do not interfere with the baseline subscriber. Note that it is the Signadot Operator that automatically injects the *SIGNADOT_SANDBOX_NAME* environment variable into the pods running in a sandbox.


**In Sandbox:**


- The *SIGNADOT_SANDBOX_NAME* environment variable is set to the name of the sandbox.
- The subscriber name becomes pubsub-grp-\[SANDBOX_NAME\]


**In Baseline:**


- When running in the baseline environment, *SIGNADOT_SANDBOX_NAME* is not set or is empty.
- The subscriber name defaults to ‘pubsub-grp’.


**Extracting Headers from Pub/Sub Messages** :


Above will retrieve the routing key from the message headers. This will determine whether the message is relevant to this consumer or not.


**Deciding Whether to Process the Message** :


The *shouldProcess()* function plays a crucial role in determining if this subscriber should process a message based on its routing key. This function is part of the new code written in the custom Pub/Sub client wrapper, which matches the message routing key with the sandbox name as returned from the ‘Routes’ API.


The *shouldProcess* function determines if a message should be processed based on the presence of a routing key and whether the workload is a sandbox or a baseline. It retrieves a set of valid *routingKeys* from the cache. If the workload is sandboxed (indicated by *sandboxName* not being empty), it processes only messages with a routing key that matches the sandbox’s routing key set. For baseline workloads, it processes messages without a routing key, ignoring any messages designated for sandboxed workloads. This ensures that sandboxed messages are isolated from baseline processing.


## **Step 5: Create Sandboxes**


Let’s create the sandboxes for subscriber and publisher. Note that there is no change needed in the configuration YAML files for either publisher or subscriber . We should login to Signadot dashboard follow the Signadot instructions to create these sandboxes.


## **Step 6: Test Sandbox Behavior with Routing Key**


Let’s verify that sandboxed subscribers process messages with routing keys while the baseline subscriber ignores them.


-


Use the Signadot Browser Extension to set the sandbox subscriber.


-


Open[http://localhost:4000](http://localhost:4000/) in your browser.


-


Send a message through the frontend UI.


**Scenario 1 - Baseline Publisher and Baseline Subscriber**


**Scenario 2 - Baseline Publisher and Sandbox Subscriber**


Select the subscriber sandbox after enabling Signadot Chrome extension.


As you can see, the sandbox subscriber subscribe this message. The separate subscriber in the sandboxed subscriber ensures that messages intended for the sandbox do not interfere with or get processed by the baseline subscriber , even if routing keys are used. As you can see, the sandboxed subscriber processes the message since the routing key matches, while the baseline subscriber also receives a copy of the message due to its separate subscription, it drops the message because it is not intended for the baseline. The message has a matching sandbox whose routing key aligns with the key in the message. If you add logs to display the subscriber group ID, they will appear in the subscriber pod’s logs, confirming our implementation. If you stop your subscriber pod and send a message through the demo app, these messages will be ignored, and the subscriber will only subscribe those messages that were sent after it resumed running.


**Scenario 3 - Sandbox Publisher and Baseline Subscriber**


This time, we will select “Sandbox Producer” when enabling Signadot Chrome extension.


So even though you can see there is a routing key, but still the baseline subscriber picked this message. This is because the routing key sent by the publisher is unmatched. The baseline subscriber is programmed to process any message not specifically routed to a sandbox. In cases where a routing key is present but doesn’t match any active sandbox, the baseline subscriber treats it as general traffic and processes it by default.


### **Conclusion**


This tutorial has demonstrated how to implement selective message consumption in a Pub/Sub-based application, which enables isolated feature testing within a shared environment. By leveraging subscriber, routing keys, and Signadot’s sandboxed environments, we were able to separate test traffic from production flows in a controlled manner.


As systems scale, Signadot’s sandboxing capabilities present an efficient way for teams to test feature-specific traffic within complex architectures, helping avoid the overhead associated with duplicating environments. For development teams seeking enhanced testing precision and speed without increased infrastructure costs, Signadot provides a powerful and scalable solution.
