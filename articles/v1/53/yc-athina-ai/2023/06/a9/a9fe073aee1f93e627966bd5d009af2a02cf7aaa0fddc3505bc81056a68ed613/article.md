---
schema_version: "1.0.0"
document_id: "a9fe073aee1f93e627966bd5d009af2a02cf7aaa0fddc3505bc81056a68ed613"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompt-tuning-large-language-models-on-personalized-aspect-extraction-for-recommendations"
published_at: "2023-06-02T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.924529+00:00"
content_hash: "sha256:4e1bfbcbef7f8eac9786e1b917c6cc385f31a8b2bc502e12195fdf99ecbbd48c"
---

# Prompt Tuning Large Language Models on Personalized Aspect Extraction for Recommendations

Do not index


Original Paper


[https://arxiv.org/abs/2306.01475](https://arxiv.org/abs/2306.01475)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2306.01475](https://arxiv.org/abs/2306.01475)


**By:**[Pan Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20P) ,[Yuyan Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20Y) ,[Ed H. Chi](https://arxiv.org/search/cs?searchtype=author&query=Chi%2C%20E%20H) ,[Minmin Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20M)


**Abstract:**


> Existing aspect extraction methods mostly rely on explicit or ground truth aspect information, or using data mining or machine learning approaches to extract aspects from implicit user feedback such as user reviews. It however remains under-explored how the extracted aspects can help generate more meaningful recommendations to the users. Meanwhile, existing research on aspect-based recommendations often relies on separate aspect extraction models or assumes the aspects are given, without accounting for the fact the optimal set of aspects could be dependent on the recommendation task at hand.
>
>
> In this work, we propose to combine aspect extraction together with aspect-based recommendations in an end-to-end manner, achieving the two goals together in a single framework. For the aspect extraction component, we leverage the recent advances in large language models and design a new prompt learning mechanism to generate aspects for the end recommendation task. For the aspect-based recommendation component, the extracted aspects are concatenated with the usual user and item features used by the recommendation model. The recommendation task mediates the learning of the user embeddings and item embeddings, which are used as soft prompts to generate aspects. Therefore, the extracted aspects are personalized and contextualized by the recommendation task. We showcase the effectiveness of our proposed method through extensive experiments on three industrial datasets, where our proposed framework significantly outperforms state-of-the-art baselines in both the personalized aspect extraction and aspect-based recommendation tasks. In particular, we demonstrate that it is necessary and beneficial to combine the learning of aspect extraction and aspect-based recommendation together. We also conduct extensive ablation studies to understand the contribution of each design component in our framework.


---


###


Summary Notes


##


Boosting Personalization in Recommendations Using Large Language Models


The field of AI and machine learning is rapidly advancing, with a strong focus on creating more personalized recommendation systems.


A groundbreaking approach introduced by Pan Li and his colleagues harnesses the power of Large Language Models (LLMs) to merge aspect extraction directly with the recommendation process, paving the way for unprecedented levels of personalization and user satisfaction.


###


Overcoming Traditional Limitations


Recommendation systems have traditionally kept aspect extraction separate from the actual recommendations. This separation often leads to recommendations that lack personal relevance to the user.


####


Innovative Integration with LLMs


Li's team offers a solution that integrates aspect extraction into the recommendation engine through the use of LLMs. This method not only simplifies the recommendation process but also significantly improves how user feedback is understood and utilized.


####


Key Components


- **Soft Prompt Tuning** : This technique fine-tunes LLMs to extract user-specific aspects from feedback, incorporating user and item information for highly personalized aspect extraction.


- **Personalized Recommendations** : The model uses extracted aspects and a neural attention mechanism to generate tailored recommendations for each user.


###


Demonstrated Success


This integrated model was tested on datasets from TripAdvisor, Amazon, and Yelp, showcasing its superior performance over existing models in metrics such as Precision@3, Recall@3, F1-Score, RMSE, MAE, and AUC.


####


Key Benefits


- **Increased Personalization** : Directly integrating aspect extraction with recommendations leads to more personalized outcomes.


- **Enhanced Performance** : The model consistently outperforms traditional systems across all tested metrics.


###


Implications and Future Work


For AI engineers, especially those in enterprise settings, this methodology represents a significant step forward in developing personalized, user-centric recommendation systems. It promises scalability and effectiveness across diverse scenarios.


####


Looking Ahead


Potential improvements include refining prompt design and embedding techniques and exploring solutions for cold-start challenges in recommendation systems.


###


Final Thoughts


The fusion of aspect extraction and recommendation through LLMs and personalized prompts offers a new frontier in creating personalized, understandable recommendation systems.


This research provides a solid foundation and guidance for AI engineers aiming to enhance personalization and user satisfaction in their projects.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
