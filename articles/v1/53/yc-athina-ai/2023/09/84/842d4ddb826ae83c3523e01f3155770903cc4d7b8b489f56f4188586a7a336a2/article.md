---
schema_version: "1.0.0"
document_id: "842d4ddb826ae83c3523e01f3155770903cc4d7b8b489f56f4188586a7a336a2"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/promptbreeder-self-referential-self-improvement-via-prompt-evolution"
published_at: "2023-09-28T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:ac1cc6c697094ba366f6e6d1916264675e757ad82c99c4b2bf6bc5c567f1fe61"
---

# Promptbreeder: Self-Referential Self-Improvement Via Prompt Evolution

Do not index


Original Paper


[https://arxiv.org/abs/2309.16797](https://arxiv.org/abs/2309.16797)


Blog URL


[blog.athina.ai /promptb...volution](https://blog.athina.ai/promptbreeder-self-referential-self-improvement-via-prompt-evolution)


**Original Paper:**[https://arxiv.org/abs/2309.16797](https://arxiv.org/abs/2309.16797)


**By:**[Chrisantha Fernando](https://arxiv.org/search/cs?searchtype=author&query=Fernando%2C%20C) ,[Dylan Banarse](https://arxiv.org/search/cs?searchtype=author&query=Banarse%2C%20D) ,[Henryk Michalewski](https://arxiv.org/search/cs?searchtype=author&query=Michalewski%2C%20H) ,[Simon Osindero](https://arxiv.org/search/cs?searchtype=author&query=Osindero%2C%20S) ,[Tim Rocktäschel](https://arxiv.org/search/cs?searchtype=author&query=Rockt%C3%A4schel%2C%20T)


**Abstract:**


> Popular prompt strategies like Chain-of-Thought Prompting can dramatically improve the reasoning abilities of Large Language Models (LLMs) in various domains. However, such hand-crafted prompt-strategies are often sub-optimal. In this paper, we present Promptbreeder, a general-purpose self-referential self-improvement mechanism that evolves and adapts prompts for a given domain. Driven by an LLM, Promptbreeder mutates a population of task-prompts, and subsequently evaluates them for fitness on a training set. Crucially, the mutation of these task-prompts is governed by mutation-prompts that the LLM generates and improves throughout evolution in a self-referential way. That is, Promptbreeder is not just improving task-prompts, but it is also improving the mutationprompts that improve these task-prompts. Promptbreeder outperforms state-of-the-art prompt strategies such as Chain-of-Thought and Plan-and-Solve Prompting on commonly used arithmetic and commonsense reasoning benchmarks. Furthermore, Promptbreeder is able to evolve intricate task-prompts for the challenging problem of hate speech classification.


---


###


Summary Notes


####


Revolutionizing AI with Self-Improving Prompts: An Introduction to PROMPTBREEDER


In the fast-paced world of artificial intelligence (AI), Large Language Models (LLMs) are leading the charge in innovation across various sectors.


The effectiveness of these models largely depends on the prompts they are given, which has traditionally been a manual and tedious process.


PROMPTBREEDER is here to change that, introducing an automated, evolutionary approach to prompt engineering, promising to significantly boost the efficiency and adaptability of AI systems.


####


**The Challenge with Traditional Prompting**


Effective prompting is crucial for the performance of LLMs, guiding them to understand and execute tasks accurately. However, manually crafting these prompts can be:


- Time-consuming


- Difficult to scale


- Less effective as task complexity increases


####


**What PROMPTBREEDER Offers**


PROMPTBREEDER addresses these challenges by using an evolutionary algorithm to automate prompt evolution, enhancing LLM reasoning capabilities. It uniquely evolves two types of prompts:


- **Task-specific prompts** that direct the AI's task performance


- **Mutation-prompts** that guide the evolution of these task prompts over time


####


**How PROMPTBREEDER Operates**


- **Initialization** : Starts with initial sets of task and mutation-prompts.


- **Generation of Variations** : Creates variations of these prompts using an LLM.


- **Evaluation and Evolution** : Variations are evaluated for effectiveness, with the best-performing prompts evolved through a genetic algorithm.


- **Iterative Improvement** : This process refines both task and mutation-prompts across generations, enhancing system performance progressively.


####


**The Self-Referential Mechanism**


At its core, PROMPTBREEDER's innovation lies in its ability to self-improve by evolving not just the task prompts but also the mutation-prompts.


This recursive self-improvement is inspired by self-referential systems in neural networks and represents a significant advancement in prompt engineering automation.


####


**Achievements of PROMPTBREEDER**


PROMPTBREEDER has outshone traditional prompting methods in various benchmarks, demonstrating an exceptional capacity for iterative and specific task adaptation.


This underscores its potential to significantly elevate AI efficiency and adaptability.


####


**The Future with PROMPTBREEDER**


PROMPTBREEDER not only advances our understanding of automated prompt engineering but also opens the door to the development of self-improving AI systems.


It reduces the reliance on manual prompting, paving the way for more sophisticated AI models.


Future work will focus on scaling this approach to a wider range of tasks and models, heralding exciting possibilities in AI advancement.


####


**Conclusion**


PROMPTBREEDER marks a pivotal step forward in creating more capable and adaptable AI systems by merging evolutionary algorithms with self-referential mechanisms for prompt engineering.


For AI engineers and enterprises, this methodology signifies a path toward unprecedented levels of AI application efficiency and innovation. As we venture further into this new era, PROMPTBREEDER's role in the evolution of AI is becoming increasingly prominent, promising a future where AI can adapt and evolve with minimal human input, driven by the bounds of imagination.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
