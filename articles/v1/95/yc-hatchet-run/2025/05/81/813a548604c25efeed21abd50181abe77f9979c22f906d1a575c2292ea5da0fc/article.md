---
schema_version: "1.0.0"
document_id: "813a548604c25efeed21abd50181abe77f9979c22f906d1a575c2292ea5da0fc"
company_key: "yc-hatchet-run"
company: "Hatchet"
source_id: "yc-hatchet-run-rss-3f16f06bd764"
canonical_url: "https://hatchet.run/blog/warning-event-loop-blocked"
published_at: "2025-05-27T00:00:00+00:00"
first_seen_at: "2026-08-09T22:57:06.374584+00:00"
fetched_at: "2026-08-09T22:57:07.291413+00:00"
content_hash: "sha256:844bedd8e3de79361ce8f8282b28420743bc4c0ea5760ac000261015df375d58"
---

# Warning! The Event Loop May Be Blocked

Matt Kaye


Engineer


Hatchet


*Since you’re here, you might be interested in checking out[Hatchet](https://hatchet.run/) —the platform for running background tasks, data pipelines and AI agents at scale.*


Blocked event loops are, by far, the most common problem we see when providing support to Hatchet users. If you use Hatchet, and Hatchet’s Python SDK in particular, you might’ve seen a warning like this:


> *Warning: THE TIME TO START THE STEP RUN IS TOO LONG, THE MAIN THREAD MAY BE BLOCKED*


Scary! Let’s talk through what’s going on under the hood, and some possible causes for this warning in Hatchet and how to effectively debug.


> *Note: New to` async` /` await` and event loops in Python? I’d recommend checking out[FastAPI’s async documentation](https://fastapi.tiangolo.com/async/) quickly before getting started here. Hatchet handles synchronous and asynchronous work very similarly to FastAPI.*


### Blocking I/O


First and foremost, in the vast majority of cases, this scary warning from Hatchet is being caused by the event loop being blocked. And if the event loop is blocked, there’s a very good chance that some code it is trying to run (read: a Hatchet task) is doing some blocking work. The[asyncio documentation](https://docs.python.org/3/library/asyncio-dev.html#running-blocking-code) puts their recommendation for how to handle blocking functions correctly very eloquently, in one sentence that gets right to the crux of the issue:


> Blocking (CPU-bound) code should not be called directly.


> *Note: CPU-bound work in the simplest terms is work that spends most of its time doing actual computation as opposed to e.g. waiting for some external process (like an API call) to complete. Importantly, using e.g.` requests.get` to make an API call also (confusingly) falls under this definition of “CPU-bound” even though it’s also just waiting, since while it waits the program cannot context switch to some other work running in the event loop (because` requests` is not async).*


##### A simple example of a blocked loop


Let’s give a simple example, which we’ll come back to later as a helpful debugging strategy. We’ll first write two functions:


Loading syntax highlighting...


And let’s run these concurrently with` asyncio.gather` and` asyncio.create_task` :


Loading syntax highlighting...


If you run this code, you’ll see logs like this:


Loading syntax highlighting...


On the other hand, you can run two tasks running the non-blocking function concurrently as you’d expect:


Loading syntax highlighting...


Which results in the logs below. Note that the output from the two tasks,` A` and` B` , are interleaved, indicating that they’re correctly running concurrently.


Loading syntax highlighting...


If you were to run code like this in a Hatchet task, you’d see the scary warning from above.


### Understanding the Problem


The long and short of the problem here, as so nicely put by the` asyncio` documentation, is that if some async code is doing *anything* blocking, then *everything* else will need to wait for that blocking operation to complete. This means that if you have a Hatchet worker running 1,000 tasks concurrently and *one* of them does something blocking, none of your other tasks will run while that blocking operation is happening.


Some common (and some less common) examples of blocking operations might include:


- Making a synchronous API call using` requests.get`
- Performing a synchronous database operation using` psycopg` , such as running an expensive` SELECT` statement that takes a long time to complete
- Running a CPU-bound algorithm, such as[solving a Sudoku puzzle](https://en.wikipedia.org/wiki/NP_(complexity))


In each of these cases, while this work is happening, no other async work on your Hatchet workers will be able to progress. We see some interesting and scary behavior if we run some blocking code in Hatchet.


Loading syntax highlighting...


Here we define a few tasks, one which is async and does blocking work (` time.sleep` ), one which is sync and does blocking work (` time.sleep` ), and one that is async and does non-blocking work (` asyncio.sleep` ).


As an experiment, we can run them as follows to simulate what might happen in a production environment:


Loading syntax highlighting...


The intention of this example is to first kick off the non-blocking sync and async tasks, let them start to process, then kick off the blocking task, let it start to process, and finally kick off the non-blocking sync task again, and then let all of them complete. The worker logs are illustrative:


Loading syntax highlighting...


Here’s a play-by-play of what happened:


1. The non-blocking sync and async work starts, and their logs are interleaved (as you’d expect, since Hatchet runs tasks concurrently).
2. We see this internal event:` run: start step: blocking:blocking` , indicating that the worker has now started running the blocking task.
3. After that log, we stop seeing any` Non blocking async` logs, as the event loop is blocked. Notice that at this point, we continue to see` Non blocking sync` logs. This is an important design decision in Hatchet. Hatchet runs synchronous tasks in a thread pool so they can be executed in a non-blocking way, which means that once a sync task has started, it can continue executing even if the main event loop is blocked.
4. We receive a` start step run` event, which indicates the last run has been triggered:` rx: start step run: 7742df98-169f-4afa-9075-e43c8b3ea8df/non_blocking_sync:non_blocking_sync` . Importantly, you might expect that since this task is sync, it will be executed correctly without being blocked, similarly to how the previous one was in 3). This is not the case! Since the event loop is blocked, Hatchet *cannot begin to execute this task run* , which is why we immediately start seeing the scary warning log.
5. ` THE TIME TO START THE STEP RUN IS TOO LONG, THE MAIN THREAD MAY BE BLOCKED: Waiting Steps 1`
6. We get a finished event for the blocking step:` finished step run: blocking:blocking`
7. The scary warning goes away, and we immediately go back to business as usual, seeing the next` Non blocking async 2` log. Importantly, this index (2) was where it left off before, but it “slept” for about six seconds (the duration of the blocking task), as opposed to the one second that we intended, between log lines.
8. All of the remaining work completes, including the new` Non blocking sync` task starting and finishing.


### Debugging


So you’re seeing the scary warning: Now what?


#### Turn on asyncio ’s DEBUG mode


` asyncio` has a[debug mode](https://docs.python.org/3/library/asyncio-dev.html#debug-mode) , which will give you more observability into the async operations that your worker is doing.


This will log warnings about slow callbacks and provide additional information about tasks that are taking too long.


#### Look for obviously blocking code


First line of defense: look for things that are obviously blocking. API calls, database operations, for loops doing something involved or running many iterations, and so on. Depending on what the problem is, there are different ways to handle different situations:


- If you’re making API calls with` requests` or similar, try using` aiohttp` instead to make the calls async.
- If you’re using` psycopg2` or similar synchronous database libraries for database I/O, try using` asyncpg` or` psycopg\[binary\]` with` asyncio` support instead, to make database operations async.
- If you’re relying on an external library that does not provide async methods, try wrapping the methods in` asyncio.to_thread` to run them in a separate thread so they don’t block the main event loop. For example:` await asyncio.to_thread(some_blocking_function, arg1, arg2)` .
- Similarly, if you have some expensive CPU-bound work (see: solving Sudoku), use` asyncio.to_thread` there too to offload the work to a separate thread.


As a last resort, you can also change your tasks from being async to sync, although we don’t recommend this in the majority of cases.


#### Use a linter


[Ruff](https://docs.astral.sh/ruff/) , via` flake8` (for example), has an[ASYNC linting rule](https://docs.astral.sh/ruff/rules/#flake8-async-async) to help you catch potential issues in async code.


#### Instrument your code


If you’ve resolved all of the obvious issues but the Scary Warning ™ ️


is still popping up, instrumenting your code can help find the bottleneck. Hatchet’s Python SDK provides an OpenTelemetry Instrumentor, which allows you to easily export traces and spans from your Hatchet workers. If you have some long-running tasks (or long start times), you can use the traces to get a better sense for what might be blocking. In particular, if there are some async operations that appear to just be hanging for significantly longer durations than they should take, this is a good indication they’re being blocked by something.


Similarly, you can also instrument your code with the[AsyncioInstrumentor](https://opentelemetry-python-contrib.readthedocs.io/en/latest/instrumentation/asyncio/asyncio.html) and other, similar instrumentors depending on other tools in your stack.


#### Run your code separately from Hatchet


As a last resort, another thing to try is running your code in a fashion similar to how we did above, outside of Hatchet, by creating async tasks and using` gather` to run them concurrently. If there’s blocking behavior, it’ll be apparent when one of the tasks is blocked.


### Takeaways


Blocked event loops can significantly impact the performance of your Hatchet workers, causing tasks to wait unnecessarily and triggering those scary warning messages. We added the scary warning to the SDK to help flag that something might be blocking the loop. Note that it’s not *always* an indication that the event loop is blocked, but it’s a hint that something might be wrong.


By following the debugging steps outlined in this post, you should be able to:


1. Identify blocking code in your async functions
2. Replace synchronous operations with asynchronous alternatives
3. Offload CPU-bound work to separate threads using` asyncio.to_thread`
4. Use instrumentation to triangulate performance bottlenecks


To reiterate the main point from the start of the post, taken directly from the` asyncio` documentation:


> Blocking (CPU-bound) code should not be called directly.
