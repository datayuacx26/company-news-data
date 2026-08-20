---
schema_version: "1.0.0"
document_id: "15ef62ff0ef6b07a443679cc6306f0279e981a8dcf94bec530ce047c98d4cdf0"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/deficiency-of-large-language-models-in-finance-an-empirical-examination-of-hallucination"
published_at: "2023-11-27T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:c1b3a4dce6396a347571f57188b33f99b1ae479508a608a8635702ced1f72cc5"
---

# Deficiency of Large Language Models in Finance: An Empirical Examination of Hallucination

Do not index


Original Paper


[https://arxiv.org/abs/2311.15548](https://arxiv.org/abs/2311.15548)


Blog URL


[blog.athina.ai /deficie...cination](https://blog.athina.ai/deficiency-of-large-language-models-in-finance-an-empirical-examination-of-hallucination)


**Original Paper:**[https://arxiv.org/abs/2311.15548](https://arxiv.org/abs/2311.15548)


**By:**[Haoqiang Kang](https://arxiv.org/search/cs?searchtype=author&query=Kang%2C%20H) ,[Xiao-Yang Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20X)


**Abstract:**


> The hallucination issue is recognized as a fundamental deficiency of large language models (LLMs), especially when applied to fields such as finance, education, and law. Despite the growing concerns, there has been a lack of empirical investigation. In this paper, we provide an empirical examination of LLMs' hallucination behaviors in financial tasks. First, we empirically investigate LLM model's ability of explaining financial concepts and terminologies. Second, we assess LLM models' capacity of querying historical stock prices. Third, to alleviate the hallucination issue, we evaluate the efficacy of four practical methods, including few-shot learning, Decoding by Contrasting Layers (DoLa), the Retrieval Augmentation Generation (RAG) method and the prompt-based tool learning method for a function to generate a query command. Finally, our major finding is that off-the-shelf LLMs experience serious hallucination behaviors in financial tasks. Therefore, there is an urgent need to call for research efforts in mitigating LLMs' hallucination.


---


###


Summary Notes


####


Navigating Large Language Models in Finance: Overcoming Hallucination Risks


Large Language Models (LLMs) are at the forefront of revolutionizing the finance sector through advancements in understanding and generating human language.


These models are reshaping portfolio management, market trend analysis, and sentiment evaluation.


However, their integration into financial operations introduces the challenge of hallucination—where LLMs produce convincing yet incorrect information.


####


Understanding Hallucination in Financial LLMs


The hallucination issue with LLMs is particularly problematic in finance, where precision is crucial. The risk of generating misleading data could lead to poor decisions and significant financial losses. Despite the growing use of LLMs, there's a noticeable lack of research on their propensity to hallucinate, especially in tasks needing exact financial knowledge or data retrieval.


####


Examining Hallucination in Finance through Empirical Studies


Recent research by Haoqiang Kang and Xiao-Yang Liu marks a pivotal effort in exploring LLM hallucination within financial contexts.


This study tests LLMs' grasp of financial concepts and their accuracy in retrieving historical stock prices, alongside evaluating methods to counter hallucination. Their findings highlight a worrying trend of factually incorrect outputs by general-purpose LLMs on finance-specific tasks.


####


Strategies to Counter Hallucination: A Guide for AI Engineers


To mitigate hallucination risks, AI engineers, particularly those in enterprise settings, should consider the following strategies:


- **Few-shot Learning and DoLa** : These methods may help to some extent but their effectiveness is limited by the LLM's pre-existing knowledge gaps. They're more beneficial for tasks similar to those the model encountered during training.


- **Retrieval Augmentation Generation (RAG)** : RAG significantly boosts output factuality by integrating external knowledge sources. It excels in tasks that need the latest information or data not included in the model's training.


- **Prompt-based Tool Learning** : Designing specific prompts to guide the LLM can markedly improve its task performance. This strategy is particularly useful for enhancing accuracy in explaining financial terms or accessing historical data without extensive model retraining.


####


Implementing Mitigation Strategies: A Practical Approach


1. **Identify Critical Accuracy Tasks** : Pinpoint financial tasks where accuracy is non-negotiable and evaluate the LLM's current performance.


1. **Choose a Suitable Strategy** : Based on the task and the LLM's limitations, select an appropriate mitigation method. RAG is preferable for tasks needing external knowledge, whereas prompt-based learning suits more defined tasks.


1. **Test and Adjust** : Apply the strategy in a test environment, monitor performance, and tweak as needed.


1. **Embrace Continuous Learning** : Continually update the model or its external data sources with new financial information to maintain accuracy and relevance.


####


Conclusion: Advancing LLMs in Finance


While the challenge of hallucination in LLMs is significant, ongoing research and effective mitigation strategies are equipping AI engineers with the tools to overcome these hurdles.


By following the outlined approaches, enterprises can enhance the reliability and accuracy of their LLM applications, leading to better financial decision-making.


The future should focus not only on refining these strategies but also on discovering new methods to further improve LLM performance in finance. Achieving highly accurate and reliable LLMs is essential for their successful integration into the financial industry, ensuring they become invaluable assets for the future.
