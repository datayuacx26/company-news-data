---
schema_version: "1.0.0"
document_id: "da688c0c51b37016d487f2feea4cade39d6768e9b80f37679b5a68bbf81199b1"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-subscription-box-app"
published_at: "2026-05-27T13:10:01+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:3ac83948ee2d038c45c640fcadeb95aa9a61d73d26d292a45f7d56d2587be826"
---

# How to Build a Subscription Box App with AI (Payments, Inventory, and Shipping)

## What Your App Needs vs What Blink Includes


Requirement What it takes What Blink includes


Subscriber database PostgreSQL or similar ✅ Auto-provisioned


Auth (login/register) Clerk, Firebase Auth, or custom ✅ Built in


Stripe recurring billing Stripe API + webhook handling ✅ Stripe keys in env vars


Admin dashboard Custom React + API ✅ Describe and generate


Customer portal Separate build ✅ Describe and generate


Hosting + SSL Vercel, Railway, or similar ✅ Included


Blink is a full-stack AI app builder — database, auth, and hosting are all included. No separate Supabase account. No Vercel config. No DevOps. You describe your subscription box platform and Blink generates it: UI, API, database schema, Stripe integration, and all.


## Build Your Subscription Box Platform


1


#### Set up subscriber accounts and subscription plans


Start with the core data model — what a subscriber is and what plans they can be on:


> "Create a subscription box platform. Build a subscribers table with: email, name, shipping address, billing status (active, paused, cancelled, past_due), and plan (monthly, quarterly). Create a plans table with: plan name, price, billing interval, and trial days. Build signup and login with email/password. After login, show the subscriber's current plan and next billing date."


Blink generates the database tables, authentication flow, and subscriber dashboard. The database is automatically provisioned — no connection strings to configure.


2


#### Configure Stripe recurring billing


> "Integrate Stripe for recurring subscriptions. On signup, create a Stripe customer and attach the selected plan as a subscription. Handle these webhook events: payment_succeeded (mark subscription active), payment_failed (mark as past_due and send email), subscription_canceled (mark as cancelled). Store the Stripe subscription ID and current period end date on each subscriber record."


Add your Stripe secret key and webhook signing secret in the Environment Variables panel — two fields, no config files. Blink handles the webhook endpoint, signature verification, and all subscription lifecycle events automatically.


