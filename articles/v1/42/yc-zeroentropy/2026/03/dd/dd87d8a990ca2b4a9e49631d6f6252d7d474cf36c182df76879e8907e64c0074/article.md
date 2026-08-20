---
schema_version: "1.0.0"
document_id: "dd87d8a990ca2b4a9e49631d6f6252d7d474cf36c182df76879e8907e64c0074"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/introducing-zembed-1-the-worlds-best-multilingual-text-embedding-model/"
published_at: "2026-03-02T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:0ee91d26d7d6a998540db162277520dc364546fd42b8517b29445ef5736e3ece"
---

# Introducing zembed-1: The World's Best Text-Embedding Model

TL;DR


- Today, ZeroEntropy is launching the world’s most accurate[embedding model](https://www.zeroentropy.dev/concepts/embedding/) , which outperforms all other embedding models by up to +7% on[Recall@100](https://www.zeroentropy.dev/concepts/recall-at-k/) , including OpenAI Large, Qwen3 4B, BGE-M3, Gemini Embeddings, Cohere v4, and Voyage-4-nano.
- zembed-1 is a 4B open-weight multilingual embedding model distilled directly from our own state-of-the-art[reranker](https://www.zeroentropy.dev/concepts/reranker/) zerank-2.
- zembed-1 is available now through the ZeroEntropy search engine, our API, HuggingFace, and AWS Marketplace.
- **🎉 We are offering 50% off on document embeddings until June 1st!**


# A New Standard in SOTA Embeddings


## State-of-the-Art Accuracy Across Verticals


[zembed-1](https://huggingface.co/zeroentropy/zembed-1) delivers the highest retrieval accuracy of any embedding model available today. Across a broad evaluation spanning private and public benchmarks, zembed-1 outperforms every model we tested — and when paired with our reranker[zerank-2](https://huggingface.co/zeroentropy/zerank-2) , the gap widens further.


The gains hold across verticals. zembed-1 shows particularly strong performance on finance, healthcare, and legal, where specialized domain vocabulary and nuanced relevance ranking matter most.


## Accuracy, Latency, Affordability: Pick Three


Most embedding models force a trade-off between accuracy, speed, and cost. zembed-1 doesn’t.


We invested heavily in performance engineering,[quantization](https://www.zeroentropy.dev/concepts/embedding-quantization/) , and dimensional flexibility — three levers that let you tune the accuracy-cost trade-off at inference time, without retraining. Quantization compresses each dimension from 32-bit floats down to 8-bit integers or single-bit binary, cutting storage per[vector](https://www.zeroentropy.dev/concepts/vector/) by 4× or 32×. Flexible dimensionality lets you truncate embeddings from the full 2048 dimensions down to as few as 40, discarding the least informative components while preserving the most. Combine both, and you can shrink a vector from 8 KB to under 128 bytes — with a controlled, predictable accuracy trade-off at every step.


## Detailed Evals: Multilingual and Vertical Breakdown


Zembed’s accuracy advantage isn’t limited to English or general benchmarks. Here’s how our model compares across verticals and languages against the full competitive field.


zembed-1 is multilingual from the ground up. Over half of the training data used to create zembed-1 was in languages other than English. With our focus on well-calibrated[cross-lingual query-document pairs](https://www.zeroentropy.dev/concepts/cross-lingual-retrieval/) , you get exactly the same[Elo-trained](https://www.zeroentropy.dev/concepts/elo-score/) relevance judgement whether the query is in English, Japanese, Arabic, or any other major language.


For full evaluation results across all datasets and configurations, see the detailed spreadsheet[here](https://docs.google.com/spreadsheets/d/1qFXGZLMg6-O5tVLIJS3tpf5QNJxCHiiQtj35dZub4vY/edit?gid=0#gid=0) .


## What Model Configuration is Best for Me?


zembed-1 gives you two knobs to tune at inference time — no retraining required:


**Dimensions (40 → 2056):** You can truncate embeddings from the full 1024 dimensions down to as few as 40. Unlike[Matryoshka-style training](https://www.zeroentropy.dev/concepts/mrl-matryoshka/) , we use a cheap client-side linear transformation that preserves more information at every dimension count — so you get better accuracy at the same storage cost.


**Quantization (float32 / int8 / binary):** Each dimension can be stored as a 32-bit float, compressed to an 8-bit integer (4× smaller), or reduced to a single bit (32× smaller). At full dimensions with float32, a single vector is 4 KB. At 256 dimensions with int8, it’s just 256 bytes — with minimal accuracy loss.


For a full guide covering trade-offs and considerations when picking an embedding model, check out our latest blog[here](https://www.zeroentropy.dev/articles/introducing-zembed-1-the-worlds-best-multilingual-text-embedding-model/how-to-overcome-poor-search-results-with-the-right-embedding-solution) .


## How We Trained zembed-1


Most embedding models are trained on binary signals: a document is either relevant or not relevant. For zembed-1, we took a fundamentally different approach by[distilling](https://www.zeroentropy.dev/concepts/knowledge-distillation/) zembed-1 from our state-of-the-art reranker, zerank-2.


By distilling this signal into the model, zembed-1 inherits a relevance intuition that no standard contrastive training can match. This innovation unlocks disruptive accuracy gains, across verticals, and languages.


## Get Started


zembed-1 is available today with a[context window](https://www.zeroentropy.dev/concepts/context-window/) of 32k tokens through multiple deployment options:


### Get Started


zembed-1 is available today through multiple deployment options.


[→ ZeroEntropy API fully managed, lowest-friction path to production](https://docs.zeroentropy.dev/models)[→ HuggingFace open weights, run it on your own infrastructure](https://huggingface.co/zeroentropy/)[→ AWS Marketplace deploy within your existing AWS environment](https://aws.amazon.com/marketplace/seller-profile?id=seller-nurj4uavxb4z2)


```text
from   zeroentropy   import   ZeroEntropy
zclient   =   ZeroEntropy()
response   =   zclient.models.embed(
model  =  "zembed-1"  ,
input_type  =  "query"  ,   # "query" or "document"
input  =  "What is retrieval augmented generation?"  ,   # string or list[str]
dimensions  =  2560  ,   # optional: must be one of [2560, 1280, 640, 320, 160, 80, 40]
encoding_format  =  "float"  ,   # "float" or "base64"
latency  =  "fast"  ,   # "fast" or "slow"; omit for auto
)
```


**Documentation** :[docs.zeroentropy.dev](https://docs.zeroentropy.dev/)


**HuggingFace** :[huggingface.co/zeroentropy](https://huggingface.co/zeroentropy)


**Get in touch** :[Discord community](https://discord.gg/5mkQCTnmY9) orcontact@zeroentropy.dev


Talk to us if you need a custom deployment, volume pricing, or want to see how zembed-1 + zerank-2 performs on your data.
