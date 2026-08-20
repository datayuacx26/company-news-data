---
schema_version: "1.0.0"
document_id: "9e30856b368322dfb1dbf0a218d67eaff0c2fe31b33a183c805c8515051073f1"
company_key: "yc-lingodotdev"
company: "Lingo.dev"
source_id: "yc-lingodotdev-news-import-c9a255f52565"
canonical_url: "https://lingo.dev/en/blog/introducing-lingodotdev-v1"
published_at: "2026-03-15T00:00:00+00:00"
first_seen_at: "2026-07-24T09:46:09.719150+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:2b9a1a8e09d8d53a198a1de8f74f469b60da186f38bfc459479d7727259e28fd"
---

# Introducing Lingo.dev v1.0

🎉


Lingo.dev v1.0


Every localization team knows the pattern. Translators unfamiliar with the product get brand terms wrong. LLM wrappers have no memory across requests. After a few releases, terminology drifts - and nobody notices, because holistic quality scores say 0.95 and move on.


We measured the gap.[Injecting glossary context at inference time reduces terminology errors by 17-45%](https://lingo.dev/en/research/retrieval-augmented-localization) across five LLM providers and five European languages. We call this retrieval augmented localization (RAL). LLMs crossed the quality threshold for production translation - but only with the right context pipeline. Without it, every isolated request is a fresh opportunity for terminology drift. At Lingo.dev, after providing localization infrastructure for translating 200M+ words for customers like Mistral, Solana, SoSafe, and Cal.com, we built` v1.0` around it.


## Localization engine #


A[localization engine](https://lingo.dev/en/docs/platform/engines) is a stateful translation API on Lingo.dev - a RAL implementation that persists domain context across every request. Configure once, call everywhere:


- **[Models](https://lingo.dev/en/docs/platform/llm-models)** - pick any model from the OpenRouter catalog. Rank models per locale with fallback chains - when the primary model is unavailable, the engine routes to the next without dropping the request.
- **[Glossary](https://lingo.dev/en/docs/platform/glossaries)** - map source terms to target translations per locale pair. The engine injects matching terms into every request. When Cal.com's engine encounters "Workspace," the glossary resolves it before the LLM generates a single token - no translator onboarding required.
- **[Brand voice](https://lingo.dev/en/docs/platform/brand-voices)** - define tone and register per locale. Formal for German legal content, conversational for Japanese marketing, technical for English documentation.
- **[Instructions](https://lingo.dev/en/docs/platform/instructions)** - set per-locale rules for specific patterns: French elision rules, Portuguese spelling conventions, German quotation marks, Italian anglicism preferences.


The engine is stateful. Every glossary term, every brand voice rule, every instruction persists across requests. The first translation through a new engine benefits from zero context. The thousandth benefits from everything the team has configured since day one. In diff-based CI/CD workflows, each build retranslates only what changed. The engine's statefulness is what prevents terminology drift - the same drift that plagues both human translators and stateless LLM wrappers.


Test configurations in the[playground](https://lingo.dev/en/docs/platform/playground) before they go live. Try a string, compare across locales, push to production when it's right.


## How you call it #


Three integration paths. Same engine, same context, same quality:


**CLI** - point at your repo, translate all configured locales in one command:


bash


```text
npx   lingo.dev@latest   run
```


**CI/CD** - the[GitHub integration](https://lingo.dev/en/docs/workflows/github) opens a pull request with translated strings on every push. Review translations in the diff, merge when ready. No handoffs. No waiting for an external team.


**API** - call the engine directly from backend code:


bash


```text
curl   -X   POST   https://api.lingo.dev/process/localize   \
-H   "X-API-Key:   $LINGO_API_KEY  "   \
-H   "Content-Type: application/json"   \
-d   '{
"engineId": "eng_abc123",
"sourceLocale": "en",
"targetLocale": "es",
"data": {
"welcome": "Welcome to the future of payments",
"cta": "Get started"
}
}'
```


Every request is logged: model used, tokens consumed, fallback status, glossary terms applied, instructions matched. The[reports dashboard](https://lingo.dev/en/docs/platform/reports) surfaces word volume, token consumption, top locales, glossary coverage, and change rates - so you can see exactly which terms are being enforced and where coverage gaps remain.


## Model freedom #


Terminology consistency shouldn't depend on which LLM vendor you happen to use. RAL separates the consistency layer - glossary, brand voice, instructions - from the model that executes the translation. The enrichment context steers any model toward domain-accurate output. Swap GPT-5.4 for Claude Opus between releases without reconfiguring a single glossary term.


This separation also eliminates vendor lock-in. Closed-model translation APIs bind glossary, style rules, and terminology to one provider. A model regression means a support ticket. A better model from another provider is out of reach. On Lingo.dev, the model is a configuration parameter - the consistency layer persists regardless of which model runs underneath.


When we[tested five providers on the EU AI Act](https://lingo.dev/en/research/retrieval-augmented-localization) , Mistral with a 72-term glossary (MQM 0.940, where 1.0 is error-free) approached Google's raw quality (0.938). The cost difference is an order of magnitude. A well-built glossary reduces how much intelligence the model needs. As glossaries mature, the platform flags when a cheaper model would match the same quality threshold.


## Quality scoring #


Fixing terminology drift is only half the problem. The other half: standard quality metrics can't even see it. Our[research](https://lingo.dev/en/research/retrieval-augmented-localization) found that holistic translation quality scores - the kind used by benchmarks and leaderboards - reported identical scores for raw and glossary-augmented translations, while dimensional scoring counted 17-45% fewer terminology errors. The gap that RAL closes is invisible to the metric the industry uses to measure translation quality.


A problem you can't measure is a problem you can't fix. Lingo.dev v1.0 ships with[AI Reviewers](https://lingo.dev/en/docs/platform/ai-reviewers) - automated quality checks that evaluate each translation by dimension, not by a single holistic number. Define criteria in natural language: "Are all HTML tags preserved?" or "Rate naturalness for a native speaker." An independent LLM evaluates the output - if GPT-5.4 translates, Claude Sonnet scores, eliminating self-assessment bias.


Cross-model, dimension-specific evaluation catches what holistic scores miss: hallucinated terms, shifted register, broken placeholders, and the terminology drift that accumulates silently across releases.


## What this means #


**If you're already using Lingo.dev** - the CLI, CI/CD, and MCP tools keep working as before.` v1.0` adds engine configuration: model selection, fallback chains, brand voice, glossaries, quality scoring. All per locale. All from the dashboard. Zero migration.


**If you're new to Lingo.dev** -[create a free developer account](https://lingo.dev/en/orgs/~) and get a pre-configured localization engine. First translated build in 4 minutes with the[CLI](https://lingo.dev/en/docs/cli/quickstart) , or integrate via the[API](https://lingo.dev/en/docs/api) .


Your next release ships to every locale with consistent terminology. The glossary you build in week one enforces every brand term across every language, every build - the configuration cost is front-loaded, not multiplied per translation. When a developer changes a paragraph, the CI pipeline retranslates just that paragraph - same engine, same glossary, same quality threshold. Translation stops being a handoff. It becomes a build step.


Lingo.dev v1.0 is purpose-built for engineering teams, localization managers, and product leads who need localization to behave like infrastructure. Teams that prefer coordinating human translators through manual review rounds may find traditional localization platforms a better fit.


[Create a free developer account](https://lingo.dev/en/orgs/~) to get started, or[book a demo](https://lingo.dev/en/demo) .


RAL is to localization what RAG is to generation. Lingo.dev v1.0 is where you run it.


## Next steps #


[Create an engine Configure your first localization engine](https://lingo.dev/en/docs/platform/engines)[API reference Integrate via the localization API](https://lingo.dev/en/docs/api)[RAL research The study behind v1.0](https://lingo.dev/en/research/retrieval-augmented-localization)
