---
schema_version: "1.0.0"
document_id: "4b4949367c2e6a07250b0dc517a2cfa3eefe8b3f73d49fadf197a5c0a7b5c334"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1-30-0"
published_at: "2021-11-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:c97f8b09f18c9b4a238959f14f1ac4cdc6e0da5a07005953865ee7a7c2d92f93"
---

# Array 1.30.0

# Array 1.30.0


Nov 17, 2021


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


PostHog 1.30.0 is a milestone release! We've introduced a brand new, faster user interface, automatic conversion signal detection with correlation analysis, the ability to save insights for future use and a fully revamped recordings playback experience. And that's just for starters!


> **Postgres-based deployments are now deprecated** in favor of ClickHouse-backed installations. It's important to migrate your installation to keep getting the latest updates and features.Read more
>
>
> about this below.


##


PostHog 1.30.0 release notes


> Don't see the new features on your self-hosted deployment? Remember to[update your PostHog instance](https://posthog.com/docs/runbook/upgrading-posthog)
>
>
> .


**Release highlights:**


- PostHog 1.30.0 release notes


- Fresh new look-and-feel


- Correlation analysis


- Saved insights


- Fully revamped recordings


- Other improvements \\& fixes


- Deprecation \\& removal notices


- Help us improve PostHog


- Community


- Community MVP 🏆


- Community shoutouts


- Open roles


###


Fresh new look-and-feel


We reworked PostHog's UI philosophy from the ground up, all to offer you the most intuitive and sleek user experience possible. With 1.30.0 we're introducing our redesigned navigation experience – codenamed Lemonade for its freshness. An evolution of the interface you know, it's simply much nicer packaging for the existing features, along with a new helpful addition: breadcrumbs for hierarchical navigation. We hope you'll find this a joy to use. And if you have any feedback regarding the redesign, we'd love to hear your thoughts!


In addition to the new coat of paint, we've been working on performance improvements – codenamed Turbo Mode. Although not visible at first glance, recent under-the-hood changes make switching between pages feel smoother and snappier.


###


Correlation analysis


Want to understand why users convert or churn? Presenting: Correlation analysis. This nifty new insight automatically matches funnels to any relevant conversion signals, giving you effortless correlation information such as "Users in Canada are 5x more likely to convert" or "Users in Chrome are 3x less likely to convert". This is a very powerful feature which enables you to take funnel optimization to the next level.


Read more on the[Correlation analysis docs](https://posthog.com/docs/user-guides/correlation)


.


> 🎁 Correlation analysis is a premium feature and requires a PostHog Scale or Enterprise license.[Learn more](https://posthog.com/pricing)
>
>
> .


###


Saved insights


Tired of creating the same insights multiple times? You can now save insights on PostHog without adding them to a dashboard. Further, you're able to see, search and filter a list of insights created by other team members - which makes it a lot easier to collaborate with PostHog.


###


Fully revamped recordings


The recordings experience just got a lot better. We added a new recordings tab that enables you to filter and search across multiple days (replacing the[old sessions tab](https://posthog.com/blog/sessions-removal)


). Once you've found the recording you want to watch, there's a brand new player experience that loads much faster and overlays events on the seekbar. Find the right spot in a recording quickly and understand better what your users are doing.


###


Other improvements & fixes


- **Turbo mode** . You'll notice a significant speed improvement when using PostHog. App navigation will now happen almost instantly. In particular, you'll notice navigation between dashboards and insights happens without any delay.
- **Duplicate dashboards** . Thanks to community member[Yuvaraj J](https://github.com/PostHog/posthog/pull/6476)


, you can now easily duplicate dashboards instead of manually recreating them. Thanks, Yuvaraj!
- **Security on Docker builds.** We've moved to a different base image for Docker (` alpine` ) and this new image build solves a lot of security vulnerabilities on upstream dependencies.
- **Improved query performance** . We improved how person properties are handled which results in up to 2x faster queries.
- Have a large number of dashboards? You'll now be able to **easily search the dashboard list.**
- Fixed a bug that caused the app to believe there was a new version available when it wasn't ready to be shipped.
- Fixed bugs with person counts not matching between an insight graph and the person list.
- Significantly faster frontend builds as we transitioned from` webpack` to` esbuild` .
- Fixed a bug that prevented creating cohorts from trends.
- Success and error toast alerts will now show at the bottom of the screen so it no longer covers critical elements in the page.
- Plus **350+ more** improvements & fixes.


###


Deprecation & removal notices


1. This version (` 1.30.0` ) will be the last version where we support a Postgres-only deployment of PostHog. See[our migration guide](https://posthog.com/docs/migrate/migrate-to-cloud)


for instructions on moving over to a ClickHouse version. ClickHouse provides faster queries and is optimized for very large volumes of data, and you will also get a new lot of features.
2. We're now fully removing the legacy Sessions list page. Read more about it,[in this blog post](https://posthog.com/blog/sessions-removal)


.


##


Share your feedback


We'd love to hear anything you have to say about PostHog, good or bad. As a thank you, we'll share some awesome[PostHog merch](https://merch.posthog.com/)


.


Want to get involved?Email us to schedule a 30 minute call


with one of our teams to help us make PostHog even better!


##


Community


###


Community MVP 🏆


Thanks to all our community members for helping move PostHog forward! This release cycle's Community MVP goes to[asherf](https://github.com/asherf)


!


Asher pushed several PRs to improve the Python code quality in the main PostHog app.


###


Community shoutouts


We want to thank each and every community member that contributed to this release of PostHog!


- [asherf](https://github.com/asherf)


🏆
- [banagale](https://github.com/banagale)


- [pixlwave](https://github.com/pixlwave)


- [romj](https://github.com/romj)


- [Nishant-Sagar](https://github.com/Nishant-Sagar)


- [xrendan](https://github.com/xrendan)


- [inbreaks](https://github.com/inbreaks)


- [Jaspreet-singh-1032](https://github.com/Jaspreet-singh-1032)


- [mether](https://github.com/mether)


- [jyuvaraj03](https://github.com/jyuvaraj03)


Looking to contribute? We've recently improved our CI process and tests will now run smoothly if you create a PR from a fork.


##


Open roles


Join us in helping make more products successful! We're currently hiring for the following roles:


- Developer Educator
- Operations Manager
- Sales Engineer
- Technical Customer Success Manager
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
