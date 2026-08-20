---
schema_version: "1.0.0"
document_id: "cd64b83facbfb00ba4107fd254295fc6b782a8f558b939980040f5c2116a5975"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/workflows-beta"
published_at: "2025-11-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T22:25:49.638513+00:00"
content_hash: "sha256:17d3a45a37fb6e8703142d96d744831cc154cd6bc753a285d2c67a3a2a94308e"
---

# Workflows graduate to beta! Product data, meet automation

# Workflows graduate to beta! Product data, meet automation


- [Sara Miteva](https://posthog.com/community/profiles/35224)


Nov 12, 2025


- [Workflows](https://posthog.com/blog/workflows)


When we first introduced[Workflows (in alpha)](https://posthog.com/blog/workflows-alpha)


, it was still a bit rough around the edges, and many of you were brave enough to test it. Since then, we’ve spent a lot of time fixing, refining, and adding power where it counts.


Now, Workflows has officially graduated to beta.


So, what does that mean in practice? You can finally automate product-led actions, like sending emails, updating properties, or triggering Slack alerts, directly from your PostHog data. No syncing tools, no duct-taped integrations, no API key scavenger hunts.


Everything runs on the same event data you already track. Want to send a welcome email when a user completes onboarding? You can build the email and trigger it with an event or a delay. Want to adding branching logic based on their upgrade path? You can do that too.


Workflows now support a range of actions:


- Send emails, from simple notifications to full[drip campaigns](https://posthog.com/docs/workflows/email-drip-campaign)


- Trigger Slack messages or webhooks based on live product events
- Add delays, conditions, and branches to control when and how actions fire
- Update user properties or trigger events as part of your automation


If you’ve used Zapier, Make, Brevo, Active Campaign or similar tools before, the idea will feel familiar, but this time, everything happens inside PostHog. It’s faster, more reliable, and you don't need to send data to third-party platforms.


Teams like[Grantable](https://posthog.com/customers/grantable)


are already seeing the benefits. Evan Rallis, who leads product & growth there, told us:


> “PostHog Workflows just lives on top of the event data and the amazing user data you already have. The setup was incredibly easy.”


Evan said it's almost twice as fast to build automations in Workflows than with other tools, in part because he doesn't need to switch to a third-party tool and wait for data to sync.


Workflows are still free during beta, so now’s a good time to experiment. Try building something small – maybe an onboarding reminder, a feedback request, or a Slack alert when a user hits a key milestone.


You can learn more in the[docs](https://posthog.com/docs/workflows)


or jump straight into launching your first[workflow](https://app.posthog.com/workflows)


.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
