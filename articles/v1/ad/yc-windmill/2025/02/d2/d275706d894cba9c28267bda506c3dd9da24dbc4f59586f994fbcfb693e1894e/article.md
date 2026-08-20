---
schema_version: "1.0.0"
document_id: "d275706d894cba9c28267bda506c3dd9da24dbc4f59586f994fbcfb693e1894e"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-12b6d71fe86e"
canonical_url: "https://www.windmill.dev/blog/investing-case-study"
published_at: "2025-02-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:07.070377+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:a75eb314e2e0321dfeb7268937a488f45cd80aa988c0b47df306491f7aec6e74"
---

# Windmill for AI Workflows - Investing.com Case Study

> "The most appreciatable thing about Windmill is the feedback and support. The UI is great, it was just easy to start and the functionality was really acceptable. Overall experience is great - it's evolving really fast."


This testimonial is based on conversations with Yonathan Adest (CTO at Investing.com) and Denis Cherepanov & Alex Avaneysan from the AI squad at[Investing.com](https://www.investing.com/) .


## What before Windmill?​


[Investing.com](https://www.investing.com/) is one of the top financial markets platforms worldwide, providing real-time data, quotes, charts, financial tools, and news services. Our AI squad needed a workflow orchestrator to consolidate our various automation processes in one place.


While evaluating different tools like[Airflow](https://airflow.apache.org/) , our primary focus was on speed of implementation, as we needed a solution we could deploy quickly. We found that Airflow would have required significant setup time for our workflows, whereas Windmill offered us an intuitive UI, built-in tracing and logging, and rapid setup through[Docker Compose](https://www.windmill.dev/docs/advanced/self_host#docker) . The ability to easily port our existing code and the self-explanatory interface made Windmill the obvious choice for our team.


## How we use Windmill​


Our AI team leverages Windmill for various automation workflows:


### Content automation pipeline​


Our core workflow manages content processing and distribution. When articles are submitted via[webhooks](https://www.windmill.dev/docs/core_concepts/webhooks) , our system leverages AI and vector embeddings to automatically tag and link content to relevant financial entities. The system assigns relevance scores to each article, which determines whether push notifications should be triggered for specific user segments. Additionally, the pipeline assists with initial content translations, which are then refined through human review. We orchestrate this entire process—from content ingestion to smart distribution—through Windmill.


### Automated stock analysis​


We recently launched a project generating comprehensive PDF reports for individual stocks. These reports include extensive data analysis and trading recommendations. Our workflow pulls data from multiple sources, processes it through our AI models, and generates detailed reports—all orchestrated within Windmill.


### ETL & data processing​


We heavily rely on Windmill for our[ETL](https://www.windmill.dev/docs/core_concepts/data_pipelines) workflows—extracting data from our various sources, transforming it (often using our AI models for tasks like summarization), and loading results into our PostgreSQL database. All processing happens within Windmill, which serves as our central hub for AI data operations.


## Why we chose Windmill​


Several key factors made Windmill the right choice for our team:


- **Quick setup** : The intuitive UI and Docker Compose setup enabled us to get started immediately. The[Helm chart](https://www.windmill.dev/docs/advanced/self_host#helm-chart) and Kubernetes support allowed us to quickly scale up to production.
- **Excellent support** : We consistently receive responses within 15-30 minutes for technical questions.
- **Comprehensive logging** : Built-in[logging](https://www.windmill.dev/docs/core_concepts/audit_logs) capabilities make our debugging and monitoring straightforward.
- **Evolving features** : Regular[updates](https://www.windmill.dev/changelog) that address our needs, like improved parallelism handling.
- **API integration** : The ability to expose workflows as[APIs](https://www.windmill.dev/docs/core_concepts/webhooks) gives us additional flexibility.


We've been particularly impressed with how Windmill has evolved to address early challenges. For example, initial workarounds we needed for handling parallel processing of large datasets are no longer necessary thanks to recent updates.


While we primarily use Windmill for automated workflows rather than its UI generation capabilities, it has become an integral part of our infrastructure, handling everything from periodic data crawling to complex AI-driven content generation.


[Windmill](https://www.windmill.dev/) is an[open-source](https://github.com/windmill-labs/windmill) and[self-hostable](https://www.windmill.dev/docs/advanced/self_host/) developer platform to build, orchestrate, and monitor internal tools and data pipelines, combining the power of code with the velocity of low-code. We turn your scripts into internal apps and composable steps of flows that automate repetitive workflows.


You can[self-host](https://www.windmill.dev/docs/advanced/self_host/) Windmill using a` docker compose up` , or go with the[cloud app](https://app.windmill.dev/user/login) .
