---
schema_version: "1.0.0"
document_id: "53be277dee4e4e8ec0e4b0431a9b638b7e718f76b34a66877357cfafed794155"
company_key: "yc-lingodotdev"
company: "Lingo.dev"
source_id: "yc-lingodotdev-news-import-c9a255f52565"
canonical_url: "https://lingo.dev/en/blog/the-localization-api"
published_at: "2026-04-01T00:00:00+00:00"
first_seen_at: "2026-07-24T09:46:09.719150+00:00"
fetched_at: "2026-07-28T21:25:41.488077+00:00"
content_hash: "sha256:4607599f6167d90d37682ebeb7a61bc19e7a17dfab110cd6698600331e0ed074"
---

# The Localization API

Before 2010, accepting a payment online meant applying for a merchant account with a bank. Weeks of paperwork. Credit checks. Minimum transaction requirements. Then you integrated with a payment gateway through an XML API where the test environment barely resembled production. You handled PCI compliance yourself - storing card numbers, managing encryption keys, passing security audits.


Today, a developer drops a few lines of code into a checkout page. Compliance, fraud detection, currency conversion, payouts - handled behind the API. The developer never sees the complexity. The complexity didn't go away. It got encapsulated.


This keeps happening. A category of professional work - previously requiring specialists, vendors, and months of coordination - gets compressed into an API call. The call is simple. What runs behind it is not.


## Professional localization before the API #


Localizing a product into multiple languages meant hiring a translation vendor. The vendor assigned translators - often unfamiliar with the product, the domain, or the existing terminology. You sent a terminology guide. The translator read it, mostly. Translations came back in 5-10 business days. Three terms were wrong. You sent them back for revision. Another 3 days.


Meanwhile, someone needed to manage the glossary, track which strings changed since the last batch, verify that the German translations used the right quotation marks, and confirm that the Portuguese output used European spelling, not Brazilian. This coordination happened across spreadsheets, emails, and Slack threads.


For teams that tried to automate with LLMs - the translation was fast, but the quality assurance wasn't. Every request started from zero. No memory of approved terms. No awareness of brand voice. No verification that the output matched the domain's terminology conventions.


