---
schema_version: "1.0.0"
document_id: "0280a575775ce8a1ac2281d6f80b0853f9c0bef12e6e7a82bf848d795eed4876"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-is-now-a-connector-on-perplexity-computer"
published_at: "2026-08-07T07:00:00+00:00"
first_seen_at: "2026-08-07T11:30:43.921443+00:00"
fetched_at: "2026-08-07T11:30:50.752104+00:00"
content_hash: "sha256:95103384706525705ba99b33255a55fc92e3825fb0606e1e57c36a62e0cc1960"
---

# Supabase is now a connector on Perplexity Computer

Supabase is available as a[connector on Perplexity Computer](https://www.perplexity.ai/connectors/supabase?utm_source=supabase&utm_medium=partner&utm_campaign=builders_blog_%2Baug6) . With Supabase as a persistent data layer, Computer can read from and write back to your Postgres tables, keeping state across runs without custom glue code.


[Perplexity Computer](https://www.perplexity.ai/computer) is an agent that runs multi-step work across 400+ apps, including GitHub, Vercel, Datadog, Stripe, and Slack. Connect your Supabase project and your database joins that loop.


## What you can do with the Supabase connector#


The connector handles the kind of work you'd normally context-switch into the Supabase Dashboard or psql for:


- Answer questions about production data: ask which tables grew the most this week, and Computer runs the query and answers.
- Debug from a support ticket: pull a user record, check their last sign-in, and find their failed payments in one prompt.
- Test Edge Functions: invoke` send-welcome-email` with a test payload without leaving the chat.
- Put recurring queries on a schedule: Computer runs your weekly growth query and posts the report to Slack every Monday.


Because Computer spans your other tools, a single prompt can too. For example, join Stripe cancellation events with your Supabase behavioral data to find where users drop off, then ship the fix through a GitHub PR.


## Get started#


- [Connect Supabase on Perplexity](https://www.perplexity.ai/connectors/supabase?utm_source=supabase&utm_medium=partner&utm_campaign=builders_blog_%2Baug6)
- New to Supabase?[Start your project for free](https://supabase.com/) , then connect it to Computer
