---
schema_version: "1.0.0"
document_id: "03854a33bab0f292c5f27c59d8bd57796e60996ccbadb40ecd5628953d230ee2"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-stripe-subscription-app"
published_at: "2026-05-17T00:27:26+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:13:06.313483+00:00"
content_hash: "sha256:11928ef2af5fbf026fb289f088f39bc33415c6eb4ee29f773ed189883ee54ee1"
---

# How to Build a Stripe Subscription App (Without Writing Backend Code)

## Step-by-Step: Build Your Subscription App


1


#### Start with a prompt that describes your billing model


Go to[blink.new](https://blink.new/) and describe your app with the billing structure upfront:


> "Build a SaaS app where users sign up, choose a plan (Starter at $29/month or Pro at $79/month), pay via Stripe Checkout, and get access to features based on their plan. 14-day free trial on all plans. Stripe billing portal for plan changes and cancellations."


Blink generates the full application: the auth system, the database schema (with a subscriptions table linked to your users), the Stripe integration, and the webhook handler. Database is automatically included — no Supabase setup needed.


2


#### Create your Stripe products and prices


In your[Stripe](https://stripe.com/) Dashboard, go to Products and create each plan. Copy the Price ID for each one — it starts with` price_` . You'll need these for environment variables.


Start in test mode (` sk_test_...` keys) so you can run the full billing flow without real charges.


3


#### Add your Stripe keys to Blink's environment variables


In your Blink project, go to Settings → Environment Variables and add:


```text
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
STRIPE_PRICE_STARTER=price_...
STRIPE_PRICE_PRO=price_...


```


With Blink, there's no server config to edit, no deploy pipeline to update. Variables go live the moment you save them — no Vercel config needed.


4


#### Register your webhook endpoint in Stripe


In Stripe Dashboard → Developers → Webhooks → Add Endpoint.


Your webhook URL:` https://\[your-app\].blink.new/api/stripe/webhook`


Select these events:


- ` checkout.session.completed`
- ` customer.subscription.updated`
- ` customer.subscription.deleted`
- ` invoice.payment_failed`


Copy the webhook signing secret (` whsec_...` ) and save it as` STRIPE_WEBHOOK_SECRET` . With Blink, the webhook handler is already written and deployed — you're just pointing Stripe at it.


5


#### Test the complete subscription flow


Use Stripe's test card` 4242 4242 4242 4242` with any future expiry and any CVV. Walk through every scenario:


1. Sign up → choose a plan → Stripe Checkout → trial starts → verify access
2. Customer Portal → upgrade/downgrade → verify tier changes
3. Test card` 4000 0000 0000 9995` → failed payment → verify recovery flow
4. Use Stripe's test clock to advance 14 days → verify trial expiry and billing start


Everything you're testing — the database writes, subscription status checks, portal redirects — Blink generated from your prompt.


6


#### Add a pricing page and feature gates


Prompt Blink: *"Add a pricing page with monthly/annual toggle. Show the Starter and Pro plans side-by-side with a feature comparison. Free users see an upgrade prompt when they try to access paid features."*


Blink updates the UI and wires the feature gates to subscription status in your database. Auth is built in — no Firebase Auth or Clerk needed.


## How Stripe Integrates Inside Blink


The four operations that run your subscription app — and how Blink handles each:


**Subscription creation:** When a user completes Stripe Checkout, Stripe fires` checkout.session.completed` . Blink's webhook handler receives it, extracts the customer ID and subscription ID, and writes the subscription record to your database. Feature access updates immediately.


**Plan changes:** Upgrades and downgrades fire` customer.subscription.updated` . The handler reads the new plan, updates the user's tier in the database, and the feature gates respond on the next request. Stripe handles proration automatically.


**Cancellation:** When a user cancels in the billing portal, Stripe fires` customer.subscription.deleted` . Blink's handler marks the subscription canceled and sets the access end date to the current period end — so the user keeps access through the period they've already paid for.


**Failed payments:**` invoice.payment_failed` triggers a recovery email with a link to update the payment method. The account stays active during Stripe's Smart Retry window. After that, access is restricted until the payment resolves.


[Stripe](https://stripe.com/) processes over $817 billion in payments annually. The webhook event system is battle-tested at scale. What's fragile is hand-written handlers that don't account for all event states — out-of-order delivery, retries, race conditions. Blink generates handlers that cover the complete subscription lifecycle correctly from the first prompt.


## Testing Your Subscription Flow


Before switching to live keys, run each scenario explicitly. Don't rely on the happy path alone.


**Required test matrix:**


- **Happy path:** Sign up → plan selection → Checkout → trial active → feature access verified
- **Trial expiry:** Advance Stripe test clock 14 days → billing begins → still active
- **Upgrade:** Starter → Customer Portal → upgrade to Pro → access level changes immediately
- **Cancellation:** Customer Portal → cancel → access continues to period end → locks after
- **Failed payment:** Card` 4000 0000 0000 9995` → recovery email → card update → access restored
- **Webhook replay:** Use Stripe Dashboard to replay a` checkout.session.completed` event → verify idempotency (no duplicate record)


Stripe's test clock lets you simulate time passage — advance 14 days to test trial expiry or 30 days to test renewal billing. Find it in Stripe Dashboard → Billing → Test clocks.


## Going Live


When every test scenario passes, the switch to production takes about five minutes.


1. Replace` sk_test_...` with` sk_live_...` in Blink's environment variables
2. Create a new webhook endpoint in Stripe's live mode, same URL
3. Replace the webhook secret with the live` whsec_...`
4. Run one real payment as verification
5. Connect your custom domain in Blink project settings


Hosting is included with Blink — no Vercel account, no separate DNS configuration, no deploy pipeline. You add your domain in the project settings and the app is live on it.


Deploying your Stripe subscription app on Blink — from localhost to production in minutes


Blink


Keep test and live configurations separate. Blink supports different environment variable sets for development and production — use this to leave your test flow intact while billing runs in live mode.


## Build This With Blink


The fastest approach: start with one billing prompt, then extend with follow-up prompts.


**Prompt 1 — Core billing:**


> "Build a SaaS with Stripe subscription billing. Monthly ($29) and annual ($278) plans. 14-day free trial. Webhook handler for subscription lifecycle."


**Prompt 2 — Pricing page:**


> "Add a pricing page with monthly/annual toggle and a feature comparison table."


**Prompt 3 — Admin metrics:**


> "Add an admin dashboard showing MRR, active subscribers by plan, trial conversion rate, and churn this month."


**Prompt 4 — Failed payment recovery:**


> "When payment fails, email the user with a link to update their card. Show a warning banner in the app until payment is resolved."


Each prompt is an independent task. The AI agent handles the database changes, the Stripe API calls, and the UI updates. You describe the behavior; Blink writes the implementation.


The[subscription SaaS market grows 17% year-over-year](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/the-growth-of-the-subscription-economy) . The barrier to entry isn't the idea — it's the infrastructure wiring. With Blink, the infrastructure is already there.


Start your subscription app at[blink.new](https://blink.new/) . Related:[how to build a SaaS without writing code](https://blink.new/blog/build-saas-without-coding) and[how to build a membership site](https://blink.new/blog/how-to-build-a-membership-site) .


## Frequently Asked Questions


No. You describe the billing behavior — plans, trial duration, feature gates — and Blink's AI agent writes the Stripe integration, webhook handler, and database schema. The only non-Blink task is creating your products and prices in the Stripe Dashboard, which takes about 10 minutes. No backend code to write or debug.


Blink generates handlers for the full subscription lifecycle:` checkout.session.completed` ,` customer.subscription.created` ,` customer.subscription.updated` ,` customer.subscription.deleted` , and` invoice.payment_failed` . These cover plan creation, upgrades, downgrades, cancellation, and failed payment recovery — every event a subscription app needs.


Stripe Checkout — the hosted payment page — is the right choice for most subscription apps. It handles PCI compliance automatically, converts better than custom forms, and requires zero payment UI code. Blink integrates with Stripe Checkout by default. If you need a fully branded payment experience embedded in your UI, prompt Blink to use Stripe Elements instead.


Stripe's Customer Portal handles all plan changes. Upgrades take effect immediately with automatic proration. Downgrades take effect at the end of the billing period. Blink wires the portal into your app automatically. When Stripe sends a` customer.subscription.updated` event, Blink's webhook handler updates the user's access tier in the database immediately.


Stripe charges 2.9% + 30¢ per successful transaction, plus 0.5% for subscription billing. No monthly fees — you only pay when you process payments. For a $29/month plan, Stripe takes roughly $1.15 per transaction. See the current[Stripe pricing page](https://stripe.com/pricing) for full details.


Yes. Stripe supports per-seat billing (charge per team member per month) and usage-based billing (charge based on API calls, messages, or events) natively. Prompt Blink: "Add per-seat pricing at $15/user/month. When the admin adds a team member, add a seat to their Stripe subscription automatically. When they remove a member, remove the seat at the next billing cycle."


[Stripe](https://stripe.com/) maintains 99.999% uptime. For webhooks specifically, Stripe retries failed delivery attempts for up to 72 hours — so even if your server is temporarily unavailable, no billing events are lost. Blink's webhook handler is idempotent, meaning it processes the same event multiple times safely without creating duplicate records.


Yes — a subscription app and a membership site use the same architecture: users pay, get a tier, and certain content is gated behind that tier. See[how to build a membership site](https://blink.new/blog/how-to-build-a-membership-site) for a content-focused variation. The Stripe integration steps are identical; only the content gating logic differs.
