---
schema_version: "1.0.0"
document_id: "c673c5bafbaaff7651876941dda2ce102d44e30596cdc4454a2827ea4c936489"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/dream-background-memory-consolidation-for-ai-agents"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T18:30:59.228146+00:00"
fetched_at: "2026-08-04T18:43:13.169222+00:00"
content_hash: "sha256:a5ce561215c8ccd930ae9482798c41c65fd7bccc74ae9d78a5ff311d55cd00db"
---

# Dream: Background memory consolidation for AI agents

*Mem0 now cleans up agent memory in the background. Duplicates get merged, outdated facts get marked as superseded without losing history, and recurring behavior gets summarized into higher-level memories. Available today on Pro and Enterprise plans.*


Human memory depends on sleep. The brain records all day and organizes at night: replays the day's experiences, strengthens the ones that matter, links them to older memories, and fades the rest. Long-running agents do the recording but not the organizing, and over time their memory starts to behave like a human mind with no sleep: recall gets noisier, contradictions accumulate, and old facts crowd out new ones. Dream gives agents the organizing phase, and does for an agent's memory what sleep does for ours.


### How agent memory degrades over time


Agents write memories in the middle of conversations. The write path has to be fast, so each write decision sees only the current conversation and a small set of similar memories retrieved for context. It doesn’t see the rest of the store.


This means some problems are invisible at write time, no matter how good the extraction is:


-


The same fact gets stored several times in different words. Exact-match deduplication catches identical text, but "prefers window seats" and "always books window seats on long-haul flights" are different strings.


-


A fact changes and the old version stays active. If "lives in Tokyo" was written four months ago and never comes up in the retrieval context when "moved to Osaka" is written, nothing connects them. Both stay in the store, and both come back in search results.


-


Related facts stay scattered. A user who mentions yoga classes in January, early wake-ups in March, and sleep questions in May has a routine that no single memory describes.


Stale and duplicate memories occupy retrieval slots, add tokens to every search response, and sometimes cause the agent to state something that stopped being true months ago. In our production data, the median active project carries a few hundred memories that duplicate or contradict other memories in the same project.


Databases solved a similar problem a long time ago by splitting the work: accept writes fast, and run a separate maintenance process in the background. LSM stores run compaction, Postgres runs vacuum, git runs garbage collection. Memory systems for agents have generally shipped the fast write path without the maintenance process.


Dream is the maintenance process.


### What Dream does


Dream runs three operations on your project's memories.


**Merge.** When a newer memory contains all the information of an older one plus more, the older memory is marked as` merged` and points to its replacement. Merged memories are hidden from search results by default. You can still fetch them with` include_merged=true` .


**Supersede.** When a newer memory replaces an older fact, the older memory is marked as` superseded` and points to the memory that replaced it. Superseded memories still appear in search by default, because history is often useful. Pass` latest_only=true` to get only current facts.


**Synthesize.** A background job looks at groups of related memories and writes a new summary memory when several independent observations support one. For example: a user with separate memories about Tuesday and Thursday yoga classes, 6:45 AM wake-ups, and questions about sleep quality gets one new memory describing the morning yoga routine, with the IDs of all source memories stored on it. Synthesis is deliberately conservative. Single observations, one-off events, and repeated questions about a topic do not produce summaries.


Nothing is deleted in any of these operations. Every change is recorded as a state change with a pointer to the newer memory, and you can review each change in the dashboard. Memories marked` immutable` or` exclude_from_dream` are skipped entirely.


### How it works


Merge and supersede decisions happen during extraction, when a new memory clearly relates to an existing one that is present in the write context. The rules are strict. A merge requires the new memory to preserve all the information in the old one. A supersede requires a clear replacement of the same fact. Anything ambiguous results in no action. The updates are applied with conditional writes, so concurrent operations on the same memory cannot conflict.


Synthesis runs separately, on a schedule, off the request path. It groups candidate memories by similarity and evaluates each group. The performance characteristics matter for production use. Add and search latency do not change: consolidation adds no work to either path.


Lifecycle states are stored as an indexed column on each memory, so` latest_only` filtering is a simple indexed query.


### Turning it on


Dream is available for all Pro and enterprise users. On the Pro plan, open the Dream tab in the dashboard and enable it per project. It processes new activity from the moment you enable it and does not touch your existing memories retroactively. Runs happen weekly per project per eligible user_ids.


In the SDKs (Python and TypeScript), search and get_all accept` latest_only` and` include_merged` , and memory objects now include` lifecycle_state` and` replacement_memory_id` .


Docs:[https://docs.mem0.ai/platform/features/dream](https://docs.mem0.ai/platform/features/dream)


Enable it from the Dream tab:[https://app.mem0.ai/dashboard/dream](https://app.mem0.ai/dashboard/dream)
