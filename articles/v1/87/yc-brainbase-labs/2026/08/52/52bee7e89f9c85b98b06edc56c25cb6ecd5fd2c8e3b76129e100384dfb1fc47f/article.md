---
schema_version: "1.0.0"
document_id: "52bee7e89f9c85b98b06edc56c25cb6ecd5fd2c8e3b76129e100384dfb1fc47f"
company_key: "yc-brainbase-labs"
company: "Brainbase Labs"
source_id: "yc-brainbase-labs-news-import-4e6e4e598b0a"
canonical_url: "https://brainbaselabs.com/blog/meta-muse-spark-live-on-brainbase"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T23:50:10.484155+00:00"
fetched_at: "2026-08-12T23:50:15.713808+00:00"
content_hash: "sha256:90918fcc2cf4b4615b79516f88c5bc819282148151fe61a89b7fe8d238f4f525"
---

# @meta/muse-spark is live on Brainbase

Meta's Muse Spark is now available on Brainbase as` @meta/muse-spark` . The Brainbase identifier tracks the current Muse Spark 1.2 model, and it works across the same managed harnesses and sandboxes as every other supported model.


Muse Spark 1.2 is Meta's coding-optimized release from the Muse family: a natively multimodal reasoning model built for tool use, visual reasoning, extended agent sessions, and multi-agent orchestration.


## # What is Muse Spark?


Muse Spark is the first model family from Meta Superintelligence Labs. Version 1.2 focuses on real coding workflows, higher first-attempt accuracy, and more reliable tool calling. Its one-million-token context window is designed for long sessions where an agent needs to retain actions, retrieve earlier decisions, and compact context without losing the critical path.


That makes Muse Spark a natural fit for repository-wide engineering, computer-use workflows, and agents that need to move between code, visual input, and tools without rebuilding their context every turn.


## # Muse Spark benchmarks


Artificial Analysis scores Muse Spark 1.2 at 54 on its Intelligence Index, up three points from Muse Spark 1.1 and eleven points from the first Muse Spark release. The largest jump is in agentic knowledge work: GDPVal-AA v2 rises 260 Elo to 1631. Select a metric to see the comparison.


Muse Spark 1.2 benchmark results


Relative comparison · exact values shown above each bar


Scale starts at 49


- Muse Spark 1.2: 54
- Muse Spark 1.1: 51
- Grok 4.5: 54
- GPT-5.5: 55


Source:[Artificial Analysis, Muse Spark 1.2](https://artificialanalysis.ai/articles/muse-spark-1-2)


The quality gain comes with more inference-time work. Artificial Analysis measured roughly 53% more input tokens and 36% more output tokens per Intelligence Index task than Muse Spark 1.1. That raises measured task cost from $0.29 to $0.40, while keeping Muse Spark near the cost-efficiency frontier for models in its intelligence cluster.


## # Muse Spark pricing and context


Meta kept Muse Spark 1.2's per-token rates unchanged from version 1.1. Cached input is especially inexpensive for agents that repeatedly reuse repository context, policies, or a long conversation prefix.


Muse Spark 1.2 Price or limit


Input tokens $1.25 / 1M tokens


Cached input $0.15 / 1M tokens


Output tokens $4.25 / 1M tokens


Context window 1,000,000 tokens


AA measured cost $0.40 / Intelligence Index task


Model-token pricing is separate from sandbox runtime, tool calls, and other infrastructure usage. Actual cost depends on reasoning effort, prompt caching, and how long the agent stays in the loop.


## # Run Muse Spark on Brainbase


Choose Muse Spark in the model field. Here it runs inside OpenCode, but changing the harness to Codex, Claude Code, Cursor, or another supported option does not change the model integration:


POST /v2/threads


bash


```text
curl https://api.brainbaselabs.com/v2/threads \
-H   "Authorization: Bearer $BRAINBASE_API_KEY"   \
-H   "Content-Type: application/json"   \
-d   '  {
"agent"  :   {
"harness"  :     "opencode"  ,
"model"  :     "@meta/muse-spark"
},
"input"  :     "Trace the failing integration test, implement the fix, and explain the regression."
}   '
```


## # Where Muse Spark fits


Muse Spark is compelling for long-context engineering agents, multimodal workflows, and tool-heavy tasks where cached input can keep recurring context cheap. It is not at the absolute top of every static reasoning benchmark, but its agentic gains, one-million-token window, and $1.25/$4.25 pricing make it a practical production model.


Evaluate Muse Spark on the traces that matter to you: repository maintenance, UI work, computer use, or multi-step knowledge tasks. Brainbase lets you run the same agent spec across Muse Spark and other models, then compare quality, latency, and cost without changing the surrounding infrastructure.


Read Meta's[Muse Spark 1.2 model overview](https://developer.meta.com/ai/models/muse-spark/) for the official capability details.
