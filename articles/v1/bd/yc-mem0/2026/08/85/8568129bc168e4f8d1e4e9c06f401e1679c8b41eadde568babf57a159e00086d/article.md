---
schema_version: "1.0.0"
document_id: "8568129bc168e4f8d1e4e9c06f401e1679c8b41eadde568babf57a159e00086d"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/semantic-vs-episodic-vs-procedural-memory-in-ai-agents-a-complete-comparison"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T01:33:50.372312+00:00"
fetched_at: "2026-08-18T01:33:52.159736+00:00"
content_hash: "sha256:1ddca1785c885d842aff5f06207c7158964456e5f848ea8a4bf83608f3315641"
---

# Semantic vs Episodic vs Procedural Memory in AI Agents: A Complete Comparison

An agent that treats memory as one flat bucket is guaranteed to serve every kind of memory with the same undifferentiated mechanism. That's not a guess; it's what happens mechanically once you actually separate all the major kinds of memories out: a stated preference, a specific thing that happened last Tuesday, and a five-step routine that works, don't behave the same, don't fail the same way, and don't deserve the same storage or retrieval rules. If you've been searching for semantic vs episodic vs procedural memory AI comparisons, looking for the practical difference rather than the academic one, here it is: it's the difference between a memory system that serves each job well and one flat store that quietly underperforms at all three.


## **What is semantic memory?**


Semantic memory is general knowledge, detached from when or where it was learned: "the user prefers dark mode," "this service talks to Postgres," "the developer uses four-space indentation." It's the closest thing to a durable profile an agent builds of a user or a project. Most of what teams build first, and often mistake for the whole of agent memory, is semantic.


It's stored as a fact, retrieved by relevance, and rarely needs to expire. Its characteristic failure is the missing clock: a fact that was true once and is silently wrong now, because semantic storage tempts you to drop the time dimension entirely.


