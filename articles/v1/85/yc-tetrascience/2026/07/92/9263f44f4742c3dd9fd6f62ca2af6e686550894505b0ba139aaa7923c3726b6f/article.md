---
schema_version: "1.0.0"
document_id: "9263f44f4742c3dd9fd6f62ca2af6e686550894505b0ba139aaa7923c3726b6f"
company_key: "yc-tetrascience"
company: "TetraScience"
source_id: "yc-tetrascience-news-import-63b926bd0a66"
canonical_url: "https://www.tetrascience.com/blog/bring-your-own-model-tetra-os"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-08-01T00:12:49.265905+00:00"
fetched_at: "2026-08-01T00:12:50.675114+00:00"
content_hash: "sha256:bb567ada12bc79bde2b03fc64c51f8c588ee520922cb1bbdf77aca14b5843b73"
---

# Run Your Models Where Your Data Lives with Tetra BYOM

If you've released a custom model on your company’s biopharma data, you already know the friction. You spent months training it on proprietary data. But getting it into production has meant extracting that sensitive data, shipping it to shadow infrastructure for inference, and spending weeks justifying the output to regulators — with provenance already lost and the audit trail broken. Scientists end up doing DevOps and Quality work on models they built themselves, because there's no other path.


Tetra’s new Bring-Your-Own-Model (BYOM) service fixes that problem by doing one thing: it lets you register a model you already built and trained inside the Tetra OS, so the model runs next to your data instead of the other way around. You point BYOM to where the model lives; it gets a governed, authenticated endpoint, and from there it's callable from any AI workflow on the platform, exactly like a foundation model would be. No export step. No shadow infrastructure. The model comes to where the data already sits inside the Tetra OS and its Scientific Data Foundry.


**How it works**


BYOM connects to wherever your model actually lives today:


- Private Hugging Face repos, via a repository key
- Databricks Unity Catalog for internal model registries.
- Inference post-model registration and inference endpoints


For our first BYOM release, we prioritized registration via a command-line interface (CLI) + a YAML config file or Docker image. Once registered, the model gets an authenticated endpoint and appears in the workflow layer the same way a foundation model does, with the same pattern and telemetry. There's no separate "custom model" path at that layer. A model you trained and a foundation LLM look identical to whatever's calling them.


**What's governed today, and what's coming**


Every model goes through scan-gated promotion before it's usable. Nothing reaches the endpoint unscanned. The endpoint itself is authenticated. Every request and response is captured in Tetra OS's round-trip audit trail, the same one that already covers every other AI interaction on the platform, including the evaluation framework that generates compliance-ready artifacts like software hazard analyses.


While we have developed deep observability (model inference input and output) and lifecycle management across model versions, we know that the next evolution will need automated audit tooling. What's already in place today is enough to run a model in a regulated environment with a defensible audit story; what's coming makes that story easier to modify, produce, and easier to scale across more models.


**Exporting data breaks audits**


Manual data movement or curation presents a substantial challenge to audits, where data explainability, and thus lineage, is critical. Even if you can trace a model output to its paired input, do you possess the provenance of that model input data? Data leaving your environment with nothing tracking where it went; model inputs and outputs with no lineage back to the request that produced them; and a regulated result you can't actually reproduce six months later when someone asks how you got it – these are the risks to your science and process without the proper data backbone. TetraScience’s job is to make sure there's an answer. We built this, and it’s working in production.


**The agent-driven model library**


BYOM is a piece of a larger architectural foundation. We aim to enable a scientist to ask a question in plain language, and then have our agent route the input to the most appropriate model in your environment, whether it's ours or yours. The scientist doesn't need to know or care which model responded – our audit-imbued infrastructure will track it the same either way.


None of that works without BYOM. An agent can't route to a model that isn't registered, and it can't be trusted to route to a model that isn't a governed object in the platform to begin with. As LLMs keep getting more capable, that governance boundary matters more, not less. Companies are going to want to expose more through APIs while keeping the underlying data and IP inside a boundary they control.


**Getting access**


BYOM is in limited early release right now, activated in coordination with our team rather than fully self-service on day one. Ask your Tetra engagement team to get you access. After that one-time activation, it's self-service.


If you've got custom models sitting in private repos that need to run against your governed scientific data, we’d love to start the conversation on how we can help you move them into production.


[Request a demo →](https://www.tetrascience.com/demo)