Then LLMs crossed the quality threshold for production translation. Not because they got better at languages — because it became possible to[inject domain context at inference time](https://lingo.dev/en/research/retrieval-augmented-localization) and get consistent, terminology-accurate output. The missing piece was the context pipeline around the model.


## Professional localization behind the API #


One POST. Results arrive via webhook as each language completes.


bash


```text
curl   -X   POST   https://api.lingo.dev/jobs/localization   \
-H   "X-API-Key:   $LINGO_API_KEY  "   \
-H   "Content-Type: application/json"   \
-d   '{
"sourceLocale": "en",
"targetLocales": ["de", "fr", "ja", "ko", "pt-BR",
"es", "it", "zh-Hans", "nl", "sv", "pl", "tr", "ar", "th"],
"data": {
"title": "Introduction to Machine Learning",
"steps": [
{ "heading": "What is ML?", "body": "Machine learning is a subset of artificial intelligence." },
{ "heading": "Supervised Learning", "body": "Training a model with labeled data." }
]
},
"callbackUrl": "https://your-app.com/webhooks/translations"
}'
```


202 back in milliseconds. The caller is free to continue. Each language processes independently through the localization engine. German arrives in 4 seconds. Japanese in 6. Arabic in 8. Each result hits your webhook the moment it's ready. For real-time progress in your UI, connect a[WebSocket](https://lingo.dev/en/docs/api/localization) to the job group - "3 of 16 languages ready" updating live.


The developer never manages the complexity. The complexity didn't go away. It got encapsulated - just like payments did.


## What runs behind the API #


When a localization job starts, the platform runs a multi-step pipeline through the engine's configuration. Six steps, each addressing a specific failure mode that surfaces when you skip it.


1


### Source refinement


An AI agent pre-edits the source text before translation begins. Ambiguous phrasing, inconsistent terminology, culturally loaded idioms - rewritten for translatability. This eliminates the garbage-in, garbage-out problem that degrades every downstream step.


2


### Context enrichment


The engine retrieves the glossary, brand voice, and locale-specific instructions configured for this language pair. Matched glossary terms are injected into the LLM's context window. The model sees the correct term mapping before generating a single token. This is[retrieval augmented localization](https://lingo.dev/en/research/retrieval-augmented-localization) — the step that[reduces terminology errors by 17-45%](https://lingo.dev/en/research/retrieval-augmented-localization) across providers.


3


### LLM translation


The engine selects the highest-priority model for this locale pair from the[configured fallback chain](https://lingo.dev/en/docs/platform/llm-models) . If the primary model fails, the engine routes to the next ranked model automatically. The caller never sees the failover.


4


### Human post-editing


Optional. A qualified translator reviews and corrects the AI-generated draft — focusing on what the model got wrong, not translating from scratch. The platform handles translator sourcing and matches to content domain; results arrive through the same webhook. No vendor management, no approval step in the UI. Turnaround: hours, not weeks.


Enterprise


Human post-editing is available in private beta on enterprise plans.[Contact us](https://cal.com/team/lingodotdev/talk) to enable it for your engines.


5


### AI post-editing


Optional. After the human edit, an AI agent applies a final consistency pass. Formatting validation, glossary re-verification, tone alignment with brand voice - preserving the human translator's intent while enforcing engine-wide standards. The human improves accuracy. The AI enforces consistency.


Enterprise


AI post-editing is available in private beta on enterprise plans.[Contact us](https://cal.com/team/lingodotdev/talk) to enable it.


6


### Back-translation verification


Optional. An independent model translates the output back into the source language. The agent compares the back-translation to the original source, flags semantic divergence, and adjusts segments where meaning shifted during translation. This catches errors that forward-only evaluation misses.


Enterprise


Back-translation verification is available in private beta on enterprise plans.[Contact us](https://cal.com/team/lingodotdev/talk) to enable it.


Each step is configurable per engine. Source refinement, human post-editing, AI post-editing, and back-translation can be enabled or disabled independently. Context enrichment and LLM translation are always on - they are the core of the engine.


A team localizing marketing copy might enable all six steps. A team localizing internal documentation might run context enrichment and LLM translation only. The API call is the same either way. The pipeline adapts.


## Delivery #


Results arrive through the channel you configured - no polling required.


**Webhooks** - each language is delivered the moment it completes. A German translation that finishes in 4 seconds arrives immediately, without waiting for Japanese to finish in 6. Every webhook includes a cryptographic signature following the[Standard Webhooks](https://github.com/standard-webhooks/standard-webhooks) spec. Failed deliveries retry with exponential backoff up to 5 attempts.


**WebSocket** - connect to a job group for real-time progress. Every event includes a full state snapshot: total jobs, completed jobs, failed jobs, per-language status. Your frontend never maintains local state - the server pushes the current truth on every event.


**Failure isolation** - if Japanese fails but German succeeds, the German translation is delivered normally. The failed job appears with an error message. The group status becomes` partial` . Retry by submitting a new request with only the failed locales.


## What this means #


Compliance, terminology governance, domain adaptation, human review, quality scoring - handled behind the API. The developer sends content and target locales. The localization manager configures the engine. The platform runs the workflow.


The coordination that used to require a project manager, a vendor relationship, and a shared spreadsheet now happens inside a durable background workflow with webhook delivery, failure isolation, and real-time progress streaming.


If your localization need is a one-time document translation with no consistency requirements across releases, the API is overbuilt for your needs. The engine's value compounds over time - glossary terms accumulate, brand voice sharpens, quality scores trend upward. Lingo.dev is purpose-built for products that ship continuously, where the same content changes every sprint and consistency across locales is non-negotiable.


[Read the full API reference](https://lingo.dev/en/docs/api/localization) , or[book a demo](https://cal.com/team/lingodotdev/talk) to see the pipeline in action.


## Next steps #


[Async API reference Full endpoint documentation with examples](https://lingo.dev/en/docs/api/localization)[Localization engines Configure the pipeline behind the API](https://lingo.dev/en/docs/platform/engines)[RAL research How context enrichment reduces errors 17-45%](https://lingo.dev/en/research/retrieval-augmented-localization)
