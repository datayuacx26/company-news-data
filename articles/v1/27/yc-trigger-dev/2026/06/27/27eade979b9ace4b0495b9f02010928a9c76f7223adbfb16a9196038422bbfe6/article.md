---
schema_version: "1.0.0"
document_id: "27eade979b9ace4b0495b9f02010928a9c76f7223adbfb16a9196038422bbfe6"
company_key: "yc-trigger-dev"
company: "Trigger.dev"
source_id: "yc-trigger-dev-news-import-c59968a07222"
canonical_url: "https://trigger.dev/blog/incident-report-jun-30-2026"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-24T05:26:40.243460+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:bf043ef4d49b19e36ff83471ce64c21e70c054c1d3ac8e68e06ad966050e64c7"
---

# Runs list and logs degraded

Before we get into what happened, I want to say up front that this is the second incident in just over a week, and that's not the experience we want to give you. A lot of you build your businesses on top of Trigger.dev, and for several hours on June 30 our dashboard, runs list, and logs were slow or unavailable. We're sorry. The most important thing to say first is that your runs kept executing the whole time. The surfaces you use to see and debug those runs did not, and that matters too.


This is a long writeup because the cause was subtle and we think the detail is worth sharing.


## TL;DR impact and causes


For roughly six and a half hours on June 30 (about 09:00 to 15:30 UTC, with the worst of it between 09:15 and 11:00), the **runs list and logs in the dashboard and API were degraded** : slow, timing out, or intermittently failing. p99 latency on the runs list went from around 1.5 seconds to around 30 seconds at the peak. New logs and traces stopped being ingested from about 10:16 until the early afternoon, the stretch when the events table was over its part ceiling.


**Your runs kept executing normally throughout.** Run execution and the run queue don't touch ClickHouse, and the systems behind them (Postgres and Redis) were healthy the entire time. The degraded surfaces are powered by ClickHouse, the analytical database we use for the runs list, logs, and traces. ClickHouse is where the problem was.


The problem was caused by a chain of events:


1. On June 17, our ClickHouse cluster was automatically upgraded from 25.12 to 26.2. That version enforces a new limit on how complex a column's data type can be, and it applies that limit to background merges (the housekeeping that keeps the database fast). The limit cannot be raised for merges; it is effectively hard-coded.
2. Our runs and events tables store task` output` ,` error` , and span attributes as ClickHouse` JSON` columns. Because different tasks return very different shapes, the combined schema of these columns is large. After the upgrade, the background merges that combine these columns started failing, against data that had been perfectly valid for months. A compatibility safeguard on ClickHouse's side that should have preserved the old behavior was broken.
3. On the evening of June 29, to fix a separate bug where some runs had started disappearing from the runs list, we raised that complexity setting (` input_format_binary_max_type_complexity` ) for our reads. That fixed the reads, but it also removed a protection that had been keeping high-complexity data out of our runs table, and it let the failing-merge problem spread.
4. On June 30, the failing merges caused database parts to pile up. That, combined with the way the runs list query has to reconcile those parts in memory, drove ClickHouse into its memory ceiling (around 180 GiB) and it started rejecting some queries and writes.


We recovered by scaling the database up and surgically removing the data that couldn't be merged. ClickHouse applied a service-level fix later in the day, and the durable fix is a setting that keeps these JSON types simple enough to merge, alongside a compatibility bug fix they're shipping.


## What we're changing


### 1. Keeping the JSON types simple


ClickHouse is shipping a fix for the compatibility regression behind this. In the meantime we're applying their recommended setting, which makes mixed-type arrays infer as a simple` Array(Dynamic)` instead of the deeply nested types that blew past the limit, so new data stays well under it. We're also weighing whether to lean on these native` JSON` columns less, since this isn't the first time they've caused us trouble, but the setting addresses the immediate cause.


### 2. More alerting on ClickHouse


We weren't watching the right ClickHouse internals closely enough: part counts, merge failure rates, and database memory. Those are the exact signals that would have surfaced this to us 12+ hours earlier, and we're wiring them into our monitoring with alerts.


### 3. Separating reads from writes


Heavy dashboard queries competing for memory with the database's own merge work is part of what tipped this over. We're standing up a dedicated read-only ClickHouse instance for the runs list and log reads, so those queries can no longer contend for the same memory as the merges.


### 4. Creating fewer parts


We're evaluating smaller, more granular table partitions and larger insert batches. Both would give the background merges more headroom and make any single problem part smaller and easier to deal with.


### 5. Safer production changes


The setting change on June 29 fixed one bug but removed a protection we didn't realize was load-bearing. We're tightening how we make production ClickHouse changes, testing them against the background-merge path and production-shaped data before they go live, so a fix for one problem can't open another.


### 6. Protecting the runs list from any single caller


