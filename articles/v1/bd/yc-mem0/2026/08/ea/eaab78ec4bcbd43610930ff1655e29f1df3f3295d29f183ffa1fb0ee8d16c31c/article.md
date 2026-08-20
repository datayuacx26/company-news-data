---
schema_version: "1.0.0"
document_id: "eaab78ec4bcbd43610930ff1655e29f1df3f3295d29f183ffa1fb0ee8d16c31c"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/event-based-memory-systems-for-long-running-ai-agents"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T00:13:52.711845+00:00"
fetched_at: "2026-08-07T00:13:53.827066+00:00"
content_hash: "sha256:749a3beed0496ddf468e8c6068801fae3b440924900ae990edafeedb8dad8e86"
---

# Event-Based Memory Systems for Long-Running AI Agents

Our agents are built to run long conversations, but they are not built to remember each and everything we tell them. In the complete lifecycle of a conversation, an agent gets a stream of tool calls, failures, corrections, inputs from the user, some random text like “okay” or “this looks good,” and a lot more across sessions that can be hours or days apart.


Most memory layers are tuned for the wrong problem. They are built to pull reference or a fact out of a conversation. A background coding agent running for three days doesn't need "the user prefers dark mode." It needs to know that it already tried patching` auth.py` twice, that both times failed for the same reason, and that trying it a third time is not going to go any better. Task progress matters more than anything a chat-memory layer was built to extract.


This relates to event-based memory systems that back your long-running agents to learn in-depth, rather than just preferences. In this blog, we'll look at how[Mem0](https://mem0.ai/) supports an event-based memory system.


