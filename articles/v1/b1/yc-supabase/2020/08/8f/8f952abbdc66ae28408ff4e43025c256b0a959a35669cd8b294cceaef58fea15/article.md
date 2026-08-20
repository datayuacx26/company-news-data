---
schema_version: "1.0.0"
document_id: "8f952abbdc66ae28408ff4e43025c256b0a959a35669cd8b294cceaef58fea15"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-alpha-july-2020"
published_at: "2020-08-02T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:e042e275aecf7655d50661b2dc540d3ab2fe79e3581f3cf5d2db6f48057282a3"
---

# Supabase Alpha July 2020

After 5 months of building, we're releasing one of our most anticipated features: Supabase Auth.


### Quick Demo#


Watch a full demo:


### Auth#


This month, we're ecstatic to announce a feature we think you'll love: Supabase Auth. It's too big to fit into a monthly update so look out for a full update in the next few days.


We want to make it easy to get started adding Auth to your app, so we've released a[simple example and a video tutorial](https://dev.to/supabase/create-a-slack-clone-with-next-js-and-supabase-3lhd) which shows you how to implement a basic auth system using PostgreSQL's Row Level Security.


### Table Editor#


We've made some massive improvements to our Table Editor that we're excited to share.


#### Relationship drill down#


Last month we made it easy to drill into your table relationships. This month, we make it possible to drill multiple levels deep.


#### Add, delete, and download rows#


We're making it easier to manipulate your data. Next month, you'll be able to add and remove columns directly from the Table view.


### New Postgres Extensions#


If you launch a new Supabase project, you'll have access to several new Postgres extensions:


- [pgsql-http](https://github.com/pramsey/pgsql-http) : HTTP client for PostgreSQL, retrieve a web page from inside the database.
- [pgjwt](https://github.com/michelp/pgjwt) : PostgreSQL implementation of JSON Web Tokens
- [plpgsql_check](https://github.com/okbob/plpgsql_check) : a linter tool for language PL/pgSQL
- [pljava](https://github.com/tada/pljava) : write Java in your stored procedures, triggers, and functions


### Kaizen#


We have a number of small improvements:


- Added Auth documentation to the auto-generated docs in each project
- Added a new` or` filter[to the client library](https://supabase.com/docs/library/get#or)
- Table View now remembers which tabs you had open.
- We have[released](https://github.com/supabase/postgres-meta/releases) a lot of new functionality to[postgres-meta](https://github.com/supabase/postgres-meta) , a server for for managing Postgres internals via a REST interface.
- Performance: the "flash of black" which was appearing on page transition is now gone


### Get started#


- Start using Supabase today:[supabase.com/dashboard](https://supabase.com/dashboard)
- Make sure to[star us on GitHub](https://github.com/supabase/supabase)
- Follow us[on Twitter](https://twitter.com/supabase)
- Become a[sponsor](https://github.com/sponsors/supabase)
