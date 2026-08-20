---
schema_version: "1.0.0"
document_id: "eac1f9c24854f54d119d8fbdd4235aee9755fccb2f1952f23613bfe818af0784"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/restore-credential-resync"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-08-04T01:46:03.369367+00:00"
fetched_at: "2026-08-05T03:48:36.198781+00:00"
content_hash: "sha256:b34c034ae75de01acf723b4348acba2a98673de1928d2a628897c6651bd2a683"
---

# Fixed stale database credentials left behind after a restore

A WAL-G physical restore restores the whole PGDATA directory, including` pg_authid` , so credentials from the backup snapshot overwrote any password rotated after that backup was taken. Credential reapplication was previously only queued when a restore was part of an in-progress clone, so a plain restore to the same project, or an unpause (which shares the same completion path), skipped it entirely and could leave` db_user` with a stale password. Logical restores are unaffected:` pg_dumpall` runs with` --no-role-passwords` , so a logical backup never carries password hashes to begin with.


Supabase now reapplies current credentials after every physical restore completes, regardless of whether it is part of a clone.


A related fix also closes a data-loss path in cloning status tracking: reapplying credentials for a non-clone restore could, on retry exhaustion, flip an unrelated and already-completed clone's status back to failed, which could let a later stray clone request overwrite that project's data. Status transitions are now scoped to clones that are actually in progress.
