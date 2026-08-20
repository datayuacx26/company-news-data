---
schema_version: "1.0.0"
document_id: "ffc6b38267bf34bf190d3363164123641200d62316c5ff696b9d52564f3c4fbd"
company_key: "yc-cotool"
company: "Cotool"
source_id: "yc-cotool-news-import-939d766e0118"
canonical_url: "https://www.cotool.ai/research/macos-threat-investigation"
published_at: null
first_seen_at: "2026-07-24T11:21:10.833703+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:5af429e7f52baf550ef9f874a17b5f408b7bdfec853f9c40be0e6434c65340ce"
---

# macOS Threat Investigation

### Accuracy (All Tracks)


**GPT-5.4 led at 87%** , followed by Opus 4.6 (84%) and GPT-5.3 Codex (82%). GPT-5.4 was the most consistent across tracks (83-92%), while Gemini 3.1 Pro scored 89% on threat hunting but dropped to 57-58% elsewhere.


#### Accuracy by Model


OpenAI


Anthropic


Zhipu


Google


Moonshot


### Track Breakdown


**Each track rewarded different strengths** . IR was hardest (top score 83%). Gemini 3.1 Pro dominated threat hunting at 89%. GPT-5.4 and Opus 4.6 tied at 92% on detection engineering. One question stumped every model: locating the hidden credential cache path.


### Cost (All Tracks)


**IR was the most expensive track across models** . Kimi K2.5 had the best value at $0.36/task for 68% accuracy, roughly 8x cheaper than GPT-5.4. GPT-5.4 Mini hit $0.23/task on detection engineering while still scoring 71%.


#### Cost per Task


Google


Moonshot


OpenAI


Zhipu


Anthropic


### Speed (All Tracks)


**GPT-5.4 Mini finished tracks in ~5 minutes** . GPT-5.4 and GPT-5.3 Codex averaged ~37 minutes. Gemini 3.0 Flash was slowest at 89 minutes per track.


#### Task Duration (avg)


OpenAI


Moonshot


Google


Anthropic


Zhipu


### Reliability (All Tracks)


**Eight of nine models achieved 100% completion** . Gemini 3.1 Pro had 2 failures out of 40 runs (95%).


#### Task Completion Rate


OpenAI


Anthropic


Zhipu


Moonshot


Google
