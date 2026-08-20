---
schema_version: "1.0.0"
document_id: "97ee8a06d95a0d9d4429aa199b901bdcccec4b33f4a2eb6ce44453674fe2a336"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/hard-prompts-made-easy-gradient-based-discrete-optimization-for-prompt-tuning-and-discovery"
published_at: "2023-06-01T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.924529+00:00"
content_hash: "sha256:478983356066ff1519e4e61fa0a933ab23dab30d345f6f75f853c00c3e29ed1e"
---

# Hard Prompts Made Easy: Gradient-Based Discrete Optimization for Prompt Tuning and Discovery

Do not index


Original Paper


[https://arxiv.org/abs/2302.03668](https://arxiv.org/abs/2302.03668)


Blog URL


[blog.athina.ai /hard-pr...iscovery](https://blog.athina.ai/hard-prompts-made-easy-gradient-based-discrete-optimization-for-prompt-tuning-and-discovery)


**Original Paper:**[https://arxiv.org/abs/2302.03668](https://arxiv.org/abs/2302.03668)


**By:**[Yuxin Wen](https://arxiv.org/search/cs?searchtype=author&query=Wen%2C%20Y) ,[Neel Jain](https://arxiv.org/search/cs?searchtype=author&query=Jain%2C%20N) ,[John Kirchenbauer](https://arxiv.org/search/cs?searchtype=author&query=Kirchenbauer%2C%20J) ,[Micah Goldblum](https://arxiv.org/search/cs?searchtype=author&query=Goldblum%2C%20M) ,[Jonas Geiping](https://arxiv.org/search/cs?searchtype=author&query=Geiping%2C%20J) ,[Tom Goldstein](https://arxiv.org/search/cs?searchtype=author&query=Goldstein%2C%20T)


**Abstract:**


> The strength of modern generative models lies in their ability to be controlled through text-based prompts. Typical "hard" prompts are made from interpretable words and tokens, and must be hand-crafted by humans. There are also "soft" prompts, which consist of continuous feature vectors. These can be discovered using powerful optimization methods, but they cannot be easily interpreted, re-used across models, or plugged into a text-based interface.
>
>
> We describe an approach to robustly optimize hard text prompts through efficient gradient-based optimization. Our approach automatically generates hard text-based prompts for both text-to-image and text-to-text applications. In the text-to-image setting, the method creates hard prompts for diffusion models, allowing API users to easily generate, discover, and mix and match image concepts without prior knowledge on how to prompt the model. In the text-to-text setting, we show that hard prompts can be automatically discovered that are effective in tuning LMs for classification.


---


###


Summary Notes


####


Hard Prompts Optimization for Generative Models: A Simplified Guide


In the fast-paced world of artificial intelligence (AI), prompt engineering is a key technique for guiding generative models to produce the desired outputs.


This is particularly important for AI Engineers in enterprise environments where accurate and interpretable model outputs are crucial.


Today, we explore an innovative approach to optimizing text-based prompts, focusing on "hard prompts" - human-readable tokens that have presented challenges in engineering efficiency and effectiveness.


###


Introduction to Prompt Engineering


Prompt engineering is an intriguing aspect of AI, especially in working with generative models. It involves creating prompts, or instructions, to direct these models to generate specific outputs. While optimizing "soft prompts" (continuous-valued embeddings) has been relatively straightforward due to their numerical nature, "hard prompts" (human-readable tokens) offer better interpretability and model transferability but are harder to optimize.


###


Challenges with Hard Prompts


Hard prompts are tricky to optimize using traditional methods because of their discrete nature. This often leads to a process that requires a lot of guesswork, despite their advantages in interpretability and reusability across models.


###


Optimizing Hard Prompts: The PEZ Algorithm


The PEZ algorithm is a major breakthrough in prompt engineering. It's a gradient-based discrete optimization method designed to make hard prompt engineering more efficient and effective. Here's a brief overview:


- **Continuous Embeddings Maintenance** : It keeps continuous embeddings to represent potential hard prompts.


- **Projection to Discrete Tokens** : These embeddings are periodically turned into discrete tokens to calculate gradients.


- **Embeddings Update** : With these gradients, the embeddings are refined, improving the hard prompts.


####


Benefits of the PEZ Algorithm


- **Interpretability** : Produces human-readable prompts, making it easier for engineers to adjust and improve them.


- **Reusability** : Allows prompts to be used across different models, increasing their utility.


- **Efficiency** : Makes the process more systematic and less dependent on trial-and-error.


###


Experiments and Results


The PEZ algorithm has proven its effectiveness in experiments using datasets like LAION, MS COCO, and Celeb-A, outperforming baselines like FluentPrompt and AutoPrompt. It has shown great performance in tasks like text-to-image generation and language classification.


###


Importance for AI Engineers


For AI Engineers, particularly in enterprise settings, the ability to efficiently generate interpretable and reusable prompts is invaluable. It streamlines the development process, enhances model adaptability, and leads to more precise AI-generated content.


###


Conclusion: A New Era in Prompt Engineering


The PEZ algorithm represents a significant advancement in optimizing hard prompts, making it easier to combine the optimization ease of soft prompts with the desirability of hard prompts. This opens new possibilities for AI applications in image generation and language tasks.


###


Starting with the PEZ Algorithm


For those interested, the PEZ algorithm is available on GitHub. It offers AI Engineers a chance to improve their models and contribute to the evolution of prompt engineering.


Optimizing hard prompts with techniques like the PEZ algorithm marks a major progress in AI development, emphasizing the importance of efficient, interpretable, and adaptable prompt engineering.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
