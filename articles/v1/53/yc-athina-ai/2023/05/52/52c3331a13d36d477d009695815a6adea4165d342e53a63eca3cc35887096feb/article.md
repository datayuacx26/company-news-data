---
schema_version: "1.0.0"
document_id: "52c3331a13d36d477d009695815a6adea4165d342e53a63eca3cc35887096feb"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/graph-toolformer-to-empower-llms-with-graph-reasoning-ability-via-prompt-augmented-by-chatgpt"
published_at: "2023-05-11T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:14ff9dbcdfe792de7de14c1146845651516fb68ce0d6b4d1f5fc816fc8584487"
---

# Graph-ToolFormer: To Empower LLMs with Graph Reasoning Ability via Prompt Augmented by ChatGPT

Do not index


Original Paper


[https://arxiv.org/abs/2304.11116](https://arxiv.org/abs/2304.11116)


Blog URL


[blog.athina.ai /graph-t...-chatgpt](https://blog.athina.ai/graph-toolformer-to-empower-llms-with-graph-reasoning-ability-via-prompt-augmented-by-chatgpt)


**Original Paper:**[https://arxiv.org/abs/2304.11116](https://arxiv.org/abs/2304.11116)


**By:**[Jiawei Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20J)


**Abstract:**


> In this paper, we aim to develop a large language model (LLM) with the reasoning ability on complex graph data. Currently, LLMs have achieved very impressive performance on various natural language learning tasks, extensions of which have also been applied to study the vision tasks with multi-modal data. However, when it comes to the graph learning tasks, existing LLMs present very serious flaws due to their several inherited weaknesses in performing {multi-step logic reasoning}, {precise mathematical calculation} and {perception about the spatial and temporal factors}.
>
>
> To address such challenges, in this paper, we will investigate the principles, methodologies and algorithms to empower existing LLMs with graph reasoning ability, which will have tremendous impacts on the current research of both LLMs and graph learning. Inspired by the latest ChatGPT and Toolformer models, we propose the Graph-ToolFormer (Graph Reasoning oriented Toolformer) framework to teach LLMs themselves with prompts augmented by ChatGPT to use external graph reasoning API tools. Specifically, we will investigate to teach Graph-ToolFormer to handle various graph data reasoning tasks in this paper, including both (1) very basic graph data loading and graph property reasoning tasks, ranging from simple graph order and size to the graph diameter and periphery, and (2) more advanced reasoning tasks on real-world graph data, such as bibliographic networks, protein molecules, sequential recommender systems, social networks and knowledge graphs.


---


###


Summary Notes


####


Unveiling Graph-ToolFormer - A Leap in Enhancing Language Models with Graph Reasoning


The domain of artificial intelligence is witnessing rapid advancements, especially with **Large Language Models (LLMs)** such as GPT and BERT transforming natural language processing and computer vision.


However, their proficiency in managing graph-based data, crucial for understanding intricate relationships, remains limited. Addressing this, the **Graph-ToolFormer** framework emerges as a groundbreaking solution to seamlessly integrate graph reasoning with LLMs.


####


Introduction


Graph-based data, found in social and biological networks, offers profound insights, yet its integration with LLMs has been challenging.


Enter **Graph-ToolFormer** : a game-changing framework designed to empower LLMs with the capability to perform graph reasoning tasks efficiently.


####


Background


Previous attempts to blend **Graph Neural Networks (GNNs)** with LLMs have shown promise but lacked a comprehensive approach for effective graph reasoning. Graph-ToolFormer fills this gap, offering a holistic framework.


####


The Graph-ToolFormer Framework


Graph-ToolFormer enriches LLMs by embedding hand-crafted prompts and instructions to generate API calls for external graph reasoning tools, enhancing their ability to handle various graph reasoning tasks, from basic to complex.


####


Implementation Highlights


- **Crafting Prompts** : Creating specific prompts with instructions and API call placeholders.


- **Processing** : Using ChatGPT to refine these prompts with suitable API function calls.


- **Training LLMs** : Training models like GPT-J and LLaMA with these augmented prompts to interact with external graph reasoning tools.


####


Experiments


Extensive testing across domains like social networks and biological graphs confirmed that Graph-ToolFormer significantly boosts LLMs' graph reasoning capabilities, maintaining their linguistic efficiency.


####


Future Directions


Graph-ToolFormer sets the stage for further enhancements in LLMs' graph reasoning abilities and efficiency. Integrating it with diverse neural network models could herald even more breakthroughs.


####


Contributions


Graph-ToolFormer's introduction is a significant milestone in AI, offering:


- A unique framework for augmenting LLMs with graph reasoning tasks.


- Applicability in various domains and graph types.


- Thorough validation of its effectiveness through experiments.


####


Visual Insights


**Figure 1** illustrates Graph-ToolFormer's capacity to handle multiple graph reasoning tasks, showcasing how LLM responses are enhanced with API calls to external tools, providing a visual understanding of its capabilities.


####


Open Resources


All source codes, data, and pre-trained models are openly available on GitHub, encouraging further research and development in this innovative field.


Graph-ToolFormer represents not just a step, but a leap towards harnessing the full potential of LLMs in processing graph-based data, opening new avenues for complex reasoning tasks in AI.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
