---
schema_version: "1.0.0"
document_id: "0e9937ce3e828535b191f6a95a1c96bcda62a1acc475c6cac072df3c11a624a4"
company_key: "yc-1code-21stdev"
company: "1code (21st.dev)"
source_id: "yc-1code-21stdev-news-import-0b053d108858"
canonical_url: "https://21st.dev/blog/incident-review-database-defacement-2026-02-26"
published_at: "2026-02-26T00:00:00+00:00"
first_seen_at: "2026-07-24T04:15:31.771016+00:00"
fetched_at: "2026-07-28T21:26:27.452652+00:00"
content_hash: "sha256:7c2af44ebf632818f69b75378c6337c416a341e7e6f9e80034187520c6205e39"
---

# Incident Review: Database Defacement Attack

## What Happened


An attacker used an automated Python script (` python-httpx/0.28.1` ) from IP` 103.136.147.84` to exploit our Supabase database through the publicly available anon API key.


The attacker first used the GraphQL endpoint (` /graphql/v1` ) to run an introspection query, which returned the complete database schema - every table name, column, type, and relationship. With this map, they executed mass GraphQL mutations to deface records across our database. Simultaneously, they attempted REST API attacks (PATCH/DELETE) against views and tables - most REST attempts failed against views, but the GraphQL mutations succeeded due to permissive RLS policies.


67 GraphQL POST requests returned HTTP 200 within a single 3-second window, using ~15 parallel worker threads. The volume of requests caused database overload, leading to cascading statement timeouts on all legitimate traffic starting from ~4:00 AM PST.


## Why It Happened


Overly permissive RLS policies on three critical tables, combined with an enabled-but-unused GraphQL endpoint:


Table Policy Vulnerability


components "Allow updating likes_count" UPDATE with` qual: true` ,` with_check: true` - any anonymous user could update any column on any component


demos "Enable insert for all users" / "Enable update for all users" INSERT with` with_check: true` , UPDATE with` qual: true` - fully open writes for anyone, including anon


submissions "Allow authenticated users to submit" / "Enable update for all users" INSERT with` with_check: true` , UPDATE with` qual: true` - fully open writes for anyone


GraphQL introspection gave the attacker a complete schema map for free. Without it, they were reduced to guessing - we saw repeated` demo_bookmarks.nodeId does not exist` failures in their REST probes.


## Losses


**Data defacement:** The attacker overwrote names, descriptions, and website URLs in` components` and` demos` tables with racial slurs and pornographic URLs. This was user-facing - visitors saw defaced content until the backup was restored.


The code-related fields (` code` ,` demo_code` ,` compiled_css` ,` tailwind_config_extension` ,` global_css_extension` ) store URLs pointing to R2/CDN storage. The attacker overwrote these URLs with pornographic links. As a result, users who copied component code via the "copy prompt" feature received the HTML content of pornographic websites instead of actual component code, since the frontend fetches and serves whatever content the stored URL points to.


The actual CDN/R2 bundle files were not affected - only the database pointers to them were overwritten, and were restored via PITR backup.


**No API keys, secrets, or user credentials were leaked.** The attack was limited to defacement of public-facing content in the Postgres data layer.


## How We Fixed It


### Immediate response


- Restored database from PITR backup (snapshot from ~2:40 AM PST, Feb 26)
- Deleted and re-created read replicas after restore


### Disabled GraphQL


- Disabled` pg_graphql` extension entirely (not used anywhere in the application)
- Eliminates schema introspection as a reconnaissance vector and closes the mutation path


### RLS policies patched


**components**


Policy Before After


INSERT` {public}` , owner-or-admin check` {authenticated}` only, owner-or-admin check


UPDATE ("Allow updating likes_count")` qual: true` ,` with_check: true` - anyone could update any row **Removed entirely**


UPDATE Owner-or-admin check Owner-or-admin check (unchanged, now the only UPDATE policy)


**demos**


Policy Before After


INSERT` {public}` ,` with_check: true` - anyone could insert any demo` {authenticated}` only, owner-or-admin check


UPDATE` qual: true` - anyone could update any demo Owner-or-admin check


**submissions**


Policy Before After


INSERT` {public}` ,` with_check: true` - anyone could insert` {authenticated}` only, must own the referenced component or be admin


UPDATE` {public}` ,` qual: true` - anyone could update` {authenticated}` only, admin-only


### Post-fix verification


- Confirmed attacker IP` 103.136.147.84` is absent from all subsequent logs
- No PATCH/DELETE/GraphQL requests observed in the three hours after fix
- All traffic patterns returned to normal legitimate usage
