---
schema_version: "1.0.0"
document_id: "edbff5ee241d234df50a85818e211ea4f49b8ab5d88ebea25e9cb6c1e5c74ad9"
company_key: "draftkings-inc-class-a-common-stock"
company: "DraftKings Inc."
source_id: "draftkings-inc-class-a-common-stock-rss-016c40719db2"
canonical_url: "https://medium.com/draftkings-engineering/soak-testing-as-part-of-sdlc-a07b2dbc3fd7"
published_at: "2025-03-18T14:51:09+00:00"
first_seen_at: "2026-07-20T04:35:13.112015+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:52ab7a55792e2a420d4b21b19aec3d256d6217c5cb7e189e0d7ebd77e0fc37bf"
---

# Soak Testing as part of SDLC

Software Engineering


Software Development


Performance Testing


# Soak Testing as part of SDLC


[Zlatin Stanimirov](https://medium.com/@stanimirovv?source=post_page---byline--a07b2dbc3fd7---------------------------------------)


9 min read


·


Mar 18, 2025


--


## Overview


Picture this: your software runs flawlessly in initial tests, and the load tests pass with flying colors, only to crash spectacularly after a few days in production.


It’s the kind of nightmare scenario that keeps developers and engineering managers up at night.


How do you make sure your application can handle continuous use over a long period? That’s where Soak Testing comes in. A type of test that could be the key to your application’s long-term success.


Press enter or click to view image in full size


## What is Soak Testing?


**Soak Testing** is a type of performance testing that checks how a system handles a steady, high load over an extended period. The goal isn’t to break the system but to see how it performs under 80–90% of its expected peak load for a long time.


Press enter or click to view image in full size


The goal of **Soak Testing** is to find how the system will behave with an expected load for an extended run.
Some questions that **Soak Testing** will answer are:
1. Is the test going to deplete the system’s resources?


2. Is resource consumption ever-growing, even if not depleted?


3. Does the P99 latency remain the same for the entire testing period?


If a system **passes a Soak Test** , it will stay stable, not run out of resources, and maintain a consistent P99 response time.


In most tests, the infrastructure setup and load stay the same. What usually changes is the software version and test duration.


The duration of a soak test will depend heavily on the system and its seasonality. If the peak is for hours, days will do. If it is days, consider soaking for a week or more. If the duration is too short, not all issues will be discovered; too long and resources will be wasted.


## What issues do Soak Tests catch?


**Soak Tests** help identify issues caused by resource depletion. This could include running out of compute power, memory, storage, network capacity, or downstream service limits. Using both end-to-end and isolated tests gives the best chance of catching all potential issues.


Some apps will have memory leaks. Applying a high load over a long period will gradually worsen any resource leaks. Eventually, they may crash the process with an out-of-memory exception. Alternatively, they might appear in monitoring dashboards as steadily increasing memory usage or unusual spikes during garbage collection.


DataDog’s[ForeCast monitors](https://docs.datadoghq.com/monitors/types/forecasts/?tab=linear) or other advanced analytical tools can reduce the soak test’s run time while still providing valid indications of potential problems.


Press enter or click to view image in full size


Some apps may have wrong cleanup policies. A database table or Kafka topic that keeps growing indefinitely can lead to performance issues. As the volume increases, performance will degrade. In some cases, it may even fill up the disk and cause the system to crash. Depending on the storage technology used, when the disk is filled, the system restore may require high access privileges or be a risky, non-trivial task. Such a scenario will require full-blown disaster recovery and may cause significant downtime to the system.


Additionally, **Soak Testing** has a good track record of capturing garbage collection anomalies or performance degradation caused by them. Memory management is platform-specific; however, for most platforms, the behavior of the runtime is different between low and high load and bursts of many requests. Sustaining a reasonable load over a long duration can highlight performance degradation due to memory allocation or fragmentation.


Even if the cleanup policies protect the system from disastrous scenarios, the performance of growing collections will degrade over time. **Soak tests** are a good way to understand if there are degradation tipping points after which the system will not be within its SLAs.


A positive side effect of **Soak Testing** is that as the size of a collection increases it may showcase missing indexes not only in SQL databases like MySQL or Postgres but also in NoSQL databases like MongoDB.


**Soak tests** can also be used to estimate the required resources of the system. During a Soak Test, suppose 2 billion requests are made. In a production environment, this might represent a week’s expected load. This data can help estimate the required network bandwidth, log storage, and other storage needs more realistically. Soak Tests are not the only way to estimate resource usage. However, they involve a high number of real executed requests. This makes it easier to detect any non-linear growth or sudden spikes in network and storage costs. These patterns are harder to spot when comparing resources based on a smaller sample, like 1 million requests.


During high-profile events, DraftKings consistently handles millions of requests per minute for several hours, so 1 million requests is hardly a representative number.


Additionally, **Soak Testing** can help with capacity planning, for example, planning to add new Brokers to a Kafka Cluster or new Shards to a database.


In short, **Soak tests** can help discover potentially fatal issues with the system. Soak Tests run for a long time, but building a Soak Testing suite doesn’t have to be expensive. If load, spike, or stress testing suites already exist, you can repurpose their test cases, reducing the effort required. What will change is the user count, requests per second, and test duration.


## How to develop a Soak Test suite?


Interested in giving **Soak Testing** a try? Here is what you need to do.


First, you need a performance test suite. Functional test suites are aimed to cover as many code paths and go through as many edge cases as possible. In contrast, performance tests typically focus on the most common or successful execution paths. If your project has a dedicated functional test suite, chances are some of the tests can be reused with small tweaks.


The main difference is how they run. Functional tests usually do not run on a live service. However, soak and performance tests must run on a live service. Otherwise, any detected resource exhaustion issues will not be accurate.


To run **Soak tests** effectively, you might need a dedicated test runner. Most built-in test runners are optimized for functional tests. In contrast, specialized frameworks such as[k6](https://k6.io/) or[Locust](https://locust.io/) are tailored for performance testing, allowing you to simulate real-world load scenarios more accurately.


Here is what a trivial functional test may look like, note that the code is executed against a server object, not a live instance:


Press enter or click to view image in full size


*Functional test example*


Here is how the same test can look as a performance test suite: It is run against a live instance, and extra parameters define the load under which the endpoint should be tested.


Press enter or click to view image in full size


*Performance test example*


Another aspect is that the functional test validates the response more strictly, whereas the performance test validates only whether the request is successful, not the format, payload correctness, etc.


When designing your load testing suite, it’s essential to **replicate the real-world interplay** of API calls, not just the overall throughput. Rather than simply pushing a high volume of requests to individual endpoints, your **Soak tests** should mirror the actual usage patterns observed in production.


This means incorporating realistic sequences of API interactions, which can help uncover performance issues that only emerge when specific endpoints are called in combination. Such issues might include resource contention or subtle timing dependencies that are not evident when endpoints are tested in isolation.


Moreover, a soak test’s effectiveness lies in its ability to simulate frequent and infrequent yet resource-intensive operations. By structuring your tests to reflect the diverse and interdependent flows of user behavior, you gain a more comprehensive understanding of your system’s performance under sustained load.


This structured approach enhances the fidelity of the simulation and facilitates the early detection of hidden bottlenecks, ensuring that your live service can gracefully handle peak and off-peak traffic alike.


When you have a soak test suite that covers some of the flows you are interested in, it is time to start getting value out of it.


## How to integrate Soak Testing as part of the SDLC?


The goal of the software development life cycle is to create a process that will deliver business value at reasonably predictable intervals, at reasonable cost, and with a low defect rate.


Its goal is to maximize business value while reducing the risk and the generated waste. Having feedback as early and fast as possible is the de facto standard to reduce both risk and waste. The earlier a problem is identified the less time, resources, and less risk there is in fixing it.


Since **Soak tests** run for a while, they usually sit in a different place in the CI/CD process than the unit or integration tests. **Soak tests** should rarely be a requirement for merging a pull request. They are often run post-merge into the main branch and provide feedback asynchronously.


Proper metrics and traces must be in place for soak tests to bring value. Unless the origin of the problem can quickly be identified, the lack of system observability may require engineering effort better spent fixing the observability issues first. When a bottleneck is discovered, it usually needs to be addressed. The less time spent debugging the bottleneck, the quicker the bottleneck will be fixed.


The best-case scenario is to run the tests in full isolation, meaning that the infrastructure is created and seeded for a specific test suite. It lives for the duration of the test run and is recycled afterward.


In DraftKings, we have developed our[own solution](https://medium.com/draftkings-engineering/enhancing-quality-and-resiliency-draftkings-isolated-testing-solution-part-1-7f08bb8dd4a7) called[CleanRoom](https://medium.com/draftkings-engineering/enhancing-quality-and-resiliency-draftkings-isolated-testing-solution-part-2-dfa6c8638514) .


If you have something similar to CleanRoom, you can run the **Soak tests** in parallel. This allows you to test every PR against them and get feedback. If the latest changes cause performance issues, you’ll know right away.


If running Soak Tests in isolation is not possible, running them once per sprint can be a good compromise. This approach keeps infrastructure costs low while still providing valuable data and helping catch performance regressions.


Common ways in which to **utilize Soak tests** :


1. Run **Soak Tests** only when changes are made to specific directories. This is especially useful if your project uses a mono repo.
2. Trigger a run after work hours and have a daily report. That way if there is a degradation only the merges from a single day must be reviewed
3. Trigger a run automatically, once per sprint. This greatly reduces infrastructure costs. However, it works best for teams with lower bandwidth. A longer gap between tests means issues may accumulate over time.
4. Run **Soak tests** before significant events for your business


There isn’t a silver bullet for the optimal frequency to run soak tests. It is always on a case-by-case basis. If you need to determine it, start from the essence of your business.


Ask yourself if there are significant moments during the year that are critical to the business’s long-term success. Consider the infrastructure costs and the time to parse and understand the output of the tests.


In DraftKings, different teams run soak tests at different cadences. However, before significant events we always run company-wide performance tests that also include soak tests.


## Wrapping up


**Soak Tests** can catch a lot of problems that usually aren’t at the forefront of a developer’s mind and are often non-trivial to fix. They can provide insights into an important slice of the system which can help a development team shape parts of their technological backlog.


Making them a part of a product’s software development life cycle will reduce the risk of production outages, increase system visibility, and even showcase missing observability tools or disaster recovery runbooks for your product.


If you want to sleep soundly without worrying that your service may run out of resources, try soak tests!
