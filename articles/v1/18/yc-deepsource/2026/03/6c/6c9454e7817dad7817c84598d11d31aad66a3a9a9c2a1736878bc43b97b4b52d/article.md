---
schema_version: "1.0.0"
document_id: "6c9454e7817dad7817c84598d11d31aad66a3a9a9c2a1736878bc43b97b4b52d"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/byok"
published_at: "2026-03-24T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:d6a2953b8fde7de22dfed97803d4d8af60be3e27f4b81c23a00888e1e4ba7c72"
---

# BYOK for AI Review

Last updated on Mar 24, 2026


Enterprise teams considering to adopt AI code review run into the same set of questions: where does inference happen, which model provider do we use, and how does this fit into the cloud agreements we already have in place? We're introducing Bring Your Own Keys (BYOK) on DeepSource Enterprise Server to solve this.


For organizations with restrictions on how their source code is shared with AI model providers, it's now easier to adopt[DeepSource AI Review](https://deepsource.com/blog/deepsource-next) .


## Full control over AI inference


If you have committed spend or negotiated rates with a cloud provider, AI Review runs against that budget. No separate AI line item, no new vendor to approve in procurement. Some teams standardize on Anthropic, others on OpenAI or Gemini, and BYOK lets each team use the model family that matches their requirements, whether technical, contractual, or both.


Swapping providers is a simple config change. Update the API key, and the next PR analysis picks up the new model. There is no migration, no data export, no downtime. Static analysis, SCA, secrets detection, IaC scanning, and the PR Report Card all run identically no matter which LLM sits behind AI Review.


## Supported providers


We're starting with support for three model families:


Model Cloud providers


Anthropic Claude Amazon Bedrock, direct API


OpenAI GPT Codex Azure OpenAI, direct API


Google Gemini GCP Vertex AI, direct API


Configuration requires two model deployments:


- a flagship model that powers Autofix™ and AI Code Review, and
- a smaller, faster model that handles everything else (like generating issue descriptions, filtering, summarization)


Splitting workloads this way keeps token costs down and inference fast, while maintaining quality.


## Security and compliance


With BYOK on DeepSource Enterprise Server, inference calls go directly from Enterprise Server to your model provider, without passing through DeepSource Cloud or any third-party endpoint. If your org has a BAA with Azure OpenAI or a data residency agreement with GCP Vertex AI, those terms govern every AI feature on DeepSource.


This matters for teams operating under SOC 2, HIPAA, FedRAMP, or internal policies that require DPAs with every vendor in the data path. BYOK keeps AI features inside your existing compliance boundary, with no additional agreements to negotiate.


---


BYOK is generally available on all Enterprise Server deployments starting today. If you're already on Enterprise Server, reach out to your point of contact to enable it. If you're interested in bringing AI code review to your team,[Contact sales](https://deepsource.com/contact/sales) to discuss!