For a deeper look at Stripe subscription patterns, see our guide on[building a Stripe subscription app](https://blink.new/blog/how-to-build-stripe-subscription-app) .


3


#### Build inventory tracking and SKU management


> "Add inventory tracking. Create a products table with: SKU, name, description, quantity on hand, quantity reserved, and reorder threshold. Each box cycle should reserve inventory when orders are generated and deduct when shipped. Show a low-stock warning when available quantity (on hand minus reserved) drops below the reorder threshold."


The` on_hand - reserved = available` model prevents overselling. When a subscriber's renewal generates an order, inventory gets reserved immediately. When you mark the shipment sent, the reservation converts to a deduction.


> "Add an admin inventory panel showing all SKUs, current stock levels, and how many units are reserved for the upcoming cycle. Add a 'Receive stock' button that logs a receiving event and updates on-hand quantity."


4


#### Add cycle management and shipping dates


> "Add a box cycle system. Each cycle has a start date, billing cutoff date, shipping cutoff date (last day subscribers can change address or pause), and target ship date. Generate orders for all active subscribers when a cycle starts billing. Show each cycle's status: upcoming, billing, fulfillment, shipped, complete."


Two cutoff dates matter — billing cutoff (when charges run) and shipping cutoff (when address changes lock). Keep them separate. Address changes after the shipping cutoff go to the next cycle, not the current one.


> "Add a cutoff rules system. When a subscriber tries to update their shipping address and the shipping cutoff has passed, show a message that the change applies to the next cycle and save it for next time."


5


#### Build pause, cancel, and resume flows


> "Add subscriber self-service controls. Let subscribers: pause their subscription (choose resume date from 1–3 months out), cancel (with a 'reason for cancelling' prompt), and resume a paused subscription early. When paused, skip billing and order generation for that subscriber until the resume date. When cancelled, keep their account and history but stop all future billing."


The pause flow is the single most effective churn-reduction tool in subscription commerce. Subscribers who want to cancel often accept a 1-month pause instead. The data on this is consistent across the industry — offer it prominently.


6


#### Build the customer portal


> "Build a customer-facing portal. Show: current plan and next billing date, upcoming box cycle status, shipping address with an edit button, billing history with downloadable receipts, and a 'Manage subscription' section with pause/cancel/upgrade options. If the subscription is past_due, show a 'Update payment method' button that opens a Stripe billing portal."


The Stripe billing portal handles payment method updates without you storing card data. One line of code redirects subscribers to a Stripe-hosted page where they update their card, then returns them to your portal.


## Stripe Subscription Setup: Monthly, Quarterly, and Trials


Stripe subscriptions in your custom box app — no platform commission, full control


Blink


Stripe recurring billing has three patterns worth setting up from the start.


**Monthly and quarterly billing** are straightforward price objects in Stripe. The prompt:


> "Create two Stripe products: 'Monthly Box' at $39.99/month and 'Quarterly Box' at $34.99/month (billed as $104.97 every 3 months). On signup, let the subscriber choose their billing interval. Store the Stripe price ID on each subscription record."


**Trial periods** give new subscribers a free first cycle before billing starts. This consistently increases signup-to-paid conversion rates:


> "Add a 14-day free trial to new monthly subscriptions. During trial, create the Stripe subscription but don't charge until the trial ends. Show a trial badge in the subscriber portal with days remaining."


**Failed payment handling** is where most platforms lose subscribers silently. Set up proper dunning:


> "When a payment fails, update the subscriber status to past_due. Send an email with a link to update their payment method. Retry the charge after 3 days, then after 5 more days. If payment still fails after the second retry, set status to cancelled and stop shipment generation."


Stripe handles the retry logic natively — your app just needs to listen to the right webhook events (` invoice.payment_failed` ,` invoice.payment_action_required` ) and update subscriber status accordingly.


For more on the full Stripe subscription pattern including proration and upgrade flows, see[how to build a membership site](https://blink.new/blog/how-to-build-membership-site) .


## Inventory and Shipping Integration


The two inventory fields that prevent most subscription box problems are` reserved` and` available_to_promise` :


```text
on_hand: 340       (physical stock in warehouse)
reserved: 285      (allocated to upcoming cycle orders)
available: 55      (available_to_promise = on_hand - reserved)


```


When the cycle billing runs and orders generate,` reserved` increases by the subscriber count. When you mark orders shipped,` reserved` decreases and` on_hand` decreases by the same amount. The` available` number tells you whether you can add more subscribers to this cycle.


**Shipping date management** connects cycle cutoffs to actual carrier bookings. The prompt:


> "Add a shipping management panel. For each cycle in 'fulfillment' status, show a list of orders with: subscriber name, shipping address, order status (pending, packed, shipped), and tracking number field. Add bulk actions: mark selected orders as packed, mark as shipped (with tracking number), and export a CSV for carrier upload. When all orders are shipped, mark the cycle complete."


**Address validation** catches the common problems before ship day:


> "Before generating shipping labels for each cycle, validate all subscriber addresses. Flag orders with: missing apartment/unit number, unrecognized ZIP code, or PO Box (if you don't ship to PO Boxes). Show flagged orders in a separate queue with edit and override options."


Address problems caught on day 1 of the cycle cost nothing. Caught on ship day, they cost a full cycle's delay for that subscriber.


## The Database Schema


Blink provisions the database automatically. For a subscription box platform, these are the core tables:


Table Key columns


` subscribers` id, email, name, stripe_customer_id, status, plan_id


` subscriptions` id, subscriber_id, stripe_subscription_id, status, current_period_end


` plans` id, name, stripe_price_id, interval, price, trial_days


` box_cycles` id, name, billing_cutoff, ship_cutoff, target_ship_date, status


` cycle_orders` id, cycle_id, subscriber_id, status, shipping_address


` products` id, sku, name, on_hand, reserved, reorder_threshold


` shipments` id, order_id, carrier, tracking_number, shipped_at


None of these tables require you to open a database console or write SQL migrations. Describe the data structure, Blink creates it.


## Expanding the Platform


Once your core cycle is running, three features consistently reduce churn and increase order value:


**Subscriber referral codes** — give each subscriber a unique code, credit their account when someone signs up with it:


> "Add a referral system. Each subscriber gets a unique referral code. When a new subscriber signs up using a code, give the referring subscriber $10 account credit applied to their next renewal. Track referral counts in the subscriber portal."


**Add-on products** — let subscribers add individual products to their upcoming box:


> "Add an 'Add to my box' feature. Show available add-on products before the shipping cutoff. When a subscriber adds a product, create a line item on their cycle order and charge them the add-on price immediately. Deduct add-on quantity from inventory when the order generates."


**Box customization** — let subscribers choose box variants or swap items:


> "Add a box customization flow. Before each cycle's shipping cutoff, email subscribers a link to customize their box. They can choose between variant options (scent, size, style). Store their choices on the cycle order. Show customization status in the admin cycle view."


For a comparison of the e-commerce app approaches, see[how to build an e-commerce app with AI](https://blink.new/blog/how-to-build-ecommerce-app-with-ai) .


A custom subscription box platform with subscriber management and Stripe recurring billing


Blink


No. The entire build in this guide uses plain-English prompts — no HTML, SQL, or JavaScript required. Blink generates the full-stack app: database schema, API endpoints, subscriber portal, admin panel, and Stripe integration. You add your Stripe keys in the environment variables panel and the billing runs automatically.


Blink is free to start — no credit card required. Cratejoy charges a 15% referral fee on every order (initial and renewals), plus 2.9% + $0.30 payment processing on marketplace orders. On 500 subscribers at $40/month, Cratejoy fees run roughly $3,600/month. With a custom build on Blink, your only ongoing cost is Stripe's payment processing (2.9% + $0.30) — no platform cut, no referral fee.


Stripe Subscriptions handles the billing schedule independently. Once you create a subscription for a customer, Stripe charges them on the interval you set (monthly, quarterly) and sends webhook events to your app: payment succeeded, payment failed, subscription canceled. Your app listens to those events and updates subscriber status. No cron jobs needed on your side — Stripe owns the billing clock.


Stripe retries failed payments automatically on a schedule you configure (default: 3, 5, and 7 days). Each retry sends a` invoice.payment_failed` webhook. Your app updates the subscriber to` past_due` status and sends a payment update email. If all retries fail, Stripe cancels the subscription and sends` customer.subscription.deleted` — your app marks the subscriber cancelled and stops order generation.


Yes, and you should build this prominently. The pause flow in Step 5 lets subscribers choose a resume date 1–3 months out. Subscribers who see a pause option convert at significantly higher rates than those who only see "cancel." Stripe supports pausing subscriptions natively — charges stop during the pause period and resume automatically on the date the subscriber selects.


The data model in this guide supports multiple box types through the` plans` table. Each plan can reference different box compositions — monthly subscribers get Box A contents, quarterly subscribers get Box B. Inventory reservations tie to the specific cycle order, so the system tracks which SKUs are allocated to which tier independently. You prompt Blink: "Add box tier variants — monthly plan uses SKU set A, quarterly plan uses SKU set B" and it updates the schema and fulfillment logic.


Yes. Export your subscriber list as a CSV (email, name, shipping address, plan, next billing date). Import into your Blink app's subscriber table. For active Stripe subscriptions, you can migrate customers to your new Stripe account using Stripe's migration tools — customers keep their payment method and billing date without re-entering card details. The bulk import prompt: "Add a CSV import tool for subscribers — map columns: email, name, address, plan, next_billing_date. Create Stripe customers for each imported subscriber."
