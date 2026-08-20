---
schema_version: "1.0.0"
document_id: "f9c81a617c162e1516800acbd279fc7e275aed63cc1b19241669f86f7b3d630d"
company_key: "yc-odigos-technologies-inc"
company: "Odigos Technologies Inc."
source_id: "yc-odigos-technologies-inc-rss-eb41174e661a"
canonical_url: "https://odigos.io/blog/your-incident-doesnt-fit-in-1m-tokens"
published_at: "2026-04-09T00:00:00+00:00"
first_seen_at: "2026-07-25T17:02:44.020335+00:00"
fetched_at: "2026-08-20T02:53:26.937902+00:00"
content_hash: "sha256:169a131bd2dc10c246113bab0b3034c12f92a32b6d21c81a0a509ee92921f85d"
---

# Your Incident Doesn't Fit in 1M Tokens

AI-powered incident analysis sounds compelling until you do the math. A 1M-token context window holds roughly 5,000 OpenTelemetry spans. A real incident generates 9 million. This post breaks down the arithmetic and explains why the bottleneck isn't the model, it's the instrumentation, sampling, and retention pipeline that feeds it.
