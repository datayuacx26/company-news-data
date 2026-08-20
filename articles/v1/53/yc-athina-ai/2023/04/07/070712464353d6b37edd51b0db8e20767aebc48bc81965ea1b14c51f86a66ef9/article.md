---
schema_version: "1.0.0"
document_id: "070712464353d6b37edd51b0db8e20767aebc48bc81965ea1b14c51f86a66ef9"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/progressive-visual-prompt-learning-with-contrastive-feature-re-formation"
published_at: "2023-04-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:d6a416b93ad7c02871d68207c89fdc95dc7ddde14f813778e37e5bf638577ab4"
---

# Progressive Visual Prompt Learning with Contrastive Feature Re-formation

Do not index


Original Paper


[https://arxiv.org/abs/2304.08386](https://arxiv.org/abs/2304.08386)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2304.08386](https://arxiv.org/abs/2304.08386)


**By:**[Chen Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20C) ,[Haocheng Shen](https://arxiv.org/search/cs?searchtype=author&query=Shen%2C%20H) ,[Fengyuan Shi](https://arxiv.org/search/cs?searchtype=author&query=Shi%2C%20F) ,[Boheng Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20B) ,[Yixuan Liao](https://arxiv.org/search/cs?searchtype=author&query=Liao%2C%20Y) ,[Xiaoxin Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20X) ,[Limin Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20L)


**Abstract:**


> Prompt learning has been designed as an alternative to fine-tuning for adapting Vision-language (V-L) models to the downstream tasks. Previous works mainly focus on text prompt while visual prompt works are limited for V-L models. The existing visual prompt methods endure either mediocre performance or unstable training process, indicating the difficulty of visual prompt learning. In this paper, we propose a new Progressive Visual Prompt (ProVP) structure to strengthen the interactions among prompts of different layers. More importantly, our ProVP could effectively propagate the image embeddings to deep layers and behave partially similar to an instance adaptive prompt method. To alleviate generalization deterioration, we further propose a new contrastive feature re-formation, which prevents the serious deviation of the prompted visual feature from the fixed CLIP visual feature distribution. Combining both, our method (ProVP-Ref) is evaluated on 11 image benchmark datasets and achieves 7/11 state-of-theart results on both few-shot and base-to-novel settings. To the best of our knowledge, we are the first to demonstrate the superior performance of visual prompts in V-L models to previous prompt-based methods in downstream tasks. Meanwhile, it implies that our ProVP-Ref shows the best capability to adapt and to generalize.


---


###


Summary Notes


##


Simplifying Visual Prompt Learning in AI with ProVP


Vision-Language (V-L) models such as CLIP have been a game-changer in the AI field, enabling systems to better understand the relationship between images and text. These models are widely used for things like recommending content or automatically adding tags to images.


However, updating these models for new tasks while maintaining their original strengths remains a challenge. Traditional methods, like fine-tuning the whole model, often lead to overfitting and loss of general knowledge. This is where **Progressive Visual Prompt Learning** comes into play.


###


The Challenge at Hand


Adapting V-L models to new tasks is tricky. The usual method of retraining the model on a new dataset can cause overfitting, particularly with smaller datasets, and can erase the valuable, generalized knowledge acquired during the initial training.


Prompt learning offers a promising solution by adjusting a small set of parameters, known as "prompts", while leaving the rest of the model unchanged.


This technique has mainly been applied to the textual aspects of V-L models, leaving the visual side less explored and prone to issues such as unstable training and suboptimal performance.


###


Progressive Visual Prompt Learning: A New Approach


**Progressive Visual Prompt (ProVP) learning** introduces innovative solutions to overcome the shortcomings of previous visual prompt learning methods:


- **Progressive Connection Strategy:** ProVP connects prompts across different layers progressively, rather than learning them separately for each layer. This enhances prompt interaction, leads to better learning, and ensures training stability.


- **Contrastive Feature Re-formation:** This technique helps retain the model's ability to generalize by ensuring that the modified visual features (via prompts) remain aligned with the original pre-trained model's feature distribution, preserving its valuable pre-learned knowledge.


###


Results and Impacts


The effectiveness of ProVP has been validated on several benchmark datasets, including ImageNet, Caltech101, and OxfordPets, with notable outcomes:


- Outperformed other methods on 7 out of 11 benchmark datasets.


- Significantly enhanced the model's generalization ability, crucial for applying AI to real-world tasks.


- Demonstrated the value of the progressive prompt strategy and Contrastive Feature Re-formation through detailed studies.


###


Looking Forward


ProVP's development marks a significant step forward in making V-L models more adaptable and efficient for specific tasks by improving upon visual prompt learning's stability and performance issues.


It opens the door to potentially optimizing prompts across both visual and textual components, which could further elevate the capabilities of V-L models.


In summary, ProVP offers a powerful framework for boosting the performance and versatility of V-L models, heralding a new era of possibilities for AI applications across various sectors.


As research in this area progresses, we can anticipate even more innovative solutions that will continue to broaden the horizons of AI technology.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
