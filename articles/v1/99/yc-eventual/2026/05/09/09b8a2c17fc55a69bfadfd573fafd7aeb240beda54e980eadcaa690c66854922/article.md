---
schema_version: "1.0.0"
document_id: "09b8a2c17fc55a69bfadfd573fafd7aeb240beda54e980eadcaa690c66854922"
company_key: "yc-eventual"
company: "Eventual"
source_id: "yc-eventual-news-import-1ca85739ec11"
canonical_url: "https://www.eventual.ai/blog/first-class-observability-in-daft"
published_at: "2026-05-25T08:00:00+00:00"
first_seen_at: "2026-07-25T03:47:26.048705+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:90ed359df4e40cb23991085e2b79a860f723e783f490323e4af5b7581cf4150b"
---

# First-class observability in Daft

## TLDR


- **New Daft dashboard makes it easy to introspect operator memory, row throughput, and morsel-driven tasking locally/distributed**
- **OTel Endpoints for production telemetry.** Just set the` OTEL_EXPORTER_OTLP_ENDPOINT` environment variable for your existing collector to point Daft OTel metrics at your existing collector.
- **Stuck detection and a Unix style "TOP" view of tasks.** Help isolate the root cause of slow, stuck, or failed queries.
- **` DAFT_TRACE` for console debugging.** Set` DAFT_TRACE=pretty` for a structured trace of execution events on stdout — no dashboard required.


## What does it mean for Daft to be observable?


> ***Understand what Daft is doing with your query, enough to track down failures or tune for performance.***


Daft powers exabyte-scale pipelines at the world's top technology companies. Until now, teams running Daft in production had to craft observability dashboards by hand. These teams shouldn't have to guess about their data infrastructure, especially given the cost and complexity of operating distributed systems at scale.


Today we're adding new observability tooling that makes` Operators` ,` Tasks` ,` Rows` , and` Memory` first class citizens. The Daft Dashboard ships with Daft itself powered by OTel compatible instrumentation endpoints. Both Python and Rust are now wired with better logging, metrics, and traces. The result is a frontend experience that is not only a better tool for debugging, but also a more accurate reflection of Daft's morsel driven execution model.


For users, this means that when a query is slow, fails, or gets stuck, they can now see what Daft was doing and which operators are taking consuming the most memory. The Tasks view makes it easy to see the longest running tasks for each operator. Overall, these improvements help users introspect Daft's runtime behavior so they can understand and optimize their queries and pipelines.


## Start with the dashboard


The Daft Dashboard is a web UI for inspecting query execution. It ships with the standard` daft` package. Start the server, point your script at it, and Daft will push query events into the UI while the job runs.


This matters most when the query is slow, stuck, or failing. The dashboard now shows the physical plan, live operator stats, distributed task progress, failure context, and memory-related metrics in one place. You can use it locally while developing a query, or point a Ray cluster at a dashboard URL that every actor can reach.


```text
uv   add   daft
uv   run   daft   dashboard   start
export   DAFT_DASHBOARD_URL  =  http  ://  localhost  :  3238
uv   run   my_script.py
# then open http://localhost:3238
```


```text
python   -m   venv   .venv
source   .venv/bin/activate
pip   install   daft
daft   dashboard   start
export   DAFT_DASHBOARD_URL  =  http  ://  localhost  :  3238
python   my_script.py
# then open http://localhost:3238
```


### Find the slow operator


Open the dashboard and the first useful view is the plan tree. It shows the physical operators Daft is running, how data flows between them, and which operators are taking the most time.


Operator cards now include wall-clock time and CPU duration. Slow operators are colored differently from fast ones. Pipeline arrows make the direction of execution explicit. This gives you a fast answer to the first debugging question: where is the query spending time?


