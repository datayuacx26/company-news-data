---
schema_version: "1.0.0"
document_id: "664ed7c583fc075ed08065dc7a5c3d151f136e2c5706db28eaf63c61adf465fb"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/stripe-engine-as-sync-library"
published_at: "2025-07-15T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:847cb5c53bed0489935f8678947abf5a4426774e05c7214aca55b16f30d8867d"
---

# Stripe-To-Postgres Sync Engine as standalone Library

We're excited to announce that[stripe-sync-engine](https://github.com/supabase/stripe-sync-engine) is now available as a standalone npm package:[@supabase/stripe-sync-engine](https://www.npmjs.com/package/@supabase/stripe-sync-engine) !


Previously distributed only as a Docker image (` supabase/stripe-sync-engine` ), you can now plug this into any backend project—whether you're using Node.js, running Express on a server, or even deploying on Supabase Edge Functions.


Stripe-Sync-Engine is a webhook listener that transforms Stripe webhooks into structured Postgres inserts/updates. It listens to Stripe webhook events (like` invoice.payment_failed` ,` customer.subscription.updated` , etc), normalizes and stores them in a relational format in Postgres.


## Why sync Stripe data to Postgres?#


While Supabase offers a convenient[foreign data wrapper](https://supabase.com/partners/integrations/supabase_wrapper_stripe) (FDW) for Stripe, sometimes you want your Stripe data *locally available* in your Postgres database for:


- **Lower latency** : Avoid round-trips to the Stripe API.
- **Better joins** : Query subscriptions, invoices, and charges together.
- **Custom logic** : Build fraud checks, billing dashboards, and dunning workflows directly from your own database.


## New: Use it as an npm package#


You can now install and run the Stripe sync engine directly inside your backend:


`
_10


npm install @supabase/stripe-sync-engine


`


And use it like this:


`
_10


import { StripeSync } from '@supabase/stripe-sync-engine'


_10


_10


const sync = new StripeSync({


_10


databaseUrl: 'postgres://user:pass@host:port/db',


_10


stripeSecretKey: 'sk_test_...',


_10


stripeWebhookSecret: 'whsec_...',


_10


})


_10


_10


// Example: process a Stripe webhook


_10


await sync.processWebhook(payload, signature)


`


For a full list of configuration options, refer to our[stripe-sync-engine README](https://github.com/supabase/stripe-sync-engine/blob/main/packages/sync-engine/README.md) .


## Use via Supabase Edge Function#


To use the Stripe-Sync-Engine in an[Edge Function](https://supabase.com/edge-functions) , you first have to ensure that the schema and tables exist. While you can technically do this inside the Edge Function, it is recommended to run the schema migrations outside of that. You can do a one-off migration via


`
_10


import { runMigrations } from '@supabase/stripe-sync-engine'


_10


;(async () => {


_10


await runMigrations({


_10


databaseUrl: 'postgresql://postgres:..@db.<ref>.supabase.co:5432/postgres',


_10


schema: 'stripe',


_10


logger: console,


_10


})


_10


})()


`


or include the[migration files](https://github.com/supabase/stripe-sync-engine/tree/main/packages/sync-engine/src/database/migrations) in your regular migration workflow.


Once the schema and tables are in place, you can start syncing your Stripe data using an Edge Function:


`
_31


import 'jsr:@supabase/functions-js/edge-runtime.d.ts'


_31


_31


import { StripeSync } from 'npm:@supabase/stripe-sync-engine@0.39.0'


_31


_31


// Load secrets from environment variables


_31


const databaseUrl = Deno.env.get('DATABASE_URL')!


_31


const stripeWebhookSecret = Deno.env.get('STRIPE_WEBHOOK_SECRET')!


_31


const stripeSecretKey = Deno.env.get('STRIPE_SECRET_KEY')!


_31


_31


// Initialize StripeSync


_31


const stripeSync = new StripeSync({


_31


databaseUrl,


_31


stripeWebhookSecret,


_31


stripeSecretKey,


_31


backfillRelatedEntities: false,


_31


autoExpandLists: true,


_31


})


_31


_31


Deno.serve(async (req) => {


_31


// Extract raw body as Uint8Array (buffer)


_31


const rawBody = new Uint8Array(await req.arrayBuffer())


_31


_31


const stripeSignature = req.headers.get('stripe-signature')


_31


_31


await stripeSync.processWebhook(rawBody, stripeSignature)


_31


_31


return new Response(null, {


_31


status: 202,


_31


headers: { 'Content-Type': 'application/json' },


_31


})


_31


})


`


1. Deploy your Edge Function initially using` supabase functions deploy`
2. Set up a Stripe webhook with the newly deployed Supabase Edge Function url
3. Create a new .env file in the` supabase` directory


`
_10


# Use Dedicated pooler if available


_10


DATABASE_URL="postgresql://postgres:..@db.<ref>.supabase.co:6532/postgres"


_10


STRIPE_WEBHOOK_SECRET="whsec_"


_10


STRIPE_SECRET_KEY="sk_test_..."


`


1. Load the secrets using` sh supabase secrets set --env-file ./supabase/.env`


As webhooks come in, the data is automatically persisted in the` stripe` schema. For a full guide, please refer to our[repository docs](https://supabase.github.io/stripe-sync-engine/) .


## Final thoughts#


If you're building with Stripe and Supabase,[stripe-sync-engine](https://github.com/supabase/stripe-sync-engine) gives you a reliable, scalable way to bring your billing data closer to your database and application. Whether you want better analytics, faster dunning workflows, or simpler integrations—this package is built to make that seamless.