> **If this got you Interested?** **Get yourself a**[Mem0 API Key](https://app.mem0.ai/dashboard) **and let's dive deeper.**


## **What makes a memory event-based?**


Most memory work in agents is fact-based, whether it knows it or not, and that works till think is worth remembering. But it breaks down the moment what matters is something that happened, at a specific point, with a specific result, because a fact-store flattens "when" out of the picture entirely.


Event-based memory keeps the "when." Each unit of memory is a dated, situated record of one thing that actually occurred, a tool-call, an error, a correction, tied to the moment it happened and the task it happened inside. Cognitive science calls this episodic memory, and unlike a fact, an event doesn't get overwritten when the world changes; it gets superseded and kept as history, while whatever it taught the agent gets carried forward on its own.


## **Shape of one event**


The separation of fact vs event only works if an event has a consistent shape. Not a chat message, but a structured receipt of what actually happened. Here’s an example to understand this better:


```text
from   pydantic   import    BaseModel  ,    Field
from    typing import Optional, Dict, Any
from   datetime   import    datetime


class   AgentEvent (  BaseModel  )  :
event_id :    str
timestamp :    str   =  Field  (  default_factory  = lambda  :   datetime. utcnow  (  )  . isoformat  (  )  )
actor :    str                          #    e  . g  . ,    "coding_agent_v1"
event_type :    str                     #    e  . g  . ,    "tool_execution_failure"
task_id :    str
action :    str                         #    e  . g  . ,    "apply_git_patch"
payload :    Dict  [  str  ,    Any  ]             #  e  . g  . ,    {  "file"  :    "auth.py"  ,    "error"  :    "ImportError"  }
learned_constraint :    Optional  [  str  ]   #  e  . g  . ,    "auth.py requires pyjwt>=2.0"
```


```text
from   pydantic   import    BaseModel  ,    Field
from    typing import Optional, Dict, Any
from   datetime   import    datetime


class   AgentEvent (  BaseModel  )  :
event_id :    str
actor :    str                          #    e  . g  . ,    "coding_agent_v1"
task_id :    str
action :    str                         #    e  . g  . ,    "apply_git_patch"
```


```text
from   pydantic   import    BaseModel  ,    Field
from    typing import Optional, Dict, Any
from   datetime   import    datetime


class   AgentEvent (  BaseModel  )  :
event_id :    str
actor :    str                          #    e  . g  . ,    "coding_agent_v1"
task_id :    str
action :    str                         #    e  . g  . ,    "apply_git_patch"
```


In the above code snippet,


-


` timestamp` is when this was observed.


-


` actor` is who or what did this: agent, tool, or user.


-


` action` and` payload` are what was attempted and what actually came back.


-


` learned_constraint` is optional on purpose because most events don't produce one. But when one does, it's the durable fact worth carrying forward.


The event has to carry the result, and ideally the cause: we tried X, and it failed because of Y. That's the difference between a memory that prevents a repeated mistake and one that just documents the mistake happening twice.


Three patterns follow from this, and they're not steps in a sequence; they're three different jobs running at three different speeds: write every event as it happens, read past events before acting on one, and maintain the pile by periodically compacting it into something durable. Let's understand each pattern one by one:


## **Pattern 1 (write): Appending raw receipts**


One pattern for adding memories to[Mem0](https://mem0.ai/) is to append every event the instant it happens, exactly as it occurred, without any summarization or LLM pass. This makes for a lossless append, controlled by the` infer` parameter: setting` infer=False` stores the raw content as-is.


```text
# Save the raw execution receipt without LLM extraction latency
mem0  . add  (
messages  = [  {
"role"  :  "assistant"  ,
"content"  :  f"Failed running patch on   {  event  . payload  [  'file'  ]  }  :   {  event  . payload  [  'error'  ]  }  "
}  ]  ,
user_id  = event  . task_id  ,
metadata  = {
"category"  :  event  . event_type  ,
"action"  :  event  . action  ,
"is_active"  :  True  ,
}  ,
infer  = False  ,     # append the exact receipt, no extraction
)
```


```text
# Save the raw execution receipt without LLM extraction latency
mem0  . add  (
messages  = [  {
"role"  :  "assistant"  ,
}  ]  ,
user_id  = event  . task_id  ,
metadata  = {
"category"  :  event  . event_type  ,
"action"  :  event  . action  ,
"is_active"  :  True  ,
}  ,
infer  = False  ,     # append the exact receipt, no extraction
)
```


```text
# Save the raw execution receipt without LLM extraction latency
mem0  . add  (
messages  = [  {
"role"  :  "assistant"  ,
}  ]  ,
user_id  = event  . task_id  ,
metadata  = {
"category"  :  event  . event_type  ,
"action"  :  event  . action  ,
"is_active"  :  True  ,
}  ,
infer  = False  ,     # append the exact receipt, no extraction
)
```


This has to be fast, and it has to be lossless, because it's sitting directly in the agent's execution path, and we don't want a summarization pass in the critical section of a retry loop. The` is_active` parameter in the metadata is also a manual version of something Mem0's write path can also do for you automatically. It helps with a later contradiction to mark this receipt as outdated without deleting it.


## **Pattern 2 (read): Checking before you retry**


Before the agent repeats a risky action, it should check its own history for that specific action first. Here's the check, in about ten lines:


```text
def    check_past_failures  (  task_id  :  str  ,    file_path  :  str  )  :
# Query this task's history for the file, raw receipts if compaction
# hasn't run yet, or the compacted lesson once it has
past_events   =  mem0  . search  (
query  = f"failures on   {  file_path  }  "  ,
user_id  =  task_id  ,
)
# Return the extracted constraints to inject into the system prompt
return    [  e  [  "memory"  ]    for    e    in    past_events  ]
```


```text
def    check_past_failures  (  task_id  :  str  ,    file_path  :  str  )  :
# Query this task's history for the file, raw receipts if compaction
# hasn't run yet, or the compacted lesson once it has
past_events   =  mem0  . search  (
query  = f"failures on   {  file_path  }  "  ,
user_id  =  task_id  ,
)
# Return the extracted constraints to inject into the system prompt
return    [  e  [  "memory"  ]    for    e    in    past_events  ]
```


```text
def    check_past_failures  (  task_id  :  str  ,    file_path  :  str  )  :
# Query this task's history for the file, raw receipts if compaction
# hasn't run yet, or the compacted lesson once it has
past_events   =  mem0  . search  (
query  = f"failures on   {  file_path  }  "  ,
user_id  =  task_id  ,
)
# Return the extracted constraints to inject into the system prompt
return    [  e  [  "memory"  ]    for    e    in    past_events  ]
```


Notice it doesn't care whether Pattern 3 has run yet. Early on, before anything's been compacted, this reads Pattern 1's raw receipts directly. Later, once a pattern's recurred enough to get compacted, this reads the condensed lesson instead, same code, no changes needed.


That's the reason why the raw ledger exists in the first place. Without this check, an agent with a perfect memory and a retry loop behaves exactly like an agent with no memory at all, because nothing stops it from repeating what it already knows doesn't work. With it, the constraint discovered on attempt one,` auth.py requires pyjwt>=2.0` , gets read back and injected before attempt two even starts.


Think about what this actually does to the model's context. Without this filtering, a long session's raw tool output, stack traces, retries, and partial diffs can run into thousands of tokens by the time something fails a third time. With it, the same history collapses into a handful of active constraints:


```text
WITHOUT event memory (raw tail of the session, ~12,000 tokens):
[full stdout of attempt 1]   ...  [full stack trace]   ...  [diff]   ...
[full stdout of attempt 2]   ...  [full stack trace]   ...  [diff]


```


```text
WITHOUT event memory (raw tail of the session, ~12,000 tokens):
[full stdout of attempt 1]   ...  [full stack trace]   ...  [diff]   ...
[full stdout of attempt 2]   ...  [full stack trace]   ...  [diff]


```


```text
WITHOUT event memory (raw tail of the session, ~12,000 tokens):
[full stdout of attempt 1]   ...  [full stack trace]   ...  [diff]   ...
[full stdout of attempt 2]   ...  [full stack trace]   ...  [diff]


```


Here, one event memory makes the model re-read everything it already tried, using up about 12000 tokens. While the other just tells it what's true right now.


## **Pattern 3 (maintain): Compacting the ledger**


A raw log keeps everything, but re-reading all of it every time gets wasteful. So we often take the entries about one repeated problem and feed them back into Mem0, letting it summarize them into one clear takeaway instead of many separate ones. Here's what that looks like:


```text
# Background process running periodically over the event log
def    compact_session_events  (  task_id  :  str  ,    raw_event_logs  :  list  [  str  ]  )  :
# Let Mem0 infer the overarching lesson from the historical event stream
mem0  . add  (
messages  = raw_event_logs  ,
user_id  = task_id  ,
metadata  = {  "type"  :  "compacted_lesson"  }  ,
infer  = True  ,     # LLM extracts the persistent, generalized pattern
)
```


```text
# Background process running periodically over the event log
# Let Mem0 infer the overarching lesson from the historical event stream
mem0  . add  (
messages  = raw_event_logs  ,
user_id  = task_id  ,
metadata  = {  "type"  :  "compacted_lesson"  }  ,
infer  = True  ,     # LLM extracts the persistent, generalized pattern
)
```


```text
# Background process running periodically over the event log
# Let Mem0 infer the overarching lesson from the historical event stream
mem0  . add  (
messages  = raw_event_logs  ,
user_id  = task_id  ,
metadata  = {  "type"  :  "compacted_lesson"  }  ,
infer  = True  ,     # LLM extracts the persistent, generalized pattern
)
```


This isn't about` infer=False` vs` infer=True` being better. It's an architecture where both are doing a job the other can't. The raw log (ledger) is cheap, immediate, and never loses anything. While the compaction pass is expensive, periodic, and it's the only place a recurring pattern actually gets to mean something instead of sitting there as five separate dated failures.


Mem0's extraction reads the whole batch and adds one condensed memory back, rather than touching the raw receipts underneath it. That's what turns five dated` ImportError` receipts into one current constraint worth searching for, without the raw history ever getting rewritten.


## **Handling state drift**


A lesson learned once doesn't get to stay true forever, and this is the part a single conversation never really has to deal with. The constraint that was true last week quietly stops being true, and none of that shows up as a new chat message, only as a different outcome the next time the agent tries the thing it used to avoid.


Trusting the old lesson forever is the wrong move. But deleting it the second new evidence shows up is also wrong, because that erases the exact audit trail you'd want if the new evidence turns out to be wrong instead. What you actually want to do is mark the old belief as outdated, but kept and dated, while the new one takes over as current.


Mem0 already has a feature built for this, called[Dream](https://mem0.ai/blog/dream-background-memory-consolidation-for-ai-agents) . **Supersede** runs automatically on the write path; when a new memory contradicts an existing one, the old one gets marked outdated instead of being deleted. On the read side,` latest_only=True` returns only the current, non-superseded memory:


```text
# Only the current constraint, old ones filtered out automatically
current   =  mem0  . search  (
query  = "dependency constraints on auth.py"  ,
user_id  = task_id  ,
latest_only  = True  ,
)


# Everything on file, including anything already superseded
full_history   =  mem0  . search  (
query  = "dependency constraints on auth.py"  ,
user_id  = task_id  ,
latest_only  = False  ,
)
```


```text
# Only the current constraint, old ones filtered out automatically
current   =  mem0  . search  (
query  = "dependency constraints on auth.py"  ,
user_id  = task_id  ,
latest_only  = True  ,
)


# Everything on file, including anything already superseded
full_history   =  mem0  . search  (
query  = "dependency constraints on auth.py"  ,
user_id  = task_id  ,
latest_only  = False  ,
)
```


```text
# Only the current constraint, old ones filtered out automatically
current   =  mem0  . search  (
query  = "dependency constraints on auth.py"  ,
user_id  = task_id  ,
latest_only  = True  ,
)


# Everything on file, including anything already superseded
full_history   =  mem0  . search  (
query  = "dependency constraints on auth.py"  ,
user_id  = task_id  ,
latest_only  = False  ,
)
```


Dream also has a **Synthesis** mode that does something close to Pattern 3's compaction automatically. Good to know it exists, and good to know when it doesn't fit: it needs a Pro plan or higher, runs as a background job with up to a day of delay instead of on demand, and only works on memories scoped to a bare` user_id` , so anything scoped to an` agent_id` or` run_id` falls outside it. That's exactly why Pattern 3 is worth knowing how to do yourself, same idea, on your own schedule, with scoping you control.


> If you want to explore Dream demo, I’d suggest giving our this[blog](https://mem0.ai/blog/stale-ai-agent-memory-and-how-mem0-dream-fixes-it) a read.


## **Key takeaways**


Every arrow in the above diagram is one of the patterns above: write, read, and maintain. The ledger is durable, the compaction pass can run on any schedule, and the read path only ever asks for what's current.


A twenty-minute chat doesn't build up enough repeated failures for a pattern to matter, and it doesn't run long enough for the world underneath it to change. An agent that's alive for days does both, again and again, for as long as it keeps running.


Get this right and a long-running agent's memory actually compounds, every recurring failure turns into a constraint instead of a repeated mistake, and every constraint stays open to new evidence instead of hardening into something the agent trusts long after it's stopped being true. Get it wrong, and you get the opposite: the agent that's been running the longest ends up the one with the most confidently wrong beliefs, which is a strange thing to build on purpose and a very easy thing to build by accident.


### **Further reading**


-


[Mem0's infer=True vs infer=False](https://mem0.ai/blog/mem0-s-infer-true-vs-infer-false-the-fact-your-ai-support-bot-forgot)


-


[Semantic Memory for AI Agents](https://mem0.ai/blog/semantic-memory-for-ai-agents)


-


[Long-Term Memory for AI Agents: The What, Why and How](https://mem0.ai/blog/long-term-memory-ai-agents)


-


[AI Memory Management for LLMs and Agents](https://mem0.ai/blog/ai-memory-management-for-llms-and-agents)


Mem0 is an intelligent, open-source memory layer designed for LLMs and AI agents to provide long-term, personalized, and context-aware interactions across sessions.


Get your free API Key here:[app.mem0.ai](https://app.mem0.ai/)


Sign up using[app.mem0.ai](https://app.mem0.ai/) or self-host Mem0 from our open-source[GitHub repository](https://github.com/mem0ai/mem0) .


## **Frequently asked questions**


### Q. Why not just use a bigger context window instead of building this?


The problem was never about running out of space; it's what happens once everything fits. A model reasoning over a full session's raw tool output, retries and stack traces included, ends up re-deriving the same conclusion it already reached last time, at full token cost, every single time. This architecture isn't about fitting more in; it's about only putting in what's actually still true.


### Q. Why does the outcome matter more than the action in an event record?


The action alone doesn't tell the agent anything it can act on. Knowing "applied the patch" doesn't stop a second, identical attempt. Knowing "applied the patch and it failed with an ImportError because the pinned dependency is too old" does. The outcome, and ideally the cause, is what turns a log entry into something worth changing behavior over.


### Q. What decides when a recurring failure is worth compacting instead of staying a pile of separate receipts?


That's a judgment call; tune it to how expensive a wasted retry actually is in your system. A cheap, idempotent action can tolerate a few blind repeats before it's worth the compaction pass. An action with real side effects- a deploy, a payment, a destructive migration- should compact on the first failure, not the third.


### Q. What stops Supersede from getting stuck on an outdated constraint?


Supersede resolves a contradiction the moment new evidence shows up; it doesn't go looking for contradictions on its own. If a constraint could plausibly go stale, build in an occasional re-validation, a periodic retry of the thing the constraint told you to avoid, so new evidence actually gets the chance to arrive.


### Q. Does this only apply to coding agents and background tasks?


No, though that's where it's most obviously necessary. Any agent whose job spans more than one short session- a support agent tracking an open ticket across days, a research agent running the same kind of query repeatedly- faces the same shape of problem: raw events accumulate, some of them repeat, and the ones that repeat need to turn into something the agent actually uses, not just something it stored.
