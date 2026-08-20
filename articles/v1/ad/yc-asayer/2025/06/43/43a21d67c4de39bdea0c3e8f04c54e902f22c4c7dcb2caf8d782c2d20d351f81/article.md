---
schema_version: "1.0.0"
document_id: "43a21d67c4de39bdea0c3e8f04c54e902f22c4c7dcb2caf8d782c2d20d351f81"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-rss-5a4068f5753f"
canonical_url: "https://forum.openreplay.com/t/self-hosted-spot-keeps-crashing/513"
published_at: "2025-06-24T18:00:44+00:00"
first_seen_at: "2026-08-17T14:12:50.872421+00:00"
fetched_at: "2026-08-17T14:12:52.808979+00:00"
content_hash: "sha256:f296dadf64d4d616f3d0527a143e6598d933dc908c04d8d8a5916bc02e5fb53f"
---

# Self hosted spot keeps crashing

I’ve tried pretty much everything.


Cloud: AWS (16gb, 4cpu, 50GB storage)
Ubuntu: First I tried on Ubuntu 24, then after seeing the note in docs, I created a new VM with Ubuntu 22 but to no avail.
Session replay, etc. all other services remain operational. But the moment I save the Spot for the very first time spot-openreplay-xxxxxxxxxxx pod enters crashloop. I’ve tried; Install to Ubuntu, Install using Kubernetes, and cloned github repo and tried to run it without provided helm charts. Was not able to resolve the issue.


Today I accidentally saw this forum post in hope of someone also facing the very same issue. Otherwise, I was pretty much fed up and thinking of switching to PostHog.
