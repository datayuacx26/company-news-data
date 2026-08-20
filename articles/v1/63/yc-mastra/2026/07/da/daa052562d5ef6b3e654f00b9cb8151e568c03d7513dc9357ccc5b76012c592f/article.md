---
schema_version: "1.0.0"
document_id: "daa052562d5ef6b3e654f00b9cb8151e568c03d7513dc9357ccc5b76012c592f"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/agent-memory-layers"
published_at: "2026-07-25T00:00:00+00:00"
first_seen_at: "2026-07-26T15:27:54.793425+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:4247e9847d6284334c1f5f57658743cc568f1ca5e6b0611726bd17a707b64229"
---

# AI Agent Memory: Persistence, Layers, and Tradeoffs

Mastra memory stores conversation data outside the model. On each call, Mastra chooses which stored messages and memory records to send back to the model. To pin down what that means in practice, I ran the memory stack locally and tested what actually survives a process restart; the configuration and the result are later in this article.


In Mastra, a thread is a single conversation and a resource is the user or project that owns it. Mastra uses those IDs to load the right messages and memory records for each call.


## Why do agents need memory?


Every API call starts fresh. The model only sees the instructions and context that the application sends with that call. Without memory, it cannot know what someone said earlier, which task is still open, or which customer record belongs to the conversation.


The context window is limited, so sending an entire archive back on every call is not a memory strategy. The application needs to choose the records that matter for the next response, place them in the model input, and keep the full record outside the model for later use. That is what gives a conversation continuity without treating the context window as a database.


## What are the different types of memory in Mastra?


Mastra has four types of memory:


- Message history
- Working memory
- Semantic recall
- Observational memory


Mastra keeps them separate because they are read in different ways.


### Message history


Message history gives the model the recent messages from the current conversation. When someone says “do the second one,” the model can see the earlier options and know what the request refers to.


` lastMessages` sets how many messages Mastra sends. Mastra loads the newest messages in that limit and puts them back in conversation order before sending them to the model.


### Working memory


Working memory is a short document stored with a thread or resource. Mastra adds it to the system message for calls using that thread or resource, so the model does not have to search old messages for it.


For a support resource, that record might hold:


- A preferred contact method
- The current plan
- An unresolved issue
- A constraint the agent must honor


Use working memory for information the agent should see on every call. Keep permissions and payment status in the application system that owns them. The memory record can be read-only or point to the current value.


### Semantic recall


Semantic recall uses a vector store to find older messages related to the current request. Mastra sends each match with nearby messages so the model can see the conversation around it.


Thread-scoped recall searches one conversation. Resource-scoped recall can search that resource’s threads, but Mastra rejects the request when it does not include a resource ID. A missing ID never expands the search.


### Observational memory


Observational memory is for a thread that no longer fits in the message window. It creates observations from the older messages and sends those observations to the model instead of sending every old message.


The messages after the observation boundary stay in the prompt as raw conversation. Mastra keeps the original messages in storage, so the observation does not replace the record.


## How does agent memory persist?


The model does not keep a conversation after a call ends. Its context window only contains the input for that call. Agent memory starts with a durable store that keeps the conversation and any memory records after the model returns.


Each request names a thread and usually a resource. Mastra uses those IDs to read the records for that conversation and owner, then builds a bounded model input from them. The model receives the selected context, not the database itself. After the call, Mastra stores the new messages and runs the memory updates configured for that agent.


That separation is what makes a restart ordinary. A new process has no copy of the old context window, but it can load the same thread and resource from storage and assemble a new context window for the next call. A file-backed SQLite store, a database, or another durable backend decides how long those records remain available.


## What are the tradeoffs between agent memory layers?


Each layer solves a different problem and adds a different cost:


- Message history is simple and exact, but the prompt gets larger as the thread grows
- Working memory is predictable because it is always in system context, but the application needs a clear update policy
- Semantic recall can find older messages, but it needs embeddings and can retrieve a message that is related without being useful
- Observational memory keeps long threads small, but the observations need to be checked against the original messages


Use the smallest layer that answers the problem. A support agent with short conversations may only need message history and working memory. A research agent that returns to old work may also need semantic recall or observational memory.


## How does Mastra assemble memory context for an agent?


Before a model call, Mastra reads the thread and resource IDs from the request. It loads the messages and memory records for those IDs. It then sends the selected context to the model. After the call, it stores the new messages and applies the configured memory updates.


For a run with every layer enabled, the context comes together in this order:


- Resolve the thread and resource
- Load the recent messages from the thread
- Add working memory to system context
- Retrieve relevant older messages through semantic recall
- Add observational memory for the older portion of the conversation


## Why are a thread and a resource separate?


Consider a customer who opens a new billing conversation next month. The new thread starts with no earlier messages, but the agent can still read the customer record for the same resource. Keeping the IDs separate gives the application both behaviors.


The same rule applies to retrieval. A cross-thread search needs a resource ID. If the request does not provide one, Mastra does not search other threads.


## What does a persistence configuration look like?


This is the configuration I used to test persistence for this article. It gives a support agent:


- A local SQLite file for its conversations
- A limited window of recent messages
- A customer record for every call


