---
schema_version: "1.0.0"
document_id: "29f1d75627eb1af054b6757deeb79a89179d92622ef7eaae3c925731bbf65d32"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/the-openlayer-mcp-server-automatic-thresholds-bigquery-integration-and-anomaly-detection-project"
published_at: "2025-06-14T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:5dd60cfd23203994beb1ce4d9fb34c004c1f30f27d135b127d5595a08b467b12"
---

# The Openlayer MCP server, Automatic thresholds, BigQuery Integration and Anomaly Detection, Project-level access groups

June 14, 2025


[The Openlayer MCP server, Automatic thresholds, BigQuery Integration and Anomaly Detection, Project-level access groups](https://www.openlayer.com/changelog/the-openlayer-mcp-server-automatic-thresholds-bigquery-integration-and-anomaly-detection-project)


We’re introducing an exciting new feature to our observability platform: automatic thresholds for tests and anomaly detection.


Openlayer now supports **automatic thresholds** for tests, which are data-driven and adapt to your AI system over time. Whether you're monitoring cost, data quality, or GPT eval scores, we'll suggest thresholds based on historical patterns to take the guesswork out of defining acceptable criteria for your system.


We’ve also introduced **anomaly detection** to flag test results that deviate from the norm. This means you’ll get alerted when something’s off based on the automatic thresholds that we predict.


Both features are designed to take the pain out of manual setup and make your evaluations more proactive and intelligent. To get started, just create a new test in the Openlayer app and choose **automatic** when setting the threshold.


## Features


- •


MCP


Release the Openlayer MCP server so users can use Openlayer tests in IDE workflows
- •


SDKs


Add OpenLIT integration notebook
- •


SDKs


Add a convenience function that copies tests from one project to another
- •


SDKs


Add an option to wait for commit completion to push function
- •


SDKs


Add async OpenAI tracer
- •


API


Support creating tests from the API
- •


Evals


Support for automatic thresholds
- •


UI/UX


Daily feature distribution graphs for tabular data projects
- •


Evals


Add a column statistic test that supports mean, median, min, max, std, sum, count and variance
- •


Evals


Add a raw SQL query test
- •


Integrations


Add support for directly integrating a project with BigQuery tables for continuous data quality monitoring
- •


Evals


Add an anomalous column detection test
- •


Platform


Add root cause analysis and segment distribution graphs to various tests’ diagnostic page
- •


Evals


Add support for Gemini 2.0 models for LLM-as-a-judge tests
- •


Platform


Add a priority property to tests (critical, high, medium, low)
- •


Platform


Include or exclude inference pipelines when creating tests in a project
- •


Platform


Add record count, last record received date to inference pipeline
- •


Evals


Support running monitoring mode tests on the entire history of data rather than moving windows
- •


Platform


On-premise deployment guides for OpenShift, AWS EKS
- •


Security


Permissions at a project-level through access groups


## Improvements


- •


Platform


Immediately execute tests in monitoring mode
- •


Platform


Parse OpenTelemetry traces from Semantic Kernel, Spring AI
- •


Platform


Test failures will not cause the commit’s status to fail
- •


Evals


LLM-as-a-judge base prompt tweaks to improve consistency


## Fixes


- •


UI/UX


Broken link in connected Git repo settings
- •


Evals


Increase LLM-as-a-judge criteria character limit
- •


UI/UX


Enable sorting data tables by booleans
- •


Platform


Surface OpenAI refusals to user in LLM-as-a-judge tests
- •


Platform


Add a notification when batch data uploads fail
