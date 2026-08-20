---
schema_version: "1.0.0"
document_id: "a13b5eebe0b95dad9891549b4e2b0a23e0888544fa50acb355bcd872337e076c"
company_key: "yc-zep-ai"
company: "Zep AI"
source_id: "yc-zep-ai-rss-0db2565bfdf1"
canonical_url: "https://blog.getzep.com/smart-context-assembly-fewer-tokens-higher-quality/"
published_at: "2026-06-04T15:20:25+00:00"
first_seen_at: "2026-07-24T08:02:00.467869+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:4135f764360fef95322c4505d3ad065e4eb7e0a403ba88c1c13a3e8188e92c8f"
---

# Smart Context Assembly: Fewer Tokens, Better Quality

Today we're announcing **Smart Context Assembly** , an upgrade to how Zep's default Context Block is built. It produces a smaller Context Block: fewer tokens, and more accurate answers. The same` thread.get_user_context` call you already make now returns the new Context Block automatically, with no SDK update required. It's available now to all Zep customers.


## What Smart Context Assembly is


[Smart Context Assembly](https://help.getzep.com/retrieving-context?ref=blog.getzep.com#zeps-context-block) is the new method that builds Zep's default Context Block. It retrieves across all six of Zep's context types: facts, entities, episodes, Observations, thread summaries, and the user summary.


The previous Context Block was built by running a separate search per context type with a fixed result limit per type, then combining the results into the block. That produced the same shape regardless of the query: a fixed number of facts, a fixed number of entities, and so on. Smart Context Assembly ranks candidates from the other five types simultaneously and fills the block up to a 2,500-character budget. The block's shape adapts to each query.


Smart Context Assembly is powered by[Auto Search](https://help.getzep.com/searching-the-graph?ref=blog.getzep.com#auto-search) , a new graph search mode we're also announcing today. Auto Search runs the cross-scope ranking and the character-budget packing. It's also exposed as a standalone feature through` graph.search` , so you can call it directly from your own code.


## Why it matters


Smart Context Assembly produces higher-quality context per token. On the LoCoMo benchmark, that means far fewer tokens for only a small accuracy cost: it reaches 86.5% accuracy on a median of 2,680 tokens, versus 94.7% on 5,760 tokens for the legacy Context Block — 54% fewer tokens for about 8 points of accuracy.


LoCoMo benchmark: Smart Context Assembly uses 2,680 median tokens at 86.5% accuracy, versus 5,760 tokens at 94.7% for the legacy Context Block — less than half the tokens at comparable accuracy.


In other runs, fewer tokens have even come with higher accuracy. On one LoCoMo run, a medium-sized legacy Context Block scored 80%, while a smaller Context Block from Smart Context Assembly scored 85%.


Fewer tokens means lower cost per call and more room in the context window for the rest of your agent’s prompt. The character budget also makes prompt sizes predictable across users and queries, so you can size context to your model and prompt budget without per-scope tuning.


## How to use it


To use Smart Context Assembly, keep calling` thread.get_user_context` as you already do; it now returns the new Context Block automatically.


```text
context = client.thread.get_user_context(thread_id="thread_abc123")
print(context.context)


```


To use Auto Search directly, call` graph.search` with` scope="auto"` . The rendered Context Block comes back on` results.context` . Set` max_characters` to size it to your model's context window, and pass` return_raw_results=True` to also get the retrieved items as structured arrays.


```text
results = client.graph.search(
user_id="user_abc",
query="What does Alice work on?",
scope="auto",
max_characters=4000,
)


print(results.context)


```


## Getting started


To get started, see the[Smart Context Assembly docs](https://help.getzep.com/retrieving-context?ref=blog.getzep.com#zeps-context-block) . Smart Context Assembly and Auto Search are both available to all Zep customers today.
