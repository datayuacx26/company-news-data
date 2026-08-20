---
schema_version: "1.0.0"
document_id: "01fb01b2a49a3c12a53220a371a4308aa742ab87ef545db3274ced589962db88"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/46320-breaking-change-in-pg-graphql-1-6-0-graphql-introspection-disabled-by-default"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:b30ef618cbdd39ea0962f0227f6b0dd32bbac0168b5c8ab52016cc3487b90513"
---

# Breaking change in pg_graphql 1.6.0 — GraphQL introspection disabled by default

> **Edit 2026-06-13:** Rollout date pushed from 2026-06-15 to 2026-06-29 to allow additional AMI build verification. No other changes to the rollout plan.


Starting with pg_graphql **1.6.0** (shipping in new Supabase projects from **2026-06-29** ), GraphQL introspection is **disabled by default** .


## Who is affected#


- **New projects** created on or after 2026-06-29 will run pg_graphql 1.6.0+ and have introspection disabled by default.
- **Existing projects** are not affected unless and until you upgrade pg_graphql to 1.6.0+ (e.g. by upgrading your project's Postgres version). Older projects keep their current behaviour.


## What's changing#


Previously,` __schema` and` __type` queries worked without any configuration. From 1.6.0, they return an error unless you explicitly opt in:


`
_10


{ "errors": \[{ "message": "Unknown field \\"__schema\\" on type Query" }\] }


`


If your project uses any of the following, you'll need to opt in before relying on introspection:


- **Supabase Studio GraphQL explorer** (GraphiQL) — uses introspection to display your schema and provide autocomplete
- **External GraphiQL or GraphQL Playground**
- **Apollo DevTools**
- **Relay compiler**
- **Code generators** (e.g.` graphql-codegen` )
- Any tool that calls` __schema` or` __type` directly


**Regular data queries are not affected.**` accountCollection` ,` insertIntoAccountCollection` , etc. continue to work normally regardless of this setting.


## How to opt in#


Run this SQL once per schema you want to expose introspection on:


`
_10


comment on schema public is e'@graphql({"introspection": true})';


`


If you already have a comment on your schema (e.g. for` inflect_names` ), combine the keys:


`
_10


comment on schema public is e'@graphql({"inflect_names": true, "introspection": true})';


`


## Why this change#


Introspection exposes your full API surface — all types, fields, and relationships — to anyone who can reach the endpoint. Disabling it by default reduces the risk of API enumeration and makes it easier to keep private schemas private.


## Further reading#


- [Configuration docs — Introspection](https://supabase.github.io/pg_graphql/configuration/#introspection)
- [pg_graphql changelog](https://supabase.github.io/pg_graphql/changelog/)
