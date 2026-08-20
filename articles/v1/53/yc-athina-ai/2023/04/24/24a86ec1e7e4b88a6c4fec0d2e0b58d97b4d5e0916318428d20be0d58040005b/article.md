---
schema_version: "1.0.0"
document_id: "24a86ec1e7e4b88a6c4fec0d2e0b58d97b4d5e0916318428d20be0d58040005b"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/refiner-reasoning-feedback-on-intermediate-representations"
published_at: "2023-04-04T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:b1359bfd984eafeccb66f181fc4e6efdc4f48cc9c66758328ff9959266b854aa"
---

# REFINER: Reasoning Feedback on Intermediate Representations

Do not index


Original Paper


[https://arxiv.org/abs/2304.01904](https://arxiv.org/abs/2304.01904)


Blog URL


[blog.athina.ai /refiner...ntations](https://blog.athina.ai/refiner-reasoning-feedback-on-intermediate-representations)


**Original Paper:**[https://arxiv.org/abs/2304.01904](https://arxiv.org/abs/2304.01904)


**By:**[Debjit Paul](https://arxiv.org/search/cs?searchtype=author&query=Paul%2C%20D) ,[Mete Ismayilzada](https://arxiv.org/search/cs?searchtype=author&query=Ismayilzada%2C%20M) ,[Maxime Peyrard](https://arxiv.org/search/cs?searchtype=author&query=Peyrard%2C%20M) ,[Beatriz Borges](https://arxiv.org/search/cs?searchtype=author&query=Borges%2C%20B) ,[Antoine Bosselut](https://arxiv.org/search/cs?searchtype=author&query=Bosselut%2C%20A) ,[Robert West](https://arxiv.org/search/cs?searchtype=author&query=West%2C%20R) ,[Boi Faltings](https://arxiv.org/search/cs?searchtype=author&query=Faltings%2C%20B)


**Abstract:**


> Language models (LMs) have recently shown remarkable performance on reasoning tasks by explicitly generating intermediate inferences, e.g., chain-of-thought prompting. However, these intermediate inference steps may be inappropriate deductions from the initial context and lead to incorrect final predictions. Here we introduce REFINER, a framework for finetuning LMs to explicitly generate intermediate reasoning steps while interacting with a critic model that provides automated feedback on the reasoning. Specifically, the critic provides structured feedback that the reasoning LM uses to iteratively improve its intermediate arguments. Empirical evaluations of REFINER on three diverse reasoning tasks show significant improvements over baseline LMs of comparable scale. Furthermore, when using GPT-3.5 or ChatGPT as the reasoner, the trained critic significantly improves reasoning without finetuning the reasoner. Finally, our critic model is trained without expensive human-in-the-loop data but can be substituted with humans at inference time.


---


###


Summary Notes


####


Enhancing Language Models' Reasoning with REFINER


Language models (LMs) have significantly impacted natural language processing tasks, from automating customer service to content creation.


However, their reasoning abilities, especially in complex scenarios, need improvement. The REFINER framework aims to boost these capabilities by refining the reasoning process through iterative feedback.


####


Key Features of REFINER


REFINER enhances LMs by using a unique approach that involves a critic model providing detailed feedback to a generator model.


This process allows for the continuous improvement of reasoning steps, making complex problem-solving more effective.


####


REFINER's Structure


- **Generator Model** : Generates steps towards solving reasoning tasks.


- **Critic Model** : Assesses these steps, offering detailed feedback to improve accuracy.


####


How REFINER Works


To build a strong Critic model, REFINER uses:


- **Feedback Generation Strategies** : These include rule-based changes and synthetic data creation with models like GPT-3.5.


- **Semi-Structured Feedback** : This feedback, which mentions specific error types and offers hints, helps refine the Generator model's outputs.


####


Testing REFINER


REFINER was evaluated on various tasks:


1. **Math Word Problems (MWP)** : Creating correct math equations from text.


1. **Synthetic Natural Language Reasoning (sNLR)** : Making logical deductions based on rules and facts.


1. **Moral Action Generation** : Determining moral actions in different contexts.


####


Achievements


REFINER outperformed standard models, improving the creation of reasoning steps and final outcomes across all tested areas.


It even enhanced the performance of advanced models like GPT-3.5 without specific adjustments, demonstrating the value of detailed feedback over simpler feedback forms.


####


Benefits of Using REFINER


- **Better Reasoning** : Focuses on refining reasoning steps for higher accuracy in complex tasks.


- **Flexible Interaction** : Allows dynamic exchanges between the Generator and Critic models during both training and use.


- **Human-in-the-loop** : Supports incorporating human feedback, enhancing adaptability for complex issues.


####


Future Directions and Impact


REFINER represents a major advancement in making LMs capable of more sophisticated reasoning. It opens possibilities for improving LMs' interpretability and reliability. Future work could extend its application to more tasks and develop Critic models that handle a broader range of errors.


####


Conclusion


REFINER is a promising step towards creating LMs that can effectively understand and reason through complicated issues.


Its application could revolutionize AI solutions in scenarios where complex reasoning is crucial, offering a path towards more advanced, trustworthy, and efficient AI systems.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