```text
import   {   Agent   }   from   '  @mastra/core/agent  '
import   {   LibSQLStore   }   from   '  @mastra/libsql  '
import   {   Memory   }   from   '  @mastra/memory  '


const   storage   =   new   LibSQLStore  ({
id:   '  support-memory  '  ,
url:   '  file:./support-agent.db  '  ,
})


const   memory   =   new   Memory  ({
storage,
options: {
lastMessages:   16  ,
workingMemory: {
enabled:   true  ,
scope:   '  resource  '  ,
template:   `  # Customer context
- Preferred contact method
- Current plan
- Open issue
- Important constraint  `  ,
},
},
})


const   supportAgent   =   new   Agent  ({
id:   '  support-agent  '  ,
name:   '  Support Agent  '  ,
memory,
})
```


This configuration makes three choices:


- **` LibSQLStore` with a` file:` URL** keeps messages in a SQLite file after the process restarts
- **` lastMessages: 16`** sends at most 16 messages from one thread to the model
- **` workingMemory` with` scope: 'resource'`** sends the customer record with every call for that customer


I tested the LibSQL setup by saving two messages through one` Memory` instance. I then created a new instance with the same file, resource, and thread.` recall` returned both messages. The test covers message history persistence only. It does not test semantic recall or observational memory.


The working-memory template is plain Markdown. A support teammate can read it and change it when it is wrong.


The request names both scopes.


```text
await   supportAgent.  generate  (  '  Can you check my renewal?  '  ,   {
memory: {
resource:   '  customer-42  '  ,
thread:   '  billing-2026-07  '  ,
},
})
```


That call reads the billing messages and the customer record. On a new billing thread, the model starts with no earlier messages and still receives the record for` customer-42` .


[Semantic recall](https://mastra.ai/docs/memory/semantic-recall) is for finding older messages.[Working-memory updates](https://mastra.ai/docs/memory/working-memory) change the customer record.[Observational memory](https://mastra.ai/docs/memory/observational-memory) gives a long thread a shorter record.


## Other memory architectures


While we built the Mastra memory around the use-cases we observed, research projects make different choices about what to store and what to send back to the model. Here are some of the papers are interesting foundations.


### 1. MemGPT treats memory like an operating system


[MemGPT](https://arxiv.org/abs/2310.08560) (Packer et al., arXiv:2310.08560) compares the context window to RAM. It holds:


- System instructions
- A writable working context
- A FIFO message queue


Recall storage and archival storage stay on disk. The model decides what to page in and out through function calls.


### 2. A-MEM makes memories link to each other


[A-MEM](https://arxiv.org/abs/2502.12110) (Xu et al., arXiv:2502.12110) borrows the Zettelkasten method. Every memory becomes a structured note with:


- A timestamp
- Keywords
- Tags
- A context description


New notes link to related old notes. A memory-evolution step can rewrite an old note when new information changes what it means.


### 3. MemoryOS memory tiers


[MemoryOS](https://arxiv.org/abs/2506.06326) has three personal-memory tiers:


- Short-term memory
- Mid-term memory
- Long-term memory


Dialogue-chain FIFO updates move memory from short-term to mid-term. Segmented paging promotes it to long-term. On the LoCoMo benchmark, the authors report an average F1 improvement of 49.11% over baselines on GPT-4o-mini. The paper shows the architecture in detail.


### 4. Zep gives every fact a validity interval


[Zep](https://arxiv.org/abs/2501.13956) (Rasmussen et al., arXiv:2501.13956) stores memory as a temporal knowledge graph. Each fact records when it became true and when it stopped being true. That lets Zep replace “the renewal is on the 15th” without deleting the older fact.


The paper reports 94.8% on the Deep Memory Retrieval benchmark against MemGPT's 93.4%. It also reports up to 18.5% accuracy improvement on LongMemEval and about 90% lower response latency than full-context baselines.


## What memory layer to build first?


If I were building a new agent today, I’d begin with stored message history and resource boundaries. Everything else depends on that layer.


Working memory is useful when the agent needs the same short record in more than one thread. Semantic recall is for finding older messages. Observational memory is for threads that are too long to send as raw history.


Each memory layer needs its own check:


- Start a new thread and verify the correct resource record is present
- Retrieve an older decision and inspect the messages around it
- Compare an observation with the messages that produced it


These checks show which part needs work:


- Scope
- Retrieval
- Compression


They also make it easier to see how Mastra builds the model input.


## FAQs about agent memory


### Is agent memory the same as the context window?


No. A context window is the content visible during one model call. Memory is the durable record Mastra can load for a later call.


### Do I need a vector database for agent memory?


No. Recent history and working memory use direct storage reads. Add a vector index when the agent needs to locate related material in a larger message archive.


### Where should payment and permission data live?


Keep the authoritative value in the application system that owns it. Memory can hold a reference or readable summary, but the agent should read the current value before taking an action that depends on it.


### Can the Open Knowledge Format serve as a memory layer?


It can provide the knowledge layer around an agent. It does not replace conversation history or workflow persistence.


### How is memory different from workflow persistence?


Memory gives a later model call context about prior work. Workflow persistence keeps the state required to resume interrupted work.