> **If this got you Interested? Get yourself a**[Mem0 API Key](https://app.mem0.ai/dashboard) **and let's try it out.**


```text
from    mem0    import    MemoryClient


mem   =  MemoryClient  (  api_key  = "your-api-key"  )


mem  . add  (
"The user prefers dark mode in the editor and works primarily in Python."  ,
user_id  = "dev_123"  ,
)


results   =  mem  . search  (  query  = "What are the user's editor preferences?"  ,    filters  = {  "user_id"  :  "dev_123"  }  )
```


```text
from    mem0    import    MemoryClient


mem   =  MemoryClient  (  api_key  = "your-api-key"  )


mem  . add  (
"The user prefers dark mode in the editor and works primarily in Python."  ,
user_id  = "dev_123"  ,
)


```


```text
from    mem0    import    MemoryClient


mem   =  MemoryClient  (  api_key  = "your-api-key"  )


mem  . add  (
"The user prefers dark mode in the editor and works primarily in Python."  ,
user_id  = "dev_123"  ,
)


```


Nothing exotic here, this is just Mem0's default` add()` /` search()` . That's the point: semantic memory is the baseline case Mem0's extraction is built around; no special parameter needed.


## **What is episodic memory?**


Episodic memory is a specific past experience, dated and situated: "on Tuesday, mocking the network failed to fix that test." It's stored as an event with a timestamp and retrieved by similarity to the current situation and by recency. It earns its place whenever an agent must not repeat itself, because the value lives in the particular, dated occurrence, not a generalized fact.


Its characteristic failure is keeping the fact but dropping the outcome: "tried mocking the network" survives, "it didn't work" doesn't, and the agent tries the same failed fix again.


```text
mem  . add  (
"Attempted to fix the flaky test by mocking the network call. It did not resolve the failure."  ,
user_id  = "dev_123"  ,
run_id  = "debug-session-2026-08-11"  ,
metadata  = {  "type"  :  "event"  ,    "outcome"  :  "failed"  }  ,
)


results   =  mem  . search  (
query  = "What have we already tried for the flaky test?"  ,
filters  = {  "AND"  :  [  {  "user_id"  :  "dev_123"  }  ,    {  "run_id"  :  "debug-session-2026-08-11"  }  ]  }  ,
)
```


```text
mem  . add  (
user_id  = "dev_123"  ,
run_id  = "debug-session-2026-08-11"  ,
metadata  = {  "type"  :  "event"  ,    "outcome"  :  "failed"  }  ,
)


results   =  mem  . search  (
query  = "What have we already tried for the flaky test?"  ,
)
```


```text
mem  . add  (
user_id  = "dev_123"  ,
run_id  = "debug-session-2026-08-11"  ,
metadata  = {  "type"  :  "event"  ,    "outcome"  :  "failed"  }  ,
)


results   =  mem  . search  (
query  = "What have we already tried for the flaky test?"  ,
)
```


Same` add()` /` search()` mechanism as semantic memory, the thing that actually makes this episodic is how you use it: a` run_id` scoping it to a specific session, and metadata that captures the outcome, not just the action.


## **What is procedural memory?**


Procedural memory is a reusable skill or routine: a five-step deploy, a debugging checklist that consistently works. Done well, it's stored as a callable procedure and improved over time rather than merely recalled. It earns its place when the same multi-step job recurs and should get better with repetition, not just get remembered.


Its characteristic failure is a skill that outlives the world it encoded: a routine that keeps executing after the pipeline it was built for has changed, because it got reused without ever being revalidated.


```text
# a senior dev teaches the procedure once, scoped to the agent, not to themselves
mem  . add  (
[  {  "role"  :  "user"  ,    "content"  :  "When we add a new REST endpoint, we always: "
"1. Add the route. 2. Write the handler. 3. Register it. "
"4. Add a unit test. 5. Update the OpenAPI spec. Never skip the test or the spec."  }  ]  ,
agent_id  = "coding-agent"  ,
infer  = False  ,
)


# a different developer, months later, gets the taught convention without being told
hits   =  mem  . search  (
query  = "checklist for adding a REST API endpoint"  ,
filters  = {  "agent_id"  :  "coding-agent"  }  ,
)
```


```text
mem  . add  (
"1. Add the route. 2. Write the handler. 3. Register it. "
agent_id  = "coding-agent"  ,
infer  = False  ,
)


hits   =  mem  . search  (
query  = "checklist for adding a REST API endpoint"  ,
filters  = {  "agent_id"  :  "coding-agent"  }  ,
)
```


```text
mem  . add  (
"1. Add the route. 2. Write the handler. 3. Register it. "
agent_id  = "coding-agent"  ,
infer  = False  ,
)


hits   =  mem  . search  (
query  = "checklist for adding a REST API endpoint"  ,
filters  = {  "agent_id"  :  "coding-agent"  }  ,
)
```


Here's all three run back to back against a real hosted Mem0 account:


```text
$  python    three_types_demo  . py


===============
SEMANTIC    MEMORY
===============
-  User   prefers dark mode  in    the   editor
-  User   works primarily  in    Python


===============
EPISODIC   MEMORY
===============
-  User   attempted to fix a flaky test by mocking the network call ,    but   the failure persisted |  outcome  :  failed


=================
PROCEDURAL    MEMORY
=================
-  When   we add a new REST endpoint ,    we   always:  1  .  Add    the    route  .  2  .  Write    the    handler  .  3  .  Register    it  .  4  .  Add    a    unit    test  . 5.  Update    the    OpenAPI    spec  .  Never    skip    the    test    or    the    spec


```


```text
$  python    three_types_demo  . py


===============
SEMANTIC    MEMORY
===============
-  User   prefers dark mode  in    the   editor
-  User   works primarily  in    Python


===============
EPISODIC   MEMORY
===============


=================
PROCEDURAL    MEMORY
=================


```


```text
$  python    three_types_demo  . py


===============
SEMANTIC    MEMORY
===============
-  User   prefers dark mode  in    the   editor
-  User   works primarily  in    Python


===============
EPISODIC   MEMORY
===============


=================
PROCEDURAL    MEMORY
=================


```


The semantic and episodic memories were rewritten and split by Mem0's extraction, that's` infer=True` , the default, pulling out the facts it thinks matter rather than storing your sentence verbatim. The procedural memory came back exactly as written, because` infer=False` tells Mem0 to skip extraction and store the input as-is, which is exactly what you want for a routine; you don't want a checklist quietly summarized.


## **Similarities across all three**


None of these three types has its own dedicated parameter in Mem0: not semantic, not episodic, not procedural. All three are the same` add()` /` search()` primitives underneath. What actually makes one "procedural" instead of "semantic" is which scope key you use and how you use it:` user_id` for a personal fact,` run_id` plus outcome metadata for a dated event,` agent_id` for a routine that belongs to the job, not to whoever taught it. That last part matters more than it looks, scoping a procedure to` agent_id` instead of` user_id` is exactly what lets one developer teach a convention once and a different developer benefit from it later without being told, since the memory belongs to the agent doing the work, not the person who happened to explain it.


## **Semantic vs episodic vs procedural memory**


**Semantic**


**Episodic**


**Procedural**


**Stores**


General facts, detached from time


A specific, dated experience


A reusable skill or routine


**Example**


"Prefers dark mode"


"Mocking the network failed on Tuesday"


"The five-step deploy process"


**Retrieved by**


Relevance


Similarity plus recency


Direct call, by task


**Characteristic failure**


Missing clock, stale but confidently stated


Fact kept, outcome dropped, mistake repeats


Outdated routine, reused without revalidation


**Mem0 mechanism**


` add()` scoped with` user_id`


` add()` scoped with` run_id` and outcome metadata


` add()` scoped with` agent_id` ,` infer=False`


## **Why an agent needs all three, not just one**


Match the type to the behavior you actually want to improve. A stable preference is semantic. A failure to avoid repeating is episodic. A routine to reuse and refine is procedural. Reach for only one of these, and the other two behaviors don't disappear; they just get served badly by a mechanism that was never built for them: preferences that decay because nothing marks them as durable, mistakes that repeat because only the fact survived and not the outcome, or skills that never improve because they're stored the same way as a one-time note.


## **Wrapping up**


Semantic vs episodic vs procedural memory in AI agents comes down to three different questions a memory has to answer: what's true, what happened, and how to do something. Mem0 doesn't hand you three separate features for these; it hands you one set of primitives,` add()` ,` search()` ,` user_id` ,` run_id` ,` agent_id` ,` metadata` , flexible enough that the same mechanism cleanly serves all three jobs once you scope it deliberately, instead of one flat store trying to be all three at once.


## **Further Reading**


-


[Semantic Memory for AI Agents](https://mem0.ai/blog/semantic-memory-for-ai-agents)


-


[Episodic memory for AI agents](https://mem0.ai/blog/episodic-memory-for-ai-agents)


-


[Procedural Memory Explained: Teaching AI Agents How to Perform Tasks](https://mem0.ai/blog/procedural-memory-explained-teaching-ai-agents-how-to-perform-tasks)


-


[Long-Term Memory for AI Agents: The What, Why and How](https://mem0.ai/blog/long-term-memory-ai-agents)


—


[Mem0](https://mem0.ai/) is an intelligent, open-source memory layer designed for LLMs and AI agents to provide long-term, personalized, and context-aware interactions across sessions.*


Get your free API Key here:[app.mem0.ai](https://app.mem0.ai/?utm_source=mem0_blog&utm_medium=kw_blog&utm_campaign=how_to_build_a_code_review_agent_using_mem0&utm_content=how_to_build_a_code_review_agent_using_mem0) or


Self-host mem0 from our[open-source GitHub repository](https://github.com/mem0ai/mem0) .


—


## **Frequently Asked Questions**


**Q. Semantic memory vs procedural memory: what's the difference?**


Semantic memory stores what's true, general facts and preferences detached from when they were learned. Procedural memory stores how to do something, a reusable skill or routine that should improve with repetition. A user's dark mode preference is semantic; a five-step deploy checklist is procedural.


**Q. What is semantic vs episodic vs procedural memory in AI agents?**


Semantic memory answers "what's true." Episodic memory answers "what happened, when, and what was the outcome." Procedural memory answers "how do I do this?" All three are stored and retrieved differently, and an agent that only has one of them fails at the jobs the other two are meant for.


**Q. Does an AI agent need all three memory types?**


Yes, if it needs to be reliable across sessions. Skipping semantic memory means restating preferences every time. Skipping episodic memory means repeating mistakes because only the fact survived, not the outcome. Skipping procedural memory means never getting faster at a recurring task.


**Q. Does Mem0 have a built-in parameter for each memory type?**


No, none of the three has its own dedicated parameter. All three are Mem0's same` add()` /` search()` primitives, differentiated by convention:` user_id` scoping for a personal fact (semantic),` run_id` scoping plus outcome metadata for a dated event (episodic), and` agent_id` scoping with` infer=False` for a routine that belongs to the job rather than the person who taught it (procedural).


**Q. What's the most common mistake teams make with agent memory types?**


Treating memory as one flat bucket. It's the single most common way memory underdelivers: the type with the strictest requirement, a preference that must never expire, a mistake that must not repeat, a skill that must keep working, ends up served by the loosest common denominator, and the failure doesn't show up loudly; the agent just gets quietly worse over time.
