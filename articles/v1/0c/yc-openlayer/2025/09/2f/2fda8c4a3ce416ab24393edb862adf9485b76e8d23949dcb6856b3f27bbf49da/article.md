---
schema_version: "1.0.0"
document_id: "2fda8c4a3ce416ab24393edb862adf9485b76e8d23949dcb6856b3f27bbf49da"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/test-bundles-new-tests-support-for-new-python-runtimes"
published_at: "2025-09-20T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:b3eb582ac7fbabbec9f12c5d180a50919ef13e46fca721482854b6a3586c84cd"
---

# Test bundles, new tests, support for new Python runtimes

September 20, 2025


[Test bundles, new tests, support for new Python runtimes](https://www.openlayer.com/changelog/test-bundles-new-tests-support-for-new-python-runtimes)


We’re very excited to introduce **test bundles** to the Openlayer platform! Easily create a set of tests related to use cases or policies of interest, such as the EU AI Act, OWASP, agentic workflows, data quality, and more. Comprehensive test bundles ensure you catch errors across all your key use cases before they slip through the cracks.


Along with test bundles, we’ve added some new tests to our library: groundedness for LLMs, toxicity score, checks for prompt injections, and checks to see if the outputs recommend a competitor company. You can create these tests today in the Openlayer app!


## Features


- •


Platform


Introduced new metrics and tests, such as toxicity, groundedness, and others
- •


Platform


Released test bundles for the EU AI Act, OWASP, agentic workflows, data quality, and others
- •


CLI


Support for new Python runtimes for development mode


## Improvements


- •


Platform


Improved the data polling and exception handling for the BigQuery integration
- •


UI/UX


Enhanced navigation icons across the app, improving visual clarity and user experience.
- •


API


Improved the handling of attributes from the latest version of the OpenTelemetry GenAI semantic conventions
- •


Platform


Enhanced secret management interface
- •


Platform


Improved the explanations for LLM-based metrics
- •


Platform


Filtering improvements, including filtering tests by priority, status, name, and others.
- •


CLI


Improve date range parsing for the export command of the CLI
- •


SDKs


Trace functionality refactoring for the Openlayer TypeScript SDK with improvements to various integrations, including the LangChain callback handler, and Bedrock Agents
- •


Docs


Improved documentation for integrations like BigQuery, Oracle OCI, and others


## Fixes


- •


SDKs


Python SDK bug fixes for the tracing feature when the traced function yields generators
- •


Platform


Speed up PII detection test
- •


SDKs


Better JSON serialization for platform data uploads
