---
schema_version: "1.0.0"
document_id: "3d9ddd27fed908b030708abc7cf10a40816aa89bff9997cf3c8e124cb8a0bdde"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/bigquery-snowflake-in-community-edition"
published_at: "2026-07-25T00:00:00+00:00"
first_seen_at: "2026-07-25T10:27:53.432388+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:f1dbb0fad920e659340b38eee03c3cecd3350989a890be673352ce4d8595fdd8"
---

# BigQuery and Snowflake available in the community edition

### [BigQuery and Snowflake available in the community edition](https://www.windmill.dev/changelog/bigquery-snowflake-in-community-edition)


Script editor


SQL


[v1.771.0](https://github.com/windmill-labs/windmill/releases/tag/v1.771.0)


[Docs](https://www.windmill.dev/docs/getting_started/scripts_quickstart/sql)


The BigQuery and Snowflake script languages are no longer gated behind an Enterprise license. Both are now selectable in the script editor, the flow step picker and the app inline script picker on every edition, including self-hosted community instances, and both executors ship in the standard community builds. Oracle DB and MS SQL remain Enterprise-only runtimes.


#### New features


- BigQuery and Snowflake are selectable as script languages on the community edition, with no license prompt or disabled tile
- The same applies to the flow step picker and the app inline script picker
- Both executors are compiled into the standard community edition builds, so existing BigQuery and Snowflake resources work without an Enterprise license
- Oracle DB and MS SQL runtimes as dedicated languages remain Enterprise and cloud only
