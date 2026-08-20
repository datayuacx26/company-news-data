---
schema_version: "1.0.0"
document_id: "12b4b4ebfca17cc059ba93fb46f78d2d2554546b4f4d57e42e56f57da8cc5c17"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/saml-directory-sync-new-llm-as-a-judge-models-and-website-refresh"
published_at: "2025-01-16T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:aa4a55c9270201cdc69340480c7f5906f67cc34634c7905533b3b4289320e21c"
---

# SAML Directory Sync, new LLM-as-a-judge models, and website refresh

January 16, 2025


[SAML Directory Sync, new LLM-as-a-judge models, and website refresh](https://www.openlayer.com/changelog/saml-directory-sync-new-llm-as-a-judge-models-and-website-refresh)


We’ve added lots of features and enhancements across our platform, focused on improving performance, expanding functionality, and streamlining workflows. To highlight a few:


**🔐 Increased security with SAML SSO directory sync.** You can now sync SAML SSO on Openlayer with existing security groups. Openlayer can now more seamlessly fit into your organization’s security policies.


**🧑‍⚖️ New LLM-as-a-judge models.** We’ve expanded the models available to act as judges for LLM-as-a-judge tests. You can now use models from Cohere and Vertex AI when running these tests.


**🎨 Website refresh.** We’ve given our website a brand refresh, including lots of fun animations showcasing the Openlayer product in action and case studies from some of our customers.


## Features


- •


SDKs


Faster batch uploads with pyarrow support
- •


SDKs


Push commits to the platform via the Python SDK
- •


UI/UX


Tabular view of test results in test modals
- •


UI/UX


Add pie graph for test results in project home
- •


Evals


Add Faithfulness and Answer Correctness metrics for RAG systems
- •


Platform


Use Cohere, Vertex AI models as options for LLM-as-a-judge metrics
- •


API


Add \`expand\` to inference pipeline GETs so projects and workspaces are included in the response body
- •


Platform


New "Viewer" role in workspaces that doesn’t have write, update or delete permissions on resources
- •


SDKs


Support for async data uploads, and faster upload speeds
- •


Platform


Directory sync with SAML


## Improvements


- •


API


Lower latency for data stream endpoint
- •


UI/UX


Update tooltips and rendering of statuses in test cards
- •


UI/UX


Make sections in test modals collapsible
- •


API


Add skipped and failing test counts in project version and inference pipeline objects
- •


API


Better error messages for invalid data configs when streaming data
- •


Platform


More intuitive status messages for skipped tests
- •


Documentation


Add code samples in Java


## Fixes


- •


Platform


Generate outputs step was not failing gracefully
- •


UI/UX


Surface user-facing error messages upon SSO login failures
- •


UI/UX


Better failure message when password reset link has expired
- •


Platform


Improved rate limiting
- •


Integrations


Slack notifications for create pipeline now includes name
- •


Platform


Answer Correctness metric was breaking when output was not a string
