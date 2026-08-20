---
schema_version: "1.0.0"
document_id: "7be8fbc4ca039d44f432901dd6a33b4c626f1434cfc0b8906287cc47ecc1cc6e"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/project-level-secrets-tracing-llm-requests-with-opentelemetry"
published_at: "2025-03-15T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:ddc0844a7ca7d5e4ea0696bbec1c0c0502143f73652734c06cebd33106301bf0"
---

# Project-level secrets, tracing LLM requests with OpenTelemetry

March 15, 2025


[Project-level secrets, tracing LLM requests with OpenTelemetry](https://www.openlayer.com/changelog/project-level-secrets-tracing-llm-requests-with-opentelemetry)


We’ve shipped new ways to manage secrets and API keys across your Openlayer projects, making it easier to scale and stay secure.


Now, you can add **project-level secrets** directly from the Platform. This means API keys, auth tokens, and other sensitive values can be securely stored and referenced across your tests without needing to duplicate them or hardcode anything.


For our on-premise users, you can now **set default API keys** for **LLM-as-a-judge** across your entire deployment. No need to configure keys for every individual workspace or project, just set once and go.


## Features


- •


SDKs


Add endpoint to retrieve commit by ID
- •


Templates


Add default test cases and metrics to various LLM projects in templates repo
- •


API


Add workspace creation/retrieval, API key creation, and member invitation endpoints
- •


API


Add \`/versions/{id}\` endpoint to the public API
- •


Evals


Add JSON schema validation test
- •


Evals


Support Azure OpenAI deployments for LLM-as-a-judge tests
- •


Platform


Support project-level secrets
- •


Evals


Add gpt-4o-mini to the LLM evaluator
- •


Platform


Set default API keys for LLM-as-a-judge for an entire on-prem deployment
- •


SDKs


Add support for tracing with OpenTelemetry
- •


Platform


Search, sort and filter inference pipelines in the UI and via the API


## Fixes


- •


UI/UX


Render status message in commit details
- •


Integrations


Handle GitHub commit with empty username
- •


Evals


Issue with creating feature value tests
