---
schema_version: "1.0.0"
document_id: "bdf740427e700575a4de311d2685830c6843a108178aaa83cac157ef43d73e2e"
company_key: "yc-compliant-llm"
company: "compliant-llm"
source_id: "yc-compliant-llm-news-import-79fa5784f6e7"
canonical_url: "https://compliantllm.com/blog/train-highly-performant-model-like-llama-3-8b"
published_at: null
first_seen_at: "2026-07-23T06:18:10.598093+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:55b3dbba48d1ff8c406288e290f6369c5219a457d2f2fa2f791538460db8aa1c"
---

# How to train a small, highly performant model like Llama-3-8B?

## **Tl;Dr;**


-


Train on a large amount of high-quality data.


-


Use LLMs to curate and filter data based on various quality metrics including toxicity (NSFW, abuse, harm, etc), deduplication, diversity, correctness, and style.


-


With fine-tuning, an 8B param model can match the performance of GPT-4 for custom use cases at 1/50th of the cost.


## **Why the 8B param model?**


Fits into 1 GPU.


Cheaper and faster in training and inference.


## **Optimize model performance**


Llama-3 was pre-trained on 15T tokens and fine-tuned on 10M tokens of high-quality data.


Improving the size and quality of the dataset led to improvements in model performance.


Llama-3-8B is comparable to Mixtral - a 4x larger model.


The model showed continuous improvement with more pre-training tokens, even at 15T tokens - showing that there is room to get further improvements with more data, even for a 8B param model.


## **Fine-Tuning**


Supervised fine-tuning of open-source models is already mainstream.


An easy-to-adopt a modification of this is preference ranking - where a model is shown multiple, ranked outputs for a given prompt. This helps the model perform better in questions that it might previously have hallucinated on. Training of this kind improves the LLM's ability to pick the right answer given a prompt in the wild.


Llama-3 used 10M human annotated samples for fine-tuning.


## **Data Filtering using LLMs**


LLMs fine-tuned for data quality evaluation and filtering are used to further check and filter the data. This is essential at the large volumes of data as used by Llama-3.


Data quality checks include:


-


Correctness checks


-


Style and form checks


-


Context relevancy - ensuring that the response respects the context passed along with the query


-


Filter abusive, harmful, NSFW data


Using multiple LLMs to generate data eliminates bias in human-annotated datasets that lead to biased models.


## **AI-Generated Data**


LLMs are used to generate high-quality question-answer pairs and instruction datasets for fine-tuning. The diversity of LLM-generated data can be maintained by applying algorithms like evol-instruct thereby reducing the problem of over-fitting on training data.


## **AI Safety and Alignment**


Enforcing the responsible use of AI according to local laws is important to all enterprises. This includes the prevention of child pornography, leakage of PII, and prevention of AI use for violence, harassment, and self-harm.


At the same time, for use cases like AI therapists and AI girlfriends, there is a need to ensure that the model is not censored and follows appropriate user instructions.


Using synthetic data to simulate illegal behavior along with expected good behavior is essential to ensure the right model alignment for a specific use case.
