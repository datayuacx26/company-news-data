---
schema_version: "1.0.0"
document_id: "7f9785b3f13b90b0b1310f7d8d714ea4d715b9dbb542a13d13fa6c248b7e4c7a"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/llms-can-understand-encrypted-prompt-towards-privacy-computing-friendly-transformers"
published_at: "2023-12-15T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:20.635269+00:00"
content_hash: "sha256:e0a52bd75e3ba45279ebcd2b9d88f31f3740c066027ddfad1033d61514a0858a"
---

# LLMs Can Understand Encrypted Prompt: Towards Privacy-Computing Friendly Transformers

Do not index


Original Paper


[https://arxiv.org/abs/2305.18396](https://arxiv.org/abs/2305.18396)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2305.18396](https://arxiv.org/abs/2305.18396)


**By:**[Xuanqi Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20X) ,[Zhuotao Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20Z)


**Abstract:**


> The community explored to build private inference frameworks for transformer-based large language models (LLMs) in a server-client setting, where the server holds the model parameters and the client inputs its private data (or prompt) for inference. However, these frameworks impose significant overhead when the private inputs are forward propagated through the original LLMs. In this paper, we show that substituting the computation- and communication-heavy operators in the transformer architecture with privacy-computing friendly approximations can greatly reduce the private inference costs while incurring very minor impact on model performance. Compared to state-of-the-art Iron (NeurIPS 2022), our privacy-computing friendly model inference pipeline achieves a 5× acceleration in computation and an 80% reduction in communication overhead, while retaining nearly identical accuracy.


---


###


Summary Notes


####


Enhancing Privacy in Language Models: A New Approach to Private Inference


Language models like GPT-3 have transformed how machines interpret human language, playing a pivotal role in services from automated support to personalized content. However, their use in handling sensitive data has sparked privacy concerns, primarily because they require plaintext inputs, risking data exposure to service providers.


A new study presents a novel method for private inference, allowing language models to process encrypted data, safeguarding user privacy.


This post aims to provide AI engineers with an in-depth look at this development, highlighting its significance in enhancing privacy in AI applications.


###


The Challenge with Private Inference in Transformers


As language models become more common, the need for better privacy protections has become urgent. Traditional models need unencrypted queries, posing a significant privacy risk.


Private inference offers a solution by encrypting user inputs so that they can be processed without decryption.


However, the complex nature of transformer-based models, which many modern language models use, makes private inference challenging due to their computational demands.


###


Previous Methods for Preserving Privacy


Earlier efforts to maintain privacy during inference used Homomorphic Encryption (HE) and Secure Multiparty Computation (MPC).


HE faced difficulties with the non-linear operations in language models, causing high computational costs. While MPC and polynomial encoding provided alternatives, they lacked in efficiency and scalability.


###


A New Framework for Privacy


The study introduces a framework designed for private inference on transformers, using privacy-computing friendly operators to reduce computational load without losing accuracy.


This method cleverly replaces expensive operations with more efficient ones and uses secret-sharing for inputs and weights, fitting well with transformer models.


####


Techniques for Private Transformer Inference


- **Linear Operations** : The framework uses encrypted matrix multiplication protocols to keep data encrypted during linear operations.


- **Non-linear Operations** : It approximates non-linear functions like GELU, softmax, and layer normalization with privacy-friendly operations, using cryptography to ensure computation privacy.


####


Efficiency Optimization Techniques


The framework employs several techniques to boost efficiency:


- **Communication Efficiency** : It optimizes matrix multiplication protocols to reduce the required data size for transmission, enhancing speed.


- **Hardware Utilization** : Using GPU acceleration for homomorphic encryption operations significantly improves efficiency, making private inference feasible for enterprise use.


###


Performance Insights


Performance tests on models like BERT-Tiny have shown notable improvements in speed and communication efficiency. Compared to existing systems like Iron, this new framework excels at performing privacy-friendly model inference.


###


Looking Ahead


This study marks a significant step forward in addressing privacy concerns with language models, offering a method for efficient private inference on transformer models.


As technology evolves, we anticipate further enhancements and practical applications of these techniques, leading to a new phase of privacy-focused AI development.


The potential for this research to influence future work in the field is considerable, promising improved privacy safeguards in our increasingly data-centric world.


###


Further Exploration


For those interested in the technical details, supplementary materials and an extensive list of references are available for a deeper dive into the study. This information provides valuable insight into the research's comprehensive approach, highlighting the ongoing efforts to enhance privacy in language models through private inference.


As AI becomes more intertwined with our digital existence, protecting sensitive information remains crucial, underscoring the importance of continued innovation in privacy computing.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
