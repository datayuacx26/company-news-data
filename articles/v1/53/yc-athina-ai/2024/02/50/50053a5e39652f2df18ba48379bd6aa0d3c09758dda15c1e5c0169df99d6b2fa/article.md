---
schema_version: "1.0.0"
document_id: "50053a5e39652f2df18ba48379bd6aa0d3c09758dda15c1e5c0169df99d6b2fa"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/dept-decomposed-prompt-tuning-for-parameter-efficient-fine-tuning"
published_at: "2024-02-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:508434b2afdb4a2e06a93b034cb14ac5f70fddb55c2b240753e86871c75daf3d"
---

# DePT: Decomposed Prompt Tuning for Parameter-Efficient Fine-tuning

Do not index


Original Paper


[https://arxiv.org/abs/2309.05173](https://arxiv.org/abs/2309.05173)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2309.05173](https://arxiv.org/abs/2309.05173)


**By:**[Zhengxiang Shi](https://arxiv.org/search/cs?searchtype=author&query=Shi%2C%20Z) ,[Aldo Lipani](https://arxiv.org/search/cs?searchtype=author&query=Lipani%2C%20A)


**Abstract:**


> Prompt tuning (PT), where a small amount of trainable soft (continuous) prompt vectors is affixed to the input of language models (LM), has shown promising results across various tasks and models for parameter-efficient fine-tuning (PEFT). PT stands out from other PEFT approaches because it maintains competitive performance with fewer trainable parameters and does not drastically scale up its parameters as the model size expands. However, PT introduces additional soft prompt tokens, leading to longer input sequences, which significantly impacts training and inference time and memory usage due to the Transformer's quadratic complexity. Particularly concerning for Large Language Models (LLMs) that face heavy daily querying. To address this issue, we propose Decomposed Prompt Tuning (DePT), which decomposes the soft prompt into a shorter soft prompt and a pair of low-rank matrices that are then optimised with two different learning rates. This allows DePT to achieve better performance while saving substantial memory and time costs compared to vanilla PT and its variants, without changing trainable parameter sizes. Through extensive experiments on 23 natural language processing (NLP) and vision-language (VL) tasks, we demonstrate that DePT outperforms state-of-the-art PEFT approaches, including the full fine-tuning baseline, in some scenarios. Additionally, we empirically show that DEPT grows more efficient as the model size increases. Our further study reveals that DePT integrates seamlessly with parameter-efficient transfer learning in the few-shot learning setting and highlights its adaptability to various model architectures and sizes.


---


###


Summary Notes


####


Introducing Decomposed Prompt Tuning for Efficient AI Fine-Tuning (DePT)


In the fast-paced world of artificial intelligence (AI), particularly in areas like natural language processing (NLP) and tasks combining vision and language, fine-tuning large language models (LLMs) in a resource-efficient way is crucial.


Traditional methods are effective but often require a lot of computational power, limiting their wider use. Parameter-efficient fine-tuning (PEFT) methods, such as prompt tuning (PT), have been developed to update only a small part of the model's parameters, making the process more manageable.


However, PT has its own set of challenges, including slow progress, sensitivity to how it's initially set up, and the need for longer input sequences that increase training and inference times.


Enter Decomposed Prompt Tuning (DePT), a new technique that modifies PT for better performance with less computational effort.


####


Introduction


Fine-tuning LLMs for specific tasks is essential for modern AI applications but can be very demanding in terms of resources, especially with the growing size of LLMs.


PEFT methods like PT offer a solution by fine-tuning only a small fraction of the model's parameters. Despite its advantages, PT's method of extending input sequences poses significant challenges.


DePT addresses these issues by enhancing the efficiency and effectiveness of PT.


####


How DePT Works


- **Prompt Tuning (PT):** PT involves adding a small, trainable prompt matrix to input embeddings, optimizing only this matrix to improve the model. It's popular due to its efficiency and ability to utilize the knowledge from pre-trained models.


- **Decomposed Prompt Tuning (DePT):** DePT builds on PT by breaking down the prompt matrix into a shorter prompt and two smaller matrices. This results in:


####


Experiments and Results


DePT was thoroughly tested on 21 NLP and 2 vision-language tasks. It outperformed both the latest PEFT methods and traditional full fine-tuning in several cases. Highlights include:


- Over 20% savings in training time and memory usage compared to PT.


- Increased benefits with larger models, making it especially useful for LLMs.


- High performance in few-shot learning scenarios, even with limited data.


####


Contributions


DePT significantly advances PEFT by:


- **Methodological Innovation:** It overcomes the limitations of PT by decomposing the soft prompt, offering a more efficient approach.


- **Enhanced Performance:** DePT matches or exceeds the performance of other PEFT methods and full fine-tuning on various tasks.


- **Scalability and Adaptability:** Its efficiency gains are more pronounced with larger models, and it works well with different PEFT strategies.


####


Conclusion and Future Directions


DePT marks a significant step forward in efficiently fine-tuning LLMs, addressing the main issues with prompt tuning through its innovative approach.


Future research will explore integrating DePT with other PEFT methods, expanding its application areas, and optimizing its components for even better performance.


####


Acknowledgments


The development of DePT was enriched by feedback from ICLR reviewers and supported by UCL, highlighting the collaborative nature of AI research.


DePT represents a major development in PEFT methods, offering a scalable, efficient, and effective solution for adapting LLMs to diverse tasks and setting the stage for future innovations in the field.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
