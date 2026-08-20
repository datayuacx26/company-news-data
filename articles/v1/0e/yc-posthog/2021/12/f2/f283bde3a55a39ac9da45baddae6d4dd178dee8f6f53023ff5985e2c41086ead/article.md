---
schema_version: "1.0.0"
document_id: "f283bde3a55a39ac9da45baddae6d4dd178dee8f6f53023ff5985e2c41086ead"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1-31-0"
published_at: "2021-12-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:fe74aa42975b53c40b3ae1ac5a835c1bf3f37c01902ae44971a8f4a4aaeadcdb"
---

# Array 1.31.0

# Array 1.31.0


Dec 16, 2021


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


Happy holidays from PostHog! PostHog 1.31.0 is our last release of the year, introducing Group Analytics, improved Correlation Analysis, a revamped user experience on Insights and 350+ more improvements and fixes. Please note that Postgres-based installations are no longer supported for PostHog 1.31.0.


> **IMPORTANT!** Do not upgrade to this version if you have deployed PostHog using Postgres. PostHog no longer supports Postgres as of v1.30.0 and you must upgrade to ClickHouse first.


##


PostHog 1.31.0 release notes


> Don't see the new features on your self-hosted deployment? Remember to[update your PostHog instance](https://posthog.com/docs/runbook/upgrading-posthog)
>
>
> .


**Release highlights:**


- Group Analytics


- Improved Correlation Analysis


- Improved user experience


###


New: Group Analytics


Introducing Group Analytics! Group Analytics enable you to analyze groups, which aggregate events within PostHog. You can have multiple groups and they can even change dynamically.


Group Analytics is especially useful if you have a B2B product, as you will now be able to create a Company group type which tracks all unique users within a company, then create insights such as retention by company and events performed by unique companies.


Visit our[Group Analytics documentation](https://posthog.com/docs/user-guides/group-analytics)


to find out what else is possible with Group Analytics.


> 🎁 Group Analytics is a premium feature and requires a PostHog Scale or Enterprise license.[Learn more](https://posthog.com/pricing)
>
>
> .


###


Improved: Correlation Analysis


Correlation Analysis just got better! In addition to significantly improving the UI & UX, we've also introduced a details option for advanced users looking for deeper understanding of how events and properties contribute to conversion or drop-offs. This new detail view shows a complete confusion matrix which shows true positives, true negatives, false negatives and false positives. We've also added a correlation score from` -1.0` to` 1.0` to signal how strongly an event or property correlates with conversion or drop-off.


###


Polished: Experience of insights


Insights and dashboards are the core of PostHog's analytics capabilities, which is why we're putting extra focus on making using them *spark joy* . This release brings various improvements to the experience:


- You can now easily link to saved insights like so:[https://app.posthog.com/insights/vMA1IlmP](https://app.posthog.com/insights/vMA1IlmP)


. While unwieldy query parameters were previously required, now all PostHog wants for Christmas is the ID of the insight. Merry sharing!
- Visualization of funnels has been reworked for improved readability of results, particularly when using breakdown. This makes comparing conversion based on properties easier than ever.
- Searching events & properties is now significantly faster. We've changed the way these properties are shown and now list them by popularity within the project. Create those insights faster!
- The funnel query builder has been streamlined – essential settings are better exposed, while advanced options can be expanded when you need them.
- The dashboard grid has been simplified to avoid annoying situations where your carefully crafted layout becomes misaligned on a different screen resolutions. Instead of four complicated layouts, there are now two: single-column for mobile devices, and multi-column for larger screens.


Expect further major improvements to this area in 1.32.


###


Enhanced: App design and performance


Following up on the[overhaul of navigation in 1.30](https://posthog.com/blog/the-posthog-array-1-30-0#fresh-new-lookandfeel)


, we've made major UI improvements to the app's most used pages. These top-level views have been overhauled for uniformity, clarity, and snappiness.


Breadcrumbs are now fully dynamic and adjust to the current page in all situations.


The sidebar has been made more graceful: it adjusts to the screen size in a smarter way, and will remember your preferences in the browser.


###


Other improvements & fixes


- Recordings now load up to a few times faster.
- Fixed bug in feature flags when in certain cases a 0% release was considered as a 100% release.
- Fixed bug where private project names were shown to members who shouldn't have access.
- Plus 350+ improvements & fixes.


###


Deprecation & removal notices


1. This version (1.31.0) no longer supports a Postgres-only deployment of PostHog. Read[our migration guide](https://posthog.com/docs/migrate/migrate-to-cloud)


for instructions on moving over to a ClickHouse version. ClickHouse provides faster queries and is optimized for very large volumes of data, and you will also get a new lot of features.
2. We're[deprecating the Sessions insight](https://posthog.com/blog/sessions-removal)


(distribution of session length). Please[reach out](https://posthog.com/support)


if you have any feedback on this.


##


Share your feedback


We'd love to hear anything you have to say about PostHog, good or bad. As a thank you, we'll share some awesome[PostHog merch](https://merch.posthog.com/)


.


Want to get involved?Email us to schedule a 30 minute call


with one of our teams to help us make PostHog even better!


##


PostHog News


Welcome Cameron DeLeone! Cameron joined PostHog to help us level up our Customer Success experience. Cameron is a definite no for pineapple on pizza (🍍 on 🍕).


> I've always been a food lover, and started talking about food at 7 months old (my first word was "broc" for broccoli). I haven't shut up about it since.


##


Community


Want to help improve PostHog? We always welcome contributions from our community! Check out our[contributing resources](https://posthog.com/docs/contribute)


to get started.


###


Community shoutouts


We want to thank each and every community member that contributed to this release of PostHog!


- [abtinmo](https://github.com/abtinmo)


- [k4kuz0](https://github.com/k4kuz0)


- [vicampuzano](https://github.com/vicampuzano)


- [jyuvaraj03](https://github.com/jyuvaraj03)


- [ajsharp](https://github.com/ajsharp)


- [maxmue](https://github.com/maxmue)


- [hjweddie](https://github.com/hjweddie)


- [asherf](https://github.com/asherf)


- [chasovskiy](https://github.com/chasovskiy)


- [joesaunderson](https://github.com/joesaunderson)


- [Jaspreet-singh-1032](https://github.com/Jaspreet-singh-1032)


##


Open roles


Join us in helping make more products successful! We're currently hiring for the following roles:


- Full Stack Engineering - Growth
- Operations Manager
- Software Engineer


Learn more about these roles on our[Careers page](https://posthog.com/careers)


.


Don't see a role for you? We're always looking for exceptional people, so reach out to us via the link above.


---


*Enjoyed this? Subscribe to our[newsletter](https://newsletter.posthog.com/subscribe) to hear more from us twice a month!*


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
