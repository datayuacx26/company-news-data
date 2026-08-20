---
schema_version: "1.0.0"
document_id: "b5fe2e275455813d759dab108e2fbc3054b58a532fe189d9458e7acc1e554b85"
company_key: "yc-hyperspell"
company: "Hyperspell"
source_id: "yc-hyperspell-news-import-13fe3511aa44"
canonical_url: "https://www.hyperspell.com/blog/context-windows-are-a-distraction"
published_at: null
first_seen_at: "2026-08-09T23:13:54.426753+00:00"
fetched_at: "2026-08-09T23:13:55.393599+00:00"
content_hash: "sha256:361229a45d388a5b1c98ebeaa637c51cf5a99a6fe77d7d07461cdb69614fd797"
---

# Context Windows: Why Bigger Isn't Better

Every few months a model vendor announces a bigger context window. 200K tokens. Then 1M. Then 10M. The implication is always the same: if you can fit everything in the prompt, you don't need retrieval, you don't need a memory layer, you don't need to think hard about what the model sees.


This is the most common architectural objection we hear at Hyperspell. It is also wrong, in ways that are well documented in the research literature, and the problems get worse as windows grow, not better.


The short version: more tokens make a model see more, not understand more. The long version takes the rest of this article. By the end you'll know when long context helps, when it actively hurts, and what mental model to use instead.


## The bigger-is-better assumption


The argument for infinite context is clean. More context means more information available to the model. More information means better answers. Eventually windows get large enough that retrieval becomes a workaround for a problem we no longer have.


It maps onto how humans work: reading more of a document usually means understanding it better. It maps onto storage trends: memory got cheap, so why not context. And it removes a hard engineering problem, figuring out what the model actually needs.


There are four reasons it doesn't hold up. Attention does not scale linearly. Retrieval quality is structural, not a workaround. Cost grows quadratically with context length. And freshness is a problem windows cannot solve at any size.


A bigger context window lets a model see more of your data. It does not help the model decide which parts matter, and it does not keep that data fresh.


## Why doesn't attention scale linearly?


### Lost in the middle


