---
schema_version: "1.0.0"
document_id: "0109dd8046e41563d1d43053cf0e5092eaae17525443fe4869bb49652631bf55"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/how-we-optimized-our-akka-application-using-datadogs-continuous-profiler/"
published_at: "2021-09-30T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:e6bcff2cce1dd5550e89e0343ad42304a82c7aa6ff7b39abe6bbcac251d0054e"
---

# How we optimized our Akka application using Datadog’s Continuous Profiler

Vladimir Zhuk


Performance bottlenecks are not always (or some might say, never) where you expect them. We have all been there, knowing that there was a latency, but not finding it in any of the expected places. There is nothing worse than seeing that there’s a latency and having to guess at where it is. This is where a profiler comes in - profiling is a form of dynamic program analysis that measures software resource usage, like CPU time or memory allocation, which means you can actually understand your program’s performance rather than guess at what might be slowing it down. At Datadog, we had a perfomance issue (completely unexpectedly) related to a third party framework called[Akka](https://akka.io/) .


When developing in Java or Scala, using frameworks like Akka simplifies writing high-load concurrent, distributed applications and reduces the complexity of writing thread-safe code that is easily parallelized on multi-core machines to maximize application performance. But unfortunately, it abstracts many low-level implementation details. The types of details that can cause performance bottlenecks if used in a sub-optimal way.


Our performance issue was caused by the` ForkJoinPool` in our Java application that is based on the Akka framework. We could see that our Java application was using way more CPU than we expected. But it was the type of thing that we didn’t notice right away, it was a gradual realization that our app just shouldn’t be using that much CPU. The issue was responsible for 20-30% of CPU used by this service until we detected it with the Datadog Continuous Profiler. This article explains how we discovered and fixed this issue.


## How we spotted the issue


We use Akka to parallelize the processing of log events. Akka offers a runtime for actor-based concurrent and distributed computation. It is based on binding different units of work, called actors, to a set of dispatchers, which are responsible for scheduling execution of actors’ tasks, and are built on top of various kinds of thread pools.


There is an Akka dispatcher based on the` ForkJoinPool` to handle consecutive stages of our log processing pipelines.


So we initially thought the issue was with log processing, and we rolled out what we were confident was going to yield much better performance! While running A/B tests of a new algorithm to optimize log parsing, we expected to see a reduction in CPU usage. However, enabling this optimization showed almost no gain, and in some cases, it even caused performance degradation. We were dissapointed, and so we dug in further, using the[Continuous Profiler](https://www.datadoghq.com/blog/code-optimization-datadog-profile-comparison/) .


After looking at the flame graphs, the optimization we made helped us reduce CPU time spent on log parsing, as expected. However, we started to spend more CPU time inside of the` ForkJoinPool.scan()/Unsafe.park()` methods. It wasn’t clear why this method caused a performance issue. To investigate, we changed the summary table to display the top list of threads consuming the most CPU time.


We noticed something very odd: in the summary table, the threads from the dedicated “work” pool only spent ~1% of CPU inside of this stack frame. We expected them to be the majority. So we checked the threads from other thread pools and came across the main offender, the threads of the default Akka dispatcher spent most of its CPU time in this method. The default Akka dispatcher is used when no other dispatchers are explicitly set for a given actor and is also based on` ForkJoinPool` by default. By diving deeper into the stack frames of these threads, we noticed that many of them were running the same actor – LatencyReportActor, which reported some latency-related metrics for a sample of log events.


## Detailed analysis of the root cause


To find out how this actor could be related to spending more time inside` ForkJoinPool.scan()/Unsafe.park()` methods, we needed a deeper understanding of what this method does under the hood.


We discovered that the` ForkJoinPool` attempts to maintain enough active threads by dynamically adding, suspending, or resuming internal worker threads. When there are not enough worker threads, the pool spawns a new thread. When there are no tasks for an active thread, the thread is suspended (by calling` Unsafe.park()` ). After being idle for a certain period (60 seconds by default), worker threads get terminated.


In the method` ForkJoinPool.scan()` , a worker thread checks pending tasks, and if there are too many pending tasks compared to the number of active threads, it either resumes a suspended worker thread, if available, by calling` Unsafe.unpark()` , or it spawns an entirely new thread. Also, the set pool parallelism level caps the maximum number of threads.


The observed behavior is explained by having an **irregular flow** of incoming tasks combined with the **high maximum number of threads** of the underlying thread pool. Because the` ForkJoinPool` keeps activating and deactivating worker threads between the periods of stable load, there are short, frequent spikes, which waste CPU time on making expensive` Unsafe.park()/unpark()` native calls.


This hypothesis perfectly matched` LatencyCheckpointActor` ’s behavior, which indeed does not have a regular flow of tasks – it receives a few hundred events every second, reports the latency for them in a matter of milliseconds, and then waits until the next batch of events arrive. The number of actor instances, as well as the size of the underlying thread pool, were limited only by the number of available processor cores. So every second, Akka wakes up all 32 threads (the number of cores) to handle events as fast as possible, and then puts them to sleep once all events have been processed.


## How we fixed it


To verify this theory, we moved this actor from the default Akka dispatcher to our main “work” dispatcher, which is also based on a` ForkJoinPool` , but has a stable flow of log processing tasks, with a single-line config change:


```text
1   latency.reporter {    2            router  :   round-robin-pool    3            nr-of-instances  : {  the number of cores  }    4            dispatcher  :   work-dispatcher    5   }
```


And the overall CPU usage dropped by 30% on average across different services, and profiles confirmed that this was due to the decrease in CPU time spent inside of` ForkJoinPool.scan()` . The default Akka dispatcher’s thread pool also shrank from 32 to 2 threads, confirming our hypothesis.


## Conclusion


If your application is using` ForkJoinPool` , directly or using the Akka framework, keep an eye on how much CPU time is being spent inside the` ForkJoinPool.scan()` method. If it is getting too high (over 10-15%), you might need to adjust your thread pool settings to keep the load more or less stable and avoid unnecessary parking and unparking of threads. If this does happen to you, consider trying the following options:


-


limit the number of actor instances (if you are using Akka)


-


limit the maximum number of threads in a thread pool


-


reduce the number of used thread pools to spread the load


-


balance the load with some kind of task queues which can accumulate frequent short spikes of tasks


The ultimate goal is to keep the number of active threads in a` ForkJoinPool` more or less stable, avoiding short-time fluctuations.


There were a few lessons we learned here:


First, don’t trust your intuition when it comes to performance in production. Our initial regression was introduced by a minor, non-critical change (adding an auxiliary reporting actor), which seemed innocent performance-wise. An innocuous change can have dramatic performance impact.


Second, it’s much easier to overlook performance issues if they happen inside some trustworthy third-party code. Though many of our engineers had seen the CPU usage inside the` ForkJoinPool.scan()` , they didn’t take action because it was not directly related to the changes they were making - the stack frame was not linked in any way to our application codebase. Naturally developers tend to trust third-party library code, but tools like continuous profiling can help to spot and debug performance bottlenecks in any part of your application.


-
-
-
