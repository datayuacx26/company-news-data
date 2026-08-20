---
schema_version: "1.0.0"
document_id: "a8a17d389755f0337bcca4ddbb7c422efcc10a419b118bd3a0d87828e91a0c60"
company_key: "yc-confident-ai"
company: "Confident AI"
source_id: "yc-confident-ai-news-import-3974c9e901d3"
canonical_url: "https://www.confident-ai.com/blog/launch-week-q2-2026-day-4-synthetic-data-generation-pipeline"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-21T14:44:45.986766+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:704b57dd9711a5e4240102916e1c6537344033306794a8c39bfc0b5d520c5543"
---

# Introducing Synthetic Data Generation Pipelines: Customize how you generate data

Today we're launching **Synthetic Data Generation Pipelines** on Confident AI: a simple way to bring your team's synthetic data generation setup onto the platform without losing the flexibility that made it useful locally.


Synthetic data is most useful when it is grounded in the same information your AI uses in production. Now you can build that process as a pipeline: pick your data sources, configure the generation steps, and let Confident AI turn that context into evaluation-ready examples.


app.confident-ai.com


Configure a synthetic data generation pipeline by choosing the data sources and generation steps that fit your use case.


## The problem: local pipelines did not consolidate well


The teams building the best synthetic datasets usually already had something working locally. They had custom scripts, hand-picked sources, prompt templates, filters, evolution steps, and styling logic that matched their use case.


The problem started when that setup needed to become shared infrastructure. Once teams tried to consolidate generation on a platform, they often had to flatten everything into a rigid workflow: one source, one generation path, one way to produce examples.


That made the platform easier to centralize, but worse for the teams actually generating data.


## Mix and match your context


Every team keeps its source material in different places. Product docs might live in Google Drive, customer records in Salesforce, warehouse data in Snowflake, and domain-specific media in a custom corpus.


Synthetic Data Generation Pipelines let you bring those sources together instead of forcing generation to start from a single bucket of text.


Use sources like:


- **Snowflake** for structured warehouse data
- **Salesforce** for customer and business context
- **Google Drive** for docs, specs, and knowledge files
- **Custom corpora** for images, video, or domain-specific source material


Choose the sources that matter for a dataset, then reuse that setup whenever you need more examples.


## Customize each generation step


The pipeline gives you control over how data is created, not just where it comes from.


You can configure the maximum number of concurrent generation requests, decide whether expected outputs should be generated alongside each golden, and tune the downstream steps that shape the final dataset.


The flow is straightforward:


1. Select the data sources to pull context from
2. Construct context from those sources
3. Filter the generated candidates for quality
4. Evolve the examples into useful variations
5. Apply the final styling for your dataset


That means you can generate examples for support bots, internal search, sales assistants, document QA, multimodal workflows, and any other AI system that depends on real context.


## From source material to eval data


The goal is simple: make it easier to create datasets that actually resemble production, while preserving the generation logic your team already knows works.


Instead of keeping that logic trapped in local scripts, you can define the generation pipeline once on Confident AI and keep improving it as your eval needs change.


## Get started


Synthetic Data Generation Pipelines are live on Confident AI now.


Open your project, create a generation config, and start by selecting the data sources you want Confident AI to use.


---


Do you want to brainstorm how to evaluate your LLM (application)? Ask us anything in our[discord](https://discord.com/invite/a3K9c8GRGt) . I might give you an "aha!" moment, who knows?


## Standardize AI Quality for the entire org, not just individual teams


Give all AI use cases the same quality bar with all-in-one evals, observability, and red teaming, and enforce them at scale.


AI evals for product teams, not just engineers.


Observability for production traffic.


Red teaming for security and safety.


AI governance for multiple projects at once.


[Book a Demo](https://www.confident-ai.com/book-a-demo?utm_source=blog&utm_medium=content&utm_campaign=q2_2026_launch_week&utm_content=product)[Or sign up](https://app.confident-ai.com/auth/signup?utm_source=blog&utm_medium=content&utm_campaign=q2_2026_launch_week&utm_content=product)