In 2023, Liu et al. tested how language models actually use long inputs. On multi-document question answering and key-value retrieval, performance was highest when the relevant information sat at the beginning or end of the context, and degraded significantly when it sat in the middle. This held even for models explicitly marketed as long-context ([Liu et al., "Lost in the Middle"](https://arxiv.org/abs/2307.03172) ).


Stretching the window didn't change where attention concentrated. The middle stayed dim.


### Context rot


Two years later, Chroma Research extended this work. They tested 18 leading models, including Claude 4 (Opus and Sonnet variants), GPT-4.1 and o3, Gemini 2.5 Pro and Flash, and Qwen3, across needle-in-haystack variants, the LongMemEval benchmark, and a repeated-words task ([Chroma Research, "Context Rot," July 2025](https://www.trychroma.com/research/context-rot) ).


The headline finding: performance grows increasingly unreliable as input length grows, across every model tested. Even a single distractor token reduces accuracy relative to the baseline. The phrase that stuck, and that Anthropic later adopted in its own engineering writing, is context rot. Bigger isn't broken at one specific token count. It just gets steadily less reliable.


### Real context vs advertised context


The RULER benchmark (COLM 2024) evaluated 17 models that all claim context windows of 32K tokens or larger. Only half maintained satisfactory performance at 32K. Almost all failed on harder tasks well before their stated limit, even though most could pass the simpler needle-in-a-haystack test ([Hsieh et al., "RULER"](https://arxiv.org/abs/2404.06654) ).


Translation: a "200K context window" is a marketing number. The usable budget is smaller, and how much smaller depends on the task and the model.


## The numbers that actually matter


Read context-window announcements with two numbers in mind: advertised and effective. The advertised number tells you the input limit before the API rejects your request. The effective number tells you how much of that input the model can use without quality degradation.


For agents, the gap matters more than for chatbots. An agent makes decisions on retrieved facts. Degraded recall in the middle of a long context produces confidently wrong actions, not hedged answers. The model still produces fluent output. It just acts on the wrong subset of what you gave it.


The failure mode is gradual, which is what makes it dangerous. Performance doesn't fall off a cliff at the advertised limit. It erodes, often invisibly, well before. This shows up as flaky evals, intermittent regressions on real user queries, and a slow loss of trust as agents get answers right "most of the time."


## What do long contexts cost?


Self-attention has O(n²d) complexity. Doubling context length quadruples both compute and memory ([Vaswani et al., "Attention Is All You Need"](https://arxiv.org/abs/1706.03762) ). This is not an implementation detail. It's the foundational arithmetic of every current transformer model.


The dollar math is harder to ignore than the academic math. At frontier pricing of $5 per million input tokens for Claude Opus 4.5, a 100K-token request costs about 50 cents in input tokens alone, before the model produces a single output token. At ten thousand requests per month, that's around $5,000 just to put bytes in front of the model ([Anthropic API pricing](https://platform.claude.com/docs/en/about-claude/pricing) , as of publication). The cost isn't in the answer. It's in the volume of context you decided was worth attending to.


Latency follows the same pattern. A targeted retrieval call processes a few thousand tokens; a naive long-context call processes a hundred times that, and the user waits while it does. Attention costs scale faster than serving infrastructure improves, so this gap is not closing.


"Just in case" context is the most expensive way to hedge. Every irrelevant token taxes both your budget and the model's accuracy on the tokens that matter.


## Why does retrieval quality beat context size?


### Precision over volume


Ten highly relevant tokens beat ten thousand marginally relevant ones. Attention dilutes. Context that is technically present but sparsely attended to is not actually being used.


This is now the consensus framing in the field. Anthropic's engineering team describes context as a finite resource with diminishing marginal returns ([Anthropic, "Effective context engineering for AI agents"](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) ). The discipline that follows is choosing what enters the prompt, not collecting more of it.


### Pre-processing as the unlock


Retrieval does the filtering before the model spends an attention slot on anything. The system can rank by recency, source authority, and explicit metadata before the model is even invoked. This is what we built Hyperspell to do: connect to a user's existing workspace data across 50+ integrations, index it continuously, and return the right context for each call rather than dumping everything in.


The point isn't that any specific implementation is special. Any production agent eventually needs this layer. You can build it yourself across email, Slack, Notion, calendar, CRM, and the rest, or you can rent it. Either way, the architecture that emerges is retrieval, not windowing.


A minimal call against this kind of memory layer looks like the following in Python ([docs](https://docs.hyperspell.com/) ):


` from hyperspell import Hyperspell client = Hyperspell(api_key="API_KEY", user_id="YOUR_USER_ID") response = client.memories.search( query="what did Joe think about the Q3 launch plan?", sources=\["slack", "gmail"\], ) print(response.documents)`


Two sources, one query, one ranked set of relevant documents back. The model never sees the full Slack history or the full inbox. It sees the slice that matters for this question, with metadata attached so it can reason about provenance and freshness.


### Freshness and structure that windows can't provide


Context windows are static per request. Whatever you stuff in is frozen at the moment you send the call. A memory layer maintains structured data across requests, with timestamps, source attribution, and custom metadata the application can filter on.


Freshness is a permanent advantage of retrieval over windowing. No model release fixes it. If your user's calendar changed an hour ago and your prompt was assembled this morning, the bigger context window doesn't help you.


A bigger context window can hold more of yesterday's data. It can't tell you what changed this morning.


## When do large context windows actually help?


Long context is a real feature. It just isn't a replacement for retrieval. It earns its place in three scenarios:


**Single-document analysis.** A long PDF, a research paper, a codebase you want fully in scope. The document genuinely fits, and freshness doesn't matter.


**Long single-session conversations.** Maintaining a complete chat history within one session, where state lives entirely in the conversation and not in external systems.


**Closed-corpus tasks.** A finite set of materials where you need broad awareness across the whole, and the corpus rarely changes.


The defining condition is that you need everything, not the right thing. When the entire corpus is the input, windowing wins.


## When should you use retrieval instead?


Retrieval wins anywhere those conditions don't hold. The clearest signals:


-


Knowledge that doesn't fit in any window by definition: the user's email, Slack, calendar, CRM, docs.


-


Multi-source contexts where source authority varies and the model needs to know which Slack channel or which CRM record is canonical.


-


Enterprise data where permission filtering must happen before generation, not after.


-


Production systems where cost per request is measured and matters.


-


Anywhere freshness matters, which in practice is most agentic workloads.


Use long context when


Use retrieval when


You need everything in one document


You need the right slice of many sources


The corpus fits and rarely changes


The data is large and updated continuously


Freshness is not relevant


Stale answers cause real harm


Cost per call is not a constraint


Per-request cost shapes unit economics


State lives in the conversation


State lives in the user's workspace


## The right mental model


Treat the context window as RAM and the memory layer as storage with intelligent caching. You don't dump your hard drive into RAM at boot. You don't dump your workspace into a context window at every call.


Retrieve what's needed for the task. Curate, don't accumulate. The pattern that scales is the one where the system chooses context before the model sees anything, not the one where the system relies on the model to find signal in noise.


This is the same shift that happened when applications moved from loading entire database tables into memory to issuing targeted queries. The infrastructure to do that for AI agents is what platforms like Hyperspell provide today. The point of the shift, then and now, is that bigger inputs don't fix retrieval. Better retrieval does.


## Frequently asked questions


### Will infinite context windows eventually replace retrieval?


No. Even at infinite size, attention dilutes and freshness remains unsolved. Retrieval ranks by recency, source authority, and metadata before the model is invoked. Those benefits don't disappear when the window grows. They become more valuable, because the cost of including irrelevant tokens grows quadratically.


### When does a 1M-token window make sense?


For single-document or single-corpus analysis where the material genuinely fits and freshness isn't a factor. A long PDF, a closed codebase, a research dataset. Not for workspace data, which changes continuously and is spread across many sources with different access controls.


### Isn't this just RAG vs long context?


Related, but narrower. RAG is one implementation of retrieval. A memory layer goes further: multi-source search across user-connected data, continuous indexing, source weighting, and permission handling. The argument for retrieval over windowing covers all of these, not just the simplest vector-search variant.


### How do I measure if context rot is hurting my agent?


Run RULER-style evals at your target context length ([Hsieh et al.](https://arxiv.org/abs/2404.06654) ). Compare focused inputs against full inputs on real questions, the way Chroma did with LongMemEval. If accuracy on the same questions drops as you add more context, you have measurable rot, and you have a number to optimize against.


## Key takeaways


-


Bigger context windows make models see more, not understand more.


-


Of 17 models claiming 32K+ context, only half perform satisfactorily at that length (RULER).


-


Self-attention scales as O(n²): doubling context length quadruples compute and memory cost.


-


Across 18 frontier models, performance grows unreliable as input length grows (Chroma Research, "Context Rot").


-


Retrieval beats windowing on freshness, cost, and accuracy for any non-trivial knowledge base.


-


Use long context for everything; use retrieval for the right thing.


## Sources


-


Liu, Nelson F., et al. "Lost in the Middle: How Language Models Use Long Contexts." TACL 2024. https://arxiv.org/abs/2307.03172


-


Chroma Research. "Context Rot: How Increasing Input Tokens Impacts LLM Performance." July 2025. https://www.trychroma.com/research/context-rot


-


Hsieh, Cheng-Ping, et al. "RULER: What's the Real Context Size of Your Long-Context Language Models?" COLM 2024. https://arxiv.org/abs/2404.06654


-


Vaswani, Ashish, et al. "Attention Is All You Need." 2017. https://arxiv.org/abs/1706.03762


-


Anthropic Engineering. "Effective context engineering for AI agents." https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents


-


Anthropic. API pricing. https://platform.claude.com/docs/en/about-claude/pricing


-


Hyperspell Documentation. https://docs.hyperspell.com
