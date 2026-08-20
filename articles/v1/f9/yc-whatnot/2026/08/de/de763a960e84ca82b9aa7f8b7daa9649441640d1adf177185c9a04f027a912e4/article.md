---
schema_version: "1.0.0"
document_id: "de763a960e84ca82b9aa7f8b7daa9649441640d1adf177185c9a04f027a912e4"
company_key: "yc-whatnot"
company: "Whatnot"
source_id: "yc-whatnot-rss-30861744a6f8"
canonical_url: "https://medium.com/whatnot-engineering/whatnot-at-snowflake-summit-2026-a18a855f529d"
published_at: "2026-08-12T17:54:54+00:00"
first_seen_at: "2026-08-12T22:03:50.739008+00:00"
fetched_at: "2026-08-20T03:40:21.870140+00:00"
content_hash: "sha256:ad560b22c9b2f5cb5ef7e805d802eb9c64cd9d648aadab44b8196099b9f28fa0"
---

# Whatnot at Snowflake Summit 2026

Earlier this summer, several members of the Whatnot Data Platform team joined Snowflake on stage at Snowflake Summit to share how we’re building the data platform behind Whatnot’s live marketplace.


This post covers what we talked about, why we built it, and future direction.


*Caption: The Whatnot team were everywhere at the Summit!*


Snowflake recently published a recap of our sessions, which you can read here:[Observability at Scale: Whatnot at Snowflake Summit](https://www.snowflake.com/en/blog/observability-at-scale-whatnot-snowflake-summit/) .


### Data at Whatnot


Every live auction, bid, chat message, purchase, moderation action and recommendation produces data. That translates into billions of events each day powering recommendations, seller analytics, and customer support. For us, data isn’t just for internal analysis. We use it to power the product while millions of people are buying and selling in real time. That creates an unique set of engineering challenges.


As Whatnot has grown, we’ve had to evolve our platform to support thousands of datasets, hundreds of developers, and an increasingly diverse set of workloads while maintaining reliability, governance, and developer velocity. At Summit, we talked about the investments we have made to get engineers to vetted data faster


### Accelerating Builders with AI


The Whatnot data platform is a living system. Schemas evolve, pipelines are deployed continuously, business definitions change, and new data products are published every day. Keeping up with that pace of change has become one of the biggest challenges for developers. In our first session, we shared how we’re using Snowflake Cortex AI and Semantic Views to make that knowledge more accessible.


By grounding AI in governed business definitions and trusted metadata (see this[earlier post](https://medium.com/whatnot-engineering/lessons-learned-from-scaling-data-scientists-with-ai-e7aa7b3235b4) for more details), builders can ask natural language questions about their data, quickly identify the right datasets and metrics, and investigate issues without manually searching documentation or relying on the Data team. Whatnot’s full-time employees now use AI weekly, and much of that usage runs through the data platform.


The transition from clunky, manual data pulls to agentic AI completely transformed the company’s internal culture:


- Widespread adoption: Within 90 days of launch, over 80% of Whatnot’s 1,000+ employees were actively using the agentic solution.
- Universal access: 17 different company departments reached 100% active utilization. Teams such as performance marketing, talent acquisition and business operations became entirely self-sufficient.
- Deeper strategic analysis: Rather than just pulling static lists, teams are using conversational AI for complex work, such as tracking international weekly trends, matching messy data fields and building seller churn prediction models.


What previously required ad hoc data requests planned weeks in advance has been compressed into data available at the speed of typing. This drastically reduces technical friction, allowing product teams to run SQL execution checks, test feed-generation logic and rapidly iterate on predictive models (such as seller churn) without waiting on engineering queues.


### Observability by Default


Developer velocity only matters if engineers can trust the data products they’re building on. Our second session explored why observability has become a foundational capability for Whatnot’s data platform. Every day, we orchestrate thousands of data assets across Snowflake, dbt, and Dagster, where small regressions in freshness, quality, or performance can quickly cascade into downstream systems.


Historically, tracking platform health in real time was a massive headache. Standard utilization logs were delayed by three to four hours, which is far too slow to catch a live pipeline error. The alternative was running massive diagnostic scans every 15 minutes, which burned through the compute budget and required strict admin security clearances just to view the logs.


Setting up high-quality system alerts used to require data engineers to write over a hundred lines of intricate SQL code. This technical barrier effectively locked business analysts and operational managers out of the loop. To address this, we adopted Snowflake’s overhauled event tables, which improved event ingestion speeds by 10x and made comprehensive logging significantly more efficient.


All Whatnot’s data products are now monitored by default with freshness and error rate alerts directed to the relevant product owner using CI-enforced metadata. Developers can opt in to define their own custom data quality checks and asset-level SLAs. Moving forward, we are focused on using these insights to provide a company-wide view of platform health and accelerate time to repair when problems occur.


### Looking ahead


The rest of 2026 is straightforward. It’s a company-wide view of platform health, faster fixes, and better context for the AI tools our developers already rely on. By making trustworthy data easier to discover and the health of that data easier to understand, we can help teams move faster without negatively impacting reliability.


And if solving problems like these sounds interesting, we’re[hiring](https://careers.whatnot.com/) .


---


[Whatnot at Snowflake Summit 2026](https://medium.com/whatnot-engineering/whatnot-at-snowflake-summit-2026-a18a855f529d) was originally published in[Whatnot Engineering](https://medium.com/whatnot-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
