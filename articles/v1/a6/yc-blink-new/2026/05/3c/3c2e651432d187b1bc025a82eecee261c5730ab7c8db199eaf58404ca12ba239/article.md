---
schema_version: "1.0.0"
document_id: "3c2e651432d187b1bc025a82eecee261c5730ab7c8db199eaf58404ca12ba239"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/add-stripe-payments-ai-app"
published_at: "2026-05-21T12:52:41+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:b560aff2e64f5bf2cb618e551548559ad5b0607fd18a42bb6536c3985f20af74"
---

# How to Add Stripe Payments to Your AI-Built App (No Backend Code Required)

## Step 1: Set Up Your Stripe Account


1


#### Create a free Stripe account


Go to[stripe.com](https://stripe.com/) and sign up. Free to create — Stripe charges 2.9% + $0.30 per successful card transaction, with no monthly fee.


2


#### Get your API keys


In the Stripe Dashboard, navigate to Developers → API Keys. You will see a **publishable key** (safe for frontend code) and a **secret key** (server-side only, never expose to the browser). For testing, use the test keys — they start with` pk_test_` and` sk_test_` .


3


#### Create your product and price


Go to Products → Add Product. Set a name, description, and pricing. For a subscription, choose "Recurring" and set the billing interval. Stripe creates a **Price ID** (e.g.,` price_1234abc` ) that you reference in your checkout code.


## Step 2: Choose Your Integration Path


Stripe offers three levels of integration complexity. Choose based on what you need:


**Option A: Stripe Payment Links (no code required)**


Stripe can generate a hosted payment page with a single URL. No code needed — go to Payment Links in your dashboard, configure your product, and Stripe gives you a URL like` buy.stripe.com/abc123` . Share it anywhere.


The limitation: you cannot programmatically know when a payment succeeds. Payment Links work for one-time payments where you collect orders manually. They do not work for subscription apps where you need to unlock features after payment.


**Option B: Stripe Checkout (minimal backend required)**


Stripe Checkout is a hosted payment page that you redirect users to from your app. Your backend creates a checkout session with the Stripe API, returns a URL, and the user completes payment on Stripe's servers. No custom payment form required — PCI compliance is Stripe's responsibility.


After payment, Stripe redirects the user to your` success_url` . But you still need webhooks to reliably unlock features — redirects can fail if the browser closes.


**Option C: Webhooks (required for subscriptions)**


Webhooks are HTTP POST requests that Stripe sends to your server when events happen. The critical ones:


- ` payment_intent.succeeded` — one-time payment completed
- ` customer.subscription.created` — new subscription started
- ` customer.subscription.updated` — plan changed or payment method updated
- ` customer.subscription.deleted` — subscription cancelled
- ` invoice.payment_failed` — payment failed (90% of subscription revenue loss comes from failed payments — handle this)


Your webhook endpoint receives these events and updates your database. This is what upgrades users from "free" to "paid" in your app.


## Step 3: Set Up Webhooks


Stripe webhooks connecting payment events to your app's database in real time


Blink


### With a standard backend (Express example)


Your webhook endpoint needs to:


1. Verify the request came from Stripe (using` stripe.webhooks.constructEvent` )
2. Handle the relevant event types
3. Update your database


A minimal webhook handler looks like this:


```text
// POST /webhooks/stripe
app.  post  (  '/webhooks/stripe'  , express.  raw  ({ type:   'application/json'   }),   async   (  req  ,   res  )   =>   {
const   sig   =   req.headers[  'stripe-signature'  ];
let   event;


try   {
event   =   stripe.webhooks.  constructEvent  (req.body, sig, process.env.  STRIPE_WEBHOOK_SECRET  );
}   catch   (err) {
return   res.  status  (  400  ).  send  (  `Webhook Error: ${  err  .  message  }`  );
}


switch   (event.type) {
case   'customer.subscription.created'  :
case   'customer.subscription.updated'  :
const   subscription   =   event.data.object;
await   db.users.  update  ({
where: { stripe_customer_id: subscription.customer },
data: { subscription_status: subscription.status }
});
break  ;
case   'customer.subscription.deleted'  :
await   db.users.  update  ({
where: { stripe_customer_id: subscription.customer },
data: { subscription_status:   'cancelled'   }
});
break  ;
}


res.  json  ({ received:   true   });
});
```


Two common mistakes here:


- **Not verifying the signature** — anyone can POST to your webhook URL. The` constructEvent` check prevents fake events.
- **Missing idempotency** — Stripe retries failed webhooks. Your handler must be idempotent: processing the same event twice should not double-charge or double-upgrade a user.


### With Blink


With[Blink](https://blink.new/) , the database to store payment records is included automatically. When you prompt Blink to add Stripe billing, it generates the webhook endpoint, configures the event handlers, and creates the database columns — all in one step. You still need to add your Stripe API keys to Blink's environment variables, but the wiring is handled automatically.


## Step 4: Connect Stripe Customers to Your Users


The most common integration bug: a payment succeeds, but the webhook has no way to know which user paid.


The fix is to create a Stripe customer ID for each user when they sign up, and store it in your users table:


```text
// When a user signs up:
const   customer   =   await   stripe.customers.  create  ({
email: user.email,
metadata: { user_id: user.id }
});


await   db.users.  update  ({
where: { id: user.id },
data: { stripe_customer_id: customer.id }
});
```


Then in your checkout session creation, pass that customer ID:


```text
const   session   =   await   stripe.checkout.sessions.  create  ({
customer: user.stripe_customer_id,
mode:   'subscription'  ,
line_items: [{ price:   'price_1234abc'  , quantity:   1   }],
success_url:   `${  process  .  env  .  APP_URL  }/dashboard?payment=success`  ,
cancel_url:   `${  process  .  env  .  APP_URL  }/pricing`  ,
});
```


Now your webhook can look up` event.data.object.customer` and find the right user every time.


With[Blink](https://blink.new/) , auth is built in — connecting Stripe customer IDs to your users happens automatically when you add the Stripe integration. The user ID to Stripe customer ID mapping is generated and maintained for you.


## Step 5: Test in Stripe Test Mode


Before going live, test every payment scenario using Stripe's test card numbers. Test mode uses your` pk_test_` and` sk_test_` keys — no real money moves.


Essential test cards:


- **` 4242 4242 4242 4242`** — successful payment (any future expiry, any 3-digit CVC)
- **` 4000 0000 0000 0002`** — card declined
- **` 4000 0025 0000 3155`** — requires 3D Secure authentication
- **` 4000 0000 0000 9995`** — insufficient funds


Test your webhook locally using[Stripe CLI](https://stripe.com/docs/stripe-cli) :


```text
stripe   listen   --forward-to   localhost:3000/webhooks/stripe
```


This forwards Stripe test events to your local server so you can verify the webhook handler works before deploying.


Going live with Stripe — checklist complete, real payments ready to accept


Blink


## Step 6: Go Live


Before switching to live keys, verify:


- Webhook signature verification is active (not commented out)
- All event types you handle are registered in your Stripe Dashboard (Developers → Webhooks → Add endpoint)
- Your` success_url` and` cancel_url` point to production URLs, not localhost
- Failed payment handling is live (` invoice.payment_failed` updates the user status)
- Your database has the` stripe_customer_id` column indexed for fast webhook lookups
- You have tested the full flow end-to-end in test mode including cancellation and failed payments


Replace your test API keys with live keys in your production environment. Switch your Stripe webhook endpoint URL to your production server. You are live.


## Common Mistakes to Avoid


**Not handling` invoice.payment_failed`**


90% of subscription revenue loss comes from failed payments. If your webhook ignores this event, users whose cards decline remain on the paid tier indefinitely. Always handle failed payments — at minimum, flag the account for review and send an email.


**Blocking on webhook success**


Do not make the user wait for your webhook to process before showing the success page. Redirect the user to` success_url` immediately after checkout. Process the webhook asynchronously — it can take a few seconds to arrive.


**Using redirect URL as the only confirmation**


If a user closes their browser after paying but before the redirect completes, the` success_url` never fires. Your webhook is the reliable source of truth. Build your app logic around webhook events, not redirect parameters.


**Skipping idempotency keys**


If you create a Stripe customer or checkout session in response to a user action, use idempotency keys to prevent duplicate records if the request is retried. Stripe supports this natively with the` Idempotency-Key` header.


## Frequently Asked Questions


For Stripe Payment Links, no — Stripe hosts the payment page and you share a URL. For subscriptions where your app needs to know about payments and unlock features accordingly, yes — you need a server-side webhook endpoint.[Blink](https://blink.new/) includes a backend automatically, so adding Stripe subscriptions with webhooks is a single prompt. Other AI app builders like Bolt generate only frontend code, meaning you need to add a backend separately.


Stripe charges 2.9% + $0.30 per successful card transaction in the US. There is no monthly fee. For international cards and some payment methods, additional fees apply. See[Stripe's pricing page](https://stripe.com/pricing) for current rates. The 2.9% + $0.30 rate is standard across all Stripe plans until you reach enterprise volume.


Payment Links are static URLs you generate in the Stripe Dashboard — no code required. They work for one-time purchases where you do not need to programmatically gate features after payment. Stripe Checkout is a hosted payment page your backend creates dynamically, allowing you to pass customer information and receive reliable webhook confirmation. For subscription SaaS apps, Checkout + webhooks is the standard approach.[Blink](https://blink.new/) uses Checkout + webhooks automatically when you add subscription billing.


Webhooks are the only reliable way Stripe notifies your server that a subscription was created, renewed, or cancelled. URL redirects after checkout can fail — users close browsers, connections drop, ad blockers sometimes block redirects. Webhooks are server-to-server: Stripe sends the event directly to your webhook endpoint regardless of what the user's browser does. For subscription apps, ignoring a` customer.subscription.created` event means the user paid but your database never updated.[Blink](https://blink.new/) generates the webhook handler automatically when you add billing.


Yes, but it requires adding a backend that Bolt and Lovable do not generate. The typical path: export your Bolt or Lovable code to GitHub, add a Next.js API route or Express server for the webhook endpoint, connect Supabase for the database, and configure Stripe manually. That is 4-8 hours of setup. Alternatively, rebuild in[Blink](https://blink.new/) — Blink includes the backend and database automatically, so the Stripe webhook integration is a single prompt. See also:[how to build a membership site](https://blink.new/blog/how-to-build-a-membership-site) .


Be specific about your plan structure and where in the UI it should appear. Example: "Add Stripe subscription billing to this app — $9/month Basic plan and $29/month Pro plan. Allow users to subscribe from their profile page. When a user subscribes, update their plan field in the database. When a subscription is cancelled, revert their plan to free. Add a webhook endpoint for customer.subscription.created, customer.subscription.updated, and customer.subscription.deleted events."[Blink](https://blink.new/) will generate the full integration including the webhook handler, database columns, and frontend UI.


If you are building a SaaS app with subscriptions, Stripe + a connected backend is the required combination — there is no shortcut past the webhook. The shortcut is choosing a builder that includes the backend. See[how to build a SaaS app in a weekend](https://blink.new/blog/build-saas-in-a-weekend) and[how to build a membership site with AI](https://blink.new/blog/how-to-build-membership-site) for what to build next.
