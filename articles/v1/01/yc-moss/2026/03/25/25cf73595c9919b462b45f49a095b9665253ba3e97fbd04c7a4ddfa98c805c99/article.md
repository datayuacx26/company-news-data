---
schema_version: "1.0.0"
document_id: "25cf73595c9919b462b45f49a095b9665253ba3e97fbd04c7a4ddfa98c805c99"
company_key: "yc-moss"
company: "Moss"
source_id: "yc-moss-rss-e49397d0a304"
canonical_url: "https://www.moss.dev/blog/remove-the-network-hop"
published_at: "2026-03-17T00:00:00+00:00"
first_seen_at: "2026-07-27T03:53:19.807128+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:a56392302c05cdfeacb5f19c5476ccae72186ae2266e6d703008c7fb27d6f075"
---

# What Happens When You Remove the Network Hop from RAG

In our[last post](https://www.moss.dev/blog/retrieval-latency-tax) , we made the case that retrieval, not the LLM, is the dominant latency bottleneck in real-time AI applications. We showed the numbers. We named the problem.


This post is about what happens when you fix it.


We took a production RAG pipeline, a voice agent doing knowledge-base lookups over a managed vector database, and ran a controlled experiment. Same data. Same queries. Same embedding model. Same LLM. The only variable: where the retrieval happens. In one configuration, retrieval goes over the network to a cloud-hosted vector database. In the other, the index lives in the same process as the agent, and retrieval is a local function call.


The difference isn't incremental. It's architectural.


## The Baseline: A Typical Cloud RAG Pipeline


Here's the setup we profiled. It's not a strawman. It's what most production RAG applications look like today.


A voice agent running on a cloud VM receives transcribed speech from a streaming ASR service. It sends an embedding request to an embedding API, receives the vector back, queries a managed vector database (in this case, hosted in the same cloud region), gets the top-k results, assembles the prompt with retrieved context, and sends it to an LLM for generation. The generated text streams to a TTS service for audio synthesis.


We instrumented every hop. Here's the median latency breakdown for the retrieval portion alone, from the moment the agent has the user's transcribed text to the moment it has retrieved context ready for prompt assembly:


Step Median P95 P99


Embedding API call 22ms 38ms 67ms


Network to vector DB 12ms 24ms 51ms


Vector search (DB processing) 18ms 31ms 44ms


Network return 11ms 22ms 48ms


Deserialization + re-ranking 4ms 7ms 12ms


**Total retrieval** **67ms** **122ms** **222ms**


67ms median looks manageable. But look at the P99: **222ms for a single retrieval call.** And remember, sophisticated agents make two to three retrieval calls per turn. At P99, that's **450–670ms** just in retrieval, before the LLM has seen a single token.


This is the tail latency trap. Your median looks fine. Your P99 is destroying your user experience. And in voice applications,[tail latency is the experience](https://www.rack2cloud.com/deterministic-networking-ai-infrastructure/) , because users don't perceive averages. They perceive the worst moments.


## What Changes When You Go Local


Now, the same pipeline with retrieval co-located in the agent process. The index, a compact, pre-built vector index, is loaded into the agent's memory at startup. When the agent needs to retrieve, it calls a local function. No network. No serialization. No connection pools.


Step Median P95 P99


Embedding (local model) 3ms 5ms 8ms


Vector search (in-process) 1.2ms 2.1ms 3.4ms


Re-ranking 0.8ms 1.4ms 2.1ms


**Total retrieval** **5ms** **8.5ms** **13.5ms**


Read that again. Median retrieval dropped from **67ms to 5ms** . P99 dropped from **222ms to 13.5ms** . That's a **13x improvement at median** and a **16x improvement at P99** .


> **Three retrieval calls per turn: 15ms total instead of 670ms at P99. Over half a second reclaimed.**


That's enough to transform a sluggish voice agent into one that feels instantaneous.


## Where the Time Actually Disappeared


The numbers are dramatic, but the *why* matters more than the *what* . Let's trace where the latency evaporated.


**The network round-trip vanished.** This is the obvious one, but it's worth quantifying. In the cloud baseline, the network contributed 23ms at median and 99ms at P99, just for the two hops to the vector database and back. In the same region. On a fast network. With keep-alives and connection pooling. The local configuration has zero network latency because there is no network. The data is in the same address space as the code that needs it.


**Serialization overhead disappeared.** Every network call requires serializing the query (embedding vector + filter parameters) into a wire format, transmitting it, deserializing on the database side, processing, serializing the results, transmitting back, and deserializing again. With in-process retrieval, the query is a function call with a pointer to the vector. The results are a pointer to the matches. No copying. No encoding. No protocol overhead.


**Connection management evaporated.** Managed vector databases require connection pools, authentication tokens, TLS handshakes, and retry logic. These are well-engineered systems, but they add overhead, especially at P99, where you occasionally hit a cold connection, a pool exhaustion event, or a TLS renegotiation. Local retrieval has none of this. The index is a data structure in memory. You call a function. It returns.


**The embedding step shrank.** In the cloud baseline, embedding required an API call to an external service: 22ms median, 67ms at P99. With a co-located lightweight embedding model (quantized, optimized for the target hardware), the same embedding operation takes 3ms. The model is smaller, yes, but for retrieval queries (short text, not documents), a well-optimized compact model achieves nearly identical recall at a fraction of the latency.


**Tail latency collapsed.** This is the most underappreciated benefit. Cloud services have inherently variable latency: network jitter, garbage collection pauses on the database, load balancer rebalancing, noisy neighbors on shared infrastructure. These factors don't affect median much, but they blow up P99. In-process retrieval on dedicated hardware has near-deterministic latency. The P99/P50 ratio dropped from 3.3x (cloud) to 2.7x (local). A much tighter distribution.


## The Compounding Effect


Here's what happens to the full voice agent pipeline when retrieval drops from 200ms+ to under 15ms:


Component Cloud RAG (P95) Local RAG (P95)


ASR 150ms 150ms


Retrieval (x2 calls) 244ms 17ms


LLM (TTFT) 280ms 280ms


TTS (first audio) 120ms 120ms


**Total to first audio** **794ms** **567ms**


That 227ms difference is the gap between an agent that *barely* meets the[800ms voice interaction benchmark](https://www.pnas.org/doi/10.1073/pnas.0903616106) and one that sails under it with room to spare. Room for an extra retrieval call. Room for a more capable (slower) LLM. Room for re-ranking, safety checks, or citation generation.


> **Fast retrieval doesn't just make retrieval better. It gives you back architectural headroom to invest in everything else.**


## "But Will It Scale?"


The immediate objection is obvious: an in-process index can't hold as much data as a cloud database. This is true, and it's the wrong frame.


The question isn't whether a local index can replace a cloud database for every workload. It's whether the *retrieval that happens on the hot path* , the queries that sit between user input and agent response, needs to go over a network.


Most voice agents and copilots retrieve from knowledge bases that range from tens of thousands to a few million chunks. A well-compressed vector index for 1 million 256-dimensional vectors occupies roughly 500MB. That's well within the memory budget of any modern server, and feasible even for browser-based WASM runtimes with aggressive compression.


The pattern that works: **keep the network database as the system of record, but distribute compact, pre-built indexes to the runtimes that need them.** The index at the edge is a read-optimized projection of the data, not a replacement for the primary store. Updates flow from the source through an indexing pipeline, and fresh indexes are distributed to runtimes on a cadence that matches the data's rate of change.


This is the same pattern the web itself runs on. CDNs don't replace origin servers. They distribute read-optimized copies to the edge so that the hot path (serving a page to a user) doesn't pay the round-trip to the origin. The insight is identical:


> **Move the data closer to where it's consumed, not the consumer closer to the data.**


## The Queries That Don't Need a Network


Not every query should go local. Analytical queries over your full corpus ("find all documents tagged 'compliance' from the last quarter") belong in a server-side database. Complex joins across multiple indexes, aggregations, and batch processing are server workloads.


But the queries that AI agents make in real time follow a predictable pattern:


**Short query vectors.** User utterances, typically 5–30 tokens, embedded into a single vector. The query payload is tiny.


**Small result sets.** Top-5 or top-10 chunks. The agent doesn't need the full corpus ranked. It needs a handful of highly relevant passages to inject into the prompt.


**Repeated over narrow scopes.** A voice agent handling customer support queries for a specific product retrieves from a knowledge base of maybe 10,000–50,000 chunks. A copilot autocompleting code retrieves from a repo of maybe 100,000 chunks. These are small, bounded corpora.


**Latency-critical.** Every millisecond between the user's input and the agent's response is perceived. These queries are the definition of hot-path operations.


This profile is a perfect match for in-process retrieval. Small index, small queries, small results, extreme latency sensitivity. Sending these queries over a network is like routing every function call through a REST API: architecturally possible, but the overhead dominates the actual work.


## What Changes in the Developer Experience


Beyond raw latency, removing the network hop simplifies the entire developer workflow.


**No infrastructure to manage.** No database cluster to provision, scale, monitor, or pay for by the hour. The index is a file. You load it at startup. If the process restarts, it reloads.


**No connection strings.** No configuring regions, authentication, connection pools, retry policies, or timeout values. No debugging why retrieval is slow at 3 AM because the connection pool is exhausted.


**Deterministic testing.** Your CI pipeline can load the same index file and run the same queries with the same results. No flaky tests because the hosted database had a latency spike. No mocking retrieval calls for unit tests. Just call the real thing.


**Offline capability.** If your agent runs on a device or in a browser, local retrieval works without an internet connection. The knowledge base traveled with the runtime. This isn't a niche requirement. It's table stakes for mobile applications, aircraft systems, field service tools, and any deployment where connectivity is intermittent.


## The Shift in Mental Model


The deeper change isn't technical. It's conceptual. For the past several years, the default mental model for retrieval in AI applications has been: *"Retrieval is a network service you call."* This model was inherited from how we've always built database-backed applications. It was never questioned because it's how databases work.


But retrieval in an AI agent isn't the same as a database query in a web application. A web app makes one or two database calls per page load, with a latency budget of 200ms that users won't notice. An AI agent makes multiple retrieval calls per conversational turn, with a latency budget of 50ms or less if it wants to feel responsive.


> **The new mental model: retrieval is a function you call, not a service you query.**


Same semantics. Same results. Fundamentally different performance characteristics. The index is a local data structure, not a remote service. Querying it is a function call, not a network request.


This is the same conceptual shift that happened when[SQLite](https://sqlite.org/whentouse.html) proved that not every application needs a client-server database. Not every retrieval needs a client-server vector store. The workload characteristics of real-time AI retrieval (small, fast, repeated, latency-critical) are precisely the characteristics that favor embedded, co-located data structures over network services.


## What This Means for What You're Building


If you're building a real-time AI application (a voice agent, a copilot, a conversational search product), run this experiment yourself.


**Profile your retrieval path.** Measure the full round-trip, including embedding, network transit, search, and deserialization. Compare median to P99. If P99 is more than 3x your median, network variability is dominating your tail latency.


**Calculate your retrieval budget.** Take your total acceptable turn latency (800ms for voice, maybe 1.5s for text), subtract ASR, LLM, and TTS. Whatever's left is your retrieval budget. If your current retrieval exceeds it, the architecture is the bottleneck, not the implementation.


**Estimate your index size.** Count your chunks, multiply by your embedding dimensions times 4 bytes (for float32) or 1 byte (for int8 quantized). If the result fits in memory (and for most agent workloads, it will), the data can live in-process.


The network hop in your RAG pipeline isn't a fixed cost. It's an architectural choice. And for the class of workloads that defines the next generation of AI applications (real-time, conversational, latency-critical), it's a choice worth reconsidering.


> **The retrieval doesn't need to be faster. It needs to be *closer* .**


---


*Sri Raghu Malireddi is a co-founder of Moss. Previously ML Lead at Grammarly and Microsoft, with publications at ACL and NAACL and multiple patents in real-time ML.[LinkedIn](https://linkedin.com/in/r4ghu) ·[X](https://x.com/srimalireddi)*


*Harsha Nalluru is a co-founder of Moss. Previously Tech Lead at Microsoft, where he architected the core stack of the Azure SDK powering 400+ cloud services.[LinkedIn](https://linkedin.com/in/imharshanalluru) ·[X](https://x.com/ImHarshaNalluru)*
