---
schema_version: "1.0.0"
document_id: "8e7682340f31b9de5a539abb103188624a1465a16704678883eaad596844fbd9"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-a-waitlist"
published_at: "2026-06-13T12:58:10+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:48:58.638835+00:00"
content_hash: "sha256:8916522059e5322c5d5124faa1494487cbd5d38ddde7011f03d26eeb7dec52fd"
---

# How to Build a Waitlist Landing Page with AI (And Grow It From 0 to 1,000)

## How to Build With Blink


### Start Prompt


Paste this into Blink to scaffold your waitlist:


```text
Build a waitlist landing page. Users sign up with their email.
After signup, show them their position number and a unique referral link.
When someone signs up through their referral link, the referrer moves up one spot.
Admin dashboard at /admin shows: total signups, daily chart, top referrers,
and a searchable member table with position and referral count.


```


This single prompt generates: the landing page, email capture, position assignment logic, referral link generation, referral tracking, and the admin dashboard. Database included (no Supabase). Auth for admin login included (no Clerk). Hosting included (no Vercel).


### Adding the Viral Referral Loop


After the initial scaffold, add:


```text
Add a "your impact" widget on each member's profile page showing how many
friends they referred and how many spots they moved up. Add a referral
leaderboard to the admin dashboard showing the top 10 referrers.


```


Gamelification and social proof keep members engaged between signup and launch.


### Automating Confirmation Emails


```text
Send an automated confirmation email when someone joins. Include:
their position number, their unique referral link, and one sentence
describing what they're waiting for.


```


Blink uses its included email infrastructure — no SendGrid API key or configuration needed.


### Building the Admin Dashboard


```text
Add a CSV export button to the admin dashboard that downloads all members
with their email, position, referral count, and signup date.


```


Waitlist admin dashboard showing referral tracking, daily signups, and position counts


Blink


## Proven Waitlist Tactics That Work


**Moving position counter on the homepage.** Show the current waitlist size live: "Join 4,847 people waiting." Update it in real time as new people sign up. The moving number creates urgency without false scarcity.


**First-100 exclusive incentive.** Give the first 100 signups something tied to your product: lifetime discount, beta access, a bonus feature. Mention it in the hero: "First 100 get 50% off for life." Early adopters love being first.


**Progress bar to launch milestone.** "847 of 1,000 signups before we launch." A concrete threshold gives members a shared goal and a reason to share.


**Social proof block below the fold.** Add 5-10 quotes from real people who are excited about what you're building. If you don't have any yet, ask your 10 most enthusiastic early supporters to post publicly.


## Custom vs. Beehiiv, ConvertKit, and Waitlist.email


Feature Blink (custom)[Beehiiv](https://www.beehiiv.com/)[Waitlist.email](https://waitlist.email/)


Custom referral tracking logic ✓ No Yes (limited)


Full admin dashboard ✓ Limited No


Custom position counter logic ✓ No Yes


Your own domain and full branding ✓ Yes Limited


100% data ownership ✓ Limited Limited


Monthly cost Blink plan $39+/mo $29+/mo


Waitlist.email is the fastest off-the-shelf option if you want referral mechanics without building anything. Build custom when you need specific referral logic, full admin visibility, integration with your future product, or you want to own the data from day one.


## Blink Advantages for Waitlist Projects


Waitlist apps need 4 infrastructure pieces: a database for email storage and referral tracking, auth for admin access, email delivery for confirmations and updates, and reliable hosting. Blink includes all four.


No Supabase for the database. No Clerk for admin login. No Vercel for hosting. No SendGrid for email. 200+ AI models to iterate quickly on design and copy. One bill instead of 5+ tools.


For more on building your first AI-powered app, see the[vibe coding beginners guide](https://blink.new/blog/vibe-coding-beginners) and[how to build a SaaS app with AI](https://blink.new/blog/build-saas-app-with-ai) .


## FAQ


Add email verification: send a confirmation link that must be clicked before the position is assigned. Unverified signups don't get a position number and don't appear in the admin count. Prompt Blink: "Add email verification. Members only receive their position after clicking the link in their confirmation email."


Yes. Prompt Blink to add a webhook that fires on each new verified signup. Send the event to Zapier, Make, or your CRM. You can also prompt for a direct integration with Mailchimp or ConvertKit to add verified signups to an email list automatically.


Moving up in line is often enough. If you need stronger motivation, tie the incentive to your product: early access, a discount, an exclusive feature for the top 10 referrers. Monetary cash incentives attract low-quality referrals. Product-native incentives attract real future users.


Send a weekly update email with real progress: what you built this week, how many new people joined, and any preview screenshots or demo clips. Waitlists go cold from silence, not from time. Communicate weekly and people stay engaged.


Yes. Prompt Blink: "Add an optional upgrade step after signup. Members can pay $X to reserve their spot and move to the front of the list. Use Stripe for payment." This adds pre-sale revenue before launch and signals real intent from your early adopters.
