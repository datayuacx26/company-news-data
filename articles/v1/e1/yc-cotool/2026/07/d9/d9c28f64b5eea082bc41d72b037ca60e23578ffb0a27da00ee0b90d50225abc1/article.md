---
schema_version: "1.0.0"
document_id: "d9c28f64b5eea082bc41d72b037ca60e23578ffb0a27da00ee0b90d50225abc1"
company_key: "yc-cotool"
company: "Cotool"
source_id: "yc-cotool-news-import-939d766e0118"
canonical_url: "https://www.cotool.ai/research/sigma"
published_at: null
first_seen_at: "2026-07-21T15:09:13.770553+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:00eebcd8243d416778ebd5f19cc4a23779d8693da24cb9900e8dcf0e556021d7"
---

# Sigma Detection Classification

### F1 Score


**Claude Opus 4.5 achieved the highest hierarchical F1 score at 66%** , followed by Sonnet 4.5 at 60% and a cluster of models around 56-57% (Gemini 3.0 Pro, Gemini 3.0 Flash, GPT-5.2). Among open-weight models, **DeepSeek v3.2 led at 48% F1** .


#### F1 Score by Model


Anthropic


Google


OpenAI


Xai


Deepseek


Minimax


### Precision vs Recall


Since ground truth labels are community-contributed and often incomplete, recall is the more reliable metric. It measures coverage of human-intended labels, while precision is penalized when models correctly identify techniques the author simply forgot to tag. **Gemini 3.0 Flash achieved the highest recall at 71%** , followed by Opus 4.5 (69%) and Sonnet 4.5 (69%). Among open-weight models, **DeepSeek v3.2 led with 49% recall** .


### Precision vs Recall


Top-right is best (high precision and recall)


anthropic


google


openai


xai


deepseek


minimax


qwen


### Cost Efficiency


**Gemini 3.0 Flash offered the best frontier cost efficiency at ~$0.39/1000 samples** with 56% F1. Among open-weight models, **DeepSeek v3.2 was remarkably cheap at ~$0.08/1000 samples** with 48% F1, nearly 5x cheaper than Gemini Flash. For highest accuracy, Opus 4.5 cost ~$3.84/1000 samples for 66% F1.


#### Cost per Task


Deepseek


Google


Minimax


Anthropic


OpenAI


Qwen


### Speed


**Haiku 4.5 was the fastest frontier model at 0.75s** average per sample, followed by GPT-5.2 at 1.2s. Among open-weight models, **Qwen3 235B was fastest at 0.97s** , though with lower accuracy (38% F1). DeepSeek v3.2 offered a better speed/accuracy trade-off at 1.9s with 48% F1.


#### Task Duration (avg)


Anthropic


Qwen


OpenAI


Deepseek


Google


Minimax
