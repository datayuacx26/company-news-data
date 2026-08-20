---
schema_version: "1.0.0"
document_id: "2a2cb7e79b14a81de4de9c49c7143f48b7945059ed3e7d8464b3e0c28c082f73"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/complete-design-system-overhaul-snowflake-integration"
published_at: "2025-08-21T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:99befd9f850f323572686ebe8da099b1190c5841fd85be5679b05abcfdfe20ba"
---

# Complete design system overhaul, Snowflake Integration

August 21, 2025


[Complete design system overhaul, Snowflake Integration](https://www.openlayer.com/changelog/complete-design-system-overhaul-snowflake-integration)


This month, we’re excited to unveil our brand new UI! We’ve defined an improved design system, including updated and thoughtfully-crafted styles and components to give the product a fresh, engaging look and feel. The new design system supports both light and dark modes, so be sure to check out both and find the one that suits you.


With the new look, we’ve made a number of other experience improvements, including adding priority levels for tests, improving the navigation and information hierarchy, and adding more data visualizations throughout the product.


We can’t wait for you to try it out and hear your thoughts.


## Features


- •


UI/UX


Brand new UI that's faster, slicker and more enjoyable to use
- •


SDKs


Support tracing Bedrock models
- •


SDKs


Support tracing OpenAI Agents
- •


SDKs


Support tracing Pydantic AI systems
- •


SDKs


Support tracing LangGraph systems
- •


Security


New "Member restricted" role, which can perform member actions without viewing data source data
- •


Integrations


Directly connect Snowflake tables to projects
- •


UI/UX


View project, datasets and table dropdowns when connecting BigQuery tables
- •


Platform


Allow hosting Openlayer on subpaths in on-prem deployments
- •


Platform


Allow users to override LLM costs with custom costs
- •


Evals


Include standard deviation score in LLM-as-a-judge and Ragas test results
- •


Evals


New prompt injection test to detect adversarial attacks on LLM systems


## Improvements


- •


Platform


Rename "inference pipelines" to "data sources" to capture broader scope
- •


Platform


Better skipped test messages for metrics that require ground truths
- •


API


Speed up endpoints that return record counts and last record date for data sources
- •


Evals


Show per-row scores for metrics like semantic similarity, exact match in data tables


## Fixes


- •


Evals


Tests that use both historical data and auto thresholds were erroring
- •


API


Speed up data source creation request
- •


Platform


Re-run tests that are stuck in running state
- •


API


Allow streaming data with numpy arrays in the body
