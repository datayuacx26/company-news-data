---
schema_version: "1.0.0"
document_id: "cb247142ebe6be1488c9ab6db164444de828955149ed26b3fb0a43fb5a617e8b"
company_key: "yc-cotool"
company: "Cotool"
source_id: "yc-cotool-news-import-939d766e0118"
canonical_url: "https://www.cotool.ai/research/botsv3"
published_at: null
first_seen_at: "2026-07-24T11:21:10.833703+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:e0199f410e00f465565f63a5134aaa3cbd5d63701ec1f71129df12c63a2435a5"
---

# BOTSv3 Blue Team CTF

### Accuracy


**GPT-5.2 achieved the highest accuracy at ~69%** , followed by GPT-5.1 and Opus 4.5 at 65%. GPT-5 and Sonnet 4.5 scored 63% and 61% respectively. Among open-weight models, Qwen3 Coder led at 43%, while MiniMax M2 and GPT-OSS-120b ranged from 25-29%.


#### Accuracy by Model


OpenAI


Anthropic


Google


Qwen


Minimax


### Speed


**Opus 4.5 was the fastest competitive model** at just 113s average, roughly half the time of Haiku 4.5 (240s), despite presumably being a larger model. This suggests reasoning efficiency can outweigh raw inference latency in long-horizon agentic tasks.


#### Task Duration (avg)


OpenAI


Deepseek


Anthropic


Qwen


Minimax


Google


### Cost


**GPT-5.1 delivered 65% accuracy at just ~$1.67/task** , the best cost-to-accuracy ratio among frontier models. GPT-5.2 cost ~$4.03/task for 69% accuracy, while Opus 4.5 cost ~$5.14/task for 65%. Among open-weight models, Qwen3 Coder offered strong value at ~$0.22/task (43% accuracy), with MiniMax M2 and GPT-OSS-120b even cheaper at ~$0.15 and ~$0.10/task respectively.


#### Cost per Task


OpenAI


Deepseek


Google


Minimax


Qwen


Anthropic


### Token Efficiency


**Among frontier models, GPT-5 was the most token-efficient at ~793K per task** , while Sonnet 4.5 consumed ~2.4M, over 3x more. GPT-5.2 (2.1M) and GPT-5.1 (1.2M) were notably less efficient than their predecessor. Among smaller models, GPT-5 Mini led at just ~87K tokens per task, followed by DeepSeek v3.2 (126K) and GPT-OSS-120b (198K).


#### Token Usage per Task


OpenAI


Deepseek


Google


Anthropic


Minimax


### Reliability


Most models achieved **100% task completion** , including GPT-5.2, GPT-5.1, GPT-5, Sonnet 4.5, Haiku 4.5, GPT-5 Mini, Qwen3 Coder, MiniMax M2, GPT-5 Nano, and DeepSeek v3.2. However, some models suffered from many unrecoverable errors, particularly GPT-OSS-120b (69% completion) and the Gemini models (Gemini 3.0 Pro at 92%, Gemini 2.5 Pro at 84%, and Gemini 2.5 Flash at 88%). This suggests potential struggles with long-context log investigation tasks.


#### Task Completion Rate


OpenAI


Anthropic


Qwen


Minimax


Deepseek
