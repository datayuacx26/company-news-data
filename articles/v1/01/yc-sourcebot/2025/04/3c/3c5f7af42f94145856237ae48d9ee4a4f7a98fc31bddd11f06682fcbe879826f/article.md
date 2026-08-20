---
schema_version: "1.0.0"
document_id: "3c5f7af42f94145856237ae48d9ee4a4f7a98fc31bddd11f06682fcbe879826f"
company_key: "yc-sourcebot"
company: "Sourcebot"
source_id: "yc-sourcebot-news-import-69a1f8dc01ea"
canonical_url: "https://www.sourcebot.dev/changelog/bitbucket-support"
published_at: "2025-04-25T00:00:00+00:00"
first_seen_at: "2026-07-22T14:19:52.577262+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:990d0366d5d9b1e4a393db67d97c34cb136683db5269ae76c71a657039cf34e0"
---

# Bitbucket support

### Bitbucket


We added support for indexing repositories from **Bitbucket Cloud** and **Bitbucket Data Center** . See the[docs](https://docs.sourcebot.dev/docs/connections/bitbucket-cloud) for setup details.


### Search contexts


A search context lets you group repositories together so you can focus a search on a specific part of your codebase - frontend, backend, infrastructure, etc. For example:


- ` context:data_engineering userId` — search for` userId` across your Data Engineering repositories
- ` context:k8s ingress` — search ingress-related content across your Kubernetes configs
- ` ( context:project1 or context:project2 ) logger\\.debug` — find debug logging calls across multiple projects


Search contexts are part of our enterprise license.


### Open-core business model


Alongside this release, we've adopted an open-core model: Sourcebot's core stays MIT licensed, with paid enterprise features layered on top - the same approach taken by companies like GitLab, PostHog, and Langfuse. Search contexts are our first enterprise-licensed feature. If you're interested in an enterprise license, reach out via our[contact form](https://www.sourcebot.dev/contact) or atteam@sourcebot.dev .


### Community shoutouts


- [stevealx](https://github.com/stevealx) contributed the initial Bitbucket support implementation 🙏
