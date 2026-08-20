---
schema_version: "1.0.0"
document_id: "f10d5256de1288c4f7dc8b7a97ec2cc8cf4cced61bd338ae314b812e35f99e1d"
company_key: "yc-bayes-impact"
company: "Bayes Impact"
source_id: "yc-bayes-impact-news-import-46ce02fd9463"
canonical_url: "https://www.bayesimpact.org/blog/sovereign-ai-public-services/"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T19:26:18.828150+00:00"
fetched_at: "2026-07-31T19:26:20.476384+00:00"
content_hash: "sha256:44daff8ccdda412a344bbbac941c1c95f9635bf1162c65c05c3d62d884cd6ed9"
---

# Sovereign AI for public services: running open models on infrastructure you own

Sovereign AI means an institution runs its AI on infrastructure it controls, with models it can inspect, and data that never leaves its walls. For many of our government partners this is not a preference, it is a hard line: they cannot send citizen data to a cloud API, full stop. So we built the[Bayes Platform](https://www.bayesimpact.org/ai-agents-for-public-services) to run entirely on open models the institution hosts itself. Here is why that matters, and how we do it in practice.


## Why sovereignty is a requirement, not a feature


Public institutions hold data they are bound to protect: health records, employment histories, legal files. For a hospital or an employment agency, handing that to a third-party model is often simply not allowed. And even where it is allowed, it creates a dependency no public body should sign up for: a price that can change, an API that can be deprecated, a provider that owns the relationship with your citizens.


Sovereign AI removes the question. The model runs on the institution’s own infrastructure, the data stays in-house, and the code is open so their teams can verify exactly what it does. Trust stops being a promise and becomes something you can inspect.


## Open models are finally good enough


The reason this works now is that open models got good. Our engineers run Gemma 4 27B and Mistral Small 3.1 24B on vLLM, on dedicated infrastructure, and for the tasks public services actually need (answering from validated documents, filling forms field by field, citing every source) they hold their own.


They even make us better. When we tested our tools against a mid-range open model, its literalness exposed design flaws the larger cloud models had been quietly forgiving. Our CTO wrote up that lesson in[why we test every tool with a mid-range model first](https://www.bayesimpact.org/blog/tool-calling-open-models) . Weaker, stricter models are honest testers.


## What sovereignty looks like in practice


Documents are extracted, chunked and embedded on the institution’s own stack, then stored in its own database. When an agent answers, it retrieves from that store and grounds every response in the organization’s validated sources, with nothing sent to an outside API. The same platform that runs on Google Cloud for a partner who wants it runs entirely self-hosted for a partner who cannot.


## Open-source is half the sovereignty


Sovereignty is not only where the model runs, it is whether you can walk away. The Bayes Platform is MIT-licensed: a partner can deploy it, change it, and take it over without us. Every use case joins a shared digital commons that other public institutions can reuse. There is no lock-in, because lock-in is the opposite of what a public service should accept.


## Why this matters for the mission


We are a nonprofit. Our success is measured by the impact we spread, not by how dependent our partners become on us. Sovereign, open-source AI is how that belief becomes technical reality: France Travail, the AP-HP and justice archives in Togo run agents grounded in their own knowledge, on terms they control. For the fuller picture of how these agents are built and governed, we laid it out here:[AI agents for public services](https://www.bayesimpact.org/ai-agents-for-public-services) .


Running open models on sovereign infrastructure was harder a year ago. It gets easier every month, and we are building for that curve, not around it.
