---
schema_version: "1.0.0"
document_id: "50e3f5f2da5a5738ef6a000e2626427ce3e10e88dee91b0e1735d15cf25fe5e0"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/fairness-guided-few-shot-prompting-for-large-language-models"
published_at: "2023-03-23T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:7f74bb5a6f60b0ca25e2ca0ea7c1a27503141d21dd847371f5718ba8f602effa"
---

# Fairness-guided Few-shot Prompting for Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2303.13217](https://arxiv.org/abs/2303.13217)


Blog URL


[blog.athina.ai /fairnes...e-models](https://blog.athina.ai/fairness-guided-few-shot-prompting-for-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2303.13217](https://arxiv.org/abs/2303.13217)


**By:**[Huan Ma](https://arxiv.org/search/cs?searchtype=author&query=Ma%2C%20H) ,[Changqing Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20C) ,[Yatao Bian](https://arxiv.org/search/cs?searchtype=author&query=Bian%2C%20Y) ,[Lemao Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20L) ,[Zhirui Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20Z) ,[Peilin Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20P) ,[Shu Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20S) ,[Huazhu Fu](https://arxiv.org/search/cs?searchtype=author&query=Fu%2C%20H) ,[Qinghua Hu](https://arxiv.org/search/cs?searchtype=author&query=Hu%2C%20Q) ,[Bingzhe Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20B)


**Abstract:**


> Large language models have demonstrated surprising ability to perform in-context learning, i.e., these models can be directly applied to solve numerous downstream tasks by conditioning on a prompt constructed by a few input-output examples. However, prior research has shown that in-context learning can suffer from high instability due to variations in training examples, example order, and prompt formats. Therefore, the construction of an appropriate prompt is essential for improving the performance of in-context learning. In this paper, we revisit this problem from the view of predictive bias. Specifically, we introduce a metric to evaluate the predictive bias of a fixed prompt against labels or a given attributes. Then we empirically show that prompts with higher bias always lead to unsatisfactory predictive quality. Based on this observation, we propose a novel search strategy based on the greedy search to identify the near-optimal prompt for improving the performance of in-context learning. We perform comprehensive experiments with state-of-the-art mainstream models such as GPT-3 on various downstream tasks. Our results indicate that our method can enhance the model's in-context learning performance in an effective and interpretable manner.


---


###


Summary Notes


####


Blog Post: Simplifying Fairness in Large Language Models through Better Prompting


In the world of AI, Large Language Models (LLMs) like GPT-3 and BLOOM are making waves with their ability to learn from context (known as in-context learning or ICL).


Yet, they face a big hurdle: the instability in their performance due to how prompts are constructed, often leading to predictive bias.


This blog explores a fresh strategy aimed at making prompt construction fairer, improving both the fairness and performance of LLMs.


####


Understanding the Challenge of Predictive Bias


In-context learning allows models to understand and perform tasks by looking at a few examples in the prompt, eliminating the need for retraining.


However, the way examples are chosen, their order, and how prompts are formatted can majorly impact results.


The core problem is predictive bias, where the model's predictions unintentionally lean towards certain outcomes based on prompt design.


####


Introducing Fairness-Guided Prompting


####


Evaluating Predictive Bias


To address this, we focus on identifying and reducing predictive bias in prompts.


A new metric helps measure a prompt's fairness by checking how evenly a model predicts outcomes using a "content-free" prompt.


This metric is key for linking prompt fairness with improved in-context learning outcomes.


####


Strategies for Fair Prompt Construction


We propose two strategies for building fairer prompts:


- **T-fair-Prompting** : This approach calculates the predictive bias of individual examples and uses the top-k least biased ones to construct the prompt. It's simple yet effective, with a complexity of O(N).


- **G-fair-Prompting** : A more advanced method, G-fair-Prompting uses a greedy search algorithm to progressively choose examples that best improve the overall fairness score. It's more demanding computationally but significantly better at constructing high-quality prompts.


####


Testing and Results


We tested these strategies on various tasks with models like GPT-3, and the results were promising:


- Both T-fair-Prompting and G-fair-Prompting improved in-context learning.


- G-fair-Prompting consistently outdid T-fair-Prompting and even some of the best current methods in reducing predictive bias.


####


Benefits of Fairness-Guided Prompting


This approach offers several advantages:


- **Efficiency** : Both methods are computationally practical, especially T-fair-Prompting.


- **Effectiveness** : They clearly enhance LLM performance in in-context learning tasks.


- **Interpretability** : These strategies improve prompt quality in a direct and clear way, unlike some methods that tweak model embeddings or make after-the-fact adjustments.


####


What's Next


This exploration into fairness-guided prompting opens up new possibilities for making LLMs more reliable and fair in in-context learning.


Future research could look into different ways to measure prompt fairness and apply these strategies across more models and scenarios.


####


Code and More Information


For those interested in trying out or learning more about fairness-guided prompting, the code is available on GitHub:[https://github.com/MaHuanAAA](https://github.com/MaHuanAAA) .


This study contributes to the growing body of knowledge on in-context learning and prompt optimization.


####


Conclusion


Fairness-guided prompting is a vital step towards overcoming the challenges of predictive bias in large language models.


By centering on the construction of fair prompts, we're moving towards more reliable, equitable, and effective AI applications in various fields.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
