---
schema_version: "1.0.0"
document_id: "1e6957b47f98fe1849509d175667ca2d5c20a0ff9dfcdb022de106d6de084dd2"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1-40-0"
published_at: "2022-09-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:8b12780f1c2b928b4c87177a713d27d52b46e3d5ba0891cbe305693287f2ef8b"
---

# Array 1.40.0: Interface improvements and more!

# Array 1.40.0: Interface improvements and more!


Sep 26, 2022


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


Want to know more about what we're up to?[Subscribe to our new newsletter](https://newsletter.posthog.com/subscribe)


, which we send once every two weeks!


> Running a self-hosted instance? Check out our[guide to upgrading PostHog](https://posthog.com/docs/runbook/upgrading-posthog)
>
>
> .


##


PostHog 1.40.0 release notes


**Release highlights:**


- New:Redesigned calendar and date components


- New:Overhauled annotations


- New:Revamped persons modals


- New:Recordings domain settings


- Improved:Retention insight update


###


New: Redesigned calendar and date components


The 1.40.0 update is all about visual polish, and we're starting with new calendar and date components. These are some of the most used modals in PostHog, so we've given careful attention to providing an improved and more unified experience.


###


New: Overhauled annotations


We've overhauled[annotations](https://posthog.com/manual/annotations)


with a new look and interface. This change resolves several layout and interaction issues that had previously been impacting annotations. We've also touched up the annotations page to highlight important information better!


###


New: Revamped persons modals


Clicking datapoints in an insight is one of our favorite ways to analyze the users behind the numbers. The new person modal now looks and feels like the rest of the app. We've made it easier for you to search and perform actions on those lists of users, too.


###


New: Recordings domain settings


Struggling with keeping toolbar and recordings settings separate? Us too... That's why the settings for managing valid domains for recordings and toolbar are now split, so that they can be adjusted independently!


###


Improved: Retention insight update


Once again, we've cleaned up the UI on the retention insight to keep up with all the styling updates we've had across the board. The end result is more consistent with the rest of our product, with lots of little touches which add visual flair and create an improved experience.


###


Other improvements & fixes


Version 1.40 also adds hundreds of other improvements and fixes, including...


- **Improvement:** Filters for test and internal users can now be set to apply on all new insights ([#11589](https://github.com/PostHog/posthog/pull/11589)


)
- **Improvement:** Event volume and query counts are now emphasized in data management ([#11806](https://github.com/PostHog/posthog/pull/11806)


)
- **Improvement:** Profile pictures now will show more information on hover ([#11849](https://github.com/PostHog/posthog/pull/11849)


)
- **Fix** : Fixed an issue where group properties weren't showing up on search previously ([#11813](https://github.com/PostHog/posthog/pull/11813)


)
- **Fix** : Fixed an issue that was causing property statistics to be calculated infrequently ([#11815](https://github.com/PostHog/posthog/pull/11815)


)


View the commit log in GitHub for a full history of changes:[release-1.39.0...release-1.40.0](https://github.com/PostHog/posthog/compare/release-1.39.0...release-1.40.0)


.


##


Give us your feedback


We’re always working on improving PostHog and would love to talk to you! Please[schedule a 30 minute call](https://calendly.com/posthog-feedback)


with one of our Product, Engineering, or Marketing team members to help us understand how to improve. As a thank you for your time, we'll be giving away awesome[PostHog merch](https://merch.posthog.com/)


!


##


Contributions from the community


We always welcome contributions from our community and this time we want to thank the following people...


- [bobeagan](https://github.com/bobeagan)


for correcting a documentation typo.
- [iamwacko](https://github.com/iamwacko)


for fixing an OpenSSL dependency issue.
- [Thebigbignooby](https://github.com/Thebigbignooby)


for correcting an error in our documentation.
- [Klamas1](https://github.com/klamas1)


for adding nodeSelector and tolerations to hooks.


Do you want to get involved in making PostHog better? Check out our[contributing resources](https://posthog.com/docs/contribute)


to get started, or head to[our community page](https://posthog.com/posts)


. We also have a[list of Good First Issues](https://github.com/PostHog/posthog/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)


for ideas on where you can contribute!


##


Open roles at PostHog


Want to join us in helping make more products successful? We're currently hiring for remote candidates in any of the following roles:


- [Product Designer](https://posthog.com/careers/product-designer)


- [Senior Data Engineer](https://posthog.com/careers/senior-data-engineer)


- [Site Reliability Engineer - Kubernetes](https://posthog.com/careers/site-reliability-engineer-kubernetes)


- [Full Stack Engineer](https://posthog.com/careers/full-stack-engineer)


Curious about what it's like to work at PostHog? Check out our[careers page](https://posthog.com/careers)


for more info about our all-remote team and transparent culture. Don’t see a specific role listed? That doesn't mean we won't have a spot for you.Send us a speculative application!


*Follow us on[Twitter](https://twitter.com/PostHog) or[LinkedIn](https://linkedin.com/company/posthog) for more PostHog goodness!*


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
