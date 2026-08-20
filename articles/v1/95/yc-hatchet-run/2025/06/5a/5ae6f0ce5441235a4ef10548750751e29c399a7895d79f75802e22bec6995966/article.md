---
schema_version: "1.0.0"
document_id: "5ae6f0ce5441235a4ef10548750751e29c399a7895d79f75802e22bec6995966"
company_key: "yc-hatchet-run"
company: "Hatchet"
source_id: "yc-hatchet-run-rss-3f16f06bd764"
canonical_url: "https://hatchet.run/blog/fastapi-background-jobs-to-hatchet"
published_at: "2025-06-26T00:00:00+00:00"
first_seen_at: "2026-08-09T22:57:06.374584+00:00"
fetched_at: "2026-08-09T22:57:07.291413+00:00"
content_hash: "sha256:a2be8520315948f48022f40f4128775bae4a79496e3c693711e6e96579afce20"
---

# From FastAPI Background Tasks to Hatchet

Matt Kaye


Senior Software Engineer


Hatchet


[FastAPI](https://fastapi.tiangolo.com/) , Python’s new favorite web framework, offers a lightweight, easy-to-use[background tasks](https://fastapi.tiangolo.com/tutorial/background-tasks/) feature for triggering async tasks that can run later without blocking an incoming request from completing. Of course, this is a killer feature: Eventually every app needs a way to run background tasks.


#### FastAPI Background Tasks 101


Under the hood, FastAPI background tasks just[wrap](https://github.com/fastapi/fastapi/blob/master/fastapi/background.py#L38-L59) the[Starlette implementation](https://github.com/encode/starlette/blob/739ea4928b11d4b4cb2b366ccad11405ef3034c4/starlette/background.py#L18-L42) , which simply[awaits the background task after sending the response to the client](https://github.com/encode/starlette/blob/739ea4928b11d4b4cb2b366ccad11405ef3034c4/starlette/responses.py#L167-L168) .


This is a handy trick. First, we send the response, and then we run the background task in a non-blocking way afterwards. As FastAPI advertises, this lets you do things like sending emails, processing data, etc. that the client does not need to wait for.


#### Getting to Production


As you start getting ready to move your FastAPI application to a production setting, you might notice some issues with how FastAPI handles background tasks. In no particular order, a handful of the most immediate issues are:


1. There’s very little observability here. Since the task is just awaited when the response is sent, it’s difficult to know if the background tasks are actually completing or if they’re being dropped.
2. If the server shuts down, you might have background tasks still waiting to be run. If they don’t complete before the application is terminated, those tasks will be dropped, leading to data loss.
3. There’s no way to handle common task queueing issues like concurrency, retries, and so on.
4. All of this work is being run on your web server, even though it’s done in a non-blocking way. This means that background tasks still will eat up CPU and memory on your server, which can be hard to debug, and can lead to performance issues for clients.


These issues, in addition to others that are likely to come up, are the reason you might migrate your FastAPI background tasks over to a more robust tool like[Hatchet](https://hatchet.run/) when you’re getting ready to ship your app to production.


#### Migrating to Hatchet


Hatchet’s functionality is built to solve exactly these problems, and many more that you’ll face as you continue to scale and overcome new obstacles! For the issues above, Hatchet solves them by:


1. Having a fully featured dashboard showing task statuses, runtimes, etc. and by providing additional tools like[an OpenTelemetry integration](https://docs.hatchet.run/home/opentelemetry) to help you monitor your application.
2. Hatchet will “reassign” tasks that do not run to completion to a new (running) worker, so you don’t need to worry about your worker shutting down and a task being dropped. Workers can safely shut down without data loss.
3. Hatchet offers lots of[concurrency](https://docs.hatchet.run/home/concurrency) ,[rate limiting](https://docs.hatchet.run/home/rate-limits) ,[retrying](https://docs.hatchet.run/home/retry-policies) , and many more features to help you build background tasks that adhere to your business logic.
4. You can scale your Hatchet workers independently of your web server


Porting your tasks from FastAPI background tasks to Hatchet is simple—all you need to do is create Hatchet tasks out of the functions you’re passing to` add_task` . For instance:


Loading syntax highlighting...


Would become:


Loading syntax highlighting...


And that’s it! When you trigger the Hatchet task (in this case, in[“fire and forget”](https://docs.hatchet.run/home/run-no-wait) style), your task will be sent through the Hatchet Engine to your worker, where it will execute, and report its result in the dashboard for you to see. Or if something goes wrong, you can be notified.


#### Feature Comparison


Feature FastAPI Background Tasks Hatchet


Setup complexity ✅


Minimal ✅


Simple


Observability ❌


None ✅


Full dashboard + metrics


Reliability ❌


Tasks can be lost ✅


Guaranteed execution


Retries ❌


Manual implementation ✅


Built-in with policies


Concurrency control ❌


None ✅


Configurable limits


Scaling ❌


Tied to web server ✅


Independent worker scaling


Error handling ❌


Basic ✅


Alerts, configurable retries, on-failure tasks


#### Ready to Migrate?


Check out our[blog post on Hatchet using modern Python](https://hatchet.run/blog/task-queue-modern-python) for a thorough introduction to Hatchet.


You can get up and running in just five minutes on[Hatchet Cloud](https://cloud.hatchet.run/) . And if you’d like to learn more, you can find us:


- On[GitHub](https://github.com/hatchet-dev/hatchet)
- On[Discord](https://hatchet.run/discord)


Or check out[our documentation](https://docs.hatchet.run/) .
