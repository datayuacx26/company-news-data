---
schema_version: "1.0.0"
document_id: "cb0b2e904b83ac999cc4d82b82b2d6361618264932986696ca48ecd7bfe73cab"
company_key: "yc-taskade"
company: "Taskade"
source_id: "yc-taskade-rss-a662ed9a0141"
canonical_url: "https://www.taskade.com/blog/rag-retrieval-history"
published_at: "2026-08-08T00:00:00+00:00"
first_seen_at: "2026-08-08T03:30:33.781191+00:00"
fetched_at: "2026-08-08T03:30:35.455909+00:00"
content_hash: "sha256:6f5eb5bf81e0771b63bc81344b4273b6697b6dc0910f5653d523faeb4fec65db"
---

# The History of RAG: How AI Learned to Look Things Up (2026)

[Blog](https://www.taskade.com/blog)


[AI](https://www.taskade.com/blog/ai)


The History of RAG: How AI…


On this page (18)


Somewhere in room LM-404 of the James Madison Memorial Building in Washington DC, there is a wooden cabinet full of index cards. It is the pre-1978 copyright catalog of the Library of Congress: millions of registrations typed onto 3-by-5 cards, and for most of the twentieth century the working index for answering the question "who owns this?" You did not memorize the catalog. You looked things up in it.


Large language models spent their first few years trying to do the opposite. Everything they knew, they knew from memory, compressed into billions of weights during training. Ask about last week and you got a confident guess. Ask about your own company's documents and you got fiction.


The fix has a name and a birthday. On May 22, 2020, twelve researchers submitted a paper to arXiv called[Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) . This is the story of what came before it, what it started, and where retrieval ended up six years later.


Photo: Library of Congress copyright card catalog, Michael Holley via Wikimedia Commons, public domain.


> **TL;DR:** Retrieval-augmented generation got its name on May 22, 2020, when Patrick Lewis and 11 co-authors posted the RAG paper to arXiv. Six years later retrieval is a decision an agent makes, not a fixed pipeline step.[Build a knowledge-grounded agent free](https://www.taskade.com/create) .


---


## What Is Retrieval-Augmented Generation?


Retrieval-augmented generation is a technique where a model searches an external collection of documents before answering, then writes its answer from the passages it retrieved. The 2020 paper framed this as combining two kinds of memory: **parametric** knowledge baked into the weights during training, and **non-parametric** knowledge sitting in a searchable index that can be swapped, updated or audited without retraining anything.


That split is the entire idea, and it explains every property RAG has. Update the index and the model's answers update. Delete a document and the model stops citing it. Point the index at a private corpus and the model answers questions it was never trained on.


For the plain-English definition, see[retrieval-augmented generation](https://www.taskade.com/wiki/ai/retrieval-augmented-generation) in the wiki, and[parametric knowledge](https://www.taskade.com/wiki/ai/parametric-knowledge) for the half RAG is compensating for.


---


## The Complete RAG Timeline, 1975 to 2026


Retrieval-augmented generation is 6 years old as a term and roughly 50 years old as an engineering problem. Here is the full arc, with the primary source for each entry.


Year What shipped What changed Why it mattered


1975[Vector space model](https://dl.acm.org/doi/pdf/10.1145/361219.361220) , Salton, Wong and Yang Documents become vectors; similarity becomes geometry The mathematical premise every embedding search still runs on


1994[Okapi BM25](https://www.semanticscholar.org/paper/Okapi-at-TREC-3-Robertson-Walker/fa8ffa9f8d8367b987845ae88f2adcac2689f110) at TREC-3, Robertson and Walker Keyword ranking with length and frequency normalisation Still the baseline dense retrieval is measured against in 2026


2013[word2vec](https://arxiv.org/abs/1301.3781) , Mikolov et al., January Words get dense vectors that encode meaning Semantic similarity becomes computable at scale


Feb 2017[FAISS](https://arxiv.org/abs/1702.08734) , Johnson, Douze and Jégou Billion-vector similarity search on GPUs Made large-scale vector search practical, not theoretical


Mar 2017[DrQA](https://arxiv.org/abs/1704.00051) , Chen, Fisch, Weston and Bordes Search Wikipedia, then read the result to find an answer The retrieve-then-read shape RAG would inherit


Aug 2019[Sentence-BERT](https://arxiv.org/abs/1908.10084) , Reimers and Gurevych Finding the closest pair among 10,000 sentences drops from ~65 hours to ~5 seconds Made embedding-based search cheap enough to deploy


Feb 2020[REALM](https://arxiv.org/abs/2002.08909) , Guu et al. A retriever trained *during* pre-training, 4 to 16 points better on open QA Proved retrieval could be learned, not bolted on


Apr 2020[DPR](https://arxiv.org/abs/2004.04906) , Karpukhin et al. Dense dual-encoder beats BM25 by 9 to 19 points on top-20 accuracy The retriever the RAG paper actually used


**May 2020** **[The RAG paper](https://arxiv.org/abs/2005.11401) , Lewis et al.** **Retriever and generator trained end to end; state of the art on 3 open QA tasks** **Gave the pattern its name and its architecture**


Jan 2021[Pinecone](https://www.pinecone.io/blog/announcing-vector-database/) launches, Jan 27, $10M seed Vector search becomes a managed service You no longer needed a research team to run one


Apr 2021[pgvector](https://github.com/pgvector/pgvector/blob/master/CHANGELOG.md) 0.1.0, Apr 20 Vector columns inside ordinary Postgres Removed "do we adopt a new database?" from the decision


Oct 2022 LangChain, Harrison Chase, first commit Oct 24 RAG becomes a short glue script Weekend projects, then production systems


Nov 2022 LlamaIndex, Jerry Liu, repo opened early November Document ingestion and indexing as a library The other half of the assembly problem


Mar 2023[ChatGPT retrieval plugin](https://github.com/openai/chatgpt-retrieval-plugin) , open sourced A frontier lab ships RAG as a product surface Retrieval stops being a research topic


May 2023[Claude 100K context](https://www.anthropic.com/news/100k-context-windows) , May 11 ~75,000 words in one prompt The first serious "do we still need retrieval?" moment


Jul 2023[Lost in the Middle](https://arxiv.org/abs/2307.03172) , Liu et al. Accuracy sags for facts placed mid-prompt Showed a big window is not the same as a usable one


Aug 2023 pgvector 0.5.0 adds HNSW indexing, Aug 28 Approximate search at production latency in Postgres Made the boring option genuinely fast


Oct 2023[Self-RAG](https://arxiv.org/abs/2310.11511) , Asai et al. Reflection tokens let a model critique its own retrieval The first clearly agentic retrieval design


Dec 2023[RAG survey](https://arxiv.org/abs/2312.10997) , Gao et al. Names three paradigms: Naive, Advanced, Modular Gave the field a shared vocabulary


Feb 2024[GraphRAG](https://www.microsoft.com/en-us/research/blog/graphrag-unlocking-llm-discovery-on-narrative-private-data/) , Microsoft Research, Feb 13 Knowledge graph plus community summaries Answers questions no single chunk contains


Mar 2024[RAG 2.0](https://contextual.ai/introducing-rag2/) , Contextual AI, Mar 19 Retriever and generator optimised as one system Argued bolted-together stacks were the ceiling


Sep 2024[Contextual Retrieval](https://www.anthropic.com/news/contextual-retrieval) , Anthropic, Sep 19 Retrieval failure rate down 49%, or 67% with reranking Proved better chunking beat bigger models


Nov 2024[Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) , Nov 25 An open standard for connecting models to data sources Retrieval becomes a connector, not a pipeline


Feb 2025[LaRA benchmark](https://arxiv.org/abs/2502.09977) , Feb 14, later at ICML 2025 2,326 cases: no silver bullet between RAG and long context Ended the binary framing with evidence


Apr 2025[Llama 4 Scout](https://ai.meta.com/blog/llama-4-multimodal-intelligence/) , Apr 5 A 10M-token context window in an open-weight model Made "just paste everything" technically possible


2025-26[Agentic RAG survey](https://arxiv.org/abs/2501.09136) , Jan 15 2025, and the shift to context engineering Retrieval becomes an agent decision inside a reasoning loop Where the field actually is today


---


## Before RAG Had a Name: Fifty Years of Looking Things Up


Search predates RAG by about 45 years, and every layer of a modern retrieval stack has an ancestor in that period. The 1975 vector space model by Gerard Salton, A. Wong and C. S. Yang turned documents into coordinates so that "related" became a distance you could compute. BM25, introduced by Stephen Robertson, Steve Walker and colleagues at TREC-3 in 1994, ranked documents by term frequency with sane normalisation for document length. It is still a strong baseline in 2026, which is a remarkable run for a 32-year-old scoring function.


What changed in the 2010s was *what* the vectors meant. Salton's vectors counted words. After[word2vec](https://arxiv.org/abs/1301.3781) in 2013 and BERT in 2018, vectors encoded meaning, so "how do I cancel my plan" could match a document titled "subscription management" with no shared keywords.[Sentence-BERT](https://arxiv.org/abs/1908.10084) in August 2019 made that practical, cutting the work of finding the closest pair among 10,000 sentences from roughly 65 hours to about 5 seconds.


The plumbing arrived at the same time.[FAISS](https://arxiv.org/abs/1702.08734) from Facebook AI Research, posted to arXiv on February 28, 2017, reported building a nearest-neighbour graph over a billion vectors in under 12 hours on four GPUs.[HNSW](https://arxiv.org/abs/1603.09320) , posted by Yury Malkov and Dmitry Yashunin on March 30, 2016, gave approximate nearest-neighbour search a graph structure with logarithmic scaling, and is the index type most vector stores still default to.


Read the mechanics in[vector embeddings](https://www.taskade.com/wiki/ai/vector-embedding) ,[embeddings](https://www.taskade.com/wiki/ai/embeddings) , and[semantic search](https://www.taskade.com/wiki/ai/semantic-search) .


---


## 2020: The Year Retrieval Got a Name


Four papers in a little over three months turned retrieval from a preprocessing step into an architecture. Three of them came out before the RAG paper and made it possible.


Paper Date Contribution Why it fed RAG


[REALM](https://arxiv.org/abs/2002.08909) Feb 10, 2020 Latent knowledge retriever trained unsupervised during pre-training Showed the retriever could be *learned* , gaining 4 to 16 points on open QA


[DPR](https://arxiv.org/abs/2004.04906) Apr 10, 2020 Dual-encoder dense retrieval beating BM25 by 9 to 19 points at top-20 Became the retriever inside RAG


[ColBERT](https://arxiv.org/abs/2004.12832) Apr 27, 2020 Late interaction: encode separately, match fine-grained at query time Two orders of magnitude faster than BERT-based rankers


[RAG](https://arxiv.org/abs/2005.11401) May 22, 2020 Generator plus dense Wikipedia index, trained end to end Named the pattern and set state of the art on 3 open QA tasks


The RAG paper's authors were Patrick Lewis, Ethan Perez, Aleksandra Piktus, Fabio Petroni, Vladimir Karpukhin, Naman Goyal, Heinrich Küttler, Mike Lewis, Wen-tau Yih, Tim Rocktäschel, Sebastian Riedel and Douwe Kiela, across Facebook AI Research, University College London and New York University. It was accepted at NeurIPS 2020.


Two details are worth keeping. First, the paper's contribution was not "search then prompt," which DrQA had done in 2017. It was training the retriever and the generator *together* , so the retriever learned what the generator actually found useful. Second, the paper's own framing of parametric versus non-parametric memory is still the cleanest way to explain the technique to a non-specialist, and it anticipated the[knowledge cutoff](https://www.taskade.com/wiki/ai/knowledge-cutoff) problem years before anyone was complaining about it in public.


---


## The Vector Database Boom, 2021 to 2023


Between January 2021 and mid-2023, retrieval infrastructure went from research code to a product category.[Pinecone](https://www.pinecone.io/blog/announcing-vector-database/) came out of stealth with a managed vector database and a $10M seed round led by Wing Venture Capital on January 27, 2021. Milvus had been open sourced by Zilliz in October 2019, with Weaviate and Qdrant arriving as open-source alternatives over the following couple of years. Postgres got[pgvector](https://github.com/pgvector/pgvector/blob/master/CHANGELOG.md) 0.1.0 on April 20, 2021, and HNSW indexing in 0.5.0 on August 28, 2023, which is the moment "add a vector column to the database you already run" became a serious answer.


Then ChatGPT launched, every company wanted a chatbot over its own documents, and two libraries turned the assembly problem into a weekend. Harrison Chase pushed the initial commit of LangChain on October 24, 2022. Jerry Liu opened the repository that became LlamaIndex in early November 2022. On March 23, 2023 OpenAI shipped plugins for ChatGPT and open sourced the[retrieval plugin](https://github.com/openai/chatgpt-retrieval-plugin) , and retrieval stopped being a research topic entirely.


The stack that congealed in that period is still the thing most people mean when they say "RAG":


```text
INGEST  (offline, once per document)
Source docs  ──►  Chunk      ──►  Embed        ──►  Store
PDFs, tickets     500-1,000       embedding         vector index
wiki, email       tokens each     model             + metadata filters QUERY   (online, every single question)    Question ─► Embed ─► Search ─► Rerank ─► Assemble ─► Generate                same      top-k     relevance  prompt +     model writes                model     nearest   model      k passages   the answer
The whole system's accuracy is capped by the SEARCH step.    A perfect model cannot answer from a passage it never received.
```


That last line is the one most teams learn the expensive way. More on the failure modes in[vector databases explained](https://www.taskade.com/blog/vector-databases) and[what is retrieval-augmented generation](https://www.taskade.com/blog/what-is-retrieval-augmented-generation) .


---


## Why Naive RAG Started Breaking


The standard 2023 pipeline failed in predictable ways, and almost all of them happen before the model generates a single token. Anthropic's[Contextual Retrieval](https://www.anthropic.com/news/contextual-retrieval) work, published September 19, 2024, is the cleanest public evidence: simply prepending explanatory context to each chunk before embedding cut top-20 retrieval failure rate by 35%, adding contextual BM25 took it to 49%, and adding a reranker reached 67%. No model change. Just better retrieval.


The placement failure has its own landmark paper.[Lost in the Middle](https://arxiv.org/abs/2307.03172) , submitted July 6, 2023 by Nelson Liu, Kevin Lin, John Hewitt, Ashwin Paranjape, Michele Bevilacqua, Fabio Petroni and Percy Liang, found that performance "is often highest when relevant information occurs at the beginning or end of the input context, and significantly degrades when models must access relevant information in the middle of long contexts." A large[context window](https://www.taskade.com/wiki/ai/context-window) is not the same as a uniformly usable one, which is exactly the phenomenon now called[context rot](https://www.taskade.com/wiki/ai/context-rot) .


Symptom Root cause The fix that works


Correct-sounding answer, wrong facts Wrong passages retrieved Rerank; add contextual chunk headers


Model says "not in the documents" but it is Chunk boundary split the idea Overlapping chunks; larger windows; parent-document retrieval


Works on demos, fails on real questions Embeddings match topic, not intent Hybrid search: dense vectors plus BM25


Good retrieval, still wrong answer Evidence buried mid-prompt Reorder so top-ranked passages sit first and last


Cannot answer "what are the themes here" No single chunk holds the answer GraphRAG or hierarchical summarisation


---


## Agentic RAG: Retrieval Becomes a Decision


By late 2023, the interesting systems stopped retrieving on every query and started deciding.[Self-RAG](https://arxiv.org/abs/2310.11511) , submitted October 17, 2023 by Akari Asai, Zeqiu Wu, Yizhong Wang, Avirup Sil and Hannaneh Hajishirzi, trained a model to emit special reflection tokens that let it judge whether retrieval was needed at all and whether the retrieved passages actually helped. That is the hinge: retrieval moved from a fixed pipeline stage into the model's reasoning loop.


The[December 2023 RAG survey](https://arxiv.org/abs/2312.10997) by Yunfan Gao and co-authors named the progression Naive, Advanced and Modular RAG, and the[January 2025 agentic RAG survey](https://arxiv.org/abs/2501.09136) by Aditi Singh and co-authors documented what came next: agents embedded in the retrieval pipeline, using reflection, planning, tool use and multi-agent collaboration to pick a strategy instead of following a fixed workflow. If you want the pattern rather than the papers, see[agentic RAG](https://www.taskade.com/wiki/ai/agentic-rag) and[agentic design patterns](https://www.taskade.com/blog/agentic-design-patterns) .


---


## GraphRAG: When the Answer Is in No Single Chunk


Microsoft Research announced[GraphRAG](https://www.microsoft.com/en-us/research/blog/graphrag-unlocking-llm-discovery-on-narrative-private-data/) on February 13, 2024, authored by Jonathan Larson and Steven Truitt, with the full method published as[From Local to Global](https://arxiv.org/abs/2404.16130) on April 24, 2024, led by Darren Edge. The problem it solves is structural. Ask a vector index "what are the recurring themes in these 3,000 support tickets" and it returns the five tickets most similar to your question, which is useless, because the answer is a property of the whole collection and lives in no individual document.


GraphRAG has a language model build an entity-and-relationship graph over the corpus, cluster that graph into communities, and pre-summarise each community. A broad question is then answered by combining community summaries rather than by finding chunks. On corpora around one million tokens the paper reports substantial gains over conventional RAG on comprehensiveness and diversity for global sensemaking questions. Microsoft's own blog is careful here: the comparison used a language model as judge on qualitative criteria, not a numeric accuracy benchmark.


See[GraphRAG](https://www.taskade.com/wiki/ai/graphrag) for the definition and[knowledge graph agents](https://www.taskade.com/blog/knowledge-graph-agents) for how graph-shaped memory behaves in practice.


---


## RAG vs Long Context vs Fine-Tuning: The Decision Table


The most useful thing in this article is this table, because the three techniques get compared as if they were rivals when they solve three different problems. Retrieval changes what the model **knows** . Long context changes how much it can **see at once** . Fine-tuning changes how it **behaves** .


Dimension RAG (retrieve it) Long context (paste it all) Fine-tuning (bake it in)


**What it actually changes** The facts available at answer time How much the model sees in one pass Style, format, behaviour, tone


**Freshness** Immediate: update the index, done Immediate, but you re-send everything each call Stale until you retrain


**Corpus ceiling** Effectively unbounded Bounded by the window: up to 10M tokens in Llama 4 Scout Bounded by training budget


**Cost per query** Low: you send a handful of passages High: you pay for every token, every call Low per query, high one-time


**Traceability** Strong: cite the exact retrieved source Weak: the answer could come from anywhere in the blob None: the source dissolves into weights


**Permissions** Natural: filter the index by who is asking Awkward: you must pre-filter before pasting Impossible: weights cannot be scoped per user


**Best for** Private docs, policies, tickets, anything that changes One long document, one meeting, one codebase in one sitting House format, classification, consistent output shape


**Breaks when** Retrieval misses the passage The evidence lands mid-prompt and gets skimmed The underlying facts change


The binary framing was settled with evidence rather than opinion.[LaRA](https://arxiv.org/abs/2502.09977) , posted in February 2025 and presented at ICML 2025, tested 2,326 cases across four QA task categories and three types of naturally occurring long text, and concluded there is no silver bullet: the optimal choice depends on model parameter size, long-text capability, context length, task type and the characteristics of the retrieved chunks. Contextual AI's[RAG 2.0](https://contextual.ai/introducing-rag2/) argument from March 19, 2024 lands in the same place from the other direction, noting that systems optimised end to end use substantially less compute than long-context approaches at higher accuracy.


A 30-second version:


If this is true Reach for


The knowledge changes weekly or faster RAG


Different users may see different documents RAG


You need to show *where* an answer came from RAG


It is one document and it fits comfortably in the window Long context


You are exploring, not shipping Long context


The model gets the facts right but the format wrong Fine-tuning


You need a specific tone across thousands of outputs Fine-tuning


Two of the above are true Combine them, this is the normal case


Longer treatment in[fine-tuning vs RAG vs prompting](https://www.taskade.com/blog/fine-tuning-vs-rag) and[fine-tuning](https://www.taskade.com/wiki/ai/fine-tuning) .


---


## What RAG Looks Like in 2026


Retrieval in 2026 is a capability an agent invokes, not a pipeline a system runs. Three things drove that.


First, standardisation. The[Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) , announced November 25, 2024 as an open standard for two-way connections between AI tools and data sources, turned "wire up a retriever" into "connect a server." Retrieval became one connector among many rather than bespoke plumbing per application.


Second, the framing shifted from prompts to[context engineering](https://www.taskade.com/wiki/ai/context-engineering) : deciding what belongs in a limited window, in what order, at what fidelity, and what gets dropped. Retrieval is one lever in that budget, alongside[context compaction](https://www.taskade.com/wiki/ai-agents/context-compaction) ,[prompt caching](https://www.taskade.com/wiki/ai/prompt-caching) and summarisation.


Third, the failure vocabulary matured. Teams now debug with specific terms rather than "the AI is wrong," which is why[hallucinations](https://www.taskade.com/wiki/ai/hallucinations) , context rot and[stateless versus stateful](https://www.taskade.com/wiki/ai/stateless-vs-stateful) behaviour are worth knowing by name.


---


## How Taskade Puts Retrieval to Work


In Taskade, the knowledge an agent retrieves from is the workspace you already have. You attach projects, uploaded documents and files to an[AI agent](https://www.taskade.com/agents) , and that material becomes the ground it answers from, no vector database to provision and no ingestion pipeline to maintain. Step-by-step setup lives in[agent knowledge](https://www.taskade.com/learn/agents/knowledge) and[build a knowledge chatbot](https://www.taskade.com/learn/agents/build-knowledge-chatbot) .


That is the Memory half of **Workspace DNA** . Memory lives in your projects across 7 views: List, Board, Calendar, Table, Mind Map, Gantt and Org Chart. Intelligence reads that memory: agents run on 15+ frontier models from OpenAI, Anthropic, Google and open-weight providers, with web search, persistent memory, custom slash commands and multi-agent collaboration. Execution acts on it:[automations](https://www.taskade.com/automate) fire on triggers and push results back through 100+ bidirectional[integrations](https://www.taskade.com/integrations) , and every run writes new material back into memory. The loop closes.


For agents that build rather than answer,[Taskade Genesis](https://www.taskade.com/create) grounds generated apps in the same workspace context, described in[app context](https://www.taskade.com/learn/genesis/app-context) and[the memory graph](https://www.taskade.com/learn/genesis/memory-graph) . Browse working examples in the[Community Gallery](https://www.taskade.com/community) , or read the architecture in[Workspace DNA](https://www.taskade.com/blog/workspace-dna-architecture) and[the AI agent stack](https://www.taskade.com/blog/ai-agent-stack) .


Paid plans start at **Pro $10 per month billed annually** , which includes agents with knowledge attachment, automations and integrations.


---


## Frequently Asked Questions


What is retrieval-augmented generation?


A technique where a language model searches an external body of documents before it answers, then writes its answer using the passages it found. The model's weights supply language ability. The retrieved documents supply facts. This lets a model cite sources, use information newer than its training data, and answer questions about private documents it was never trained on.


Who invented RAG and when?


The term was introduced in[Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) , submitted to arXiv on May 22, 2020 and published at NeurIPS 2020. Twelve authors, led by Patrick Lewis, across Facebook AI Research, University College London and New York University.


What did the original 2020 RAG paper actually do?


It paired a pre-trained sequence-to-sequence generator with a dense vector index of Wikipedia and trained the two together end to end. The paper framed this as combining parametric memory, meaning knowledge in weights, with non-parametric memory, meaning knowledge in a searchable index. It set the state of the art on three open-domain QA tasks.


What came before RAG?


Decades of information retrieval: the 1975 vector space model, BM25 at TREC-3 in 1994, word2vec in 2013, BERT in 2018 and Sentence-BERT in 2019. The immediate precursors were DrQA in March 2017, REALM in February 2020 and Dense Passage Retrieval in April 2020.


Why did vector databases explode in 2023?


ChatGPT made it obvious that every organisation wanted a chatbot over its own documents. Pinecone had launched a managed vector database in January 2021, Milvus went open source in October 2019, and pgvector added HNSW indexing in August 2023. LangChain and LlamaIndex, both from late 2022, turned assembling the pieces into a weekend project.


What is agentic RAG?


Retrieval treated as a decision the model makes rather than a fixed pipeline step. The agent decides whether to search, chooses the query, judges the results, and searches again if they are inadequate.[Self-RAG](https://arxiv.org/abs/2310.11511) in October 2023 introduced reflection tokens for exactly this. See[agentic RAG](https://www.taskade.com/wiki/ai/agentic-rag) .


What is GraphRAG?


A Microsoft Research method announced February 13, 2024 that builds an entity-and-relationship graph over a corpus, clusters it into communities, and pre-summarises each one. It targets questions where the answer is a property of the whole collection rather than of any single document.


Does long context make RAG obsolete?


No. The[LaRA benchmark](https://arxiv.org/abs/2502.09977) at ICML 2025, with 2,326 test cases, concluded there is no silver bullet: the right choice depends on model, context length, task and retrieval quality. Long context removed the need for retrieval on small self-contained corpora. Large, fresh, private or permission-scoped corpora still favour retrieval on cost, latency and traceability.


Should I use RAG or fine-tuning?


Use RAG when the model needs facts it lacks. Use fine-tuning when the model needs a behaviour it lacks. Retrieval changes what the model knows. Fine-tuning changes how it writes. They are frequently combined. See[fine-tuning vs RAG vs prompting](https://www.taskade.com/blog/fine-tuning-vs-rag) .


Why do RAG systems still hallucinate?


Usually because retrieval failed before generation began: chunks split at the wrong boundary, embeddings matching topic instead of intent, or relevant text buried mid-prompt.[Lost in the Middle](https://arxiv.org/abs/2307.03172) showed accuracy peaks when evidence sits at the beginning or end of the input and degrades significantly in the middle.


How do I build a knowledge-grounded AI system without writing code?


Attach your projects, documents and files to a Taskade[AI agent](https://www.taskade.com/agents) as its knowledge base. The agent grounds answers in that material and can trigger[automations](https://www.taskade.com/automate) across 100+ integrations. Walkthrough in[agent knowledge](https://www.taskade.com/learn/agents/knowledge) .


---


## From Card Catalogs to Agents That Decide


The Library of Congress card catalog worked because someone decided what each card said, where it filed, and how you would find it again. That is still the job. The 2020 RAG paper did not invent looking things up. It gave a model a way to do it and, more importantly, a way to be *trained* on doing it well.


Six years on, the interesting question is no longer whether a model can search. It is whether it knows when to. The systems that work in 2026 treat retrieval as a judgement inside a reasoning loop, grounded in memory that keeps accumulating: Memory feeds Intelligence, Intelligence triggers Execution, Execution writes new Memory.


That loop is what[Taskade Genesis](https://www.taskade.com/create) runs on. Your workspace is the index. Your[agents](https://www.taskade.com/agents) are the readers. Your[automations](https://www.taskade.com/automate) are the execution.


**▲ ■ ●**


[Try Taskade free →](https://www.taskade.com/create)


---


## Further Reading


### RAG and Retrieval


- [What Is Retrieval-Augmented Generation?](https://www.taskade.com/blog/what-is-retrieval-augmented-generation) : the plain-English explainer
- [Fine-Tuning vs RAG vs Prompting](https://www.taskade.com/blog/fine-tuning-vs-rag) : how to choose, with cost math
- [Vector Databases and Embeddings Explained](https://www.taskade.com/blog/vector-databases) : what actually happens inside the index
- [Knowledge Graph Agents](https://www.taskade.com/blog/knowledge-graph-agents) : graph-shaped memory in practice
- [Retrieval-Augmented Generation](https://www.taskade.com/wiki/ai/retrieval-augmented-generation) ·[Agentic RAG](https://www.taskade.com/wiki/ai/agentic-rag) ·[GraphRAG](https://www.taskade.com/wiki/ai/graphrag) ·[Vector Database](https://www.taskade.com/wiki/ai/vector-database) ·[Corpus](https://www.taskade.com/wiki/ai/corpus)


### Context and Memory


- [Context Engineering Field Guide](https://www.taskade.com/blog/context-engineering) : the discipline retrieval now lives inside
- [Types of Memory in AI Agents](https://www.taskade.com/blog/ai-agent-memory) : what survives between sessions
- [How Do Large Language Models Work?](https://www.taskade.com/blog/how-do-llms-work) : transformers, attention, generation
- [Context Window](https://www.taskade.com/wiki/ai/context-window) ·[Parametric Knowledge](https://www.taskade.com/wiki/ai/parametric-knowledge) ·[Hallucinations](https://www.taskade.com/wiki/ai/hallucinations) ·[Agent Harness](https://www.taskade.com/wiki/ai-agents/agent-harness)


### More Histories


- [The History of Workflow Automation](https://www.taskade.com/blog/automation-history) : from punch cards to agents
- [The History of Anthropic and Claude](https://www.taskade.com/blog/anthropic-claude-history)
- [The History of Mermaid.js](https://www.taskade.com/blog/history-of-mermaid) : diagrams as code
- [What Is Agentic Engineering?](https://www.taskade.com/blog/what-is-agentic-engineering)


### Explore Taskade


- [Taskade Genesis](https://www.taskade.com/create) : one prompt, one living app
- [AI Agents](https://www.taskade.com/agents) : knowledge, memory, tools, multi-agent collaboration
- [Automations](https://www.taskade.com/automate) : triggers and actions across 100+ integrations
- [Community Gallery](https://www.taskade.com/community) : working apps you can clone
