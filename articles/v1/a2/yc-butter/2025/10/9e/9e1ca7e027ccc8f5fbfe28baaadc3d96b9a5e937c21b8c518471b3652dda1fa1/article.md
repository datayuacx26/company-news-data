---
schema_version: "1.0.0"
document_id: "9e1ca7e027ccc8f5fbfe28baaadc3d96b9a5e937c21b8c518471b3652dda1fa1"
company_key: "yc-butter"
company: "Butter"
source_id: "yc-butter-rss-cfe99054af6a"
canonical_url: "https://blog.butter.dev/changelog-0001"
published_at: "2025-10-10T07:00:00+00:00"
first_seen_at: "2026-07-24T22:18:01.434032+00:00"
fetched_at: "2026-07-28T20:55:41.466450+00:00"
content_hash: "sha256:ddf10223085fee3839375aef29a7ecf87838e76b0bf6d949458679851d036a22"
---

# Changelog #0001

Hi everyone! Welcome to our first of many weekly changelogs, set to run every Friday. Follow along for weekly updates and improvements.


This week was primarily focused on ensuring compatibility with upstream providers (OpenAI, for the time being) and downstream agent frameworks.


## Features


### Brotli compression support


Added support to handle large compressed responses from OpenAI.


### Transparent forwarding


All requests to unsupported endpoints, such as Responses or Encodings, are now transparently forwarded without involving the cache. This allows popular agent libraries pointed at Butter proxy to operate as expected, with caching only on the Chat Completions requests.


### Request pagination


Dashboard home-page loads are now much faster for users with moderate request traffic.


## Security & Uptime


-


Data access controls improved on UI backend


-


Uptime monitors and alerts configured


## Fixes


-


Various UI rendering/flickering bugs


-


Stale-segfault bug in proxy


-


Broken links in cache graph UI


-


Mobile reactivity improved*


- *with a message that says “use desktop please”
