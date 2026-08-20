---
schema_version: "1.0.0"
document_id: "b767f6ddaac0dfef95d9f925fd4637d01ac9c6fe0fed490f8411662719b1dd3d"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-referral-program"
published_at: "2026-05-24T01:43:45+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:f429749453d3af11fa23b8712073e1c35ccebf3a86cd2823c96602d9d5b6aafa"
---

# How to Build a Referral Program With AI (The Dropbox Model, No Code Required)

## Build It: Six Steps


1


#### Set up the data model


Start a Blink app and prompt it to create three tables:` referrals` (referrer_id, referred_user_id, status, created_at, converted_at),` rewards` (user_id, type, amount, status, referral_id, issued_at), and a` referral_code` column on your users table. Blink generates the schema and an admin panel to query all three. The database is included — no Supabase account needed, no external DB to configure.


2


#### Generate referral codes on signup


Prompt Blink to auto-generate an 8-character alphanumeric` referral_code` whenever a new user is created. Expose a public page at` /refer/\[code\]` . When a visitor lands on that URL, store the referral code in a cookie with a 7-day expiry. This makes attribution survive multi-session signups — someone can click your link on Monday and complete registration on Thursday and still be counted. Auth is built in, so user IDs are already available for the code lookup.


3


#### Wire up conversion attribution


On every new user signup, check for a referral cookie. If one exists: look up the referrer by code, create a referral record with` status: 'pending'` , and store` referred_by_user_id` on the new user. Then add a conversion hook on your defined event — first payment, first feature activation, or whatever your product defines as "success." When that event fires, update the referral to` converted` and trigger reward creation for both sides.


4


#### Build the reward handler


Describe the reward logic to Blink: on conversion, create reward records for both the referrer and the referred user with` status: 'pending'` . Add a cron job that runs every five minutes — it finds pending rewards with no` issued_at` , adds credits to each user's balance (or calls your Stripe payout endpoint for cash rewards), and marks each reward` issued` . Blink generates the cron scheduler automatically. No separate queue service or external job runner needed.


5


#### Build the user-facing share page


Ask Blink to build a` /dashboard/referrals` page: the user's unique share URL, a copy-to-clipboard button, pre-filled tweet and email templates, current reward balance, and a list of their successful referrals with statuses. If you want milestone mechanics — "refer 3 friends to unlock Pro for a month" — add a progress bar tied to the converted referral count. That visible counter was the mechanic that kept Dropbox's storage bar driving continued sharing.


6


#### Build the admin analytics view


Ask Blink to generate an admin page showing: total referrals this week and this month, referral conversion rate versus organic conversion rate, top referrers by count, pending versus issued reward totals, and a cohort LTV comparison between referred users and non-referred users. This view is what gets the program prioritized internally. When the team can see that referred users are worth 2-3× more and cost less to acquire, the referral program stays maintained.


Total build time: 3-5 hours. Zero separate SaaS subscriptions. Referral data lives in your app's own database — queryable alongside everything else you know about your users, with no API calls to an external service.


Custom referral program generating viral signups


Blink


## Build vs Buy: What You're Actually Choosing Between


ReferralHero Viral Loops Build with Blink


Monthly cost $199–$399 $99–$279 Blink plan


Member limits 10k–50k by tier 10k–25k by tier Unlimited


Data ownership Vendor Vendor **Yours**


Custom reward types Limited Limited **Unlimited**


Fraud rules Fixed, built-in Fixed, built-in Custom — you write the rules


App integration API / webhook JS widget **Native — same app, same DB**


Build time Minutes Minutes 3-5 hours


The tradeoff is honest: SaaS tools are faster to set up. A custom build takes longer but gives you full data ownership, unlimited member scale, native integration with your product, and the ability to define exactly what triggers a reward and what triggers a fraud flag.


If referrals are a core growth mechanic — not a side experiment — owning the implementation is the right call.


For more on building full-stack apps with AI, the[build a SaaS app guide](https://blink.new/blog/build-saas-in-a-weekend) covers the broader application architecture. The[waitlist app guide](https://blink.new/blog/how-to-build-waitlist-app) covers adjacent mechanics — viral waitlists use the same referral-link-plus-reward pattern with a queue instead of a signup. If you're still evaluating tools, the[best AI app builders comparison](https://blink.new/blog/best-ai-app-builders) breaks down what each platform includes out of the box.


## Frequently Asked Questions


The core mechanics — data model, referral link generation, attribution, and reward logic — take 3-5 hours in Blink. A full admin analytics dashboard adds another 1-2 hours. That compares favorably to integrating a SaaS tool via webhook, writing custom reward logic on top of it, and maintaining the integration when their API changes.


No. Blink handles the database schema, backend logic, cron scheduling, and hosting automatically. You describe what you want — "create a referral record when a new user signs up via referral link, then trigger a reward when they complete their first payment" — and Blink generates the implementation. Auth is built in, so referral codes tie to existing user accounts without any additional setup.


The core difference is data ownership and integration depth. SaaS tools store your referral data on their servers, enforce member limits by pricing tier, and connect to your app via webhooks. A custom build stores everything in your own database, has no member limits, and the referral logic runs as native application code — so you can query referral attribution alongside billing data, churn signals, and product usage without any API round-trips.


When a new visitor lands on a` /refer/\[code\]` URL, you store the referral code in a cookie with a 7-day expiration window. On signup, your app checks for that cookie, looks up the referrer by code, and creates a referral record linking the two users. The 7-day window covers most real-world signup flows — someone can click the link, explore the product, and sign up days later while still being correctly attributed.


Two-sided rewards — where both the referrer and the referred user receive something — consistently outperform one-sided programs by roughly 2×. Dropbox gave both sides free storage, directly tied to product value. The strongest reward types are product credits (free month, bonus usage) rather than cash, because they bring people back to the product. Define your conversion event carefully: rewarding on signup is cheap to game; rewarding on first payment or first meaningful activation is fraud-resistant.


The most common patterns: self-referrals using different email addresses, fake account signups to harvest rewards, and coordinated abuse from the same IP range or email domain. Build in: reject referrals where the new user's IP matches the referrer's IP, add a 24-48 hour minimum between referral and reward eligibility, and flag clusters of referrals from the same email domain for manual review. You can implement all of these as custom rules in your Blink app — something fixed-rule SaaS tools don't let you do.


Yes. The reward handler creates separate reward records for both the referrer and the referred user on conversion. You define the reward type and amount independently for each side — a common pattern is a larger reward for the referrer and a welcome discount for the new user, since the discount reduces friction on their first conversion event. Both reward records go through the same pending → approved → issued pipeline and appear in the admin analytics view.
