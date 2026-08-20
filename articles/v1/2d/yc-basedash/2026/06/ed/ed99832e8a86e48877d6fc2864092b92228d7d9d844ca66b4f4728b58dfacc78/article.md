---
schema_version: "1.0.0"
document_id: "ed99832e8a86e48877d6fc2864092b92228d7d9d844ca66b4f4728b58dfacc78"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/what-we-learned-running-background-jobs-on-postgres/"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T22:07:09.326483+00:00"
content_hash: "sha256:b7d0535074fa9d6d028c160c5bd99dc4134dda8bd5e5d9784667b492b6e7d23e"
---

# What we learned running background jobs on Postgres

Almost every async thing Basedash does runs through a job queue. AI chat responses, automations, the hourly insights we generate per org, database syncs, billing rollups to Stripe. All of it.


We don’t run Redis or SQS for this. We run[pg-boss](https://github.com/timgit/pg-boss) , which is a job queue built on Postgres, using` SELECT ... FOR UPDATE SKIP LOCKED` for delivery. The pitch is simple: one fewer system to operate, and the queue is in the same database as the data the jobs touch, so enqueuing a job and writing a row can share a transaction.


We still like that trade. But Postgres-as-a-queue has sharp edges, and over the last few months we hit three of them in production. None of them threw the kind of error that pages you at 2am. They just did the wrong thing until we looked at the missing work.


Here’s what broke, and what we changed.


## Incident 1: a queue policy that dropped 92% of our jobs


Our insights feature generates a daily digest for each org. A cron fires hourly, finds the orgs due in that hour, and fans out one` generate-insight-post` job per org.


One day we noticed an org hadn’t received a single insight in weeks. Then we looked closer and it wasn’t just them. Most orgs in any given hourly cohort were getting nothing. Only one org per hour came through.


The jobs weren’t failing. They were never landing.


The cause was the queue’s pg-boss policy. We’d set` generate-insight-post` to the` stately` policy, which is a singleton variant. Without a` singletonKey` ,` stately` allows exactly **one job in the` created` state at a time** . The scheduler emits ~13 jobs in a cohort with` Promise.all` , the first` send()` creates its row, and every subsequent` send()` returns` null` .


That return value is the whole story. pg-boss doesn’t throw when a policy rejects an enqueue. It hands you back` null` and moves on, and we weren’t checking it.


```text
const   jobId   =   await   getJob  (generateInsightPostJob).  emit  (payload, options);
// jobId === null  →  the job was silently rejected, not enqueued
```


The reason it surfaced when it did is the part I find most annoying. The queue had used` stately` for ages without trouble, because every job used to be emitted with` startAfter` of about 1 minute. The first job moved from` created` to` active` almost immediately, which freed the singleton slot before the next` send()` ran. Classic race that happens to resolve in your favor.


Then we added staggering, spreading each cohort’s` startAfter` across the full hour to smooth out load. Now the first job sat in` created` for up to 60 minutes, holding the slot the entire time. Every other org in the cohort got dropped. The bug had been there all along, and staggering just held the slot long enough to expose it.


The fix had two parts. First, we made the drop loud.` BaseJob` now inspects the enqueue result and emits a` job.enqueue_dropped` Sentry counter plus a warning log whenever` send()` comes back` null` :


```text
if   (result   ===   null  ) {
// pg-boss silently rejected the job (singleton constraint, queue policy).
// Track it so we can alert on unexpected drops.
Sentry.metrics.  count  (  'job.enqueue_dropped'  ,   1  , { attributes });
logger.  warn  ({ msg:   'Job enqueue dropped'  ,   ...  attributes });
}
```


Second, we stopped relying on a queue-wide singleton for dedup. Each insight emit now carries` singletonKey: organizationId` , so the “one at a time” guarantee is scoped per org instead of per queue. Duplicate posts were already prevented by a deterministic job ID plus a Prisma unique-constraint guard, so the global singleton was redundant anyway. The per-org` dropped` count now shows up in our logs on every fan-out, which means we’ll see the next regression in minutes instead of weeks.


## Incident 2: batch jobs starving the people actually waiting


All chat-assistant generation ran through a single queue,` generate-chat-assistant-response` , at concurrency 10. That one queue served two very different workloads.


One is someone sitting in the app who just sent a message and is watching for the first token. The other is an automation fanning out across hundreds of orgs at the top of the hour, where nobody is watching anything in real time.


With one shared queue, a big scheduled fan-out could grab all 10 slots. A user who sent a message right then sat behind a wall of batch work, waiting seconds for a worker to come free. Same priority, same pool, very different expectations.


We split it into two queues backed by the exact same workflow:


- ` generate-chat-assistant-response-ui` , concurrency **6** , for user-initiated work
- ` generate-chat-assistant-response-automation` , concurrency **4** , for scheduled and data-change automations


“UI” here means user-initiated, not the web UI specifically. Anyone waiting on a live response counts, so Slack mentions and approval resumptions ride the UI pool too. Only fully automated work goes on the automation pool. Total concurrency per replica stays at 10, now split 6/4. Same capacity, just partitioned so a batch fan-out can’t eat the share a live request needs.


The risk with two queues is drift. If a new source gets routed to the wrong pool, you’ve recreated the problem. So the source-to-queue mapping lives in exactly one place, and every emit goes through one helper:


```text
export   function   chatAssistantQueueForSource  (
source  :   ChatAssistantSource  ,
)  :   'ui'   |   'automation'   {
return   AUTOMATION_QUEUE_SOURCES  .  has  (source)   ?   'automation'   :   'ui'  ;
}


export   async   function   emitChatAssistantResponseJob  (  payload  ,   options   =   {}) {
const   target   =
chatAssistantQueueForSource  (payload.source)   ===   'automation'
?   generateChatAssistantResponseAutomationJob
:   generateChatAssistantResponseUiJob;
return   getJob  (target).  emit  (payload, options);
}
```


There’s a real gotcha when you rename a live queue, though. During a rolling deploy, old web replicas still emit to the old queue name while new worker replicas only listen on the new` -ui` and` -automation` queues. Any job that lands on the old queue in that window has no consumer.


We already had a safety net for this: a 90-second heartbeat watcher that re-emits any chat message stuck in the` GENERATING` state. During the cutover it re-routed orphaned jobs onto the new queues within a minute and a half, so the worst case was a short delay, not a lost response.


## Incident 3: the connection pool that gave out every hour


pg-boss talks to Postgres through a connection pool, and each replica gets its own. We had` max: 5` .


At the top of most hours we’d see a cluster of these in Sentry:


```text
DatabaseConnectionError: Connection terminated due to connection timeout
```


Always at cron time, never in between. The cause was the same fan-out pattern from incident 1, viewed from the pool’s side. Firing N concurrent` send()` calls means N concurrent attempts to check out a connection, and against a pool of 5 the rest queue up until they time out.


We built a small harness to size the pool against a 200-job burst instead of guessing:


pool max acquire timeouts in a 200-job run


5 56


10 0


15 0


10 got us off the cliff with headroom to spare. Our peak connection usage is around 40 against a cap of 397, so there’s plenty of room, and 15 bought us nothing over 10.


Bumping the pool treats the symptom, though. The deeper fix is to stop firing N concurrent enqueues in the first place. For fan-outs we added an` emitMany` path that does a single bulk` pgBoss.insert()` per target queue instead of a` send()` per job:


```text
// Before: N concurrent send() calls racing a pool of 5
await   Promise  .  all  (orgs.  map  ((  org  )   =>   job.  emit  (  buildPayload  (org))));


// After: one bulk insert per queue, one connection
await   job.  emitMany  (orgs.  map  ((  org  )   =>   ({ payload:   buildPayload  (org) })));
```


One trade comes with it.` insert()` resolves to` void` , so unlike the single-job path we can’t see which rows got skipped by a` singletonKey` collision (Postgres handles those with` ON CONFLICT DO NOTHING` ). So on the bulk path our “enqueued” count is an upper bound, not an exact total, and` job.enqueue_dropped` only fires on the per-job path. We decided that’s an acceptable blind spot for fan-outs, as long as we’re honest about it in the metric’s definition.


## What we’d tell past us


A few things we’ll carry into the next queue we build, on Postgres or anywhere else.


1. **Check what your enqueue returns.** pg-boss signals a rejected job with a` null` return, not an exception. If you` await job.emit(...)` and ignore the result, a dropped job looks identical to a successful one. The same trap exists in plenty of queue clients.
2. **Read the policy docs before you pick a policy.**` stately` and` singleton` without a` singletonKey` mean one job at a time across the whole queue, which is almost never what a fan-out wants. Scope dedup with a key that matches your real uniqueness (for us, the org).
3. **Keep interactive and batch work in separate lanes.** Same code is fine, same queue is not. Different queues with their own concurrency budgets stop a batch fan-out from parking the slot a live request needs.
4. **Size pools against your burstiest second, not your average.** Everything looked healthy on the minute-level dashboards. The pain lived in the 200ms after each cron tick. And prefer one bulk insert over N concurrent sends when you can.
5. **Build the metric that would have caught it.** Every one of these was invisible until we added` job.enqueue_dropped` , per-org emit logs, and a stagger-delay distribution. The fix took an afternoon; noticing was the hard part.


If you like this kind of measure-then-fix writeup, we have a couple more in the same shape:[what was killing our healthy Kubernetes pods](https://www.basedash.com/blog/what-was-killing-our-healthy-kubernetes-pods) and[how virtualization sped up our tables by 4 to 5x](https://www.basedash.com/blog/how-virtualization-increased-our-table-performance-by-500) .


And if you want to see what all these background jobs actually power,[Basedash](https://www.basedash.com/) is an AI-native BI platform that turns your database into dashboards and answers without anyone writing SQL.
