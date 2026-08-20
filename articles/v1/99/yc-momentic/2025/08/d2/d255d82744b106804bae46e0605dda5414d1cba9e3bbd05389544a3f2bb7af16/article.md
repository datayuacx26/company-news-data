---
schema_version: "1.0.0"
document_id: "d255d82744b106804bae46e0605dda5414d1cba9e3bbd05389544a3f2bb7af16"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/failure-recovery"
published_at: "2025-08-21T00:00:00+00:00"
first_seen_at: "2026-07-22T04:45:46.439360+00:00"
fetched_at: "2026-07-23T16:54:58.821228+00:00"
content_hash: "sha256:aa44346db58ce39ed096835f8758d17cbe3ff9fa76dfe8b7e3ea4201364ee121"
---

# Introducing Failure recovery

Keep your tests high-signal. Failure recovery enables Momentic tests to automatically recover from transient UI and network issues so only real regressions reach your team.


## Why we built it


End-to-end tests often fail for reasons you do not control: latency spikes, third-party banners, or a surprise marketing modal. Nothing in your product is actually broken, yet CI turns red and your engineers get inundated with noise.


Failure recovery filters out this noise by letting our autonomous agent get a failing test back on track without changing the original intent.


## How it works


- **Agentic auto-recovery.** When a step fails, Momentic analyzes the browser state, understands the original intent, performs the minimal fix, then retries the original action and assertion.
- **Custom instructions.** Add guidance like “If a marketing dialog appears, close it and continue” to define what counts as recoverable in your app.


Failure recovery configurations


## Common use cases


- Intermittent popups or cookie banners that block clicks.
- Slow or flaky network moments that require a short wait and retry.
- Third-party overlays that temporarily intercept input.


## Building the AI-native testing platform


Real world applications are dynamic and so should be your tests.


Software changes every day. Selectors shift, copy gets tweaked, entire screens are refactored.


Momentic's AI-native platform treats tests as intent, not brittle scripts.


You describe the outcome you want. Our agents plan, act, recover from noise, and keep tests aligned with what you actually care about.


## Getting started


Open **Test options -> Failure recovery** , turn it on, and add any custom instructions. You can start running reliable, higher-signal suites today.


If you’re tired of flaky tests,[book a demo](https://momentic.ai/sales) to get started with Momentic.
