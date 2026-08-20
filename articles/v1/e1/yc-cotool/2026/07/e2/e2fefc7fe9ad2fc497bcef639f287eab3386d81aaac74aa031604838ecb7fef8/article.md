---
schema_version: "1.0.0"
document_id: "e2fefc7fe9ad2fc497bcef639f287eab3386d81aaac74aa031604838ecb7fef8"
company_key: "yc-cotool"
company: "Cotool"
source_id: "yc-cotool-news-import-939d766e0118"
canonical_url: "https://www.cotool.ai/research/cybermetric"
published_at: null
first_seen_at: "2026-07-24T11:21:10.833703+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:b6bb24d837fe22a316f41b3e66a7e1033675cac3084a3cf79a4e26eb9fb3f309"
---

# CyberMetric

### Remarkably Tight Spread


**All 13 models scored between 86.9% and 90.8%** —a gap of just 3.9 percentage points from top to bottom. To test whether this reflects memorization, we gave GPT-5 Mini the questions without answer choices and forced a letter-only response: it scored 25.45%, consistent with random guessing (25%). This suggests the high scores reflect genuine cybersecurity knowledge, not dataset contamination.


#### Accuracy by Model


Anthropic


Google


OpenAI


Qwen


Zhipu


Minimax


### Closed vs Open-Weight


Closed models cluster between 88.5–90.8%, open-weight between 86.9–87.8%—a **consistent but narrow ~2pp gap** . Within each group the spread is even tighter: 2.3pp among closed models, under 1pp among open-weight. GPT-5 Mini (89.3%) nearly matches GPT-5.2 (89.4%), suggesting that model scale provides diminishing returns on static knowledge tasks at this saturation level.


### Cost Varies 200x


**Qwen3 Coder Next costs just $0.024 per 1,000 questions** while Claude Sonnet 4.5 costs $4.77—a 200x difference for only 2.4pp less accuracy. **GPT-5 Mini offers the best frontier value** at $0.38/1K questions with 89.3% accuracy, and GPT-OSS 120B delivers 87.3% at just $0.03/1K—making high-quality cybersecurity knowledge evaluation accessible at minimal cost.


#### Cost per Task


Qwen


OpenAI


Minimax


Zhipu


Moonshot


Google


### Reliability


**All frontier models achieved 100% completion** (GPT-5.2, GPT-5 Mini, Sonnet 4.5, Haiku 4.5, Opus 4.6, Gemini 3.0 Pro). Among OpenRouter models, most maintained >99% completion. GLM-4.7 had the lowest at 97.4% with 264 errors. Gemini 3.0 Flash was excluded entirely due to a batch API failure resulting in 0% completion.


#### Task Completion Rate


Anthropic


Google


OpenAI


Minimax


Qwen
