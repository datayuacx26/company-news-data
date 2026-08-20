---
schema_version: "1.0.0"
document_id: "0a0213d5b42d348027c704887fbdcc6f12b5b8b879f65409d65e3eb77bd85996"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-restaurant-ordering-app"
published_at: "2026-05-05T00:47:45+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:40.928893+00:00"
content_hash: "sha256:397a5141980ade0dfad192731d210a4449be9cd777e9580ab8c1d88e7be5c689"
---

# How to Build a Restaurant Ordering App (Without Giving 30% to DoorDash)

## Step 2: Configure Your Menu


After Blink generates the app, you need real menu data. Ask Blink to build the admin menu editor:


```text
Add an admin panel where restaurant staff can:
- Add menu items with name, description, price, photo URL, and category
- Set items as available or unavailable (86'd items)
- Reorder items within categories
- Create and rename categories


```


Menu data lives in Blink's Postgres database. Categories and items are stored with a sort order, so the admin drag-and-drop reorder writes back to the database immediately. No CSV imports, no CMS — the admin panel is your menu management tool.


## Step 3: Set Up Payments with Stripe


Blink integrates with Stripe through its payment infrastructure. Ask:


```text
Integrate Stripe Checkout for order payments.
When a customer places an order, create a Stripe Checkout session
for the order total. On successful payment, create the order record
and send a confirmation email. Store the Stripe payment intent ID
with the order for refund handling.


```


Stripe's[standard commission structure](https://get.doordash.com/en-us/merchant/pricing) for DoorDash runs 15–30% per transaction, plus potential tablet fees and marketing spend. Stripe charges 2.9% + 30¢ per transaction. On a $30 order, that's $1.17 to Stripe versus up to $8.40 to DoorDash. The difference is your margin.


You will need a Stripe account and API keys. Get them at stripe.com/register. Add your Stripe secret key to Blink's environment variables panel — it never appears in your generated code.


## Step 4: Real-Time Order Notifications


An ordering app without order alerts is a missed ticket. Ask Blink to add order notifications:


```text
When a new order is placed and payment confirmed:
1. Send a push notification to the restaurant admin dashboard
2. Play an alert sound in the open admin tab
3. Show the order at the top of the "Incoming Orders" queue
4. Email the restaurant address with order details


```


The admin dashboard becomes a live queue. New orders appear at the top with item details, special instructions, and the customer's name. Staff mark orders as "In Progress" and "Ready" — the status pushes back to the customer's order tracking screen.


Restaurant staff managing digital orders on a tablet dashboard


Blink


## Step 5: Customer Accounts and Repeat Orders


The competitive advantage of owning your platform is owning your customer list. A third-party delivery app owns that data. Your app does not.


Ask Blink to build the customer account layer:


```text
Add customer accounts with:
- Email/password registration and login
- Save delivery addresses
- Order history with the ability to reorder with one tap
- Email receipts automatically sent on order confirmation


```


Customer accounts are managed by Blink's auth layer — you do not build session handling or password hashing. The customer's order history lives in your Postgres database, queryable and exportable. You can run a promotion to every customer who ordered in the last 30 days. DoorDash cannot give you that list.


## Step 6: Delivery Zone Mapping (Optional)


If you deliver, add a delivery zone check before checkout:


```text
Add a delivery zone validator. At checkout, ask for the customer's
delivery address. Check whether the address falls within [your ZIP codes
or a defined radius from your address]. If outside the zone, show a
"Pickup only" message and allow the order to proceed as pickup.


```


The delivery zone logic runs server-side in Blink's backend. No Google Maps billing required for zone validation — a simple ZIP code list or haversine distance check handles 95% of restaurant delivery zones.


## Step 7: Launch With a Custom Domain and QR Code Menu


Your app ships with a Blink subdomain immediately. Connect your own domain in the Blink dashboard — no DNS configuration knowledge required.


For table ordering, generate a QR code that links directly to your menu URL. Free QR code generators like qr-code-generator.com create print-ready codes in 30 seconds. Print them on table cards, receipts, or your front window.


Staff the launch by testing the full order flow before going live: place a test order, confirm Stripe test mode shows the charge, verify the admin panel receives the notification, and mark the order complete. Then switch Stripe from test mode to live mode.


1


#### Place a test order in Stripe test mode


Use Stripe's test card number 4242 4242 4242 4242 with any future expiry and any 3-digit CVC. Verify the order appears in your admin queue.


2


#### Confirm the notification flow


Check that the order notification appears in the admin tab, the confirmation email sends to the customer address, and the status updates show in the customer's order tracking screen.


3


#### Switch Stripe to live mode


In your Blink environment variables, replace the Stripe test secret key with your live secret key. Replace the test publishable key in the frontend environment. Place a $1.00 real transaction to confirm end-to-end.


4


#### Connect your custom domain


Add your domain in the Blink dashboard. Point your domain's DNS records to Blink's nameservers or add the CNAME record shown. SSL is provisioned automatically.


5


#### Print and deploy QR codes


Generate QR codes pointing to your menu URL. Deploy table cards and update your social media links to point to the ordering page.


## What This Costs vs. DoorDash


DoorDash Your Blink App


Commission per order 15–30% 0%


Monthly platform fee $0 (but you pay per order) Blink Pro from $20/mo


Stripe processing Included in their take 2.9% + 30¢ per transaction


Customer data ownership DoorDash owns it You own it


Custom branding Limited Complete


Setup time Same day (but locked in) 1–2 hours


On $10,000/month in orders, DoorDash at 25% takes $2,500. Stripe at 2.9% + 30¢ on 200 average orders of $50 each takes $640. Your Blink subscription at $20/month is $20. You keep an additional $1,840 every month — before considering the value of owning your customer list.


## Frequently Asked Questions


No coding knowledge is required. Blink generates the full-stack app from your natural-language description — menu management, order queue, Stripe payments, and customer accounts. You interact with the builder through chat prompts, not code. If you want to make changes after the initial build, you ask Blink to update specific features the same way.


Yes. Blink generates responsive apps that work on mobile browsers — no separate app download required. Your customers open the URL on their phone and get the full ordering experience. For a native iOS or Android app, ask Blink to build with a mobile-first layout and generate a progressive web app (PWA) manifest so customers can add it to their home screen.


Stripe deposits your earnings to your linked bank account on a daily or weekly schedule you set in your Stripe dashboard. Funds are typically available 2 business days after the transaction. You receive the full order amount minus Stripe's 2.9% + 30¢ processing fee — no platform markup on top.


Yes. Run both in parallel until your direct ordering volume proves sustainable. Many restaurants offer a small incentive — a free drink or 10% off — for customers who order direct. Once your direct ordering app builds a customer base, the commission savings compound monthly. The transition is gradual, not a switch.


Blink's hosting scales with traffic — you do not manage servers. The admin order queue shows all incoming orders in real time with timestamps. You can add an "order throttling" feature that limits new orders when the queue exceeds a threshold, or adds estimated wait times to the customer checkout screen.


Yes. Ask Blink to add staff accounts with an "admin" role separate from customer accounts. Each staff member logs in with their own credentials. You can restrict who can modify the menu versus who can only manage orders.[See how to build subscription and multi-user features →](https://blink.new/blog/how-to-build-subscription-app-stripe)
