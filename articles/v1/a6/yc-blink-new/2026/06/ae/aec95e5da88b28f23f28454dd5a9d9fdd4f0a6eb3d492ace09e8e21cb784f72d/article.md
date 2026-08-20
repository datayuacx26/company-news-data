---
schema_version: "1.0.0"
document_id: "aec95e5da88b28f23f28454dd5a9d9fdd4f0a6eb3d492ace09e8e21cb784f72d"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-subscription-app-stripe"
published_at: "2026-06-13T12:44:34+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:48:58.638835+00:00"
content_hash: "sha256:447c821425a7e5ca38c494680d9876d82f966f50b20871b468a280925787b5f0"
---

# How to Build a Subscription App with Stripe: Complete Guide 2026

## How to Build With Blink


Open[Blink](https://blink.new/) and start with this prompt:


> "Build a SaaS subscription app with two tiers: Free (5 projects, core features) and Pro ($29/month — unlimited projects, advanced features, priority support). Use Stripe for billing. Auth via email and Google OAuth. Users can upgrade directly from the dashboard. Include a customer portal for managing subscriptions and a billing history page."


Blink generates the complete stack:


- **User auth system** — email and password, Google OAuth, session management, password reset emails
- **Stripe Checkout** — one-click upgrade flow for logged-in users, redirecting to Stripe's hosted payment page
- **Webhook endpoint** — registered at` /webhook/stripe` , handles all subscription lifecycle events
- **Subscription status in database** —` user.plan` column updated on every Stripe event
- **Tier enforcement middleware** — checks` user.plan` before allowing access to Pro features in every API route
- **Customer portal redirect** — "Manage Billing" button opens Stripe's built-in billing portal
- **Upgrade prompts** — "Upgrade to Pro" banners appear at feature limits


Blink generating a SaaS subscription pricing page with Stripe Checkout integration


Blink


## The Webhook Handler: The Critical Piece


Stripe communicates subscription changes by sending HTTP POST requests to a URL you provide. Your app must handle at minimum these three events:


- ` customer.subscription.created` — user subscribed; update database, enable Pro features immediately
- ` customer.subscription.deleted` — subscription cancelled or permanently failed; downgrade to free tier
- ` invoice.payment_failed` — payment failed; notify the user, start retry window


Blink generates webhook handling code that:


1. Validates the webhook signature (prevents spoofed events)
2. Processes events idempotently (same event processed twice does not double-apply changes)
3. Updates` user.plan` in the database atomically
4. Logs every event for debugging


Register the webhook URL in the[Stripe Dashboard](https://dashboard.stripe.com/webhooks) under Developers > Webhooks. Use your Blink app's URL with the` /webhook/stripe` path. Add the webhook signing secret as a Blink environment variable.


## Adding the Customer Portal


Stripe's customer portal lets subscribers manage their own subscriptions without you building a billing UI. They can update their payment method, upgrade or downgrade plans, cancel (with your configured cancellation flow), and download invoices.


Add a "Manage Billing" button to your app's account settings that redirects to Stripe's portal. Blink generates this redirect flow automatically. You enable and configure the customer portal in your[Stripe Dashboard](https://dashboard.stripe.com/settings/billing/portal) under Billing > Customer Portal.


## What You Configure in Stripe Dashboard


Blink handles all the code. You configure the product side in Stripe directly — about 20 minutes of work:


- **Products** — create your subscription products (Free, Pro, Team)
- **Prices** — set monthly and annual pricing for each product
- **Price IDs** — copy the` price_XXXX` ID for each price and add to Blink's environment variables
- **Customer portal** — enable under Billing > Customer Portal, configure which features show
- **Webhook endpoint** — register your Blink app's webhook URL, copy the signing secret


No code changes required for any of these.


## Common Pitfalls (And How Blink Avoids Them)


**Webhook idempotency** — Stripe may deliver the same event more than once. If your handler runs twice, you could grant Pro access twice or apply credits twice. Blink generates idempotent handlers that check if an event was already processed before acting.


**Failed payment handling** — Stripe retries failed payments on a schedule using Smart Retries (machine learning-optimized timing). Your webhook must handle` invoice.payment_failed` to notify the user and` invoice.payment_succeeded` after a retry succeeds. Blink generates both handlers.


**Proration on plan changes** — when a user upgrades mid-period, Stripe calculates the prorated amount automatically. Do not calculate this in your code. Let Stripe handle it and update` user.plan` on the` customer.subscription.updated` event.


**Free trial conversion** — if you offer a free trial, requiring a payment method upfront converts significantly better than card-free trials. Set this in your Stripe product configuration. Blink configures the trial period when you specify it in the prompt.


## Cost Math at $10K MRR


At $10,000 MRR with 500 subscribers at $20/month:


Fee Monthly cost


Stripe transaction fee (2.9% + 30¢ per charge) ~$295


Stripe Billing subscription fee (0.7%) $70


Blink Pro plan $29


**Total infrastructure** **~$394**


**Revenue retained** **~96%**


Compare to the traditional approach without Blink: Clerk auth ($25/mo) + Supabase database ($25/mo) + Vercel hosting ($20/mo) + your time wiring them together. Blink replaces auth, database, and hosting. You pay for Stripe only.


For the full approach to building a complete SaaS product, see[build a SaaS app with AI](https://blink.new/blog/build-saas-app-with-ai) . For founders building without a technical co-founder,[vibe coding for non-technical founders](https://blink.new/blog/vibe-coding-for-non-technical-founders) covers the complete mindset shift.


## Frequently Asked Questions


Stripe Checkout alone handles one-time payments. For recurring subscriptions, you need Stripe Billing, which adds 0.7% to each transaction. It provides subscription management, invoicing, proration, and Smart Retries for failed payments. At $10K MRR, that is $70/month — worth it to avoid building subscription logic yourself.


Create two Stripe Price objects for each tier: one monthly, one annual (typically 15–20% discount). Give each a separate price ID. In Blink, add both price IDs as environment variables. When users choose a billing period at checkout, pass the corresponding price ID to Stripe Checkout. Blink generates the pricing toggle UI and the logic to pass the correct ID.


Stripe retries the payment automatically using Smart Retries. On the first failure, your webhook receives` invoice.payment_failed` — send the user an email to update their payment method. If all retries fail, Stripe sends` customer.subscription.deleted` and your webhook downgrades the user to free. Blink generates all three webhook handlers automatically.


Yes, but card-required trials convert significantly better because users who add a card have committed. For card-free trials, send a card collection email before the trial ends. Stripe supports both flows. Specify your preference in the Blink prompt — Blink configures the Stripe checkout session accordingly.


Blink generates middleware that reads` user.plan` from the database and gates access to paid features. For API endpoints, the middleware returns a 403 with an upgrade prompt if the user's plan does not include the feature. For UI, the dashboard shows upgrade prompts at feature limits. Every limit check reads from the database — never from a client-side variable that users could manipulate.


Yes. Give Blink context about your existing database schema and API structure, then ask it to add Stripe subscription billing. It adds the webhook endpoint, a subscription status column, and tier enforcement middleware to the existing codebase. Existing users get a` plan: 'free'` status automatically. The migration approach avoids rebuilding what already works.
