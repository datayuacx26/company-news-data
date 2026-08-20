---
schema_version: "1.0.0"
document_id: "f4daffa0c1acd487d658b4969457ef07d339f1c3a08f0bdcb0bcd20dcc26659d"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-joins-the-stripe-projects-developer-preview"
published_at: "2026-03-19T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:aca3c2c7a75aa998a6e4f200ba96eea89488f8e258e720f3a47c2cb2ae005794"
---

# Supabase Joins the Stripe Projects Developer Preview

Supabase is part of the[Stripe Projects](https://projects.dev/) developer preview. Stripe Projects is a new workflow in the Stripe CLI that lets developers and AI agents provision real services and get working credentials from a single command. With Supabase in the catalog, one command provisions a Postgres database along with the rest of Supabase (Auth, Storage, Edge Functions, and Realtime) without ever opening a dashboard.


AI Agents need a fast way to provision infrastructure, with deterministic steps and real credentials. They cannot click through a browser or guess at setup docs.


Stripe Projects offers a standard provisioning workflow for AI Agents and humans. One command handles account setup, resource provisioning, and credential delivery. Credentials land in your environment in a format that both humans and agents can use. Your Supabase account and dashboard stay yours and are always accessible.


We worked closely with Stripe as a co-design partner on the integration spec. That means provisioning a Supabase project through Stripe Projects is deterministic and repeatable. Whether you run it yourself or an agent runs it for you, the steps are the same.


## How it works#


Install the Stripe CLI, then run:


`
_10


stripe plugin install projects


_10


stripe projects init my-app


_10


stripe projects add supabase/project


_10


stripe projects env --sync


`


That's it. A new Supabase project is created in your account with a full Postgres database ready to connect. To open the Supabase dashboard directly:


`
_10


stripe projects open supabase


`


If you already have a Supabase account, the flow will prompt you to link it instead of creating a new one.


## What you get#


**A complete Supabase project**


Every project includes a Postgres database, Auth, Storage, Edge Functions, and Realtime.[Learn more in the docs](https://supabase.com/docs/guides/database/overview) .


**Credentials in your environment**


After provisioning,` stripe projects env --sync` writes your project credentials to a local` .env` file. They are also stored in your Stripe Project so agents can access them directly.


**Your account, your data**


The database lives in your own Supabase account. You keep full access to the Supabase dashboard, your connection strings, and your data. Nothing is proxied or white-labeled.


**Credential rotation**


You can rotate your database credentials at any time:


`
_10


stripe projects rotate supabase/project


`


This updates the stored secrets in your Stripe Project automatically.


## Get started#


This is a developer preview. Install the Stripe CLI, then run:


`
_10


stripe projects init my-app


_10


stripe projects add supabase/project


`


[Install the Stripe CLI](https://docs.stripe.com/stripe-cli/install)


[Stripe Projects docs](https://docs.stripe.com/stripe-projects)


We want to hear from you. If you run into issues or have feedback,[reach out on Discord](https://discord.supabase.com/) or open an issue on[GitHub](https://github.com/supabase/supabase/issues) .
