---
schema_version: "1.0.0"
document_id: "c94dd7e118df0cc71a8ae46804fd5e3d94edf0b8b5b8e72c1f5be504d77ca00d"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/ci-for-agentic-engineering"
published_at: "2026-03-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:56:50.434513+00:00"
content_hash: "sha256:84818a62b0569cd1bc33986450b910df598caba05365c1b645e9ffb091e33941"
---

# What we need from CI for agentic engineering

It's not a controversial statement to say that software engineering has dramatically changed over the past 18 months. The way I write code today is hardly ever by hand. Instead, I can now prompt an agent, or even fleets of agents, to take my wild ideas and make them a reality.


Just in the past three months we went from having to be intimately in the loop with our agents to now throwing them all kinds of things and letting them cook. We're all collectively moving at a velocity never before seen.


But, we're all hamstrung by the elephant in the room: existing CI platforms and paradigms simply can't keep up.


CI was designed for humans.


## The impedance mismatch


The CI systems we have today are built around specific assumptions: a developer writes some code, pushes it to a branch, opens a pull request, and waits. Most engineers context switch to something else while waiting for CI to finish. The traditional processes measure feedback loops in minutes or tens of minutes. It wasn't great but it was fine enough.


With agents writing code, that assumption collapses. An agent can write code in seconds, commit it, monitor CI, read the results, watch for regressions in the logs, detect issues, fix them, and push again – in a loop – much faster than any CI system was designed to support. The agent isn't doing something else while CI runs. It's blocked.


Today CI systems force humans back into the loop. To help the agent context switch to something else while waiting for CI. To paste errors from CI back into the agent and say "fix it". To help the agent get logs about what regressed.


Every time an agent has to push code and wait for CI or have a human help it understand its results, you've added friction to a process that's supposed to be autonomous.


CI is now in the critical path in a way it never quite was before.


## What CI needs to be for agents


If you think about what makes CI painful specifically for agentic workflows, although engineers often talk about wanting these capabilities as well, a few things stand out:


### Targeted reruns, not full pipeline reruns


Nothing is more annoying than trying to debug one step inside of a fifteen step CI workflow. That drives engineers nuts. But it actively blocks agents from finishing their work. When an agent is finishing a feature, waiting for a 15-minute pipeline to finish while validating a 3-line fix inside of a fifteen step workflow is genuinely wasteful in terms of time and tokens. Agents need to be able to rerun a single job inside of a workflow – not restart from scratch every time.


### Run real CI on local patches


Agents and engineers today have to commit and pray to learn if their debugging hypothesis is correct. That's the worst possible inner loop: write a change, commit it, push it up, wait 5 minutes, get a result, shit it's broken, revert, try again. CI should be able to be invoked with local file changes from the agent writing the code.


### All context behind an API


Most CI systems were designed for humans clicking through dashboards. Their API is a bolted on concept, not something designed to be the primary interface. Agents don't click through dashboards. They need to trigger runs, poll status, retrieve logs, and make decisions programmatically. If the API isn't there to give agents context, you're forcing them into hacky workarounds.


### Speed and orchestration at scale


A single engineer can be operating tens of agents simultaneously. Several agents, several branches, all needing CI at the same time. The latency, queueing, and run time of your CI pipelines and their backing providers matter a lot more when you have 20 agents all trying to validate changes at once. Not to mention that you need pricing that is clear without any hidden gotchas like one minute minimums.


## What this looks like with Depot CI


We built Depot CI and simultaneously used it as the CI system for our own agentic engineering workflows. We made design decisions that make Depot CI a fundamentally different system that engineers and agents can use.


First, it's intended to support arbitrary syntaxes or code that represent a CI pipeline. GitHub Actions is the first syntax we're supporting, but we're working on our own SDK if you want to use code. We're also planning to make the syntax system open source so that you can define your own.


You or your agent can convert your GitHub Actions workflows to Depot CI with a single command:


```text
depot   ci   migrate
```


