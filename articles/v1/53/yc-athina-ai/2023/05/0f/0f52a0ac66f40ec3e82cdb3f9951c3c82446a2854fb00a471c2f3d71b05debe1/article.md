---
schema_version: "1.0.0"
document_id: "0f52a0ac66f40ec3e82cdb3f9951c3c82446a2854fb00a471c2f3d71b05debe1"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/hierarchical-prompting-assists-large-language-model-on-web-navigation"
published_at: "2023-05-23T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:1d77410fdedb6bb0c76bdc8c279ad5a02fd30d62e9d0c1b7282fabbd26ee665a"
---

# Hierarchical Prompting Assists Large Language Model on Web Navigation

Do not index


Original Paper


[https://arxiv.org/abs/2305.14257](https://arxiv.org/abs/2305.14257)


Blog URL


[blog.athina.ai /hierarc...vigation](https://blog.athina.ai/hierarchical-prompting-assists-large-language-model-on-web-navigation)


**Original Paper:**[https://arxiv.org/abs/2305.14257](https://arxiv.org/abs/2305.14257)


**By:**[Abishek Sridhar](https://arxiv.org/search/cs?searchtype=author&query=Abishek) ,[Robert Lo](https://arxiv.org/search/cs?searchtype=author&query=Lo%2C%20R) ,[Frank F. Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20F%20F) ,[Hao Zhu](https://arxiv.org/search/cs?searchtype=author&query=Zhu%2C%20H) ,[Shuyan Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20S)


**Abstract:**


> Large language models (LLMs) struggle on processing complicated observations in interactive decision making tasks. To alleviate this issue, we propose a simple hierarchical prompting approach. Diverging from previous prompting approaches that always put the full observation (e.g. a web page) to the prompt, we propose to first construct an action-aware observation which is more condensed and relevant with a dedicated SUMMARIZER prompt. The ACTOR prompt then predicts the next action based on the summarized observation. While our method has broad applicability, we particularly demonstrate its efficacy in the complex domain of web navigation where a full observation often contains redundant and irrelevant information. Our approach outperforms the previous state-of-the-art prompting mechanics by 6.2% on task success rate, demonstrating its potential on interactive decision making tasks with long observation traces.


---


###


Summary Notes


##


Revolutionizing AI Decision-Making with ASH: A New Frontier for AI Engineers


In the fast-paced world of artificial intelligence (AI), particularly in the realm of large language models (LLMs), the challenge to automate complex decision-making through natural language is growing.


AI engineers, especially those in enterprise settings, are constantly seeking efficient solutions. The introduction of the hierarchical prompting method known as ASH (Actor-Summarizer-Hierarchical) marks a significant advancement in tackling these complexities.


###


**The ASH Prompting Mechanism: Transforming AI**


ASH stands out with its dual-component structure, consisting of the **Summarizer** and the **Actor** , designed to enhance task execution precision. Here's how they work:


- **Summarizer** : Filters essential information from raw data, ensuring decisions are based on relevant details.


- **Actor** : Uses the summarized data to predict and carry out actions accurately.


For instance, in an AI navigating an e-commerce site to find a product, the Summarizer would identify key product features from search results. The Actor would then accurately select the product based on these features.


###


**Implementation and Impact**


Applying ASH in simulated e-commerce web navigation tasks resulted in a 6.8% increase in task success rate compared to traditional methods. This boost signifies a major advancement in managing complex tasks and long observation sequences.


###


**Diving Into the Experiment**


The experiments were conducted using the Webshop dataset, reflecting the complexity of real online shopping, and employed models like CODE-DAVINCI-002 and GPT-3.5-TURBO, fine-tuned for ASH’s requirements.


###


**Why ASH Excels**


Key benefits of ASH include:


- **Structured Approach** : Simplifies the observation process, reducing cognitive load and leading to better decisions.


- **Efficiency and Accuracy** : Prioritizes important information for quicker, more precise decisions.


ASH outperforms traditional methods with its structured, efficient approach, especially in complex scenarios.


###


**Conclusion: ASH's Bright Future**


ASH represents a breakthrough in using LLMs for interactive, complex decision-making. Its success in web navigation is just the start, with potential applications across various fields awaiting exploration.


###


**What's Next for ASH**


The adaptability of ASH to different interactive environments and its potential to improve decision-making across domains present exciting opportunities for future research.


###


**Acknowledgements**


This pioneering work, supported by the Air Force Research Laboratory and contributions from academia, showcases the impact of collaborative innovation in AI.


For AI engineers in enterprise settings, ASH is more than a new tool—it's a step towards smarter, more efficient AI decision-making. The future is structured, hierarchical, and full of promise.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
