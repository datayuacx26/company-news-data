---
schema_version: "1.0.0"
document_id: "f92e2c086c5036e9336e620bc00bd86fae79b0d6e6bb2f49d77ea764d8bebe75"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/workflows-ga"
published_at: "2025-12-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T22:24:48.249354+00:00"
content_hash: "sha256:673381d1f8ab80dbbfb9427924e6ce0ae1ddd300529ba2cff5b9f3abdd388c56"
---

# Your product data just got a job: Workflows is now out

# Your product data just got a job: Workflows is now out


- [Sara Miteva](https://posthog.com/community/profiles/35224)


Dec 30, 2025


- [Workflows](https://posthog.com/blog/workflows)


#### Contents


-
-
-
-
-


If your product data was a person, they'd basically be unemployed. They could get picked up in the occasional "freelance" query, but would ideally be looking for more steady work.


We’ve always been good at collecting product data. In PostHog, you could see when a user hits an important milestone in your product, but couldn’t act on it. If PostHog already knows what happened, why does the next step have to happen in another system? That question is what led to Workflows.


Workflows is basically PostHog's version of an employment agency, giving your data a steady job it can do all the time.


We introduced[Workflows in alpha](https://posthog.com/blog/workflows-alpha)


asking: could we let teams define what happens next directly on top of the same events and user data they already trust for analytics?[During the beta](https://posthog.com/blog/workflows-beta)


, we saw it used to automate onboarding, coordinate feature rollouts, and trigger messages based on real product behavior.


After seeing this usage and getting positive feedback, **[Workflows](https://app.posthog.com/workflows)** is now generally available.


##


What does Workflows do?


**[Workflows](https://app.posthog.com/workflows)** lets you automate product-led actions directly from your PostHog data. When something happens in your product, you can respond immediately by sending an email, updating a user property, or triggering a Slack alert. This is done without syncing tools, duct-taped integrations, or API key scavenger hunts.


If you’ve used tools like Zapier, Make, Brevo, or ActiveCampaign, building workflows will feel familiar. The difference is that everything runs inside PostHog, without sending data to third-party tools or platforms.


Everything runs on the same event data you already track. Whether you’re sending a welcome email after onboarding or personalizing notifications based on an upgrade path, it all happens inside PostHog, without shipping data off to third-party platforms.


##


Your product data, without the duct tape


Because your product data already lives in PostHog, Workflows can react to it in real time. You can trigger automation from:


- Product events and user actions
- Person property and cohort membership updates
- Feature flag exposure and experiment variants


There’s nothing to sync and nothing to reconcile later. When something meaningful happens in your product, Workflows can respond immediately by updating user state, branching logic, or triggering the next step in the flow.


This also means your automation logic no longer lives in five different tools. Conditions, delays, exits, and actions sit next to the data they depend on, which makes workflows easier to build, easier to debug, and easier to evolve over time. Instead of coordinating systems that only partially agree on who a user is or what they’ve done, everything runs in one place, where your product truth already exists.


##


Product changes and messaging in a single workflow


Workflows doesn’t stop at sending emails. Because it sits on top of PostHog’s feature flags and messaging system, a single workflow can coordinate both product changes and communication.


In one flow, you can:


- Enable or disable a feature flag
- Assign users to a specific variant
- Update person properties for future targeting


This keeps what users see in the product and what they’re told about it in sync, without relying on separate systems to catch up later.


##


Less theory, more shipping


At[Croissant](https://posthog.com/customers/croissant)


, Workflows is used to act on intent signals without moving data between tools. Their growth team triggers follow-ups directly from PostHog events, keeping experimentation and iteration fast.


> “The main benefit is that everything’s already in PostHog. Syncing data across tools is always hit-or-miss, and expensive. Now it’s all in one place, and we can iterate way faster.”


At[Grantable](https://posthog.com/customers/grantable)


, Workflows builds on an analytics foundation where every feature is instrumented and measured. Product usage data now feeds automation directly, instead of stopping at dashboards.


> “We track usage, pipe that into dashboards for activation and retention, and now we’re starting to act on it automatically with Workflows.”


Different products, same pattern: using trusted product events as the source of truth and acting on them without adding another automation layer to maintain.


**PostHog's free tier** allows you to send 10,000 messages monthly with all channels (push, email, SMS, and CDP events) included. After that, prices start at $0,005 per send, depending on the channel. Pricing decreases with volume and every channel has a generous volume discount.


Compared to most automation and messaging tools on the market, this makes Workflows one of the most accessible and cost-effective options available. Traditional automation platforms charge per task, quickly becoming expensive as workflows grow more complex, while customer messaging tools often bundle pricing around user profiles or locked plans. PostHog takes a different approach – you only pay for what you actually send, with no feature gating, no per-user pricing, and no extra tooling required to connect your product data.


##


Start building with Workflows


If your product already knows when someone signed up, finished onboarding, or hit a meaningful usage milestone, that information shouldn’t stop at a dashboard.


With Workflows, those signals can trigger messages, updates, or follow-up actions immediately, without routing data through another tool first. The result is fewer moving parts, less glue code, and your data & actions living in the same tool you already love.


Learn more in the[docs](https://posthog.com/docs/workflows)


or jump straight into launching your first[workflow](https://app.posthog.com/workflows)


.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
