---
schema_version: "1.0.0"
document_id: "2b25582dbb5902a3cb22996f50bad10f263a1ba1c8fae15c5a56517dd3dc550e"
company_key: "yc-ramain"
company: "RamAIn"
source_id: "yc-ramain-news-import-43a68eac1f6d"
canonical_url: "https://ramain.ai/resources/event-triggers"
published_at: null
first_seen_at: "2026-07-24T11:17:00.876485+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:e4d55cf9f10bc1bafe33d9e936b1bd2d069c8c7d94842a8d84c2b4f38e58474a"
---

# Event triggers: starting workflows from email, API, and humans

\[Workflow launch\]


A useful workflow should not require a user to open the builder every time. Ramain supports workflows that can start from email triggers, API calls, direct user launches, and human-run sessions.


## Email triggers are workflow records


Workflow triggers are stored as client-scoped records with workflow id, type, name, enabled state, configuration, creator, and timestamps. Enabled email triggers can be listed across clients for inbox matching.


When an email trigger matches, Ramain records an email trigger run with extracted inputs, extraction result, run id, status, reply status, error state, and completion time.


## APIs use scoped keys


Published workflows can also be exposed through API keys. Keys have prefixes, hashed token storage, scopes such as workflows:run and runs:read, expiration, revocation, and last-used tracking.


That lets an external system launch a workflow without granting broad access to the portal.


## The same workflow can have many starts


A workflow can be launched by an operator from the workbench, triggered by inbound email, or called as an API endpoint. The trigger changes how inputs arrive, not what the browser workflow knows how to do.


This is the difference between a demo automation and an operational primitive.


Key takeaway


Triggers turn a workflow into an operational endpoint. The same automation can be launched by a human, an inbox, or another system.
