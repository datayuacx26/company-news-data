---
schema_version: "1.0.0"
document_id: "15aa33e98c2b18d3d0dc3a489305644e551b4a797bece1d0052ee165587911ae"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/boosted-prompt-ensembles-for-large-language-models"
published_at: "2023-04-12T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:3dfadd46a6ef5f9067617027e257784097f90aa19b0235b8def30c92ef09b642"
---

# Boosted Prompt Ensembles for Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2304.05970](https://arxiv.org/abs/2304.05970)


Blog URL


[blog.athina.ai /boosted...e-models](https://blog.athina.ai/boosted-prompt-ensembles-for-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2304.05970](https://arxiv.org/abs/2304.05970)


**By:**[Silviu Pitis](https://arxiv.org/search/cs?searchtype=author&query=Pitis%2C%20S) ,[Michael R. Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20M%20R) ,[Andrew Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20A) ,[Jimmy Ba](https://arxiv.org/search/cs?searchtype=author&query=Ba%2C%20J)


**Abstract:**


> Methods such as chain-of-thought prompting and self-consistency have pushed the frontier of language model reasoning performance with no additional training. To further improve performance, we propose a prompt ensembling method for large language models, which uses a small dataset to construct a set of few shot prompts that together comprise a \`\`boosted prompt ensemble''. The few shot examples for each prompt are chosen in a stepwise fashion to be \`\`hard'' examples on which the previous step's ensemble is uncertain. We show that this outperforms single-prompt output-space ensembles and bagged prompt-space ensembles on the GSM8k and AQuA datasets, among others. We propose both train-time and test-time versions of boosted prompting that use different levels of available annotation and conduct a detailed empirical study of our algorithm.


---


###


Summary Notes


##


Boosting Large Language Model Abilities with Enhanced Prompt Techniques


In the realm of artificial intelligence, Large Language Models (LLMs) such as GPT-3 have been breaking new ground. They've shown remarkable skill in learning quickly with just a few examples. This skill has been further improved by introducing reasoning steps, or a "chain of thought," into their processes, significantly enhancing their capabilities. Building on this progress, we've developed a new method to push the boundaries even further: **boosted prompt ensembles** .


###


Background: Preparing the Ground for New Developments


The success of LLMs isn't just about their design; it also heavily relies on how they're prompted. The way we design these prompts can greatly influence a model's effectiveness in tackling tasks. Researchers have been optimizing this through techniques like automatic prompt engineering and strategies for selecting the best examples. Additionally, methods like self-consistency, which generate multiple reasoning paths and choose the most consistent solution, have improved the models' reasoning abilities. Inspired by the concept of boosting in ensemble learning—which improves performance by concentrating on difficult examples—we see a new opportunity for enhancing LLM performance.


###


What Are Boosted Prompt Ensembles?


At the core of our method is a set of few-shot prompts that together help the LLM handle a wider variety of problems.


This is done by focusing on examples where the model shows uncertainty or lower performance. We've developed two variations of this approach:


- **Train-time boosting:** Uses a labeled dataset to find and concentrate on challenging examples.


- **Test-time boosting:** Utilizes the model's own predictions to spot and adjust to difficult cases, which is especially useful when facing new types of problems.


###


Solid Results: Beating Standard Approaches


Our extensive testing, including on datasets like AQUA and GSM8k, reveals that boosted prompt ensembles consistently surpass traditional methods, such as single-prompt strategies and bagged ensembles.


This is particularly true for situations with small training sets or less-than-ideal initial prompts. Our method's robustness is clear across various scenarios, showing a notable boost in model performance.


###


Insights from Our Analysis


Our detailed examination highlights several important findings:


- **Broad Effectiveness:** Boosted prompting outperforms other methods in a range of test scenarios.


- **Initial Prompt Quality:** While starting with a good prompt helps, our method can effectively improve from a less optimal beginning.


- **Flexible to Ensemble and Sample Size:** The technique works well with different numbers of prompts and samples, adjusting as needed.


- **Consistency Across Different LLMs:** Our boosted prompting approach works well across various LLMs, showcasing its versatility and effectiveness.


###


Conclusion: Advancing LLM Reasoning Capabilities


Boosted prompt ensembles mark a significant step in improving the reasoning abilities of Large Language Models without extra training.


By strategically focusing on more challenging examples, this method not only enhances performance but also deepens the model's understanding, enabling it to tackle complex tasks more effectively.


This advancement has wide implications for AI applications needing advanced reasoning and decision-making, representing a crucial development in intelligent system design.


In conclusion, boosted prompt ensembles introduce a powerful and innovative way to improve LLM performance, setting a new standard for tackling complex AI challenges.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
