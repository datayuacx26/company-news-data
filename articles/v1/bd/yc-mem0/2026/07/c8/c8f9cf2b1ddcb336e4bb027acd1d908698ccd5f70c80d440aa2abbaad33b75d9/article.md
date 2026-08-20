---
schema_version: "1.0.0"
document_id: "c8f9cf2b1ddcb336e4bb027acd1d908698ccd5f70c80d440aa2abbaad33b75d9"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/5-breakthrough-papers-shaping-ai-agent-memory-at-icml-2026"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-24T17:53:59.780181+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:ea4eb81d4f0350d1d6f85268d8b761deeaab9f1cd4d84e1c933ff36217c3f721"
---

# Top 5 AI Agent Memory Papers from ICML 2026

The[2026 ICML](https://icml.cc/) memory papers report clear quantitative gains in accuracy and cost, yet the most cited finding points in a different direction. MemoryArena shows that agents that appear to have solved existing dialogue benchmarks fail on realistic multi-session tasks.


The pattern is direct. Retrieval keeps getting better, compression techniques are effective, and structured processes beat raw generation. Evaluation still lags behind real workloads, which is why MemoryArena dominates the hallway conversations and post-conference discussions.


## **Efficiently cutting the token and computing cost of memory**


Efficiency is the most straightforward win for production agents. Several papers stand out for concrete gains on both accuracy and cost, with SimpleMem and EAM for GUI agents providing representative examples of structured retrieval applied in different domains.


SimpleMem reports notable improvements in F1 scores together with large reductions in memory tokens. The method combines compression with intent-aware retrieval. Instead of dumping all historical context into the prompt, SimpleMem clusters and compresses memories around user intents, then retrieves compressed summaries aligned to the current query type. This is retrieval as a structured process, not a blind context stuffing strategy.


EAM for GUI agents targets a different domain. It reports accuracy gains alongside substantial token reductions. The core idea is to track application state and actions as a knowledge graph and retrieve graph segments relevant to the current GUI task. Memory retrieval happens over structured nodes and edges, not free-form generated notes.


Both methods underline the same theme. When memory is treated as a structured process, rather than raw text generation, agents become cheaper and more accurate.


## **SimpleMem compresses and aligns memory to intent**


[SimpleMem](https://icml.cc/virtual/2026/poster/61640) is built around three steps.


-


First, it compresses past interactions. Instead of saving every utterance verbatim, it performs hierarchical clustering over interaction segments, then uses a small language model to generate concise summaries per cluster. These compressed summaries form the long-term memory.


-


Then, it learns intent centroids. Supervision comes from task labels and heuristic signals such as API calls, tools used, or explicit user goals. Intents form a low-dimensional space where memory items can be aligned.


-


Retrieval is intent-aware. Given a new query, SimpleMem predicts its intent, finds the nearest compressed summaries, and constructs the context from those items only. The result is a narrower, more relevant context window that avoids general keyword-based search over all history.


The paper’s key result is that compression plus intent alignment yields better F1 scores than simple vector search over full transcripts, while cutting token usage significantly on long-running agents without any custom hardware.


SimpleMem does not fully solve temporal reasoning. It treats memory items independently and relies on compressive summaries to maintain local time order. For multi-session dependencies, a more explicit temporal model is still needed.


## **EAM for GUI agents structured state over knowledge graphs**


[EAM for GUI agents](https://icml.cc/virtual/2026/poster/65931) focuses on agents that control applications through GUI actions. Instead of free-form textual memory, it constructs an explicit knowledge graph of the user’s actions and application state.


Nodes represent windows, widgets, user inputs, and key application entities. Edges capture relationships and temporal transitions, such as a button press triggering a state change. Long-term memory is this evolving graph, not a scroll of dialogues.


Retrieval happens over the graph. When the agent faces a new task, it queries the graph for relevant subgraphs, such as prior configurations of the same form or similar sequences of clicks that led to success. The retrieved subgraph is serialized into a compact textual context for the language model.


This yields measurable accuracy improvements and substantial reductions in prompt tokens for GUI agents. The structured approach makes it easier to avoid irrelevant logs and focus on the exact slice of past behavior that matters.


However, EAM is domain-specific. It shines on GUI workflows but does not directly apply to open-ended dialogue or multi-tool agents without careful schema design.


## **The evaluation gap**


MemoryArena is the standout benchmark paper at ICML 2026. Its main result is uncomfortable and useful. Agents that saturate LoCoMo and similar long context dialogue benchmarks perform poorly on realistic interdependent multi-session tasks.


MemoryArena defines evaluation suites where user sessions span days or weeks, and where later tasks depend on decisions taken in earlier sessions. There are hidden dependencies, partial information, and cases where forgetting is actually beneficial. Traditional dialogue benchmarks that focus on single-session recall fail to capture this shape.


The paper’s most quotable point is simple. Scores on LoCoMo and related benchmarks do not predict performance on realistic multi-session agents. In fact, several high-scoring systems fail basic tasks such as correctly reusing prior preferences across sessions without overfitting to noise.


This validates a pattern that many teams have suspected, and that Mem0’s own writing has flagged for some time. Dialogue benchmarks are not equivalent to real agent tasks. Evaluating on narrow recall metrics hides issues such as over retention, misaligned forgetting, and failures to reconcile conflicting information over time.


MemoryArena does not claim to be a perfect benchmark. It is an early attempt to model session structure, temporal dependencies, and cross-session goals. But it clearly shows that agent memory needs evaluation grounded in long-running workflows, not just synthetic recall tasks.


## **Rethinking how memory is structured, not just retrieved**


Two additional ICML 2026 papers push beyond retrieval efficiency into the shape of memory itself. MRAgent and MemEvolve both argue that memory is active and dynamic, not a passive store.


MRAgent starts from the premise that memory is reconstructed, not retrieved. Instead of pulling static snippets, it builds an evolving graph of entities, events, and relations, and reconstructs a situation-specific memory view for each query.


MemEvolve applies meta-learning to memory, modifying both the stored knowledge and the architecture that uses it. Over time, the system optimizes not just what it remembers, but how it remembers and retrieves.


These approaches address two shortcomings of pure retrieval systems. They allow agents to reconcile conflicting information and adapt memory structures to changing tasks. They also better match human-like memory, where recollection is a constructive process.


## **MRAgent graph-based active reconstruction**


[MRAgent](https://arxiv.org/abs/2606.06036) builds memory as a dynamic knowledge graph. Each interaction updates nodes and edges that represent entities, facts, events, and relations. Edges carry temporal and causal labels that can be used to reconstruct timelines and chains of reasoning.


When the agent receives a new query, MRAgent does not retrieve a fixed set of past snippets. Instead, it runs a reconstruction procedure. This procedure traverses the graph, selects relevant subgraphs, and composes a situation-specific memory view, often in the form of a narrative or structured summary tailored to the query.


This active reconstruction yields clear gains on benchmarks such as LoCoMo and LongMemEval. The gains are largest in scenarios with conflicting information or long chains of dependent events. MRAgent can suppress outdated facts and highlight current ones by using temporal signals in the graph.


The tradeoff is complexity. Maintaining and traversing a rich graph adds compute and design overhead. For simple single-session tasks, plain retrieval remains faster and sufficient.


## **MemEvolve jointly evolving knowledge and architecture**


[MemEvolve](https://icml.cc/virtual/2026/poster/61379) treats memory as a meta-learning problem. It does not fix the format of memory or the retrieval strategy. Instead, it trains a controller that co-evolves the knowledge store and the architecture that uses it.


The system starts with a base memory representation, such as key-value pairs or small graphs, and a retrieval policy. During training, it explores changes to both the representation and the policy, such as adding new fields, restructuring clusters, or modifying retrieval heuristics. Rewards come from downstream agent performance on varied tasks.


MemEvolve reports meaningful gains over strong baselines and shows cross-LLM generalization. Memory policies learned with one language model can transfer to another, provided the interfaces are compatible. This suggests that memory management can be treated as a model-agnostic capability.


In practice, MemEvolve is harder to deploy than SimpleMem. It requires long training runs and careful reward design. For production teams, it may be more suitable as a lab technique for discovering good memory patterns, rather than deploying a fixed version in production.


## **Comparison of key ICML 2026 memory approaches**


The main ICML 2026 papers can be usefully compared along several dimensions relevant to production agents.


**Paper / Approach**


**Core Idea**


**Primary Gain**


**Token / Cost Impact**


**Best Use Case**


SimpleMem


Compression plus intent aware retrieval


Improved F1 and relevance


Large token reduction


General task-oriented dialogue agents


EAM for GUI


Knowledge graph over the GUI state


Accuracy improvement


Significant token reduction


Application and GUI control agents


MemoryArena


Multi-session realistic benchmark


Better evaluation realism


Neutral


Testing long-running agents


MRAgent


Graph-based active reconstruction


Benchmark performance gains


Moderate overhead


Agents with complex temporal reasoning


MemEvolve


Meta evolving memory and architecture


Performance gains over baselines


Training cost increase


Research and adaptive memory systems


Each approach covers a different part of the memory problem. SimpleMem and EAM focus on efficiency and practical gains. MemoryArena addresses evaluation. MRAgent and MemEvolve explore new structures and adaptive methods.


## **How Mem0 fits into this landscape**


[Mem0](https://mem0.ai/) is designed around the same core themes that appear across these papers. It treats memory as a structured process, supports multi-session scopes, and makes retrieval explicit rather than relying on raw context stuffing.


Mem0 provides an API and storage layer for long-term memories with metadata, timestamps, and user-scoped contexts. Retrieval combines semantic search with filters such as session, task, and type, in a manner similar to intent-aware retrieval in SimpleMem. For GUI and tool-based agents, Mem0 can store structured state transitions and expose them for selective retrieval, close to the ideas in EAM.


On the evaluation side, Mem0 does not define a benchmark, but it was built with the MemoryArena pattern in mind. Multi-session data can be stored and queried as realistic sequences, allowing teams to run their own task-shaped evaluations rather than relying on single-session recall scores.


For teams that want more advanced structures, Mem0 can store graph-like data and reconstructed summaries as first-class objects. This makes it possible to implement MRAgent-style reconstruction on top of Mem0’s storage and retrieval primitives.


## **Integrating Mem0 into a production agent**


Mem0 can be integrated into an agent pipeline with a few clear steps. The example below uses Python and assumes a simple conversational agent, but the same pattern applies to tool calling or GUI agents.


> [👉Get a free API key at app.mem0.ai](https://app.mem0.ai/) **to follow along (free tier, no credit card, includes all the**` **add()**` **and**` **search()**` **calls shown below).**


```text
import    os
import    openai
from    mem0   import    MemoryClient


#  Configure   APIs
openai  . api_key   =  os  . getenv  (  "OPENAI_API_KEY"  )
mem0_api_key   =  os  . getenv  (  "MEM0_API_KEY"  )


mem0   =  MemoryClient  (  api_key  = mem0_api_key  )


USER_ID   =  "user_123"    #  Stable   identifier   across   sessions


def   store_memory  (  user_id  ,    text  ,    metadata  = None  )  :
""  "Store a memory item in Mem0 with optional metadata."  ""
mem0  . add  (
[  {  "role"  :    "user"  ,    "content"  :    text  }  ]  ,
user_id  = user_id  ,
metadata  = metadata   or  {  }  ,
)


def   retrieve_memories  (  user_id  ,    query  ,    limit  = 5  )  :
""  "Retrieve relevant memories for the current query."  ""
results   =  mem0  . search  (
query  ,
filters  = {  "user_id"  :    user_id  }  ,
limit  = limit  ,
)
return    [  m  [  "memory"  ]    for    m    in    results  . get  (  "results"  ,    [  ]  )  ]


def   build_prompt  (  user_input  ,    retrieved_memories  )  :
""  "Construct the model prompt using structured retrieval."  ""
memory_block   =  "\n"  . join  (  f  "-  {  m  }  " for m in retrieved_memories)
prompt   =  f  ""  "
You   are   a   production   assistant   agent  .


Relevant    long  - term   context :
{  memory_block  }


Current   user   input :
{  user_input  }


Use   the   context   when   helpful  ,    but   do    not   repeat   it   verbatim  .
Respond    concisely   and   ask   for    clarification   if    needed  .
""  "
return    prompt  . strip  (  )


def   chat_with_agent  (  user_input  )  :
#  Structured   retrieval :    query   Mem0   for    relevant   past   context
memories   =  retrieve_memories  (  USER_ID  ,    query  = user_input  )


prompt   =  build_prompt  (  user_input  ,    memories  )


response   =  openai  . ChatCompletion  . create  (
model  = "gpt-4.1-mini"  ,
messages  = [  {  "role"  :    "user"  ,    "content"  :    prompt  }  ]  ,
)


reply   =  response  . choices  [  0  ]  . message  [  "content"  ]  . strip  (  )


#  Store   new    memory   with    simple   intent  - aware   metadata
metadata   =  {
"type"  :    "conversation"  ,
"intent_hint"  :    "support"  ,    #  could    be   predicted   by   a   classifier
}
store_memory  (  USER_ID  ,    text  = user_input  ,    metadata  = metadata  )
store_memory  (  USER_ID  ,    text  = reply  ,    metadata  = {  "type"  :    "agent_response"  }  )


return    reply


if    __name__   ==  "__main__"  :
#  Example   loop
print  (  "Agent with Mem0 memory. Type 'exit' to quit."  )
while   True :
user_input   =  input  (  "You: "  )  . strip  (  )
if    user_input  . lower  (  )   ==  "exit"  :
break
answer   =  chat_with_agent  (  user_input  )
print  (  f  "Agent: {answer}"  )
```


```text
import    os
import    openai
from    mem0   import    MemoryClient


#  Configure   APIs
openai  . api_key   =  os  . getenv  (  "OPENAI_API_KEY"  )
mem0_api_key   =  os  . getenv  (  "MEM0_API_KEY"  )


mem0   =  MemoryClient  (  api_key  = mem0_api_key  )


USER_ID   =  "user_123"    #  Stable   identifier   across   sessions


def   store_memory  (  user_id  ,    text  ,    metadata  = None  )  :
""  "Store a memory item in Mem0 with optional metadata."  ""
mem0  . add  (
[  {  "role"  :    "user"  ,    "content"  :    text  }  ]  ,
user_id  = user_id  ,
metadata  = metadata   or  {  }  ,
)


def   retrieve_memories  (  user_id  ,    query  ,    limit  = 5  )  :
""  "Retrieve relevant memories for the current query."  ""
results   =  mem0  . search  (
query  ,
filters  = {  "user_id"  :    user_id  }  ,
limit  = limit  ,
)


def   build_prompt  (  user_input  ,    retrieved_memories  )  :
""  "Construct the model prompt using structured retrieval."  ""
prompt   =  f  ""  "
You   are   a   production   assistant   agent  .


Relevant    long  - term   context :
{  memory_block  }


Current   user   input :
{  user_input  }


Respond    concisely   and   ask   for    clarification   if    needed  .
""  "
return    prompt  . strip  (  )


def   chat_with_agent  (  user_input  )  :
#  Structured   retrieval :    query   Mem0   for    relevant   past   context
memories   =  retrieve_memories  (  USER_ID  ,    query  = user_input  )


prompt   =  build_prompt  (  user_input  ,    memories  )


response   =  openai  . ChatCompletion  . create  (
model  = "gpt-4.1-mini"  ,
messages  = [  {  "role"  :    "user"  ,    "content"  :    prompt  }  ]  ,
)


#  Store   new    memory   with    simple   intent  - aware   metadata
metadata   =  {
"type"  :    "conversation"  ,
}
store_memory  (  USER_ID  ,    text  = user_input  ,    metadata  = metadata  )


return    reply


if    __name__   ==  "__main__"  :
#  Example   loop
print  (  "Agent with Mem0 memory. Type 'exit' to quit."  )
while   True :
user_input   =  input  (  "You: "  )  . strip  (  )
if    user_input  . lower  (  )   ==  "exit"  :
break
answer   =  chat_with_agent  (  user_input  )
print  (  f  "Agent: {answer}"  )
```


```text
import    os
import    openai
from    mem0   import    MemoryClient


#  Configure   APIs
openai  . api_key   =  os  . getenv  (  "OPENAI_API_KEY"  )
mem0_api_key   =  os  . getenv  (  "MEM0_API_KEY"  )


mem0   =  MemoryClient  (  api_key  = mem0_api_key  )


USER_ID   =  "user_123"    #  Stable   identifier   across   sessions


def   store_memory  (  user_id  ,    text  ,    metadata  = None  )  :
""  "Store a memory item in Mem0 with optional metadata."  ""
mem0  . add  (
[  {  "role"  :    "user"  ,    "content"  :    text  }  ]  ,
user_id  = user_id  ,
metadata  = metadata   or  {  }  ,
)


def   retrieve_memories  (  user_id  ,    query  ,    limit  = 5  )  :
""  "Retrieve relevant memories for the current query."  ""
results   =  mem0  . search  (
query  ,
filters  = {  "user_id"  :    user_id  }  ,
limit  = limit  ,
)


def   build_prompt  (  user_input  ,    retrieved_memories  )  :
""  "Construct the model prompt using structured retrieval."  ""
prompt   =  f  ""  "
You   are   a   production   assistant   agent  .


Relevant    long  - term   context :
{  memory_block  }


Current   user   input :
{  user_input  }


Respond    concisely   and   ask   for    clarification   if    needed  .
""  "
return    prompt  . strip  (  )


def   chat_with_agent  (  user_input  )  :
#  Structured   retrieval :    query   Mem0   for    relevant   past   context
memories   =  retrieve_memories  (  USER_ID  ,    query  = user_input  )


prompt   =  build_prompt  (  user_input  ,    memories  )


response   =  openai  . ChatCompletion  . create  (
model  = "gpt-4.1-mini"  ,
messages  = [  {  "role"  :    "user"  ,    "content"  :    prompt  }  ]  ,
)


#  Store   new    memory   with    simple   intent  - aware   metadata
metadata   =  {
"type"  :    "conversation"  ,
}
store_memory  (  USER_ID  ,    text  = user_input  ,    metadata  = metadata  )


return    reply


if    __name__   ==  "__main__"  :
#  Example   loop
print  (  "Agent with Mem0 memory. Type 'exit' to quit."  )
while   True :
user_input   =  input  (  "You: "  )  . strip  (  )
if    user_input  . lower  (  )   ==  "exit"  :
break
answer   =  chat_with_agent  (  user_input  )
print  (  f  "Agent: {answer}"  )
```


This pattern illustrates several ICML 2026 themes in a practical form. Retrieval is structured and query aware, not a dump of all transcripts. Memory is multi-session, keyed by user identifiers. Metadata allows future extensions such as intent clustering or knowledge graph reconstruction without changing the core agent logic.


For GUI agents, the same approach can store structured events:


```text
def   log_gui_action  (  user_id  ,    window  ,    widget  ,    action  ,    value  = None  )  :
mem0  . add  (
[  {  "role"  :    "user"  ,    "content"  :    f  "GUI    action: {action }   on  {  widget  }    in    {  window  }  "}],
user_id  = user_id  ,
metadata  = {
"type"  :    "gui_action"  ,
"window"  :    window  ,
"widget"  :    widget  ,
"action"  :    action  ,
"value"  :    value  ,
}  ,
)
```


```text
mem0  . add  (
user_id  = user_id  ,
metadata  = {
"type"  :    "gui_action"  ,
"window"  :    window  ,
"widget"  :    widget  ,
"action"  :    action  ,
"value"  :    value  ,
}  ,
)
```


```text
mem0  . add  (
user_id  = user_id  ,
metadata  = {
"type"  :    "gui_action"  ,
"window"  :    window  ,
"widget"  :    widget  ,
"action"  :    action  ,
"value"  :    value  ,
}  ,
)
```


A retrieval query can then filter by` "type": "gui_action"` and specific window or widget names, which closely match the EAM knowledge graph intuition.


## **Limitations of current memory patterns**


The ICML 2026 papers and the patterns described here do not solve all aspects of agent memory.


-


Temporal and causal reasoning is still brittle. Even graph-based methods like MRAgent can struggle with very long chains of events or conflicting updates over months. Production agents often need domain-specific logic on top of memory.


-


Forgetting strategies are underexplored. Most systems focus on recall, but practical agents must discard or downrank information that becomes outdated or irrelevant. Automatic forgetting policies that balance privacy, utility, and model quality remain an open area.


-


Cross-user and cross-agent memory sharing is barely addressed. Today’s methods assume mostly isolated users and single agent architectures. Multi-agent systems and organizational knowledge sharing call for new privacy and synchronization mechanisms.


-


Evaluation benchmarks like MemoryArena are at an early stage. They highlight problems with legacy benchmarks, but they do not yet cover the full diversity of real-world workflows. Teams still need task-specific evaluation harnesses to validate memory behavior.


## **Frequently Asked Questions**


### Q. Is agent memory a solved problem?


Agent memory is not solved. Efficiency and retrieval have improved significantly, but realistic multi-session tasks still expose failures in recall, forgetting, and temporal reasoning. Current research offers strong building blocks, not complete solutions.


### Q. What is the difference between retrieval and reconstruction in memory systems?


Retrieval focuses on finding and returning stored items that match a query, typically using semantic search or filtering. Reconstruction creates a new situation-specific view of memory by combining and interpreting stored information, often using graphs or generative models to resolve conflicts and highlight relevant details.


### Q. Why do multi-session benchmarks like MemoryArena matter?


Multi-session benchmarks capture longer time horizons and interdependent tasks that resemble real user behavior. They show that agents can score well on single-session recall yet fail at maintaining coherent preferences and decisions over time.


### Q. How does Mem0 help with agent memory in production?


Mem0 provides an explicit memory layer with APIs for adding, searching, and organizing long-term data across sessions. It supports structured retrieval, metadata, and multi-user scopes so teams can implement the efficiency and evaluation patterns highlighted by recent research without building their own storage and retrieval infrastructure.


## **Further Reading**


-


[The 2026 Token Optimization Playbook Cut AI Agent Memory Costs](https://mem0.ai/blog/the-2026-token-optimization-playbook-cut-ai-agent-memory-costs-3%e2%80%934x)


-


[State Of AI Agent Memory 2026](https://mem0.ai/blog/state-of-ai-agent-memory-2026)


-


[Give Your AI Agent Memory And Guardrails Mem0 Docker Sandboxes](https://mem0.ai/blog/give-your-ai-agent-memory-and-guardrails-mem0-docker-sandboxes)


-


[Memory Poisoning: How Bad Inputs Corrupt Your AI Agent's Memory](https://mem0.ai/blog/memory-poisoning-how-bad-inputs-corrupt-your-ai-agent-s-memory)


—


[Mem0](https://mem0.ai/) *is an intelligent, open-source memory layer designed for LLMs and AI agents to provide long-term, personalized, and context-aware interactions across sessions.*


Get your free API Key here:[app.mem0.ai](https://app.mem0.ai/?utm_source=mem0_blog&utm_medium=kw_blog&utm_campaign=5_breakthrough_papers_shaping_ai_agent_memory_at_icml_2026&utm_content=5_breakthrough_papers_shaping_ai_agent_memory_at_icml_2026) or self-host mem0 from our[open source GitHub repository](https://github.com/mem0ai/mem0) .


—
