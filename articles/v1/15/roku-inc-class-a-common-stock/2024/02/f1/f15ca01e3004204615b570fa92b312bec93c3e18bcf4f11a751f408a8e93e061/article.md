---
schema_version: "1.0.0"
document_id: "f15ca01e3004204615b570fa92b312bec93c3e18bcf4f11a751f408a8e93e061"
company_key: "roku-inc-class-a-common-stock"
company: "Roku Inc."
source_id: "roku-inc-class-a-common-stock-rss-7fd84d003d99"
canonical_url: "https://engineering.roku.com/scaling-success-rokus-journey-to-building-a-canonical-metadata-generation-platform"
published_at: "2024-02-08T21:34:20+00:00"
first_seen_at: "2026-07-20T23:21:49.245762+00:00"
fetched_at: "2026-08-20T03:06:22.032514+00:00"
content_hash: "sha256:6df4dfc34244006de981a81fc31e76517d1a843bbc319f41c1308d06d713664e"
---

# Scaling Success: Roku’s Journey to Building a Canonical Metadata Generation Platform

###### ***Written by Polina Vlasenko, Brian Colvin, Aravind Ilangovan***


## **Once upon a time…**


…Roku embarked on a journey to construct a cloud platform, enabling users to search for content across various channels. What began as a modest project with a handful of partners and a content metadata database containing only thousands of movies and series, initially manageable as a batch on an engineer’s laptop, evolved into a tale of growth and popularity


.


## **What happened next?**


Roku Cloud was becoming more and more powerful


, the number of content and features was rapidly expanding


. The count of tv channels grew from just a few to thousands, and the quantity of media content soared from thousands to millions. Eventually, it grew so large that even the largest AWS EC2 virtual machine took over 6 hours to generate a dataset on maximum CPU and memory capacity. It means that soon it would have been impossible to onboard new content. Additionally, addressing a single specific metadata issue required the regeneration of the entire dataset, leading to hours of displaying lower-quality metadata. People started talking—first in a kitchen corner, then the conversation moved to a meeting room. You know how it happens. Finally, it was ready to be said out loud: it is time for a big change!


What was needed from our brand new platform?


- Fast – Aimed for near real-time processing.
- Cheap – Cost-effective without unnecessary load.
- Linearly scalable – To manage the growing dataset.
- Future-proof – Designed to accommodate any new business features.


Easy peasy, right?


So let’s talk about the details.


As the platform utilized as much information as possible, a content change could be triggered by multiple sources, like a partner catalog update, an editorial override, content deduplication cluster adjustment, different TV channel settings, and even a Wikipedia modification. Moreover, there are dependencies between the data entities. For example, series metadata relies on episode information, or actor metadata depends on filmography availability on the Roku platform. All these updates should leave data in a consistent state.


After consideration, we decided to choose


