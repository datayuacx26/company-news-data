---
schema_version: "1.0.0"
document_id: "ec5bca0ac6086d6f6c3616f32f0eb1f919325d567506b136caedf09752fc61bd"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/multitask-prompt-tuning-enables-parameter-efficient-transfer-learning"
published_at: "2023-03-06T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:96e31d01e6169c0a5b4626c7c0ee13599e88a2cd8fdcadacd0a177435df945ac"
---

# Multitask Prompt Tuning Enables Parameter-Efficient Transfer Learning

Do not index


Original Paper


[https://arxiv.org/abs/2303.02861](https://arxiv.org/abs/2303.02861)


Blog URL


[blog.athina.ai /multita...learning](https://blog.athina.ai/multitask-prompt-tuning-enables-parameter-efficient-transfer-learning)


**Original Paper:**[https://arxiv.org/abs/2303.02861](https://arxiv.org/abs/2303.02861)


**By:**[Zhen Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20Z) ,[Rameswar Panda](https://arxiv.org/search/cs?searchtype=author&query=Panda%2C%20R) ,[Leonid Karlinsky](https://arxiv.org/search/cs?searchtype=author&query=Karlinsky%2C%20L) ,[Rogerio Feris](https://arxiv.org/search/cs?searchtype=author&query=Feris%2C%20R) ,[Huan Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20H) ,[Yoon Kim](https://arxiv.org/search/cs?searchtype=author&query=Kim%2C%20Y)


**Abstract:**


> Prompt tuning, in which a base pretrained model is adapted to each task via conditioning on learned prompt vectors, has emerged as a promising approach for efficiently adapting large language models to multiple downstream tasks. However, existing methods typically learn soft prompt vectors from scratch, and it has not been clear how to exploit the rich cross-task knowledge with prompt vectors in a multitask learning setting. We propose multitask prompt tuning (MPT), which first learns a single transferable prompt by distilling knowledge from multiple task-specific source prompts. We then learn multiplicative low rank updates to this shared prompt to efficiently adapt it to each downstream target task. Extensive experiments on 23 NLP datasets demonstrate that our proposed approach outperforms the state-of-the-art methods, including the full finetuning baseline in some cases, despite only tuning 0.035% as many task-specific parameters.


---


###


Summary Notes


##


Streamlining AI Model Adaptation with Multitask Prompt Tuning


In the dynamic world of AI and machine learning, customizing pre-trained language models for specific tasks is a common practice. However, as these models grow larger, this process becomes more resource-intensive.


This challenge has led to the development of more efficient transfer learning strategies like Adapters, BitFit, and notably, Prompt Tuning (PT). Despite their benefits, these methods often face limitations in performance or flexibility across different tasks.


This is where Multitask Prompt Tuning (MPT) comes in, offering an innovative solution that enhances efficiency and applicability by utilizing shared knowledge between tasks.


###


Tackling the Parameter Efficiency Challenge


Traditionally, fine-tuning has been the preferred method for adapting large language models to specific tasks, requiring adjustments to a vast number of parameters.


To address this, parameter-efficient methods aim to modify fewer parameters without significantly impacting performance.


PT, for example, modifies a model using a small set of task-specific vectors. However, PT can be sensitive to how it's initially set up and generally performs worse than full fine-tuning.


###


Introducing Multitask Prompt Tuning


####


A Fresh Perspective


MPT brings a new dimension to PT by employing a two-phase process: source training and target adaptation. In the first phase, MPT creates a shared prompt matrix from individual task prompts, building a foundation of common knowledge.


This shared prompt is then fine-tuned for new tasks in the target adaptation phase with minimal adjustments.


####


The Power of Low-Rank Updates


A critical aspect of MPT is its reliance on low-rank updates during target adaptation. This technique allows the model to adapt to the specifics of new tasks efficiently, without the need for a large increase in parameters, thus balancing efficiency with enhanced performance.


###


Proven Effectiveness of MPT


The superiority of MPT isn't just theoretical. When tested across 23 varied NLP tasks, including challenging benchmarks like GLUE and SuperGLUE, MPT consistently outperformed other methods, including full fine-tuning, in most cases. It achieved these results with significantly fewer parameters, showcasing its efficiency. MPT's robustness and adaptability have also been proven across different model sizes and in few-shot scenarios.


###


The Future of Language Model Applications


MPT is paving the way for broader and more efficient use of large language models across various domains. By making it easier to adapt these models to a wide range of tasks without extensive parameter tuning, MPT is opening up new possibilities for their application.


###


Conclusion


Multitask Prompt Tuning represents a significant advancement in AI and machine learning, offering a blend of efficiency and adaptability. Its approach to utilizing shared knowledge across tasks while optimizing parameter use marks a pivotal shift.


For AI engineers, particularly those in enterprise settings, adopting MPT could lead to new levels of efficiency and performance in model adaptation.


As AI technology continues to evolve, staying informed about such innovations becomes crucial. MPT exemplifies the impact of creative solutions in overcoming the constraints of traditional methods, heralding a new era of efficiency and versatility in AI model adaptation.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
