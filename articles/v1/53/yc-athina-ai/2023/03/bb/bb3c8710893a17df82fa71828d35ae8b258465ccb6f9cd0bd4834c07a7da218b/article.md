---
schema_version: "1.0.0"
document_id: "bb3c8710893a17df82fa71828d35ae8b258465ccb6f9cd0bd4834c07a7da218b"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompt-generate-then-cache-cascade-of-foundation-models-makes-strong-few-shot-learners"
published_at: "2023-03-03T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:58c722f6d1e69bcc154e0335124b68f5cf77bbb41e9ccd6149673798fdf408c4"
---

# Prompt, Generate, then Cache: Cascade of Foundation Models makes Strong Few-shot Learners

Do not index


Original Paper


[https://arxiv.org/abs/2303.02151](https://arxiv.org/abs/2303.02151)


Blog URL


[blog.athina.ai /prompt-...learners](https://blog.athina.ai/prompt-generate-then-cache-cascade-of-foundation-models-makes-strong-few-shot-learners)


**Original Paper:**[https://arxiv.org/abs/2303.02151](https://arxiv.org/abs/2303.02151)


**By:**[Renrui Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20R) ,[Xiangfei Hu](https://arxiv.org/search/cs?searchtype=author&query=Hu%2C%20X) ,[Bohao Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20B) ,[Siyuan Huang](https://arxiv.org/search/cs?searchtype=author&query=Huang%2C%20S) ,[Hanqiu Deng](https://arxiv.org/search/cs?searchtype=author&query=Deng%2C%20H) ,[Hongsheng Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20H) ,[Yu Qiao](https://arxiv.org/search/cs?searchtype=author&query=Qiao%2C%20Y) ,[Peng Gao](https://arxiv.org/search/cs?searchtype=author&query=Gao%2C%20P)


**Abstract:**


> Visual recognition in low-data regimes requires deep neural networks to learn generalized representations from limited training samples. Recently, CLIP-based methods have shown promising few-shot performance benefited from the contrastive language-image pre-training. We then question, if the more diverse pre-training knowledge can be cascaded to further assist few-shot representation learning. In this paper, we propose CaFo, a Cascade of Foundation models that incorporates diverse prior knowledge of various pre-training paradigms for better few-shot learning. Our CaFo incorporates CLIP's language-contrastive knowledge, DINO's vision-contrastive knowledge, DALL-E's vision-generative knowledge, and GPT-3's language-generative knowledge. Specifically, CaFo works by 'Prompt, Generate, then Cache'. Firstly, we leverage GPT-3 to produce textual inputs for prompting CLIP with rich downstream linguistic semantics. Then, we generate synthetic images via DALL-E to expand the few-shot training data without any manpower. At last, we introduce a learnable cache model to adaptively blend the predictions from CLIP and DINO. By such collaboration, CaFo can fully unleash the potential of different pre-training methods and unify them to perform state-of-the-art for few-shot classification. Code is available at
>
>
> [this https URL](https://github.com/ZrrSkywalker/CaFo)


---


###


Summary Notes


##


Enhancing Few-shot Learning with Cascades of Foundation Models (CaFo)


###


Introduction


Few-shot learning is a crucial challenge in AI, where the goal is to develop models that can accurately learn from limited data.


Traditional methods have made progress, but often struggle with complex real-world scenarios. Enter the "CaFo" framework, a groundbreaking approach that uses a series of pre-trained models to dramatically improve few-shot learning.


###


The Challenges of Few-shot Learning


The essence of few-shot learning is to teach models to accurately classify or understand data with only a few examples.


Although techniques like meta-learning and metric learning have been developed to tackle this, they frequently require elaborate setup and may not work well across different tasks.


###


Background Work


Lately, the focus has shifted towards using large pre-trained models, which are trained on vast datasets and can offer a rich understanding for few-shot learning.


Specifically, combining language and visual models has shown great promise in enhancing the models' understanding of both text and images in few-shot tasks.


###


Introducing the CaFo Framework


The CaFo framework is a breakthrough in few-shot learning, uniquely combining four types of pre-trained models to support the learning process:


- **CLIP** : Provides understanding of textual and visual data relationships.


- **DINO** : Enhances visual recognition.


- **DALL-E** : Creates synthetic images to expand training data.


- **GPT-3** : Generates linguistic prompts to guide the learning focus.


These models are integrated through a three-step process called Prompt, Generate, then Cache. GPT-3 starts by creating prompts that help CLIP understand the task.


DALL-E follows by generating synthetic images for more diverse examples. Lastly, a learnable cache model uses predictions from CLIP and DINO, refining the output with the few-shot examples.


###


Performance and Results


Testing CaFo across 11 datasets proved its outstanding performance in few-shot learning, especially in settings with very limited data.


It consistently surpassed existing methods, showcasing its adaptability and learning capabilities from minimal examples.


###


Conclusion and Key Takeaways


CaFo stands out by cleverly using diverse pre-trained models to address few-shot learning's challenges. Its unique cache model efficiently combines predictions for better accuracy. The framework is highly adaptable, promising for applications facing data scarcity.


####


Key Contributions


- **Pre-training Knowledge Integration** : Combining CLIP, DINO, DALL-E, and GPT-3 into a unified learning approach.


- **Innovative Cache Model** : An adaptive method to enhance prediction accuracy in few-shot scenarios.


- **Thorough Validation** : Proven effectiveness of CaFo across multiple datasets.


###


Looking Forward


Future enhancements could include integrating additional pre-trained models and expanding to more domains, potentially making CaFo even more versatile and powerful for few-shot learning.


The CaFo framework offers AI Engineers in enterprises a new strategy for overcoming few-shot learning challenges, leveraging the power of foundation models to develop more versatile, efficient, and adaptable AI systems.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
