---
schema_version: "1.0.0"
document_id: "68758dd655aa3618e07dff711967fcd6a6dc986817f9dffd378abd2476d18d4d"
company_key: "yc-muzz"
company: "Muzz"
source_id: "yc-muzz-news-import-4a23516b0f87"
canonical_url: "https://engineering.muzz.com/scaling-muzz-social-to-over-a-million-users-on-day-one"
published_at: "2025-05-16T21:55:24+00:00"
first_seen_at: "2026-07-25T16:24:04.083021+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:fcc375c8ce78cefaf3df8b909fc4f699c953edc919d66cf57bc674a13a1be0db"
---

# Scaling Muzz Social to over a million users on day one

At Muzz, our mission is to help Muslims around the world meet.


This year we launched our new social network – Muzz Social – enabling Muslims everywhere to make friends, connect with their local communities and engage with other Muslims globally in a respectful and halal space.


We knew we’d be launching Muzz Social to an existing large and active user base, rather than scaling organically from the ground up over a period of time. Therefore, we focussed on using architectural design patterns and AWS services to build a responsive, real-time application with foundations in data resiliency and cost-effectiveness that can scale seamlessly.


# API Gateway


Muzz Social makes use of an API gateway to proxy all API calls from our mobile apps to our backend gRPC microservices.


Like all Muzz services, the API gateway is written in Go and receives RESTful HTTP requests with data encoded as protocol buffers instead of JSON. Protocol buffers are an efficient binary data serialisation format with a strict contract between client and server. This results in efficient payloads over the wire which helps in conserving bandwidth and improving the response times across mobile networks – key considerations for users with limited data plans or those in regions with poor connectivity. In addition user input can be more easily validated and the wire protocol is backwards compatible.


The API gateway can aggregate responses from several microservices into a single response to simplify the interaction for the client. For example feed items can be fetched from one service and the profiles of the authors of those posts from another. This in turn enables our downstream services to be tightly written to focus on specific business requirements.


In addition to validating input an API gateway enables other design patterns to be implemented such as load shedding, circuit breaking as well as enforcing rate limits.


## Load shedding


Load shedding involves strategically dropping incoming requests to prevent system overload and ensure sustained performance and availability of backend services. Load shedding is particularly important in scenarios where sudden spikes in demand occur, and it serves as a protective measure to avoid system failures.


At Muzz we affectionately call this term “teapotting” named after the HTTP 418 code the server returns to the client (which in turn displays an appropriate message to the user). With Muzz members around the world making over 100 million requests every day to our systems, when we bring services back up following planned maintenance we want to avoid the “thundering herd problem” so we traffic shape by bringing individual countries and clients back online with increasing thresholds.


## Circuit breaking


The circuit breaker design pattern is a software design pattern used to enhance system stability and resilience by preventing a cascade of failures when part of a system becomes unresponsive.


It works similarly to an electrical circuit breaker, which cuts off the electric supply to prevent overheating and potential fires. In software terms, the circuit breaker monitors calls to a particular service or resource, and if it detects a threshold number of failures within a certain time period, it “trips” the circuit breaker.


Once tripped, further calls to the service are automatically blocked for a predefined cooldown period, allowing the troubled service time to recover. During this period, the system can return a default response or raise an error. After the cooldown, the circuit breaker allows a limited number of test requests to pass through. If these are successful, it closes the circuit, resuming normal operations.


## The transactional outbox pattern


When a single operation writes to two or more systems, there is a risk that one of the writes might fail. It’s crucial to manage this scenario effectively within your code.


At Muzz Social, when a new user signs up, several behind-the-scenes tasks are triggered to ensure a seamless onboarding experience. A Muzz Social profile is created in the same MySQL database that hosts both the Muzz account and, if applicable, the Marriage profile. This setup allows us to implement the transactional outbox pattern efficiently.


Here’s how it works: Both the account and social profile data are written within a single transaction to ensure consistency. In the same write transaction, we also create an “outbox” record in another table. This outbox record acts much like sending a message to a queue, but it eliminates the risk of the database write succeeding while the message send fails. Later, a worker process scans these outbox records to perform any required downstream processing. This method ensures data integrity and reliable subsequent processing.


# Event Driven Architecture


We use DynamoDB as the primary database for all content on Muzz Social. With flexible key-value data storage it offers exceptionally high throughput however you have to ensure you plan your data access patterns upfront. Two DynamoDB tables are used for storing entities (such as groups, posts and comments) and another for relationships between entities (group membership, comment relationships and post upvotes).


One of DynamoDB’s best features is Streams. Streams capture a time-ordered sequence of item-level modifications which enables applications to view items as they appeared before and after they were modified, in near-real time.


EventBridge Pipes receives every event in the stream and enriches the event with a Lambda function. This is sent into an EventBridge bus which can trigger multiple downstream actions


