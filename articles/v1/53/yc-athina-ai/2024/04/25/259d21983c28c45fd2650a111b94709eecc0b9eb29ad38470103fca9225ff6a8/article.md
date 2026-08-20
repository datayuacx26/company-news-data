---
schema_version: "1.0.0"
document_id: "259d21983c28c45fd2650a111b94709eecc0b9eb29ad38470103fca9225ff6a8"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/demystifying-chains-trees-and-graphs-of-thoughts"
published_at: "2024-04-05T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:02.237076+00:00"
content_hash: "sha256:21db2d23840b50711d0eb8b550f57a1d1b19d6ee2696092ab87c158d744f8eea"
---

# Demystifying Chains, Trees, and Graphs of Thoughts

Do not index


Original Paper


[https://arxiv.org/abs/2401.14295](https://arxiv.org/abs/2401.14295)


Blog URL


[blog.athina.ai /demysti...thoughts](https://blog.athina.ai/demystifying-chains-trees-and-graphs-of-thoughts)


**Original Paper:**[https://arxiv.org/abs/2401.14295](https://arxiv.org/abs/2401.14295)


**By:**[Maciej Besta](https://arxiv.org/search/cs?searchtype=author&query=Besta%2C%20M) ,[Florim Memedi](https://arxiv.org/search/cs?searchtype=author&query=Memedi%2C%20F) ,[Zhenyu Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20Z) ,[Robert Gerstenberger](https://arxiv.org/search/cs?searchtype=author&query=Gerstenberger%2C%20R) ,[Guangyuan Piao](https://arxiv.org/search/cs?searchtype=author&query=Piao%2C%20G) ,[Nils Blach](https://arxiv.org/search/cs?searchtype=author&query=Blach%2C%20N) ,[Piotr Nyczyk](https://arxiv.org/search/cs?searchtype=author&query=Nyczyk%2C%20P) ,[Marcin Copik](https://arxiv.org/search/cs?searchtype=author&query=Copik%2C%20M) ,[Grzegorz Kwaśniewski](https://arxiv.org/search/cs?searchtype=author&query=Kwa%C5%9Bniewski%2C%20G) ,[Jürgen Müller](https://arxiv.org/search/cs?searchtype=author&query=M%C3%BCller%2C%20J) ,[Lukas Gianinazzi](https://arxiv.org/search/cs?searchtype=author&query=Gianinazzi%2C%20L) ,[Ales Kubicek](https://arxiv.org/search/cs?searchtype=author&query=Kubicek%2C%20A) ,[Hubert Niewiadomski](https://arxiv.org/search/cs?searchtype=author&query=Niewiadomski%2C%20H) ,[Aidan O'Mahony](https://arxiv.org/search/cs?searchtype=author&query=O%27Mahony%2C%20A) ,[Onur Mutlu](https://arxiv.org/search/cs?searchtype=author&query=Mutlu%2C%20O) ,[Torsten Hoefler](https://arxiv.org/search/cs?searchtype=author&query=Hoefler%2C%20T)


**Abstract:**


> The field of natural language processing (NLP) has witnessed significant progress in recent years, with a notable focus on improving large language models' (LLM) performance through innovative prompting techniques. Among these, prompt engineering coupled with structures has emerged as a promising paradigm, with designs such as Chain-of-Thought, Tree of Thoughts, or Graph of Thoughts, in which the overall LLM reasoning is guided by a structure such as a graph. As illustrated with numerous examples, this paradigm significantly enhances the LLM's capability to solve numerous tasks, ranging from logical or mathematical reasoning to planning or creative writing. To facilitate the understanding of this growing field and pave the way for future developments, we devise a general blueprint for effective and efficient LLM reasoning schemes. For this, we conduct an in-depth analysis of the prompt execution pipeline, clarifying and clearly defining different concepts. We then build the first taxonomy of structure-enhanced LLM reasoning schemes. We focus on identifying fundamental classes of harnessed structures, and we analyze the representations of these structures, algorithms executed with these structures, and many others. We refer to these structures as reasoning topologies, because their representation becomes to a degree spatial, as they are contained within the LLM context. Our study compares existing prompting schemes using the proposed taxonomy, discussing how certain design choices lead to different patterns in performance and cost. We also outline theoretical underpinnings, relationships between prompting and other parts of the LLM ecosystem such as knowledge bases, and the associated research challenges. Our work will help to advance future prompt engineering techniques.


---


###


Summary Notes


####


A Simplified Guide to Advanced Prompting Schemes for AI Engineers


AI Engineers are pushing the boundaries of natural language processing (NLP) by enabling AI systems to reason and make decisions in a human-like manner.


This guide explores advanced prompting schemes, crucial for arithmetic, commonsense, and symbolic reasoning tasks in AI, and discusses their implications for professionals in the field.


###


Understanding Reasoning in AI


Reasoning is a core function of NLP, allowing AI to process and respond to information in a way that mirrors human thought. The effectiveness of an AI system's reasoning abilities hinges on the choice of prompting scheme.


####


Arithmetic Reasoning


Arithmetic reasoning enables AI to tackle mathematical challenges. Here's how different prompting schemes stack up:


- **Input-Output (IO) Prompting** : Struggles with math tasks.


- **Chain of Thought (CoT)** : Offers substantial improvements in accuracy across various math datasets.


- **Zero-shot-CoT** : Shows promise in certain areas, though not surpassing CoT.


- **Path of Thought (PoT)** : Excels in financial datasets, outperforming CoT.


Different schemes like decomposition and refinement show varying levels of success in math reasoning, highlighting the importance of context and task complexity.


####


Commonsense Reasoning


For AI to apply broad knowledge to new scenarios, the choice of prompting scheme is key:


- **CoT** : Performs better than IO prompting in datasets requiring strategic answers.


- **SelfAsk** : Further enhances performance, especially in complex, multi-hop questions.


Refinement strategies improve AI's contextual understanding, enabling more sophisticated reasoning.


####


Symbolic Reasoning


This involves abstract problem-solving, where:


- **CoT** : Excels in familiar tasks but struggles with unfamiliar ones.


- **CoS** : Offers a noticeable boost in spatial reasoning tasks over CoT.


####


Tree and Graph Schemes


The effectiveness of tree and graph-based schemes varies greatly with the task and dataset:


- Tree schemes are useful for decomposable problems and creative tasks.


- Graph schemes enhance performance in arithmetic and commonsense reasoning but at a computational cost.


####


Refinement Schemes for Specialized Domains


In coding tasks, refinement schemes like SELF-REFINE improve readability and efficiency, showing their value in niche areas.


####


Understanding Topology & Scheduling


The structuring of reasoning steps and the organization of prompts play a critical role in the effectiveness of prompting strategies.


###


Looking Ahead: Future Research


Promising avenues for future research include exploring new topology classes, enhancing single-prompt strategies, integrating with advanced neural networks, and leveraging hardware acceleration for better performance.


###


Conclusion


For AI Engineers in enterprise environments, choosing the appropriate prompting scheme is vital for developing advanced AI systems.


This guide lays the groundwork for further exploration and innovation in AI reasoning, pointing towards the development of more advanced, efficient, and human-like AI solutions.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
