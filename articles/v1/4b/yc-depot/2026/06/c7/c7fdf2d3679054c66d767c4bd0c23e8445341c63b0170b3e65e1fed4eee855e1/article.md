---
schema_version: "1.0.0"
document_id: "c7fdf2d3679054c66d767c4bd0c23e8445341c63b0170b3e65e1fed4eee855e1"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-depot-ci-api"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:49:35.808686+00:00"
content_hash: "sha256:eac631412db35c45ece3542c299dac040770e6cb7fc3a7d4875327ffc7a9f908"
---

# Now available: Depot CI API and CLI

We promised full programmability when we launched Depot CI back in March. Today we make it official: the Depot CI API and CLI are generally available. We believe that you and your agents should be able to do everything you can do in the dashboard from the CLI or API. When you're deep at work in your terminal or editor and need answers about your CI, you don't have to go look. You can just ask.


## Why I care about this one


I work at Depot, so feel free to discount my enthusiasm accordingly. But this one's personal.


The way I work now, I don't sit in dashboards. I sit in my editor with an agent next to me. When CI breaks, I don't want to alt-tab into a browser, hunt down the run, scroll the logs, and rebuild the whole story in my head. I want to ask the thing right in front of me "what broke?" and get an answer.


That only works if the agent can reach the same information I can. Before today, it couldn't. The dashboard knew things the API and CLI didn't, so the agent was stuck, and so was I.


Software that only talks through a browser is software your agent can't help you with. That was the gap. This closes it.


## What's new


Under the hood, our protobuf/Connect API is the source of truth, and we generate an OpenAPI v3 spec straight from it. That spec is the public contract. The CLI reads from it, your scripts read from it, your agent reads from it. One description of reality instead of a doc that goes stale the second someone ships.


On top of that, the stuff that used to be dashboard-only now lives everywhere you work:


- See your recent runs and workflows, and dig into any run, workflow, job, or attempt.
- Pull logs by ID, tail them live, or export them in bulk.
- Kick off a workflow, grab a single run, or cancel an entire run in one call.
- Get the derived stuff too: AI run summaries, step summaries, run and job metrics, the artifact list.
- Manage your secrets and variables through the same commands.


Same nouns, same flags, whether you type it or call it.


## My fav:` depot ci diagnose`


My favorite thing in here is` diagnose` . It does the part of CI debugging I dislike the most, which is figuring out what even went wrong before I can start fixing it.


Point it at whatever ID you've got:


```text
depot   ci   diagnose   --run   <  run-i  d  >
depot   ci   diagnose   --workflow   <  workflow-i  d  >
depot   ci   diagnose   --job   <  job-i  d  >
depot   ci   diagnose   --attempt   <  attempt-i  d  >
```


Instead of dumping logs on you, it tells you what it thinks broke. The failed step. The error lines that matter. And a real read on what went wrong and what to do about it. Not "here's the wreckage, good luck." More like "this is the thing, here's the likely cause, here's your next move."


A few things I like about it:


It collapses big matrix failures. If 7 cells died, you get "showing 3 of 7 similar attempts" instead of seven near-identical walls of text.


It hands you the exact commands to go deeper. Want the logs? It gives you the command. Want the step summary? Same. Want to open it in the browser anyway? There's the URL.


It doesn't fire-hose you. No dumping thousands of jobs or full logs by default. If there's more to see, it tells you how to narrow.


I love letting the agent run with it. "CI is red, go figure out why" used to be something I had to do by hand, because the agent couldn't see the runner. Now` diagnose` gives it (and me) a straight answer to start from.


## Getting started


If you've got the Depot CLI, you already have this. Update to the latest version and go:


```text
depot   ci   run   list
depot   ci   workflow   list
depot   ci   diagnose   --run   <  run-i  d  >
```


It's all available over the API too, using the OpenAPI v3 spec as the contract, so you can wire it into scripts or hand it to an agent.


For details, check out the Depot CI[API reference](https://depot.dev/docs/api/ci/reference) and[CLI reference](https://depot.dev/docs/cli/reference/depot-ci) .


## What it costs


Nothing new. The API and CLI use the same usage-based[pricing](https://depot.dev/pricing) Depot already has, which is all public on our pricing page. If a call spins up compute, like running a workflow, you pay for that compute the same as you always did. Listing your runs, diagnosing a failure, hitting the API itself, none of that adds a separate charge. There's no API tax.


## What's next


We're shipping new Depot CI stuff fast right now, and the entire point of this project was to stop letting the API and CLI fall behind the dashboard. So as we add things, expect them to land in the API and CLI right next to the dashboard. Anyway, update your CLI, and next time something goes red, try pointing` diagnose` at it instead of opening the dashboard. Or don't even do that yourself. Ask your agent to.


## Related posts


- [Now available: Depot CI](https://depot.dev/blog/now-available-depot-ci)
- [What we need from CI for agentic engineering](https://depot.dev/blog/ci-for-agentic-engineering)
- [Now available: Sherlock can analyze Depot CI workflows and jobs](https://depot.dev/blog/now-available-sherlock-depot-ci-analysis)


Andrew "Watts" Watkins


Engineering Manager at Depot
