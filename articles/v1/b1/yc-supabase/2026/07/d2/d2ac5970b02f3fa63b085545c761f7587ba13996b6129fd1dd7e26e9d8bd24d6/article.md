---
schema_version: "1.0.0"
document_id: "d2ac5970b02f3fa63b085545c761f7587ba13996b6129fd1dd7e26e9d8bd24d6"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/admin-api-metrics-panic-fix"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-29T23:01:14.192857+00:00"
fetched_at: "2026-07-29T23:01:15.477466+00:00"
content_hash: "sha256:0b8fc04bcbde85539e2bf1ef496ce3cf6942d292248870d2d1ca0bf07cc21a51"
---

# Fixed a panic in project metrics collection that could drop metrics

The privileged metrics endpoint could intermittently panic while serving concurrent requests, because a single parser instance was shared across those requests instead of each request getting its own. The panic stopped metrics collection for the affected project until the metrics service restarted.


Each request now gets its own parser instance, so concurrent metrics requests no longer interfere with each other.
