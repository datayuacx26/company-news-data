---
schema_version: "1.0.0"
document_id: "f0de0160365c8824ca47b9e957e9f970db11c2827f9a60099c1e6014c6c226a0"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/rethinking-visual-prompt-learning-as-masked-visual-token-modeling"
published_at: "2023-12-15T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:20.635269+00:00"
content_hash: "sha256:a11ddc52ef29fd32891c62e304bac289c822c834038967e7c0b5c27648096fef"
---

# Rethinking Visual Prompt Learning as Masked Visual Token Modeling

Do not index


Original Paper


[https://arxiv.org/abs/2303.04998](https://arxiv.org/abs/2303.04998)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2303.04998](https://arxiv.org/abs/2303.04998)


**By:**[Ning Liao](https://arxiv.org/search/cs?searchtype=author&query=Liao%2C%20N) ,[Bowen Shi](https://arxiv.org/search/cs?searchtype=author&query=Shi%2C%20B) ,[Xiaopeng Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20X) ,[Min Cao](https://arxiv.org/search/cs?searchtype=author&query=Cao%2C%20M) ,[Junchi Yan](https://arxiv.org/search/cs?searchtype=author&query=Yan%2C%20J) ,[Qi Tian](https://arxiv.org/search/cs?searchtype=author&query=Tian%2C%20Q)


**Abstract:**


> Prompt learning has achieved great success in efficiently exploiting large-scale pre-trained models in natural language processing (NLP). It reformulates the downstream tasks as the generative pre-training ones to achieve consistency, thus improving the performance stably. However, when transferring it to the vision area, current visual prompt learning methods are almost designed on discriminative pre-trained models, and there is also a lack of careful design to unify the forms of pre-training and downstream tasks. To explore prompt learning on the generative pre-trained visual model, as well as keeping the task consistency, we propose Visual Prompt learning as masked visual Token Modeling (VPTM) to transform the downstream visual classification into the pre-trained masked visual token prediction. In addition, we develop the prototypical verbalizer for mapping the predicted visual token with implicit semantics to explicit downstream labels. To our best knowledge, VPTM is the first visual prompt method on the generative pre-trained visual model, which achieves consistency between pre-training and downstream visual classification by task reformulation. Experiments show that VPTM outperforms other visual prompt methods and achieves excellent efficiency. Moreover, the task consistency of VPTM contributes to the robustness against prompt location, prompt length and prototype dimension, and could be deployed uniformly.


---


###


Summary Notes


####


Simplifying Visual Prompt Learning for AI's Next Leap


The advancement of AI has been significantly shaped by large-scale pre-trained models, especially in areas like computer vision and natural language processing (NLP).


However, adapting these models to new tasks can be inefficient and resource-intensive.


This is where the innovative approach of prompt learning comes in, initially successful in NLP and now making waves in visual tasks through a method called Visual Prompt Learning as Masked Visual Token Modeling (VPTM).


This blog post explores how VPTM is revolutionizing the adaptation of pre-trained models for visual tasks.


###


Challenges with Pre-Trained Models


Pre-trained models are crucial for modern AI, excelling in various tasks from image recognition to language analysis.


The main challenge with these models is their adaptation to new tasks, which can be resource-heavy and inconsistent due to differences in task formulations during the pre-training and fine-tuning stages.


###


What is Visual Prompt Learning?


Prompt learning offers a solution by guiding models to apply their learned knowledge to new tasks with minimal retraining.


This concept is now being applied to the visual domain through Visual Prompt Learning as Masked Visual Token Modeling (VPTM).


####


Key Concepts of VPTM


VPTM aims to align pre-training and downstream tasks by treating them both as generative tasks. This is done by:


- **Task Reformulation** : Making the downstream task compatible with the generative pre-training phase.


- **Token Insertion** : Adding a masked token and learnable prompt tokens to the input.


- **Leveraging Pre-Trained Models** : Using models (e.g., BEITv2) to predict the replacement for the masked token.


####


The Role of Prototypical Verbalizer


A vital element of VPTM is the prototypical verbalizer, which connects predicted visual tokens to specific labels. It works by:


- **Mapping Tokens to Labels** : Using prototype vectors for each class and calculating similarity to determine the class.


###


VPTM's Effectiveness


Testing on various datasets has shown that VPTM not only achieves higher accuracy but also demonstrates better generalization than existing methods.


Its resilience to variations in prompt location, length, and prototype vector dimensions highlights its adaptability and efficiency.


###


The Future with Visual Prompt Learning


VPTM introduces a promising approach to visual task adaptation, achieving consistency with pre-training tasks and outperforming in both performance and generalization.


This method could greatly reduce the computational costs of adapting pre-trained models to new tasks, marking a significant progression in AI research.


###


Conclusion


Adapting prompt learning for visual tasks through VPTM marks a major advancement in AI.


By aligning downstream tasks with the generative nature of pre-training, VPTM provides an efficient, robust, and adaptable method that enhances AI systems' performance in visual tasks.


As this approach is further explored and refined, the possibilities for prompt learning in visual domains seem limitless, promising a future where AI can adapt to new tasks with minimal effort.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
