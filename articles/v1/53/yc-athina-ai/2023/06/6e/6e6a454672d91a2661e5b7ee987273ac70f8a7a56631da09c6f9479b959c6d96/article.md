---
schema_version: "1.0.0"
document_id: "6e6a454672d91a2661e5b7ee987273ac70f8a7a56631da09c6f9479b959c6d96"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/compositional-exemplars-for-in-context-learning"
published_at: "2023-06-20T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.924529+00:00"
content_hash: "sha256:66de96425cef2c1baeb1c5b44bc1e5f21322fb42187a81aa9a8e4e98ff9179e6"
---

# Compositional Exemplars for In-context Learning

Do not index


Original Paper


[https://arxiv.org/abs/2302.05698](https://arxiv.org/abs/2302.05698)


Blog URL


[blog.athina.ai /composi...learning](https://blog.athina.ai/compositional-exemplars-for-in-context-learning)


**Original Paper:**[https://arxiv.org/abs/2302.05698](https://arxiv.org/abs/2302.05698)


**By:**[Jiacheng Ye](https://arxiv.org/search/cs?searchtype=author&query=Ye%2C%20J) ,[Zhiyong Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20Z) ,[Jiangtao Feng](https://arxiv.org/search/cs?searchtype=author&query=Feng%2C%20J) ,[Tao Yu](https://arxiv.org/search/cs?searchtype=author&query=Yu%2C%20T) ,[Lingpeng Kong](https://arxiv.org/search/cs?searchtype=author&query=Kong%2C%20L)


**Abstract:**


> Large pretrained language models (LMs) have shown impressive In-Context Learning (ICL) ability, where the model learns to do an unseen task via a prompt consisting of input-output examples as the demonstration, without any parameter updates. The performance of ICL is highly dominated by the quality of the selected in-context examples. However, previous selection methods are mostly based on simple heuristics, leading to sub-optimal performance. In this work, we formulate in-context example selection as a subset selection problem. We propose CEIL (Compositional Exemplars for In-context Learning), which is instantiated by Determinantal Point Processes (DPPs) to model the interaction between the given input and in-context examples, and optimized through a carefully-designed contrastive learning objective to obtain preference from LMs. We validate CEIL on 12 classification and generation datasets from 7 distinct NLP tasks, including sentiment analysis, paraphrase detection, natural language inference, commonsense reasoning, open-domain question answering, code generation, and semantic parsing. Extensive experiments demonstrate not only the state-of-the-art performance but also the transferability and compositionality of CEIL, shedding new light on effective and efficient in-context learning. Our code is released at
>
>
> [this https URL](https://github.com/HKUNLP/icl-ceil)


---


###


Summary Notes


###


Revolutionizing AI with CEIL: A New Approach to In-context Learning


In the fast-paced world of natural language processing (NLP), the ability of large language models (LMs) to learn new tasks using a few examples, known as in-context learning (ICL), has been a game-changer.


However, choosing the right examples for ICL is tricky and critical for its success. Traditional methods for selecting these examples haven't been very effective, often leading to poor or redundant choices.


Enter "Compositional Exemplars for In-context Learning" (CEIL), a groundbreaking approach aiming to drastically improve how we select examples for LMs, making ICL more robust and efficient.


####


Understanding the Role of DPPs


CEIL is built on Determinantal Point Processes (DPPs), a type of probabilistic model perfect for choosing diverse and relevant subsets.


DPPs work by creating a kernel matrix from the items' feature vectors and using its determinant to measure the probability of selecting a subset.


This method favors subsets with diverse, high-quality items, ideal for in-context example selection.


####


Key Features of CEIL


- **Subset Selection Problem** : CEIL views example selection as a subset selection problem, using DPPs to model the joint probability of potential example sets, ensuring diversity and relevance.


- **Conditional DPPs** : It introduces conditional DPPs that adapt the selection process to specific input requirements, making the model more flexible and effective.


- **Contrastive Learning Setup** : By integrating a scoring function from the LM into the DPP model, CEIL enhances its prediction capabilities, choosing more useful example subsets.


####


Proven Results


CEIL has been tested across 12 datasets covering 7 NLP tasks, such as sentiment analysis and semantic parsing. It consistently outperformed existing methods, demonstrating superior generalization and compositional understanding capabilities, which are crucial for complex queries.


####


Impact on AI Engineering


For AI engineers, particularly those in enterprise settings, CEIL offers a significant leap forward. It provides a more reliable method for in-context learning by ensuring the diversity and relevance of examples. This has vast implications for tasks that require deep understanding, improving generalization and performance.


Moreover, CEIL's use of DPPs opens new research avenues in probabilistic models for machine learning, offering exciting possibilities for future innovations.


####


Conclusion


CEIL represents a major advancement in enhancing in-context learning for language models.


By utilizing Determinantal Point Processes for smarter example selection, it not only boosts the performance and robustness of LMs in various NLP tasks but also highlights the potential of probabilistic models to tackle complex machine learning challenges.


As AI progresses, methodologies like CEIL will be key to unlocking the full potential of machine learning models, leading to smarter, more adaptable, and efficient AI systems.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
