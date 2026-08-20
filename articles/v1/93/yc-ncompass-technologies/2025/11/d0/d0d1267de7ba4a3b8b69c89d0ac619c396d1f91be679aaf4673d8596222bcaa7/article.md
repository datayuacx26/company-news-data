---
schema_version: "1.0.0"
document_id: "d0d1267de7ba4a3b8b69c89d0ac619c396d1f91be679aaf4673d8596222bcaa7"
company_key: "yc-ncompass-technologies"
company: "nCompass Technologies"
source_id: "yc-ncompass-technologies-rss-225d7ec730f1"
canonical_url: "https://community.ncompass.tech/t/system-relies-on-docker/18"
published_at: "2025-11-20T23:00:02+00:00"
first_seen_at: "2026-07-25T15:45:43.528659+00:00"
fetched_at: "2026-07-28T22:25:10.100738+00:00"
content_hash: "sha256:7cbf610311cd1ebc8dd4ce57d59c0f65d107a79e3535efd94e5058d68ea2ad4a"
---

# System relies on Docker

[AdityaRajagopal](https://community.ncompass.tech/u/AdityaRajagopal)


20 November 2025 23:00 1


Our VSCode extension launches Docker in the backend for the “business logic”, but we’ve noticed that it’s a pain to setup on various systems.


We’ve built it this way primarily for reproducibility.


We’re building a version of the tool which doesn’t run docker on the backend, so our tool is easier to setup on people’s machines.


[AdityaRajagopal](https://community.ncompass.tech/u/AdityaRajagopal)


9 December 2025 18:13 2


This is now fixed. We no longer require Docker and only require two things to be installed in your system:


- Redis
- Python 3.10+


Update the version of your extension to v0.0.9+
