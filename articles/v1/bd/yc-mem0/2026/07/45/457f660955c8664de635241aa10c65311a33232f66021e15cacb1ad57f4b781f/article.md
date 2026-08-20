---
schema_version: "1.0.0"
document_id: "457f660955c8664de635241aa10c65311a33232f66021e15cacb1ad57f4b781f"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/build-a-local-coding-agent-with-mem0-and-ollama"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-23T17:29:03.310285+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:739c88ce0fec2a7bf847da6e9a50aa9bfb1f9d7fa6b125860b9bf7d88447bf82"
---

# Build a Local Coding Agent with Mem0 and Ollama

## Quick Takeaways


> -
>
>
> A Mem0-optimized prompt is 39% shorter than raw conversation history compring 247 tokens vs. 407, using the same model on the same hardware.
>
>
> -
>
>
> The naive stack failed JSON parsing on all 3 runs. While, the Mem0-optimized stack succeeded on all 3.
>
>
> -
>
>
> The entire local stack runs with Mem0, Qdrant embedded, and Ollama all run in-process.
>
>
> -
>
>
> Moving from the Mem0 Platform` MemoryClient` to a self-hosted setup is a config swap, so the` add()` ,` search()` , and` get_all()` calls stay the same.
>
>
> -
>
>
> Token cost stays roughly constant per turn with Mem0. The naive stack's cost grows with every turn as history accumulates.


A 4B parameter model running locally on a laptop generated valid JSON on every single attempt. The same 4B model, on the same laptop, in the same minute, failed JSON parsing three times out of three. The model didn't change, nor did the hardware. The only thing that changed was how context reached the model: 407 tokens of raw conversation history versus 247 tokens of distilled memory from Mem0.


That 160-token difference is what separates a working agent from a broken one. Just the difference between dumping everything the user ever said into the system prompt and retrieving only the five facts that matter for this specific task.


This article builds both stacks from scratch, runs them head-to-head on the same code-generation task, and shows you exactly where the naive approach breaks and why.


