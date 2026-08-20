---
schema_version: "1.0.0"
document_id: "e84886b479142a8eb7c31cec77c1a4724e33793b8fc626506f958475343d3dc3"
company_key: "groupon-inc-common-stock"
company: "Groupon Inc."
source_id: "groupon-inc-common-stock-rss-99044b78d036"
canonical_url: "https://medium.com/groupon-eng/varnish-to-redis-migration-887ad6d805d2"
published_at: "2022-03-22T15:39:20+00:00"
first_seen_at: "2026-07-20T04:36:53.869647+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:62cc4388cfd08f65dd8736b240c887fc6d3e88c130a4debab67996b63adea593"
---

# Varnish to Redis Migration

Varnish


Redis


Kubernetes


Kafka


Distributed Transaction


# Varnish to Redis Migration


[Ravikumar](https://medium.com/@rtanikonda?source=post_page---byline--887ad6d805d2---------------------------------------)


5 min read


·


Mar 22, 2022


--


At Groupon, we are in the process of migrating our app workloads to AWS EKS. As part of the migration, we are also in the process of re-architecting our services to make them cloud & Kubernetes optimised.


In this article, we will focus on the impact of cloud migration on Varnish caching at Groupon.


For the uninitiated, Varnish is a high-performance reverse caching proxy. For simplicity, you can think of it as a general proxy such as HAProxy or Nginx with caching support so that it doesn’t have to go to a backend service every time it receives a request.


Internally, Varnish cache uses pthreads heavily(to the tune of 1000s) to serve the requests. In contrast, Redis is single-threaded.


> ***Varnish at Groupon & Place Read Service***


We have been using Varnish for many of our high-traffic services. These read-heavy services get traffic to the tune of millions of RPM.


One such service is our place read service which is called on each & every deal page visit; it will be the focus of this article. Place read service stores all the locations a deal can be redeemed.


Ex: Redemption location details of the deal[https://www.groupon.com/deals/parent-big-littles-3](https://www.groupon.com/deals/parent-big-littles-3) is highlighted below.


Press enter or click to view image in full size


> **Traffic pattern of place read service**


The service is called more than a billion times a day with peak traffic of ~2 million requests per minute.


Press enter or click to view image in full size


> ***Current architecture (simplified)***


Press enter or click to view image in full size


The traffic is served by a cluster of 4 Varnish servers (40GB RAM). The current Varnish hit rate is 95%. This is admittedly a low hit rate for a read-heavy service, the main reason being our TTL-based cache invalidation logic.


> ***Varnish cache invalidation***


We are using TTL-based cache invalidation. This usually leads to a problem where Varnish starts bombarding the backend service when most of the keys expire at the same time. Request coalescing is not very helpful when the unique keys count is in millions (which is the case with our service).


As a workaround, we use randomised TTLs (between 30 to 60 mins) so that all the keys don’t expire at the same time.


This TTL-based cache invalidation has impacted the cache hit rate by ~5% negatively as we are deleting even the cache eligible keys at the end of the TTL. We are planning to address this problem by migrating the expiration logic from TTL-based to event-based wherein the keys persist in the cache until they are explicitly evicted by an external agent that knows when the data is no longer up to date.


This should improve the hit rate to ~100% as most of the data is static.


> ***Problem with Varnish on Kubernetes***


While Varnish has been serving us faithfully for a very long time, one real downside of Varnish is that it is not very container friendly when we want to cache large data sets on Kubernetes. The main problems are data duplication (proportional to the number of pods) & frequent cache warming.


As a result, we were tasked with exploring the alternatives.


## Approaches considered.


> ***Migrate Varnish to Kubernetes.***


*Pros*
Performance-wise the best
Request coalescing


*Cons* High levels of data duplications (proportional to the number of pods)
Large size containers is not a good idea
Data consistency


> ***Use Google Guava based in-memory caching.***


One idea is to use Guava for in-memory caching backed by Kafka for cache invalidation. i.e. whenever there is a request to write to the DB, there will be a corresponding Kafka event to invalidate the corresponding keys.


This brings us to a new problem of *dual-write.* Writing to two or more systems without a distributed transaction or an algorithm that ensures eventual consistency can cause data inconsistencies. One way to avoid this is to split the transaction into multiple steps. The Saga pattern, Change Data Capture implementations like Debezium, transactional outbox pattern, etc. use this approach to achieve eventual consistency in the transaction.


In our case, we have decided to go ahead with the transactional outbox pattern, which is essentially a scheduled job that runs every 30 minutes to find all the modified keys & fire corresponding cache invalidation events.


Press enter or click to view image in full size


*Pros* Simple & as fast as Varnish
Provides key level access to cache invalidation


*Cons* JVM heap size limitations
High levels of data duplication (proportional to the no. of pods)
Data consistency
Scalability


> ***Use Redis for caching.***


Another option is to use Redis as a separate cache storage layer. Redis is widely used across Groupon. On top of that, Redis 6 has introduced ***client-side caching*** which is a very useful feature for our use case as the top 1% of the keys get >90% of the traffic in our service.


*Client-side caching is a technique used in order to create high-performance services. It exploits the available memory in the application servers, which usually are distinct computers compared to the database nodes, in order to store some subset of the database information directly on the application side.*


Press enter or click to view image in full size


*Pros:* Centralised & decoupled from app
No data duplication
Linearly scalable
Provides a rich set of advanced data structures
Client side caching


*Cons:* Nothing substantial! ** Redis with ***client-side caching*** almost matched the Varnish performance & we use ElastiCache which is a managed Redis solution from AWS. Groupon has a centralised DevOps team to take care of Redis servers so there is minimal maintenance overhead on the service team.


**Conclusion:**


After a thorough analysis, we went ahead with the Redis approach as it is as good as Varnish latency-wise (with client-side caching), cloud-friendly, and scales horizontally. Our team is well-versed in Redis & ElastiCache (which is a managed Redis solution from AWS) made the transition easy.


Ref:
1)[https://redis.io/topics/client-side-caching](https://redis.io/topics/client-side-caching)
2)[https://redis.io/commands/client-caching](https://redis.io/commands/client-caching)
3)[https://quantumagile.fr/distributed-data-for-microservices-event-sourcing-vs-change-data-capture/](https://quantumagile.fr/distributed-data-for-microservices-event-sourcing-vs-change-data-capture/)
