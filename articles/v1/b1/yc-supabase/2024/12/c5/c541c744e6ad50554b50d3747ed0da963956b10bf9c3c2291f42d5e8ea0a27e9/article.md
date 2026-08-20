---
schema_version: "1.0.0"
document_id: "c541c744e6ad50554b50d3747ed0da963956b10bf9c3c2291f42d5e8ea0a27e9"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-cron"
published_at: "2024-12-04T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:eab9bc8d8b652a674299d4133d9a1951436b6e0e10e469a52fd99b64c5027b3f"
---

# Supabase Cron

Today we're releasing[Supabase Cron](https://supabase.com/modules/cron) , a new Postgres Module that makes recurring Jobs simple and intuitive inside your database.


It's designed to work seamlessly with the entire Supabase platform. Create recurring Jobs to run SQL snippets and call database functions, Supabase Edge Functions, and even remote webhooks.


Supabase Cron is built on the powerful[pg_cron](https://github.com/citusdata/pg_cron) extension by the team at[Citus Data](https://github.com/citusdata) .


It's a Supabase policy to[support existing tools](https://supabase.com/docs/guides/getting-started/architecture#support-existing-tools) wherever possible, and the Citus Data team have generously licensed their extension with the OSI-compatible[PostgreSQL license](https://github.com/citusdata/pg_cron?tab=PostgreSQL-1-ov-file) .


We're very thankful to all the contributors and we look forward to our continued work with the community.


### What's a Cron?#


[Cron](https://en.wikipedia.org/wiki/Cron) is a tool for scheduling recurring tasks that run at specified intervals. These periodic tasks are called “Cron Jobs”. Common use-cases include:


- **Maintenance:** delete or archive old data.
- **Reporting and analytics:** save daily or weekly reports for analysis.
- **Automation:** send periodic emails, like newsletters or reminders.
- **Monitoring** : perform health checks on your database and log the results.
- **Performance:** automate vacuuming tables and rebuilding indexes.


Supabase Cron stores the scheduling logic within Postgres and runs your Jobs accordingly while integrating with the rest of the Supabase primitives - Dashboard, Edge Functions, and AI Assistant.


## How Do You Use Supabase Cron?#


You can create Jobs either via the Dashboard or SQL.


For this post we'll focus on the Dashboard. You can refer to the[documentation](https://supabase.com/docs/guides/cron/quickstart) for SQL.


Within the Dashboard you can define schedules using standard cron syntax and the special` pg_cron` seconds syntax for sub-minute schedules or use natural language.


## Job Types#


You can choose between four types of Jobs based on what you need to execute:


### SQL Snippets#


Create an inline SQL query or command to run on your database periodically. Use this for tasks like:


- Generating reports.
- Cleaning up stale data.
- Refreshing[Materialized Views](https://supabase.com/docs/guides/database/tables?queryGroups=database-method&database-method=sql#materialized-views) .


### Database Functions#


Call a Postgres function. This is useful for workflows, such as:


- Batch processing operations.
- Running periodic maintenance tasks.
- Performing multi-step updates using transactions.


### HTTP Requests (webhooks)#


Trigger an external HTTP endpoint. Use this for:


- Starting external workflows.
- Syncing data with third-party APIs.
- Sending notifications to external systems.


### Supabase Edge Functions#


Run a serverless function to execute custom logic. Examples include:


- Creating embeddings.
- Sending automated email updates.
- Fetching external data and storing it in your database.


These options cover a wide range of use cases, helping with everything from database management to external integrations.


### Observe and Debug Jobs#


Wondering why a Job failed? You can view the history of all Jobs and their logs in the Dashboard. You can see when a Job started, how long it took, and what the result was.


For a deeper dive, you can view Jobs in the[Logs Explorer](https://supabase.com/dashboard/project/_zvmkyvanngopzqaabmvx_/logs/pgcron-logs) .


### Try Supabase Cron today#


Getting started is easy:


1. Visit the[Integrations page](https://supabase.com/dashboard/project/_/integrations) in your project.
2. Enable the **Cron** Postgres Module.
3. Create your first scheduled Job.


We're looking forward to seeing how you use Supabase Cron to help automate your workflows!
