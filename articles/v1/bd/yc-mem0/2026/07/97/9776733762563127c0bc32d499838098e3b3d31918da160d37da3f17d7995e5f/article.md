---
schema_version: "1.0.0"
document_id: "9776733762563127c0bc32d499838098e3b3d31918da160d37da3f17d7995e5f"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/benchmarked-openai-memory-vs-langmem-vs-memgpt-vs-mem0-for-long-term-memory-here-s-how-they-stacked-up"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-23T17:29:03.310285+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:7c216ee00733e3ac0f0538d91eb3d96c3e473726d1bed376d03f2b3d8ce14c96"
---

# OpenAI Memory vs. LangMem vs. Letta (MemGPT) vs. Mem0: Which Performs Best?

Long-term memory is no longer just about storing conversation history. The strongest memory systems combine selective extraction, hybrid retrieval, entity linking, and token efficiency.


Mem0’s[latest memory algorithm](https://mem0.ai/research) uses single-pass ADD-only extraction, multi-signal retrieval, and built-in entity linking. Across current benchmarks, it reports strong accuracy while keeping retrieved context under 7,000 tokens per query.


## **TL;DR:**


Layer


Judge accuracy ↑


p95 latency ↓


Tokens/query ↓


Stand-out capability


**Mem0**


66.9 %


**1.4 s**


**≈ 2K**


Best accuracy-vs-speed-vs-cost balance


Mem0ᵍ


**68.5 %**


2.6 s


≈ 4K


Strongest on timeline / relational queries


OpenAI Memory


52.9 %


0.9 s


≈ 5K


Fastest setup, shallow recall


LangMem


58.1 %


60 s


≈ 130*


OSS playground; too slow for chat


## **Why This Matters?**


LLMs can stream text fluently, but they still lose continuity once useful context falls outside the active window. Real agents need to remember preferences, timelines, decisions, and facts across days or weeks.


The production question is no longer “can we store memory?” It is “can we retrieve the right memory, at the right time, without stuffing the prompt?”


That is why memory systems are now evaluated across three practical constraints:


-


Accuracy on long-term recall


-


Latency during retrieval and response generation


-


Token cost per query


A memory layer that is accurate but expensive is hard to scale. A fast but shallow memory layer will miss the details that make agents feel useful.


## The Four Memory Approaches


System


Storage strategy


Retrieval strategy


OpenAI Memory


Built-in saved memories inside ChatGPT


Managed retrieval with limited developer control


LangMem


Developer-defined memory stores and workflows


Custom retrieval and memory management patterns


MemGPT


Context treated as active memory, with overflow stored externally


Paging-style memory management


Mem0


Selective extracted memories with entity linking


Hybrid retrieval using semantic, keyword, and entity signals


## **Benchmark Setup**


The current Mem0 benchmark suite evaluates memory across LoCoMo, LongMemEval, and BEAM.


-


[LoCoMo](https://mem0.ai/research#:~:text=BENCHMARKS-,LoCoMo,-1%2C540%20questions%20%E2%80%A2%205) **** tests memory recall across multi-session conversations, including single-hop, multi-hop, open-domain, and temporal questions.


-


[LongMemEval](https://mem0.ai/research#:~:text=LOCOMO-,LongMemEval,-500%20questions%20%E2%80%A2%206) **** evaluates single-session recall, assistant recall, preference recall, knowledge updates, temporal reasoning, and multi-session memory.


-


[BEAM](https://mem0.ai/research#:~:text=LONGMEMEVAL-,BEAM,-BEAM%201M%3A%20700) **** tests memory at 1M and 10M token scales. It is especially relevant for production because it cannot be solved by simply expanding the context window.


The key metrics are:


-


benchmark accuracy


-


average tokens per query


## Results


Mem0’s latest token-efficient memory algorithm reports the following benchmark results:


Benchmark


Score


Avg tokens/query


LoCoMo


92.5


6,956


LongMemEval


94.4


6,787


BEAM 1M


64.1


6,710


BEAM 10M


48.6


6,910


The important point is not just the score. It is the token budget. Full-context approaches on these benchmarks can consume 25,000+ tokens per query. Mem0 stays under 7,000 tokens per retrieval call.


Here is a category-wise breakdown for each benchmark:


### LoCoMo


LoCoMo tests single-hop, multi-hop, open-domain, and temporal memory recall across conversational sessions.


Category


Old Algorithm


New Algorithm


Delta


Overall


71.4


92.5


+21.1


Single-hop


76.6


92.3


+15.7


Multi-hop


70.2


93.3


+23.1


Open-domain


57.3


76.0


+18.7


Temporal


63.2


92.8


+29.6


***Mean tokens: 6,956***


The two biggest gains are on temporal queries (+29.6) and multi-hop reasoning (+23.1). For a developer, this means the new algorithm handles questions like "when did the user first mention X?" or "what led to the user's current decision?" much more reliably. Both categories directly test the ADD-only architecture and the entity linking layer.


### LongMemEval


LongMemEval evaluates memory across single-session and multi-session contexts, including knowledge updates and temporal reasoning.


Category


Old Algorithm


New Algorithm


Delta


Overall


67.8


94.4


+26.6


Single-session user


94.3


98.6


+4.3


Single-session assistant


46.4


98.2


+51.8


Single-session preference


76.7


96.7


+20.0


Knowledge update


79.5


93.6


+14.1


Temporal reasoning


51.1


97.0


+45.9


Multi-session


70.7


88.0


+17.3


***Mean tokens: 6,787***


The biggest gains are on single-session assistant (+53.6), temporal reasoning (+42.1), and knowledge updates (+16.7). The jump in assistant memory recall means the new system reliably remembers things your agent said. That is the kind of thing developers assume will work until they actually test it. The old algorithm had a blind spot for agent-generated facts that the new one does not.


### BEAM


BEAM evaluates memory systems at 1M and 10M token scales across ten task categories, including preference following, temporal reasoning, and contradiction resolution. It is the only public benchmark that operates at context volumes production AI agents actually encounter.


**Category**


**1M**


**10M**


**Overall**


**64.1**


**48.6**


preference_following


88.3


90.4


instruction_following


85.2


82.5


information_extraction


70.0


56.3


knowledge_update


65.0


75.0


multi_session_reasoning


65.2


26.1


summarization


63.5


46.9


temporal_reasoning


61.8


16.3


event_ordering


53.6


20.2


abstention


52.5


40.0


contradiction_resolution


35.7


32.5


***Mean tokens (1M): 6,719 Mean tokens (10M): 6,914***


Performance is meaningfully stronger at 1M than at 10M. At the 10M scale, retrieval gets harder because similar content appears multiple times across the window, and the memory system cannot always surface the exact correct memory over other close matches.


## **What Changed in the Memory Architecture**


Mem0’s current algorithm has three major pieces.


-


**Single-pass ADD-only extraction:** The system extracts new memories in one pass and only adds facts. It does not overwrite or delete older memories during extraction. When information changes, the new fact is stored alongside the old one, preserving history.


-


**Multi-signal retrieval:** Search combines semantic similarity, BM25 keyword matching, and entity matching. The signals are fused into one final score.


-


**Built-in entity linking:** External graph memory has been replaced by entity linking inside the memory stack. Entities are extracted from memories, stored in a parallel entity collection, and used to boost relevant results during search.


## **Practical Recommendations**


Scenario


Best layer


Why


Fast prototype inside ChatGPT


OpenAI Memory


No extra infrastructure


Research or custom memory experiments


LangMem


Flexible open-source workflows


Context paging experiments


MemGPT


Clear RAM/disk mental model


Production chat assistant


Mem0


Strong accuracy-token-latency balance


Timeline or relationship-heavy memory


Mem0 with entity linking


Entity boosts help connected-context retrieval


## **Reproducing Current Mem0 Behavior**


**For the latest Mem0 open-source setup:**


```text
pip   install   -- upgrade   mem0ai
```


```text
pip   install   -- upgrade   mem0ai
```


```text
pip   install   -- upgrade   mem0ai
```


**For hybrid search and entity extraction:**


```text
pip   install   -- upgrade   "mem0ai[nlp]"
python   - m   spacy   download   en_core_web_sm
```


```text
pip   install   -- upgrade   "mem0ai[nlp]"
python   - m   spacy   download   en_core_web_sm
```


```text
pip   install   -- upgrade   "mem0ai[nlp]"
python   - m   spacy   download   en_core_web_sm
```


**Note:** *The*` \[nlp\]` *extra currently works with Python 3.10–3.12. On Python 3.13, spaCy dependencies may fail to build.*


**For Qdrant users, install**` **fastembed**` **to enable BM25 sparse-vector search:**


```text
pip   install   fastembed
```


```text
pip   install   fastembed
```


```text
pip   install   fastembed
```


**Search now uses filters for entity IDs:**


```text
results   =  m  . search  (
"what did Alice say about deployment?"  ,
filters  = {  "user_id"  :    "alice"  }  ,
top_k  = 20  ,
threshold  = 0.1  ,
)
```


```text
results   =  m  . search  (
"what did Alice say about deployment?"  ,
filters  = {  "user_id"  :    "alice"  }  ,
top_k  = 20  ,
threshold  = 0.1  ,
)
```


```text
results   =  m  . search  (
"what did Alice say about deployment?"  ,
filters  = {  "user_id"  :    "alice"  }  ,
top_k  = 20  ,
threshold  = 0.1  ,
)
```


External graph memory setup is no longer required. The old` enable_graph` and` graph_store` configuration paths have been removed from the current OSS SDK.


## **Summary**


Large context windows delay forgetting, but they do not solve memory problems. A production memory layer has to decide what to store, how to preserve changes over time, and how to retrieve the right facts cheaply.


Mem0


Production memory layer with ADD-only extraction and hybrid retrieval


Best balance of accuracy, latency, and token efficiency


OpenAI Memory


Built-in memory inside ChatGPT


Fastest setup, limited developer control


LangMem


Open-source memory toolkit


Useful for experimentation and custom memory workflows


MemGPT


Memory paging architecture


Strong conceptual model for context management


The strongest pattern is selective memory plus hybrid retrieval. Mem0’s latest architecture reflects that direction: ADD-only extraction, entity linking, and multi-signal search under a practical token budget.


If your agent needs to live longer than its context window, memory is no longer optional infrastructure. It is part of the core agent stack.