The[depot ci migrate](https://depot.dev/docs/cli/reference/depot-ci#depot-ci-migrate) command discovers your existing workflows, applies compatibility fixes, and copies the result to your` .depot` directory. Your` .github` folder stays intact — you can run GitHub Actions and Depot CI in parallel while you validate, then cut over when you're ready.


### Targeted reruns


Agents can now kick off a CI workflow without having to trigger any other kind of event or push code to your repository. You can do that with:


```text
depot   ci   run   --workflow   .depot/workflows/ci.yml
```


Or if you only want to run a specific job, you can do that with:


```text
depot   ci   run   --workflow   .depot/workflows/ci.yml   --job   smoke_container
```


Or if you want to have your agent[debug a specific step in a job](https://depot.dev/docs/ci/how-to-guides/debug-with-ssh) , you can do that with:


```text
depot   ci   run   --workflow   .depot/workflows/ci.yml   --job   smoke_container   --ssh-after-step   3
```


### Run real CI on local patches


What if your agent has local changes it hasn't committed yet? It can run CI against them directly — no push required. Every` depot ci run` command picks up local file changes automatically. The agent writes a change, validates it, and only commits once it knows it works. No more pushing broken commits just to find out they're broken.


### All context behind an API


Depot CI provides a full API for agents to interact with. Today, you or your agents can access the API through the Depot CLI. You can trigger workflows as described above. But you can also poll status, retrieve logs, and make decisions programmatically.


Check what workflows are queued or running:


```text
depot   ci   run   list
```


Check the status of a specific workflow run:


```text
depot   ci   status   <  run-i  d  >
```


Retrieve logs for a specific job attempt:


```text
depot   ci   logs   <  job-attempt-i  d  >
```


### Speed and orchestration at scale


The previous three are all about helping agents rapidly iterate and validate their changes with real CI.


But for that to work, the speed and reliability of the CI system must be able to scale to the needs of the agentic workflows. The volume at which agents can validate and test their changes is going to be massive.


A single engineer today might be running five, ten, or twenty agents simultaneously — each on its own branch, each needing CI to validate its work. That's not a hypothetical, that's just how people are working right now. The queueing behavior, cold start latency, and per-job runtime of your CI system matter a lot more in that world than they did when a team of ten humans was pushing thirty commits a day.


Depot CI is a completely new category of CI system built with performance at every step. Plumbing that moves a commit to a running job with minimal queue time. Runners optimized for speed and security with our single-tenant architecture. Flexibility to make CI workflows as fast as you need them to be.


It's all there from the start. Not an afterthought. Not a bolt on. Not a checkbox.


We charge for Depot CI by the second. No one minute minimums. Your agent can validate its changes in seconds instead of minutes, and you only pay for the seconds it takes to run.


## The bigger picture


Targeted reruns, local patch execution, programmatic API access for logs/triggering jobs/checking status, observability into what's happening in your job, fast orchestration, low queue times, and reliable execution. Engineers have been asking for these things for years. But no CI system was built around them from the start.


We built a programmable CI system from the ground up with all of these concepts baked in. Depot CI is built for engineers and agents. It's the system engineers have been asking for. And it's also the system we need to support the velocity of agentic engineering.


## FAQ


Why can't existing CI systems like GitHub Actions keep up with agentic workflows?


Traditional CI was built around a human-paced loop: push code, open a PR, wait, context switch. Agents don't context switch. They write code in seconds and need feedback just as fast. When an agent is blocked waiting 10-15 minutes for a full pipeline to finish, that's dead time where it could be iterating. The queueing, cold starts, and lack of programmatic APIs in most CI systems turn what should be a tight feedback loop into a slow, manual one.


How does Depot CI let agents run CI on local changes without pushing?


When you run` depot ci run` , the CLI automatically detects uncommitted changes relative to the default branch, uploads a patch, and applies it after checkout in each job. The agent doesn't need to push or open a PR to validate its work. Once the agent confirms the changes pass, it pushes. This eliminates the "commit and pray" cycle where you push broken code just to see if CI passes.


Can an agent rerun just one failing job instead of the entire CI pipeline?


Yes. With` depot ci run --workflow .depot/workflows/ci.yml --job <job-name>` , an agent can target a specific job inside a workflow. It can also drop into a debug session after a specific step with` --ssh-after-step` to investigate interactively. This means an agent fixing a 3-line bug doesn't have to wait for 14 unrelated jobs to finish before getting feedback on the one that matters.


How does per-second billing change the economics of running CI for multiple agents at once?


When you're running 10 or 20 agents simultaneously, each kicking off CI runs to validate small changes, per-minute minimums add up fast. A job that takes 8 seconds still costs you a full minute on most platforms. Depot CI charges by the second with no minimums, so those quick validation runs cost exactly what they should. That's a meaningful difference when your agents are running hundreds of short CI jobs per day.


## Related posts


- [The end of push-wait-guess CI](https://depot.dev/blog/the-end-of-push-wait-guess-ci)
- [Now available: Depot CI](https://depot.dev/blog/now-available-depot-ci)
- [What we learned accelerating 100 million builds in 2025](https://depot.dev/blog/depot-2025-recap)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
