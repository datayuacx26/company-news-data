---
schema_version: "1.0.0"
document_id: "12cb011c75d10ce4a1ac22299b956bb419b14ca45d3444a381379da241f98725"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2026-06-12-how-we-cut-kafka-consumer-deployment-costs-by-83/"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:f2069ee0cbb606157c93dac1d5eb58f2f22e9548f652839dd3c9ef53b78a0da6"
---

# How We Cut Kafka Consumer Deployment Costs by 83%

This post walks through a layered performance investigation that cut PSE-kafka’s infrastructure costs by 83% and ended a run of 19 P1 incidents.


# Problem Background


PSE-kafka (price-search-engine-kafka) is an internal microservice. It consumes hotel price Kafka messages and pushes ads to an external Ads service, which drives real revenue for trivago. But the service had a few persistent problems.


- CPU usage in production sat very low, around 10%.
- On startup, it took a long time to join its Kafka consumer group, which dragged out every deployment.
- It suffered from high consumer lag. The replica count had already reached the partition count, so we could no longer cut lag by adding replicas.


The stakes were high. We push hotel prices to the external Ads service so it can display them and send users to our site. Every hotel price has an expiry time, so as lag grew we increasingly fell back to pushing prices that had already expired. We had to fix this fast.


# Investigation Process


The problem was easily reproduced in our test environment. Send enough messages to the target topic and PSE-kafka’s consumer lag climbs while consumption speed and CPU usage drop. With no hardware limit in sight, there had to be room to optimise in software.


Because the test environment let us experiment freely, I added extra logging — and found that poll() was very slow, sometimes around 10 seconds between calls. With help from colleagues, we spotted in the production logs that poll() there ran slower than max.poll.interval.ms, so the Kafka broker assumed the consumer was dead and kicked it out of the group.


The whole codebase is reactive and non-blocking, so could a blocking call have slipped in? We checked with BlockHound — none. Maybe a thread pool was saturated? We monitored with:


```text
Schedulers.  enableMetrics  ()
```


No pool was full. We also sampled JFR freely in test, and it showed every thread nearly idle.


At this point I’d run out of familiar debugging moves. No busy threads, no CPU or memory limit, yet the application kept calling poll() slowly and couldn’t consume any faster.


Then I started experimenting with the application itself. Switching the Kafka consume code from our in-house KafkaReceiverFlux to the open-source[spring-kafka](https://github.com/spring-projects/spring-kafka) library made the problem disappear — PSE-kafka caught up on lag quickly.


But switching libraries wasn’t a real fix. PSE-kafka is a pull-based consumer; we rely on backpressure so messages can’t overwhelm the application. Moving to spring-kafka would switch us to a push-based model and give that up.


Still, I now knew the consumer code was involved, so I could dig deeper. KafkaReceiverFlux is a self-built consumer that supports only auto-commit — it calls commit() when poll() happens. When processing slows down, commits fall behind actual consumption. Meanwhile,[reactor-kafka](https://github.com/reactor/reactor-kafka) disables auto-commit and commits on a separate thread, so slow processing doesn’t slow commits. **KafkaReceiverFlux** , **reactor-kafka** , and **spring-kafka** are all built on top of Kafka’s official Java client, kafka-clients.


So was switching to reactor-kafka the answer? No — it didn’t help once deployed to staging.


Then I kept going. Reading more code, I found a suspicious snippet:


```text
Mono.  delayElement  (  75   seconds   -   min  (  75   seconds,   now  ()   -   messageCreatedTime))
```


This guarantees each message is processed for at least **75** seconds, giving downstream services time to prepare their data. delayElement is non-blocking, but it’s still bound by the upstream Flux’s concurrency. We use flatMap here, which defaults to **256** . So would raising the concurrency fix it? No — no difference.


We also found a gRPC client thread pool configured too conservatively: 15 threads against a gRPC server with average latency of **7s** , max **30s** , and throughput of **250** msg/s. Yet bumping the pool size alone didn’t solve it either.


This was the most frustrating stretch of the investigation. Every tool said everything was normal, and every fix we tried on its own changed nothing. Then it clicked: maybe the application had several problems at once.


# Root Cause


- KafkaReceiverFlux calls poll() with a delay, so the offset keeps growing.
- delayElement has a built-in ceiling: flatMap concurrency / delay seconds.
- The gRPC server’s high latency turns the gRPC client’s thread pool into a bottleneck too.
- Backpressure and the issues above combine to slow the whole system.


The tricky part: these issues bite in different scenarios, so fixing any one of them alone barely moved the needle.


- Migrate only from KafkaReceiverFlux to reactor-kafka, and we catch up on offset quickly — but slow consumption remains.
- Remove only delayElement, and the gRPC thread pool is still a bottleneck while poll() and commit() stay slow — lag persists.
- Increase only the gRPC thread pool, and delayElement still forces a long in-memory delay — lag persists.


With all three addressed — migrating to reactor-kafka, raising flatMap concurrency to **10,000** , and increasing the gRPC thread pool to **400** — PSE-kafka processed **8k msg/s** .


We wouldn’t ship numbers that aggressive to production, of course. After checking with stakeholders, we added a new IsFinished flag to confirm a message had finished processing, which let us remove the delayElement call entirely. We also fine-tuned the gRPC thread pool for production. Once everything landed in prod:


- Pod count scaled down from **60** to **6** — an **83%** drop in deployment cost.
- Consumer lag disappeared, and the lag **P1** alerts haven’t returned.
- Pod startup time fell from **~60s** to **~10s** , shortening deployments.
- No business impact: Participation Rate and requests to other teams were unchanged.


# Postmortem


So what’s the full story? Back in 2019, when we first added Kafka consuming to PSE-kafka, reactor-kafka wasn’t stable enough to use. Since the codebase was reactive and we wanted a pull-based model, we wrote a heavily customised consumer. It worked well, but it had one flaw: when message processing hit its limit, the app stopped calling poll() — and so stopped calling commit() too.


As the business evolved, we added a gRPC call to another microservice. Its thread pool was sized sensibly at first, but as throughput grew, that size became a bottleneck.


We also wanted to be sure upstream had finished processing our messages, so we added a manual in-memory delay. Mono.delayElement() hands the message to a separate ScheduledThreadPool internally — which carries its own ceiling.


We’d watched PSE-kafka struggle with performance for a long time. But because it was never a single, standalone problem, the root cause stayed hidden. Each time consumer lag triggered a P1 alert, we scaled the pods up, and the problem seemed to go away — **until we committed to a focused initiative to fix it properly.**


# Key Takeaways


-


Pull-based processing is great for stability — but it can also hide problems.


-


In-memory delays are risky. Even when they work fine today, they can come back to bite you.


-


Keep watching and tuning. Parameters that were once plenty can become bottlenecks over time.
