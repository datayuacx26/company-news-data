---
schema_version: "1.0.0"
document_id: "6116b3b47d556c68d3bab9fc74a8fe9d65bf1c66b22239891ab61381518607ac"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/46080-self-hosted-supabase-upgrading-from-pg-15-to-17-breaking-change"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:92c3ea55d882c202afddac5d8a04086550c1354f4f83857b3d449416f1582a76"
---

# Self-hosted Supabase: upgrading from PG 15 to 17 (breaking change)

## What's Changing?#


The week of June 15, 2026, the default` db` image in the[self-hosted Supabase](https://supabase.com/docs/guides/self-hosting)` docker-compose.yml` will move from Postgres 15 to Postgres 17.


⚠️ If your database uses` timescaledb` ,` plv8` ,` plcoffee` , or` plls` , you cannot upgrade to the Supabase images of PG 17 - these extensions are no longer included.


This affects the **default** image tag only. Pinning to a specific` supabase/postgres` 15.x tag will continue to work, and the PG 15 images on Docker Hub will remain available.


Existing Postgres 15 data directories will **not** auto-upgrade. Bringing up the new compose file against a PG 15 volume will fail to start - PG 17 cannot read a PG 15 data directory directly.


## Why?#


- Postgres 17 is what we currently default to on the Supabase platform, and aligning the self-hosted default keeps behavior consistent across deployment models.
- PG 15 has been the self-hosted default for a long time; PG 17 brings several years of upstream improvements.
- Pinning to PG 15 remains supported for anyone who needs more time.


## Am I Affected?#


You are affected if **all** of the following are true:


- You run self-hosted Supabase from the` ./docker` directory
- You pull updates from` master` without pinning the` supabase/postgres` image tag
- Your existing data is on Postgres 15


You are **not** affected if you:


- Use the Supabase[platform](https://supabase.com/)
- Use the Supabase[CLI for local development](https://supabase.com/docs/guides/local-development) (` supabase start` ) - it's a different deployment option
- Pin to a specific Postgres 15 image and don't update
- Are starting a fresh self-hosted deployment - you'll get PG 17 from the start after the default is switched


## What Should I Do?#


If you have a self-hosted Supabase instance with data to preserve, you have two options.


### Option 1: Upgrade in place (via a utility script)#


This is the recommended path for existing data. Read the[upgrade how-to guide](https://supabase.com/docs/guides/self-hosting/postgres-upgrade-17) to learn more.


Before running the script, create your own backup of` ./volumes/db/data` and the` pgsodium` key.


The script will perform the following steps (high-level):


1. Pulls the Postgres 17 binaries and upgrade scripts
2. Stops the Supabase stack
3. Disables incompatible extensions and runs` pg_upgrade` in a temporary PG 15 container
4. Re-enables extensions, applies patches, and runs` VACUUM ANALYZE` in a temporary PG 17 container
5. Swaps data directories (the original is kept as` ./volumes/db/data.bak.pg15` )
6. Restarts the stack on Postgres 17


Rollback to PG 15 is supported - the script keeps the original data directory, and the[guide](https://supabase.com/docs/guides/self-hosting/postgres-upgrade-17#rollback) has the exact steps.


### Option 2: Stay on Postgres 15 and migrate later#


Pin the PG 15 image in your docker-compose file:


`
_10


services:


_10


db:


_10


container_name: supabase-db


_10


image: supabase/postgres:15.x


`


This is the lowest-friction option if you're not ready to upgrade. We'll continue publishing PG 15 images, but new features and fixes will land on PG 17 first (and subsequently on PG 18).


## Rollout#


Date Change


2026-04-08 PG 17 available as opt-in


2026-04-08 Upgrade[guide](https://supabase.com/docs/guides/self-hosting/postgres-upgrade-17) published in self-hosting docs


2026-05-18 This changelog published


2026-06-17 Default flips in the next self-hosted Supabase release
