---
schema_version: "1.0.0"
document_id: "0a3b555bc83ebf2b7483fdc65ec4ba2dcdb8e072c142e81a1d9a302e2239f592"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/compress-then-prompt-improving-accuracy-efficiency-trade-off-of-llm-inference-with-transferable-prompt"
published_at: "2023-05-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:c624aa289c5f47becbfd0203d0da49894f57d15c1c49520551b72bc1944a6e94"
---

# Compress, Then Prompt: Improving Accuracy-Efficiency Trade-off of LLM Inference with Transferable Prompt

Do not index


Original Paper


[https://arxiv.org/abs/2305.11186](https://arxiv.org/abs/2305.11186)


Blog URL


[blog.athina.ai /compres...e-prompt](https://blog.athina.ai/compress-then-prompt-improving-accuracy-efficiency-trade-off-of-llm-inference-with-transferable-prompt)


**Original Paper:**[https://arxiv.org/abs/2305.11186](https://arxiv.org/abs/2305.11186)


**By:**[Zhaozhuo Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20Z) ,[Zirui Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20Z) ,[Beidi Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20B) ,[Yuxin Tang](https://arxiv.org/search/cs?searchtype=author&query=Tang%2C%20Y) ,[Jue Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20J) ,[Kaixiong Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20K) ,[Xia Hu](https://arxiv.org/search/cs?searchtype=author&query=Hu%2C%20X) ,[Anshumali Shrivastava](https://arxiv.org/search/cs?searchtype=author&query=Shrivastava%2C%20A)


**Abstract:**


> While the numerous parameters in Large Language Models (LLMs) contribute to their superior performance, this massive scale makes them inefficient and memory-hungry. Thus, they are hard to deploy on commodity hardware, such as one single GPU. Given the memory and power constraints of such devices, model compression methods are widely employed to reduce both the model size and inference latency, which essentially trades off model quality in return for improved efficiency. Thus, optimizing this accuracy-efficiency trade-off is crucial for the LLM deployment on commodity hardware. In this paper, we introduce a new perspective to optimize this trade-off by prompting compressed models. Specifically, we first observe that for certain questions, the generation quality of a compressed LLM can be significantly improved by adding carefully designed hard prompts, though this isn't the case for all questions. Based on this observation, we propose a soft prompt learning method where we expose the compressed model to the prompt learning process, aiming to enhance the performance of prompts. Our experimental analysis suggests our soft prompt strategy greatly improves the performance of the 8x compressed LLaMA-7B model (with a joint 4-bit quantization and 50% weight pruning compression), allowing them to match their uncompressed counterparts on popular benchmarks. Also, we demonstrate that these learned prompts can be transferred across various datasets, tasks, and compression levels. Hence with this transferability, we can stitch the soft prompt to a newly compressed model to improve the test-time accuracy in an \`\`in-situ'' way.


---


###


Summary Notes


####


Blog Post: Making LLM Deployment More Efficient on Standard Hardware


The world of natural language processing (NLP) has been transformed by large language models (LLMs) such as GPT-3, which have the capability to generate texts that are remarkably human-like, answer queries, and even code.


Despite their impressive abilities, LLMs require considerable computational resources, making them challenging to deploy on devices with limited hardware capabilities.


This is a significant hurdle for AI Engineers at enterprise companies who need to run these models efficiently.


###


Tackling Model Compression


To address this, techniques like quantization and pruning are used to compress models, aiming to make LLMs lighter and faster, thus more suitable for a variety of devices.


However, compressing models often means sacrificing accuracy for efficiency, leading to a dilemma between maintaining performance and reducing resource consumption.


####


Enhancing Performance with Prompt Optimization


A promising solution to this dilemma lies in optimizing the prompts used to communicate with compressed models. There are two main strategies:


- **Hard Prompts** : These inform the model about its compressed state but can be hit-or-miss in effectiveness.


- **Soft Prompts** : These are flexible, learnable prompts that are fine-tuned with the model, showing great promise in improving performance without compromising efficiency.


###


Advanced Prompt Learning Techniques


Innovative research introduces a method for optimizing prompts during the training of compressed LLMs.


This approach aims to enhance model performance and ensure the prompts are versatile across different tasks and compression levels. Such adaptability is key for AI Engineers who wish to deploy LLMs in various applications without extensive retraining.


####


Experimental Results: Narrowing the Gap


Experiments show that optimized prompts significantly boost the performance of compressed models, making them nearly as effective as their uncompressed versions.


These prompts also maintain their effectiveness across different models and tasks, providing a practical way to deploy powerful LLMs on standard hardware without major quality compromises.


###


Conclusion: Expanding LLM Deployment Possibilities


The strategy of combining model compression with prompt optimization marks a significant step forward in deploying LLMs more broadly. It addresses the critical challenge of balancing accuracy with efficiency, enabling the deployment of LLMs in real-world scenarios even on less capable devices. This advancement opens up new opportunities for innovation and application development for AI Engineers.


####


Moving Toward Resource-Efficient Deployment


The future of deploying advanced AI models hinges on efficiency and adaptability. The approach outlined here offers a practical route to this future, emphasizing the importance of flexible and optimized model deployment.


For AI practitioners, this means leveraging innovative solutions like prompt optimization to fully utilize LLMs across various applications, making advanced AI more accessible and practical for widespread use.


In essence, the journey towards more efficient deployment of LLMs is progressing. By focusing on model compression and prompt optimization, we're making these powerful tools more available, regardless of hardware constraints.


This approach not only democratizes access to leading-edge AI but also drives forward innovation in NLP and related fields.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
