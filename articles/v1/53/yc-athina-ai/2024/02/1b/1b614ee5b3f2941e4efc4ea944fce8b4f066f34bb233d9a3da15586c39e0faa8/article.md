---
schema_version: "1.0.0"
document_id: "1b614ee5b3f2941e4efc4ea944fce8b4f066f34bb233d9a3da15586c39e0faa8"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/entgpt-linking-generative-large-language-models-with-knowledge-bases"
published_at: "2024-02-09T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:1ff0700370c2d67e1a5aeac3d18551d16f3fbf3f19013b209d4c4d42656d968b"
---

# EntGPT: Linking Generative Large Language Models with Knowledge Bases

Do not index


Original Paper


[https://arxiv.org/abs/2402.06738](https://arxiv.org/abs/2402.06738)


Blog URL


[blog.athina.ai /entgpt-...ge-bases](https://blog.athina.ai/entgpt-linking-generative-large-language-models-with-knowledge-bases)


**Original Paper:**[https://arxiv.org/abs/2402.06738](https://arxiv.org/abs/2402.06738)


**By:**[Yifan Ding](https://arxiv.org/search/cs?searchtype=author&query=Ding%2C%20Y) ,[Amrit Poudel](https://arxiv.org/search/cs?searchtype=author&query=Poudel%2C%20A) ,[Qingkai Zeng](https://arxiv.org/search/cs?searchtype=author&query=Zeng%2C%20Q) ,[Tim Weninger](https://arxiv.org/search/cs?searchtype=author&query=Weninger%2C%20T) ,[Balaji Veeramani](https://arxiv.org/search/cs?searchtype=author&query=Veeramani%2C%20B) ,[Sanmitra Bhattacharya](https://arxiv.org/search/cs?searchtype=author&query=Bhattacharya%2C%20S)


**Abstract:**


> The ability of Large Language Models (LLMs) to generate factually correct output remains relatively unexplored due to the lack of fact-checking and knowledge grounding during training and inference. In this work, we aim to address this challenge through the Entity Disambiguation (ED) task. We first consider prompt engineering, and design a three-step hard-prompting method to probe LLMs' ED performance without supervised fine-tuning (SFT). Overall, the prompting method improves the micro-F_1 score of the original vanilla models by a large margin, on some cases up to 36% and higher, and obtains comparable performance across 10 datasets when compared to existing methods with SFT. We further improve the knowledge grounding ability through instruction tuning (IT) with similar prompts and responses. The instruction-tuned model not only achieves higher micro-F1 score performance as compared to several baseline methods on supervised entity disambiguation tasks with an average micro-F_1 improvement of 2.1% over the existing baseline models, but also obtains higher accuracy on six Question Answering (QA) tasks in the zero-shot setting. Our methodologies apply to both open- and closed-source LLMs.


---


###


Summary Notes


####


Enhancing AI with EntGPT for Better Accuracy and Reasoning


In today's fast-evolving AI landscape, Large Language Models (LLMs) are becoming increasingly adept at processing human language. However, they often fall short in terms of factual accuracy and logical reasoning, mostly because they rely on potentially outdated or incorrect text data. EntGPT emerges as a cutting-edge solution designed to connect LLMs with structured knowledge bases, significantly improving their output's factual correctness.


This post delves into EntGPT, focusing on two main strategies: EntGPT-Prompting (EntGPT-P) and EntGPT-Instruction Tuning (EntGPT-I), and their impact on the future of LLMs.


####


EntGPT-Prompting (EntGPT-P)


EntGPT-P employs a three-step hard prompting technique that bypasses supervised fine-tuning, notably enhancing model performance on Entity Disambiguation (ED) tasks essential for generating accurate and relevant content. The steps include:


- Creating a list of potential entities.


- Enhancing these entities with context-specific prompts.


- Choosing the best entity based on the model's response.


This method not only boosts accuracy but also significantly lowers the chances of the model producing incorrect or illogical information, marking a significant advancement in using LLMs for tasks requiring high factual accuracy and reasoning.


####


EntGPT-Instruction Tuning (EntGPT-I)


EntGPT-I builds on EntGPT-P's success, further improving the model's factual grounding through instruction tuning. This approach has led to top-notch performance on ED tasks and a substantial increase in accuracy on Question Answering (QA) tasks in a zero-shot scenario, surpassing other models in six different QA benchmarks. EntGPT-I's effectiveness stems from its detailed tuning process, which tightly integrates the model's outputs with factual knowledge bases, thus minimizing inaccuracies and enhancing overall accuracy.


####


Ablation Study Insights


An ablation study on these methodologies highlights the crucial nature of each step in the EntGPT-P and EntGPT-I frameworks.


Omitting steps like generating entity candidates or prompt augmentation resulted in lower performance, emphasizing the importance of these processes in the effectiveness of the EntGPT approaches.


####


Understanding Entity Disambiguation Errors


A case study on EntGPT-P's entity disambiguation errors revealed that most mistakes were actually reasonable, with instances where the model's predictions were even more accurate than the labeled ground truth.


This suggests that EntGPT-P can potentially exceed human reasoning in certain situations, offering new insights into improving LLMs and indicating that models like EntGPT-P could provide valuable contributions beyond conventional methods.


####


Looking Forward


The potential future developments for the EntGPT framework are extensive. One exciting direction is entity linking, which could further improve the model's capability in producing factually accurate content.


Additionally, refining the entity disambiguation process promises substantial enhancements in performance on QA tasks, a key indicator of a model's understanding and reasoning skills.


####


Conclusion


EntGPT marks a notable advancement in developing LLMs that produce not only linguistically coherent but also factually accurate and logically sound content. With techniques like EntGPT-Prompting and EntGPT-Instruction Tuning, this approach significantly minimizes inaccuracies while improving factual accuracy.


For AI Engineers in enterprise settings, the benefits are significant, paving the way for deploying more dependable, accurate, and context-aware AI applications. As AI continues to advance, methodologies like EntGPT will lead the charge, steering us towards an era where AI's comprehension of the world mirrors our own.
