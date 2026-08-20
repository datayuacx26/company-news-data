---
schema_version: "1.0.0"
document_id: "747dbbd526dac1d11398567d99855804fcdcd1de9aec1c3c3e0b961c47995f24"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1-37-0"
published_at: "2022-06-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:5651b6b62ce19d05dac427db207c9cd22848684762ad37079db14ea7be934e16"
---

# Array 1.37.0: Cohorts 2.0 and event & property detail pages

# Array 1.37.0: Cohorts 2.0 and event & property detail pages


Jun 27, 2022


- [Product updates](https://posthog.com/blog/product-updates)


,
- [Release notes](https://posthog.com/blog/release-notes)


#### Contents


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-


Want to know more about what we're up to?[Subscribe to our new newsletter](https://newsletter.posthog.com/subscribe)


, which we send once every two weeks!


> Running a self-hosted instance? Check out our[Upgrading PostHog guide](https://posthog.com/docs/runbook/upgrading-posthog)
>
>
> .


##


PostHog 1.37.0 release notes


> **IMPORTANT!** This version will not run until async migration \`0004_replicated_schema\` is completed. Before upgrading, please make sure this migration is completed. Check out the[async migrations](https://posthog.com/docs/runbook/async-migrations)
>
>
> docs for details.


**Release highlights:**


- New:Cohorts 2.0


- New:Event and property detail pages


- New:Dancing hedgehogs


- Improved:Save event columns


- Improved:Faster response times


- Improved:App retry and failure logic


###


New: Cohorts 2.0


Cohorts are an extremely powerful tool. They let you define a group of users to later base your analytics on. In this release, we completely revamped how you define cohorts, giving you flexible and powerful filtering capabilities.


####


New condition types


Previously, cohorts could only be defined using the following conditions:


- Match users who have a certain property
- Match users who have completed a certain event


We've now added the following conditions:


- **Did not complete event:** Find users who aren't doing what you expect. For example, "Give me users who visited the home page, but did not click on the 'Sign up' button.".
- **Completed an event multiple times:** Find your most active users. For example, "Give me users who have 'Bought item' more than 3 times in the last 30 days".
- **Completed a sequence of events:** Find users using your product in a very specific way. For example, "Give me users who added something to their cart and then entered a promo code within a day".
- **Did not complete a sequence of events:** Find users who aren't using your product as you'd like. For example, "Give me users who added something to their cart, but didn't checkout within a day".
- **Do not have the property:** Find more specific sets of users. For example, "Give me users outside of Europe".
- **Completed an event for the first time:** Find the newest users of a feature. For example, "Give me users who bought an item for the first time in the last 7 days".
- **Completed an event regularly:** Find your power users. For example, "Give me users who bought an item in 5 out of the last 7 weeks".
- **Stopped doing an event:** Find your users that are at risk of churning. For example, "Give me users who haven't bought anything in the last 7 days, but had bought something in the 30 days prior".
- **Started doing an event again:** Find your users that did not churn. For example, "Give me users who bought something, then didn't for 2 weeks, but have again in the last 7 days".


####


AND/OR operators within cohort conditions


You can now combine these new conditions using complex AND/OR groupings.


For example, you can define a cohort that is "Give me users who are outside of Europe AND have either regularly bought an item 4 of the last 5 weeks OR bought 3 items in the past week".


With the combination of these new conditions and the new AND/OR operators, you can now define extremely powerful cohorts for finding the exact users that you're looking for.


####


Nested cohorts


But that's not all! You can also use existing cohorts to define *new* cohorts. For example, you can define a cohort as "Give me users in *Cohort X* that are not in *Cohort Y* ".


Using these nested cohorts enables you to avoid redefining the same cohort over and over again.


###


New: Event and property detail pages


We've added a new page within Data Management, which enables you to dig into the details of all events and properties. For example, on the event page, you can now see:


- When an event was first and last seen
- How many times it was sent in the last 30 days
- The top properties for that event
- A filterable list of the specific event for you to explore


###


New: Dancing hedgehogs


Waiting for an insight to load? Our favorite mascot, Max, will now keep you company!


###


Improved: Save event columns


You can now select and reorder the custom event columns that you want to see on the activity page. You can save the selected columns selected at the team level, so your colleagues can benefit too.


###


Improved: Faster response times in the app


We've made it faster to load insights and recordings. We realized that some data payloads were quite big, which resulted in the network slowing things down. To improve this, we added compression to a few of the endpoints in the app, and now see at least 50% smaller responses sent to clients.


Here's to things being faster!


###


Improved: App retry and failure logic


The retry policy of apps (formerly known as plugins) has been consolidated. Built-in retries with` RetryError` are now more widely available. Additionally, you will be notified by email if an app fails to load in your project due to a fatal` setupPlugin` error ([email configuration required](https://posthog.com/docs/self-host/configure/email)


). See[Apps developer reference](https://posthog.com/docs/apps/build/reference#maximizing-reliability-with-retryerror)


for more details.


###


Other improvements & fixes


Version 1.37 also adds hundreds of other improvements and fixes, including...


- **Fixed** : PostHog-js was logging an unnecessary warning[#10375](https://github.com/PostHog/posthog/pull/10375)


- **Fixed** : Only show dashed line if it is not the previous period.[#10345](https://github.com/PostHog/posthog/pull/10345)


- **Fixed** : Remove correlation table from the funnel preview in experiments[#10286](https://github.com/PostHog/posthog/pull/10286)


- **Fixed** : Only query for the user once when loading my flags[#10205](https://github.com/PostHog/posthog/pull/10205)


- **Fixed** : Disappearing breakdown tooltip[#10184](https://github.com/PostHog/posthog/pull/10184)


View the commit log in GitHub for a full history of changes:[release-1.36.0...release-1.37.0](https://github.com/PostHog/posthog/compare/release-1.36.0...release-1.37.0)


.


###


Deprecation and removal notices


- **For feature flags:** Feature flags can no longer depend on cohorts with behavioral conditions. They can still depend on cohorts with only property conditions. You can read[more context here](https://github.com/PostHog/posthog/issues/8628)


.


##


Share your feedback


We'd love to hear anything you have to say about PostHog, good or bad. As a thank you, we'll share some awesome[PostHog merch](https://merch.posthog.com/)


.


Want to get involved?Email us to schedule a 30 minute call


with one of our teams to help us make PostHog even better!


##


Contributions from the community


We always welcome contributions from our community and this time we want to thank the following people...


- @MichaelLampe for a correction to PostHog docs
- @girlProg for corrections to PostHog.com


Do you want to get involved in making PostHog better? Check out our[contributing resources](https://posthog.com/docs/contribute)


to get started, or head to[our community page](https://posthog.com/posts)


. We also have a[list of Good First Issues](https://github.com/PostHog/posthog/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)


for ideas on where you can contribute!


##


Open roles at PostHog


Want to join us in helping make more products successful? We're currently hiring for remote candidates in any of the following roles:


- [Senior Product Manager](https://posthog.workable.com/jobs/2490685)


- [Site Reliability Engineer - Kubernetes](https://posthog.workable.com/jobs/2105790)


- [Senior Data Engineer](https://posthog.workable.com/jobs/2482259)


- [Developer Advocate](https://posthog.workable.com/jobs/2422700)


- [Full Stack Engineer - Community Tools, Website & Docs](https://posthog.workable.com/jobs/2175512)


- [Full Stack Engineer - App](https://posthog.workable.com/jobs/2362038)


Curious about what it's like to work at PostHog? Check out our[careers page](https://posthog.com/careers)


for more info about our all-remote team and transparent culture. Don’t see a specific role listed? That doesn't mean we won't have a spot for you.Send us a speculative application!


---


*Follow us on[Twitter](https://twitter.com/PostHog) or[LinkedIn](https://linkedin.com/company/posthog) for more PostHog goodness!*


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
