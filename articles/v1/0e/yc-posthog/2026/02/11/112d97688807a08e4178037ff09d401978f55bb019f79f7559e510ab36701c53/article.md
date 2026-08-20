---
schema_version: "1.0.0"
document_id: "112d97688807a08e4178037ff09d401978f55bb019f79f7559e510ab36701c53"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/vercel-integration"
published_at: "2026-02-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T22:22:47.419354+00:00"
content_hash: "sha256:99d3a66c8892bbab1c044b3bdd27f9afe6a072da3e13f6db61619b21cfa0c5ba"
---

# PostHog × Vercel: feature flags, minus the plumbing

# PostHog × Vercel: feature flags, minus the plumbing


- [Sara Miteva](https://posthog.com/community/profiles/35224)


Feb 02, 2026


- [Product](https://posthog.com/blog/product)


#### Contents


-
-


If you’re building on Vercel and using PostHog for[feature flags](https://posthog.com/feature-flags)


and[experiments](https://posthog.com/experiments)


, you’ve probably had some version of this setup:


- flags defined in one place
- app logic living somewhere else
- a bit of glue code
- a bit of env var juggling
- a bit of “wait, where does this get evaluated again?”


Today, that gets simpler.


##


Introducing the PostHog × Vercel integration


[The new PostHog × Vercel integration](https://posthog.com/docs/integrations/vercel-marketplace)


lets you use PostHog feature flags and experiments directly in Vercel, without custom wiring or creative workarounds.


Here’s how it works:


1. You define feature flags and experiments in PostHog
2. They’re synced into Vercel’s native Flags system
3. Your Vercel apps consume them using the Vercel Flags SDK


Alongside flag syncing, the integration also takes care of credentials. Your PostHog Project ID and API key are automatically synced into Vercel environment variables.


This means no copying values between dashboards and no wondering if prod and staging are using the same project. It’s the boring kind of automation – the best kind.


In addition, thanks to the Vercel Marketplace, billing and account management are now both managed in a single location.


The integration also works with[v0 – Vercel's AI agent](https://posthog.com/blog/v0.app)


that helps users create real code and full-stack apps.


##


Get started


Feature flags help teams ship more confidently and experiment more easily. This integration keeps that experience simple – letting PostHog handle flags and experiments, and Vercel focus on running your app, without you having to stitch the two together.


If you’re already using PostHog and Vercel, you can enable the integration from the Vercel Marketplace and start syncing flags right away.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
