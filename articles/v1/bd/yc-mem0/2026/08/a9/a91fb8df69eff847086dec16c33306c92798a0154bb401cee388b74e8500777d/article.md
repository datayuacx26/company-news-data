---
schema_version: "1.0.0"
document_id: "a91fb8df69eff847086dec16c33306c92798a0154bb401cee388b74e8500777d"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/structured-vs-unstructured-memory-in-ai-agents-explained"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T14:51:27.024152+00:00"
fetched_at: "2026-08-18T14:51:29.486904+00:00"
content_hash: "sha256:ef5ffcb43bb2920421b127a6b0f6f10d93cc845e76097976cef7830093a879e6"
---

# Structured vs Unstructured Memory in AI Agents Explained

Say a developer stores "the user prefers window seats" as a memory. A second developer stores the same fact as` {"seat_preference": "window", "confidence": 0.9}` . That’s the same two facts stored in different formats.


One is easy to write and easy to read back as a sentence. The other is easy to filter, aggregate, and validate, but someone had to decide the fields ahead of time. Structured vs unstructured AI memory isn't a preference; it's a tradeoff between how cheap a memory is to capture and how much you can do with it once it's stored, and most production agents end up needing both, on purpose.


## **What is unstructured memory?**


Unstructured memory is a memory stored as natural language, a sentence or a passage, with no predefined fields. "The user prefers dark mode and is working on a Python project" is unstructured; there's no` theme` field, no` language` field, just text that an embedding model turns into something searchable by semantics.


Nothing has to be decided in advance; whatever the conversation produces gets captured, and retrieval works by similarity: ask a related question later and the right memory surfaces even if the wording doesn't match exactly. The cost shows up downstream, not at write time. You can't ask an unstructured store, "Give me everyone whose seat preference is window." You can only ask it to find text that resembles a query, and resemblance isn't the same as an exact match on a field.


