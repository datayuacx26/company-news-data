---
schema_version: "1.0.0"
document_id: "56d50381a7fee6b3b0246af7f1950e12ca54ee26225784fe0aa1a10df18e486b"
company_key: "yc-ncompass-technologies"
company: "nCompass Technologies"
source_id: "yc-ncompass-technologies-rss-225d7ec730f1"
canonical_url: "https://community.ncompass.tech/t/sdk-tracepoint-injection-fails-when-region-is-a-single-line-inside-a-torch-profiler-context/20"
published_at: "2025-11-20T23:06:35+00:00"
first_seen_at: "2026-07-25T15:45:43.528659+00:00"
fetched_at: "2026-07-28T22:25:10.100738+00:00"
content_hash: "sha256:f3f1de017b11b70a28658fa25e845c52cd3f9171baa412020073119e44d6f9e1"
---

# SDK tracepoint injection fails when region is a single line inside a torch profiler context

# [SDK tracepoint injection fails when region is a single line inside a torch profiler context](https://community.ncompass.tech/t/sdk-tracepoint-injection-fails-when-region-is-a-single-line-inside-a-torch-profiler-context/20)


[Bug Reports](https://community.ncompass.tech/c/bug-reports/5)


[AdityaRajagopal](https://community.ncompass.tech/u/AdityaRajagopal)


20 November 2025 23:06 1


If you have the following code:


```text
33 with torch.profiler.profile(….) as prof:
34     some_func()


```


and you mark line 34 in the VSCode editor as the tracepoint, the trace marker won’t show up when you run a profile.


If you have more than 1 line as follows:


```text
33 with torch.profiler.profile(….) as prof:
34     some_func()
35     ...some_other_code...


```


and then you place the tracepoint on line 34, it’ll work fine.


We’re working on figuring out what the issue is.
