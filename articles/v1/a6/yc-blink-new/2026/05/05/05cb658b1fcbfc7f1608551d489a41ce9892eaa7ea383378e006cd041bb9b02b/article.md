---
schema_version: "1.0.0"
document_id: "05cb658b1fcbfc7f1608551d489a41ce9892eaa7ea383378e006cd041bb9b02b"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-a-marketplace"
published_at: "2026-05-04T12:20:00+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:15:26.754194+00:00"
content_hash: "sha256:5dbfe0ab15be692486ff70573c533b834882a8a5216071f78a49f0eb3793f902"
---

# How to Build a Marketplace App With AI (Buyers, Sellers, Payments)

## The Exact Blink Prompt


Open[blink.new](https://blink.new/) and paste this verbatim:


```text
Build me a two-sided marketplace where:
- Sellers can create an account, list products with photos, description, price, and quantity
- Buyers can browse listings, search and filter by category/price, save favorites
- Shopping cart and Stripe checkout for purchases
- Messaging between buyers and sellers
- Review system (buyers can leave star ratings for completed purchases)
- Separate dashboards: sellers see their listings and orders, buyers see their purchases
- Admin view: see all listings, users, and transactions


```


Blink builds the full application — database automatically included, auth built in for both buyer and seller roles, file storage for listing photos. No Supabase account needed. No Vercel config. Full-stack from day 1.


Building marketplace components — character with hard hat surrounded by assembling UI elements


Blink


## Step-by-Step: Building Your Marketplace


1


#### Get the core loop working first


After the initial build, test this before anything else: can a seller post a listing, and can a buyer see and purchase it? Walk the complete loop — post → find → buy → confirm. If that works, everything else is iteration.


2


#### Set up Stripe Connect


Marketplace payments require Stripe Connect, not standard Stripe. Tell Blink: "Set up Stripe Connect for marketplace payments. When a buyer pays, automatically send 95% to the seller's connected Stripe account and keep 5% as platform fee." Sellers connect their Stripe accounts from their dashboard. Blink handles the OAuth flow.


3


#### Add multi-image upload for listings


Multiple listing photos drive conversion. Tell Blink: "Add multiple image upload to listing creation — up to 6 photos per listing, drag-and-drop reorder, first image is the thumbnail." Blink's file storage handles this automatically.


4


#### Build the review system


"Add a review system: buyers can leave a 1–5 star rating and written review after order is marked complete. Show the seller's average rating with review count on every listing." This is non-negotiable for trust.


5


#### Test the full transaction loop


Use Stripe's test card (4242 4242 4242 4242). Walk through: seller posts listing → buyer finds it → buyer pays → seller receives order → order marked complete → buyer leaves review. This is the trust loop your marketplace runs on.


## Marketplace Types This Builds


The same architecture handles many marketplace categories. After the core build, describe your variation in plain English:


**Physical goods** — handmade products, vintage items, art. Add shipping address field and tracking number input to the order flow.


**Digital products** — templates, fonts, audio, ebooks. After payment, Blink serves the file download automatically. No S3 bucket to configure.


**Services** — freelancers, coaches, consultants. Replace "add to cart" with "book a session" and connect calendar availability.


**Rentals** — equipment, vehicles, space. Add date range selection and availability calendar. Deposits handled through Stripe.


**Local classifieds** — add location-based filtering and "contact seller" flow instead of checkout.


## Monetization Models for Marketplaces


Three models work for marketplace businesses. Pick based on your category:


**Transaction fee (5–15% of each sale)** — the Airbnb/Etsy model. Aligns your incentives with seller success. Standard for physical and service marketplaces. Most new marketplaces start at 5–8%.


**Subscription (sellers pay monthly)** — the LinkedIn Jobs model. Predictable revenue, but creates friction for new sellers. Works when your buyer audience is valuable enough to justify the fee.


**Listing fee** — sellers pay per listing posted. Low friction, but revenue only comes when supply is added. Good for high-volume, low-price categories.


## What Blink Handles So You Don't Have To


The parts that kill marketplace builds aren't the features — they're the infrastructure.


Auth handles buyer and seller roles automatically. Sellers log in to the seller dashboard. Buyers land on the buyer dashboard. Admins see everything. No Firebase Auth configuration. No JWT rules to write.


The database is included automatically. No Supabase account. No connection strings to manage. No migrations to write when you add a new listing field.


File storage for listing photos is built in. Sellers upload images directly. No S3 bucket. No presigned URL logic.


Hosting is included — your marketplace ships with a` *.blink.new` URL in the same session you built it. No Vercel config. No deploy script.


For broader context on building with AI, see[how to build a SaaS app with AI](https://blink.new/blog/build-saas-app-with-ai) and[vibe coding for non-technical founders](https://blink.new/blog/vibe-coding-for-non-technical-founders) . For related architecture,[how to build a job board](https://blink.new/blog/how-to-build-a-job-board) and[how to build a membership site](https://blink.new/blog/how-to-build-a-membership-site) both share the same two-sided user model.


Marketplace analytics — character pointing at sales dashboard and upward graph


Blink


## Frequently Asked Questions


The core marketplace — two user types, listings, search, checkout, reviews, and dashboards — builds in 2–4 hours with Blink. Category-specific customizations (service bookings, digital downloads, rental calendars) add another 1–2 hours. Most founders have a working MVP the same day they start. Traditional development takes 3–9 months and $40,000–300,000.


Yes, if you want split payments between buyers, sellers, and your platform fee. Standard Stripe only accepts payments to one account. Stripe Connect handles multi-party transactions: collects from buyers, routes to seller accounts, deducts your platform fee automatically. It's free to set up — you pay Stripe's standard 2.9% + $0.30 per transaction.


Auth is built in and handles both roles automatically. When you describe your marketplace, Blink creates separate login flows, separate dashboards, and separate permission sets for buyers and sellers. Admins get full access. No Firebase Auth, no Auth0, no custom JWT rules needed.


Standard marketplace fees range from 3% for high-volume commoditized categories to 20% for specialized, high-trust verticals. Most new marketplaces launch at 5–8% — enough to be sustainable without deterring early sellers. Start low and increase as your buyer audience proves valuable to sellers.


Seed one side first. Launch with no buyers — build your seller supply with free listings, a strong directory, and direct outreach. Once buyers arrive, they find a full catalog. The reverse (launching with buyers and no supply) rarely works. Focused niches solve this faster than broad categories.


Yes. After the initial build, tell Blink to replace the shopping cart with a booking flow. Add availability calendars, session scheduling, and message threads for scope discussion. The auth and payment infrastructure stays the same — Stripe Connect handles service payments the same way it handles product payments.


Blink uses Postgres under the hood, which handles millions of listings and users without configuration changes. The database scales with your marketplace automatically. If you outgrow the Blink platform, you can export your data and self-host — the code is yours.