For example when a comment is made on a post multiple actions occur:


1. Send the post author a push notification
2. Send the post author’s device a websocket notification
3. Atomically increment the comment count on the post
4. Update the rate limit count for the user


This architecture not only supports high volumes of user activity but also maintains the system’s responsiveness and efficiency. By decoupling the generation of events from their processing, Muzz Social can scale these operations independently, adapting quickly to spikes in user activity without impacting the overall performance of the platform.


To enforce this decoupling of event producers from event consumers messages are routed from EventBridge into SQS queues instead of directly to the consumer. Consumers can then process messages from SQS at their own pace. This layer of abstraction adds flexibility to the architecture, allowing changes to be made to the event processing logic without impacting the event generation.


Additionally SQS provides message durability and retries in the event a consumer fails to process events. This is especially useful during peak traffic times where SQS is also acting as a high performance buffer whilst us enabling us to finely control the scalability of consumers – for example a consumer that sends a push notification can scale more freely, however we wouldn’t want this effect when the consumer is making a database call or downstream connection as then we’d need to scale that in simultaneously.


## The right database for the right job


Driving user engagement is not the only use for event driven architecture. DynamoDB might be our primary data store however it does not support every access pattern that is required to build a social network. EventBridge routes messages to consumers that write to secondary databases that are used for specific complex queries.


Muzz Social employs Amazon Opensearch to facilitate full-text and semantic searches across the platform. This allows users to quickly find content relevant to their interests, such as posts about halal restaurants in London or upcoming community events. Opensearch’s powerful indexing and search capabilities ensure that users receive precise and relevant results at high speeds, enhancing their overall experience on the app.


To manage the intricate social connections and group dynamics within Muzz Social, Amazon Neptune is utilised. Neptune is a graph database service that excels in storing and navigating relationships. For instance, it can efficiently build a feed of posts from groups a user has joined or track interactions between friends and community members. This enables Muzz Social to deliver a rich, connected user experience, suggesting content and discussions that are most relevant to each user’s social circle and interests.


To reduce complexity and data duplication, these secondary databases store as little as information as possible, usually just entity IDs. When detailed information is required, such as for user profiles or detailed post content, the system employs DynamoDB’s BatchGetItem operation. This strategy not only optimises data management but also enhances the performance by reducing the overhead on primary storage.


## Handling Media and Content Moderation


Muzz Social’s architecture supports robust media handling and rigorous content moderation to ensure a safe and engaging user experience. This is critical for maintaining the platform’s integrity and user trust, especially given its large and diverse user base.


To optimise media uploads, Muzz Social employs an asynchronous process. Clients initiate uploads by requesting a pre-signed URL, which directs the upload directly to an Amazon S3 bucket with transfer acceleration enabled. This method speeds up uploads by routing them to the nearest AWS edge location ([over 600 points of presence globally](https://aws.amazon.com/about-aws/global-infrastructure/?ref=engineering.muzz.com) ), enhancing the user experience, especially for those in remote areas.


Once a media file is uploaded, an event is triggered in Amazon EventBridge, which then passes the event details onto an SQS queue. Workers consume these events, processing each media file by validating and analysing its contents. This includes checks for appropriateness and relevance using AWS Rekognition to ensure compliance with community standards.


At Muzz we take user safety and content quality seriously. We use multiple services and techniques to keep our community safe including Amazon Comprehend and AWS Rekognition for automated text and image moderation. These services provide deep learning-based insights to detect inappropriate content or potential violations of platform rules. Additionally, users can report concerning content, which is then reviewed by our Community team, ensuring that any issues are addressed promptly.


Combining automated tools with human oversight allows Muzz Social to maintain a high standard of content quality and user safety. This dual approach helps balance scalability with accuracy, managing the vast quantities of user-generated content efficiently while keeping user interactions safe and relevant.


# Conclusion


Muzz Social’s deployment of a scalable event-driven architecture on AWS exemplifies how sophisticated cloud solutions can be effectively leveraged to manage large-scale social networks. This architecture has enabled Muzz Social to not only handle an extensive user base with millions of interactions efficiently but also maintain high performance and reliability.


Key Takeaways:


- Scalability and Efficiency: Utilising AWS services like DynamoDB, EventBridge, and Lambda, Muzz Social has created a system that scales dynamically and manages high volumes of data efficiently.
- Real-Time Interaction: The platform ensures real-time responsiveness, crucial for user engagement and satisfaction, through its use of event-driven processes.
- Cost-Effective Solutions: By adopting EC2 Spot Instances and optimising data handling with Protocol Buffers, Muzz Social manages operational costs effectively while scaling up.


As Muzz Social continues to evolve and expand, the integration of more advanced AWS services and possibly the adoption of emerging technologies like generative AI could offer new avenues for enhancing user interaction and platform capabilities.
