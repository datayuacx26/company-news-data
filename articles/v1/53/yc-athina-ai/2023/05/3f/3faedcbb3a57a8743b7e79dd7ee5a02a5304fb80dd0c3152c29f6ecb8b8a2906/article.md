---
schema_version: "1.0.0"
document_id: "3faedcbb3a57a8743b7e79dd7ee5a02a5304fb80dd0c3152c29f6ecb8b8a2906"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/pre-training-to-learn-in-context"
published_at: "2023-05-16T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:d4ec7f48cab340f38e8638fae6eb95e48bb1b8808e1c1ef68bb2a03438bf88b8"
---

# Pre-Training to Learn in Context

Do not index


Original Paper


[https://arxiv.org/abs/2305.09137](https://arxiv.org/abs/2305.09137)


Blog URL


[blog.athina.ai /pre-tra...-context](https://blog.athina.ai/pre-training-to-learn-in-context)


**Original Paper:**[https://arxiv.org/abs/2305.09137](https://arxiv.org/abs/2305.09137)


**By:**[Yuxian Gu](https://arxiv.org/search/cs?searchtype=author&query=Gu%2C%20Y) ,[Li Dong](https://arxiv.org/search/cs?searchtype=author&query=Dong%2C%20L) ,[Furu Wei](https://arxiv.org/search/cs?searchtype=author&query=Wei%2C%20F) ,[Minlie Huang](https://arxiv.org/search/cs?searchtype=author&query=Huang%2C%20M)


**Abstract:**


> In-context learning, where pre-trained language models learn to perform tasks from task examples and instructions in their contexts, has attracted much attention in the NLP community. However, the ability of in-context learning is not fully exploited because language models are not explicitly trained to learn in context. To this end, we propose PICL (Pre-training for In-Context Learning), a framework to enhance the language models' in-context learning ability by pre-training the model on a large collection of "intrinsic tasks" in the general plain-text corpus using the simple language modeling objective. PICL encourages the model to infer and perform tasks by conditioning on the contexts while maintaining task generalization of pre-trained models. We evaluate the in-context learning performance of the model trained with PICL on seven widely-used text classification datasets and the Super-NaturalInstrctions benchmark, which contains 100+ NLP tasks formulated to text generation. Our experiments show that PICL is more effective and task-generalizable than a range of baselines, outperforming larger language models with nearly 4x parameters. The code is publicly available at
>
>
> [this https URL](https://github.com/thu-coai/PICL)


---


###


Summary Notes


####


Enhancing AI Adaptability with Pre-Training: A Look at the PICL Framework


As artificial intelligence (AI) continues to advance, the importance of pre-trained language models (PLMs) like GPT and BERT has become increasingly evident.


These models are excellent at mimicking human text, but their ability to learn new tasks from context, known as in-context learning (ICL), has been a challenge.


This has led to the creation of the Pre-training for In-Context Learning (PICL) framework, aimed at improving PLMs' adaptability.


This blog post breaks down the PICL framework, its methodology, benefits, and its implications for AI in the enterprise.


####


The Challenge with PLMs and In-Context Learning


While PLMs have been a breakthrough for natural language processing (NLP), their performance in adapting to new tasks through context alone has been less than ideal.


This limitation affects their ability to adjust to specific, real-world tasks without extensive additional training.


####


Introducing the PICL Framework


The PICL framework is designed to bolster the in-context learning abilities of PLMs through a focused training approach. It involves:


- **Identifying Intrinsic Tasks** : Finding naturally occurring tasks within large text datasets.


- **Retrieval-based Training** : Using a retrieval mechanism to gather text with similar tasks, ensuring contextual relevance.


- **Contrastive Learning** : Differentiating between paragraphs with similar and dissimilar tasks during training.


- **Maintaining Generative Capabilities** : Training models using full sequences to preserve the generative nature of PLMs.


####


How PICL Works


The PICL approach is detailed and designed for effective improvement in ICL:


- **Data Collection** : Gathering a large text corpus and identifying paragraphs with similar tasks.


- **Task-Semantics Encoding** : Using a model to encode these paragraphs, ensuring similar tasks are closely related in the embedding space.


- **Retrieval System** : A system retrieves paragraphs with similar tasks, enabling focused training.


- **Training Process** : Models are trained with a mix of task-shared and random paragraphs, employing contrastive loss to enhance learning differences.


####


Efficacy of PICL


PICL's effectiveness is proven through extensive testing:


- **Datasets** : Tests on various classification datasets and the "SUPER-NATURAL INSTRUCTIONS" benchmark show PICL's robust performance.


- **Baseline Comparisons** : PICL outperforms existing methods like Vanilla ICL and MetaICL, showing better task-specific performance even compared to larger models.


####


Looking Ahead


PICL is a significant step forward for improving PLMs' in-context learning. It offers a path to more adaptable and efficient NLP systems, essential for enterprise AI applications. Future directions could include:


- **Integrating Explicit Instructions** : Adding human-written instructions to training could further enhance adaptability.


- **Scaling Studies** : Exploring PICL's effects with larger models and datasets could provide deeper insights.


For AI engineers in enterprises, the PICL framework represents a novel way to maximize the potential of PLMs, leading to more adaptable and capable AI systems for tackling real-world tasks.


In summary, PICL addresses pivotal limitations in current PLMs' in-context learning abilities and sets a new bar for the future of NLP and AI, opening up a world of possibilities for technical teams across various industries.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
