---
schema_version: "1.0.0"
document_id: "68d2864ba4dfc4d47152f6dbaa9b46a73558ae8b0ddd1da8a1e3e02132c103ff"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/otel-inbound-distributed-trace"
published_at: "2026-06-05T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:c367825d393de1f126bc58fb79f3809f8aa0dafd73983c0b0b212cf48fa387d3"
---

# Jobs join the caller's inbound distributed trace

### [Jobs join the caller's inbound distributed trace](https://www.windmill.dev/changelog/otel-inbound-distributed-trace)


Self-hosted


[Enterprise](https://www.windmill.dev/pricing)


[v1.719.0](https://github.com/windmill-labs/windmill/releases/tag/v1.719.0)


[Docs](https://www.windmill.dev/docs/misc/guides/otel#connecting-jobs-to-an-inbound-distributed-trace)


When OTEL tracing is enabled, a job run through a webhook or REST endpoint with a W3C traceparent header is connected to the caller's distributed trace. The job span and its flow step and script spans are relocated under the caller's span so the whole execution appears as one end-to-end trace, instead of as a separate one.


#### New features


- A traceparent header on the run endpoints connects the job to the inbound distributed trace.
- The job span, its flow steps and script subprocess relocate under the caller span in your tracing backend.
- The inbound traceparent is exposed as the TRACEPARENT env var and propagated to every flow step.
- Invalid traceparent headers are ignored, so the job_id / root_job trace lookup keeps working.
