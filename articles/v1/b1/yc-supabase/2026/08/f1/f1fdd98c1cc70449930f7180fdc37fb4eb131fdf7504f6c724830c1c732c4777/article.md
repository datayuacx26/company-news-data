---
schema_version: "1.0.0"
document_id: "f1fdd98c1cc70449930f7180fdc37fb4eb131fdf7504f6c724830c1c732c4777"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/extension-version-pinning-ignored"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-04T01:46:03.369367+00:00"
fetched_at: "2026-08-05T01:38:03.459815+00:00"
content_hash: "sha256:d06434ba7da56bd08c5cb3adf6d80050267c2237108ac43bcc50f988222a98dd"
---

# Extension version pinning is deprecated in favor of default versions

Starting **2026-08-05** , specifying an explicit version when creating or updating a Postgres extension on Supabase is deprecated. Statements like:


`
_10


create extension pgvector version '0.7.0';


_10


alter extension pg_graphql update to '1.5.9';


`


will still succeed, but the requested version is now ignored: the extension is installed at (or updated to) its **current default version** on your project's Postgres instance, and the statement emits a warning:


`
_10


WARNING: only superusers can specify extension versions, ignoring version "0.7.0" and installing the default version


`


In a future release, announced separately in advance, these statements will be **rejected with an error** instead.


### Why we're making this change#


Supabase instances ship multiple versions of some extensions side by side. Allowing any role to install or downgrade to an older version means projects can end up running extension versions with known security issues — including reintroducing vulnerabilities that were already patched in the default version. Pinning versions is now reserved for platform operations.


### What's not affected#


- Extensions you've already installed — nothing changes on running databases.
- ` create extension` /` alter extension ... update` without a version clause.
- Database backups and restores (` pg_dump` output never includes version clauses).
- Installing extensions from the dashboard.


## Migration steps#


1.


Remove` VERSION '...'` and` UPDATE TO '...'` clauses from your migrations, scripts, and tooling — the bare forms are drop-in replacements:


`
_10


-- instead of: use:


_10


create extension pgvector version '0.7.0'; --> create extension pgvector;


_10


alter extension pgvector update to '0.7.4'; --> alter extension pgvector update;


`


2.


After 2026-08-05, check your Postgres logs for` only superusers can specify extension versions` warnings to find any remaining call sites.
