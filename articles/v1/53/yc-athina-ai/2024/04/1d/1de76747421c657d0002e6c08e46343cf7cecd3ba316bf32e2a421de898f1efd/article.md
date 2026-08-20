---
schema_version: "1.0.0"
document_id: "1de76747421c657d0002e6c08e46343cf7cecd3ba316bf32e2a421de898f1efd"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompt-cache-modular-attention-reuse-for-low-latency-inference"
published_at: "2024-04-25T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:08a8b1da698798c037e4649e2d12de85856a09edce9ade9235a099617cc2b45e"
---

# Prompt Cache: Modular Attention Reuse for Low-Latency Inference

Do not index


Original Paper


[https://arxiv.org/abs/2311.04934](https://arxiv.org/abs/2311.04934)


Blog URL


[blog.athina.ai /prompt-...nference](https://blog.athina.ai/prompt-cache-modular-attention-reuse-for-low-latency-inference)


**Original Paper:**[https://arxiv.org/abs/2311.04934](https://arxiv.org/abs/2311.04934)


**By:**[In Gim](https://arxiv.org/search/cs?searchtype=author&query=Gim%2C%20I) ,[Guojun Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20G) ,[Seung-seob Lee](https://arxiv.org/search/cs?searchtype=author&query=Lee%2C%20S) ,[Nikhil Sarda](https://arxiv.org/search/cs?searchtype=author&query=Sarda%2C%20N) ,[Anurag Khandelwal](https://arxiv.org/search/cs?searchtype=author&query=Khandelwal%2C%20A) ,[Lin Zhong](https://arxiv.org/search/cs?searchtype=author&query=Zhong%2C%20L)


**Abstract:**


> We present Prompt Cache, an approach for accelerating inference for large language models (LLM) by reusing attention states across different LLM prompts. Many input prompts have overlapping text segments, such as system messages, prompt templates, and documents provided for context. Our key insight is that by precomputing and storing the attention states of these frequently occurring text segments on the inference server, we can efficiently reuse them when these segments appear in user prompts. Prompt Cache employs a schema to explicitly define such reusable text segments, called prompt modules. The schema ensures positional accuracy during attention state reuse and provides users with an interface to access cached states in their prompt. Using a prototype implementation, we evaluate Prompt Cache across several LLMs. We show that Prompt Cache significantly reduce latency in time-to-first-token, especially for longer prompts such as document-based question answering and recommendations. The improvements range from 8x for GPU-based inference to 60x for CPU-based inference, all while maintaining output accuracy and without the need for model parameter modifications.


---


###


Summary Notes


####


Accelerating AI Inference with Prompt Cache: A Breakthrough Approach


In the rapidly advancing field of AI, the efficiency of large language models (LLMs) during inference is critical.


A standout solution, **Prompt Cache** , dramatically speeds up this process by intelligently reusing attention states for different prompts.


This post explores how Prompt Cache is revolutionizing LLM efficiency, becoming an essential tool for AI Engineers.


####


Understanding the Challenge


LLMs are central to AI's progress, fueling advancements in various domains. However, their autoregressive generation of tokens is highly demanding, mainly due to the continuous recalculation of attention states.


While the Key-Value (KV) Cache method has made strides by allowing the reuse of key-value pairs within the same prompt, it doesn't support reuse across different prompts. Enter **Prompt Cache** : an evolution of KV Cache that significantly cuts down inference times through a smart, modular reuse of attention states.


####


How Prompt Cache Works


Prompt Cache revolutionizes efficiency with two key innovations:


- **Prompt Markup Language (PML):** PML organizes prompts into clear, reusable modules, each with unique position IDs. This modular structure facilitates the efficient reuse of text segments across different prompts, ensuring accurate positioning.


- **Cached Inference Process:** When a new prompt arrives, Prompt Cache quickly identifies and reuses any precomputed attention states for known modules, only calculating new states for unfamiliar segments. This slashes the time needed for inference.


These features address common challenges in reusing text segments, allowing for smooth integration into existing LLM workflows.


####


Implementing Prompt Cache


Integrating Prompt Cache is seamless with libraries like HuggingFace transformers, fitting any Transformer model that supports KV Cache.


Its implementation smartly balances CPU and GPU memory use, optimizing both storage capacity and latency without needing significant model or infrastructure changes.


####


The Impact of Prompt Cache


Prompt Cache's effectiveness is evident in its substantial reduction of inference latency—up to 60× on CPUs and 8× on GPUs—and its maintenance of high output accuracy. It also showcases efficient memory management, scaling well for enterprise use.


####


Applications and Looking Ahead


Prompt Cache is ideal for sectors with structured prompts, such as legal, healthcare, and education, reducing latency without compromising accuracy.


Future enhancements might include better GPU cache strategies and compression methods for modules, further elevating efficiency and scalability.


####


Conclusion


Prompt Cache is a transformative solution for LLM inference, offering scalable, accurate, and low-latency performance.


Its innovative approach to reusing attention states across prompts minimizes computational demands, heralding a new era of efficient and sophisticated AI applications.


For AI Engineers, adopting Prompt Cache could significantly boost performance and efficiency, marking a pivotal shift in large language model deployment.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
