---
schema_version: "1.0.0"
document_id: "6542e72d3f55b64c543bd48467e31c583f843ce1b5965b1f4b5264fa15b0c151"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/more-tests-around-latency-metrics"
published_at: "2024-02-15T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:f972463441abc453aeea4b18b86b94f9dc440e7a87a1068d87d9e2978bae4b3b"
---

# More tests around latency metrics

February 15, 2024


[More tests around latency metrics](https://www.openlayer.com/changelog/more-tests-around-latency-metrics)


We’ve added more ways to test latency. Beyond just mean, max, and total, you can now make test latency with minimum, median, 90th percentile, and 99th percentile metrics. Just head over to the Performance page and the new test types are there.


You can also create more granular data tests by applying subpopulation filters to run the tests on specific clusters of your data. Just add filters in the Data Integrity or Data Consistency pages, and the subpopulation will be applied.


## Features


- •


Evals


Ability to apply subpopulation filters to data tests (Min Latenc, Median Latency, 90th Percentile Latency, 95th Percentile Latency, 99th Percentile Latency)
- •


SDKs


Support for logging and testing runs of the OpenAI Assistants API with our Python and TypeScript clients


## Improvements


- •


API


Updated OpenAI model pricing
- •


Templates


Support for OpenAI assistants with example notebook
- •


Performance


Improved performance for monitoring projects
- •


UI/UX


Requests are updated every 5 seconds live on the page
- •


UI/UX


Ability to search projects by name in the project overview
- •


UI/UX


You can now view rows per evaluation window in test modals
- •


UI/UX


Date picker for selecting data range in test modal
- •


UI/UX


Show only the failing rows for tests
- •


UI/UX


Allow opening rows to the side in test modal tables
- •


UI/UX


Enable collapsing the metadata pane in test modals
- •


UI/UX


Skipped test results now render the value from the last successful evaluation in monitoring


## Fixes


- •


Integrations


Langchain version bug is fixed
- •


UI/UX


Metric score and explanations did not appear in data tables in development mode
- •


UI/UX


Request table layout was broken
- •


UI/UX


Now able to navigate to subsequent pages in requests page
- •


UI/UX


Fixed bug with opening request metadata
- •


Performance


Requests and inference pipeline occasionally did not load
- •


Performance


Some LLM metrics had null scores in development mode
- •


UI/UX


There was a redundant navigation tab bar in monitoring test modals
- •


Performance


Monitoring tests with no results loaded infinitely
