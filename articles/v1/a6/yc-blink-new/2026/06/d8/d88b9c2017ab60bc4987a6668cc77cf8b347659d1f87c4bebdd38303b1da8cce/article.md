---
schema_version: "1.0.0"
document_id: "d88b9c2017ab60bc4987a6668cc77cf8b347659d1f87c4bebdd38303b1da8cce"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-waitlist"
published_at: "2026-06-02T01:12:27+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:07e4881e8b4cfdc4ce58393f078e81589fe68676af42384643bba374761bd13e"
---

# How to Build a Waitlist App: Pre-Launch Signup With Email Collection and a Counter

## Building it with Blink: the prompt


Open[blink.new](https://blink.new/) and describe what you want:


> "Build a waitlist landing page for my product. It needs an email signup form, a live counter showing total current signups, a referral system where each user gets a unique link and moves up the queue when others sign up through their link, an automated confirmation email with their position number and referral link, and an admin dashboard where I can view all signups, see daily signup charts, and export the list to CSV."


Blink builds:


- A landing page with signup form and live counter
- A database to store email addresses and referral relationships (no Supabase required)
- A unique referral link per signup
- An API endpoint that handles form submissions and increments the counter atomically
- An admin page protected by login
- Automated confirmation email on signup


Auth is built in — the admin login works immediately, no Clerk or Firebase Auth setup needed. Hosting is included — your waitlist has a live URL the moment you build it. No Vercel config, no deploy pipeline.


What you described in plain English is the full product spec. The build takes under 10 minutes to generate.


## The live counter: the most important element


The counter doesn't just show a number. It signals that other people made the same decision the current visitor is about to make.


A few things that make the counter work:


**Start from a non-zero base.** Before you launch publicly, add your team and advisors. Starting at 0 is technically accurate but creates a cold-start problem — the first visitor sees no social proof. Starting at 12 (your team) is more credible than starting at 0.


**Position it next to the form.** The counter belongs visible without scrolling — adjacent to or directly below the signup form. The visitor should see the form and the social proof in the same viewport at the same time.


**Keep it real.** The number should reflect actual signups. Inflated counters get spotted, shared on social media as a negative example, and become a trust problem before you launch. A real number that grows slowly is more credible than a suspiciously round "10,000 people waiting" on day two.


For a reference on landing page structure and conversion elements that complement the counter, see[how to build a landing page](https://blink.new/blog/how-to-build-landing-page) .


A finished waitlist landing page built with Blink — email form, live signup counter, and share buttons


Blink


## The referral mechanism: turn signups into growth


The referral loop is what turns a passive waitlist into a growth channel.


Each person who signs up gets a unique referral link. When someone signs up through their link, they move up in the queue. They're incentivized to share because their position improves. Your list grows because sharing is built into the experience.


Add this to your Blink prompt: *"When someone signs up, give them a unique referral link. Each signup through their link moves them up 3 positions in the queue. Show their current position and referral count on a personalized confirmation page."*


Blink builds the referral tracking, position management, and personalized confirmation page. No additional third-party referral service needed.


The viral loop that made Robinhood's pre-launch famous — "move up the waitlist by referring friends" — is a prompt away.


## Confirmation and welcome emails


The confirmation email has two jobs: confirm the signup is real, and give the person something to share.


A good confirmation email includes:


- Their position number with context ("You're #847 — early access goes to the first 500")
- Their referral link with a clear call to action ("Share this link to move up")
- One sentence about what they're waiting for — your product's core value prop
- A timeline if you have one ("We're targeting September for early access")


Add to your Blink prompt: *"Send a confirmation email immediately after signup. Include their position number, their unique referral link, and a short message about the product. Subject line: 'You're on the list — here's your spot.'"*


The email sends from your domain if you've connected one, or from a default address to start. Both work for early-stage waitlists.


## The admin dashboard


The admin dashboard is what you open on launch day.


It shows:


- Total signups with a daily chart
- Full list with email, signup date, referral count, and current position
- Top referrers — useful for personal outreach to your most engaged early users
- One-click CSV export


Blink builds this automatically as part of the admin page from your initial prompt. Auth protects it; you log in with the credentials Blink generates.


The CSV export gives you a clean list to import into HubSpot, Mailchimp, or any email marketing tool for your launch sequence. For non-technical founders building their first launch tooling, the[vibe coding for non-technical founders](https://blink.new/blog/vibe-coding-for-non-technical-founders) guide covers the broader pattern.


Waitlist admin dashboard showing daily signup chart, total count, referral sources, and email export


Blink


## How to make your waitlist go viral


The referral mechanic is the structural lever. Beyond that:


**Post the milestone numbers.** Share your count on Twitter/X, LinkedIn, or relevant communities every 100 signups. "We just passed 500 people waiting" is news. "We launched a waitlist" is not.


**Set a deadline.** "Early access closes when we hit 1,000 signups" creates urgency. A hard cap converts better than an open-ended waitlist with no scarcity signal.


**Go to specific communities, not general ones.** Post where your target users already are — specific Slack groups, subreddits, or Discord servers. A developer tool belongs on r/programming or a relevant dev Slack, not r/startups.


**Reach out to your top referrers.** Your admin dashboard shows who's driving the most signups. Email them personally. These are your super-users before you launch — they're already selling the product for you.


## Frequently Asked Questions


Under an hour for a complete waitlist with email collection, live counter, referral system, confirmation emails, and an admin dashboard. The prompt takes two minutes to write; Blink handles the build. Blink includes the database automatically — no Supabase account, no schema setup, no connection strings.


No. You describe what you want in plain English and Blink builds it. If you want to change something after — update the email copy, adjust the counter position, add a new field to the form — describe the change and Blink updates the code. Auth is built in; hosting is included.


Each signup generates a unique URL tied to their email address. When a new visitor uses that URL to sign up, the original user's position in the queue advances. Blink stores referral relationships in the same database as signups — no third-party referral service needed. The position calculation runs server-side so the queue order stays accurate even with concurrent signups.


Your data lives in a database you own through Blink. Export it to CSV at any time from the admin dashboard. Import it into HubSpot, Mailchimp, or any email marketing tool for your launch sequence. Nothing is locked in to any external waitlist platform.


Yes. Blink generates an API endpoint for form submissions that you can call from any frontend. You can also embed the waitlist page as an iframe or use the form directly on your existing site with a few lines of JavaScript. Include the embedding requirement in your initial prompt and Blink adds the necessary code.
