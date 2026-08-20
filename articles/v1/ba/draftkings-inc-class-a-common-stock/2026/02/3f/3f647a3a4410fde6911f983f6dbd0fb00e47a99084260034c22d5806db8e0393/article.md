---
schema_version: "1.0.0"
document_id: "3f647a3a4410fde6911f983f6dbd0fb00e47a99084260034c22d5806db8e0393"
company_key: "draftkings-inc-class-a-common-stock"
company: "DraftKings Inc."
source_id: "draftkings-inc-class-a-common-stock-rss-016c40719db2"
canonical_url: "https://medium.com/draftkings-engineering/implementing-breakpoint-tests-that-include-system-recovery-209b584712fa"
published_at: "2026-02-06T13:12:41+00:00"
first_seen_at: "2026-07-20T04:35:13.112015+00:00"
fetched_at: "2026-07-28T22:21:24.537254+00:00"
content_hash: "sha256:816a697edbe6138a1668e59d9d6fdc320f906d8d35af20b00f3760d6907a15bd"
---

# Implementing breakpoint tests that include system recovery

# Implementing breakpoint tests that include system recovery


[Zlatin Stanimirov](https://medium.com/@stanimirovv?source=post_page---byline--209b584712fa---------------------------------------)


11 min read


·


Feb 6, 2026


--


**If you don’t know your system’s breaking point, the universe will assign one to you.**


Wonder how you can learn your system’s hard limits? Break Point testing. It is a type of performance test that provides crucial insights for any backend system. A tool often used to verify your system can meet expectations during its peak, such as the Super Bowl, Black Friday, or Christmas. Let’s explore the two most common definitions. Later in the article, we will look at how you can define the criteria for passing or failing a Break Point test, and look at code examples you can run and modify.


Most systems have uneven traffic throughout the day. An hour of downtime causes revenue loss and customer dissatisfaction, which can lead to reputation damage.


**Not every hour of downtime is equal.** Downtime during the peak can lead to more severe revenue and brand damage.


Every system has its limits. Hardware is finite.


For the purpose of this article, a breakpoint means that the system does not honor its Service Level Agreement(SLA). The service doesn’t need to be down, as long as it doesn’t honour its SLAs. This means that the system will be broken from a user perspective. At best, it is slow; at worst, it is completely unresponsive.


## The classical Breakpoint tests


Although there isn’t a strictly defined industry standard, the[K6 introduction to breakpoint testing](https://grafana.com/blog/2024/01/30/breakpoint-testing/) gives a very solid overview.


Press enter or click to view image in full size


In short, the goal of breakpoint testing is to understand a system’s **hard limits** .


There are a few key factors in breakpoint testing:


1. **The load is gradually increased** — it is usually increased linearly or in steps. An increase in steps means that after increasing the load, you maintain it for some time before increasing again.
2. **The system reaches extreme load** — It is way above its expected normal load.
3. **The system must break before the test is concluded** — Detecting how the system breaks is different depending on the system. Push and Pull APIs have different failure points that must be monitored to detect system failure.
4. **Auto-scaling** — Most systems nowadays have some sort of auto-scaling. If the autoscaling limit does not reflect the production experience, the breakpoint test is invalid.


Breakpoint testing, in its common definition, is a critical part of ensuring the operational excellence of any service.


## Extended Breakpoint Testing


In DraftKings, our definition is **more end-to-end** than the classical one. Our breakpoint testing **also verifies** a system’s recoverability.


Press enter or click to view image in full size


The two parts of Breakpoint testing in DraftKings


**How does the breakpoint test scenario play out?**


First, the load is gradually ramped up until the system reaches its breakpoint.


Once the breakpoint is reached, the system reduces the load to a bare minimum, even as much as zero in some cases, and waits for the system to recover.


As long as the system has recovered within the Recovery Time Objective, the test application detects it and runs a test that ramps up the load to a **realistic level** to verify that the recovery is full from a performance and functional perspective. If the system doesn’t recover within its RTO, the test is considered failed.


The extra verification, the one post-system recovery, gives extra confidence that the system can remain operational and that any overload won’t cause cascading failures and downtime.


…


It is time to move away from the theoretical and into the practical. In the next few sections, we will build a system to test and several breakpoint tests. For the breakpoint tests, we will use[Locust](https://locust.io/) , and the system under test will be run via NodeJS.


[There is a repository containing all code samples. Make sure to clone it before you proceed to the excercise section.](https://github.com/stanimirovv/breakpoint-test-example)


## A short introduction to Locust


The tests are going to be based on[Locust](https://locust.io/) . Although less performant than K6, it allows more freedom in writing complex scenarios. Typically, Locust can do up to 10,000 RPS from a process instance, whereas K6 can easily reach six-figure requests — scaling to 100,000, 200,000+ if there are enough CPUs on the machine.


Also, Locust giving more freedom doesn’t mean that K6 isn’t flexible. The K6 abstraction of stages is great for most types of performance tests, but it can’t detect a complex state, such as the system being down, and change test behavior based on it.


Locust, on the other hand, allows for more control due to its CustomLoadShape class. Towards the end of the article, we will explore how to create a performance test with Locust that can manage K6’s scale.


## Building the System Under Test


Before we start writing the tests, we need a system to test.


You can find it under the system_under_test directory in the checked-out repo. You can view the[source here.](https://github.com/stanimirovv/breakpoint-test-example/blob/main/system_under_test/system_under_test.js) As you can see, it is a simple, zero-dependency NodeJS server with a single endpoint that returns a prime number.


We experimented with the limit. For a 500K base value, the processing time spiked between 50 and 60 milliseconds. You can play around with the base value if you want the API call to be slower (or faster).


Here is an example of the diff:


```text
- const limit = 100_000 + Math.floor(Math.random() * 40_000);  + const limit = 500_000 + Math.floor(Math.random() * 40_000);
```


Verify the system under test by running it in a terminal


```text
node system_under_test/system_under_test.js
```


Next visit` localhost:3000` and you should get a response like:


```text
{"path":"/","testedUpTo":128118,"primesFound":11994,"computeMs":23}
```


## Building your first Locust test


If you have experience with Locust, feel free to skip this section.


[Follow all the steps in the Prerequisites section.](https://github.com/stanimirovv/breakpoint-test-example/tree/main) This should give you a Python virtual environment with locust installed.


Let’s run the system under test in a terminal:


```text
node system_under_test/system_under_test.js
```


Make sure that the second terminal has the Python virtual environment loaded so that it can access locust. Then, run locust by calling the locust command.


```text
source .venv/bin/activate  cd locust_hello_world  locust
```


You should see something similar:


Press enter or click to view image in full size


Press Enter, and the LocustUI will be opened in your browser.


Below is an example configuration. You can keep the defaults, but don’t forget to add the host (with http:// in front!)


Press enter or click to view image in full size


Starting the test, navigating to the Charts tab, and waiting a bit may give something like this:


Press enter or click to view image in full size


Congratulations! You have successfully run your first Locust test.


Let’s have a[quick look at the test’s source code.](https://github.com/stanimirovv/breakpoint-test-example/blob/main/locust_hello_world/locustfile.py) The entire test is less than 10 lines of code. The test itself is a class that inherits from HttpUser and defines a single task: visiting the root URL of the system under test.


## Defining the Breakpoint Test


Before we build the Breakpoint Test, we must define its conditions.


Success metrics are derived from business KPIs. For simplicity, we will use predefined metrics for this article. In a real-world scenario, it is crucial to set the SLAs with feedback from the business stakeholders. Latency and request success rate are a starting point, but maybe there are business-impacting metrics that you can use instead that will be closer to the user experience.


As defined earlier, there are two main parts we are interested in:
1. When does the system break?


2. Does it recover after going down? If yes, how much time does it take?


We need to define what ‘a system to break’ actually means. Does it mean that the process died? Or that it started returning 500s? Or that it stopped meeting its response time Service Level Agreement(SLA)? Something else?


In our case, if the **P95 response time is over 100 milliseconds,** the system is considered non-compliant. The system also has a[Recovery Time Objective (RTO)](https://csrc.nist.gov/glossary/term/recovery_time_objective) , which in this case is **the P95** to be under 170ms.


## Building the Breakpoint Test


First, let’s examine the classical breakpoint test. It detects that the system has failed, but does not verify its recovery.


[You can see the test source code here.](https://github.com/stanimirovv/breakpoint-test-example/blob/main/breakpoint_test/locustfile.py)


The test itself is fairly simple — the UserTask defines the API call that must be queried. The Website user defines how often the task will be executed for every test user.


The interesting part is the custom load test shape. As you will notice, the user load isn’t linear; it’s stepwise. Pausing on the same load for a few seconds isn’t mandatory, but staying on a number of users enables some in-flight requests to finish, which will more clearly showcase the difference between different load levels.


The conditional statement to verify if the test has failed uses[locust’s built in statistics.](https://docs.locust.io/en/stable/_modules/locust/stats.html)


Assuming you already have the system under test running and that you have a terminal in the root of the repository, this is how you can run the breakpoint test example:


```text
cd breakpoint_test  locust
```


Here is how the test results should look from the console when the breakpoint is reached:


And the Locust graphs:


Press enter or click to view image in full size


The graphs show that P95 latency is steadily increasing until it reaches the breakpoint.


Let’s add the second part of the breakpoint test — **the part that tests system recovery.**


Here is how a breakpoint test that also includes recovery testing may look:


Press enter or click to view image in full size


Note, the reduction of users may take a few seconds due to in-flight requests.


[Let’s move on to the test that also includes recovery testing.](https://github.com/stanimirovv/breakpoint-test-example/blob/main/breakpoint_test_with_recovery/locustfile.py)


This custom LoadTestShape begins with its setup. It then splits into three main branches.


Here is a diagram explaining the code:


Press enter or click to view image in full size


The ‘Breakpoint ramp up’ is chronologically first but last in the code. It is a simple branch that calculates the target players depending on the test runtime and returns the new target.


The second branch, and arguably the most complex one, is the recovery period. The recovery period has two concerns: to ensure that the target user count is 1, so the system can catch up and recover. Afterwards, it starts to ramp up the load again, to ensure that the recovered system can still handle the same load as before.


Run the Node.js server (the system under test) first, then run the Locust tests. (Hint: if the system under test isn’t live the tests will fail) Feel free to experiment with different thresholds, recovery times, and ramp-up times to get a feel for the test’s shape.


```text
cd breakpoint_test_with_recovery  locust
```


Once recovered, the test will continue to ramp up until the test time expires.


Press enter or click to view image in full size


This is an example run. As you can see, the number of users ramps up, and then when the P95 threshold is hit, it starts to decline. Due to in-flight requests and locust internals, this can take a few seconds. Then, there is a stabilization period, and the ramp-up begins again.


This test shape is a skeleton that should be modified to your specific breakpoint testing use case. Sometimes validating the recovery is more complicated, as it may also validate the response, not just the response time.


## Scaling locust


As already mentioned, Locust isn’t the most performant runtime because it is in Python. Heavy, production-grade setups can require hundreds of thousands of requests per second, so we must reach them beyond a single core.


The first thing to do is to make sure the FastHttpUser is used:


```text
class WebsiteUser(FastHttpUser):      wait_time = constant(0.5)      tasks = [UserTasks]
```


This should bump the requests per second into the low 5 digits, around 10–12k.


If more requests are needed, there are two routes forward.


The simplest route is to run the performance test suite several times in parallel. This can be done using multiple tabs, the Linux parallel command, or by running multiple replicas if you are hosting the tests on Kubernetes.


The downside of this simple approach is that there is no coordination between the different testing agents. This lack of coordination can lead to slightly less precise results as not all agents will hit the breakpoint at the exact same time. In practice, the desynchronisation between agents is minimal, as when a system degrades, it often degrades for all users.


The more complicated solution is to use Locust[distributed load generation.](https://docs.locust.io/en/stable/running-distributed.html) This provides mechanisms for coordinating load between workers, leading to more precise coordination and results, but it increases the complexity of writing the tests and adds maintenance overhead.


Consider whether the synchronization is actually needed and whether the extra complexity is worth it. If not, take the simpler approach.


## Common Pitfalls


A few recurring issues can silently sabotage the accuracy or usefulness of breakpoint tests:


- **Stale tests, fresh bugs —** As your system evolves, so must the tests. A test that once passed might no longer reflect reality — especially if infrastructure, caching, or routing changes. To avoid this, automate test versioning and review test relevance regularly as part of CI/CD.
- **200 OK ≠ Success —** Just because the API responds doesn’t mean the operation is completed. Async patterns (e.g., queueing) can mask failures behind a green checkmark. You want to validate not just the response codes, but also behavior, side effects or downstream metrics.
- **Global state ruins parallel runs —** Shared counters or flags can cause unpredictable behavior in multi-worker setups. Keep the state isolated and reset between runs. Reset state explicitly between test runs and avoid shared counters unless synchronized.
- **Do not skip the service observability** — breakpoint tests offer a good opportunity to test your system’s metrics, alarms, and dashboards. How fast will you be able to detect that the system is unhealthy? Reset state explicitly between test runs and avoid shared counters unless synchronized.
- **Take system warmup into account —** when setting the recovery SLA, do not think only of the system start-up. If your system loads data in memory, performs pre-computations,or populates caches, etc., make sure this time is also taken into account in the SLAs.
- **Validate the user experience —** sometimes, even though the system is recovered, the client doesn’t notice it. Maybe the Error Budget is exceeded? Maybe there is a retry policy with an exponential backoff that allows too generous backoff times. If the system recovers after 5 seconds, but the next retry is after 2 minutes, the system appears unavailable from a user perspective.


Keeping these issues in mind will help you save time!


## Wrapping up.


In this article, we explored how to go beyond traditional breakpoint testing by incorporating system recovery validation — a critical capability for modern, resilient systems. We began with a simple Node.js application to simulate CPU-bound load and walked through creating Locust tests that identify failure thresholds and verify post-failure recovery.


As next steps, the tests can be added to the CI/CD pipeline or hourly/daily runs, which send alerts if the tests fail or the delta between test results is different.


We built a custom LoadTestShape in Locust, to gradually ramp up traffic, detect when SLAs are breached, and simulate a recovery phase followed by a slower ramp-up to ensure the system stabilizes under normal conditions.


This type of breakpoint test gives teams a deeper understanding of their system’s resilience under pressure and prevents cascading failures during critical traffic spikes.


Whether you’re preparing for high-stakes events or integrating performance testing into your Software Development Life Cycle (SDLC), this approach provides a robust blueprint for ensuring both **capacity** and **recoverability** .
