---
schema_version: "1.0.0"
document_id: "61d2f02d126be4cdefb29949906e37c5f4f5e2ec01fe806a88e7e04d88c8bc04"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-subscription-app"
published_at: "2026-06-01T01:12:40+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:f2db6a0181bf0daa6fb5bce2ea8226d623a3809ddc60765b23c4b4dd2b863a88"
---

# How to Build a Subscription App With Recurring Payments (No Coding Required)

## What Blink builds automatically


Send that prompt and here's what exists in the generated app:


- **PostgreSQL database** — users table, subscriptions table, plan change history. Blink includes the database automatically. No Supabase account needed, no connection string to manage.
- **User authentication** — sign up, sign in, email verification, password reset. Auth is built in — no Clerk, no Firebase Auth, no Auth0 setup.
- **Stripe Checkout integration** — clicking "Upgrade to Pro" opens a real Stripe Checkout session, not a UI placeholder.
- **Webhook handler** — a live endpoint that routes and processes` checkout.session.completed` ,` customer.subscription.updated` ,` customer.subscription.deleted` , and` invoice.payment_failed` . Updates the database on every event without any manual code.
- **Plan-gated routes** — every protected page checks the current plan from the database before rendering. Free users can't reach Pro routes.
- **Billing dashboard** — a user-facing page showing current plan, next billing date, usage summary, and upgrade/downgrade buttons with one-click Stripe Checkout links.
- **Admin dashboard** — a protected` /admin` route showing all subscribers, their current plan tier, and monthly recurring revenue.
- **Deployed and live** — a real URL, not localhost. You can share it before the afternoon is over.


The only things you bring: your Stripe API keys and your Stripe product price IDs. Everything else is already there.


## The billing tier structure


Here's what the generated three-tier structure looks like:


Feature Free Pro ($19/mo) Business ($49/mo)


Core features ✅ Full access ✅ Full access ✅ Full access


Monthly usage limit 100 actions 1,000 actions Unlimited


Data export (CSV) ❌ ✅ ✅


API access ❌ ❌ ✅


Team members 1 3 Unlimited


Priority support ❌ ❌ ✅


Admin access ❌ ❌ ✅


Free exists to give users a path in before they pay. The gap between Free and Pro should be meaningful enough to drive upgrades, not so steep that users bounce before seeing value. 72% of SaaS companies launched in 2026 use a three-tier model for exactly this reason.


Adjust the feature gates in the prompt or directly in the app after it's built. Adding a new gated feature to the Pro tier is a description, not a schema migration.


Stripe homepage — the payments infrastructure for subscription billing and recurring revenue management


Blink


*Stripe homepage — the payments infrastructure for subscription billing and recurring revenue management*


## Handling subscription events


Stripe communicates with your app through webhooks. Every subscription state change fires an event. Here's how the generated webhook handler processes each one:


**New subscriber** (` checkout.session.completed` ): the user completes Stripe Checkout. The webhook records the subscription ID, plan tier, and next billing date in the database. The user's session reflects their new plan on the next request.


**Upgrade** (` customer.subscription.updated` , Free → Pro): Stripe prorates the charge for the remaining billing period. The webhook updates the plan in the database immediately — the user sees Pro features on their next page load.


**Downgrade** (` customer.subscription.updated` , Pro → Free): Stripe schedules this for the end of the current billing period. The webhook records the pending downgrade. The user keeps Pro access until renewal, then drops to Free.


**Cancellation** (` customer.subscription.deleted` ): fires at the end of the canceled billing period (or immediately on cancellation, depending on your Stripe settings). The webhook sets the user's plan back to Free.


**Failed payment** (` invoice.payment_failed` ): Stripe retries automatically on its configured retry schedule. The webhook flags the account for a "payment issue" banner while Stripe handles collection. After the retry window, Stripe sends` customer.subscription.deleted` .


None of this requires manual code. Blink generates the webhook routing logic and the database update handlers — you verify with Stripe test events, then go live.


Building a content-gated product?[How to build a membership site](https://blink.new/blog/how-to-build-a-membership-site) shows the same subscription model applied to content access tiers. If you're starting from a broader product idea,[how to build a SaaS app with AI](https://blink.new/blog/how-to-build-saas-app-with-ai) covers the full product architecture. And if you're building without a technical co-founder,[vibe coding for non-technical founders](https://blink.new/blog/vibe-coding-for-non-technical-founders) explains the mindset shift from writing code to describing products.


Launching a subscription business — three pricing tiers live, recurring revenue running, Stripe integration complete without manual infrastructure setup


Blink


*Launching a subscription business — three pricing tiers live, recurring revenue running, Stripe integration complete without manual infrastructure setup*


## Frequently Asked Questions


No — build the app first, then connect Stripe. Blink generates the full subscription structure (database schema, Stripe Checkout flow, webhook handler) without needing Stripe credentials upfront. You add your API keys in step 2, after the app exists. This means you can see and test the full UI before setting up live payments.


The example prompt is a starting point. Describe your actual tiers in the prompt: "Starter at $9/mo with 500 actions, Growth at $39/mo with 5,000 actions and team seats, Enterprise at $199/mo with unlimited everything and SLA." Blink reads the intent and builds the corresponding tier structure, plan-gating logic, and billing dashboard. Prices are controlled in Stripe's dashboard, so you can adjust them without rebuilding the app.


Yes. Add "with monthly and annual billing options, annual = 2 months free" to your prompt. Blink builds both billing periods into the checkout flow. Stripe handles proration automatically when users switch between monthly and annual. The billing dashboard shows which billing period the subscriber is on.


When Stripe fires` customer.subscription.deleted` , the webhook sets the user's plan back to Free tier. Their account and all their data stay in the database — they're a Free user, not a deleted user. Since Blink includes the database automatically, there's no external database to manage separately. You configure data retention policy and any export windows in the billing dashboard.


Yes. Add "with a 14-day free trial before the first charge" to your prompt. Stripe supports trial periods natively — users enter payment details at signup and get charged after the trial ends, or not at all if they cancel during the trial window. The generated app handles the trial state and shows the trial end date in the billing dashboard.


No. The app runs on Blink's hosting — no Vercel config needed — and the webhook handler processes Stripe events automatically. Upgrades, downgrades, and cancellations are handled by Stripe and the webhook without manual intervention. If you want to add new features or change tier structure, describe the change to Blink. No Supabase account needed, no DevOps cycle to maintain.
