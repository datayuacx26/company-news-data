---
schema_version: "1.0.0"
document_id: "b248fbf6e4ed1de3e5e093121393a4364556a223920509f50821aa24fe35b65f"
company_key: "yc-ncompass-technologies"
company: "nCompass Technologies"
source_id: "yc-ncompass-technologies-rss-225d7ec730f1"
canonical_url: "https://community.ncompass.tech/t/sdk-tracepoint-injection-fails-when-enable-rewrites-is-called-from-the-same-script-that-is-profiled/19"
published_at: "2025-11-20T23:02:57+00:00"
first_seen_at: "2026-07-25T15:45:43.528659+00:00"
fetched_at: "2026-07-28T22:25:10.100738+00:00"
content_hash: "sha256:54a4b3041bbd7488fe00529a2cd5eab076fd15f1bd5ada5ff1f96276b6b3e27a"
---

# SDK tracepoint injection fails when enable_rewrites is called from the same script that is profiled

# [SDK tracepoint injection fails when enable_rewrites is called from the same script that is profiled](https://community.ncompass.tech/t/sdk-tracepoint-injection-fails-when-enable-rewrites-is-called-from-the-same-script-that-is-profiled/19)


[Bug Reports](https://community.ncompass.tech/c/bug-reports/5)


[AdityaRajagopal](https://community.ncompass.tech/u/AdityaRajagopal)


20 November 2025 23:02 1


We’re working on this.


For now, it would be ideal if you could setup the SDK and call enable_rewrites in a different file to the file that contains the function you are profiling.
