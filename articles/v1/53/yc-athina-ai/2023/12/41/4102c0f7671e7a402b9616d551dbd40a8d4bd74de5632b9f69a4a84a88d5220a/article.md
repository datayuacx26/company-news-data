---
schema_version: "1.0.0"
document_id: "4102c0f7671e7a402b9616d551dbd40a8d4bd74de5632b9f69a4a84a88d5220a"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/ultra-dp-unifying-graph-pre-training-with-multi-task-graph-dual-prompt"
published_at: "2023-12-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:20.635269+00:00"
content_hash: "sha256:6d021d73ed2bf28ef5f1c2dacf794e191bd751eb6412b603122e7b14d65c9392"
---

# ULTRA-DP: Unifying Graph Pre-training with Multi-task Graph Dual Prompt

Do not index


Original Paper


[https://arxiv.org/abs/2310.14845](https://arxiv.org/abs/2310.14845)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2310.14845](https://arxiv.org/abs/2310.14845)


**By:**[Mouxiang Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20M) ,[Zemin Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20Z) ,[Chenghao Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20C) ,[Jundong Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20J) ,[Qiheng Mao](https://arxiv.org/search/cs?searchtype=author&query=Mao%2C%20Q) ,[Jianling Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20J)


**Abstract:**


> Recent research has demonstrated the efficacy of pre-training graph neural networks (GNNs) to capture the transferable graph semantics and enhance the performance of various downstream tasks. However, the semantic knowledge learned from pretext tasks might be unrelated to the downstream task, leading to a semantic gap that limits the application of graph pre-training. To reduce this gap, traditional approaches propose hybrid pre-training to combine various pretext tasks together in a multi-task learning fashion and learn multi-grained knowledge, which, however, cannot distinguish tasks and results in some transferable task-specific knowledge distortion by each other. Moreover, most GNNs cannot distinguish nodes located in different parts of the graph, making them fail to learn position-specific knowledge and lead to suboptimal performance. In this work, inspired by the prompt-based tuning in natural language processing, we propose a unified framework for graph hybrid pre-training which injects the task identification and position identification into GNNs through a prompt mechanism, namely multi-task graph dual prompt (ULTRA-DP). Based on this framework, we propose a prompt-based transferability test to find the most relevant pretext task in order to reduce the semantic gap. To implement the hybrid pre-training tasks, beyond the classical edge prediction task (node-node level), we further propose a novel pre-training paradigm based on a group of k-nearest neighbors (node-group level). The combination of them across different scales is able to comprehensively express more structural semantics and derive richer multi-grained knowledge. Extensive experiments show that our proposed ULTRA-DP can significantly enhance the performance of hybrid pre-training methods and show the generalizability to other pre-training tasks and backbone architectures.


---


###


Summary Notes


####


Bridging the Semantic Gap in GNN Pre-training with ULTRA-DP


Graph Neural Networks (GNNs) have become crucial for analyzing graph-structured data, ranging from social networks to molecular structures.


A major hurdle in their deployment is the need for vast amounts of labeled data for training.


Pre-training GNNs on unlabeled data has emerged as a promising workaround, yet often suffers from a semantic gap between pre-training tasks and downstream tasks, affecting performance.


The ULTRA-DP framework aims to address this issue by enhancing adaptability and effectiveness in GNNs. Let’s explore how ULTRA-DP is set to reshape GNN pre-training.


####


Understanding the ULTRA-DP Framework


ULTRA-DP introduces a unique dual-prompt mechanism for each node, integrating task and position embeddings, making GNNs more task-aware and sensitive to each node's structural context.


Key Components:


- **Dual Prompt Mechanism** : Combines task and position embeddings for each node, aiding in understanding both its role and location within the graph.


- **Task Embedding** : Encodes pre-training task information, enabling GNNs to adjust learning to the specific task, enhancing task-specific performance.


- **Position Embedding** : Based on reachability from random walks, it helps the model recognize a node’s location, adding structural context to node understanding.


- **Prompt-based Transferability Test** : Evaluates how well pre-trained models transfer to downstream tasks, using task embeddings for model fine-tuning and selection based on validation data.


- **Hybrid Pre-training Tasks** : Mixes traditional and new pre-training tasks, like node-group level k-nearest neighbors (k-NN) similarity prediction, to capture diverse structural insights, improving model performance.


####


Experiments and Results


ULTRA-DP was tested on various benchmark datasets and GNN architectures, demonstrating its effectiveness over conventional pre-training methods.


Highlights:


- **Datasets** : Tested on five datasets, including DBLP and Pubmed, with GNN architectures like GCN, GAT, and GraphSAGE.


- **Performance** : Consistently outperformed baselines, including non-pre-trained models and other pre-training approaches.


- **Ablation Studies** : Confirmed the essential role of task and position embeddings in ULTRA-DP’s success.


- **Generalizability** : Showed strong performance across different tasks and GNN types, proving its flexibility and efficiency.


####


Future Directions


The introduction of ULTRA-DP paves the way for further innovation in GNN pre-training. Future work could explore more varied pre-training tasks and delve deeper into prompt-based methods in graph learning, potentially enhancing model adaptability and performance even further.


####


Conclusion: Revolutionizing GNN Pre-training


ULTRA-DP signifies a breakthrough in overcoming the semantic gap in GNN pre-training. By leveraging dual prompts that cover both task-specific and position-specific insights, ULTRA-DP not only boosts GNN performance across numerous tasks but also sets the stage for more sophisticated and efficient graph model training strategies.


This advancement hints at the vast potential of innovative approaches to fully exploit unlabeled data, promising exciting developments for GNN applications in various fields.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
