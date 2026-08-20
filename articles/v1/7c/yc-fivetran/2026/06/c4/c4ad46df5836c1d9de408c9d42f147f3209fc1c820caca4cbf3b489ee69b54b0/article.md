---
schema_version: "1.0.0"
document_id: "c4ad46df5836c1d9de408c9d42f147f3209fc1c820caca4cbf3b489ee69b54b0"
company_key: "yc-fivetran"
company: "Fivetran"
source_id: "yc-fivetran-news-import-dafa91bc199f"
canonical_url: "https://www.fivetran.com/blog/fivetran-dbt-an-open-agent-ready-future-for-data-teams"
published_at: "2026-06-01T00:00:00+00:00"
first_seen_at: "2026-07-23T09:42:24.979492+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:9f62205d5bc6c8069e3449d40b00aa9b8fc085f37ad96b2fe397fa2aec11b892"
---

# Fivetran + dbt: An open, agent-ready future for data teams

When Fivetran and dbt Labs came together, we saw an opportunity to do more than bring two products closer together. We saw a chance to define the next generation of data infrastructure: open, interoperable, trusted, and ready for agents.


AI is already changing how data teams work. Agents are writing code, investigating issues, reasoning across systems, and automating more of the analytics engineering lifecycle. But for agents to be useful, they need more than access to data. They need context: lineage, contracts, state, performance signals, and interoperability built into the foundation they operate on.


