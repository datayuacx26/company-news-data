---
schema_version: "1.0.0"
document_id: "6c112aa5009bbd625af3e8ffa9b82b597a36778d3200f927008e73359b429d01"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/add-stripe-payments-vibe-coded-app"
published_at: "2026-06-06T12:44:28+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:bd8822ee0a5e6aab8428eb47c63ed36e6035f47a1436e28d0d83d2802b8cfa07"
---

# How to Add Stripe Payments to a Vibe-Coded App

## Step 2: Webhooks (How Stripe Talks Back to Your App)


The most critical piece. When a payment succeeds, Stripe needs to tell your app — so you can update the user's plan.


Webhooks are HTTP POST requests Stripe sends to a URL in your app. Your app receives the event and acts on it.


**Events to handle:**


- ` checkout.session.completed` → payment succeeded → update user to Pro
- ` customer.subscription.deleted` → subscription cancelled → downgrade user to Free
- ` invoice.payment_failed` → payment failed → notify user, keep plan temporarily


**How to add webhook handling:**


Tell your builder:


> "Set up a Stripe webhook endpoint at /api/webhooks/stripe. When` checkout.session.completed` fires, find the user by their Stripe customer ID and update their plan to 'pro'. When` customer.subscription.deleted` fires, downgrade their plan to 'free'."


Your builder generates the webhook endpoint. In Stripe Dashboard → Developers → Webhooks:


1. Add endpoint: your-app-url/api/webhooks/stripe
2. Select events:` checkout.session.completed` ,` customer.subscription.deleted` ,` invoice.payment_failed`
3. Copy the **webhook signing secret** and add it to your app's environment variables


The signing secret is how your app verifies the request is actually from Stripe and not a fake.


## Step 3: Protect Features Behind Plan Checks


After a user pays, their plan is updated in your database. Now you need to check that plan before showing premium features.


**Tell your builder:**


> "Add a plan check to these features: \[list them\]. If the user's plan is 'free', show an upgrade prompt instead of the feature. The upgrade prompt shows: 'This is a Pro feature. Upgrade for $20/month.' with a link to the checkout."


Feature gates work by checking` user.plan === 'pro'` before rendering premium UI. This is simple logic; any AI builder can implement it.


**The key rule:** gates should be enforced on the server side, not just the frontend. A user who modifies JavaScript in their browser should not bypass a paywall. Ensure your feature checks happen in the backend where the plan is validated against the database.


## Step 4: The Customer Billing Portal


Users need to manage their subscription: cancel, update payment method, see invoices.


Stripe Billing Portal is a hosted page (like Stripe Checkout) that handles all of this. You link users to it; Stripe handles the UI.


**Tell your builder:**


> "Add a 'Manage Billing' button on the account settings page. Clicking it should redirect the user to their Stripe Billing Portal session."


In Stripe Dashboard → Settings → Billing → Customer Portal: enable the features you want users to control (cancel, update payment, view invoices).


## Step 5: Test the Full Flow


Before launching:


1. Create a test account in your app
2. Click "Upgrade to Pro" → should redirect to Stripe Checkout
3. Pay with test card` 4242 4242 4242 4242`
4. Should redirect back to your app with plan updated to Pro
5. Verify premium features are accessible
6. Click "Manage Billing" → should open Stripe Billing Portal
7. Cancel the test subscription → your webhook should downgrade the plan to Free
8. Verify premium features are now gated again


If all 8 steps work: you are ready for live payments. Switch Stripe from Test Mode to Live Mode, update your price IDs with the live versions, and go.


## Common Mistakes


**Not verifying webhook signatures.** If you skip signature verification, anyone can POST fake events to your webhook and give themselves free Pro access. The webhook signing secret prevents this.


**Only checking plan on the frontend.** Frontend checks are easily bypassed. The plan check must happen server-side.


**Not handling subscription cancellations.** If you only handle successful payments, cancelled subscriptions don't downgrade users. Handle` customer.subscription.deleted` .


**Using test keys in production.** Stripe keys starting with` pk_test_` and` sk_test_` only work in test mode. Your production app needs` pk_live_` and` sk_live_` keys.


## Build This With Your AI Agent


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Add Stripe subscriptions to this app. Free plan: \[limits\]. Pro plan: $20/month, unlimited. Implement checkout, webhook handler, feature gates, and billing portal link."


Your agent provisions database, auth, backend, Stripe integration, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Not with an AI builder. Describe the payment flow you want — checkout, subscription management, feature gates — and the builder implements it. You set up the Stripe account and copy in the keys; the builder handles the code.


Stripe charges 2.9% + $0.30 per successful transaction. No monthly fees on the standard plan. For subscriptions, add 0.5% for Stripe Billing. No setup fees, no minimums.


Yes. Create an annual price in Stripe (e.g., $200/year = 2 months free vs $240/year monthly). Add it to your checkout flow: "Choose monthly ($20) or annual ($200/year, save $40)." Stripe handles the billing cycle difference.


Stripe retries failed payments automatically (configurable — typically 3–4 times over 2 weeks). If all retries fail, Stripe fires` customer.subscription.deleted` and your webhook downgrades the user. Configure a` invoice.payment_failed` notification email to warn users before they're downgraded.


Yes — configure the trial period on the Stripe subscription object. "Add a 14-day free trial: users enter payment info but aren't charged until day 14" is a standard Stripe configuration. Your builder can implement this.
