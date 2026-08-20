---
schema_version: "1.0.0"
document_id: "9813bc30d841daae6637cbe830e64a092b09228f11eee0f29e0c864f944f710e"
company_key: "yc-hyperspell"
company: "Hyperspell"
source_id: "yc-hyperspell-news-import-13fe3511aa44"
canonical_url: "https://www.hyperspell.com/blog/context-assembly"
published_at: null
first_seen_at: "2026-08-09T23:13:54.426753+00:00"
fetched_at: "2026-08-09T23:13:55.393599+00:00"
content_hash: "sha256:8abd759417f4ac76ec79b65a1b8576adfb6cd4bc2858e1f90fc81a8864e4206f"
---

# Context Assembly: From Retrieval to LLM-Ready Prompts

Your retrieval pipeline returns ten relevant memories. You concatenate them into the prompt. The agent ignores half and hallucinates the rest.


The problem wasn't retrieval. It was what happened next.


Context assembly is the step between memory retrieval and LLM generation that most teams treat as string concatenation. Research from[Liu et al.](https://arxiv.org/abs/2307.03172) shows that where information lands in a prompt directly affects whether the model uses it. Assembly determines whether your retrieved context helps or hurts.


At Hyperspell, we assemble context from 50+ workspace integrations on every query. Emails, Slack messages, calendar events, CRM records, and documents all compete for space in a single prompt. Here's what that work has taught us about turning raw memories into useful LLM input.


## What is context assembly?


Context assembly is the process of selecting, ordering, formatting, and injecting retrieved memories into an LLM prompt. It sits in the middle of a four-step pipeline: query, retrieval, assembly, generation.


Retrieval finds candidates. Assembly decides what the model actually sees.


This distinction matters because concatenation ignores ordering effects, duplication, token limits, and formatting. LLMs exhibit a U-shaped performance curve: they use information at the beginning and end of the context window reliably, but performance degrades for information in the middle ([Liu et al., 2024](https://arxiv.org/abs/2307.03172) ).


[Google's engineering team](https://developers.googleblog.com/architecting-efficient-context-aware-multi-agent-framework-for-production/) frames context as a "compiled view" over underlying state, not a raw dump of everything available. The assembly step is where that compilation happens.


## Why naive assembly breaks in production


Four problems emerge when teams skip assembly and go straight from retrieval to prompt.


### Deduplication


The same fact often appears in multiple sources. A meeting decision might exist in the calendar event, the Slack thread discussing it, and the follow-up email. Including all three wastes tokens and can confuse the model about whether these are three separate facts or one.


### Relevance filtering


Retrieval scores aren't perfect indicators of usefulness. A document might score well on vector similarity but contain mostly irrelevant content with a few matching keywords.[Anthropic's research on context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) describes "context rot," where adding more tokens to the context window decreases the model's ability to recall and reason over the information already there.


### Ordering


Position in the prompt matters more than most teams realize. LLMs perform best when relevant information appears at the beginning or end of the context, and worst when it's buried in the middle. Assembly must place the most critical memories where the model pays attention.


### Token budgeting


A context window must fit system instructions, retrieved memories, the user's query, and enough room for the response. Without a budget, you either truncate useful context or crowd out response quality.[One case study](https://blog.logrocket.com/llm-context-problem-strategies-2026/) demonstrated the difference: a naive approach using 140,000 tokens achieved 70% accuracy, while an engineered approach using 6,000 tokens reached over 90%.


More context isn't better context. After a point, adding memories to the prompt makes the agent worse. Assembly is about finding the right amount.


## Five context assembly strategies


Not every system needs the same approach. These strategies trade off implementation complexity, token efficiency, and latency.


### Simple concatenation


Join retrieved memories with separators and pass them to the model. This works when you have a small number of memories (under roughly 4,000 tokens) from a single source type. It breaks when sources multiply or token budgets tighten.


### Relevance truncation


Order memories by retrieval score and include them until the token budget is exhausted. Simple to implement, but it ignores diversity. You might end up with five near-duplicate results about the same topic while missing adjacent context that would have been more useful.


### Summarization


Compress memories using a smaller model before including them in the prompt.[LLMLingua](https://github.com/microsoft/LLMLingua) achieves up to 20x prompt compression with minimal performance loss.[LongLLMLingua](https://arxiv.org/abs/2310.06839) shows a 21.4% performance improvement with roughly 4x fewer tokens in long-context scenarios.


The tradeoff is latency. Compression requires an additional model call. But for agents pulling context from a dozen sources, the token savings usually justify the extra step.


### Hierarchical assembly


Group memories by type or source (emails, Slack messages, documents), summarize each group, and include group summaries in the prompt.[Google's tiered model](https://developers.googleblog.com/architecting-efficient-context-aware-multi-agent-framework-for-production/) , which separates context into session history, long-term memory, and artifacts, is a production example of this pattern. It works well when an agent pulls from diverse source types where each source carries different signal.


### Selective extraction


Extract only the relevant parts of each memory, guided by the user's query. Instead of compressing an entire document, pull out the sentences or facts that answer the question.[LangChain's context engineering guidance](https://blog.langchain.com/context-engineering-for-agents/) follows the same principle: dynamically inject only what the current turn needs.


Think of it like packing a suitcase. Concatenation throws everything in. Truncation fills it in order of importance. Summarization folds things smaller. Hierarchical assembly uses packing cubes. Selective extraction only packs what you'll wear.


Strategy


Token efficiency


Complexity


Best for


Simple concatenation


Low


Minimal


Small contexts, single source


Relevance truncation


Medium


Low


Quick wins, early prototypes


Summarization


High


Medium


Large memory sets, latency-tolerant systems


Hierarchical assembly


High


Medium-High


Multi-source agents (email + Slack + docs)


Selective extraction


Highest


High


Production systems with tight token budgets


In practice, assembly starts with retrieval parameters that shape what enters the pipeline. Here's how that looks with Hyperspell's SDK, using source weighting to prioritize certain data sources during retrieval:


` from hyperspell import Hyperspell client = Hyperspell(api_key="API_KEY", user_id="user_123") response = client.memories.search( query="what's the status of the Anderson deal?", sources=\["gmail", "slack", "vault"\], options={ "max_results": 15, "gmail": {"weight": 1.5}, "slack": {"weight": 0.5}, } ) # Gmail results ranked 3x higher than Slack # Assembly pipeline receives pre-prioritized candidates`


The weight parameter influences the re-ranker, so email results about the Anderson deal are prioritized over Slack chatter. This is assembly at the retrieval boundary: shaping what candidates enter the pipeline before any deduplication or formatting.


## How to format context for LLM comprehension


Format retrieved memories with clear structure, source attribution, and separators so the model can distinguish between sources and use them accurately. Three formatting decisions matter.


### Structured vs. prose formatting


JSON and Markdown structure help when memories are heterogeneous, mixing calendar events with emails and documents.[Anthropic's work on agent harnesses](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) found that JSON worked better than Markdown for structured feature data in their testing. Natural prose works better for homogeneous narrative context, like a series of conversation excerpts.


### Source attribution in context


Include source type, timestamp, and origin with each memory block. This helps the model cite appropriately and helps you debug when answers go wrong.[Google's multi-agent framework](https://developers.googleblog.com/architecting-efficient-context-aware-multi-agent-framework-for-production/) uses attribution tags when transferring context between agents to prevent misattribution.


A simple pattern:


` \[Source: Gmail | From: sarah@company.com | Date: 2026-03-28\] The Anderson deal is moving to contract review next week. \[Source: Slack #sales | Author: mike | Date: 2026-03-27\] Anderson team confirmed budget approval internally.`


### Separators and section headers


Clear boundaries between memory blocks prevent the model from blending information across sources. Text delimiters (like XML tags or Markdown headers) outperform special tokens for most models. It's also worth separating transient context (this turn only) from persistent context (durable memory) so real-time data doesn't get confused with stored knowledge.


## Handling edge cases in context assembly


Production systems hit scenarios that simple pipelines don't anticipate.


**No memories retrieved.** The agent should acknowledge the gap rather than hallucinate. A fallback like "I don't have information about that in your connected sources" beats a confident wrong answer.


**Too many high-quality memories.** Use diversity scoring, not just relevance ranking. If all ten results are about the same email thread, you're missing context from other sources.


**Conflicting memories.** An email might say the meeting is Tuesday while the calendar shows Wednesday. Surface the conflict rather than silently resolving it. Our article on memory conflict resolution covers patterns for handling this.


**Very long individual memories.** Google's framework uses an artifact handle pattern: large objects (CSVs, PDFs, long documents) are stored externally with lightweight references in the prompt. The model loads the full artifact only when it needs specific data.


Hyperspell's SDK supports[metadata filtering](https://docs.hyperspell.com/usage/metadata) and date scoping through the query API, which helps handle freshness and deduplication at the retrieval layer:


` response = client.memories.search( query="Anderson deal status", sources=\["gmail", "slack"\], effort=1, # LLM rewrites query for better retrieval options={ "max_results": 10, "after": "2026-03-01", # Only recent memories } )`


Setting effort=1 lets Hyperspell rewrite the query for better retrieval and extract date filters from natural language. Combined with explicit date scoping, this reduces the stale or duplicate memories that enter the assembly pipeline.


## How to measure context assembly quality


Measure assembly by tracking three things: how much of your assembled context the model uses, whether responses stay grounded in that context, and how many tokens you spend per useful answer.


**Context utilization.** Did the model use what you provided? Trace which parts of the assembled context appear in the response. Low utilization means you're wasting tokens on context the model ignores.


**Response groundedness.** Is the response based on the assembled context or on the model's parametric knowledge? Grounded responses cite the provided context. Ungrounded responses suggest the assembly missed relevant memories or formatted them poorly.


**Token efficiency.** The ratio of tokens the model used versus tokens provided. The[LogRocket case study](https://blog.logrocket.com/llm-context-problem-strategies-2026/) cut tokens from 140,000 to 6,000 while improving accuracy from 70% to over 90%. That's a direct measurement of assembly quality.


Metric


What it measures


How to track it


Context utilization


% of provided context reflected in response


Compare response content against assembled context


Response groundedness


Whether answers cite context vs. parametric knowledge


LLM-as-judge or human evaluation on sample queries


Token efficiency


Useful output per token of context provided


Track output quality relative to context token count


Staleness rate


Age of memories in assembled context


Log timestamps of included memories per query


Hyperspell's[evaluation API](https://docs.hyperspell.com/usage/evaluation) can track retrieval quality over time, giving you a baseline for measuring how assembly changes affect downstream performance.


## Frequently asked questions


### What is the difference between context assembly and RAG?


RAG (retrieval-augmented generation) is the broader pattern of retrieving external information to augment LLM responses. Context assembly is a specific step within RAG: the process of formatting and organizing retrieved results into a prompt the model can use.


### How many memories should you include in a prompt?


There is no universal number. It depends on the model's context window, query complexity, and how much space the system prompt and response need. Start with 5 to 10 results and measure context utilization. If the model ignores most of them, include fewer.


### Does context assembly add latency?


Simple strategies like concatenation and truncation add negligible latency. Summarization and selective extraction require additional model calls, which adds processing time. The accuracy improvement typically justifies the cost for production systems.


### Should you assemble context differently for different models?


Yes. Larger models with bigger context windows tolerate more raw context. Smaller models benefit more from aggressive compression and selective extraction. Position bias (the "lost in the middle" problem) varies by model, so test with your specific model.


## Key takeaways


-


Context assembly is the step between retrieval and generation that determines whether your memories help the agent or hurt it.


-


Naive concatenation fails because of deduplication, relevance filtering, ordering, and token budgeting problems.


-


Five strategies exist, from simple concatenation to selective extraction. Start simple and add complexity as your source count grows.


-


Position in the prompt matters. LLMs use information at the beginning and end more reliably than the middle.


-


Formatting context with clear structure, source attribution, and separators improves model comprehension.


-


Measure assembly quality independently from end-to-end evaluation. Track context utilization, groundedness, and token efficiency.


-


Retrieval gets the attention, but assembly does the work.


Hyperspell handles context assembly from 50+ workspace integrations so teams can focus on their agent's intelligence rather than retrieval and formatting plumbing. If you're assembling context from multiple sources, the[quickstart](https://docs.hyperspell.com/core/quickstart) takes a few minutes to get running.
