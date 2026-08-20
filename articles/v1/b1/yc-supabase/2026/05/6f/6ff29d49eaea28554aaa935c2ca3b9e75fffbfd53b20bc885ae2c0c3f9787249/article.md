---
schema_version: "1.0.0"
document_id: "6ff29d49eaea28554aaa935c2ca3b9e75fffbfd53b20bc885ae2c0c3f9787249"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/46081-self-hosted-supabase-switching-studio-from-supabase-admin-to-postgres-breaking-change"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:43:32.355791+00:00"
content_hash: "sha256:db0ecaca090f39a35157c27850126feba5410d0912be30459bdcbea3390cc8ef"
---

# Self-hosted Supabase: switching Studio from supabase_admin to postgres (breaking change)

## What's Changing?#


The week of June 15, 2026, the default` docker-compose.yml` in[self-hosted Supabase](https://supabase.com/docs/guides/self-hosting) will switch the database role used by Studio and` postgres-meta` from` supabase_admin` to` postgres` :


- ` studio.environment.POSTGRES_USER_READ_WRITE` will default to` postgres`
- ` meta.environment.PG_META_DB_USER` will default to` postgres`


This aligns self-hosted Supabase with the[platform behavior introduced in late 2022](https://github.com/orgs/supabase/discussions/9314) , which removed superuser access from the dashboard SQL editor and shifted ownership of user-created objects to` postgres` . Self-hosted has been running with the legacy behavior until now.


⚠️ Existing instances that pull this change need to **run a one-time ownership migration script** for the` public` schema, or new operations from Studio may fail on objects still owned by` supabase_admin` .


## Why?#


- Self-hosted has been three-plus years behind the platform on this. Behavior between deployment models should be consistent.
- The` postgres` role is non-superuser, so this reduces the privilege surface used by Studio's day-to-day operations.
- Migrations and scripts that work on the platform may currently fail on self-hosted because of the ownership mismatch - this closes that gap.


## Am I Affected?#


You are affected if **all** of the following are true:


- You run self-hosted Supabase from the` ./docker` directory
- You pull updates from` master` without overriding` POSTGRES_USER_READ_WRITE` or` PG_META_DB_USER`
- You have an existing instance with database objects created via Studio (which would currently be owned by` supabase_admin` )


You are **not** affected if you:


- Use the Supabase[platform](https://supabase.com/) - this has been the default since 2022
- Use the Supabase[CLI for local development](https://supabase.com/docs/guides/local-development) (` supabase start` ) - it's a different deployment option
- Are starting a fresh self-hosted deployment - objects created via Studio will be owned by` postgres` from the start


## What Should I Do?#


If you have an existing self-hosted Supabase instance, follow the[Remove superuser access guide](https://supabase.com/docs/guides/self-hosting/remove-superuser-access) :


1. Run the included script to reassign ownership of objects in the` public` schema from` supabase_admin` to` postgres` :`
_10


sh utils/reassign-owner.sh


`


2. Pull the new` docker-compose.yml` (or manually set` POSTGRES_USER_READ_WRITE: postgres` and` PG_META_DB_USER: postgres` if you maintain a customized compose file)
3. Restart the stack
4. Verify in the Studio SQL editor that` select current_user;` returns` postgres`


The script only touches the` public` schema. Supabase-managed schemas (` auth` ,` storage` ,` realtime` , etc.) and any custom schemas you've created are left intact. If you have objects in custom schemas that you want migrated, you'll need to reassign their ownership manually.


If you'd rather defer this for now, set` POSTGRES_USER_READ_WRITE` and` PG_META_DB_USER` back to` supabase_admin` in your override file. This is a default change, not a removal - but we recommend completing the migration soon to stay aligned with platform behavior.


## Rollout#


Date Change


2026-04-27 Migration[guide](https://supabase.com/docs/guides/self-hosting/remove-superuser-access) published in self-hosting docs


2026-05-18 This changelog published


2026-06-17 Default flips in the next self-hosted Supabase release
