---
schema_version: "1.0.0"
document_id: "3181e6552741b7a12ab04d178f8f18fa3a4a2610aad7a7ffd3043cf12f8aefe1"
company_key: "doubleverify-holdings-inc-common-stock"
company: "DoubleVerify Holdings Inc."
source_id: "doubleverify-holdings-inc-common-stock-rss-f6469e95d005"
canonical_url: "https://medium.com/doubleverify-engineering/scala-profiling-under-fire-3982471aaf5f"
published_at: "2026-03-10T16:40:33+00:00"
first_seen_at: "2026-07-20T23:17:33.321656+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:b5199f540af321dba4b25779695982fdfd265350af32be86fb4bf96e028f63cd"
---

# Scala Profiling Under Fire

# **Scala Profiling Under Fire**


[DV Engineering](https://medium.com/@dv-engineering?source=post_page---byline--3982471aaf5f---------------------------------------)


8 min read


·


Mar 10, 2026


--


*Written By:*[Max Alekseev](https://www.linkedin.com/in/maxalekseev/)


Developers use various tools to monitor the state of their applications at different stages of their lifecycle. While the codebase is fresh and grows fast under the umbrella of the local environment, we tend to rely on debugging tools, which were created specifically for that purpose. The app is half-baked, and the code is split open. We observe it through the lens of our IDE and with the speed of our brain. Everything is possible; we may pause execution for minutes, and the whole system is a white box — an open book for us.


As development progresses to staging environments or small trials, we are no longer afforded the luxury of debugging. It is impractical to attach a debugger to deployed code, and other debugging tools tend not to scale. A standard method to get insight into non-development environments is to add logs, which are non-structured yet readable.


However, logs cannot capture the whole state of the system. The codebase may not be available, so we drift to a black box mode. Tools such as Lightrun allow developers to dynamically instrument logs and even add basic metrics on the fly. But as load scales, I have found these tools to become prohibitively expensive. Above 100,000 requests per second, agent overhead becomes double-digit percentages of CPU time, and snapshot creation becomes impractical. There are also security constraints, since proprietary information could be easily compromised.


Finally, as services move to production, scale becomes larger, and developers often turn to exposing the application’s state to the outer world through metrics. At DV, we define metrics for all our SLAs and incorporate key performance indicators, aiming to cover as much business logic and system-relevant parameters as possible. Grafana dashboards provide us with great tools to analyze system behavior, either at the current moment or over days, weeks, or even months. Metric creation is static and defined at compile or deploy time. Once a system is deployed to production, the set of indicators can’t be changed. What is not covered by metrics remains largely unknown until the next deployment cycle.


But imagine the situation when something goes terribly wrong. Something that wakes you up in the middle of the night when all the alerts are triggered, metrics are in the red zone, service logs are filled with errors or — worse — empty, and the service itself is mostly unresponsive. You may tend to do the old ***kill -3*** and go for endless scroll over the thread dump of your app, but I believe we can do something better, something more efficient. CPU profiling under fire! Together, we will learn how to identify which part of a service is misbehaving and consuming all the resources.


## **Available Profiling Tools, and Our Tool of Choice**


There are numerous tools on the market for profiling tasks, like YourKit, JProfiler, ScalaMeter, and IntelliJ dedicated plugins, yet this is not something you immediately think of when you have to dissect an unhealthy server during a crisis. Generally speaking, those are used for stress and performance testing.


JVM-level tools, such as Flight Recorder ( ***-XX:+FlightRecorder*** ) and Mission Control ( ***-Dcom.sun.management.jmxremote.**** ), are usually also out of reach for the general population of the services, yet very useful for monitoring a subset of the instances or tracking in staging environments. Either way, if we launch our app without the relevant flags, we are out of luck.


The answer to the ongoing crisis is at the operational system level. The majority of operational systems provide profiling tools for gathering thread-related data. All Linux flavors have ***perf*** tools,[Mac OS](https://github.com/async-profiler/async-profiler) provides ***async-profiler*** , and for both cases, we have an amazing counterpart from Brendan Gregg, FlameGraphs. Thread dump output is very informative, but usually hard to analyze due to its size.


FlameGraph aids us here by performing and analyzing multiple thread dumps and presenting them in a relatively intuitive way. Moreover, due to the external nature of these tools to your application, they will remain responsive even when the app itself is not.


Let’s gather some CPU profiling data on the fly in a model app where multiple issues suddenly arise. And let’s try to figure out, based on it, where the issue is.


## **Real Life Use Case (Almost)**


We will work with a small Scala app that starts to misbehave in one of the randomly defined places. Our goal is to isolate the problematic call.


The logic of the code is trivial. We have a ***SneakyHandler*** class that randomly causes an issue on one of the operations: read, check, or write. The operation is defined on startup, via the ***misbehavingTask*** value. The class is called in an endless loop within the entry point object, ***ProfilingExample*** . We want to see in the runtime, without any logs or metrics available, which operation is problematic.


```text
object ProfilingExample extends App {      val misbehavingTask = SneakyOperation.      values.toList(Random.nextInt(SneakyOperation.values.size))    val service =       new SneakyHandler(misbehavingTask)      // logging PID for the future profiling    println(      s"Process: ${ManagementFactory.getRuntimeMXBean.getName}" +       s" throughput degraded! Problematic task: $misbehavingTask"    )      Iterator.continually {      // executing actions, one of which could lead to significant load      import service._      readFromDiskMock()      performCheckMock()      writeToKafkaMock()        // avoiding too high CPU load      Thread.sleep(100)    }.foreach(_ => ())    }    class SneakyHandler(misbehavingTask: SneakyOperation) {    def readFromDiskMock(): Unit = mockOperation(misbehavingTask, READ_FROM_DISK)    def performCheckMock(): Unit = mockOperation(misbehavingTask, PERFORM_CHECKS)    def writeToKafkaMock(): Unit = mockOperation(misbehavingTask, WRITE_TO_KAFKA)    private def mockOperation(      misbehavingOperation: SneakyOperation,       operation: SneakyOperation    ): Unit = {      if (misbehavingOperation == operation) {        List.fill(100)(Random.nextInt(100) + 1).sorted      } else {        List.fill(10)(Random.nextInt(100) + 1).sorted      }    }  }    object SneakyOperation extends Enumeration {    type SneakyOperation = Value    final val READ_FROM_DISK = Value(1)    final val PERFORM_CHECKS = Value(2)    final val WRITE_TO_KAFKA = Value(3)  }
```


While running the app, we received some basic output.


```text
Process: 60382@DV-KVXPR21QLY throughput degraded! Problematic task: WRITE_TO_KAFKA (SPOILER ALERT!)
```


In a real-life scenario, we would have a crisis here — one of our operations is suddenly much slower than it should be! Let’s run our profiler to figure out what is wrong. Numerous tools allow us to collect thread dump data, depending on the operating system. This data serves as an input to flame graphs. For a Linux environment, we would use ***perf*** and Brendan Gregg’s[scripts](https://github.com/brendangregg/FlameGraph) . For Mac, we have to use an async profiler, since there is no native ***perf*** for Mac OS.


```text
$ ./asprof -d 30 -f graph.html 60382  Profiling for 30 seconds  Done
```


The resulting output is an HTML page with CPU profiling data grouped by stack traces. Line widths are equivalent to the proportion of time this stack section spent in the CPU. Since this information is not directly available for external OS tools, FlameGraph extrapolates it by performing numerous thread dumps over predefined time intervals. If a stack trace is present, this time interval is considered active.


Press enter or click to view image in full size


Alt-Text: The illustration contains a full FlameGraph visualization, both system calls to the right and application-specific stack to the left. The horizontal axis represents the stack depth, and the vertical axis represents the stack traces. Line widths indicate the proportion of time spent in each stack section.


Looking at the FlameGraph for the first time could be pretty intimidating, but don’t panic! Let’s dissect and simplify the result. We may ignore system calls in this case (red and yellow sections to the right of the image) and focus on app stack traces (green, massive left sections). This is where most of the time was spent. Let’s go one by one over function calls. We have to progress from the bottom to the top of the call stack.


Press enter or click to view image in full size


Alt-Text: The illustration contains a detailed flame graph visualization, showing CPU profiling data of our Scala application, starting from the main method. Stack goes up to the point where our three specific test methods are called: writeToKafkaMock, performCheckMock, and readFromDiskMock.


Up until the ***ProfilingExample*** calling iterator, nothing interesting happens. But right after that, we have a split of three app calls, with one of them consuming most of the time. All three functions are on the level of SneakyHandler, but the most significant portion falls on the ***writeToKafkaMock*** call.


Press enter or click to view image in full size


Alt-Text: The illustration contains a detailed flame graph visualization, showing CPU profiling data of our Scala application, highlighting the “writeToKafkaMock” call as consuming the most CPU time.


If we go back to our app console output and reveal the spoiler by highlighting the text, we can see that “write to Kafka” was indeed the call that became problematic on this run. Good catch!


## **Real-Life Limitations**


There are several observations to note and limitations to identify.


1. The distribution between calls doesn’t perfectly match the numbers from the original code. This is because sampling operates with periodic time snapshots. The more time we spend performing the sampling, the closer our results will be to the situation.
2. The app is relatively simple. Do not expect that it will be the same in real production all the time. Usually, the change may be relatively small for the scale of the app, yet critical for a specific flow. FlameGraphs have simple search and zoom-in functions, which allow you to isolate specific areas of the stack. If that isn’t sufficient, there are apps, like Speedscope or Differential FlameGraphs, that allow us to compare different states of the same system.
3. Call stacks can be quite complex on their own, especially in Scala, where the use of anonymous calls is commonplace. This is resolvable by a simple rule: do not wrap your actual logic inside Lambdas and have good, distinctive names. It will serve you well during investigations.
4. Know your application well. Ensure that you have good metrics and logs; they will help you identify any discrepancies from a smooth run.
5. Avoid falling for common biases. If something is executed rarely, it may be because each run is exceptionally costly. If something is not logged, it may be because it is stuck in execution and the code line is unreachable. This is precisely the kind of bias that CPU profiling helps to eliminate.


## **Further Steps**


It is better to be prepared for the crisis and spend time on the investigation itself, rather than frantically picking up the tools on the fly. For containers, it is efficient and cost-free to have a Docker image with installed ***perf*** and flamegraph scripts. They occupy minimal space, consume no CPU while dormant, and are ready to serve in the case of an emergency.


[Pyroscope](https://pyroscope.io/) is another excellent tool that allows you to investigate a current issue or determine the system’s valid state from a few years ago. It also allows you to spot any degradation over time. It is obviously out of the scope of this article, but I warmly recommend that you take a look at it.


Keep in mind that we have just scratched the surface of the subject. It is worth practicing before the actual issue occurs. For in-depth details, I recommend reading[Brendan Gregg’s excellent website](https://www.brendangregg.com/) .
