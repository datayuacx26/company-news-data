---
schema_version: "1.0.0"
document_id: "a8d3c1c249b30153a1b50333e8d37f800b5bcee07cbe5ac96068edd983968728"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/realtime-schema-locked-down-against-modification"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-29T23:01:14.192857+00:00"
fetched_at: "2026-07-29T23:01:15.477466+00:00"
content_hash: "sha256:3e1ee7bf737a8dad045e85237aff0975cf5ac406ecf764a1466eeff7bd170d3d"
---

# Realtime schema is now fully locked down against modifications

## What changed#


Supabase now fully restricts modification of the` realtime` schema, shipped in[Realtime v2.112.7](https://github.com/supabase/realtime/releases/tag/v2.112.7) . Previously, even though Realtime owns objects like` realtime.messages` ,` realtime.subscription` , and their supporting functions and triggers, connections using the` postgres` role, including queries run from the Dashboard's SQL Editor, could still run destructive SQL against them, for example:


- ` ALTER TABLE realtime.messages DROP COLUMN topic`
- ` DROP TABLE realtime.messages`
- ` DROP FUNCTION realtime.topic()`
- ` DROP TRIGGER tr_check_filters ON realtime.subscription`
- ` INSERT` or` DELETE` directly on` realtime.schema_migrations`
- ` CREATE TABLE realtime.audit_log (...)` or any other new object created directly in the schema


Any of these now fail with` permission denied for schema realtime` , except creating and modifying Row Level Security policies on` realtime.messages` , needed for[Realtime Authorization](https://supabase.com/docs/guides/realtime/authorization) that is still allowed.


## Why we made this change#


Modifying or dropping objects that Realtime depends on broke Realtime features, and in some cases broke projects entirely. Creating new objects in the schema was just as risky: if a project already had, for example, a` realtime.audit_log` table, and a future Realtime migration also needed to create an object with that name, the migration would fail and Realtime would stop working for that project. The team doesn't inspect project data without prior authorization, so checking every project for name conflicts before shipping a migration isn't realistic. Every new migration carried the risk of breaking projects the team had no way to check in advance. Locking down the schema removes that risk and lets Realtime ship schema changes safely.


## Who is affected#


Projects with custom SQL, migrations, or CI steps that create, alter, or drop objects inside the` realtime` schema. This applies to hosted Supabase projects and to self-hosted Supabase deployments, which run the same restriction.


## What happens if you take no action#


If nothing in your project modifies the` realtime` schema directly, nothing changes for you.
