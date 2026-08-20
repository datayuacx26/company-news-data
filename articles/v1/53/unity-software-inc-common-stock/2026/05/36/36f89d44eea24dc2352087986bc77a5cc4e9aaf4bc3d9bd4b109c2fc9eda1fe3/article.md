---
schema_version: "1.0.0"
document_id: "36f89d44eea24dc2352087986bc77a5cc4e9aaf4bc3d9bd4b109c2fc9eda1fe3"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/albion-online-cross-platform-pvp-mmo-architecture"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.442468+00:00"
fetched_at: "2026-07-28T22:14:43.061733+00:00"
content_hash: "sha256:2386ab0fe931155bc7fbad5cf8fa7603f4340868daafca755ac78e652a82ef6d"
---

# Architecting Albion Online: How Sandbox Interactive built a PvP MMO to scale across platforms

When designing new features, the team takes a “desktop-first” approach to ideation, but Android serves as their absolute performance baseline. To maintain a unified experience, *Albion Online* ’s gameplay logic remains identical across all platforms; only the visual fidelity scales. Currently, the mobile build turns off post-processing and utilizes forward rendering, while the desktop version relies on deferred rendering. Looking ahead, Sandbox Interactive plans to transition from Unity's Built-In Render Pipeline to the[Scriptable Render Pipeline (SRP)](https://unity.com/features/graphics) to better tune graphics per device.


To maintain stability across their platform builds, the team relies on an extensive CI/CD pipeline. Jenkins provides daily builds for every platform, while built-in validation tools catch missing references, mesh size limits, and game-data errors before they become an issue. Every developer can run the full server-client stack locally, drastically speeding up feature work and debugging.
