---
schema_version: "1.0.0"
document_id: "f2da3720d44aba0d7c86e41909f1cd187650cff75dcb433efeda85b0a89ef460"
company_key: "yc-brainbase-labs"
company: "Brainbase Labs"
source_id: "yc-brainbase-labs-news-import-4e6e4e598b0a"
canonical_url: "https://brainbaselabs.com/blog/spacexai-grok-4-6-live-on-brainbase"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T23:50:10.484155+00:00"
fetched_at: "2026-08-12T23:50:15.713808+00:00"
content_hash: "sha256:bafd48ef62cb2462a99f78baa7930861b4221d91a2ce98a8480e4a1bd8bed87e"
---

# @spacexai/grok-4.6 is live on Brainbase

Grok 4.6 is now available on Brainbase as` @spacexai/grok-4.6` . Use it with any supported harness and sandbox through the same Universal Managed Agents API you already use for every other model.


SpaceXAI released Grok 4.6 on August 12 with a specific focus on long-running agents, coding, knowledge work, and ambitious visual projects. It accepts text and image input, exposes 500,000 tokens of context, and supports reasoning, function calling, and structured outputs.


## # Why Grok 4.6 matters for agents


The biggest gains are in the work agents actually do: operating a terminal, moving through a codebase, calling tools over many turns, and producing finished work artifacts. SpaceXAI reports more self-testing and verification on long trajectories, plus stronger first passes on interactive and visual applications.


That profile makes Grok 4.6 more interesting than a static benchmark bump. A model that reaches a good result in fewer turns consumes less context, makes fewer tool calls, and costs less to run as an agent.


## # Grok 4.6 benchmarks


On SpaceXAI's published evaluation set, Grok 4.6 reaches 61 on the Artificial Analysis Intelligence Index, tying GPT-5.6 Sol and gaining five points over Grok 4.5. It also leads the compared models on GDPVal-AA v2 and edges GPT-5.6 Sol on CursorBench and FrontierCode. Select a metric below to compare the reported results.


Grok 4.6 benchmark results


Relative comparison · exact values shown above each bar


Scale starts at 54


- Grok 4.6: 61
- Grok 4.5: 56
- GPT-5.6 Sol: 61
- Claude Fable 5: 62


Source:[SpaceXAI, Introducing Grok 4.6](https://x.ai/news/grok-4-6)


Independent testing from[Artificial Analysis](https://artificialanalysis.ai/articles/grok-4-6-benchmarks-and-analysis) confirms the 61 Intelligence Index score and highlights the agentic profile: 50.7% on τ³-Banking, 88.4% on Terminal-Bench v2.1, and an AA-Briefcase Elo of 1577. Its measured average cost was $0.84 per Intelligence Index task.


## # Grok 4.6 pricing and context


Grok 4.6 keeps Grok 4.5's headline pricing while adding five points on the Intelligence Index. Standard API pricing starts at the rates below; SpaceXAI applies higher rates when a request exceeds 200,000 context tokens, and its fast variant is priced at twice the standard rate.


Grok 4.6 Price or limit


Input tokens $2.00 / 1M tokens


Cached input $0.50 / 1M tokens


Output tokens $6.00 / 1M tokens


Context window 500,000 tokens


Fast variant 2× standard token pricing


These are model-token prices before sandbox runtime, tool calls, and other infrastructure usage. For reasoning-heavy agents, output tokens usually dominate, which is why the $6 output rate matters.


## # Run Grok 4.6 on Brainbase


Select the model in the agent spec and leave the rest of your stack unchanged. This example runs Grok 4.6 inside the Codex harness on a managed sandbox:


POST /v2/threads


bash


```text
curl https://api.brainbaselabs.com/v2/threads \
-H   "Authorization: Bearer $BRAINBASE_API_KEY"   \
-H   "Content-Type: application/json"   \
-d   '  {
"agent"  :   {
"harness"  :     "codex"  ,
"model"  :     "@spacexai/grok-4.6"
},
"input"  :     "Audit this repository, fix the highest-impact reliability issue, and run the tests."
}   '
```


## # Where Grok 4.6 fits


Grok 4.6 is a strong default candidate for codebase-scale work, research that spans many tools, terminal-heavy tasks, and visual app generation. Its combination of frontier scores and lower token prices also makes it useful in routing policies where GPT-5.6 Sol or Claude would otherwise be selected for every hard task.


Start with Grok 4.6 where agentic quality and cost both matter, then evaluate it against your production traces. Brainbase keeps the harness, sandbox, event stream, and observability layer constant so the model is the only variable.
