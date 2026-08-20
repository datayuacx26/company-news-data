---
schema_version: "1.0.0"
document_id: "a67f4f666bf566814d5cd46352f9bd3f20d90bb1018af4d2e21f06aeaf7332a9"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/stripe-sync-engine-integration"
published_at: "2025-12-19T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:90e6e051a2164153ba0e21334269696345a2628b2ed77ca6087140354b6bc2ca"
---

# Sync Stripe Data to Your Supabase Database in One Click

Today we are announcing a partnership with Stripe and official support for the[Stripe Sync Engine](https://supabase.com/blog/stripe-engine-as-sync-library) in the Supabase Dashboard. Now, you get a one-click integration that syncs your Stripe data directly into your Supabase database. Query your customers, subscriptions, invoices, and payments using standard SQL.


This integration is the result of a collaboration between Supabase and Stripe. Stripe engineers contributed significant improvements to the open-source sync engine, including incremental sync, flexible JSONB storage, and a new CLI.


## Why sync Stripe data to your database?#


Any application that makes money has to incorporate payments into their application and has billing data worth exploring. Understanding your revenue means joining Stripe data with your application data: Which customers are on which plan? What features do paying users actually use? Which accounts are at risk of churning?


Traditionally, developers face two options:


1. **Make API calls on demand.** Hit the Stripe API whenever you need billing data. This works until you need to join data across tables, aggregate across thousands of records, or run complex analytics.
2. **Build a sync pipeline.** Write code to fetch Stripe data, transform it, handle pagination, manage webhooks, deal with rate limits, and keep everything in sync. This typically takes days or weeks to build correctly.


The Stripe Sync Engine gives you a third option: click a button and query your Stripe data in SQL within minutes.


## How it works#


The sync engine keeps your Stripe data current through two mechanisms: webhooks for real-time updates and scheduled backfills for historical data. When you enable the integration, Supabase automatically configures both.


Once your data is synced, you can query it like any other Postgres table. Here are three scenarios we expect to be especially common.


### Find users who signed up but never converted#


Join your Stripe customers with your auth users to identify accounts that created a login but never started a paid subscription. This is the kind of query that's impractical with API calls but trivial with local data.


`
_15


select


_15


users.email,


_15


users.created_at as signed_up,


_15


now() - users.created_at as days_since_signup


_15


from


_15


auth.users


_15


left join


_15


stripe.customers on customers.email = users.email


_15


left join


_15


stripe.subscriptions on subscriptions.customer = customers.id


_15


where


_15


subscriptions.id is null


_15


and users.created_at < now() - interval '7 days'


_15


order by


_15


users.created_at;


`


### Calculate MRR by plan#


Aggregate your subscription data to see revenue broken down by product. With local data, this query runs in milliseconds regardless of how many subscriptions you have.


`
_12


select


_12


products.name as plan,


_12


count(*) as subscribers,


_12


sum(prices.unit_amount) / 100.0 as mrr


_12


from stripe.subscriptions as subscriptions


_12


join stripe.prices as prices


_12


on prices.id = (subscriptions.plan::json->>'id')::text


_12


join stripe.products as products


_12


on products.id = prices.product


_12


where subscriptions.status = 'active'


_12


group by products.name


_12


order by mrr desc;


`


### Identify at-risk accounts#


Combine billing data with application usage to find paying customers who have gone quiet. This query joins three data sources that would require separate API calls and manual correlation without the sync engine.


`
_13


select


_13


customers.email,


_13


subscriptions.current_period_end as renewal_date,


_13


max(user_events.created_at) as last_active


_13


from stripe.customers as customers


_13


join stripe.subscriptions as subscriptions


_13


on subscriptions.customer = customers.id


_13


join public.user_events as user_events


_13


on user_events.user_id = customers.metadata->>'user_id'


_13


where subscriptions.status = 'active'


_13


group by customers.email, subscriptions.current_period_end


_13


having max(user_events.created_at) < now() - interval '30 days'


_13


order by subscriptions.current_period_end;


`


## Stripe Sync Engine vs Foreign Data Wrapper#


Supabase already offers a[Stripe Foreign Data Wrapper](https://supabase.com/blog/postgres-foreign-data-wrappers-with-wasm) that lets you query Stripe data using SQL. The FDW is a thin layer that translates your SQL queries into Stripe API calls. When you run` select * from stripe.customers` , the FDW makes an API request to Stripe, transforms the JSON response into rows, and returns the results.


This works well for simple lookups. Need to check a customer's subscription status? The FDW handles it elegantly. But the abstraction breaks down when your queries get more complex or for queries that are critical to your application.


Consider a query that joins customers with subscriptions and filters by plan type. The FDW must make separate API calls for each table, fetch all the data into Postgres, and then perform the join locally. A query that takes milliseconds on local data can take seconds or minutes through the FDW. And if you're running this query frequently, you'll hit Stripe's rate limits.


The Sync Engine takes a fundamentally different approach. Instead of translating queries to API calls, it copies your Stripe data into actual Postgres tables. Your queries run against local, indexed data. Joins are fast. Aggregations are fast. There are no rate limits because you're not hitting an external API.


Ultimately, the FDW is great for occasional lookups and simple queries. The Sync Engine is built for applications that need billing data as a first-class part of their database.


## Reliable syncing with Supabase Queues#


There can be a lot of data to import from your Stripe account to your database, especially when you're setting up syncing with an existing account. To perform this backfilling process efficiently and reliably, the sync engine uses[Supabase Queues](https://supabase.com/blog/supabase-queues) (powered by pgmq) to sync data incrementally and in batches.


When you install the integration, the backfilling process will automatically start and spread the work across Edge Functions that can concurrently fetch data from the Stripe API and retry if there are any failures or rate limits.


## Getting started#


Enable the Stripe Sync Engine from your Supabase dashboard:


1. Navigate to **Integrations** in your project dashboard
2. Find **Stripe Sync Engine** and click **Install**
3. Enter your Stripe API key (we recommend a restricted key with write access to Webhook Endpoints and read-only access to all other categories)
4. Syncing will start automatically after the installation completes


The initial backfill will sync your historical data. Depending on your Stripe account size, this may take a few minutes to a few hours. Webhooks begin processing immediately, so new events are captured in real-time.


## Open source collaboration#


The Stripe Sync Engine is open source. Stripe engineers contributed improvements including:


- **JSONB storage with generated columns** for flexibility without sacrificing query performance
- **Incremental sync** using Stripe's cursor-based pagination and Supabase Queues
- **Multi-account support** for Stripe Connect platforms
- **New CLI** with proper event ordering


View the source and contribute:[github.com/stripe-experiments/sync-engine/](https://github.com/stripe-experiments/sync-engine/)


## What's next#


We're working with Stripe on additional improvements:


- Dashboard UI for detailed monitoring of the sync engine
- Pre-built SQL views for common metrics (MRR, churn, LTV)
- Support for additional Stripe objects and other customizations


Start syncing your Stripe data today. Enable the integration from your[Supabase dashboard](https://supabase.com/dashboard) .
