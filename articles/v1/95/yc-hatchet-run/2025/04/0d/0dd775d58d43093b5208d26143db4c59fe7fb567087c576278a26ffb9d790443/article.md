---
schema_version: "1.0.0"
document_id: "0dd775d58d43093b5208d26143db4c59fe7fb567087c576278a26ffb9d790443"
company_key: "yc-hatchet-run"
company: "Hatchet"
source_id: "yc-hatchet-run-rss-3f16f06bd764"
canonical_url: "https://hatchet.run/blog/task-queue-modern-python"
published_at: "2025-04-10T00:00:00+00:00"
first_seen_at: "2026-08-09T22:57:06.374584+00:00"
fetched_at: "2026-08-09T22:57:07.291413+00:00"
content_hash: "sha256:cefc5d660b1d522a21ec7e6aceead6379c4efa34fc88a1f443ffe32de4c97ab0"
---

# A task queue for modern Python applications

Matt Kaye


Engineer


Hatchet


*Since you’re here, you might be interested in checking out[Hatchet](https://hatchet.run/) —the platform for running background tasks, data pipelines and AI agents at scale.*


***Disclosure:** I’m an engineer at[Hatchet](https://hatchet.run/) , a multi-language task queue with Python support. We’re[open-source](https://github.com/hatchet-dev/hatchet) (with a cloud version) and we aim to be a drop-in replacement for Celery that supports a modern Python stack.*


### What is Hatchet?


Hatchet is a platform for running background tasks, similar to Celery and RQ. We’re striving to provide all of the features that you’re familiar with, but built around modern Python features and with improved support for observability, chaining tasks together, and durable execution.


### Modern Python Features


Modern Python applications often make heavy use of (relatively) new features and tooling that have emerged in Python over the past decade or so. Two of the most widespread are:


1. The proliferation of type hints, adoption of type checkers like[Mypy](https://mypy-lang.org/) and[Pyright](https://microsoft.github.io/pyright/#/) , and growth in popularity of tools like[Pydantic](https://docs.pydantic.dev/latest/) and[attrs](https://www.attrs.org/en/stable/) that lean on them.
2. The adoption of` async` /` await` .


These two sets of features have also played a role in the explosion of[FastAPI](https://fastapi.tiangolo.com/) , which has quickly become one of the most, if not *the* most, popular web frameworks in Python.


> *Note: If you aren’t familiar with FastAPI, I’d recommending skimming through the documentation to get a sense of some of its features, and on how heavily it relies on Pydantic and` async` /` await` for building type-safe, performant web applications.*


Hatchet’s Python SDK has drawn inspiration from FastAPI and is similarly a Pydantic- and async-first way of running background tasks.


### Pydantic


When working with Hatchet, you can define inputs and outputs of your tasks as Pydantic models, which the SDK will then serialize and deserialize for you internally. This means that you can write a task like this:


Loading syntax highlighting...


In this example, we’ve defined a single Hatchet task that takes a Pydantic model as input, and returns a Pydantic model as output. This means that if you want to trigger this task from somewhere else in your codebase, you can do something like this:


Loading syntax highlighting...


The different flavors of` .run` methods are type-safe: The input is typed and can be statically type checked, and is also validated by Pydantic at runtime. This means that when triggering tasks, you don’t need to provide a set of untyped positional or keyword arguments, like you might if using Celery.


### Triggering task runs other ways


##### Scheduling


You can also *schedule* a task for the future (similar to Celery’s` eta` or` countdown` features) using the` .schedule` method:


Loading syntax highlighting...


Importantly, Hatchet will not hold scheduled tasks in memory, so it’s perfectly safe to schedule tasks for arbitrarily far in the future.


##### Crons


Finally, Hatchet also has first-class support for cron jobs. You can either create crons dynamically:


Loading syntax highlighting...


Or you can define them declaratively when you create your workflow:


Loading syntax highlighting...


Importantly, first-class support for crons in Hatchet means there’s no need for a tool like[Beat](https://docs.celeryq.dev/en/latest/userguide/periodic-tasks.html#introduction) in Celery for handling scheduling periodic tasks.


### async / await


With Hatchet, all of your tasks can be defined as either sync or async functions, and Hatchet will run sync tasks in a non-blocking way behind the scenes. If you’ve worked in FastAPI, this should feel familiar. Ultimately, this gives developers using Hatchet the full power of` asyncio` in Python with no need for workarounds like increasing a` concurrency` setting on a worker in order to handle more concurrent work.


As a simple example, you can easily run a Hatchet task that makes 10 concurrent API calls using` async` /` await` with` asyncio.gather` and` aiohttp` , as opposed to needing to run each one in a blocking fashion as its own task. For example:


Loading syntax highlighting...


With Hatchet, you can perform all of these requests concurrently, in a single task, as opposed to needing to e.g. enqueue a single task per request. This is more performant on your side (as the client), and also puts less pressure on the backing queue, since it needs to handle an order of magnitude fewer requests in this case.


Support for` async` /` await` also allows you to make other parts of your codebase asynchronous as well, like database operations. In a setting where your app uses a task queue that does not support` async` , but you want to share CRUD operations between your task queue and main application, you’re forced to make all of those operations synchronous. With Hatchet, this is not the case, which allows you to make use of tools like[asyncpg](https://github.com/MagicStack/asyncpg) and similar.


### Potpourri


Hatchet’s Python SDK also has a handful of other features that make working with Hatchet in Python more enjoyable:


1. Lifespans (in beta) are a feature we’ve borrowed from[FastAPI’s feature of the same name](https://fastapi.tiangolo.com/advanced/events/) which allow you to share state like connection pools across all tasks running on a worker.
2. Hatchet’s Python SDK has an OpenTelemetry instrumentor which gives you a window into how your Hatchet workers are performing: How much work they’re executing, how long it’s taking, and so on.


### Thank you!


If you’ve made it this far, try us out! You can get up and running in just five minutes on[Hatchet Cloud](https://cloud.hatchet.run/) . And if you’d like to learn more, you can find us:


- On[GitHub](https://github.com/hatchet-dev/hatchet)
- On[Discord](https://hatchet.run/discord)


Or check out[our documentation](https://docs.hatchet.run/) .
