---
schema_version: "1.0.0"
document_id: "53431793b85e334ef1e2320cf2cf24d25ea268e7cf105f14f5ba4445a2d539b3"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/making-fetch-happen-building-a-general-purpose-query-and-render-scheduler/"
published_at: "2023-04-17T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:c65a079cfb631056166743c3ea9c3cbbe8b82f3f7ae9aaf1a701f6a10b2647ea"
---

# Making fetch happen: Building a general-purpose query and render scheduler

Cormac Flynn


Users expect web applications to be fast and responsive, with smooth scrolling and almost instantaneous rendering. Combining complex UI interactions with frequent data fetching, as many Datadog products do, makes optimizing for good runtime performance a challenge.[Dashboards](https://docs.datadoghq.com/dashboards/) in particular is difficult as, unlike other products, users have complete control over the size of their boards and the complexity of their queries.


This post describes how we developed a new query and render scheduler, conceived initially to optimize Dashboard performance but then generalized for use on any expensive task or fetch-heavy application.


## Dashboard scheduling v1


Dashboards had an existing scheduling system used to manage when fetches and renders occurred, composed of the periodic updater and the query and render task queues.


The **periodic updater** controls how frequently a widget requests fresh data. This cadence is determined by a number of factors - the time frame selected, whether the browser is focused or not, etc.


When a fetch is requested, the scheduler enqueues a new **query task** that defers when the fetch actually happens. Tasks for visible widgets run immediately; those for offscreen widgets are delayed based on a set of heuristics (number of pending queries, time spent on previous fetches, etc.). The main goal of the query scheduler is to produce a flatter distribution of queries in time, in order to improve browser performance and, across all dashboards, produce a more stable load on our backend services.


When data is returned, a **render task** is created. Again, renders for visible widgets happen immediately and those for non-visible widgets are delayed according to a new set of rules. The role of the render scheduler is to reduce the load on the browser’s main thread.


Fig 1 - A simplified view of the legacy query/render scheduler. Fig 1 - A simplified view of the legacy query/render scheduler.


We found that this legacy scheduler performed well compared to an unscheduled baseline, minimizing work for the browser by favoring visible widgets and more efficiently distributing fetches within a given time frame.


Fig 2 - The distribution of unscheduled query tasks over a 60s time period on a dashboard with 100 widgets. Each bar represents all visible (blue) and non-visible (red) query tasks performed within a 1000ms bucket. Fig 2 - The distribution of unscheduled query tasks over a 60s time period on a dashboard with 100 widgets. Each bar represents all visible (blue) and non-visible (red) query tasks performed within a 1000ms bucket.


Fig 3 - With the legacy scheduler enabled, we have a flatter distribution with shorter spikes & slightly fewer overall requests. This results in better browser performance, as multiple widgets are not rendering at the same time and, across all dashboards, should produce a more predictable load on our backend services. Fig 3 - With the legacy scheduler enabled, we have a flatter distribution with shorter spikes & slightly fewer overall requests. This results in better browser performance, as multiple widgets are not rendering at the same time and, across all dashboards, should produce a more predictable load on our backend services.


Despite this, there remained some significant issues with this algorithm.


Though initially conceived as a simple set of rules,the scheduler had grown to become a web of interlinked heuristics, with around 20 different parameters controlling its behavior. This produced a complex and dynamic system that made it difficult for developers to reason about how a dashboard updated and rendered its content.


These rules did not clearly separate query from render scheduling. For example, queries were given an extra delay if too many render tasks were pending, a consideration with no obvious link to the goal of throttling fetches. Similarly, renders were delayed if the fetched data was deemed sufficiently large, even when the browser had the resources to perform it. This meant that, although better than baseline, the scheduler was not working as efficiently as it might, particularly with respect to rendering.


Finally, the original scheduler had been written solely for dashboards, and was tightly bound to how a board fetched data and rendered widgets. Since then, our widgets have become more generalized components used across our product suite, with a standard framework for querying and renders.


## Building a general-purpose scheduler


We felt it was time to revisit the scheduler, not only to clean up its rules and simplify its logic but to make its benefits more generally available to applications across Datadog, on any page in which a large number of widgets are used.


Changing such a critical code path was risky, with potential impacts on both the user experience and our backend services. So we had to proceed methodically - first by evaluating the utility of the existing scheduler’s rules, then by separating query and render scheduling into discrete modules that could be tested and rolled out separately.


In the following sections, we describe this process. Firstly, how we arrived at a new algorithm for query scheduling, one that works independently of how data is rendered and could be used across Datadog. Next we outline how we implemented a general-purpose render scheduler using the new Browser Scheduling API, allowing us to schedule any sufficiently costly task.


## A Simpler Query Scheduler


We began by testing the original scheduler across dashboards of various sizes and under different conditions, to establish the true utility of its heuristics. We found that many could be discarded with no impact on performance. For example, tasks were delayed when a tab was unfocused or occluded by another window. The periodic updater already throttles updates in these cases, and the browser itself pauses JavaScript in unfocused tabs.


Next, we experimented with various scheduling algorithms, testing in the same manner. Our aim was to keep the task distribution sparse with fewer peaks, while still favoring fetches for visible widgets.


We eventually arrived at a simple algorithm that met these goals:


1.


Fetches for visible widgets are performed as soon as requested;


2.


Non-visible queries are ranked & as many as possible run per fixed time window, up to a set task limit;


3.


Task execution is paused if the number of pending fetches is sufficiently elevated.


Variations we tested differed in how the time window was calculated (e.g. scaled based on the time frame) or how offscreen tasks were ranked (e.g. favoring those close to the viewport), but we found that results between these did not differ greatly and thus chose the simplest option: a fixed time window of 2000ms, a 10 task limit, and offscreen tasks ranked by enqueue time (favoring earlier & first requests).


In contrast to the old scheduler, this new algorithm was governed by only 6 parameters (the time window duration, task limit, widget visibility etc.)


Fig 4 - A contrived example of the chosen algorithm with, in this case, a limit of 3 tasks per fixed time window. All widgets first fetch in the order they appear in the page. So, for window 1, we ignore the task limit and satisfy all visible queries (randomly, to avoid in-order rendering which can look odd). Non-visible tasks fetch according to their ranking. As time progresses, widgets naturally begin fetching in an order different from the document order. In each window, we perform all visible queries first and, if any slots remain, non-visible fetches up to the task limit. Fig 4 - A contrived example of the chosen algorithm with, in this case, a limit of 3 tasks per fixed time window. All widgets first fetch in the order they appear in the page. So, for window 1, we ignore the task limit and satisfy all visible queries (randomly, to avoid in-order rendering which can look odd). Non-visible tasks fetch according to their ranking. As time progresses, widgets naturally begin fetching in an order different from the document order. In each window, we perform all visible queries first and, if any slots remain, non-visible fetches up to the task limit.


These changes were rolled out progressively, starting with dashboards, then deployed more widely by integrating the scheduler into the data-fetching framework used by widgets across Datadog.


We found that the scheduler performed well, with the simpler logic producing a better distribution of tasks compared to the old algorithm.


Fig 5 - The legacy scheduler query task distribution for a 60s period on a dashboard with 100 widgets. Each bar represents all visible (blue) and non-visible (red) query tasks performed within a 1000ms bucket. Fig 5 - The legacy scheduler query task distribution for a 60s period on a dashboard with 100 widgets. Each bar represents all visible (blue) and non-visible (red) query tasks performed within a 1000ms bucket.


Fig 6 - The same using the new scheduler, which spreads tasks more sparsely with fewer peaks and much fewer offscreen fetches over the same time period. Fig 6 - The same using the new scheduler, which spreads tasks more sparsely with fewer peaks and much fewer offscreen fetches over the same time period.


In addition, the number of “429 (Too many requests per account)” errors dropped significantly, due in part to the more efficient task distribution. A 429 response will cause the query to be retried at a later time, so fewer errors of this nature mean data is fetched more quickly.


Fig 7 - The dashboard 429 rate drops following full rollout of the new query scheduler. Fig 7 - The dashboard 429 rate drops following full rollout of the new query scheduler.


## Render Scheduling with the Browser Scheduling API


Although the original scheduler helped distribute render tasks in time, it was ignorant of the CPU and memory resources available to the browser, meaning that expensive renders could still be inefficiently scheduled.


The[Browser Scheduling API](https://github.com/WICG/scheduling-apis) is a new API for creating prioritized tasks that are natively scheduled by the browser. This means that the most relevant work can be given a high priority and performed when the browser has sufficient resources, with less important work relegated to a background task, run when the browser is idle.


The API is currently implemented in Chromium and[Firefox Nightly](https://blog.mozilla.org/performance/2022/06/02/prioritized-task-scheduling-api-is-prototyped-in-nightly/) , with[a polyfill](https://github.com/GoogleChromeLabs/scheduler-polyfill) for other browsers.


```text
1   // Create a TaskController with one of the preset priorities (user-blocking, user-visible or background)    2   const     controller   =   new     window  .  TaskController  ({   priority:     'user-visible'   });    3
4   const     signal   =   controller  .  signal  ;    5   console  .  log  (  signal  .  priority  );   // Logs 'user-visible'    6   console  .  log  (  signal  .  aborted  );    // Logs false    7
8   // Schedule a task, passing it the signal i.e. the task inherits the controller's priority    9   window  .  scheduler  .  postTask  (()   =>     doSomething  (), {    10       signal:     controller  .  signal    11   });    12
13   // Later we can update the priority for all the controller's tasks en masse ...    14   controller  .  setPriority  (  newPriority  );    15
16   // ... or abort every task registered with the controller    17   controller  .  abort  ();
```


This is sample code using the Scheduler API. Tasks are created with either “user-blocking”, “user-visible”, or “background” priority. The controller allows tasks to be updated and aborted en masse.


We used this API to implement a new render scheduling algorithm:


1.


Renders for visible widgets are scheduled with the second-highest priority (“user-visible”), meaning tasks are run when the browser has sufficient resources;


2.


Renders for widgets outside the viewport are given the lowest priority (“background”), meaning they are performed once the browser is idle;


3.


All renders are scheduled, whether they result from a query response or a user interaction (unlike the legacy scheduler, which optimized only renders resulting from a data fetch).


We again integrated this scheduler into the framework used by all widgets, meaning that the benefits of render scheduling were felt across Datadog. Moreover, we realized that the ability to efficiently schedule expensive tasks could be generally useful, so a helper component was built to allow its wider adoption.


```text
1   // A function with an expensive async task    2   async     function     myFn  () {    3       const     result   =   await     sendTelemetryMetrics  ();    4       // ....    5   }    6
7   // The same function with the task scheduled to run in the background i.e. the browser will only execute the    8   // function when time is free on the main thread    9   async     function     myScheduledFn  () {    10       const     result   =   await     scheduler  .  postTask  (()   =>     sendTelemetryMetrics  (), {    11         priority:     'background'    12   })    13   // ...    14   }
```


This is an example of generalized use of the scheduler API. As postTask returns a Promise, any asynchronous task can be scheduled with only a small change to the code. This kind-of usage was encapsulated in a helper component.


Once deployed, we found that the scheduler significantly reduced the number and duration of widget renders. This improvement was measured wherever widgets were used - not only on dashboards.


Fig 8 - Once the render scheduler was fully enabled, the number and duration of widget renders across Datadog is significantly reduced. Fig 8 - Once the render scheduler was fully enabled, the number and duration of widget renders across Datadog is significantly reduced.


Fig 9 - The dotted line represents the aggregate widget render duration for the previous month. Fig 9 - The dotted line represents the aggregate widget render duration for the previous month.


Its effects were also evident in performance-sensitive interactions, such as widget drag-and-drop, where we saw a greatly reduced number of “long tasks” (i.e. tasks that occupy the browser’s main thread for a period greater than 50ms).


Fig 10 - A performance profile taken during widget drag-and-drop using the old scheduler. Note the red bars at the top, representing long tasks. Fig 10 - A performance profile taken during widget drag-and-drop using the old scheduler. Note the red bars at the top, representing long tasks.


Fig 11 - The same test as above performed with the new render scheduler. The number of long tasks is greatly decreased. Fig 11 - The same test as above performed with the new render scheduler. The number of long tasks is greatly decreased.


Faster widget renders and fewer long tasks mean fewer occasions when the browser is overloaded, producing a faster and more responsive UI, which we saw both in our performance metrics and experienced in our day-to-day use of Datadog.


## What’s next?


We continue to track the performance of the query scheduler in production and, with this data, intend to continue tuning its parameters to further reduce the burden of offscreen fetches without impacting on perceived performance.


The tools that came from the render scheduling work are being increasingly used across Datadog, wherever a particularly expensive task can be delayed until a later time.


-
-
-
