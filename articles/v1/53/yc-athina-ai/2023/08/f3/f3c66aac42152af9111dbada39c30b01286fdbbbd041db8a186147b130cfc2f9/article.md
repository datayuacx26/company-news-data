---
schema_version: "1.0.0"
document_id: "f3c66aac42152af9111dbada39c30b01286fdbbbd041db8a186147b130cfc2f9"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/boosting-logical-reasoning-in-large-language-models-through-a-new-framework-the-graph-of-thought"
published_at: "2023-08-16T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.228240+00:00"
content_hash: "sha256:17f9ca82aee9bb6e9b4a2af41f2de016de6df207c2555df9cf055674791ba13e"
---

# Boosting Logical Reasoning in Large Language Models through a New Framework: The Graph of Thought

Do not index


Original Paper


[https://arxiv.org/abs/2308.08614](https://arxiv.org/abs/2308.08614)


Blog URL


[blog.athina.ai /boostin...-thought](https://blog.athina.ai/boosting-logical-reasoning-in-large-language-models-through-a-new-framework-the-graph-of-thought)


**Original Paper:**[https://arxiv.org/abs/2308.08614](https://arxiv.org/abs/2308.08614)


**By:**[Bin Lei](https://arxiv.org/search/cs?searchtype=author&query=Lei%2C%20B) ,[pei-Hung Lin](https://arxiv.org/search/cs?searchtype=author&query=Lin%2C%20p) ,[Chunhua Liao](https://arxiv.org/search/cs?searchtype=author&query=Liao%2C%20C) ,[Caiwen Ding](https://arxiv.org/search/cs?searchtype=author&query=Ding%2C%20C)


**Abstract:**


> Recent advancements in large-scale models, such as GPT-4, have showcased remarkable capabilities in addressing standard queries. However, when facing complex problems that require multi-step logical reasoning, their accuracy dramatically decreases. Current research has explored the realm of \\textit{prompting engineering} to bolster the inferential capacities of these models. Our paper unveils a pioneering prompting technique, dubbed \\textit{Graph of Thoughts (GoT)}. Through testing on a trio of escalating challenges: the 24-point game, resolution of high-degree polynomial equations, and derivation of formulas for recursive sequences, our method outperformed GPT-4, achieving accuracy improvements of 89.7%, 86%, and 56% for each respective task. Moreover, when juxtaposed with the state-of-the-art (SOTA) prompting method, \\textit{Tree of Thought (ToT)}, our approach registered an average accuracy boost of 23%, 24%, and 15%.


---


###


Summary Notes


##


Enhancing Logical Reasoning in AI with the Graph of Thought Framework


The field of artificial intelligence (AI) continues to advance, with large language models (LLMs) like GPT-4 showing impressive text generation and understanding.


Yet, these models often struggle with complex logical reasoning. While existing methods like Chain-of-Thought (CoT) and Tree-of-Thought (ToT) have made some progress, they don't fully equip LLMs to tackle intricate reasoning tasks accurately.


Enter the Graph of Thought (GoT) framework - a novel approach designed to boost LLMs' logical reasoning to near-human levels.


###


Understanding the Graph of Thought Framework


The GoT framework introduces a more natural way for LLMs to process and solve complex problems by using a graph-based structure, which mirrors human problem-solving methods.


####


Key Components


- **Graph Structure** : Uses nodes to represent problem elements and edges to show logical relationships, enhancing the model's understanding and efficiency.


- **Inspection Mechanism** : Incorporates a rechecking process with a Checker function that surpasses traditional methods, ensuring only the most logical outcomes are chosen.


- **Graph Updating** : Allows for real-time updates, focusing on relevant information and reducing unnecessary data, making reasoning more streamlined.


###


Performance and Testing


GoT's effectiveness was proven through rigorous testing on various logical reasoning tasks:


- Achieved a 97% accuracy in the **24-point game** , closely matching the 98.5% human baseline.


- Excelled in solving **high-order equations** and **arithmetic sequence formulas** , demonstrating superior reasoning capabilities.


###


Advantages Over Traditional Methods


Compared to ToT and CoT, GoT stands out due to:


- Its closer mimicry of human reasoning processes, enabling more natural problem-solving.


- A dynamic graph structure and inspection mechanism that ensure higher accuracy and efficiency.


###


Conclusion


The Graph of Thought framework significantly boosts the logical reasoning abilities of LLMs, making AI more capable of handling complex reasoning tasks.


This advancement bridges the gap between AI and human cognitive abilities and has practical implications in areas requiring sophisticated problem-solving.


As AI evolves, the GoT framework is poised to unlock new potentials and applications, enhancing AI's role in addressing complex challenges.


####


Supporting Information


This blog draws on research by Bin Lei, Pei-Hung Lin, Liao Chunhua, and Caiwen Ding from the University of Connecticut and Lawrence Livermore National Laboratory. Their work provides the empirical data and insights proving GoT's effectiveness.


####


Visual Aids for Better Understanding


For a deeper grasp of the GoT framework, the original paper's visual aids are recommended:


- **Figure 1** : Compares GoT with traditional prompting methods.


- **Table 1** : Shows GoT's performance in the 24-point game.


- **Figures 2-5** : Illustrate GoT's unique problem-solving approach.


In essence, the Graph of Thought framework is a significant step toward achieving AI with human-like reasoning, offering a groundbreaking approach to logical problem-solving in AI models.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
