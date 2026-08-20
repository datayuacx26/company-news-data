---
schema_version: "1.0.0"
document_id: "ff7456b235c01b45530d655a7d27dd9d2b5f7b11a68407a87a761904e08314f3"
company_key: "yc-buildbuddy"
company: "BuildBuddy"
source_id: "yc-buildbuddy-rss-4f82164f35c8"
canonical_url: "https://www.buildbuddy.io/changelog/improved-gpu-support"
published_at: "2026-03-31T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.628240+00:00"
fetched_at: "2026-07-28T22:16:22.012675+00:00"
content_hash: "sha256:fa9847c9ce181f6acb63d4e9c31327d4cfb7d7fc5c1075af7b6991a2b7f60332"
---

# Improved GPU and device support for self-hosted executors

[← Back to changelog](https://www.buildbuddy.io/changelog)


# Improved GPU and device support for self-hosted executors


March 31, 2026 ·


Brandon Duffany


GPU support for self-hosted Linux executors is now easier to set up.


BuildBuddy's executor image now ships with GPU supporting tools (` nvidia-cdi-hook` ), and the executor now supports configuring CDI devices so executors can pass through vendor-provided GPU device configuration to child containers.


See the new setup docs for details:[GPU support for self-hosted executors](https://buildbuddy.io/docs/config-rbe#gpu-support) .