an[actor model](https://en.wikipedia.org/wiki/Actor_model) . The actor model in computer science is a mathematical framework for designing and implementing parallel and distributed systems, where actors are fundamental units of computation that communicate via message passing. This allows for a focus on business features rather than processes and abstracts complex low-level details. In this model, each entity, such as a movie, episode, series, channel, and person, has an assigned actor and their interaction occurs through message exchange. We created a distributed[Akka](https://akka.io/) cluster where each entity is represented by a[sharded actor](https://doc.akka.io/docs/akka/current/typed/cluster-sharding.html) with the unique id known as Roku ID. Akka guarantees that there is only one sharded actor with the same id, and even if the machine goes down, Akka will recreate this actor on another node. Each update in our system is a queue message with the affected Roku ID and update source. We do not allow any data in the message to be used in constructing the content, only the id, forcing the updates to be idempotent. It is useful if you want to change some logic and need to rebuild the dataset and also helps to investigate any production issues.


In case that two update messages come at the same time, they both go to the same actor. Akka actor is thread-safe, and all messages will be processed sequentially, one at a time. So after the actor receives the first message, it “locks” itself until the update is completed and will process the second message after that.


If an entity participates in other content updates often (for example, we use channel metadata to build movie entities), we keep the actor with its state in memory with TTL and lazily initialize it from persistent storage. Actors for other entities like movies are disposable and die after the update is completed. Basically, we created a distributed in-memory cache with lazy initialization from persistence storage and guaranteed consistency.


### Simple actors


##


## **Let’s delve deeper into this**


Each entity actor maintains its actual state that can be requested by other actors. For updates, it creates a child “worker” actor that collects the data needed to construct a new version of Roku content. Depending on update type and source, the entity actor chooses a scenario that identifies which worker actor implementation to use. In case of any error, this worker actor simply terminates and any memory allocated to it is freed.


To retrieve the data, the worker actor sends requests to other entity actors and to router actors, also known in Roku as service actors (similar to the Service pattern in OOP), and releases the thread. Service actors don’t have any state, so they are not unique. They exist on each application node and have a pool of routees that process an actual message from the worker actor. Some messages might be forwarded from one service actor to another while keeping the original sender to deliver a final response. At the beginning of the content update process, the worker actor initiates a periodic Tick message to itself. If the actor hasn’t received any message for a long time, we consider it a zombie actor, and the actor will terminate itself. The application will retry the update from scratch after some delay.


Once any se


rvice actor sends a response back, the worker actor saves the information to its in-memory state. Based on the response, the actor might send more messages to actors, change its behavior, or just continue waiting for the rest of responses.


### Actor scenario


## **Let’s take a look at a simple example**


You’ve probably watched the movie ‘Die Hard.’ When the content generation platform receives a message to rebuild metadata for this movie in Roku, Akka sends a message to a sharded actor with an ID equal to the unique RokuID of this content on Roku.


This actor is unique within a cluster. If this actor isn’t processing any updates at the moment, it changes its state to “in progress,” effectively locking the actor from any other concurrent updates. The entity actor creates a child worker actor responsible for collecting all the information required to create the final outcome.


First, it sends a message to the “deduplication cluster” service actor to obtain all channels where this content is available and the unique IDs of this content in partner feeds. For example, let’s say this movie is available on channel TV1 with sourceId1 and on channel TV2 with sourceId12.


Once the worker actor receives the response, it sends messages to the channel actor TV1 and the channel actor TV2 to retrieve information about TV channel settings, such as which countries this content is available to watch in, whether it’s available for all or restricted to whitelisted devices, and more. Additionally, the worker actor sends concurrent requests with sourceId1 and sourceId2 to the Partner Feeds service actor to obtain metadata provided by partners, including titles, descriptions, and images.


Furthermore, the actor sends a message to the Editorial service actor to check if any information has been edited by the Roku editorial team. Once the worker actor gathers all the requested information, it applies certain logic to determine preferences for titles, images, and other details, and it collects all available view options.


Finally, it persists the output to the datastore and sends notifications to downstream clients.


This is a simplified example of one of the existing pipelines. A real metadata update request triggers hundreds of messages to different actors and execution graph has multiple branches.


### Movie example


## **Back Pressure**


In all scenarios, our system relies on message-driven asynchronous processes. To ensure the system’s stability and prevent overloading, we implement a backpressure mechanism. We carefully manage the number of concurrent updates, making sure it never exceeds the predefined maximum limit to control CPU and memory usage.


Sometimes, external resources may experience temporary slowdowns. During such periods, the worker actor might send messages, but the service actor’s response time could be so slow that the worker actor times out and dies before the service actor can even start processing the message. In these situations, the affected original update is queued for reprocessing in another attempt. However, it is crucial for the system to recover gracefully and avoid processing a message if there is no longer any waiting for a response. To achieve this, we assign a creation timestamp to each message, and we validate that the message is not too old before allowing the service actor to process it.


Consequently, any messages affected by downtime will be discarded, but the system will swiftly recover and process new updates without any delays.


## **Let it Crash**


We are running our cluster on AWS ECS, and if a machine (EC2) crashes, AWS automatically starts a new container on another EC2. The new node uses AWS API to get IP addresses of running nodes and uses them to join the Akka cluster. If an actor from a healthy node sends a message to another node that subsequently crashes before responding, then the actor-initiated request will die after a timeout using the Tick periodic message check. Akka ensures the proper handling of this scenario by transferring all sharded actors from the failed node to other nodes in the cluster. These actors are not created automatically but are initialized upon receipt of their first message, following the rules defined by the shard manager.


Another potential challenge our cluster may face is a “split-brain”, typically occurring during network latency or slowness. To address this, we’ve implemented a routine check that compares the count of Akka nodes with ECS tasks. If the number of Akka nodes falls below the count of ECS tasks, indicating a split-brain scenario, we employ a decisive strategy. In such cases, the larger cluster should take precedence. If the number of Akka nodes (N) exceeds half the count of ECS tasks (N/2), it means that the node is assigned to the larger cluster and should continue working. Otherwise, we initiate a graceful shutdown of the application on that particular node.


## **Tracing**


No system is perfect, and it is essential to investigate and resolve any issues with minimal effort. When problems arise, having a clear understanding of what occurred leading up to the failure is invaluable. However, since all scenarios are asynchronous and stages may run on different machines simultaneously, it is necessary to establish context between these processing components.


To address this challenge, we generate a unique identifier that accompanies each message. This ID, along with content rokuId and other useful information, is included in our logs. This practice enables us to easily retrieve and correlate all relevant log messages based on the entity Roku ID or context ID, ensuring efficient issue resolution.


We also gather footprints for every content update. This allows us to generate and visualize the actual execution graph, revealing the sequence of asynchronous steps and the specific machine on which each step was executed


.


## **Conclusion**


At Roku, we have developed a content aggregation platform built as a distributed Akka cluster, where each entity is represented as an actor. This cluster is also used as a distributed in-memory cache, requiring no additional effort to maintain consistency. The platform ensures that only one update per entity Roku ID is processed at a time. This effectively creates a distributed lock per Roku ID, all managed within the same application.


Our system is linearly scalable and fault-tolerant, with Akka handling the sharding logic within the cluster. This simplicity greatly benefits our engineers in development. The cluster features autoscaling based on input message backlog, ensuring stable CPU and memory usage due to the queue consumer’s back pressure logic.


## **References**


- [Actor Model](https://en.wikipedia.org/wiki/Actor_model)
- [Akka](https://akka.io/)
- [Akka Cluster Sharding](https://doc.akka.io/docs/akka/current/typed/cluster-sharding.html)
- [Split-Brain problem](https://en.wikipedia.org/wiki/Split-brain_(computing))
- [AWS ECS](https://aws.amazon.com/ecs/)


The post[Scaling Success: Roku’s Journey to Building a Canonical Metadata Generation Platform](https://engineering.roku.com/scaling-success-rokus-journey-to-building-a-canonical-metadata-generation-platform) appeared first on[Engineering Blog](https://engineering.roku.com/) .
