---
schema_version: "1.0.0"
document_id: "8ad1f7af754f8d2537e97ac0091421be05bee8876cad87e9f0c26a708af32c4b"
company_key: "yc-flower"
company: "Flower"
source_id: "yc-flower-news-import-15689314081b"
canonical_url: "https://flower.ai/blog/2026-04-15-releasing-lizzy7b-model"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-21T20:31:29.302072+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:e0f27b13ff3c83f645efec5a8a5c8be488afbcf87bc30f3ab68395a28ed57c6d"
---

# Lizzy-7B: A UK-built Open Frontier LLM for Sovereign AI

Today, we are announcing the preview **[launch of Lizzy 7B](https://flower.ai/models/lizzy)** , a state-of-the-art[open-weight language model](https://huggingface.co/flwrlabs/Lizzy-7B) by Flower Labs built entirely in the United Kingdom. This release marks an important first step toward sovereign AI infrastructure in the UK. It combines strong model performance with the ability to deploy in controlled, local environments. While this is still a preview release, Lizzy is ready to use today across a range of real-world applications.


## Designed for the UK


Lizzy 7B is designed from the ground up for the UK. From training through evaluation, the model reflects UK-specific language, institutions, and use cases, enabling more reliable performance in domains such as financial services, public infrastructure, healthcare and government systems. This preview release represents the starting point of deeper UK-focused AI support and innovation, both in terms of model capability and ecosystem development.


## Top European Performance


In benchmarking, Lizzy delivers competitive performance against leading European models in the 7B class, including models such as the French Mistral 8B, Swiss Apertus 8B and the larger multi-country effort: EuroLLM 9B. Across a focused set of downstream evaluations, the model shows strong general capability alongside targeted UK-specific metrics that assess factual accuracy and style in local contexts. These early results are promising and will continue to improve as we expand both the evaluation suite and post-training in future iterations.


Benchmark Lizzy 7B EuroLLM 9B Apertus 8B


Britishness MCQ 71.0 77.6 **80.8**


Britishness CoT **80.1** 72.1 31.7


Britishness Domains **89.9** 69.0 32.6


MATH **77.9** 31.3 22.4


OMEGA **29.0** 4.7 5.0


BigBenchHard **69.0** 38.9 42.4


AGI Eval English **65.6** 50.2 50.4


MMLU **67.9** 57.4 63.4


GPQA **34.6** 26.8 28.1


## Ready for Adoption and Enterprise


Lizzy is built for practical deployment. It supports modern inference stacks such as vLLM, runs across diverse infrastructure environments, and can be deployed with full control over data and compute. The model is available on Hugging Face with a simple, one-click launch, making this preview immediately accessible to developers and organizations alike.


## Built on years of earlier R&D


This release builds on Flower’s broader work in decentralized AI. Lizzy leverages knowledge from a multi-year R&D effort into training across heterogeneous and distributed systems, enabling scalable development without reliance on centralized infrastructure. This foundational effort also enables Flower Labs to build sovereign models for other countries, for example Germany and beyond, along with industry-specific systems tailored to sectors like finance, healthcare, and telecommunications.


## Next Steps


We would like to thank the teams at Hugging Face, Arcee, the Olmo team at Allen Institute for AI, and the wider open-source AI ecosystem for their foundational contributions on which we have built. The release of Lizzy stands on this shared progress, and this preview marks the beginning of our continued investment in open, sovereign AI not just for the UK, but also for Europe and the entire open-source AI community.
