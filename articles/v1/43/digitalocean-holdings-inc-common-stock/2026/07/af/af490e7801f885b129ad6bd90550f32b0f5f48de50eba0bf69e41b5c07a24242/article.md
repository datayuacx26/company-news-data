---
schema_version: "1.0.0"
document_id: "af490e7801f885b129ad6bd90550f32b0f5f48de50eba0bf69e41b5c07a24242"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/now-available-evaluations"
published_at: "2026-07-01T15:41:47.311+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:19d73c765846681f2ca3c07666c826a41718b8b37833cf753b9794da287707a7"
---

# DigitalOcean Evaluations: Production Model and Router Testing for the Inference Stack

Choosing the right model or inference router for production means more than reading a leaderboard. It means validating any model or routing configuration on your own data using your prompts and your evaluation criteria before it ever reaches production, and comparing quality, latency, and cost in one place.


Evaluations, now available on the DigitalOcean Inference Engine, lets teams validate any model or inference router configuration on their own data before production. Run structured LLM-as-a-Judge evaluations across catalog models, fine-tuned models, BYOM imports, and router setups without stitching together a separate evaluation stack.


## DigitalOcean Evaluations Capabilities


Evaluations provide everything teams need to validate model and router performance before production. LLM-as-a-Judge scoring runs across any candidate in your inference stack and returns per-item scores with judge rationale, plus latency, token, and cost tracking per run. Six pre-built metrics cover the most common evaluation needs out of the box. For teams that need full control: custom rubrics, reusable presets, MCP support, and full dataset management — all in the same platform as the inference endpoints you use in production.


**Pre-Built and Custom Rubrics: Score Against Criteria That Match Your Domain**


The six pre-built metrics, correctness, completeness, faithfulness, PII, toxicity, and bias, cover common evaluation needs. For specialized domains, custom rubrics let teams define their own judge instructions and scoring criteria directly in the judge prompt.


The judge evaluates responses against these criteria and returns per-item scores with rationale. Custom rubrics can also adapt the built-in correctness metric to different data formats instead of relying on a default interpretation.


**Evaluation Presets: Save Configurations and Re-Run Without Rebuilding**


Without saved configurations, every re-run becomes a rebuild with different judge models, parameters, or prompts, making results hard to compare.


Evaluation presets store the full configuration of a run including judge model, metrics, system prompt, and parameters, so teams can reuse them across model and router versions and compare results directly across v1, v2, and v3 fine-tunes.


**MCP Support: Trigger Evaluations Programmatically**


For agentic workflows and CI pipelines, evaluations cannot be a manual step in these workflows. MCP support enables evaluation jobs to be triggered programmatically from model registration events, deployment triggers, or schedules.


API and SDK endpoints are also available for teams integrating evaluations into deployment workflows.


**Dataset Management: Manage Evaluation Data as a First-Class Resource**


Datasets can be uploaded, versioned, reused, and deleted in a single place. Each upload creates a versioned dataset linked to evaluation runs for traceability back to the source data.


Datasets support CSV and JSONL formats up to 1GB or 1,000 rows via Console or cURL. Optional ground truth columns can be included to enable faithfulness scoring.


## How to Access Evaluations


Skip the standalone eval tools. Evaluations is natively integrated into your DigitalOcean stack, so you evaluate against the same endpoints you serve on, on infrastructure we run end to end.


Evaluations supports validating any model or inference router in your stack including models from the DigitalOcean Model Catalog, Dedicated Inference endpoints, BYOM imports from Hugging Face or Spaces, and router configurations. All evaluations run against production-grade endpoints.


Evaluations supports a range of judge models, including DeepSeek-R1-Distill-Llama-70B and Qwen3-32B. Access to premium commercial models (OpenAI and Anthropic) as candidates or judges requires a tier 2 account. You can[complete a pre-payment](https://cloud.digitalocean.com/limits?i=b59231) in the Console to move to tier 2 and unlock premium model access.


Billing is based on inference tokens consumed by the candidate and the judge model. Dataset and result storage is provided at no additional charge for the first 12 months.


Your inputs, outputs, and ground truth are sent to the judge model provider for scoring only. They are not stored outside DigitalOcean and not used to train models.


Full documentation, including dataset formatting requirements, preset configuration, and MCP trigger setup, is available[here](https://docs.digitalocean.com/products/inference/how-to/evaluate-models/) .


## Start Evaluating Before You Ship


Model and router decisions don’t stop after launch. Evaluations give you a repeatable way to validate on your workloads, against your criteria, on the same endpoints your users hit, as your stack evolves. Run your first evaluation in the[DigitalOcean Cloud Console](https://cloud.digitalocean.com/model-studio/model-services/model-evaluation?i=01ab2) today.
