---
schema_version: "1.0.0"
document_id: "73da17b78b49cb81b5df55fd4ce6f7f531233084355afa0e89a2dc2d7ae59328"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/why-think-step-by-step-reasoning-emerges-from-the-locality-of-experience"
published_at: "2023-04-07T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:8a985985d837fa1553ded9f647483ef08b07f3cc87ecda7c2c54f00de4e85a56"
---

# Why think step by step? Reasoning emerges from the locality of experience

Do not index


Original Paper


[https://arxiv.org/abs/2304.03843](https://arxiv.org/abs/2304.03843)


Blog URL


[blog.athina.ai /why-thi...perience](https://blog.athina.ai/why-think-step-by-step-reasoning-emerges-from-the-locality-of-experience)


**Original Paper:**[https://arxiv.org/abs/2304.03843](https://arxiv.org/abs/2304.03843)


**By:**[Ben Prystawski](https://arxiv.org/search/cs?searchtype=author&query=Prystawski%2C%20B) ,[Michael Y. Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20M%20Y) ,[Noah D. Goodman](https://arxiv.org/search/cs?searchtype=author&query=Goodman%2C%20N%20D)


**Abstract:**


> Humans have a powerful and mysterious capacity to reason. Working through a set of mental steps enables us to make inferences we would not be capable of making directly even though we get no additional data from the world. Similarly, when large language models generate intermediate steps (a chain of thought) before answering a question, they often produce better answers than they would directly. We investigate why and how chain-of-thought reasoning is useful in language models, testing the hypothesis that reasoning is effective when training data consists of overlapping local clusters of variables that influence each other strongly. These training conditions enable the chaining of accurate local inferences to estimate relationships between variables that were not seen together in training. We prove that there will exist a "reasoning gap", where reasoning through intermediate variables reduces bias, for the simple case of an autoregressive density estimator trained on local samples from a chain-structured probabilistic model. We then test our hypothesis experimentally in more complex models, training an autoregressive language model on samples from Bayes nets but only including a subset of variables in each sample. We test language models' ability to match conditional probabilities with and without intermediate reasoning steps, finding that intermediate steps are only helpful when the training data is locally structured with respect to dependencies between variables. The combination of locally structured observations and reasoning is much more data-efficient than training on all variables. Our results illustrate how the effectiveness of reasoning step by step is rooted in the local statistical structure of the training data.


---


###


Summary Notes


##


Simplifying AI Reasoning: The Role of Locally Structured Data


In the fast-paced world of artificial intelligence (AI), understanding how AI solves complex problems is crucial.


As AI continues to advance, a key question emerges: How can we enhance AI reasoning to be both more effective and efficient? Recent studies highlight that the answer may involve using locally structured data, which could significantly improve how AI models reason.


####


Why Local Data Structure Matters


The core of efficient AI reasoning is its ability to link different pieces of data. Local data structure plays a vital role here, as it refers to data points that are closely related either by space or context. This closeness forms a network of information, enabling AI models to navigate and understand it better.


####


Overcoming Challenges with Reasoning


AI models often face challenges when they come across incomplete data. To tackle this, AI needs to reason in a step-by-step manner, much like how humans think, by making logical connections between different pieces of information.


####


How to Test AI Reasoning


Testing the effectiveness of AI reasoning involves a few crucial steps:


- **Training AI Models:** Using autoregressive transformers, models are trained on datasets that mimic human learning's local structure.


- **Setting Up Experiments:** These models are then tested on their ability to predict outcomes, comparing direct predictions and those made through reasoning.


- **Creating Datasets:** The datasets used include different local structures, with some data deliberately hidden, to imitate real-world situations of incomplete data.


####


Experiments and Findings


Research on both simple and complex probabilistic models shows that reasoning through intermediate steps significantly boosts AI performance, particularly when direct data about relationships is missing. Models trained on locally structured data perform much better than those trained on random, unstructured data.


####


The Value of Structured Learning


These findings are groundbreaking for AI development. By imitating the way humans learn, AI models become more efficient and effective. The key to their success seems to be their ability to reason through intermediate steps.


####


Looking Ahead: The Future of AI Training


This study highlights the importance of including locally structured data in AI training. This method not only supports step-by-step reasoning but could also lower the need for vast amounts of data and reduce computational costs.


Moving forward, the focus will be on exploring more complex forms of reasoning and understanding the impact of different types of data.


####


Next Steps in AI Research


The quest to develop AI that reasons and learns like humans continues. Future research will delve into abstract reasoning and the role of structured learning, aiming for breakthroughs that enhance AI's problem-solving skills.


For AI engineers, adopting these new insights and methods promises to usher in a new era of sophisticated, efficient AI models capable of navigating the world's complexities like never before.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
