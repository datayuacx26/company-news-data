---
schema_version: "1.0.0"
document_id: "6036603b2b1a80c4b00f41ff4f2a53026d5ba15e248d1a7a968c312ef878c003"
company_key: "yc-moss"
company: "Moss"
source_id: "yc-moss-rss-e49397d0a304"
canonical_url: "https://www.moss.dev/blog/we-spent-a-decade-making-ai-feel-instant"
published_at: "2026-03-10T00:00:00+00:00"
first_seen_at: "2026-07-27T03:53:19.807128+00:00"
fetched_at: "2026-07-28T20:53:30.609843+00:00"
content_hash: "sha256:aff796bc558a4abb5b4d8c4be18329cf45cfe004ef01662c974e9fea860b6649"
---

# We Spent a Decade Making AI Feel Instant. Here's What We Learned.

Last fall, we were prototyping an AI agent. The RAG pipeline was solid: good embeddings, well-indexed corpus, decent retrieval quality. On paper, it worked. In practice, every user interaction followed the same pattern: the agent needed context, so it called out to a vector database over the network. That call took anywhere from 300ms to a couple of seconds depending on load and location. The agent was making two to three retrieval calls per turn. By the time it had the context it needed, the user had been waiting over a second before anything useful started happening.


In a chatbot, a second is annoying. In a voice agent, it's a dead pause that makes people hang up. In a copilot, it's long enough for the user to context-switch to another tab and lose their train of thought.


We'd both spent our careers watching this exact problem kill otherwise good products. This post is about how we got here and what we decided to do about it.


## Sri: Speed as the Ceiling on Intelligence


Before Moss, I was an ML Lead at Grammarly, working on real-time writing assistance for 40 million daily active users. Before that, I built ML systems for Bing and Office at Microsoft.


At Grammarly, I led personalization for Grammarly Keyboard: making AI suggestions feel right on a mobile device where every millisecond counts. We ran models on-device, ranked suggestions in real-time, and optimized until the keyboard felt like it was reading your mind. That work drove 300% retention growth. The model was already good. What changed was that users could actually feel how good it was, because the suggestions arrived before they lost patience.


A principle came out of that: **in any interactive AI system, perceived intelligence is bounded by perceived speed.** I published research at ACL and NAACL, filed multiple patents in real-time ML. But the most useful thing I learned was watching real users and seeing exactly when latency killed the magic.


We solved this for writing suggestions at Grammarly. But conversational AI (voice agents, copilots, anything with back-and-forth rhythm) hit a wall that model optimization alone couldn't fix.


## Harsha: The Best Infrastructure Disappears


My path was different. I wasn't an ML engineer. I was an infrastructure engineer. At Microsoft, I was a Tech Lead on the Azure SDK team, where I architected the core client stack powering over 400 cloud services and receiving more than 100 million weekly downloads on npm.


My job was making complexity disappear. When a developer writes` new BlobClient()` and it just works, handling retries, auth, telemetry, connection pooling behind the scenes, that was my team. I built the open-source tooling and test automation systems that kept it reliable at scale.


What I took away from that: **the best infrastructure is infrastructure developers forget exists.** If a developer has to think about your infra, you've failed.


When Sri showed me what he was working on (semantic search running inside the same process as the AI agent, no external database, no network hop, no DevOps) I understood immediately. This wasn't a faster database. This was infrastructure that disappears.


## The Problem


We'd been circling the same issue from different angles.


From the ML side: you can build the most sophisticated language model in the world, but if every conversational turn requires three round-trips to a cloud database, the experience falls apart. A 300ms retrieval delay is tolerable in a chatbot. In a voice conversation, it's the difference between natural dialogue and an awkward pause that makes people hang up.


From the infrastructure side: developers building AI agents were cobbling together vector databases, sync pipelines, caching layers, and embedding services, then spending more time on plumbing than on the product. What should be a single operation ( *find the relevant context and return it* ) had become a multi-service architecture problem.


