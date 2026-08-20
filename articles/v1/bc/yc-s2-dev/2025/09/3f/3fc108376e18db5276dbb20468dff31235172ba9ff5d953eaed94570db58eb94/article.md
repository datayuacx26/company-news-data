---
schema_version: "1.0.0"
document_id: "3fc108376e18db5276dbb20468dff31235172ba9ff5d953eaed94570db58eb94"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/durable-yjs-rooms"
published_at: "2025-09-02T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:263fe27ed4163a4aa60ff842bc0e4ab4090c7437d1538db37b2a2404238023e5"
---

# Behind y-s2: serverless multiplayer rooms

Real-time "multiplayer" collaboration powers some of the best web experiences we use every day — think Google Docs, Figma, Notion. Under the hood, these products solve hard distributed systems problems: keeping many users in sync even when edits arrive out of order, connections drop, or servers crash.


A common approach to building such a collaborative "room" is to have a designated coordinator that every client talks to, paired with a background worker that periodically takes the in-memory live state and persists a checkpoint to storage. However, if the coordinator crashes, the latest checkpoint becomes the only recoverable state. This could result in losing a significant amount of work!


To fix this issue of lost state, we can introduce a durable log or a journal into the mix, which is more optimal for incremental writes. So, even in case of a crash the recovery process can catch-up from the backlog in the journal which was not yet persisted as a checkpoint. As an example,[Figma took this approach](https://www.figma.com/blog/making-multiplayer-more-reliable/) to make their multiplayer editing more reliable.


In the open source world,[Yjs](https://yjs.dev/) is a foundational[CRDT](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type) framework that solves the hard problem of merging edits without conflicts. What it does leave open is how those edits are transported, stored, and replayed — but thankfully with pluggable backends!


Many Yjs backends are available, covering the gamut from[y-redis](https://github.com/yjs/y-redis) to[y-durableobjects](https://github.com/napolab/y-durableobjects) . And now, there is[y-s2](https://github.com/s2-streamstore/y-s2) — but, really, y S2?


## Durable streams for multiplayer


With S2, we took the humble log — the stream — and turned it into a first-class cloud storage primitive. Instead of working with entire objects, you interact at the granularity of records using a simple API akin to object storage:` Append` ,` Read` , and` Trim` on a **named Stream inside a Basin** . Every record is durably sequenced at the current "tail" of the stream, no matter how many writers are active.


This means an S2 stream can serve both as storage and reliable transport: you can not only follow live updates, but also replay history from any position.


These properties makes S2 a natural fit for collaborative systems — and pairing S2 with serverless functions like Cloudflare Workers makes it straightforward to separate client vs server-side concerns. This is the magical combination that allows` y-s2` to be a serverless yet durable Yjs backend.


To see it in action, head over to[this demo](https://s2.dev/demos/y-s2) and start a sharable collaborative editor.


Your browser does not support the video tag.


A client connects to a worker through a WebSocket connection, and in turn the worker establishes an SSE ([Server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events) ) session to receive live updates from the S2 stream. Every worker invocation effectively acts like a thread bound to a client: it reads from the log to propagate updates downstream, and appends any new updates from that client back to the stream.


When a client connects, the worker performs a catch-up to restore the latest state:


1. **Load the checkpoint:** Fetch the most recent snapshot from object storage ([R2](https://www.cloudflare.com/en-ca/developer-platform/products/r2/) in this case), which includes the last processed sequence number in its metadata.
2. **Replay recent updates:** Read all records from the S2 stream between the checkpoint and the[current tail](https://s2.dev/docs/api/records/check-tail) , applying them to rebuild the current state.


At this point, the worker synchronizes the materialized document with the Yjs client, and switches to live mode: streaming document updates in real-time.


Further, each worker reactively attempts to create checkpoints after buffering updates in memory until a threshold is reached. Since every update is already durably written to the S2 stream, the buffer is only an efficiency hack — it saves the worker from re-reading those same records when composing the snapshot. But if every invocation races to checkpoint at the same time, we waste resources! Additionally, checkpointing needs to be coupled with trimming the prefix of the stream up to that point — otherwise, the stream will grow unbounded even after data has been safely persisted.


How do we ensure that only one worker at a time is allowed to write a checkpoint and trim the log, while the rest continue serving clients?


## A distributed mutex?


Martin Kleppman discusses **fencing** in his excellent article,[how to do distributed locking](https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html) . As he posits, the store **needs** to be involved for this approach to be robust — and fencing is a[native capability](https://s2.dev/docs/api/records/append#concurrency-control) in S2.


A fencing token can be set on a stream by appending a special["command record"](https://s2.dev/docs/api/records/overview#command-records) . Fencing in S2 is cooperative: an append that does not specify a fencing token will still be allowed. However if an append does include a fencing token that does not match, this results in the request failing with a` 412 Precondition Failed` . We can leverage fencing in this way to give each worker invocation an opportunity to obtain a unique "lease", or a time-bounded license, to create a checkpoint.


Another enabler to the design is that a batch of records is always appended atomically to a stream, and explicitly trimming a stream is *also* a command record. This means resetting the fencing token and trimming the stream can be committed **together** when a checkpoint has been completed, ensuring state stays consistent.


To create a checkpoint, an invocation attempts to set a unique fencing token on the stream with a unique ID and deadline (` "{uuidInBase64} {deadlineEpochSecAsStr}"` ). The worker always provides its knowledge of the current fencing token, and this way if another worker won the race, the attempt will fail.


If a snapshot takes exceptionally long, or a worker invocation fails to release the lease due to a failure, the deadline acts as an auto-expiration that allows other invocations to attempt to grab the lease again. While it is possible that multiple invocations end up attempting a snapshot in this case, only one will manage to commit.


## Side quest: making workers easier to debug


In a distributed scenario like this where we have multiple invocations trying to coordinate over checkpoints, we wanted to see logs in two different ways:


1.


**Logs per invocation:** Useful for debugging issues specific to one worker's behavior — like understanding why a particular invocation failed to acquire a lease, how it processed its buffered records, etc.


2.


**Interleaved logs for all invocations:** To get a chronological, comprehensive view of logs from all worker invocations. This would help understand the system-wide coordination and timing between different workers — like seeing the sequence of lease acquisitions, which worker successfully set a fencing token, how race conditions play out in real-time, and the overall flow of the distributed checkpointing process across all active invocations.


It takes a while for worker logs to appear on the Cloudflare dashboard and even toggling into its "Live" mode, the logs lag and sometimes error out. When it does succeed, the total volume of events is[limited to only 256 KB](https://developers.cloudflare.com/changelog/2025-04-07-increase-trace-events-limit/) which is not enough for long-running sessions. Moreover, locally when using` wrangler tail` or` wrangler dev` , worker logs for WebSocket connections never seem to show up! The overall experience was not pleasant.


I found it much more ergonomic to use S2 as the log sink and tail logs in real-time to see how workers coordinate over checkpoints. For traceability, it was easy to name individual streams by their assigned Cloudflare Ray ID. S2 supports creating unlimited streams, so every invocation can have its own dedicated log for inspection.


And it is just as easy to interleave them on a shared stream:


```text
$   s2   read   s2://hello-world/logs/workers-shared
```


---


We are only scratching the surface of possibilities here. If you are curious about building with S2, experimenting with collaborative backends, or helping improve` y-s2` , join us on[Discord](https://discord.com/invite/vTCs7kMkAf) !


---


*A prior version of this post included an inaccurate cost comparison with Cloudflare Durable Objects that was not apples-to-apples.*
