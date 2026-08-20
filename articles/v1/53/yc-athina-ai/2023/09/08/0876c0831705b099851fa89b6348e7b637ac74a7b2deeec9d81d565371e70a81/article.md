---
schema_version: "1.0.0"
document_id: "0876c0831705b099851fa89b6348e7b637ac74a7b2deeec9d81d565371e70a81"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompt-based-node-feature-extractor-for-few-shot-learning-on-text-attributed-graphs"
published_at: "2023-09-06T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:8a57f8b1a3b524107dabf4be796f3a394a83f24a1fa040a5045cf56781e831e6"
---

# Prompt-based Node Feature Extractor for Few-shot Learning on Text-Attributed Graphs

Do not index


Original Paper


[https://arxiv.org/abs/2309.02848](https://arxiv.org/abs/2309.02848)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2309.02848](https://arxiv.org/abs/2309.02848)


**By:**[Xuanwen Huang](https://arxiv.org/search/cs?searchtype=author&query=Huang%2C%20X) ,[Kaiqiao Han](https://arxiv.org/search/cs?searchtype=author&query=Han%2C%20K) ,[Dezheng Bao](https://arxiv.org/search/cs?searchtype=author&query=Bao%2C%20D) ,[Quanjin Tao](https://arxiv.org/search/cs?searchtype=author&query=Tao%2C%20Q) ,[Zhisheng Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20Z) ,[Yang Yang](https://arxiv.org/search/cs?searchtype=author&query=Yang%2C%20Y) ,[Qi Zhu](https://arxiv.org/search/cs?searchtype=author&query=Zhu%2C%20Q)


**Abstract:**


> Text-attributed Graphs (TAGs) are commonly found in the real world, such as social networks and citation networks, and consist of nodes represented by textual descriptions. Currently, mainstream machine learning methods on TAGs involve a two-stage modeling approach: (1) unsupervised node feature extraction with pre-trained language models (PLMs); and (2) supervised learning using Graph Neural Networks (GNNs). However, we observe that these representations, which have undergone large-scale pre-training, do not significantly improve performance with a limited amount of training samples. The main issue is that existing methods have not effectively integrated information from the graph and downstream tasks simultaneously. In this paper, we propose a novel framework called G-Prompt, which combines a graph adapter and task-specific prompts to extract node features. First, G-Prompt introduces a learnable GNN layer (\\emph{i.e.,} adaptor) at the end of PLMs, which is fine-tuned to better capture the masked tokens considering graph neighborhood information. After the adapter is trained, G-Prompt incorporates task-specific prompts to obtain \\emph{interpretable} node representations for the downstream task. Our experiment results demonstrate that our proposed method outperforms current state-of-the-art (SOTA) methods on few-shot node classification. More importantly, in zero-shot settings, the G-Prompt embeddings can not only provide better task interpretability than vanilla PLMs but also achieve comparable performance with fully-supervised baselines.


---


###


Summary Notes


####


Simplifying G-Prompt for Enhanced Learning on Text-Attributed Graphs


In the field of artificial intelligence (AI), merging text data with graph structures creates what we call Text-Attributed Graphs (TAGs). These are widely used in social networks, citation networks, and knowledge graphs.


Despite their potential, blending textual content with graph structures efficiently has been a tough challenge. This is where G-Prompt comes into play, offering a novel solution to enhance learning from TAGs, especially in situations where data samples are limited.


####


Understanding Text-Attributed Graphs (TAGs)


TAGs are complex data types that combine text information with the nodes of a graph. This combination makes extracting features a challenging task because it involves understanding both the text's meaning and the graph's structural relationships.


####


Challenges with TAGs


Integrating TAGs into machine learning models faces several obstacles:


- **Computational Demand:** Training models that understand both text and graph structures requires a lot of computational power.


- **Catastrophic Forgetting:** Models often struggle to learn new tasks without forgetting what they previously learned.


- **Incomplete Use of Graph Structure:** Many current methods do not fully use the graph structure, limiting the effectiveness of the learning process.


####


Introducing G-Prompt


G-Prompt addresses these issues through:


- **Graph Adapter:** A special layer added to models to help them understand graph structures better.


- **Task-specific Prompts:** Custom prompts that refine the process of extracting node features, making the model more adaptable to different tasks.


- **Self-Supervised Learning:** A training approach that prepares the graph adapter to work well with the model, focusing on tasks related to the graph's structure.


####


G-Prompt in Action


G-Prompt has been tested on real-world datasets focusing on few-shot and zero-shot learning, showcasing improved performance over current methods by about 4.1% in few-shot learning scenarios.


This demonstrates G-Prompt's ability to adapt and perform well even when trained with limited data.


The implementation of G-Prompt involves two key steps:


1. **Training the Graph Adapter:** This is done using a task that involves predicting masked parts of the graph, helping the model to better understand the graph structure.


1. **Applying Task-specific Prompts:** This step customizes the feature extraction process to suit specific tasks, using the previously trained graph-aware model.


####


Looking Forward


G-Prompt is a big step forward in using TAGs for AI applications, especially for tasks with limited data. It offers a scalable, interpretable, and adaptable approach that reduces the need for extensive retraining for different tasks.


Future research will focus on scaling G-Prompt for larger graphs and exploring various graph adapters to further improve performance and interpretability.


This opens up new possibilities for using TAGs in enterprise applications, marking a significant advancement in AI's capability to learn from complex data structures.


In summary, G-Prompt is paving the way for more effective and efficient use of text-attributed graphs in AI, particularly in challenging learning scenarios like few-shot and zero-shot learning.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
