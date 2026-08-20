---
schema_version: "1.0.0"
document_id: "b60a23f8467ffce5553d5d53676ad143a2dfe444f9bc6a21e38efcb68f107f09"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-21"
published_at: "2024-04-18T16:23:32+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:61b858f986808c6527d2fc2cdc7f008c2a41f77d8c6bc6fcbe218590b318d168"
---

# The Deep Dive Issue #21

# The Deep Dive Issue #21


### LIT, attention boost, and AI understanding


[Nassim](https://substack.com/@nas07)


Apr 18, 2024


Have you considered a career as a DJ yet? If not then it's safe to assume you've missed on


[Udio](https://www.udio.com/) , the "ChatGPT of music generation" that just hit the scene (pun non intended) last week.


While it still won't have you make a full switch from Spotify (not like it's trying to look like it right?), gotta admit there's some really cool pieces to jam on already. If you ever fancied yourself a producer, now’s the time to make your lyrics shine 🎵


### **Shedding light on the black box**


Despite performing well on many tasks, language models are prone to biases and errors. Understanding their behavior is critical, but detecting patterns with simple prompting is tedious. The Learning Interpretability Tool (LIT) provides the tools to explore model behavior efficiently.


**Why would you care?** - LIT is designed to be flexible, extensible and framework-agnostic. It has a no-code interface for running experiments so you can jump into tinkering with models.


*Overview of the LIT interface. Source: [Original Paper](https://arxiv.org/pdf/2008.05122.pdf)*


**How does it work?** - LIT's web interface consists of connected modules like a data table, embedding projector, salience maps, etc,. The Python backend handles hosting models, datasets, and interpretation components. Components like models and metrics communicate through APIs and standard Python data types.


A typical workflow using LIT looks like this:


1.


**Loading Models and Data** : You start by providing LIT with the language model you want to analyze and the relevant dataset.


2.


**Exploring and Analyzing** : The interface allows you to explore the data through visualizations and identify interesting data points or subsets.


3.


**Interpreting and Explaining** : You can use various interpretation techniques like salience maps and attention visualizations to understand why the model made specific predictions.


4.


**Generating Counterfactuals** : You can create new data points with slight modifications to observe how the model's predictions change.


5.


**Comparing and Evaluating** : LIT supports comparing models or data points side-by-side and calculating performance metrics for evaluation.


Check out the


[repository](https://github.com/PAIR-code/lit) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-21?utm_source=substack&utm_medium=email&utm_content=share&action=share)


### **The Lab**


#### **Hyper-attentive**


Google researchers released


[Infini-attention](https://arxiv.org/pdf/2404.07143.pdf) , a new attention mechanism to handle long context inputs.


Infini-attention incorporates a compressive memory system into the attention mechanism to manage long-range dependencies. Unlike traditional attention that stores all key-value pairs, it maintains a fixed-size memory. This memory is updated incrementally as new information is processed. This allows the model to retain relevant context without incurring excessive memory costs.


The long-term memory retrieval is combined with local attention that focuses on current segments, to capture both global and local context. This approach is integrated into a Transformer block, resulting in the Infini-Transformer model. The model processes input sequences in segments, applying local attention to each segment. While doing so, it uses the compressive memory to retrieve relevant information from past segments. This design enables handling infinitely long inputs while maintaining a bounded memory footprint.


Infini-Transformer demonstrates superior performance on long-context language modeling tasks. It achieves better perplexity scores than baseline models while significantly reducing memory requirements.


#### **Just the tip of the iceberg**


Mixture-of-Experts (MoE) models require more parameters than dense models. This makes them less efficient in scenarios where memory usage is crucial.


[Dense-Sparse MoE (DS-MoE)](https://arxiv.org/pdf/2404.05567.pdf) addresses the parameter inefficiency of MoE models. DS-MoE trains the model densely, using all experts in each layer. It then runs inference sparsely by activating only the top-performing experts based on their scores. Dense training ensures efficient parameter usage, similar to traditional dense models. For sparse inference, expert selection is determined either by ranking scores, or based on a predefined threshold.


Additionally, a DS-MoE incorporates a Mutual Information (MI) loss. The objective of the MI is to promote load balancing among experts and prevent underused model capacity. This loss function encourages an even distribution of workload across experts. It also maintains a focus on relevant solutions.


Finally, the traditional self-attention layer is replaced with a Mixture of Attention (MoA) heads layer. With MoA, each expert computes only a subset of query vectors to further enhance efficiency.


DS-MoE models achieve comparable performance to dense models with the same model size while activating only 30-40% of the parameters.


#### **Occam's tokens razor**


Current language models waste time and resources by treating all words in a text equally during training, regardless of their importance to the desired learning outcome.


[RHO-1](https://arxiv.org/pdf/2404.07965.pdf) is a new language model trained using Selective Language Modeling (SLM). Unlike standard language models that try to predict every word in a text, RHO-1 focuses on learning from a subset of words that are deemed more relevant to the specific task at hand. This selection process involves using a reference model to score the words in a text and identify those with a higher "excess loss," indicating their potential for greater learning impact.


RHO-1 concentrates its training efforts on the high-value words, leading to more efficient and effective learning. By selectively focusing on the most informative words, it can achieve comparable or even superior performance with significantly less training data compared to traditional models.


RHO-1 demonstrates significant improvements in both efficiency and performance compared to traditional language models, using only a small portion of training data for some tasks and improving average performance across other tasks.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


### **The Pulse**


**One chip to know you whole** - Meta has unveiled the next generation of its


[custom-designed AI chip](https://ai.meta.com/blog/next-generation-meta-training-inference-accelerator-AI-MTIA/) , the Meta Training and Inference Accelerator (MTIA v2). This new chip is specifically designed to handle ranking and recommendation models, and boasts more than double the compute and memory bandwidth of the previous version. As part of a broader full-stack development program, MTIA v2 works in tandem with a custom hardware system and optimized software stack, including integration with PyTorch and the Triton compiler.


**All about the RAG** - Cohere released


[Rerank 3](https://txt.cohere.com/rerank-3/) , another foundation model designed to dramatically improve enterprise search and Retrieval Augmented Generation (RAG) systems. It seamlessly integrates with existing databases and applications, enhancing search performance and lowering RAG costs without impacting speed. Rerank 3 boasts powerful features like handling long documents, searching multi-aspect data (emails, code, tables), and supporting 100+ languages.


**You said listening comprehension?** - AssemblyAI has launched


[Universal-1](https://www.assemblyai.com/blog/announcing-universal-1-speech-recognition-model/) , a powerful new speech recognition model boasting significant advancements in accuracy, efficiency, and multilingual capabilities. Trained on a massive dataset of over 12.5 million hours, Universal-1 excels in challenging conditions like background noise and accented speech, while also offering improved timestamp precision and efficient parallel processing for faster turnaround times.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify Dev Team.


---


On another note, we’re hosting a cool event next week alongside


[Laminar](https://www.lmnr.ai/) where we’ll demonstrate how you can seamlessly build an LLM pipeline with Unify.


[Grab your seat](https://lu.ma/UnifyxLaminar) and join us!
