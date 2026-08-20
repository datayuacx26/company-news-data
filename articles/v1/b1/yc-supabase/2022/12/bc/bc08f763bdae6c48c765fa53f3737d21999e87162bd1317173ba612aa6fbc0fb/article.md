---
schema_version: "1.0.0"
document_id: "bc08f763bdae6c48c765fa53f3737d21999e87162bd1317173ba612aa6fbc0fb"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/launch-week-6-wrap-up"
published_at: "2022-12-16T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:2e54a34680a57db989d7758bc1cd9f6d5ca16616e9ac072d2f4ae6671ca58a2f"
---

# Launch Week 6: Wrap Up

That's a wrap on Supabase Launch Week Day 6. Here's everything we shipped in one long blog post.


## New Docs, built with Next.js#


Monday


For a developer tool, documentation is more than a resource - it's part of the product. For the past two years at Supabase, this part of our product hasn't been great. Our new docs, built with Next.js, feature a completely new design, better navigation, and the promise of a fully-integrated experience.


- Read the[announcement](https://supabase.com/blog/new-supabase-docs-built-with-nextjs)
- Watch the[recap on YouTube](https://www.youtube.com/watch?v=Q1Amk6iDlF8)
- Check out[the new docs](https://supabase.com/docs)


## Storage v2: Image resizing and Smart CDN#


Tuesday


We're introducing three new features for Supabase Storage: Image resizing, webhooks, and a Smart CDN. These features are designed to work together to deliver a next-gen image resizing system.


- Read the[announcement](https://supabase.com/blog/storage-image-resizing-smart-cdn)
- Watch the[recap on YouTube](https://www.youtube.com/watch?v=NpEl20iuOtg)
- Browse the docs for[Image Transformations](https://supabase.com/docs/guides/storage/serving/image-transformations)
- Browse the docs for our[Smart CDN](https://supabase.com/docs/guides/storage/cdn/smart-cdn)
- Discuss it on[HackerNews](https://news.ycombinator.com/item?id=33969076)


## Auth: Multi-factor Authentication with RLS#


Wednesday


We released Multi Factor Authentication for you to build more secure applications. We built a unique twist so that you can use MFA within your Row Level Security Policies.


- Read the[announcement](https://supabase.com/blog/mfa-auth-via-rls)
- Watch the[recap on YouTube](https://www.youtube.com/watch?v=He7LI2mv9v0)
- Browse the[docs](https://supabase.com/docs/guides/auth/auth-mfa)
- Discuss it on[HackerNews](https://news.ycombinator.com/item?id=33984104)


## Wrappers, a FDW framework for Postgres#


Thursday


We announced[Supabase Wrappers](https://github.com/supabase/wrappers) , a framework for building Postgres Foreign Data Wrappers (FDW) which connects Postgres to external systems. We're releasing Wrappers today in Alpha, with support for Firebase and Stripe. Wrappers for Clickhouse, BigQuery, and Airtable are under development.


- Read the[announcement](https://supabase.com/blog/postgres-foreign-data-wrappers-rust)
- Watch the[recap on YouTube](https://www.youtube.com/watch?v=He7LI2mv9v0)
- Browse the[docs](https://supabase.github.io/)
- Discuss it on[HackerNews](https://news.ycombinator.com/item?id=34001493)


## Vault: secrets and encryption in Postgres#


Friday


Vault is a new Postgres extension and accompanying Supabase UI that makes it safe and easy to store encrypted secrets and encrypt other stored data in your database. We're releasing it progressively across the platform and you can expect to see it appear in the Supabase Dashboard in the coming weeks.


- Read the[announcement](https://supabase.com/blog/vault-now-in-beta)
- Learn about[Transparent Column Encryption](https://supabase.com/blog/transparent-column-encryption-with-postgres)


## pg_graphql v1: GraphQL in Postgres#


One more thing


Today released pg_graphql 1.0 release and its general availability on our platform. pg_graphql is a PostgreSQL extension that allows you to query your database using GraphQL. It is the foundation of GraphQL support in the Supabase stack.


- Read the[announcement](https://supabase.com/blog/pg-graphql-v1)
- Browse the[GitHub repo](https://github.com/supabase/pg_graphql/)
- View the[docs](https://supabase.com/docs/guides/api#graphql-api-overview)


## Postgres Point-in-Time Recovery#


One more thing


Point In Time Recovery (PITR) is now enabled from the dashboard for Pro Projects. With PITR, you can restore your database at any specified time in the past.


- Read the[announcement](https://supabase.com/blog/postgres-point-in-time-recovery)
- Review the[Production Readiness docs](https://supabase.com/docs/guides/platform/going-into-prod)


## Custom domains#


One more thing


Custom domains allow you to use your own domain for your Supabase project. We've released custom domains as an upgrade option for all projects, and vanity domains for Pro projects.


- Read the[announcement](https://supabase.com/blog/custom-domain-names)
- Visit the[Custom Domains docs](https://supabase.com/docs/guides/platform/custom-domains)


## pg_crdt: a Postgres extension for CRDTs#


One more thing


We open-sourced an experimental extension for CRDTs, pg_crdt. If you're familiar with Yjs or Automerge, then check out the work we're doing here. It's a PostgreSQL extension that allows you to use CRDTs in your database.


- Read the[announcement](https://supabase.com/blog/postgres-crdt)
- Browse the[GitHub repo](https://github.com/supabase/pg_crdt)
- Discuss it on[HackerNews](https://news.ycombinator.com/item?id=33931971)


## PostgreSQL 15: we support it#


One more thing


All new projects created on the Supabase platform are now on version 15. The PostgreSQL community released version 15 (stable) in October 2022.


- Learn what's[new in PostgreSQL 15](https://supabase.com/blog/new-in-postgres-15)
- View the full PostgreSQL 15[release notes](https://www.postgresql.org/docs/15/release-15.html)


## PostgREST 11: pre-release#


One more thing


You can now test the PostgREST 11 pre-release locally with the Supabase CLI. Some of the new features include: spreading related tables, related orders and anti-joins.


- Learn more about[PostgREST 11](https://supabase.com/blog/postgrest-11-prerelease)
- Visit the[CLI docs](https://supabase.com/docs/guides/resources/supabase-cli)


## Multilingual search in Postgres#


One more thing


We've released a new multilingual Full Text Search extension, PGroonga. It's fast, it's open source, and it's available on our platform today. If you've ever wanted to build a search engine for your app, then check out PGroonga.


- Visit the[PGroonga website](https://pgroonga.github.io/)
- Read the[Extensions Docs](https://supabase.com/docs/guides/database/extensions)


## Flutterflow for Flutter App Development#


Community Spotlight


We partnered with the team at[Flutterflow](https://flutterflow.io/) to bring you a new way to build Flutter apps. Flutterflow is a low-code platform that allows you to build Flutter apps without writing any code. It's a great way to get started with Flutter and Supabase.


- Watch the[tutorial](https://www.youtube.com/watch?v=hw9Q-NjASbU)
- Get Started with[Flutterflow](https://flutterflow.io/)


## OneSignal for Push Notifications#


Community Spotlight


We partnered with the team at[OneSignal](https://onesignal.com/) to offer push notifications in Supabase. OneSignal is a messaging platform allowing you to deliver push notifications, in-app messages, SMS, and emails to your users.


- Watch the[tutorial](https://www.youtube.com/watch?v=mw0DLwItue4)
- Get Started with[OneSignal](https://onesignal.com/)


## Getting Started#


Get started today with Supabase. It's free and open source.


- [Like some memes on Twitter](https://twitter.com/supabase)
- [Star us on GitHub](https://github.com/supabase/supabase)
- [Join the Discord](https://discord.supabase.com/)
- [Read the docs](https://supabase.com/docs)
- [Sign up](https://app.supabase.io/)