At the worst point, one customer polling the runs list API hard kept the feedback loop spinning. We're adding query-cost and rate limiting on that path so no single caller can amplify a problem for everyone.


## The full detail


### What was impacted


- **Runs list (dashboard and API):** slow, timing out, or intermittently failing for roughly 09:00 to 15:30 UTC, worst between 09:15 and 11:00. If you call` runs.list` from inside your own code, you would have seen those calls slow down or fail too.
- **Logs and traces:** new logs and spans stopped being ingested from about 10:16 until the early afternoon (while the events table was over its part ceiling) and were not visible in the dashboard during that stretch. Historical logs were readable.
- **Run execution:** unaffected. Triggering, the run queue, and execution don't depend on ClickHouse and stayed healthy throughout. No runs were lost or delayed, and nothing stopped running.
- **One data-quality note:** ClickHouse only tells us which run IDs match a list or filter; PostgreSQL is the source of truth for the run data we actually show. So when a run's ClickHouse copy was missing or stale, the effect was that it could be temporarily absent from a filtered runs list or undercounted in a dashboard total, not that we ever showed wrong details for it. Opening any run always loaded its details from PostgreSQL, so they stayed correct and complete. No run, and no run's data, was lost; what could stay off was the ClickHouse analytics copy that powers lists and counts. We measured it: while this was happening, roughly 0.3% of run writes (about 1 in 350) didn't make it into ClickHouse and were briefly missing or stale in lists. Almost all corrected on the next write; the share of runs whose analytics copy stayed wrong afterward was far smaller, on the order of 1 in 100,000 to 1 in 300,000.
- **If you were caught in it:** individual run pages kept working (they load from PostgreSQL), so you could still open a specific run by its URL even while lists were down, and we posted updates on the status page.


### Timeline of major events


All times UTC.


Time What happened


Jun 17 Our ClickHouse cluster is automatically upgraded to 26.2, which begins enforcing a new data-type complexity limit on background merges. Problem parts start accumulating.


Jun 29, ~18:00 To fix runs disappearing from the runs list, we raise the complexity setting on the ClickHouse user. Reads recover, but a protection on the runs table is removed and bad parts begin to spread.


Jun 29, evening A routine ClickHouse service cycle swaps in new replicas. CPU and memory begin climbing as failing merges retry.


Jun 30, ~05:00 First reports of slowness.


Jun 30, 09:13 Runs list read errors begin.


Jun 30, 09:23 ~88% of runs list requests are failing.


Jun 30, ~09:30 ClickHouse hits its memory ceiling (~180 GiB). Queries start failing with out-of-memory errors.


Jun 30, 09:37 We open a priority support case with ClickHouse and a public status page incident.


Jun 30, ~10:16 The events table hits the 3,000-part ceiling. New log and trace inserts start being rejected.


Jun 30, ~11:00 We confirm the underlying cause is failing background merges, and that no setting we control can fix them.


Jun 30, 11:00 to 15:00 We scale the database up (clearing the memory ceiling), then identify and remove the parts that cannot be merged so healthy merges can resume.


Jun 30, ~15:35 The runs table recovers sharply as the last problem parts clear and a stuck replica terminates, freeing merge capacity. The acute incident is over.


Jun 30, ~16:43 ClickHouse applies a service-level configuration change and full restart. Background merges have been healthy since.


Jul 1, morning About a day on, both tables are healthy: zero failing merges, June's data consolidated back to normal part counts, and the new month of data started clean.


### What caused this


ClickHouse stores a table as many immutable "parts" on disk. Every insert creates a new part, and a background process called a "merge" continuously combines small parts into bigger ones. This is what keeps reads fast. If merges fall behind, parts pile up, and ClickHouse has a hard ceiling (3,000 parts) at which it starts rejecting inserts to protect itself.


We store task` output` ,` error` , and span` attributes` as ClickHouse` JSON` columns. Internally, each part records the full set of distinct JSON paths it contains, which forms a "type tree." When a merge combines parts, it has to build one type tree that covers the union of all of them.


Version 26.2 (the upgrade we received on June 17) enforces a limit on how big that type tree can be: 1,000 nodes. The important and painful detail is **how that limit is applied to merges** . The code reads the limit from the current query's settings, and falls back to a hard-coded default when there is no query:


`
inline size_t getMaxTypeDecodingComplexity()


{


if (auto query_context = CurrentThread::getQueryContext())


return query_context->getSettingsRef()\[input_format_binary_max_type_complexity\];


return 1000; // hard-coded default when there is no query context


}


`


Background merges run with no query context. So they always use 1,000, no matter what any user, profile, or server setting says. There is no knob we, or anyone, can turn to let merges handle a bigger type. This check did not exist in 25.12, the version we ran before, so the upgrade is what introduced the regression, against data that had always been valid.


