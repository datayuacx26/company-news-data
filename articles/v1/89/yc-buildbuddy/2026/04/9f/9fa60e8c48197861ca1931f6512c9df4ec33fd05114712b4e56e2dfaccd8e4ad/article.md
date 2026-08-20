---
schema_version: "1.0.0"
document_id: "9fa60e8c48197861ca1931f6512c9df4ec33fd05114712b4e56e2dfaccd8e4ad"
company_key: "yc-buildbuddy"
company: "BuildBuddy"
source_id: "yc-buildbuddy-rss-4f82164f35c8"
canonical_url: "https://www.buildbuddy.io/changelog/cli-run-log-streaming"
published_at: "2026-04-21T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.628240+00:00"
fetched_at: "2026-07-28T22:15:48.391228+00:00"
content_hash: "sha256:28e1976946de6ce9634aadbcc6468e525bebb93a33c6e6cd00a915056470436f"
---

# Run log streaming

In addition to build logs, you can now use the[bb CLI](https://buildbuddy.io/cli) to stream` bazel run` executable output to our servers and view live-updating run logs from the UI.


This gives you a durable record of executable output, making it easy to share logs with teammates, or letting you monitor runs while away from your terminal.


To use it:


```text
bb run //app:server   --stream_run_logs
```


By default, if streaming fails, the executable will continue to run. To fail the command immediately, use` --on_stream_run_logs_failure=fail` .
