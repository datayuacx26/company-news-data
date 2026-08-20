---
schema_version: "1.0.0"
document_id: "3085e1bb8df6003f2750183fde7365e5d2123448755840f5ccb2a2bdc03a6e38"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/text-driven-prompt-generation-for-vision-language-models-in-federated-learning"
published_at: "2023-10-09T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:cb3c9e451c49a069e6af3745dfdd7c7c54084a4798e1bbd6263082c1b1439a29"
---

# Text-driven Prompt Generation for Vision-Language Models in Federated Learning

Do not index


Original Paper


[https://arxiv.org/abs/2310.06123](https://arxiv.org/abs/2310.06123)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2310.06123](https://arxiv.org/abs/2310.06123)


**By:**[Chen Qiu](https://arxiv.org/search/cs?searchtype=author&query=Qiu%2C%20C) ,[Xingyu Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20X) ,[Chaithanya Kumar Mummadi](https://arxiv.org/search/cs?searchtype=author&query=Mummadi%2C%20C%20K) ,[Madan Ravi Ganesh](https://arxiv.org/search/cs?searchtype=author&query=Ganesh%2C%20M%20R) ,[Zhenzhen Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20Z) ,[Lu Peng](https://arxiv.org/search/cs?searchtype=author&query=Peng%2C%20L) ,[Wan-Yi Lin](https://arxiv.org/search/cs?searchtype=author&query=Lin%2C%20W)


**Abstract:**


> Prompt learning for vision-language models, e.g., CoOp, has shown great success in adapting CLIP to different downstream tasks, making it a promising solution for federated learning due to computational reasons. Existing prompt learning techniques replace hand-crafted text prompts with learned vectors that offer improvements on seen classes, but struggle to generalize to unseen classes. Our work addresses this challenge by proposing Federated Text-driven Prompt Generation (FedTPG), which learns a unified prompt generation network across multiple remote clients in a scalable manner. The prompt generation network is conditioned on task-related text input, thus is context-aware, making it suitable to generalize for both seen and unseen classes. Our comprehensive empirical evaluations on nine diverse image classification datasets show that our method is superior to existing federated prompt learning methods, that achieve overall better generalization on both seen and unseen classes and is also generalizable to unseen datasets.


---


###


Summary Notes


##


Making Federated Learning Smarter with Text-Driven Prompts for Vision-Language Models


In the fast-paced world of artificial intelligence, combining vision-language models with federated learning is proving to be a promising approach for secure and efficient computation. Yet, this blend poses challenges, especially when it comes to handling diverse and spread-out datasets.


Enter the Federated Text-driven Prompt Generation (FedTPG), a novel solution designed to enhance the adaptability of models in federated learning environments. This post explores how FedTPG is setting new benchmarks for model generalization across varied classes and datasets.


###


Understanding Vision-Language Models and Federated Learning


Vision-language models, such as CLIP, are adept at deciphering the complex relationship between text and images.


Their applications range from simple image classification to complex tasks requiring a nuanced understanding of both visual and textual content.


Federated learning, conversely, is a distributed approach to machine learning that trains models across multiple devices or servers without centralizing data.


This method is particularly useful for preserving privacy and improving efficiency in situations where consolidating data isn't feasible due to privacy laws or other restrictions.


Merging these two technologies holds great potential but comes with its share of challenges, primarily how to effectively adapt vision-language models for federated learning to ensure optimal performance across diverse tasks and datasets.


###


Introducing FedTPG


FedTPG stands as a pioneering approach, utilizing text-driven prompts to make vision-language models more adaptable in federated settings.


It uses text descriptions related to specific tasks to generate context-sensitive prompts, enhancing the model's ability to generalize to new classes and datasets.


####


Background


Prompt learning techniques like CoOp and CoCoOp have made strides in making vision-language models more adaptable.


However, these methods struggle in federated settings due to the dispersed nature of data and computing resources.


FedTPG builds upon these methodologies, introducing a strategy that combines prompt learning's adaptability with federated learning's decentralized approach. It specifically targets generalization and efficiency challenges, setting a new performance standard in federated learning contexts.


####


How It Works


FedTPG's approach includes:


- **Setting the Scene:** It considers a federated learning scenario with various clients, each with unique datasets. The goal is to create a unified model that excels across different tasks and unseen data.


- **Generating Text-Driven Prompts:** By using text related to tasks, FedTPG creates prompt vectors that are context-aware, leveraging text's semantic depth to enhance task-specific performance.


- **Local and Central Training:** The model enables collaborative training among clients, optimizing the prompt generator locally. These improvements are aggregated by a central server, refining the global model.


####


Results


FedTPG's testing across nine image classification datasets showcases its superior generalization abilities to unseen classes and datasets.


It consistently surpasses other federated prompt learning methods, highlighting its effectiveness in a federated learning framework.


- **Adapting to New Classes:** FedTPG excels at adjusting to new classes within known datasets.


- **Adapting to New Datasets:** It shows remarkable generalization skills across various datasets, including different versions of ImageNet.


- **Exploratory Studies:** These confirm FedTPG's resilience against factors like class distribution among clients and the number of training examples.


###


Wrapping Up


FedTPG marks a significant advancement in tailoring vision-language models for federated learning. By smartly employing text-driven prompt generation, it not only improves model generalization across unseen classes and datasets but also redefines federated prompt learning methodologies.


Its success points to the promising integration of textual context into prompt generation, setting the stage for future exploration and application in federated learning settings.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
