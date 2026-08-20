---
schema_version: "1.0.0"
document_id: "e0341bafa0013574950104aa9d457cca7d21399e8b3987fccca4c593223a9384"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/artillery-cli-v2-0-24"
published_at: "2025-08-11T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:d5cf315e5b212cceefbe093a11fb8b4d13712a50d55772dcaaac17dfda09c3c7"
---

# Artillery CLI v2.0.24

August 11th, 2025[CLI](https://www.artillery.io/changelog/tag/cli)


# Artillery CLI v2.0.24


- Upgrade to Playwright v1.54.2
- Fix for Apdex scores not always shown at the end of test run
- Fix duration and status not always being reported correctly by the Slack plugin
- Fix for arrival phases not being distributed correctly across worker threads in TypeScript tests
- Decrease reporting lag for large tests on Azure ACI
- Remove outdated Lightstep integration via` lightstep-tracer` library. Lightstep reporting is OTel-only now.


Full release notes on GitHub:[https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.24](https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.24)
