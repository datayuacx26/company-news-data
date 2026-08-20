---
schema_version: "1.0.0"
document_id: "eae159f4fdb22f47c63b6a8a1943c93ef3cbb830fb95c102da96bc83bd121948"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-rss-5a4068f5753f"
canonical_url: "https://forum.openreplay.com/t/clarification-about-requests-table-usage-in-postgresql-vs-clickhouse-self-hosted-setup/551"
published_at: "2025-10-27T12:47:03+00:00"
first_seen_at: "2026-08-17T14:12:50.872421+00:00"
fetched_at: "2026-08-17T14:12:52.808979+00:00"
content_hash: "sha256:7a45a338e5c6c8bccd6accaace70bf4a0749113d262adeda9a6889053eb66c29"
---

# Clarification about requests table usage in PostgreSQL vs ClickHouse (self-hosted setup)

[farllonaarin](https://forum.openreplay.com/u/farllonaarin)


October 27, 2025, 12:47pm 1


Hi everyone,


We’re running a **self-hosted OpenReplay** setup and noticed that there is a` requests` table present in both **PostgreSQL** and **ClickHouse** .


The issue we’re facing is that the` requests` table in **PostgreSQL** keeps growing very quickly, which raises some storage concerns.


To better understand its role, we ran a small test:


-


We generated a new session,


-


Then manually deleted all data from the` requests` table in PostgreSQL,


-


After that, we were still able to view the full session, including network requests, in the OpenReplay interface.


This suggests that the session data might be read from ClickHouse rather than PostgreSQL.
However, before we make any decision to clear or truncate this table, we’d like to confirm if it’s **safe** to do so — or if any features depend on the data stored in the PostgreSQL version of the` requests` table.


Could someone from the team clarify the purpose of this table in PostgreSQL, and whether it’s safe to periodically clean or truncate it?


Thanks in advance for your help!


— *Farllon Costa / Aarin*


[mccrackend](https://forum.openreplay.com/u/mccrackend)


March 23, 2026, 2:02pm 2


Have you learned anything more about this topic and how it relates to the clickhouse database?


[farllonaarin](https://forum.openreplay.com/u/farllonaarin)


March 23, 2026, 2:15pm 3


No. The only thing we discovered is that when removing the record from the` requests` table in PostgreSQL, the` requests` part still appears in the session.


Here we only use the session replay part of the tool, so perhaps that’s why we didn’t notice any difference.


[mccrackend](https://forum.openreplay.com/u/mccrackend)


March 23, 2026, 3:27pm 4


Thanks for the reply.


We’re having the opposite but similar issue of clickhouse growing weekly by 10gb, but postgres staying quite flat growing at maybe 100mb. I want to purge sessions to see if that clears out the old clickhouse stored data, but am not sure anymore where the front-door for purging data is. We have a collection of custom events that are now being persisted in clickhouse (v1.25) instead of the external postgres db as it was in older versions (v1.22).
