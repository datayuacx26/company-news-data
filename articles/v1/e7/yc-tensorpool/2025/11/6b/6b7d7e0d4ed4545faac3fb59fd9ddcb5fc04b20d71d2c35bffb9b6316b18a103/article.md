---
schema_version: "1.0.0"
document_id: "6b7d7e0d4ed4545faac3fb59fd9ddcb5fc04b20d71d2c35bffb9b6316b18a103"
company_key: "yc-tensorpool"
company: "TensorPool"
source_id: "yc-tensorpool-news-import-ce508c9f2d8e"
canonical_url: "https://tensorpool.dev/blog/zeroentropy-zerank-training"
published_at: "2025-11-25T00:00:00+00:00"
first_seen_at: "2026-07-24T13:32:36.763199+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:f089c0c6927f46a8e22d1c29b6d4fc890727c75bcc74f1e0deb3956a69d8dc59"
---

# How ZeroEntropy Trained the zerank Model Family on TensorPool

ZeroEntropy's zerank


series has emerged as dominant open-weight rerankers across various verticals like customer support, legal, finance, medical, and code. Their breakthrough came from[a novel training approach](https://www.zeroentropy.dev/articles/paper-tldr-how-we-trained-zerank-1-with-the-zelo-method) using synthetic pairwise preference annotations turned into Elo scores for ranking. Both zerank-1


and the recently launched zerank-2


were trained on TensorPool.


## Models at a Glance


**zerank** is a family of open source state-of-the-art rerankers. They are cross-encoders that are LoRA fine-tunes of the Qwen3 model family.


- **[zerank-1](https://huggingface.co/zeroentropy/zerank-1)** and **[zerank-2](https://huggingface.co/zeroentropy/zerank-2)** are 4B parameters and finetuned on a Qwen3-4B base. They're released under the Creative Commons Attribution Non Commercial 4.0 license.
- The compact variant, **[zerank-1-small](https://huggingface.co/zeroentropy/zerank-1-small)** , is built on a Qwen3-1.7B base that is open-source under the Apache 2.0 license; it has 1.7B parameters.


zerank's state-of-the-art performance has led it to dominate in RAG deployments for critical domains such as customer support, legal, finance, medical, and code domains.


## What Makes zerank Special


There are a couple characteristics we think are under appreciated:


### Future Ready Training Pipeline


ZeroEntropy's novel training approach: **[zELO](https://www.zeroentropy.dev/articles/paper-tldr-how-we-trained-zerank-1-with-the-zelo-method)** . Their pipeline combines synthetic pairwise supervision with an Elo-style ranking scheme. This means that training data can be regenerated as LLMs improve. This grants them a free lunch (aside from the token cost) without the logistical drag or expense of human annotation.


### ZeroEntropy Punches Above Their Weight


The launch of zerank-1


in July 2025 showed how well ZeroEntropy is able to execute despite few resources. Cohere was navigating a complex reorganization following their $500M Series D (~$1.5B raised to date). ZeroEntropy came fresh out of their **[$4.2M seed](https://www.linkedin.com/feed/update/urn:li:activity:7348718712180420609/)** and leapfrogged the large well-resourced incumbents.


## How TensorPool Helped


TensorPool's CLI and elastic access to GPUs enabled ZeroEntropy to speed up their experimentation cycle. This enabled them to iterate faster, test more ideas, and focus on research rather than infrastructure logistics.


> We chose TensorPool because they not only provide easy to use infrastructure, but also go out of their way to ensure we get the best bang for our buck."
>
>
> **— Nicholas Pipitone, CTO of ZeroEntropy**


## Conclusion


ZeroEntropy demonstrates that smart architecture and training methodology outpaces pure capital deployment. zELO, ZeroEntropy's novel training pipeline, is the backbone of their SOTA reranker today and is positioned to be even better tomorrow. TensorPool enabled ZeroEntropy to compress their research timeline, turning the R&D cost center into a competitive advantage.


The result is the zerank reranker family that's reshaping retrieval in domains where precision matters most—proving speed and research focus matter more than headcount or funding rounds.


To learn more about ZeroEntropy and the zELO training pipeline,[check out their blog](https://www.zeroentropy.dev/articles/paper-tldr-how-we-trained-zerank-1-with-the-zelo-method) !
