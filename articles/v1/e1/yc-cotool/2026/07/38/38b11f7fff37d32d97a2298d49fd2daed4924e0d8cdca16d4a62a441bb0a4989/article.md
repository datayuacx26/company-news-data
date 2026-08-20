---
schema_version: "1.0.0"
document_id: "38b11f7fff37d32d97a2298d49fd2daed4924e0d8cdca16d4a62a441bb0a4989"
company_key: "yc-cotool"
company: "Cotool"
source_id: "yc-cotool-news-import-939d766e0118"
canonical_url: "https://www.cotool.ai/research/windows-enterprise-intrusion"
published_at: null
first_seen_at: "2026-07-24T11:21:10.833703+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:7593c4d23eb47fb0161b3d9e7fac6221ec14f3f3012965cfb23fa9795766ae14"
---

# Windows Enterprise Intrusion

### Accuracy


**GPT-5.6 Sol set a new high-water mark at 79%, ahead of Opus 4.7 (76%), Opus 4.8 (74%), GPT-5.5 (73%), and GLM-5.2 (73%)** . Track wins still rotated: Opus 4.6 topped guided investigation (87%), GPT-5.5 wrote the best IR report (76%), and GPT-5.6 Sol wrote the best threat hunting report (81%). GPT-5.6 Terra reached 70% overall at substantially lower cost, while GLM-5.2 remains the strongest open-weight result at 73%. Claude Fable 5 could not be evaluated because Anthropic's cybersecurity guardrails were triggered on all 40 tasks.


#### Accuracy by Model


OpenAI


Anthropic


Zhipu


Moonshot


### Synthesis Is Harder Than Retrieval


Nearly every model scored worse writing open-ended incident reports than answering targeted questions about the same data. **Opus 4.6 hit 87% on guided investigation but only 43-44% on reports** : it could answer specific questions about the intrusion, but covered less than half of the expected facts when asked to write the report without a question list. The best IR report (GPT-5.5) still covered only ~76% of the hidden rubric.


### Detection Rules That Actually Run


Because every rule is re-executed against the dataset, the detection-engineering questions separate models that write plausible-looking SQL from models that validate their work. **Single-event rules (string matches on command lines) were near-ceiling; multi-event correlation rules were the hardest tasks in the benchmark** . Rules requiring a join between a service-install event and subsequent network telemetry, or an anti-join against prior execution history, averaged 19-40% across all models.


### Speed and Efficiency


**Opus 4.8 finished tasks in ~2.3 minutes using ~13 tool calls on average, the fewest of any frontier model** . Gemini 3.5 Flash took 44 tool calls per task and 200M+ tokens over the full run to finish mid-table. Tool-call volume didn't predict score: on the same report sessions, GLM-5.2 made roughly three times GPT-5.5's tool calls with a third of the context per call and landed on nearly identical total tokens. What separated the strong runs was query precision, aggregating and filtering in SQL instead of paging through raw logs.


#### Task Duration (avg)


OpenAI


Anthropic


Minimax


Google


### Cost


**GPT-5.6 Sol delivered the highest score at ~$1.22/task, while GPT-5.6 Terra reached 70% at ~$0.46/task** . GPT-5.6 Luna cost ~$0.16/task for 60%, below the prior budget tier. Prompt caching is decisive in these long agent loops: most input tokens are cache reads billed at one tenth of the corresponding long-context input rate.


#### Cost per Task


OpenAI


Qwen


Moonshot


Deepseek


Anthropic


Minimax


Zhipu


### Reliability


**Gemini 3.1 Pro dropped 15% of its tasks to unrecoverable errors, and Kimi K2.6 dropped 12%** ; four others lost a task or two (Qwen3.6 Plus, Gemini 3.5 Flash, GPT-5.4, GPT-5.4 Mini).


#### Task Completion Rate


OpenAI


Anthropic


Zhipu


Moonshot
