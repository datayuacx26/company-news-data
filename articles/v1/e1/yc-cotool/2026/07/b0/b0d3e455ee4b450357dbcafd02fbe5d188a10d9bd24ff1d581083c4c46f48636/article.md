---
schema_version: "1.0.0"
document_id: "b0d3e455ee4b450357dbcafd02fbe5d188a10d9bd24ff1d581083c4c46f48636"
company_key: "yc-cotool"
company: "Cotool"
source_id: "yc-cotool-news-import-939d766e0118"
canonical_url: "https://www.cotool.ai/research/nyu-ctf"
published_at: null
first_seen_at: "2026-07-21T15:09:13.770553+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:e1951a9fde6925b26877ee09a03721c37f738b966cf437c31b96afbdf8034992"
---

# NYU CTF Bench

### Accuracy


**Claude Opus 4.6 led with 79% accuracy** , solving 64 of 81 challenges. GPT-5.2 came in next at 63%, with GPT-5.2 Codex close behind at 62%. Gemini 3.0 Flash was a standout at 54%. Within Anthropic's lineup, the gap was stark: Opus 4.6 at 79% versus Sonnet 4.5 at 27% and Haiku 4.5 at 23%. Deeper reasoning ability clearly matters more than speed for these multi-step challenges.


#### Accuracy by Model


Anthropic


OpenAI


Google


Zhipu


Minimax


Qwen


### Cost Efficiency


**Gemini 3.0 Flash offered the best value at ~$0.25/task for 54% accuracy** , over 5x cheaper than GPT-5.2 with only a ~9 percentage point accuracy trade-off. GPT-5.2 was the most cost-effective frontier model at ~$1.23/task for 63% accuracy. Claude Opus 4.6 hit the highest accuracy at $1.73/task, which is reasonable given its 79% solve rate. Some models look cheap on paper but only because they gave up quickly: GPT-OSS-120B cost just $0.04/task but solved only 4%, and Qwen3 235B spent $1.43/task for just 11%. Low cost without accuracy is not efficiency.


#### Cost per Task


OpenAI


Minimax


Google


Anthropic


Zhipu


Qwen


### Speed


**GPT-OSS-120B was the fastest at 84s average, but only solved 4% of challenges** . Being fast means little if the model isn't solving anything. Among models with meaningful accuracy, Claude Opus 4.6 hit a good balance at 556s for 79% accuracy. Claude Haiku 4.5 was quicker at 372s but only reached 23%, and MiniMax M2.1 finished in 423s for 12%. The GPT-5.2 variants were the slowest at 1092 to 1337s, likely spending more time working through each step, which paid off in accuracy.


#### Task Duration (avg)


OpenAI


Anthropic


Minimax


Qwen


Zhipu


Google


### Reliability


**Ten of eleven models completed every task without errors** . The only exception was Claude Opus 4.6, which had 1 unrecoverable error out of 81 tasks (98.8% completion). Every other model, including GPT-5.2, GPT-5.2 Codex, both Gemini variants, Sonnet 4.5, Haiku 4.5, and the open-weight models, finished all 81 tasks cleanly.


#### Task Completion Rate


OpenAI


Google


Anthropic


Zhipu


Minimax


Qwen
