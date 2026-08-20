---
schema_version: "1.0.0"
document_id: "5b58393850214c90d4072538261bac8cc35556dd236d5830dce0fc1acd31cc0a"
company_key: "yc-trainy"
company: "Trainy"
source_id: "yc-trainy-rss-dacd52d4547a"
canonical_url: "https://www.trainy.ai/blog/what-neptune-got-right"
published_at: "2026-02-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.988023+00:00"
fetched_at: "2026-07-28T22:19:43.789522+00:00"
content_hash: "sha256:a56ccfddf04af9b19317d2cd0203227b89b5482ab5d47df6790cdb1dc7aa182e"
---

# What Neptune Got Right (And How to Keep It)

Neptune is shutting down on March 5, 2026 (10am PST). For teams that have been using it for years, the real loss isn't just the migration work. It's losing one of the few trackers that stayed fast when projects got big.


At[Trainy](https://www.trainy.ai/) , we build infrastructure for GPU clusters. When the shutdown was announced, our customers started asking what they should migrate to. The recurring requirement wasn't "more features", it was responsiveness with thousands of runs and heavy time-series logging. That's what Neptune consistently nailed, and none of the alternatives our customers evaluated matched it.


So we built[Pluto](https://pluto.trainy.ai/) : an experiment tracker based on[our fork](https://github.com/Trainy-ai/pluto) of the open source project[MLOp](https://github.com/mlop-ai/mlop) . Same focus on staying snappy at scale, with a low-risk migration path. Dual-log by adding one import, validate parity, then cut over when you're ready.


This post is about what made Neptune good, what we think could be better, and how we approached building Pluto.


## What Neptune Got Right


### Scalability and responsiveness


Neptune handled scale well. You could log 50k+ metrics per run with no dropped points and no timeouts under sustained logging. Projects with thousands of runs remained usable. And the UI was fast: graphs loaded quickly, filtering the runs table didn't freeze the page. This sounds basic, but if you've used a slow experiment tracker while trying to debug a training run, you know how much it matters.


### The query system


Neptune's query language (NQL) was actually quite powerful. You could filter runs by any field:` recall > 0.9 AND learning_rate <= 0.005` . You could combine conditions with AND/OR, use comparison operators on metrics, filter by tags, owners, dates. The query builder made it easy to construct these without memorizing syntax.


### Side-by-side comparison


Neptune's side-by-side view was well designed. It stacked attributes as rows instead of columns, which made scrolling through differences easier. You could filter to show only rows with differences, set a reference run to see relative changes, and even diff long strings character-by-character.


## Where We See Room to Build


### Graph UX


Neptune's graphing was solid, but there were a few areas where we think the experience could be better:


- Legend sorting was based on current metric value, which meant the order shifted as you scrubbed through time
- Filtering views within graphs required manually hiding metrics one by one
- Smoothing was global rather than per-panel or per-metric
- Syncing axes across graphs for debugging gradient spikes required manual axis linking per chart group


### Dev Workflow


Neptune lived somewhat separately from the rest of your workflow. We'd want the ability to tie experiments to Linear/Jira issues, terminate running jobs from the UI, and view code diffs in the UI. That last one came up a lot for us: two runs, identical hyperparameters, different results. Something in the code changed, but you'd have to track down git commits and diff them yourself.


### Tensor logging


Neptune required you to render plots client-side before logging them. There was no way to natively plot raw tensors on the fly. Logging tensors directly and generating visualizations server-side would let teams iterate on what they visualize without worrying about rendering and saving images client-side.


### LLM Integration


Experiment data is a natural fit for LLM querying. Being able to ask "which runs last week had the lowest validation loss" or get automatic summaries of experiment batches would save a lot of manual dashboard digging. (We shipped an early version of this with[Pluto MCP](https://docs.trainy.ai/pluto/mcp) , currently in alpha.)


## Pluto's Stack


Pluto is built with the same focus on responsiveness that made Neptune good. It's based on[our fork of MLOp](https://github.com/Trainy-ai/pluto) , and we've been working with design partners to figure out what actually matters to teams running serious training workloads.


Under the hood:


- ClickHouse for OLAP, enabling fast analytics on metric series with 1M+ points.
- Postgres OLTP for user, organization, and run metadata.
- Rust ingestion server for fast non-blocking metric writes. We added on-disk buffering to both the client and server for increased reliability against networking hiccups.
- Ingestion server handles 2000+ req/s end-to-end per replica. P95 latency: 200ms at 2k req/s.
- React frontend for a responsive UX. We added lazy loading optimizations to increase the responsiveness of the UI even when multiple runs and charts are being loaded on a single page.
- Whole stack is self-hostable via docker-compose in about a minute. While we host this stack with multiple replicas for high availability, it's still very fast on a single machine with around 8 CPUs, 32GB RAM, and 1TB disk.


### Neptune compatibility layer


We built a compatibility layer so you can dual-log to both Neptune and Pluto with minimal code changes:


```text
import pluto.compat.neptune  # Add this line
from neptune_scale import Run  # Your existing Neptune Scale import
# Your existing Neptune Scale code works unchanged
run = Run(experiment_name="my-experiment")
run.log_configs({"lr": 0.001, "batch_size": 32})
run.log_metrics({"train/loss": 0.5}, step=0)
run.close()


```


Set the` PLUTO_PROJECT` environment variable and your metrics log to both platforms.


This is intentional. We wanted to eliminate all risk of trying Pluto. Your Neptune logging keeps working exactly as before. If Pluto breaks, you still have Neptune. If you don't like Pluto, remove the import and nothing changes. There's no commitment until you're ready.


When you are ready to stop logging to Neptune entirely:


```text
export DISABLE_NEPTUNE_LOGGING=true


```


All Neptune API calls get redirected to Pluto. No code changes required.


## Migration Guide


### Common pitfalls


Before diving in, a few things worth knowing:


- **Exports take longer than you'd expect.** Neptune explicitly warns that workspace exports can take days to weeks depending on size. Don't wait until the last week of February.
- **Artifact sizes and storage planning.** If you've been logging model checkpoints, large files, or images, figure out how much storage you'll need on the other side before you start.
- **Legacy Neptune client vs Neptune Scale.** Pluto's compatibility layer targets[Neptune Scale](https://github.com/neptune-ai/neptune-client-scale) (` neptune-scale` package), not the legacy` neptune` client. If your codebase still uses the old client, you'll need to[migrate to Neptune Scale](https://docs.neptune.ai/migration_neptune/) first.
- **Run IDs and fork mapping.** Neptune's internal run IDs and fork relationships don't have a universal standard. Make sure your export preserves the lineage you care about.
- **Verifying parity means more than metrics.** When we say "validate that everything matches," that includes step counts, tags, system metrics, and logged files, not just the loss curves.


### Step 1: Set up dual-logging


Add the compatibility import and set the` PLUTO_PROJECT` environment variable as described in the compatibility layer section above. Run this on real training workloads to validate at scale.


### Step 2: Export your Neptune history


We have an exporter that brings your historical runs into Pluto. Details:[Exporting Neptune Runs](https://docs.trainy.ai/pluto/exporting-neptune-runs)


### Step 3: Validate and give us feedback


Before removing Neptune:


- Do metrics match between both platforms?
- Is the UI responsive at your scale?
- Can your team find what they need?


This is where we need your help. If something's missing, broken, or would block your migration, tell us. Email roanak@trainy.ai or open an issue on GitHub. We're shipping fixes fast and your feedback directly shapes what we build next.


### Step 4: Cut over


Set` DISABLE_NEPTUNE_LOGGING=true` . All Neptune calls redirect to Pluto. Done.


## Pricing and Links


Pluto's hosted plan is $250/seat/month, matching Neptune's pricing.


- [Live playground](https://demo.pluto.trainy.ai/o/dev-org/projects/my-ml-project) , no signup required
- [Quickstart](https://docs.trainy.ai/pluto/quickstart)
- [Neptune migration docs](https://docs.trainy.ai/pluto/neptune-migration)
- [GitHub](https://github.com/Trainy-ai/pluto)


We're listed on[Neptune's official transition hub](https://docs.neptune.ai/transition_hub/migration/to_pluto) .


Pluto is open-source. The[Python client](https://github.com/Trainy-ai/pluto) is Apache-2.0 licensed. The[server](https://github.com/Trainy-ai/pluto-server) is AGPL-3.0. You can self-host the entire stack via docker-compose: clone the server repo, copy` .env.example` to` .env` , and run` docker compose up --build` . The server README has full setup instructions.


Questions? Email roanak@trainy.ai or[book a time](https://calendly.com/roanak/trainy-demo) .
