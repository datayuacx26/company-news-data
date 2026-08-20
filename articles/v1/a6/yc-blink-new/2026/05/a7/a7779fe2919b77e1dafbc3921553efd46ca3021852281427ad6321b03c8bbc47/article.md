---
schema_version: "1.0.0"
document_id: "a7779fe2919b77e1dafbc3921553efd46ca3021852281427ad6321b03c8bbc47"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-newsletter-app"
published_at: "2026-05-24T01:43:35+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:e8eeb2c33a0178b2e1a05a6e1c5b82da5788ae1686c43035294594ba83b457a8"
---

# How to Build a Newsletter App With AI (Own Your Platform, Keep 100% of Revenue)

## Building it: step by step


1


#### Define your data model


Start by prompting Blink to create three tables:` subscribers` (email, status, plan, created_at),` newsletter_posts` (title, body, status, scheduled_for, sent_at), and` email_events` (subscriber_id, post_id, event_type, created_at).


Blink provisions the database automatically. No Supabase account needed. The schema and a working admin UI generate together — you see a live interface before writing any SQL.


2


#### Build the public subscribe page


Prompt Blink: "Create a subscriber landing page with an email capture form, double opt-in confirmation email via Resend, and an unsubscribe link generator."


Add your Resend API key to Blink's secrets panel. Blink scaffolds the subscribe endpoint, confirmation email trigger, and unsubscribe token logic. The complete flow — form to confirmed subscriber — takes about 15 minutes to build.


[Resend's free tier](https://resend.com/pricing) covers 3,000 emails/month, which is enough to launch and grow past several hundred subscribers at zero infrastructure cost.


3


#### Add the post editor and archive


Prompt Blink to create a rich-text editor for writing issues and a public archive at` /issues` . Each published post gets its own SEO-friendly URL (` /issues/your-post-title` ) that search engines can index independently.


Free subscribers see a teaser paragraph with a paywall prompt. Paid subscribers see the full post. Blink generates the gating logic from the subscriber's plan field — you describe what you want, Blink builds the conditional.


4


#### Wire up Stripe for paid subscriptions


Add your Stripe API keys to Blink's secrets. Prompt: "Create monthly and annual subscription plans with a Stripe checkout flow. Add a webhook handler that syncs subscription status back to the subscriber record in real time."


Blink generates the full Stripe integration — checkout sessions, webhook handler, and subscriber plan updates. Auth is built in, so the subscriber portal works with the identity system from the first prompt.


Correctly implementing Stripe subscriptions — checkout, webhooks, status sync, failed payment recovery — takes a solo developer 2–3 days. With Blink: 20 minutes.


5


#### Connect email delivery


With your Resend key already in secrets, prompt Blink: "Send each newsletter_post with status 'scheduled' and scheduled_for in the past to all active subscribers in batches of 100, with an open-tracking pixel, click-tracking redirect URLs, and an unsubscribe link."


Blink generates the send function and cron trigger. Batch sending protects your sender reputation. Bounce events write back to the` email_events` table. Unsubscribes update subscriber status automatically.


6


#### Build your analytics dashboard


Prompt Blink: "Create an admin dashboard showing subscriber growth over time, open rate per issue, click rate per issue, subscriber status breakdown, and total revenue from Stripe."


Blink reads from the` email_events` and` subscribers` tables you already built. The dashboard is live-queried — refresh to see current data. No third-party analytics service needed. No config, no DevOps, deploy included.


Newsletter creator owning their platform and keeping all subscriber revenue


Blink


## Build vs. buy: the honest comparison


[Substack](https://substack.com/)[Ghost](https://ghost.org/)[Beehiiv](https://beehiiv.com/) Build with Blink


Platform fee **10%** 0% 0% **0%**


Monthly cost $0 $9–$199 $42–$99 Blink plan


Custom domain ✅ ✅ ✅ ✅


Subscriber data ownership Partial ✅ ✅ **Full**


Custom features ❌ Limited Limited **Unlimited**


Build time 0 (SaaS) 1–2 days 0 (SaaS) **4–6 hrs**


Lock-in risk High Low Medium **None**


The honest tradeoff: SaaS platforms have zero build time. Your custom platform has 4–6 hours upfront. After that, you keep 100% of reader revenue, you own the subscriber data permanently (CSV export any time), and you can change email providers by swapping one API key.


At 1,000 paid subscribers at $10/month, you recover that build time in the first week.


If you want to extend the platform with member-only content, community features, or tiered access beyond the newsletter itself, the[membership site guide](https://blink.new/blog/how-to-build-membership-site) covers that in detail. If you're evaluating all your options before committing, the[best AI app builders comparison](https://blink.new/blog/best-ai-app-builders) lays out the current landscape. And if building a full-stack SaaS alongside the newsletter is on your roadmap,[this walkthrough](https://blink.new/blog/build-saas-in-a-weekend) covers the complete flow.


## Moving from Substack


Export your subscriber list from Substack under Settings → Subscribers → Export. Import the CSV into your platform's subscribers table via Blink's admin panel.


Free subscribers migrate automatically. Paid subscribers need to re-subscribe through your new Stripe checkout — Substack blocks payment migration. A single email to your paid list with a direct checkout link typically converts 60–80% within a week.


Beehiiv exports cleanly. Ghost requires more manual work. Either way, you own the email addresses — that's the asset that matters.


## Frequently Asked Questions


The core platform — subscriber management, email delivery, paid tiers, and analytics — takes 4–6 hours in one sitting with Blink. Deploy is included; there's no separate hosting setup. A custom domain requires one CNAME record, which takes about five minutes. Total time from zero to live newsletter platform: one weekend.


No. Blink generates the database schema, backend API, Stripe integration, email delivery logic, and admin UI from plain-language prompts. You add API keys (Resend, Stripe) to the secrets panel — that's the only configuration required. If you can write a clear description of what you want built, you can build this.


Substack charges 10% of all paid subscription revenue plus ~3% Stripe fees. A self-built newsletter charges 0% platform fee — you pay only Stripe's standard 2.9% + $0.30 per transaction. At 1,000 paid subscribers at $10/month, that's roughly $12,000/year you keep instead of sending to Substack.


Resend is the cleanest option. It has a free tier covering 3,000 emails/month, straightforward DKIM/SPF setup, and an API that Blink can scaffold automatically from a prompt. For lists over 10,000 subscribers, Resend scales without requiring any infrastructure changes — you keep the same API key, same code.


Yes. Export your subscriber list from Substack as a CSV (Settings → Subscribers → Export), then import it into your platform's subscribers table. Free subscribers migrate automatically. Paid subscribers need to re-subscribe via your new Stripe checkout. Substack prevents payment migration, but an email to your paid list with a direct checkout link converts the majority within a week.


You pay Blink's platform fee (flat, not per-subscriber), Resend's email costs (free up to 3,000 emails/month, then approximately $20/month for 50,000 emails), and Stripe's 2.9% + $0.30 per transaction. No 10% revenue cut. For a creator with 500 paid subscribers at $10/month, that difference is roughly $6,000/year that stays with you instead of going to the platform.