- Interactive tree visualizer for query plans ([#6295](https://github.com/Eventual-Inc/Daft/pull/6295) )
- Physical plan tree with live operator stats ([#6299](https://github.com/Eventual-Inc/Daft/pull/6299) )
- Wall-clock and CPU duration on operator cards ([#6300](https://github.com/Eventual-Inc/Daft/pull/6300) )
- Pipeline direction arrows in the plan tree ([#6625](https://github.com/Eventual-Inc/Daft/pull/6625) )
- Heatmap coloring for operator nodes ([#6628](https://github.com/Eventual-Inc/Daft/pull/6628) )


### Inspect distributed tasks


Distributed queries fail and slow down at the task level. A plan node can look suspicious, but you still need to know which tasks are running, which are pending, and whether one task is dragging the operator behind the rest.


The new tasks view makes that visible. It shows task progress from Flotilla workers, separates pending work from running work, and ties task rows and bytes back to the plan topology. If one scan task is reading much more data than the others, or one task group is taking longer because it has a different local-plan shape, you can see that directly instead of guessing from aggregate operator time.


Running this on a cluster takes one piece of setup. Every Ray actor needs` DAFT_DASHBOARD_URL` set so it can push events to the dashboard process. Set the env var on the driver before importing Daft — the value propagates to actors automatically — and point it at a hostname routable from every node, not` localhost` .


- Draft tasks view for Flotilla ([#6783](https://github.com/Eventual-Inc/Daft/pull/6783) )
- Tasks tab as a collapsible sidebar in the Execution tab ([#6752](https://github.com/Eventual-Inc/Daft/pull/6752) )
- Per-task progress updates from Flotilla workers ([#6838](https://github.com/Eventual-Inc/Daft/pull/6838) )
- TaskScheduled events distinguishing pending vs running ([#6866](https://github.com/Eventual-Inc/Daft/pull/6866) )
- Task sources surfaced in the tasks sidebar ([#6879](https://github.com/Eventual-Inc/Daft/pull/6879) )
- Per-task rows and bytes stats with topology markers ([#6861](https://github.com/Eventual-Inc/Daft/pull/6861) )
- Task groups split by local-plan shape for accurate durations ([#6899](https://github.com/Eventual-Inc/Daft/pull/6899) )
- Smart per-node stats aggregation for distributed execution ([#6574](https://github.com/Eventual-Inc/Daft/pull/6574) )
- Partition sets passed through` repr_json` so the plan matches execution topology ([#6576](https://github.com/Eventual-Inc/Daft/pull/6576) )
- ` num_tasks` metric tracking across runtime stats ([#6716](https://github.com/Eventual-Inc/Daft/pull/6716) )


### Locate failures and stalls


When a query fails, the useful question is not only "what exception was raised?" It is "where in the plan did that exception come from?"


The dashboard now surfaces query failure details in the query view and highlights failed operators on the plan tree. It also uses subscriber heartbeats to detect queries that stop reporting progress. That gives operators a clear place to start: the failed operator, the stalled subscriber, or the task group that stopped moving.


- Surface query failure details in the query view ([#6897](https://github.com/Eventual-Inc/Daft/pull/6897) )
- Highlight failed operators directly on the plan tree ([#6930](https://github.com/Eventual-Inc/Daft/pull/6930) )
- Subscriber heartbeat and dead-query detection ([#6676](https://github.com/Eventual-Inc/Daft/pull/6676) )


### Track bytes through the plan


Out-of-memory failures are hard to debug when all you know is that a worker died. You need to know what the query was doing when memory climbed, and which operators were expanding or retaining data.


Daft now reports per-operator bytes in and bytes out. The dashboard shows inflation and deflation metrics so you can see where data expands as it moves through the plan. The tasks view also carries rows and bytes at task granularity, which helps separate a genuinely expensive operator from a skewed input partition.


For production telemetry, Daft exports OTel metrics that can be sent to your existing collector. The goal is straightforward: when a job runs out of memory, Daft should give you enough evidence to decide whether to repartition, exclude bad input, scale out, or change the query.


- Per-operator` bytes_in` /` bytes_out` ([#6612](https://github.com/Eventual-Inc/Daft/pull/6612) )
- Bytes inflation / deflation metrics in the dashboard ([#6640](https://github.com/Eventual-Inc/Daft/pull/6640) )
- Per-task rows and bytes stats ([#6861](https://github.com/Eventual-Inc/Daft/pull/6861) )
- Fix for Flotilla overreporting of` bytes.read` ([#6774](https://github.com/Eventual-Inc/Daft/pull/6774) )


### Run it with production guardrails


Two production details are worth calling out.


First, dashboard query state is now bounded. A long-running dashboard process should not grow forever just because many queries reported into it.


Second, the old result preview path is gone. It relied on deserializing pickle bytes from query execution, which was the wrong security tradeoff for a dashboard that might receive events from distributed workers. Removing it makes the dashboard safer to run outside a throwaway local session.


The dashboard still needs normal operational care. The current docs call out that it has no built-in authentication, stores state in memory, and should not be exposed directly to the public internet. Put it behind the same network controls you would use for any internal debugging surface.


- Bounded query state retention ([#6896](https://github.com/Eventual-Inc/Daft/pull/6896) )
- Removed the pickle-based result preview tab ([#6878](https://github.com/Eventual-Inc/Daft/pull/6878) )


## What is still planned


This launch makes the current query easier to inspect and the previous query easier to reconstruct. The next set of work is aimed at larger production fleets.


Shuffle and spill observability is next for distributed debugging. Dashboard persistence will move query history out of process memory and into a database. Timeseries support and self-hosted dashboard work come after that, followed by cluster metrics so operators can see more of the Ray environment without switching tools.


Those features are planned. The shipped features today are the dashboard views, task-level progress, failure surfacing,` DAFT_TRACE` , OTel metrics, and per-operator bytes.


## Try the dashboard


```text
uv   add   daft
uv   run   daft   dashboard   start
export   DAFT_DASHBOARD_URL  =  http  ://  localhost  :  3238
uv   run   my_script.py
# then open http://localhost:3238
```


For cluster setups, point` DAFT_DASHBOARD_URL` at a hostname routable from every Ray actor. Full docs at[docs.daft.ai/observability/dashboard](https://docs.daft.ai/en/stable/observability/dashboard/) .


Run the query. Open the dashboard. Start with the slowest operator, then drill into tasks, bytes, and failure details from there.
