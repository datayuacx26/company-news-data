---
schema_version: "1.0.0"
document_id: "2f0a7fbe8d0aebb48faf35522fc319c9f45ab1752f83de180e1f47f38d904a99"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-depot-ci"
published_at: "2026-03-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:56:52.525405+00:00"
content_hash: "sha256:57ca6b3efca9065353b680c803ef311febda1f7ce667faf817e8b0c3065a2035"
---

# Now available: Depot CI

For over three years, we’ve been making builds exponentially faster. We started by making container image builds faster, then moved on to accelerating all of CI with our own GitHub Actions runners. Adding additional services to accelerate other areas, like container registries and remote caching.


But there was always a ceiling.


We could optimize the build step itself for container image builds or the actual runner for GitHub Actions. But the entire CI system around it? The control plane, the plumbing to launch runners, the orchestration, the APIs – that was all someone else’s infrastructure and code. We could make about 30% of the system exponentially faster with Depot, but the other 70% was outside our control.


That 70% has been the pain we’ve all felt over the last several months.


So we decided to take over[100% of the CI system](https://depot.dev/products/ci) .


We’ve built our own programmable CI service to power the next step function in build acceleration for engineers and agents. Optimized for performance and, as we hope to demonstrate in the months to come, reliability.


## CI was designed for a different era


Think about what CI actually looks like today. You push code. A webhook fires. A control plane somewhere queues a job. An ephemeral VM spins up – cold, empty, with nothing on it. Your code gets cloned from scratch. Dependencies get downloaded from the internet. Your build runs. The VM gets destroyed. Every single time.


The design made sense in 2015 when the bottleneck was writing code. That era is over.


AI has dramatically shifted the bottleneck from writing code to integrating it. One engineer can command a fleet of agents to operate more like 20 engineers. They’re generating more code, across more branches, at a pace that would have been unimaginable three years ago. And all of that code needs to flow through CI.


Teams can achieve 10x code velocity with agents today, but CI can’t keep up. The slowest part of software development is everything after commit.


Sound familiar? If you'd rather try it than read about it,` depot ci migrate` gets your workflows running on Depot CI in minutes.


Quickstart


## What you can do today with Depot CI


Depot CI is a programmable CI engine designed for how you're actually building software today.


**Bring your existing GitHub Actions workflows.** Copy your workflows from` .github` into a` .depot` folder with a single command, and everything just works. GitHub Actions compatibility from day one.


**Get faster CI, immediately.** Depot CI runs on our own compute system, optimized for performance. Your CI starts in seconds, not minutes.


**Use custom runner images.**[Customize your runner image](https://depot.dev/docs/ci/how-to-guides/custom-images) — or have an agent customize it for you — so your jobs start with everything they need instead of wasting time setting up the environment from scratch.


**Give your agents a complete feedback loop.** Every piece of context about your CI pipeline can be surfaced to agents out of the box. Trigger a run against uncommitted changes, check status, and pull logs from the CLI. No GitHub event required.


**Debug and monitor your jobs.** Logs, CPU and memory metrics, and[SSH](https://depot.dev/docs/ci/how-to-guides/debug-with-ssh) are built in by default. Depot CI builds a full DAG of your pipeline with full durability and the ability to replay from anywhere. Retry specific failures. Debug from a specific point in your workflow. No more commit and pray.


**Do everything programmatically.** Not just a UI, or even a CLI — fully API-driven. Trigger jobs with local file changes before commit. Grab logs with a single command. Write your own frontend, compose workflows programmatically, build your own automation on top. Everything in Depot CI has an API.


## Pricing for Depot CI


Depot CI is billed at **$0.0001/second** . There are no minimums or additional fees for any of the additional features. All Depot[pricing plans](https://depot.dev/pricing) include Depot CI minutes that can be used in combination with any of our other products.


## What we built


Imagine a new kind of CI: You push code. Two seconds later, you're in a running job — prewarmed, with your custom image already loaded. Your code is there. Your dependencies are there. Your build runs.


Every single time.


Depot CI is an entirely new CI system built from the ground up with Depot performance baked into its core. It's not a faster version of GitHub Actions. Think of GitHub Actions as a language that Depot CI speaks — the first frontend to the engine, not the only one.


We built our own orchestrator, our own compute layer. Rather than relying on a control plane we can't even see, all of Depot CI is powered by our own plumbing.


## The architecture


As with any other Depot announcement, we love sharing what we have built at a technical level. We will have several blog posts to come in the near future that go deep into the details.


Depot CI is built on top of a system we call Switchyard – a system that takes an arbitrary task and ensures it is executed across a fleet of compute.


Depot CI


Supported syntax


GitHub Actions


GitLab CI


API


...


Engine


Parallel steps


Built-in caching


SSH debugging


Custom images


Per-second billing


Snapshotting


Switchyard


Orchestration


Dispatches workflows, resolves dependencies, durable checkpoint replay


Compute


Provisions resources, executes tasks, performance-tuned by default


Switchyard has two core components:


**The orchestrator** schedules and dispatches tasks to fleets of compute, handling dependencies between tasks, and monitors progress. It’s durable, with full checkpoint replay and snapshotting built in.


**The compute subsystem** is responsible for executing the tasks. It receives tasks from the orchestrator, provisions the necessary compute resources, and executes the task it was given. The subsystem drives compute that is fine-tuned for performance and built on top of our own sandboxes with the ability for that compute to exist in our cloud or yours via Depot Managed.


In front of Switchyard sit frontends. Think of these as translators that turn a given language, like GitHub Actions, into a language that Switchyard understands.


A frontend is a representation of a collection of tasks you’d like executed. You can take an arbitrary frontend and translate it into the lower-level tasks that Switchyard understands, and it will drive all of the orchestration and execution of those tasks.


Switchyard opens a lot of doors for what’s possible. It gives folks the ability to bake the performance and reliability of Depot into a variety of different use cases. It is the core on which Depot CI is built. But it’s a fully programmable interface that other systems can also be built on top of.


Sandbox APIs, workflow execution engines, and even your own custom CI frontends are all things that Switchyard can power today.


## Where this is going


Depot CI is the beginning of something much bigger.


We started with build acceleration because that's where the pain was sharpest. Container builds, then CI runners, now the full CI engine. Each step gave us deeper control over the stack and unlocked performance for our users that wasn't possible before.


We’re excited about the future that Depot CI will unlock for teams in this new era of software engineering. You've built up momentum in the code authoring stage. Depot CI is how you keep it all the way through to production.


## Learn more


- [Depot CI overview](https://depot.dev/docs/ci/overview)
- [Depot CI quickstart](https://depot.dev/docs/ci/quickstart)
- [Depot CI compatibility with GitHub Actions](https://depot.dev/docs/ci/compatibility)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