Our` output` column is heterogeneous by nature: every task returns a different shape, so across a table the column accumulates hundreds of distinct paths. The limit was not hit by any single run. It was hit when a merge combined many parts and the union of their schemas crossed 1,000. In one failing merge we looked at, six parts had 526, 608, 504, 635, 576, and 766 paths individually (all well under 1,000), but their union was 1,471. There was no single bad row to remove; the combined schema was simply too big for the new limit.


There is a second half to why those types got so large. A lot of our JSON values contain mixed-type arrays, for example an object and a string in the same array. ClickHouse can store that either as a flat` Array(Dynamic)` or as a deeply nested type that spells out every shape, and a setting decides which:` input_format_json_infer_array_of_dynamic_from_array_of_different_types` . On our cluster that setting was off, so those arrays became deep, high-node types. 26.2 added the complexity check that rejects such types during merges, but the companion setting that would have kept them flat was never turned on, and the compatibility safeguard that should have preserved our pre-upgrade behavior was broken on ClickHouse's side. So the upgrade is what turned data that had merged fine for months into data the merges could no longer handle.


The two affected tables behaved differently, which is worth explaining because it shaped the incident:


- **The events table had no protection at insert time** , so events with complex attributes had been landing as un-mergeable parts since June 17, failing merges in the background. A handful of stuck parts in old daily partitions was not enough to set off alarms.
- **The runs table was accidentally protected.** Its insert path serializes the output to a string in a way that runs through the same complexity check, so high-complexity runs were being rejected at the door. That rejection is what kept the runs table clean. It also produced a visible symptom: some runs "disappeared" from the runs list. On June 29 we raised that complexity setting (` input_format_binary_max_type_complexity` ) to fix that symptom, which removed the protection. From that point, high-complexity runs started landing as parts that the hard-coded merges could never combine.


### How we recovered


There was no single switch. Three things happened over several hours.


First, we **vertically scaled** the database. That cleared the immediate memory ceiling and stopped the out-of-memory query failures, but it did nothing for the merges, which were failing on complexity, not memory.


Second, we **removed the un-mergeable parts** . ClickHouse lets you "detach" a part, which takes it out of the table without deleting it (the data stays on disk and can be reattached later). We identified the specific parts that could not be merged and detached them in batches. The events table, which had lower schema complexity, drained quickly. The runs table was slower, because much of its problem was the union-of-schemas issue that has no single part to remove.


Third, and we want to be honest that we cannot fully attribute the recovery to one action: around 15:35 UTC the runs table recovered sharply. At almost the same moment, the last problem parts cleared and a stuck database replica (which had been pinned alive by a long-running backup) finally terminated, releasing locks and freeing up merge capacity to run large consolidations. Part counts collapsed from over 1,100 to under 200 in a few minutes, and merge failures went from thousands per minute to zero.


Separately, ClickHouse support applied a service-level configuration change and a full service restart later in the afternoon (around 16:43 UTC), which is what they consider the durable fix on their side until their code fix ships. We had also, earlier in the day, asked them to raise the limit at the service level; that change did not help (because of the hard-coded merge behavior above) and we reverted it.


### The knock-on problems


A few things made this worse than it needed to be:


-


**The runs list query amplifies the part problem.** To return correct results it uses ClickHouse's` FINAL` modifier, which reconciles all of a table's parts at query time. The more parts pile up, the more memory each query needs, which slowed merges further, which created more parts. It was a feedback loop, and a single customer's high-frequency polling of the runs list API was enough to keep it spinning at the worst point.


-


**The alerting gap.** Our broader alerts did fire (our API success-rate alert went off alongside the first customer reports), but that is downstream of the real problem: it flags that something is wrong without pointing at the ClickHouse internals behind it. We weren't watching those internals (part counts, merge failure rates, database memory) directly, so the clearest evidence, a chart of dashboard latency creeping up from June 17, was something we put together during the incident. That direct alerting is going in.


-


**A managed upgrade we didn't control.** The trigger was an automatic version upgrade to a database we don't run ourselves. For a system this central to the dashboard, having that little control over it made the problem harder to get ahead of.


## What's next


We're sorry. Two incidents in just over a week isn't acceptable to us. After the last one we committed to communicating better and focusing harder on reliability, and this week tested both. We kept the status page updated and stayed consistent that execution was unaffected, though we were slower to get the public incident posted than we want to be. The underlying fragility is what we have to fix, and the plan above is how we're doing it.


These native` JSON` columns have been a recurring source of fragility, and the honest read is that storing arbitrary, wildly-shaped task output in an unbounded-schema column was a latent liability that the upgrade exposed. ClickHouse's setting change keeps the immediate problem away, and we're weighing how much to keep leaning on these columns at all. Alongside that, the alerting and read/write separation we're putting in place mean that if anything like this starts building again, we'll see it long before you do.


Thank you for your patience, and for trusting us with your work. We don't take it for granted.


- Eric
