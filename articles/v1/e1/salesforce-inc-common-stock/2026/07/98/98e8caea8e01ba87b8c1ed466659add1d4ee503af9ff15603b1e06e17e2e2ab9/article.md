---
schema_version: "1.0.0"
document_id: "98e8caea8e01ba87b8c1ed466659add1d4ee503af9ff15603b1e06e17e2e2ab9"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/how-ai-rebuilt-salesforces-decades-old-localization-pipeline/"
published_at: "2026-07-23T22:59:43+00:00"
first_seen_at: "2026-07-23T23:20:31.665468+00:00"
fetched_at: "2026-07-28T20:33:07.280239+00:00"
content_hash: "sha256:00935234bf0a830770905b233f565f093153c8e718f8213743722cc72d56386f"
---

# How AI Rebuilt Salesforce’s Decades-Old Localization Pipeline

In our Engineering Energizers Q&A series, we highlight the engineering minds driving innovation across Salesforce. Today, we spotlight Teresa Marshall, Vice President of Localization. Teresa’s team delivers every Salesforce release in up to 34 languages. As Salesforce engineering accelerated, **product volume grew by more than 35% between releas** es while localization budgets and release windows remained essentially fixed, forcing a pipeline that had supported Salesforce for decades to deliver significantly more software under exactly the same constraints.


Explore how Teresa’s team reinvented that localization production pipeline with an LLM-powered AI workflow combining prompt engineering, context engineering, AI orchestration, and multi-stage AI validation. Doing so **reduced localization costs by 50 to 90%** (depending on the workflow), dramatically accelerated turnaround times, and established a scalable engineering foundation for future Salesforce releases.


##### **As Salesforce localization demands accelerated while budgets and release windows remained fixed, what made your team’s mission feel almost impossible?**


