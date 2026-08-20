---
schema_version: "1.0.0"
document_id: "1b566c92022a38fc5a4a391e365bd9f58e8a37f7e11ca233164b4d10c4f8cd8a"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/load-testing-workload-models"
published_at: "2023-02-24T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:5611ad1f7ffbfeb59e2d0be59f378bf03fe4b34a7e9921150e70aea2df10caa0"
---

# Understanding workload models

February 24th, 2023[How to](https://www.artillery.io/blog/tag/howto)


# Understanding workload models


Hassy Veldstra


You may have come across the concept of open and closed workload models, and the associated problem of coordinated omission in load testing. In this blog post we’ll look at Artillery’s workload model and whether it suffers from coordinated omission ( **tldr: it doesn’t** ), and (hot-take incoming) why most of the time neither closed nor open models are that useful for modeling real world systems.


## A quick refresher on closed and open models


In a **closed system** , the system receives a new item of work only after it finishes processing the previous one. An example of a closed workload is a worker process processing messages from a queue, one at a time, pulling in a new message after finishing processing of the previous one.


In an **open system** , new items of work arrive regardless of whether the system is already processing something else. An example of an open system is a web server serving static files. Users will send requests to the server regardless of how many other requests (from other users) the server is already processing.


## Uh oh, coordinated omission


**Coordinated omission** (CO) is a measurement issue that can happen when an open system is tested with a closed workload generator. This is how many old-school load testing tools like` ab` and` wrk` can end up generating very misleading performance measurements:


1. They open a fixed number of TCP connections are opened by the load generator
2. They send HTTP requests sequentially on each of those connections


Imagine we open 10 connections and send 100 requests on each to our web server. Now also imagine that all of those requests were served in 1ms, other than 10 requests that were sent during a 5 second pause when the server was unavailable.


We’re going to have 90 requests that took 2ms, and 10 requests that took somewhere between 5 seconds and 1ms. Our load generator will happily report a p99 latency of 1ms. Wow! Impressive, and also absolutely useless:


- A huge problem - a random 5 second stall - is hidden from us
- In the real world, requests would continue to arrive to our server, because a web server on the public internet is an open system, and all of the requests that were sent during the 5s stall window would take up to 5 seconds to be served. We’d probably see a completely different and much worse p90 value for requests in the real world.


**All closed workload generators will exhibit CO when testing open systems.**


How do you know if your load test generates a closed workload? Any time you have a fixed number of connections, “threads” or “iterations”, you will run into CO, i.e. your load test results **will lie to you** . As long as you watch out for that, you should be OK.


## Is open load generation the answer?


Yes, and also no.


An open workload generator will not suffer from CO. An example of open-loop load generation is sending requests at a constant rate, regardless of whether previous requests completed.[Gil Tene](https://www.youtube.com/watch?v=lJ8ydIuPFeU) pioneered that approach in[wrk2](https://github.com/giltene/wrk2) and it has since been adopted by other tools, such as Vegeta, and autocannon.


But there’s a problem you quickly run into with fully open load generators. **The problem is that the type of systems you can test with those is extremely narrow.**


The only way to send requests at constant RPS is if each requests is completely independent of other requests, e.g. idempotent requests, fire-and-forget requests or simple input-output requests. This leaves us with being able to test… web servers serving static files, simple input-output APIs, and not much else. Those types of systems are a common enough, sure, but most real-world systems are **transactional** , i.e. a user does` thing A` , has to wait for a response, then does` thing B` , and then depending on the results of` thing B` they may do` thing C` or` thing D` .


Take visiting GitHub as an example:


- your browser loads the homepage
- then navigates to one of the repos in the sidebar
- then goes to the Issues tab from the repo page


Constant RPS makes no sense in this scenario, as there’s implicit **back-pressure from the server** (local to each user) because requests are dependent on each other. It’s impossible to try to impose a constant rate of requests in a scenario like this.


So, what do you do to test a system like this?


## Hybrid model is the way to go


Well, you end up with something like this:


1. New users can arrive at any time, according to some probability distribution (uniform, Poisson, etc) - that’s your open loop
2. Individual users will be subject to back-pressure from the service. They will have to wait for a request to complete before sending another one. This gives us a number of closed loops, one per user, within the open arrival loop.


This is a hybrid model used by Artillery, which also maps exactly onto how most systems would be used in the real world.


### Hybrid model is safe from CO


Does this mean that latency outliers due to a temporary server stall will get hidden? No - because whilst user A is waiting for a response, a number of other users will arrive, send their initial requests, and record outsized response times.


Artillery outputs latency metrics at a configurable interval (10s by default) rather than a single aggregate report at the very end, so stalls like that are visible immediately and don’t get smoothed over by smaller measurements from the rest of the duration of the test run.


## Further reading


For a deeper dive on workload models, “Open Versus Closed: A Cautionary Tale” ([PDF](https://www.usenix.org/legacy/event/nsdi06/tech/full_papers/schroeder/schroeder.pdf) ) is a classic.


“How NOT to Measure Latency” by Gil Tene ([YouTube](https://www.youtube.com/watch?v=lJ8ydIuPFeU) ) is another classic that discusses some common pitfalls encountered in measuring latency and response time behavior.


The original GitHub discussion thread which this blog post is based on can be seen on[https://github.com/artilleryio/artillery/issues/721](https://github.com/artilleryio/artillery/issues/721) .
