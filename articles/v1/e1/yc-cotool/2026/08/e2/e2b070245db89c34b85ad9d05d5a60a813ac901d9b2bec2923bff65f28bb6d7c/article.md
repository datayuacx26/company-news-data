---
schema_version: "1.0.0"
document_id: "e2b070245db89c34b85ad9d05d5a60a813ac901d9b2bec2923bff65f28bb6d7c"
company_key: "yc-cotool"
company: "Cotool"
source_id: "yc-cotool-news-import-939d766e0118"
canonical_url: "https://www.cotool.ai/research/aws-cloud-intrusion"
published_at: null
first_seen_at: "2026-08-11T08:46:28.234690+00:00"
fetched_at: "2026-08-11T08:46:28.916528+00:00"
content_hash: "sha256:e7697ea8b9533f891ea157364cf345b4df0e26817d2997527db4564ae924d46c"
---

# AWS Cloud Intrusion

### Accuracy


**GPT-5.6 Sol led at 88.3%, narrowly ahead of GPT-5.5 at 87.9%** . Kimi K3 followed at 85.1%, ahead of GPT-5.6 Terra at 84.2%. Sol and Terra passed the critical-fact gate in all three trials; GPT-5.5 and Kimi K3 each passed two of three. These were the only four models to average above 80%.


#### Accuracy by Model


OpenAI


Moonshot


Zhipu


Anthropic


Minimax


Deepseek


### Run-to-Run Spread


**GPT-5.6 Sol was the most consistent of the top models, with only 7.5 percentage points between its 85.0% worst run and 92.5% best run** . GPT-5.5 and Terra were close at 8.8pp, while Kimi K3 ranged 16pp. GPT-5.6 Luna ranged from 35.0% to 81.3% (46.3pp). Haiku 4.5 had the narrowest range of all at 3.5pp, but around a 23% score, so a tight range means little on its own. Gemini 3.6 Flash was the extreme case: 28.1% to 77.5% across three runs, a 49.4pp swing on the same task with the same data.


### Best Open-Weight Result


**Kimi K3 led the open-weight models at 85.1%** , 6.7 points ahead of GLM-5.2 and within three points of the overall leader. Kimi K3 passed two of three trials with a 16pp range. GLM-5.2 was capable but less reliable: 78.4%, a 20.4pp range, and one passing trial.


### Saying What Failed Was the Hardest Part


Across all 16 models, defense evasion and failed expansion were the lowest-scoring phases at under 50%. Models usually found the relevant API calls but rarely said plainly that StopLogging, the higher-privilege role assumptions, and the EC2 launch **all failed** . They also confused the interactive support-role exfiltration with a separate, denied Lambda-role S3 attempt.


### Speed and Cost


**GPT-5.6 Terra was the value pick: 84.2% at about $0.68 per report and 139 seconds** , less than half GPT-5.6 Sol's cost and time for a 4.2-point score trade-off. GPT-5.6 Luna was slightly cheaper at $0.42, but its 46.3pp spread makes the average much less dependable. Kimi K3 reached 85.1% at $2.39/report and about nine minutes. Opus 5 cost $3.33/report for a 24.2% average, dragged down by cyber refusals. Costs count agent-loop tokens only (uncached input, cache writes, cache reads, and output) and exclude the LLM-judge rescoring calls.


#### Cost per Task


Deepseek


Anthropic


OpenAI


Minimax


Zhipu


Moonshot


### Cyber Refusals


**Anthropic models were the only ones to hit cybersecurity refusals at the service level** . Claude Fable 5 refused all three trials and has no score. Opus 5 and Sonnet 5 investigated the case normally, then had their final reports refused; the "Cyber refused" badge on the leaderboard marks models with at least one refused trial, and their averages (24.2% for Opus 5, 22.9% for Sonnet 5) reflect those refusals rather than investigative capability. Opus 4.8 and Haiku 4.5 were not affected.
