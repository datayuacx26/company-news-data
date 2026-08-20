---
schema_version: "1.0.0"
document_id: "2a1954d9779d74855ef41b6505ce584aadeb5cc3464cf9d2801735e814e84461"
company_key: "yc-wasmer"
company: "Wasmer"
source_id: "yc-wasmer-news-import-0c661488008f"
canonical_url: "https://wasmer.io/posts/ported-wasmer-backend-django-to-rust"
published_at: "2026-06-04T18:32:00+00:00"
first_seen_at: "2026-07-22T19:24:02.861490+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:a4baaa3fdd07da2fdb229c52633a4a4ee66b041edfe8b6dda855a07f47da3ccb"
---

# Porting our Django backend to Rust improved the infra usage by 90% · Blog · Wasmer

At Wasmer, we have been happily using Django for 7 years (it was the backend we initially used for storing the package information when[wapm.io](http://wapm.io/) was the only dynamic website Wasmer had). However, in the last year the backend **started showing issues as usage started to grow** .


At its peak, the Django backend required more than 800 GB of memory and 220 CPUs to serve normal production load. Our traffic was not extremely high, so this was a clear signal to look at ways to improve it.


The high load put a big strain on the infrastructure we need to have the site operating, and as well on our small 1-person team that maintains the infrastructure.


At the end, we decided to rewrite our 7-year-old Django backend to Rust. It’s an effort that took one person about 3 months full time (with AI assistance), and involved more team members as the Rust backend started to become more stable.


Here’s some TL;DR of the improvements:


Metric Django Rust Change


Compute CPUs 220 24 -89%


Average CPU utilization 80% 30% -62.5%


RAM 800 GB 64 GB -92%


DB connections Thousands Hundreds 3–5x fewer


Query latency Baseline 5–10x faster 5–10x


Startup time >60s worst case 1 s ~60x faster


p95 API latency 120 ms 30 ms -75%


> We measured these numbers by comparing production usage before and after the migration under similar traffic patterns. The numbers below refer to backend compute capacity, memory allocation, DB connection pressure, startup time, and API latency


## What issues did we have with Django?


1. Even though you can use await in the ORM queries, **Django doesn’t really support async database connections** and cursors ([see issue on Django repo](https://github.com/django/django/pull/18408) ), which slowed down significantly the response times when mixed up with GraphQL
2. Over time, Python’s **gradual typing** was not enough for us:` Any` and partially typed dependencies made type safety hard to enforce.
3. Easy **mismatch between types** on Wasmer Backend and Wasmer Edge (Edge is built in Rust)
4. **Slow startup** times. At its worst, startup times were over a minute


Also, our own practices didn’t help (this issues are not related to Django itself):


- We have **time pressure to deliver** , which usually means that people will do shortcuts for developing (often on typing)… which caused a terrible codebase over time
- The backend was **missing strong building principles** (it was a Frankestein maintained by different people with different programming maturity and different expertises during many years)
- We have **no Python expert in-house** (only me, Syrus, creator of Graphene GraphQL framework for Python… but that was not enough as nowadays I can’t be as present on the code as earlier in time)


These issues made the backend increasingly difficult to maintain in the last year. We tried to solve the problem by adding more money into it, however we hit some hard limits that money can’t solve.


Not everything was bad though, we did some great things in our backend:


- **API was properly abstracted by using GraphQL** (none of our clients needed changes after the rewrite)
- **Django Admin panel** made super easy to inspect and quickly fix things. *Note: We haven’t figured out how to do this properly with Rust. It’s likely we will leverage AI for creating new Dashboards*
- **Celery and task schedulers** worked surprisingly well
- **Fast iteration** times


**How the old backend was architected:**


- Django with different product silos: Users, Registry, Edge, Domains, …
- [Django](https://docs.djangoproject.com/en/6.0/topics/db/queries/) for the ORM
- [Graphene](https://graphene-python.org/) and Graphene-Django (note: I’m the creator of those frameworks, perhaps this is why it was hard for me to let go our Python backend!)
- [Celery](https://docs.celeryq.dev/en/stable/getting-started/introduction.html) as the task scheduler
- Python native libraries for local encryption
- Custom Github API client
- [Django Admin](https://docs.djangoproject.com/en/6.0/ref/contrib/admin/) panel


## Why Rust, Why Now


We decided to use Rust to rewrite our backend for this reasons:


1. Team is already **expert in Rust** , thus we can improve the[bus factor](https://en.wikipedia.org/wiki/Bus_factor) for the backend and have a shared expertise of the backend in our team
2. AI tools helped automate much of the **mechanical migration** work, especially for translating straightforward model and business logic from Python to Rust
3. Good opportunity to have **strong abstraction** and strong typing
4. **Reusability of libraries** (for example, we can reuse easily the package validation mechanisms we already have in the Wasmer Runtime)
5. Incredibly **fast startup** times (important for cold starts)


**How the new Rust backend is architected:**


- A Rust workspace split into focused crates:


- Tech silos / architecture: db, graphql, cache, iam/users, admin, tasks
- Products: Apps, Blog, Mailer


- [SeaORM](https://www.sea-ql.org/SeaORM/) for the ORM
- [Async-GraphQL](https://github.com/async-graphql/async-graphql) for the GraphQL server
- [Apalis](https://github.com/apalis-dev/apalis) for the Task scheduler
- [Fernet](https://docs.rs/fernet/latest/fernet/) for local encryption
- [Octocrab](https://github.com/xampprocky/octocrab) for the Github integration
- [Tailadmin](https://tailadmin.com/) components for the AI-generated admin panel


> Note: to be clear, we could have ported to FastAPI + SQLAlchemy and see similar gains, but we decided a new rewrite with better architectural structure was a safer choice, and also we could take advantage of the Rust expertise of our team.


### Downsides of Rust


Not everything is rainbows and unicorns on Rust land, using Rust for our backend had some tradeoffs:


- Much longer build times (which can hurt iteration speed)
- Unable to run dynamic scripts easily on our backend. Doing` python manage.py shell` was a great way to debug things on production and do temporal fixes. Now we need to be much more diligent with any code change
- Harder SQLite/Postgres dual support. We use SeaORM, and it required some manual handling of types that were available in Postgres but not SQLite (inet types).


### Were the improvements solely because of Rust?


Not entirely. Some of the improvement came from Rust’s lower memory usage, stronger typing, and async execution model. But a large part also came from using the rewrite to simplify the architecture, remove accumulated complexity, and align the backend with the rest of our Rust codebase.


We would not frame this as “Django is bad.” Django served us extremely well for seven years. The rewrite made sense because our team, runtime, edge platform, and package validation logic were already Rust-heavy.


## How we ported the Backend codebase


We approached the port strategically:


1. Do **critical migrations** and improvements on the Python backend **first**
2. Use the **same repo** for both backend and Rust
3. Reuse exactly the **same Database models and logic** in the Python backend and Rust (no mismatch of logic anywhere)
4. **On-par migrations** . We let AI build a verified that checked that the DB in each step of the migrations was exactly the same in Python and Rust
5. Use AI to **build new features in both languages at the same time** (even if Rust backend was not yet deployed to production)
6. When we felt the Rust backend was ready (about 3 months into the process), we replaced the **staging environment** (a copy of production data) and started testing more thoroughly there until we ensure there were no regressions.
7. **Everyone contributed**


Some issues we had along the way:


1. GraphQL in Rust was taking too long to compile because the length of our schema. We made the async GraphQL about 70% faster to compile the big GraphQL schema[https://github.com/async-graphql/async-graphql/pull/1796](https://github.com/async-graphql/async-graphql/pull/1796)
2. We didn’t have a built-in admin panel. We ended vibe-coding an admin panel server-side-rendered with Rust that is fully designed for our use case, and actually ended being more useful than the Django one (and perhaps more beautiful thanks to[tailadmin](https://tailadmin.com/) !)


## The results


We went from using **220 CPUs and 800 GB of ram to just 24 CPUs and 64 GB** . Thus, way less money, less things to maintain.


The number of open DB connections at any point in time have improved quite a bit, from the thousands to hundreds (about ~3-5x reduction).


The good news is that we haven’t even added caching to the Rust backend yet, and query timings are already 5-10 times faster.


Stay tuned for more news on even improved performance on our APIs!
