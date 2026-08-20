---
schema_version: "1.0.0"
document_id: "40973e64c8e282b4179f4f503046a892ec3f306cf1bb043b469b67db8b096322"
company_key: "draftkings-inc-class-a-common-stock"
company: "DraftKings Inc."
source_id: "draftkings-inc-class-a-common-stock-rss-016c40719db2"
canonical_url: "https://medium.com/draftkings-engineering/event-loop-starvation-in-nodejs-a19901e26b41"
published_at: "2025-05-29T07:52:37+00:00"
first_seen_at: "2026-07-20T04:35:13.112015+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:2af313f4bd829752b3abc8f88b9281c0357e374b4d579e8cc644ee85cc0fad25"
---

# Event Loop Starvation in NodeJS

# Event Loop Starvation in NodeJS


[Connor Stevens](https://medium.com/@c.stevens?source=post_page---byline--a19901e26b41---------------------------------------)


9 min read


·


May 29, 2025


--


This article details the concept of event loop starvation in NodeJS, how it manifests in application metrics, and how it can be mitigated in both the short and long term.


## Knowledge Prerequisites


This article assumes a baseline familiarity with NodeJS and how its event loop functions to enable non blocking I/O.


If a brush-up is needed before continuing, please reference the[NodeJS documentation](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick) .


## What is Event Loop Starvation?


Event loop starvation is the phenomenon that occurs when running tasks prevent the event loop from being able to efficiently process callbacks, I/O requests/responses, etc.


There are many things that can cause event loop starvation.


Examples include things such as:


- A long running task hogging the Javascript processing thread
- A large number of smaller tasks flooding the callback queue.


In either case, the effect is a delay in the time it takes for things to process. This results in the application becoming “unresponsive”, leading to increases in latency. It can also cause failures if the latency increase causes callers to time out waiting on a response.


## How Will My Application be Impacted?


To see the effects that event loop starvation can have, we can run some tests against a sandbox application. We will analyze application metrics in the same way an on-call engineer would while diagnosing a production issue.


### Application Overview


For all examples in this article, we used the following setup within one of our lower environment Kubernetes clusters:


Press enter or click to view image in full size


- An API gateway service that forwards requests to the web application
- A simple server side rendered (SSR) web application with three routes
- An index page with no data dependency
- An overview page with one data dependency
- A details page with two endpoint data dependencies
- An API service that sends data to be used on the pages
- All data is served statically to ensure the API stays performant and does not bottleneck tests


In all tests, we send around 100 requests per second to the three routes mentioned above. In the middle of the test, we simulate a 90 second traffic spike. During this spike, traffic on the details page increases to 500 requests per second.


### First Symptoms


In most cases, the most glaring symptom of event loop starvation will be an increase in latency for both inbound requests to and outbound requests from the application.


In the below image, a traffic spike has caused a rise in both inbound/outbound request latency on our SSR application (currently scaled out to 4 replicas).


Press enter or click to view image in full size


An intuitive reaction would be to assume latency increase is a result of an issue with a downstream service. This is because the rise in inbound request latency seems to match the rise in outbound request latency. In addition, the most impacted routes are ones with downstream data dependencies.


However, when we take a look at the metrics for the API service, we see that no latency increase was observed at all in its request processing.


Press enter or click to view image in full size


Distributed tracing tells a similar story, with the time spent from the SSR application’s perspective (397 milliseconds) being much longer than the API service’s (698 microseconds).


Press enter or click to view image in full size


To get into why the difference is as large as it is, let’s look at when the timer starts/stops from the SSR application and API service’s perspective.


Press enter or click to view image in full size


The SSR application’s request timer only stops when the response processing code is invoked. If the event loop is blocked or the callback queue is deep from high traffic, response processing will be delayed. This creates the perceived latency increase from the SSR application’s perspective that is seen in metrics.


## Can the Event Loop be Visualized?


Yes it can! Through some NodeJS specific application metrics, valuable insight is gained into how the event loop is functioning.


In this scenario, the following metrics are available to look at:


- Event Loop Utilization — The percentage of time in a given interval (100ms here) that the event loop was actively processing events.
- Event Loop Delay — The observed delay between when a tracking callback was registered and when it actually executed.
- Event Loop Iterations Per Second — The average number of event loop iterations recorded in a given second.
- Event Loop Delay Per Second — The average amount of time in a given second that the event loop was actively processing events.


Taking a look at the event loop metrics, a few key observations can be made.


In the below image, we roll up metrics by pod instead of by route to more clearly show correlation between metrics.


Press enter or click to view image in full size


### Event Loop Delay is the Primary Driver of Latency


This picture makes it very clear where the cause of our latency is coming from. As event loop delay goes up, application latency rises almost directly with it.


This is sensible, with the added time in serving a request being the time the response’s processing was left waiting on the callback queue.


### Event Loop Delay is Worst at Max Event Loop Utilization


Early on in the test, the event loop utilization numbers ran pretty hot without any notable delays being observed. Problems only begun to appear once event loop utilization reached or neared 100%.


### Per-Second Metrics Expectedly Follow Observed Metrics


Knowing how the per second metrics are calculated in comparison to the observed metrics, the results in this graph turn out as expected. As event loop utilization reaches 100%, the delay per second climbs right with it towards 1.


In addition, with the decrease in the loop’s idle time new events cannot be processed, leading to a decrease in the number of iterations performed.


## Other Application Metrics


In order to gain a complete picture of the situation, additional system metrics around application resource usage can also be observed. Specific metrics in this image include


- Pod CPU Usage (raw value)
- Pod CPU Usage (% of requested)
- Pod Memory Usage (raw value)
- V8 Heap Space Usage
- Garbage Collection Pause Counts
- How many times v8 paused execution to garbage collect
- Garbage Collection Pause Time
- How long v8 paused execution to garbage collect


Press enter or click to view image in full size


CPU metrics show that usage did rise a bit as the application experienced latency increases, but not by much if at all more than than the increase in traffic to the application at those times.


Memory metrics show a similar pattern. All numbers closely correlate with request traffic. These is sensible, since higher traffic volume increases memory usage. In turn, this also causes a higher need for garbage collection..


We also see that rises in pause time numbers do not directly correspond to an increase in latency, making it unlikely that GC pause time is the key driver of starvation, even if it may play a small part.


## Mitigating Event Loop Starvation


There are short and long term measures that can be taken to stop the bleeding during an incident and help optimize your code to prevent future occurrences.


### Short Term — Add More Event Loops (Increase Application Replicas)


True multithreaded languages like C# can use multiple CPU cores to handle more computational work. In contrast, JavaScript servers have a strict upper limit. This limit comes from the language’s single-threaded nature.


Since we only have one thread to work with, vertically scaling application replicas to have more CPU cores has no value. However, we can horizontally scale the number of replicas to increase the number of event loops available to handle requests.


Taking the same traffic pattern from the earlier examples, we can evaluate how the application performs by adding just one more replica to the mix:


Press enter or click to view image in full size


In this case, we see a significant decrease in both inbound and outbound latency, as well as an increase in peak throughput that the application is able to produce.


Looking into the event loop metrics themselves we see the same story, with some delays being observed after scale out, but much less intense than before. In this case we see a much healthier event loop, running hot at times but never reaching the point of 100% utilization and starvation.


Press enter or click to view image in full size


As for the other metrics on the site, we also see the expected story, with the lower state of “business” on each pod resulting in a slight decrease in the CPU, Memory, and Garbage Collection Pause numbers


Press enter or click to view image in full size


### Long Term — Improving Application Performance


Event loop starvation often occurs when your application is under-provisioned for its current level of traffic. Beyond just scaling out, you can also mitigate risk by optimizing application code. This allows the application to handle more user traffic per replica.


A CPU profiler can be a helpful tool to find areas of improvement in your application’s code.


Press enter or click to view image in full size


In this CPU profile, nothing really jumps off the page as a major bottleneck. The largest blocks recorded are pretty much the ones we would expect.


Going from left to right, the most prominent blocks we see are:


- The one stemming down from @remix-run/express
- This is the layer where data is fetched from the API to use on the page
- The one stemming down from react-dom
- This is the code that performs the actual server render via React’s server APIs
- The one stemming down from send
- This is our express entry point and middleware flow


Based on these observations and the visible impact from scaling out, this particular instance of starvation looks to be a result of under-provisioning. Without a clear bottleneck, it’s likely the callback queue was just overloaded by the sheer number of events.


To show how a piece of event loop blocking code would expose itself in a profile, a long running, synchronous piece of code can be added to a flow that is called on each request. In this example, one of the react components on the details page was altered to sum the numbers from 1–1,000,000 on each invocation.


```text
const BeverageDetails = () => {    let sum = 0;    for (let i = 0; i < 1000000; i++) {      sum += i;    }    // …rest of component code  }
```


Looking at this CPU profile, it is very clear that the component is an issue (the longest blue bar under react-dom), with it now taking up around 8% of the total profiling time.


Press enter or click to view image in full size


For deeper guidance on how to not block the event loop,[this document](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop) from the NodeJS team is a great resource.


## Key Takeaways


### What is Event Loop Starvation?


- It is a NodeJS Performance issue that occurs when the event loop cannot efficiently process tasks
- It occurs when the Javascript thread is blocked by long running operations or when the callback queue is flooded by large volumes of events


How Can Event Loop Starvation Be Identified?


- Event loop starvation most clearly shows itself through an increase in application latency metrics.
- Monitoring event loop usage and delays in callback processing is vital in quickly diagnosing event loop starvation and removing the risk of red herrings.


### How Can Event Loop Starvation Be Mitigated?


- The most immediate mitigation is to scale out the number of application replicas to a number that does not introduce load risk for downstream dependencies.
- CPU profilers can be valuable tools in optimizing application performance to protect against event loop starvation in the long term.


## Helpful Resources


- [The Node.js Event Loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
- [Don’t Block the Event Loop (or the Worker Pool)](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop)