> **⚡ Run this benchmark yourself. The full harness reproduces these numbers on any hardware running Ollama. A free Mem0 API key is all you need for the memory layer.**[Get a free Mem0 key.](https://mem0.ai/)


*When a context window fills up with history, three things happen at once.*


-


*Token costs scale with conversation length rather than task complexity.*


-


*Structured output reliability drops because the model navigates noise before it reaches the instruction it needs.*


-


*When the session ends, everything resets.*


[Mem0](https://mem0.ai/) breaks all three failure modes simultaneously. This article walks through exactly how that stack is built, right from configuration through benchmark, and ends with a note on swapping to a self-hosted setup if you need it.


## **The stack**


The complete system uses four components. Ollama handles all inference locally; Mem0 Platform handles memory storage and retrieval.


**Component**


**Role**


**Notes**


Mem0 Platform (` MemoryClient` )


Persistent memory: store, retrieve, search


Managed API with free tier available


Ollama


LLM inference and embeddings


Runs locally on macOS, Linux, Windows


Gemma 3 4B


Generation model


Via Ollama model library


nomic-embed-text


Embedding model


768 dimensions, via Ollama


Ollama exposes an OpenAI-compatible API at` http://localhost:11434/v1` , so the inference client uses the standard` openai` Python package without modification. Mem0 Platform manages vector storage and memory extraction on its end, so you interact with it through the` MemoryClient` SDK.


**Note:** The entire` requirements.txt` is three lines:` openai` ,` mem0ai` ,` ollama` .


## **Setting up Mem0**


Mem0 ships as two distinct interfaces.


-


The` MemoryClient` class is the platform path: Mem0 manages storage and extraction infrastructure, and you authenticate with an API key.


-


The` Memory` class is the self-hosted path where you provide the LLM, the embedder, and a vector store like Qdrant, and Mem0 runs the extraction logic on top.


This benchmark uses` MemoryClient` , and the setup is just three lines:


> **👉Wanna give it a try? Get a**[Mem0 API Key](https://app.mem0.ai/dashboard) **and try it yourself.**


### ` **memory_layer.py**` **: initialization**


```text
from mem0 import MemoryClient
import os
_mem = MemoryClient(api_key=os.environ ["MEM0_API_KEY"]


```


```text
from mem0 import MemoryClient
import os
_mem = MemoryClient(api_key=os.environ ["MEM0_API_KEY"]


```


```text
from mem0 import MemoryClient
import os
_mem = MemoryClient(api_key=os.environ ["MEM0_API_KEY"]


```


The` MEM0_API_KEY` environment variable is all the configuration required. Mem0 Platform handles vector indexing, deduplication, and extraction without any local infrastructure to manage. However, the Ollama models (Gemma 3 4B for generation) and nomic-embed-text for embeddings run locally and connect through the OpenAI-compatible endpoint.


If you later want to move everything on-premises, swapping to the self-hosted` Memory` class is a config change, not a rewrite.


## **The 3-block prompt architecture**


Every call to the agent builds a prompt with the same three-part structure. The shape is not arbitrary because each block serves a different purpose, and their order matters.


**Block**


**Content**


**Why is it first/second/last**


Static system rules


Behavioral contract, output format, hard constraints


Never changes; benefits from prefix caching on servers that support KV reuse


Mem0 distilled block


Retrieved preferences relevant to this query


Sits after the stable prefix, so cache hit is preserved; varies per request


User turn


Current request only, no history


The last position keeps the model's attention on the actual task


> **⭐️ Complete code for this project is available**[here](https://github.com/aashidutt-mem0/Build-a-Local-Coding-Agent-with-Mem0-and-Ollama)


### ` **agent** . **py**` :` **build_optimized_messages** ()`


```text
def build_optimized_messages(query: str, user_id: str) -> tuple[list [dict]


```


```text
def build_optimized_messages(query: str, user_id: str) -> tuple[list [dict]


```


```text
def build_optimized_messages(query: str, user_id: str) -> tuple[list [dict]


```


The static rules block serves a second purpose beyond instruction. Because it never changes between requests, inference servers that support KV cache reuse, including vLLM and partially Ollama, can reuse computed attention states for that prefix across calls. Placing the longest, most stable content first maximizes the chance of a prefix cache hit, reducing compute on every subsequent call to the same agent.


The Mem0 block sits after the static rules and before the user turn. Its content varies per request based on what semantic search retrieves, so the cache hit is on the stable prefix only, but that is still the majority of each prompt's length.


## **Storing and retrieving preferences**


The memory layer has two operations: retrieve before the call, write back after. The retrieve path searches Mem0 for memories relevant to the current query, sorts them for deterministic ordering, and formats them as a labeled block:


### ` **memory_layer.py**` **:**` **retrieve_context()**`


```text
def retrieve_context(query: str, user_id: str) -> str:
results = _mem.search(
query, filters={"user_id": user_id}, limit=5
)
if not results:
return ""
# unwrap paginated envelope: {"count": N, "results":  [...]  }
items = results.get("results", results) if isinstance(results, dict) else results
facts = sorted(r ["memory"]   for r in items if r.get("memory"))
lines = "\n".join(f"- {f}" for f in facts)
return f" [Persistent Context]


```


```text
def retrieve_context(query: str, user_id: str) -> str:
results = _mem.search(
query, filters={"user_id": user_id}, limit=5
)
if not results:
return ""
# unwrap paginated envelope: {"count": N, "results":  [...]  }
facts = sorted(r ["memory"]   for r in items if r.get("memory"))
lines = "\n".join(f"- {f}" for f in facts)
return f" [Persistent Context]


```


```text
def retrieve_context(query: str, user_id: str) -> str:
results = _mem.search(
query, filters={"user_id": user_id}, limit=5
)
if not results:
return ""
# unwrap paginated envelope: {"count": N, "results":  [...]  }
facts = sorted(r ["memory"]   for r in items if r.get("memory"))
lines = "\n".join(f"- {f}" for f in facts)
return f" [Persistent Context]


```


The` filters={"user_id": user_id}` syntax is important: current versions of the Mem0 SDK scope queries via a filters dictionary, not as a top-level keyword argument. Passing` user_id=` directly raises a` ValueError` . The` results.get("results", results)` unwrap handles Mem0's paginated response envelope before iterating.


The writeback path closes the loop after each successful generation:


### ` **memory_layer.py**` **:**` **writeback_from_response()**`


```text


```


```text


```


```text


```


This demo stores a task trace. A production version would parse the model's response for any newly expressed preferences like "actually use grid layout for this component" and store those as updated facts. Mem0's extraction layer handles deduplication: when a new fact contradicts an existing memory, it updates rather than appends.


What to store matters as much as how. Preferences and recurring patterns are the right candidates: "User prefers async/await over .then() chains", "User targets React 18 with TypeScript strict mode." Raw conversation turns are not as they add tokens without adding a signal, which is exactly the problem being solved. The naive benchmark stack is bloated not because it has too many facts, but because it includes all the conversational scaffolding around them.


> ***The demo pre-seeds Mem0 with 6 user preferences.** In a real agent, these accumulate naturally over time as the user corrects, refines, or adds to their preferences through normal interaction. The memory gets more useful with every session, not less.*


## **Structured output without guided decoding**


vLLM's` guided_json` feature enforces structured output at the logit level: the model is physically constrained to produce tokens forming valid JSON. It is the most reliable approach to structured generation, but it requires vLLM as the backend. Ollama does not expose this parameter.


The alternative is prompt-based enforcement with a single targeted retry. The JSON contract is injected at the end of the system message, which is the last thing the model processes before generating, and the retry gives the model its own failed output to correct rather than asking it to regenerate from scratch:


### ` **inference_client.py**` **:**` **call_optimized()**`


```text
# Appended to system message so the model sees the contract last
_JSON_CONTRACT = """
Respond ONLY with a valid JSON object — no markdown, no prose, no code fences.
The JSON must have exactly these keys: "code", "language", "file_path", "explanation".
"language" must be one of: tsx, ts, py, js, css, sh.
"""


def call_optimized(messages: list [dict]  , use_guided_json: bool = True) -> InferenceResult:
if use_guided_json:
patched = []
for m in messages:
if m ["role"]   == "system":
patched.append({"role": "system", "content": m ["content"]


```


```text
# Appended to system message so the model sees the contract last
_JSON_CONTRACT = """
Respond ONLY with a valid JSON object — no markdown, no prose, no code fences.
"language" must be one of: tsx, ts, py, js, css, sh.
"""


if use_guided_json:
patched = []
for m in messages:
if m ["role"]   == "system":
patched.append({"role": "system", "content": m ["content"]


```


```text
# Appended to system message so the model sees the contract last
_JSON_CONTRACT = """
Respond ONLY with a valid JSON object — no markdown, no prose, no code fences.
"language" must be one of: tsx, ts, py, js, css, sh.
"""


if use_guided_json:
patched = []
for m in messages:
if m ["role"]   == "system":
patched.append({"role": "system", "content": m ["content"]


```


The retry appends the failed response as an assistant turn and a correction as the next user turn. Asking the model to fix its own output is substantially more reliable than cold regeneration because it narrows the generation space that the model is revising rather than starting over.


The benchmark confirms this approach works. Across three runs, the naive stack produced unparseable output every time. The optimized stack with contract injection and retry succeeded every time.


## **The benchmark**


The harness runs a single task, i.e., "Generate a React TypeScript dashboard card component that displays ML pipeline TTFT and token count metrics", through both stacks in alternating sequence, averaged across three runs. The naive stack is intentionally loaded with authentic history: 18 turns of conversation that establish the same 6 preferences Mem0 stores as distilled facts. This represents how a real agent accumulates context over a session, not a contrived worst case.


**Metric**


**Naive stack**


**Optimized stack**


Context tokens


407


247


TTFT


374 ms


311 ms


Total latency


6.95 s


5.13 s


JSON parse success


0 / 3


3 / 3


Context reduction


Baseline (0%)


−39%


TTFT speedup


Baseline (1×)


1.2× faster


> **📊 0/3 vs. 3/3 JSON parse success with the same model.** That gap doesn't narrow with faster hardware. Test it on your own stack and[Run the harness.](https://mem0.ai/)


The headline figures, 39% context reduction and 1.2× TTFT speedup, are real but modest. On faster hardware or with a smaller model, the latency gap narrows. The JSON reliability gap does not narrow. That is the number worth focusing on.


The naive stack fails structured output because the model is navigating 407 tokens of history before it encounters the instruction to respond in JSON. By the time it reaches that instruction, the attention has been distributed across conversation scaffolding, style preferences, and follow-up questions that have nothing to do with the current task. The optimized stack delivers the same preferences in 247 tokens, cleanly labeled as` \[Persistent Context\]` , immediately before the task. The signal arrives without the noise.


> *At scale, the difference between 0% and 100% structured output reliability is not a performance consideration. It is the difference between a working agent and a broken one.*


Token reduction also compounds across a conversation. Each turn in the naive stack is longer than the last, because history grows. The optimized stack stays roughly constant: Mem0 retrieves a fixed window of relevant memories per query, i.e., five results in this implementation rather than appending the full conversation. An agent that runs 20 turns does not pay 20× the first-turn token cost.


If your deployment requires data residency or a fully air-gapped operation, Mem0's open-source` Memory` class lets you bring your own vector store and embedding model. The swap is a config change. The` add()` ,` search()` , and` get_all()` method signatures stay the same on both sides:


> **🚀 Same interface, your choice of backend.** Start with the platform today, move self-hosted when your requirements change. No code to rewrite either way.[Get your free Mem0 API key →](https://mem0.ai/)


## **Run it yourself**


Clone the repo, install the packages, and run the benchmark. The full source for all files in this article is in the demo repository. The benchmark harness at` benchmark.py --runs 3` reproduces the numbers above on any hardware running Ollama.


**01** Get a free Mem0 API key and set` MEM0_API_KEY` in your environment


**02** Install Ollama and pull the models:` ollama pull gemma3:4b && ollama pull nomic-embed-text`


**03**` pip install openai mem0ai`


**04** Clone the ****[GitHub Repo and run](https://github.com/aashidutt-mem0/Build-a-Local-Coding-Agent-with-Mem0-and-Ollama)


**05**` python benchmark.py --runs 3`


## **Further Reading**


-


[Adding Persistent Memory to Local AI Agents with Ollama](https://mem0.ai/blog/adding-persistent-memory-to-local-ai-agents-with-mem0-openclaw-and-ollama)


-


[Context Window vs Persistent Memory: Why 1M Tokens Isn't Enough](https://mem0.ai/blog/context-window-vs-persistent-memory-why-1m-tokens-isn-t-enough)


-


[Codex + Mem0 MCP: Build a Coding Agent That Remembers Your Codebase](https://mem0.ai/blog/codex-mem0-mcp-build-a-coding-agent-that-remembers-your-codebase)


-


[How to Reduce LLM Token Costs for AI Agent Memory](https://mem0.ai/blog/6-techniques-to-cut-ai-agent-memory-cost-beyond-basic-retrieval)


-


[Why Stateless Agents Fail at Personalization](https://mem0.ai/blog/why-stateless-agents-fail-at-personalization)