That is what Fivetran and dbt Labs are building together:[Open Data Infrastructure](https://www.fivetran.com/odi) for the AI era. It gives practitioners more flexibility, gives agents the context they need to do meaningful work, and gives organizations a more reliable way to move, manage, and transform data across tools.


Our commitment starts with the people who build and operate modern data systems: analytics engineers and data developers. In recent months, Fivetran has contributed[SQLMesh](https://www.fivetran.com/press/fivetran-contributes-sqlmesh-to-the-linux-foundation-to-advance-open-data-infrastructure) to the Linux Foundation to advance transparent, repeatable data transformation. We’ve also become the[steward](https://www.fivetran.com/press/fivetran-to-become-steward-of-the-great-expectations-open-source-community-and-gx-core-project) of the Great Expectations open source community and GX Core project, supporting the continued development of open source data quality frameworks.


Now, we’re bringing that joint product vision to life with a set of innovations across the data lifecycle, including replication, transformation, developer productivity, and AI-assisted workflows.


\[CTA_MODULE\]


## What we're launching and why it matters


### dbt Core v2.0: The new open source standard for data transformation


The dbt open source ecosystem is one of the largest and most active communities in data, with more than 1 billion PyPI downloads and more than 100,000 weekly active projects. From solo practitioners to the largest enterprise data teams, analytics engineers rely on dbt to transform data and build trusted analytics.


That foundation is getting a major upgrade.


Today, dbt Labs is open sourcing the Fusion runtime and releasing it as[dbt Core v2.0](https://docs.getdbt.com/blog/dbt-core-v2-is-here) under the Apache 2.0 license. This is a meaningful shift as commercial investment in dbt will now directly improve the open source distribution, rather than advancing in parallel. With dbt Core v2.0, the Rust-based engine comes into the open, bringing up to 10x faster parse times, better scalability for growing projects, a cleaner adapter contribution model, and a more modern docs experience.


Open sourcing the Fusion runtime reinforces our commitment to the Open Data Infrastructure era. Together, the Fusion runtime and Rust-based engine make dbt Core more performant, scalable, and a stronger long-term foundation for the teams that depend on it.


On June 1, dbt Core v2.0 launches in alpha in the same dbt-core GitHub repository. For users who want faster parsing but are not ready for a major version upgrade, dbt Core v1.12 launches in beta with the Rust parser. The dbt Fusion engine extends dbt Core v2.0 with richer capabilities for modern development workflows, including SQL comprehension, column-level lineage, instant feedback, and high-performance SQL linting.


\[CTA_MODULE\]


### dbt State: Build what's changed, skip what hasn't


For teams running dbt at scale, warehouse costs can be persistent and difficult to understand. Production runs often rebuild every model, even when the underlying data or code has not changed. The result is a tax on reliability and a cost center that compounds as your project grows.


[dbt State](https://www.getdbt.com/product/dbt-state) addresses this with an easy-to-use plug-in that brings state awareness, orchestration, and caching into a single cross-surface experience across dbt Core and the dbt platform.


It checks warehouse metadata and model SQL to determine what has actually changed, then builds, skips, clones, or defers each model accordingly. On average, dbt State reduces dbt-generated compute by more than 30% across production runs, without sacrificing data quality or reliability. In development, analytics engineers can iterate faster and spend less compute by running only what needs to run.


> "Before dbt State, every job rebuilt every model in the lineage. Every single time. Now, with dbt State, dbt checks if source data changed. If it didn't, the model is skipped. For us, that resulted in a 9% compute reduction, 35% fewer models built, and a 15% reduction in Snowflake backfill costs."
> — Parag Shah, VP of Data at CarGurus


What makes dbt State especially powerful is where it works: locally, in external orchestrators, on the dbt platform, in Core, and in the terminal. It’s available across dbt versions and distributions, making it useful for both analytics engineers and agents. Agentic workflows get the same efficiency, context, and guardrails that human developers do.


On June 1, dbt State launches in preview and will be available to all customers, regardless of where they do their analytics work.


\[CTA_MODULE\]


### dbt Wizard: Your personal dbt agent, wherever you work


AI coding assistants have advanced quickly over the past couple of years, but most were built for general-purpose software development. In analytics engineering, that is not enough. Generic tools can generate plausible SQL that breaks silently in production. They can suggest changes without understanding downstream tests, lineage, contracts, or defined metrics. They lack the context that makes analytics engineering different.


[dbt Wizard](https://www.getdbt.com/product/dbt-wizard) is built specifically for the analytics engineering lifecycle: asking questions, investigating issues, understanding projects, making changes, validating results, and shipping with confidence. Of the latest set of 75 ADE-bench tasks, dbt Wizard scores 76% and showed significant improvement on hard tasks over other agentic systems. The native understanding of dbt projects dramatically improves agent performance, especially as project sizes increase. The dbt Wizard is grounded in real dbt project context through a native dbt metadata engine derived from the complete artifact set. Because dbt Wizard is purpose-built for dbt work, it can reason about outcomes, not just generate syntax.


> “Before dbt Wizard, our engineers were spending more time correcting AI output than they were writing models. Now the agent actually knows our project. It gets the joins right, it respects our contracts, and it doesn't break things downstream. We've seen a 15-20% reduction in production incidents since we rolled it out."
> — Erion Krasniqi, Junior Data Scientist at Endress+Hauser Infoserve


dbt Wizard is available in the dbt platform and through the dbt Wizard CLI in the terminal. In the platform, it offers a dedicated, chat-first workspace for teams that want an integrated experience. In the terminal, the CLI brings the full loop to local development, whether a team uses the dbt platform or self-hosts dbt. Query results, execution history, and native data reasoning all live in one place. The result is higher productivity, more efficient pipeline development, and stronger analytics engineering workflows.


On June 1, dbt Wizard in Studio launches in public preview. The standalone Wizard tab and CLI launch in public beta.


\[CTA_MODULE\]


### Fivetran AI Connector Agent: New connectors in minutes, not months


One of the most persistent challenges in data replication is the long tail of SaaS and API sources that teams need but that do not yet have a prebuilt managed connector. Too often, that forces teams to build and maintain bespoke pipelines for niche source systems.


Fivetran is addressing that challenge with the AI Connector Agent. Now in beta, the AI Connector Agent generates Fivetran-managed connectors for API sources directly from API documentation in minutes. It crawls the documentation, parses and analyzes the API structure, generates the connector, then validates and refines it. Humans stay in the loop to review and resolve anything the agent does not handle cleanly.


The output is a real Fivetran-managed connector, not code the customer has to own, operate, or maintain.


\[CTA_MODULE\]


### Agents Schema: A standard context layer for AI


To support open standards for agentic work, Fivetran is releasing Agents Schema. Agents Schema lets users designate a schema in their warehouse or lake as the context layer agents can read from. It can include metric definitions, semantic models, dbt lineage, and custom business documentation, all stored in plain SQL tables.


> “The hardest part of business context is keeping it accurate as the underlying data changes. The Agents schema is an elegant and practical answer: definitions, ownership, and quality signals sit next to the models they describe, version-controlled with them. That removes the usual drift between data and documentation and makes context something engineering teams can maintain as part of their normal workflow.”
> — Petyo Pahunchev, Chief Product Officer at Infinite Lambda


Agents Schema is available through a Git repository for analytics engineers and data developers to use regardless of ingestion tool, warehouse, or governance solution.


\[CTA_MODULE\]


## Three themes connect these launches


First, we’re building for openness and interoperability. dbt Core v2.0 is the most direct expression of this commitment, but the principle runs through every launch. dbt State works across distributions and surfaces. dbt Wizard meets analytics engineers where they work. The AI Connector Agent produces managed connectors rather than customer-owned code. Agents Schema creates a portable standard for agentic context.


Second, we’re focused on developer productivity and cost efficiency. Faster parse times, fewer unnecessary model runs, project-aware AI assistance, connectors generated in minutes, and context-rich schemas all reduce operational friction. They lower compute, shorten development cycles, and give teams more time for higher-value work.


Third, we are building for AI-native data work. Agents need a foundation they can understand and reason through. They need open standards, efficient compute, trustworthy context, and broad source coverage. Everything we are shipping on June 1 is an investment in that foundation.


## What comes next


When Fivetran and dbt Labs came together, the questions on every customer's mind were reasonable ones: *Will the pace of innovation hold up? Will the things we depend on keep getting better? Will the support for our foundational analytics workflows remain open and strong?*


The joint innovations between Fivetran and dbt Labs highlight our commitment to helping data teams move faster, work more openly, and prepare for the AI era. The teams are shipping things together that neither of us would have shipped alone on a timeline and at a scale that neither organization has operated before. Our coordinated product strategy is taking shape with many developments for the future.


To our joint customers and to the broader data community: we will continue to build, invest, and operate with you at the center. The momentum is real and will only continue to accelerate from here.


Open, interoperable, agent-ready data infrastructure is the future, and together, Fivetran and dbt Labs will get you there. We'll have more to share soon. In the meantime, we'd love for you to try what we’ve shipped.


\[CTA_MODULE\]
