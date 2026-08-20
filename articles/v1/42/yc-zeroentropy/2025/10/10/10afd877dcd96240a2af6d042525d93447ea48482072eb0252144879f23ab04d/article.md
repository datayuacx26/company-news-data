---
schema_version: "1.0.0"
document_id: "10afd877dcd96240a2af6d042525d93447ea48482072eb0252144879f23ab04d"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/context-engineering-webinar-everything-you-missed/"
published_at: "2025-10-22T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:4c802a766df8918fbb312ef9bd05b7b65c5f74a24ff255fab03cb78499cf0055"
---

# Context Engineering Webinar: Everything You Missed

TL;DR


Thanks to everyone who joined our first *Context Engineering Webinar* ! If you missed it, you can[watch the replay here](https://www.youtube.com/watch?v=XD3dwywldHc) and join the[Context Engineers Discord Server](https://go.zeroentropy.dev/discord) to keep learning and connecting with the community.


# Context Engineering Webinar: Everything You Missed


Thanks to everyone who joined our first *Context Engineering Webinar* !


If you missed it, don’t worry, you can[watch the replay here](https://www.youtube.com/watch?v=XD3dwywldHc) and join the[Context Engineers Discord Server](https://go.zeroentropy.dev/discord) to keep learning and connecting with the community.


## Speakers and Contact Info


For follow-up questions or collaborations, feel free to reach out:


- Ghita Houir Alami (ZeroEntropy) — LinkedIn | Email | Twitter | Schedule a call
- Abhi Ayier (Mastra) — LinkedIn | Email | Twitter
- Prateek Chhikara (Mem0) — LinkedIn | Email | Twitter


## Join the Community


The discussion around *Context Engineering* , *RAG* , and *Agentic Search* is just beginning.


Join the **Context Engineers Discord** to share experiments, ask questions, and connect with others building the next generation of AI systems.


## What is Context Engineering?


We can all agree that[LLMs](https://www.zeroentropy.dev/concepts/large-language-model/) are incredible — but they also have real limitations. There’s a lot of buzz around *growing[context windows](https://www.zeroentropy.dev/concepts/context-window/)* , yet scaling context size doesn’t mean scaling intelligence. In fact, the opposite often happens.


As models see more and more text,[context rot](https://www.zeroentropy.dev/concepts/context-rot/) sets in — the model’s effective understanding gets diluted. This not only reduces quality but also makes responses slower and more expensive.


That’s where **Context Engineering** comes in.


It’s the iterative process of designing, updating, and optimizing *what an LLM sees* at any given moment — even across multiple reasoning steps.


This process combines:


- **[In-context learning](https://www.zeroentropy.dev/concepts/in-context-learning/)** (instructions,[few-shot examples](https://www.zeroentropy.dev/concepts/few-shot-prompting/) ),
- **Retrieval** (from a knowledge base,[memory](https://www.zeroentropy.dev/concepts/agent-memory/) , or an external API),
- And **context flow** — where agents not only *read* from memory but also *write back* summaries or updates for future use.


Search everywhere, but with intelligence.


## The Talk: “RAG vs The Agent Loop — A Fake Dichotomy”


In my talk for ZeroEntropy, I explored a common misconception: that[Retrieval-Augmented Generation (RAG)](https://www.zeroentropy.dev/concepts/rag/) and[Agentic Search](https://www.zeroentropy.dev/concepts/agentic-rag/) are opposing approaches.


They’re not. The truth is, they complement each other.


I broke down why “RAG is dead” is an oversimplification. For most production systems, **[hybrid retrieval](https://www.zeroentropy.dev/concepts/hybrid-search/) plus[reranking](https://www.zeroentropy.dev/concepts/reranker/)** remains the most accurate, cost-efficient, and stable foundation — even for agents.


Our experiments showed that *Hybrid + Rerank* pipelines often find the right answer in one shot — while weaker searches force agents into longer, costlier loops. Still, in deep research or multi-hop reasoning tasks, agentic loops remain valuable when paired with strong retrieval tools.


If you want to explore this further, our full *Context Engineering Cookbook* is available in the ZeroEntropy documentation[here](https://www.zeroentropy.dev/articles/context-engineering-webinar-everything-you-missed/docs.zeroentropy.dev) .


## Context Engineers Community


The **Context Engineers Discord** is the best place to learn more and discuss with fellow builders. Don’t miss our weekly tech talks on Fridays!