Vector databases like Pinecone, Weaviate, and Qdrant are good at what they do. But they were designed for a world where the query originates from a server, crosses a network, hits a managed cluster, and returns. That works for offline analytics or server-side RAG. It breaks down for voice agents making multiple lookups per turn, copilots that need instant recall, or anything where the user is waiting.


The problem isn't that these databases are slow. The problem is a network hop baked into the architecture. No amount of optimization on the database side eliminates that.


## The Question That Changed Everything


Back to that voice agent prototype. We tried caching, connection pooling, pre-fetching, embedding compression. Shaved off 50ms here, 30ms there. But as long as retrieval meant "send a request over a network and wait," we were fighting physics.


Then a simple question reframed everything: *What if retrieval didn't happen over the network at all? What if the index lived in the same process as the agent?*


Making that work requires an index format compact enough to distribute to browsers and edge devices, a runtime fast enough for vector similarity search in single-digit milliseconds in WebAssembly, a sync layer that keeps distributed indexes fresh without rebuilding them, and all of it packaged as a single` pip install` or` npm install` .


That's what we set out to build.


## Why Rust and WebAssembly


If the search runtime has to live inside the agent process (Node.js server, Python backend, browser tab, mobile app) you need something that compiles to every target, runs at near-native speed, and has a small memory footprint.


C++ has the performance but painful WebAssembly tooling, memory safety liabilities, and rough developer experience. JavaScript gives portability but not performance: vector math in JS is an order of magnitude slower than native.


Rust gave us native speed, memory safety without garbage collection, first-class WebAssembly compilation via` wasm-pack` , and a type system that catches entire categories of bugs at compile time. The Rust core is the single source of truth. From it, we cross-compile to WebAssembly for browsers and edge runtimes, and generate native Python and TypeScript bindings. Developers get an SDK that feels native to their language, but under the hood it's the same Rust engine everywhere.


The result: one codebase that runs identically in a Python process, a Node.js server, a browser tab, a Cloudflare Worker, or a React Native app. Same code, same performance, same API.


## What Moss Does


Moss is a real-time retrieval runtime. We coined that term because nothing existing described what we were making. Not a database (we don't store your data). Not a RAG framework (we don't orchestrate your LLM pipeline). We're the layer that makes retrieval instant and local, wherever your agent runs.


You connect your data source once. Moss indexes it, creates a compact distributable artifact, and pushes it to wherever your agent lives. When your agent needs context, it does a local lookup in under 10 milliseconds. No network hop. No cold start. No external dependency.


` pip install moss` , point it at your data, and retrieval just works.


## What's Next


We launched through Y Combinator's Fall 2025 batch and have been working closely with voice AI platforms like Pipecat and LiveKit, where retrieval latency is felt most acutely.


This post is the first in a series. Coming up:


- **The Retrieval Latency Tax** : where the bottlenecks actually live in AI agent architectures
- **Inside Moss's Architecture** : how we built sub-10ms semantic search in Rust and WebAssembly
- **Benchmarks** : reproducible performance comparisons against cloud vector databases on real agent workloads


If retrieval latency is on your critical path,[try Moss](https://portal.usemoss.dev/) or come talk to us on[Discord](https://moss.link/discord) .


---


*Sri Raghu Malireddi is a co-founder of Moss. Previously ML Lead at Grammarly and Microsoft, with publications at ACL and NAACL and multiple patents in real-time ML.[LinkedIn](https://linkedin.com/in/r4ghu) ·[X](https://x.com/srimalireddi)*


*Harsha Nalluru is a co-founder of Moss. Previously Tech Lead at Microsoft, where he architected the core stack of the Azure SDK powering 400+ cloud services.[LinkedIn](https://linkedin.com/in/imharshanalluru) ·[X](https://x.com/ImHarshaNalluru)*