The mission is to make every Salesforce product feel as though it was built natively for customers around the world. Every major release must ship in[34 languages](https://engineering.salesforce.com/how-agentforce-prevents-language-drift-in-600k-daily-multilingual-ai-workflows/) with the same functionality, quality, and schedule as the English release. Customers should not feel like they are using translated software, rather, they should feel like they’re using their product.


For decades, that mission relied on human translators, established review workflows, specialized linguistic expertise, and carefully coordinated release schedules. The challenge was not that the system was broken, but that nearly everything had already been optimized.


Historically, product-related localization volume at Salesforce grew roughly 10 to 15% between releases. More recently, that growth exceeded 35%, while the time available to translate, validate, test, and ship each release never changed. Documentation and other long-form content had already moved to conventional machine translation years earlier, capturing the obvious efficiency gains. Hiring more translators was not economically sustainable, and extending release windows was not an option. **Every release became a race against a clock that never moved.**


The engineering problem was no longer “How do we translate faster?” It became “How do we reinvent a decades-old enterprise localization approach using AI without compromising quality or delaying a single Salesforce release?”


*AI-based Localization architecture: expanding Translation Management System (TMS) capabilities.*


##### **Why wasn’t replacing human translators with a single LLM prompt enough?**


The obvious assumption is that a modern large language model could simply replace human translation. However, enterprise localization is far more complex than translating text.


Salesforce customers routinely rename objects throughout the platform. An “Account” might become a “Patient” in healthcare, and every surrounding sentence must automatically remain grammatically correct. Those rules differ across 34 languages and become even more complex with customer-defined objects. Translation also has to preserve Salesforce’s voice while respecting regional expectations around formality, branding, industry terminology, and product context. Financial Services Cloud, Health Cloud,[Sales Cloud](https://engineering.salesforce.com/sales-clouds-ai-transformation-welcome-to-the-autonomous-selling-era/) , Slack, and other products all introduce different vocabulary, user expectations, and linguistic conventions.


The difficult part was not getting an LLM to translate text, but giving AI enough context to consistently generate production-ready software across thousands of UI components, dozens of products, and every supported language.


##### **When a single prompt failed, how did your team redesign the architecture instead of simply improving the prompt?**


That realization completely changed the approach. The first instinct was to improve prompt engineering, but every improvement exposed another limitation. A prompt that handled branding might miss grammatical rules. One that solved grammar might lose product context. Another worked for Sales Cloud but produced inconsistent terminology elsewhere.


The breakthrough was combining prompt engineering with context engineering to create an AI orchestration pipeline, not better prompt engineering alone. Rather than sending UI strings through a single translation request, the pipeline orchestrates a sequence of specialized AI stages. Early prompts establish grammatical rules, Salesforce terminology, branding, and language-specific conventions. Additional prompts inject product context so the model understands whether it’s localizing Sales Cloud, Health Cloud, Slack, or another experience. The generated output then flows through dedicated editor prompts before separate validation prompts evaluate terminology, quality, and consistency. Depending on the language, a single translation can move through as many as 85 specialized prompt stages before reaching production.


Context became the real architecture. Instead of presenting isolated strings, the pipeline provides structured information about the surrounding feature, cloud, terminology, and intended customer experience so the model understands not only what it is translating, but why. For example, the pipeline first establishes whether content belongs to Sales Cloud, Health Cloud, Slack, or another product before translation begins, allowing identical interface elements to be localized appropriately for each experience.


The result is not a better translation prompt, but an **AI production pipeline that orchestrates prompt engineering, contextual intelligence, and specialized AI workflows into a scalable localization system capable of supporting enterprise software at Salesforce’s scale.**


##### **How did you prove an AI-generated release could be trusted before it ever reached customers?**


Building an enterprise AI localization pipeline was only half the challenge. The harder question was whether it could be trusted enough to ship production software. Translation accuracy alone was not enough. Every generated string still had to preserve Salesforce terminology, branding, grammar, and customer experience across 34 languages, and the entire workflow needed to perform consistently release after release.


One of the biggest discoveries was that assets localization teams had relied on for decades suddenly became critical AI infrastructure. Style guides, glossaries, terminology databases, branding guidance, and linguistic conventions had originally been written for human translators. The team redesigned those resources so LLMs could consume them as structured context, apply them consistently, and evaluate their own output. Those same assets became foundational to preserving Salesforce’s voice across every release.


Validation became its own AI pipeline. Rather than relying on a single model to determine production readiness, translations move through a workflow of AI generation, AI editing, AI validation, and human review. Each stage evaluates terminology, grammar, consistency, and overall quality before passing content to the next stage. Human reviewers don’t simply approve or reject translations. They continuously refine prompts, contextual data, and validation rules, creating a feedback loop that strengthens every future release.


The engineering challenges extended well beyond AI. Faster generation changed batching strategies, source control, testing, and deployment because the localization system now produced output far faster than the production process it replaced. Replacing translators was not the hardest part; reengineering and proving the production pipeline was.


##### **Now that you have reengineered the pipeline around AI, what engineering challenges will define its next evolution?**


As the LLM-powered localization pipeline continues evolving, one of the biggest challenges is preparing for constant change. Foundation models will continue improving and introducing new capabilities, which means the architecture has to evolve alongside them without rebuilding years of prompt engineering, context engineering, and production workflows every time the underlying model changes.


Operating an enterprise AI localization pipeline is also beginning to resemble operating any other large-scale production system. As AI accelerates engineering productivity across Salesforce, localization demand will continue growing alongside it. That shifts the focus toward familiar engineering problems: throughput, batch processing, database capacity, quality gates, and ensuring content enters the localization pipeline only when it is truly ready for release. Success increasingly depends on release engineering as much as localization engineering.


One phrase became almost a running joke throughout the project: context, context, context. The more the team worked with LLMs, the more it became clear that context engineering mattered just as much as prompt engineering. Product context. Linguistic context. Brand context. Customer context. Release context. Success depended less on choosing the perfect model than on delivering the right context at every stage of the workflow. Context wasn’t simply improving prompt quality. It had become part of the architecture itself.


Perhaps that is the biggest lesson from this journey. AI did not replace decades of localization expertise. It transformed that expertise into prompts, contextual intelligence, validation pipelines, and engineering workflows that AI can execute consistently at enterprise scale. AI systems don’t become production-ready because foundation models improve. They become production-ready because engineers build the workflows, validation, and context that make those models trustworthy at scale. For this team, t **hat means delivering the right content, with the right context, at exactly the right point in every Salesforce release.**


##### **Learn More**


- Stay connected by joining our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) .
- Explore our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to see how you can get involved.
