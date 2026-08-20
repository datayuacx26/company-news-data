---
schema_version: "1.0.0"
document_id: "eb7c6d0c008b670bc2113bc85192783d9d37e96d7ac43af17e5d4e4cdd5eeec7"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/chain-of-symbol-prompting-elicits-planning-in-large-langauge-models"
published_at: "2023-05-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:38e209446e180f17546bf8301a024d2462da1b4e509b99984e0f17db138def3a"
---

# Chain-of-Symbol Prompting Elicits Planning in Large Langauge Models

Do not index


Original Paper


[https://arxiv.org/abs/2305.10276](https://arxiv.org/abs/2305.10276)


Blog URL


[blog.athina.ai /chain-o...e-models](https://blog.athina.ai/chain-of-symbol-prompting-elicits-planning-in-large-langauge-models)


**Original Paper:**[https://arxiv.org/abs/2305.10276](https://arxiv.org/abs/2305.10276)


**By:**[Hanxu Hu](https://arxiv.org/search/cs?searchtype=author&query=Hu%2C%20H) ,[Hongyuan Lu](https://arxiv.org/search/cs?searchtype=author&query=Lu%2C%20H) ,[Huajian Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20H) ,[Yun-Ze Song](https://arxiv.org/search/cs?searchtype=author&query=Song%2C%20Y) ,[Wai Lam](https://arxiv.org/search/cs?searchtype=author&query=Lam%2C%20W) ,[Yue Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20Y)


**Abstract:**


> In this paper, we take the initiative to investigate the performance of LLMs on complex planning tasks that require LLMs to understand a virtual spatial environment simulated via natural language and act correspondingly in text. We propose a benchmark named Natural Language Planning and Action (Natala) composed of a set of novel tasks: Brick World, NLVR-based Manipulations, and Natural Language Navigation. We found that current popular LLMs such as ChatGPT still lack abilities in complex planning. This arises a question -- do the LLMs have a good understanding of the environments described in natural language, or maybe other alternatives such as symbolic representations are neater and hence better to be understood by LLMs? To this end, we propose a novel method called CoS (Chain-of-Symbol Prompting) that represents the complex environments with condensed symbolic spatial representations during the chained intermediate thinking steps. CoS is easy to use and does not need additional training on LLMs. Extensive experiments indicate that CoS clearly surpasses the performance of the Chain-of-Thought (CoT) Prompting in all three planning tasks with even fewer tokens used in the inputs compared with CoT on ChatGPT and InstructGPT. The performance gain is strong, by up to 60.8% accuracy (from 31.8% to 92.6%) on Brick World for ChatGPT. CoS also reduces the number of tokens in the prompt obviously, by up to 65.8% of the tokens (from 407 to 139) for the intermediate steps from demonstrations on Brick World. Code and data available at:
>
>
> [this https URL](https://github.com/hanxuhu/chain-of-symbol-planning)


---


###


Summary Notes


##


Boosting AI's Spatial Understanding with Chain-of-Symbol Prompting


In the field of artificial intelligence (AI), technologies like Large Language Models (LLMs) such as ChatGPT have significantly advanced our ability to generate human-like text.


However, these models often struggle with spatial reasoning and planning—key for understanding and interacting with the physical world. This is particularly evident in tasks that require knowledge of space and object manipulation.


Chain-of-Symbol Prompting (CoS) is an innovative approach aimed at improving this aspect. By converting detailed natural language into simple, symbolic forms, CoS enhances LLMs' ability to reason about space. This blog examines CoS's mechanics, its implications for AI, and its potential impact on spatially dependent industries.


###


Understanding Spatial Tasks


Spatial reasoning covers tasks like navigating environments and manipulating objects following certain rules. We focus on three tasks to test LLMs:


- **Brick World:** Involves manipulating bricks based on specific instructions.


- **NLVR-based Manipulation:** Uses the Natural Language Visual Reasoning dataset to rearrange objects in boxes as per given rules.


- **Natural Language Navigation:** Entails finding the shortest route in a virtual space using landmarks.


These tasks highlight the challenges of spatial reasoning and the limitations of LLMs when relying on natural language alone.


###


The Chain-of-Symbol Method


Chain-of-Symbol Prompting is a breakthrough in overcoming these challenges. Its key features include:


- **Enhanced LLM Performance:** CoS uses symbolic representations to boost accuracy and reduce computational needs, leading to cost savings.


- **Versatility:** Shows strong performance across different tasks and languages, highlighting its adaptability.


- **Efficient Representation:** CoS's symbols effectively capture spatial relationships, making it easier for LLMs to process and understand.


###


Testing and Findings


Experiments with models like ChatGPT across various spatial tasks compared different prompting methods: zero-shot Chain-of-Thought (CoT), few-shot CoT, and few-shot CoS. Results showed:


- **Brick World:** CoS performed better than CoT, particularly with complex instructions.


- **NLVR-based Manipulation & Natural Language Navigation:** CoS was more accurate and efficient than CoT.


- **Spatial QA:** On the SPARTUN dataset, CoS surpassed traditional methods, proving its effectiveness in realistic scenarios.


###


The Takeaway


Chain-of-Symbol Prompting stands out as a potent method to enhance LLMs' spatial reasoning, offering a more cost-effective prompting technique. Its development represents a significant advancement in AI and natural language processing (NLP).


###


Wider Implications


Beyond academia, CoS's ability to improve AI’s spatial understanding could revolutionize robotics, navigation, and gaming. This advancement could make AI technologies more intuitive, laying the groundwork for innovations that change our lives and industries.


In essence, Chain-of-Symbol Prompting is a major leap toward equipping Large Language Models with a better grasp of the physical world.


Its ongoing refinement holds promise for exciting applications across various fields, envisioning a future where AI seamlessly navigates and interacts with both the spatial and linguistic domains.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
