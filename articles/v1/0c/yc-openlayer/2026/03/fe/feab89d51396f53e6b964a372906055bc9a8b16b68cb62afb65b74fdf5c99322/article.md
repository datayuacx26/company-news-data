---
schema_version: "1.0.0"
document_id: "feab89d51396f53e6b964a372906055bc9a8b16b68cb62afb65b74fdf5c99322"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/stronger-security-multimodal-tracing-and-expanded-governance-controls"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:5277d63d00f5adc9dabb7a1aafbae35f6a4c5b225cecf568cebba081b7d993b0"
---

# Stronger security, multimodal tracing, and expanded governance controls

March 4, 2026


[Stronger security, multimodal tracing, and expanded governance controls](https://www.openlayer.com/changelog/stronger-security-multimodal-tracing-and-expanded-governance-controls)


This release strengthens enterprise-grade AI governance with platform-wide multi-factor authentication (MFA), deployment-wide directory sync, and more granular project-level access controls. We’ve expanded observability with fully multimodal traces, images, audio, and PDFs now render inline, giving teams richer visibility into model and agent behavior. Governance and framework pages now surface rule-result summaries to streamline audit readiness and compliance reporting. We’ve also added new integrations, SDK enhancements, API controls, and CI-compatible CLI improvements to support automation across modern AI workflows.


## Features


- •


Security


Multi-factor authentication (MFA)
- •


UI/UX


Images and audio render inline in trace steps and data tables
- •


UI/UX


PDF preview in traces and data tables
- •


Security


Deployment-wide directory sync with project-level access group support
- •


UI/UX


Governance and frameworks pages now show a summary of rule results
- •


Platform


Data download in development mode
- •


On-Prem


Diagnostics page for testing connections and sending test emails
- •


Integrations


Langflow integration
- •


SDKs


OpenAI Responses API support in TypeScript SDK
- •


API


Pull rows from specific test results via API
- •


API


Delete projects via API
- •


Platform


Custom metrics bundle now viewable and downloadable from the UI
- •


Integrations


MCP: custom functions callable from Openlayer


## Improvements


- •


UI/UX


Openlayer system tags renamed to "categories"; user-defined labels remain "tags"
- •


Platform


Status page now reflects real-time system health
- •


CLI


openlayer push wait mode is now CI-mode compatible
- •


CLI


openlayer metrics run now supports a -force flag
- •


CLI


openlayer metrics run prints which metrics are being run
- •


Platform


Project owners now have admin-level access for all project-level actions
- •


SDKs


Session and user IDs now accept a broader range of characters
- •


SDKs


LangChain tracer now supports VertexAI API Keys
- •


Security


Directory sync now supports a NoAccess group level for finer-grained access control
- •


SDKs


ConversationalSearchService traces now include document chunks and nested step hierarchy
- •


UI/UX


Test overview column shows more detail at a glance
- •


UI/UX


"Advanced settings" renamed to "Evaluation settings" in test configuration
- •


UI/UX


Improvements to the manual evaluation window config in test creation
- •


API


API key authentication now supported for test result endpoints


## Fixes


- •


SDKs


LangChain callback handler now correctly attributes sessions and user IDs
- •


UI/UX


Custom metrics no longer show a duplicate tag in the test creation page
- •


Platform


Email notifications no longer sent to users without access-group access to a project
- •


On-Prem


Copy buttons now work correctly in on-prem deployments
- •


Platform


OTel traces no longer record empty inputs and outputs
- •


UI/UX


Context relevancy test now correctly indicates when ground truth is being used
- •


Platform


Ragas metrics no longer fail intermittently with key errors
- •


Platform


Prompt injection tests now return consistent row counts
- •


API


Data export now correctly applies the selected start and end time range
- •


Platform


Project-level environment variables now apply correctly to commits
- •


UI/UX


Detected PII types now render as tags in test results
- •


Integrations


Fixed a constraint that prevented the same Copilot Studio bot config across multiple workspaces
- •


UI/UX


Trace data no longer overflows the trace detail modal