> **If this got you Interested? Get yourself a**[Mem0 API Key](https://app.mem0.ai/dashboard) **and let's try it out.**


```text
from    mem0    import    MemoryClient


mem   =  MemoryClient  (  api_key  = "your-api-key"  )


mem  . add  (
"The user prefers window seats and usually flies economy."  ,
user_id  = "traveler_42"  ,
)


results   =  mem  . search  (  query  = "What are this user's flight preferences?"  ,    filters  = {  "user_id"  :  "traveler_42"  }  )
```


```text
from    mem0    import    MemoryClient


mem   =  MemoryClient  (  api_key  = "your-api-key"  )


mem  . add  (
"The user prefers window seats and usually flies economy."  ,
user_id  = "traveler_42"  ,
)


```


```text
from    mem0    import    MemoryClient


mem   =  MemoryClient  (  api_key  = "your-api-key"  )


mem  . add  (
"The user prefers window seats and usually flies economy."  ,
user_id  = "traveler_42"  ,
)


```


Mem0's extraction turns that sentence into a stored fact automatically, this is the same` add()` /` search()` you'd use for any memory, no special setup for "unstructured" because unstructured is the baseline everything else builds on.


## **What is structured memory?**


Structured memory is a memory with a defined shape, specific fields with specific types, the kind of thing you'd put in a database column:` {"seat_preference": "window", "cabin_class": "economy", "confidence": 0.9}` . It's built for exactly the query an unstructured store can't answer cleanly: filter on a field, aggregate across records, validate that a value is one of a known set.


Someone has to decide the fields before the data exists, and a fact that doesn't fit the schema either gets dropped or forced into a field it doesn't quite belong in. Rigid structure also breaks the moment reality doesn't match the shape you picked; a new kind of preference nobody planned for either needs a schema migration or gets silently lost.


```text
mem  . add  (
"The user's flight booking preference."  ,
user_id  = "traveler_42"  ,
metadata  = {  "seat_preference"  :  "window"  ,    "cabin_class"  :  "economy"  ,    "confidence"  :  0.9  }  ,
infer  = False  ,
)


results   =  mem  . search  (  query  = "flight preference"  ,    filters  = {  "user_id"  :  "traveler_42"  }  )
# results[0]["metadata"] == {"seat_preference": "window", "cabin_class": "economy", "confidence": 0.9}
```


```text
mem  . add  (
"The user's flight booking preference."  ,
user_id  = "traveler_42"  ,
infer  = False  ,
)


```


```text
mem  . add  (
"The user's flight booking preference."  ,
user_id  = "traveler_42"  ,
infer  = False  ,
)


```


` infer=False` here matters, because it tells Mem0 to store the metadata exactly as given instead of running it through extraction, which is what you want when the structure is the point.


## **Structured vs unstructured AI memory**


Now we have a basic idea about structured and unstructured memory, let's compare the two side by side:


**Parameters**


**Unstructured**


**Structured**


**Shape**


Free text, no predefined fields


Defined fields with types


**Write cost**


Low, capture whatever comes up


Higher, fields decided in advance


**Retrieval**


Similarity search, approximate


Exact match, filterable, aggregatable


**Validation**


None built in


Can enforce types, allowed values


**Handles novelty**


Well, anything fits as a sentence


Poorly, needs a schema change


**Degrades how**


Gets noisy, contradictions pile up unless resolved


Gets brittle, breaks when reality outgrows the fields


**Best fit**


Conversational context, explanations, nuance


Filters, reporting, anything downstream code reads directly


Real production agents rarely pick one. Structured memory stays useful longer because someone is intentionally maintaining its shape, while a purely unstructured store tends to just accumulate text until retrieval quality quietly degrades. The stronger pattern is structured facts for what needs to be queried precisely, unstructured text for the explanation and nuance around it, both pointing at the same underlying record instead of living in two disconnected systems.


## **AI memory schema design: what actually needs deciding**


"Schema design" for agent memory isn't as heavy as a database migration, but skipping it entirely is how a metadata field turns into an unfilterable mess. Here are a few decisions that are worth making before the first memory gets written:


-


**Pick flat keys:** Mem0's` search()` filters match metadata by equality on top-level keys only,` {"metadata": {"status": "confirmed"}}` works, but a nested` {"metadata": {"booking": {"cabin": "economy"}}}` fails with an "unsupported metadata operator" error. Nested JSON stores and comes back fine; you just can't filter into it later, so anything you'll actually query needs to live at the top level.


-


**Fix your vocabulary:** If one memory stores` "status": "confirmed"` and another store` "status": "verified"` for the same meaning, an equality filter on either one misses the other.


-


**Version the shape:** A` schema_version` field costs nothing to add and saves you from guessing later whether an old memory's metadata means what a new one means, especially once the agent's been running long enough that the schema has changed underneath it.


The schema is a discipline you bring to how you write to it, not a constraint that Mem0 imposes.


## **JSON memory format LLM agents actually use**


JSON is the format that` metadata` already accepts, and it's what most tool-calling and function-calling interfaces expect on the way in and out of a model. But the same flexibility that makes JSON easy to extend is what makes it easy to end up with a memory store where every record has a slightly different shape.


```text
mem  . add  (
"Customer support escalation, billing dispute over a duplicate charge."  ,
user_id  = "support_agent"  ,
run_id  = "ticket-48291"  ,
metadata  = {
"schema_version"  :  1  ,
"category"  :  "billing_dispute"  ,
"priority"  :  "high"  ,
"resolved"  :  False  ,
}  ,
infer  = False  ,
)
```


```text
mem  . add  (
"Customer support escalation, billing dispute over a duplicate charge."  ,
user_id  = "support_agent"  ,
run_id  = "ticket-48291"  ,
metadata  = {
"schema_version"  :  1  ,
"category"  :  "billing_dispute"  ,
"priority"  :  "high"  ,
"resolved"  :  False  ,
}  ,
infer  = False  ,
)
```


```text
mem  . add  (
"Customer support escalation, billing dispute over a duplicate charge."  ,
user_id  = "support_agent"  ,
run_id  = "ticket-48291"  ,
metadata  = {
"schema_version"  :  1  ,
"category"  :  "billing_dispute"  ,
"priority"  :  "high"  ,
"resolved"  :  False  ,
}  ,
infer  = False  ,
)
```


If a second engineer six months later adds` "details": {"amount": 42.50, "duplicate_charge_id": "ch_9182"}` to a different memory, storage won't complain, Mem0 accepts arbitrary nested JSON in` metadata` and hands it back intact on` search()` . But you've now got one record you can filter on` priority` and one you can't, because` details` is nested, and nothing else in your codebase agreed to look for it there.


## **Building it with Mem0**


Unstructured storage (` infer=True` , the default) makes a judgment call about whether something is worth remembering. Most of the time, that call is right; a stable preference gets captured reliably. But for a fact that reads as transient or operational rather than personal, that same judgment call can go the other way, and go the other way silently, no error, no warning, just nothing stored.


Let’s see how you can make something like this work with Mem0:


> **Get yourself a**[Mem0 API Key](https://app.mem0.ai/dashboard) **and try this out!**


```text
from    mem0    import    MemoryClient


mem   =  MemoryClient  (  api_key  = "your-api-key"  )


# a durable preference: extraction's sweet spot
mem  . add  (
"The user prefers dark mode in the editor and works primarily in Python."  ,
user_id  = "pref-user"  ,
)


# an operational fact: a transient blocker status
mem  . add  (
"The payments migration is currently blocked because the vendor sandbox API is down."  ,
user_id  = "blocker-user"  ,
)


# the same blocker fact, stored structured instead, no judgment call involved
mem  . add  (
"Payments migration is blocked on the vendor sandbox API."  ,
user_id  = "blocker-user-structured"  ,
metadata  = {  "type"  :  "blocker"  ,    "owner"  :  "priya"  ,    "status"  :  "open"  ,    "priority"  :  "high"  }  ,
infer  = False  ,
)
```


```text
from    mem0    import    MemoryClient


mem   =  MemoryClient  (  api_key  = "your-api-key"  )


# a durable preference: extraction's sweet spot
mem  . add  (
"The user prefers dark mode in the editor and works primarily in Python."  ,
user_id  = "pref-user"  ,
)


# an operational fact: a transient blocker status
mem  . add  (
user_id  = "blocker-user"  ,
)


# the same blocker fact, stored structured instead, no judgment call involved
mem  . add  (
"Payments migration is blocked on the vendor sandbox API."  ,
user_id  = "blocker-user-structured"  ,
infer  = False  ,
)
```


```text
from    mem0    import    MemoryClient


mem   =  MemoryClient  (  api_key  = "your-api-key"  )


# a durable preference: extraction's sweet spot
mem  . add  (
"The user prefers dark mode in the editor and works primarily in Python."  ,
user_id  = "pref-user"  ,
)


# an operational fact: a transient blocker status
mem  . add  (
user_id  = "blocker-user"  ,
)


# the same blocker fact, stored structured instead, no judgment call involved
mem  . add  (
"Payments migration is blocked on the vendor sandbox API."  ,
user_id  = "blocker-user-structured"  ,
infer  = False  ,
)
```


Here is the output:


The preference stored fine, every time, split cleanly into two facts. The blocker sentence is where it gets interesting: run across four separate sessions (twelve trials total, three fresh runs each), it came back empty ten times and stored twice, with no way to predict which from the outside. The structured version,` infer=False` with the fields spelled out, stored on the first and only try, every single time, because there was no judgment call left to make.


## **Wrapping up**


Structured and unstructured memory aren't competing designs; they're answers to two different questions. "What did the user say?" is answered well by a sentence. "Show me every record where X is true" needs a field. Most agents that get memory right aren't choosing one; they're deciding, deliberately, which facts earn a field and which stay prose, and revisiting that decision as the agent's job changes. Mem0 doesn't force that decision for you either way,` add()` with` infer=True` gives you the unstructured default,` metadata` with` infer=False` gives you the structured layer, and both live on the same memory record, retrievable by the same` search()` , so the two never have to be two different systems.


## **Further Reading**


-


[AI Agent Memory: The Complete Guide](https://mem0.ai/blog/memory-in-agents-what-why-and-how)


-


[Context Engineering for AI Agents: How to Route Queries to Memory](https://mem0.ai/blog/context-engineering-for-ai-agents-how-to-route-queries-to-memory)


-


[AI Agent Frameworks and How to Choose a Memory Strategy](https://mem0.ai/blog/ai-agent-frameworks-and-how-to-choose-a-memory-strategy)


-


[AI Agent Memory 2026: Progress Benchmark Report](https://mem0.ai/blog/state-of-ai-agent-memory-2026)


## **Frequently Asked Questions**


**Q. What is the difference between structured and unstructured AI memory?**


Unstructured memory stores a fact as natural language and retrieves it by similarity, no fields decided in advance. Structured memory stores a fact as defined fields with types, retrieved by exact match, filtering, and aggregation. Unstructured is cheaper to write, structured is more useful to query, and most agents end up needing both.


**Q. How do I approach AI memory schema design?**


Decide, before you start writing memories at scale, which fields you'll actually filter on and keep those flat, since equality-based filters only match top-level metadata keys. Fix your vocabulary for those fields early, version the schema so old and new memories don't silently mean different things, and let free text carry anything you haven't modeled yet instead of forcing every fact into a field.


**Q. What's the best JSON memory format for LLM agents?**


Flat, and only as deep as you're prepared to filter into. JSON metadata can store nested objects without complaint, but exact-match filters only reach top-level keys, so anything nested is retrievable as part of the full record but not queryable on its own. Keep fields you'll filter on flat, use consistent field names across every write path, and reserve nesting for details you only need once you've already found the record.


**Q. Can Mem0 store both structured and unstructured memory?**


Yes, on the same record.` add()` with the default` infer=True` handles unstructured text, extracting and storing facts automatically. Passing` metadata` with` infer=False` stores exactly the structured fields you give it, no extraction. Both are retrieved through the same` search()` call, so an agent isn't choosing between two separate memory systems.


**Q. Why does structured memory stay reliable longer than unstructured memory?**


Because someone has to actively maintain its shape, add a field, deprecate one, and agree on what a value means, that discipline is forced by the format itself. Unstructured memory has no equivalent forcing function, so without some other maintenance process, it just accumulates text and contradictions until retrieval quality quietly gets worse.
