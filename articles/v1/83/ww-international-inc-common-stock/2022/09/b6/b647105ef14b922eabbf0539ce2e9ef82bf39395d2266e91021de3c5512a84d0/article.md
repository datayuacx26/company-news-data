---
schema_version: "1.0.0"
document_id: "b647105ef14b922eabbf0539ce2e9ef82bf39395d2266e91021de3c5512a84d0"
company_key: "ww-international-inc-common-stock"
company: "WW International Inc."
source_id: "ww-international-inc-common-stock-rss-f4e3db081dfb"
canonical_url: "https://medium.com/ww-tech-blog/replacing-an-existing-backend-service-a-retrospective-f4df68d3014a"
published_at: "2022-09-28T18:54:17+00:00"
first_seen_at: "2026-07-20T23:20:13.695109+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:8163e53d59616bc8ee3a4aa018ed4337feae6ac788fc23edb25fee6f4b18dad9"
---

# Replacing an existing backend service: A retrospective

Kafka


Postgres


Node


JavaScript


Expressjs


# Replacing an existing backend service: A retrospective


[Alex Oxrud](https://medium.com/@aoxrud?source=post_page---byline--f4df68d3014a---------------------------------------)


13 min read


·


Sep 28, 2022


--


WeightWatchers® encourages members to live a healthy lifestyle, which usually involves improving eating habits, sleeping better, mindfulness, and exercise. Today we are going to talk about how we replaced the legacy health tracking system that’s responsible for tracking sleep, weight, and exercise data and some of the lessons we learned along the way.


**WHY**


The legacy system relied heavily on a third-party service that acted as a middleman between data providers like Apple Health, Fitbit, Garmin, and our WeightWatchers services. They were responsible for integrating with those providers and normalizing the data before sending it to WeightWatchers. There were various drawbacks with the legacy system:


- Did the issue occur on the member’s device? The data provider? The third-party service? WeightWatchers services?
- The database design did not scale well because it wasn’t originally designed to support third-party data and it was hard to track updates to existing data.
- There was little auditability which made it difficult to debug.
- It was expensive to operate and maintain.


There was an opportunity to rebuild the application so it is reliable and correct to address those pain points and also support future use cases, we called it Health Tracking Service.


**HEALTH TRACKING SERVICE (HTS)**


HTS is responsible for integrating with third-party data providers, storing exercise data, and forwarding sleep and weight data to our respective microservices.


Since HTS was going to replace an existing system, we used that as an opportunity to deploy the service to the production environment in the background and send it production-level reads and writes which helped us discover issues and address scaling concerns without impacting members.
When HTS was ready, we would switch the traffic from the existing system to HTS without the impacting the member’s experience.


We started the design process by utilizing a diagramming tool that helped us reason about the flow of data and present our ideas and methods to the engineering council for review.


We ended up creating 26 (and counting) pages of diagrams from very high level to very low level and it helped communicate ideas effectively.


Press enter or click to view image in full size


High level diagram of the components that make up the Health Tracking Service


The Health Tracking System is denoted by the purple colored boxes. At the core of HTS is a Node.js HTTP server that is responsible for handling communication between external clients and internal systems. It is serving tens of thousands of requests per minute to support our WeightWatchers apps and data from third-party data providers like Fitbit. We recognized that providers will send large amounts of data into HTS continuously and processing synchronously would present some performance and scaling challenges. For that reason, we decided to use a streaming architecture where the core of HTS simply queues the provider’s data for asynchronous processing.


**STREAMING ARCHITECTURE**


We used[Apache Kafka](https://kafka.apache.org/) for our event-based streaming system, which allowed us to process data in near real time. That grants better auditability since each event is a single immutable event from which we can derive data in a deterministic and repeatable way. The event from each provider would be placed in a provider-specific Kafka topic with multiple partitions. Each partition would have its own consumer, which allowed us to scale each provider independently. Each consumer would be responsible for validating and processing that particular provider’s event.


Press enter or click to view image in full size


An illustration of how Apache Kafka topic, partitions, and consumers might be visualized.


Before the event would be placed in the topic, we recorded it into an event log database table that would be updated every time the event entered a new phase. The event log entry also helped deduplicate events in the case there was a network issue or the provider sent duplicate requests before the original request was processed.


We used a hash function on the event as the unique identifier to determine if there is a duplicate entry. This worked well for push events since it contained all necessary data, but presented challenges with ping events. A ping event indicates that there are updates that need to be fetched; so while the event payload is the same, the fetched data from the provider may have changed. In the case of ping events, we added an additional check to see if we’ve seen the hash and it has not been processed.


Press enter or click to view image in full size


De-duplication business logic that supports ping and push type events


**THE EVENT LOG**


One important tool for debugging is the ability to trace data so all events are logged to a PostgreSQL table, which keeps track of the state of the event as it moves through the system.


It allowed us to have some insights about the system:


- Who is the event for?
- How was it initiated? (provider notification vs manual action)
- Was the event picked up for processing?
- Did the event succeed or fail?
- If it failed, what was the reason?
- How long does it take to process the event?


**THE PROCESSING**


In general, the processing boils down to:


- Fetch incoming data from the provider (in the case of ping events).
- Verify the integrity by validating the provider data against a known schema.
- Transform, filter, and aggregate the data.
- Fetch the member’s previously stored data to compare against the incoming data.
- Store the updated data.


The event might have information about the member’s exercise, sleep, and weight. HTS makes its best effort to process all the data in the event. If part of the event cannot be processed, it will skip that part and continue processing the rest of the event.


There are many systems that are involved in processing a single event, which means there is a large surface area for failures.


**REASONS FOR FAILURES**


Failure to process an event can have many culprits, including but not limited to:


- It failed validation because it expected a specific data structure and/or values.
- It failed to fetch data from external or internal systems; maybe there was a network fault, service didn’t respond fast enough, or the service was unavailable.
- It failed to read from the database.
- It failed to write to the database.


**RETRY MECHANISMS**


Because the system was designed with immutable events in mind, it guarantees — no matter if the event gets processed once or multiple times — that it will result in the same set of operations. This makes implementing a retry mechanism easier.


The retry mechanism consisted of detecting failures and inserting the event into a dead-letter queue, which was just another Kafka topic that would try to process the event again.


Replaying the event in order to try again is very easy, but it has one significant drawback: If the event failed because a rate limit has been exceeded, then it will quickly retry again. There is no concept of backoffs built in without impacting the overall throughput of the system. HTS retries failed events a couple of times before giving up.


In the case that all the retries executed during the outage time period, it would have consistently failed and given up. It means that once the external systems are restored, the events are not processed again. This is where our scheduled job comes in that runs every night during off-peak hours that is responsible for retrying failed events in the hope that the outage has been resolved by then.


Press enter or click to view image in full size


Complete look at how events are processed and retried and stored


The streaming architecture provided by Apache Kafka guarantees that events are delivered in order and have high throughput due to processing events in a distributed manner. The system is able to derive all state updates from a single event thus replaying the event makes it easier to recover from bugs and makes HTS more fault-tolerant.


**APACHE KAFKA NEWBIES**


For my team, this was the first time using Apache Kafka and[node-rdkafka](https://github.com/Blizzard/node-rdkafka) and we had made some assumptions about how they worked that were true until they weren’t.


> *“The bugs that cause these kinds of software faults often lie dormant for a long time until they are triggered by an unusual set of circumstances. In those circumstances, it is relieved that the software is making some kind of assumption about its environment — and while that assumption is usually true, it eventually stops being true for some reason.”*
>
>
> Source: Richard I. Cook: “How Complex Systems Fail,” Cognitive Technologies Laboratory, April 2000.


Press enter or click to view image in full size


A person attempting to play table tennis using a pool cue stick


**KAFKA: RUNNING HEADLESS**


All Kafka consumers need a connection to the database so they can process an event and update state. If the database is unavailable, then the Kafka consumers are instructed to crash so they can be restarted. It turns out that[node-rdkafka operates the consumers in a separate node thread](https://github.com/Blizzard/node-rdkafka/issues/222#issuecomment-325055014) . So when the database went down, it killed the main javascript thread, but the Kafka consumer stayed alive listening to new events and[committing their offset](https://docs.confluent.io/platform/current/clients/consumer.html#offset-management) , but never persisting the state into the database.


When the database encountered a fatal error, the code was calling *process.exit(1),* which wasn’t picked up by the Kafka consumers. Once we changed our implementation to *process.kill(process.pid, “SIGINT”),* node-rdkafka was able to detect the going down signal and crashed the consumer as designed.


**KAFKA: RACE CONDITION #1**


Our Quality Assurance (QA) team flagged an issue that sometimes the exercises are duplicated and it was very hard to replicate.


We noticed that when events were published to the Kafka topic they were assigned random partitions that would be processed by different consumers and create a[race condition](https://en.wikipedia.org/wiki/Race_condition) .


We addressed this issue by using a[deterministic algorithm](https://en.wikipedia.org/wiki/Deterministic_algorithm) to place events from the same member in the same partition.


Press enter or click to view image in full size


Use the event’s member uuid as the partitioning key


**KAFKA: RACE CONDITION #1.1**


The QA team observed that duplicate data did reduce in frequency, but did not eliminate the problem. The system guarantees that an event from the member will always be processed by the same consumer. However, each consumer was configured to process multiple events in parallel to maximize the throughput. When there were two or more back-to-back events from the same member, it created a race condition.


Once we understood that multiple events were being processed simultaneously in the same consumer, it revealed a well-known database problem due to the[read-modify-write](https://en.wikipedia.org/wiki/Read%E2%80%93modify%E2%80%93write#:~:text=Read%E2%80%93modify%E2%80%93write%20instructions%20often,read%E2%80%93modify%E2%80%93write%20sequences.) approach taken when processing events.


Press enter or click to view image in full size


The diagram on the left is the assumption we had, the diagram on the right is what was happening.


Two events for the same member are picked up for processing in parallel. Each processor makes a database query and arrives at the same conclusion that it needs to create a new record in the database. So both processors create a record resulting in duplicate data.


Since events from the same member are guaranteed to end up in the same consumer, we could detect back-to-back events using an in-memory hash map and throttle the processing so one event was processed at a time per member while still being able to process multiple members concurrently.


The event log gave us auditability, the streaming architecture gave us reliability and some fault tolerance, but we needed visibility into performance and availability. That’s where New Relic came into the picture.


**NEW RELIC AND THE MEMORY LEAK**


Integrating with New Relic allowed us to have visibility into the overall health of the system from a single dashboard. We could see the throughput of the systems, any errors raised, and set up alerts in case certain thresholds were exceeded. The integration of New Relic is a simple process of adding the agent to the application and setting up a configuration file. We added New Relic, confirmed it was working, and moved on.


After exposing our Kafka consumers to production level traffic, we observed that the consumers would consume all available memory and crash. Only to be restarted again and go through the cycle again.


We thought it was our[least recently used caching strategy](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_(LRU)) to avoid querying the database multiple times for the same member, which was somehow growing beyond the established limits.


We had many theories as to what could be causing the memory leak, but we struggled to find the culprit in the code. This haunted us for a week until an engineer from the platform team reached out to us and said our Kubernetes pods were running out of storage and it revealed that the New Relic agent log files were gigabytes in size. That led us to recheck all the New Relic agent settings and documentation. Under “New Relic Logging Level,” we saw


> “ *Do not use debug or trace logging unless New Relic Support asks you to use them. These levels of logging can generate excessive overhead. For most situations, use info.”*
>
>
> [https://docs.newrelic.com/docs/apm/agents/nodejs-agent/installation-configuration/nodejs-agent-configuration/#logging_config](https://docs.newrelic.com/docs/apm/agents/nodejs-agent/installation-configuration/nodejs-agent-configuration/#logging_config)


During the implementation of the New Relic agent, we had set the logging level to “trace” and that made it into the production environment. Changing the logging level back to “info” brought our system back to good health and the requests per minute increased by 10,000.


Press enter or click to view image in full size


New Relic charts showing system metrics spikes and sharp drop offs. After fix was applied, the charts look stable.


**DATABASE: CHANGE DATA CAPTURE**


The PostgreSQL database used by HTS uses[Debezium](https://debezium.io/) to stream changes directly from the database. This allowed other microservices within the WeightWatchers ecosystem to subscribe to HTS events by simply listening to a Kafka topic.


Only the data that is stored in the database is broadcasted, so if downstream services need to interpret the data, they will need to duplicate the business logic already owned by HTS. It was more performant and distributable to store the derived data along with the raw data so downstream services did not have to duplicate the business logic.


In order to store the derived data, we had to do some migrations and that presented some challenges:


**DATABASE: SCHEMA MIGRATION**


As the HTS requirements evolved, the database schema also changed. This means that some code releases required database schema migrations. These same migrations worked perfectly in our staging environment so we thought it would be safe to run it in production.


That’s when we relearned that some statements work perfectly fine and fast in small data sets, but become problematic when executed in large data sets. In one particular example, we had an alter table statement that added a column with a calculated value. This caused the entire table to be locked, which halted all processing of the system. Our database administrators were quick to jump in and help us unlock the table and come up with a better migration strategy.


**DATABASE: SECONDARY REPLICA**


The database migrations going forward resulted in spinning up a secondary database that follows the primary database. Run the migrations in the secondary database and then promote the secondary as the primary and drop the old primary.


This approach worked very well, but one time we were too eager to shut down the old primary and we did so before the replication could finish, so we ended up losing data. We were, however, able to recover the data by restoring the old primary and manually copying over missing records.


**DATABASE: THE SEQUENCE**


Spinning up the secondary database to do migrations has been very successful, but at some point during the evolution of the system, we made a decision to introduce a sequential primary key to the event logs table, which receives a lot of IOPS. This decision forced us to better coordinate the switchovers between old primary and secondary because we had to determine the rate at which the sequence increments over a period of time and then set the starting sequence in the secondary at a much higher level so the replication didn’t create conflicting entries. In retrospect, the only reason for the sequence was merely for comparing events age by looking at the IDs and to allow easy ordering of the set which all could’ve been done by the “created at” column.


**DATABASE: MAX CONNECTIONS**


The database selected for HTS started small and we increased it in size as we needed it. HTS started with the goal of supporting three providers and quickly grew to nine (and counting) different providers. Each provider has many consumers that all connect to the database.


When a new deployment is made, new Kubernetes pods are brought online and then traffic is slowly switched over to the new pods and the old pods are brought down.


For a brief period of time during the deployment, the database connections almost doubled. This caused us to reach the max connections allowed by the database so this meant that new deployments could not connect to the database and would be stuck in a crash loop and the deployment would fail.


The about 300 pods that help power HTS each maintained a connection pool of 10 and 90% of them were idle. We have bumped the max database connections allowed, reduced some unnecessary pods, and switched to[pgbouncer](https://www.pgbouncer.org/) , which is a connection pooler for PostgreSQL.


The switch to pgbouncer reduced the connections to PostgreSQL by 80%, which also reduced CPU usage by 30%, increased available memory by 25%, and reduced IOPS by 30%.


Press enter or click to view image in full size


Decline in connections once pgbouncer began to be utilized


**CONCLUSION**


Over the last seven days, the system has averaged 41,900 rpm and had a peak of 85,600 rpm.


Average traffic pattern of Health Tracking Service from Sept 20 — Sept 26


The official launch of the Health Tracking Service in early August 2022 went very smoothly and didn’t experience any significant issues because we battle tested the system with production traffic in the background. There were a lot of things we learned that we will keep in mind as we continue to grow and build new systems. **** This wouldn’t have been possible without the help of the amazing and talented people of WeightWatchers.
