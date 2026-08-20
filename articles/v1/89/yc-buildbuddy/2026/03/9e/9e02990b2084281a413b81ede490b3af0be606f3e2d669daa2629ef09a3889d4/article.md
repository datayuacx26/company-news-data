---
schema_version: "1.0.0"
document_id: "9e02990b2084281a413b81ede490b3af0be606f3e2d669daa2629ef09a3889d4"
company_key: "yc-buildbuddy"
company: "BuildBuddy"
source_id: "yc-buildbuddy-rss-4f82164f35c8"
canonical_url: "https://www.buildbuddy.io/changelog/bb-explain"
published_at: "2026-03-09T11:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.628240+00:00"
fetched_at: "2026-07-28T20:53:30.609843+00:00"
content_hash: "sha256:177c47a891476c02490590fcd0625f92fd0317581026b64e42437810601e66b3"
---

# bb explain: understand why your build re-ran actions

` bb explain` shows a structural diff of two compact execution logs. It helps answer the common questions: "Why did Bazel re-execute that action?" and "What changed between these builds?"


It can highlight non-hermetic outputs and differences in inputs, environment variables, and arguments.


It works with your last two builds automatically, or you can point it at any invocation IDs:


```text
bb explain    bb explain --old <OLD_INVOCATION_ID> --new <NEW_INVOCATION_ID>
```


You can download the` bb` CLI[here](https://www.buildbuddy.io/cli/) . More details in[Fabian's BazelCon talk](https://www.youtube.com/watch?v=jsIzSkaUcx8) .
