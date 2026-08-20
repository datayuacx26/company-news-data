---
schema_version: "1.0.0"
document_id: "79ec7bc6fd38efd479802eac93beacc18e2cf89b838d864e663d870367ffc54a"
company_key: "yc-mux"
company: "Mux"
source_id: "yc-mux-atom-4708df60f240"
canonical_url: "https://www.mux.com/blog/protect-your-prod-environment-new-rate-limit-controls"
published_at: "2026-06-03T17:11:11.291+00:00"
first_seen_at: "2026-07-20T23:20:16.234471+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:2192115f99136fec409e94016164e4b8fc66c20698ecf3133739802f3a288c62"
---

# Protect your prod environment: New rate limit controls

Published on


June 3, 2026 (about 2 months ago)


# Protect your prod environment: New rate limit controls


By[Aaron Hedges](https://www.mux.com/team/aaron-hedges) • 3 min read •[Product](https://www.mux.com/blog/category/product)


---


Rate limits keep an API reliable for everyone. Each account gets a cap on how many requests it can make in a given window, so one account can't take out the API for everyone else.


Mux, traditionally, has managed rate limits by giving each account a single bucket of requests. That works for a lot of use cases up to now. But as accounts grow and customers have used Mux in different ways, we've seen where that approach starts to fall short. A runaway script in dev, for example, or a batch job summarizing old content can eat through an account’s limit and start refusing requests in production.


Today we're introducing two levers to give you more granular control to prevent that from happening: per-environment limits and token priority.


## Per-environment limits


Dev and production environments carry different risks. In production, every refused request impacts a real user’s experience. In dev, your audience and your tolerance for throttled experiences is much higher. You're poking at things, running a quick test, fixing a bug, or occasionally writing a script that loops more than you meant it to. That's the whole point of a dev environment.


Up to now, both environments drew from the same bucket, putting production at risk. Now every Mux environment gets its own rate limit bucket. Dev, staging, and production are fully isolated. Which means, a mistake in dev stays in dev, and your end-user is none the wiser.


As of today, this is automatically applied to your Mux environments. There’s nothing you need to do to enable this.


## High and low priority tokens


In addition to per-environment limits, we’re giving you a way to get even more granular.


Even inside a single environment, not all API calls are equally important. A user uploading a video needs that call to work now. A[Mux Robots](https://www.mux.com/blog/mux-robots) job summarizing your back catalog can wait for a second or two.


To address this, we have given every environment two buckets: one for high priority tokens and one for low priority tokens. You pick the priority when you create the token. This allows you to assign a low priority token so that it doesn’t compete with the calls critical to your end-users.


Here’s a good rule of thumb:


- **High priority:** anything a end-user is waiting on like uploads, edits, playback ID creation, and real-time API calls.
- **Low priority:** background jobs, bulk operations, crawlers, analytics exports.


You can create high and low priority tokens on your organization's[settings page](https://dashboard.mux.com/settings) . Existing tokens default to high priority and will keep working the way they always have.


## Bonus: rate limit headers on every response


We're also adding the standard X-RateLimit-Limit


and X-RateLimit-Remaining


headers on every API response. You can use these to monitor consumption in real time and slow down your requests before you ever hit a 429 error.


## Get started


You can start using these new levers and buckets today.[Reach out](https://www.mux.com/support) if you have any questions or comments, we'd love to hear these new rate limit levers are fitting into your workflows.


## Written By


### [Aaron Hedges – Product Manager](https://www.mux.com/team/aaron-hedges)


Aaron lives in Michigan with a house full of people and animals.
When not playing the role of father/jester, he plays obscure tabletop games, watches bad movies and loses regularly in online games.


## Leave your wallet
where it is


No credit card required to get started.


[Sign up Sign up](https://dashboard.mux.com/signup)
