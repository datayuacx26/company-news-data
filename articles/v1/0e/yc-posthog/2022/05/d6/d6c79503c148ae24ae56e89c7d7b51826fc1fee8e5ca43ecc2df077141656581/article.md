---
schema_version: "1.0.0"
document_id: "d6c79503c148ae24ae56e89c7d7b51826fc1fee8e5ca43ecc2df077141656581"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1-36-0"
published_at: "2022-05-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:f278cef38b960eee2078f0c6e7d5b027b7c32b846546eaab85939e85b84a4a9b"
---

# Array 1.36.0: Introducing AND/OR filtering, timezone support and universal search

# Array 1.36.0: Introducing AND/OR filtering, timezone support and universal search


May 30, 2022


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
-


Want to know more about what we're up to?[Subscribe to our new newsletter](https://newsletter.posthog.com/subscribe)


, which we send once every two weeks!


> Running a self-hosted instance? Check out our[Upgrading PostHog guide](https://posthog.com/docs/runbook/upgrading-posthog)
>
>
> .


[Patch 1.36.1](https://github.com/PostHog/posthog/compare/release-1.36.0...release-1.36.1)


: This update addresses an issue where some deployments, right after upgrading to 1.36.0, required a restart to fully ingest events again. If you're already on 1.36.0, you only need to update if your instance has problems ingesting events.


##


PostHog 1.36.0 release notes


**Release highlights:**


- Changed:Plugins are now Apps


- New:AND/OR filtering


- New:Multi-dashboard insights


- New:Universal Search


- New:Timezone support for insights


- New:Redesigned Funnels


- Improved:A more accessible color palette


- Improved:Accessible color palette


- Improved:Improved series switching in Persons


- Improved:Better preflight checks


- News:Restack joins PostHog Marketplace


###


Changed: Plugins are now Apps


You may have noticed that we recently changed the name of our plugins, rebranding them to Apps. On the surface this may seem like a simple cosmetic change, but it plays in to the growing capability of plugins apps on PostHog.


We'll have more to share about[what's possible with apps](https://github.com/PostHog/posthog/issues/9654#issuecomment-1133222836)


in the future. For now you can check out[our refreshed library](https://posthog.com/apps)


and a handful of new apps for services such as Amazon Kinesis and[Intercom](https://posthog.com/apps/intercom)


. Enjoy!


###


New: AND/OR filtering


A frequent request in[feedback calls](https://calendly.com/posthog-feedback)


is for more powerful filtering options when creating insights. Specifically, for AND/OR filtering. Finally, we've delivered exactly that.


AND/OR operators can now be used in filters to mix and match conditions into new combinations and identify new sorts of user behavior. We're still interested in any other requests you may have though, so why not[schedule a feedback call](https://calendly.com/posthog-feedback)


and let us know your suggestions?


###


New: Multi-dashboard insights


Got an insight you'd like to have on two dashboards at the same time? Previously you had to duplicate it and keep the copies in sync all the time. What a pain!


Now, from PostHog 1.36 onwards, you can add a single insight to multiple dashboards at once. That means you don't need to worry about maintaining multiple insights and keeping all those copies in sync! Check[the dashboard docs](https://posthog.com/docs/user-guides/dashboards)


for more info.


###


New: Universal search


Ever struggled to find an old insight, event or cohort in your PostHog project? We've added universal search to the familiar "Search" box in the top navigation, so now you have one tool you can use to find *anything* on PostHog.


###


New: Timezone support for insights


PostHog projects have included a timezone setting since[version 1.24](https://posthog.com/blog/the-posthog-array-1-24-0)


, enabling you to manually convert dates and times shown in the app. But 1.36 takes it to the next level.


As of 1.36, all insights queries take into account the timezone you've set in your Project Settings, enabling more accurate results for projects using timezones other than UTC. This is especially helpful when grouping data by day, week, or month!


###


New: Redesigned Funnels


Ever thought funnels on PostHog take up a bit too much space? Ever struggled to read longer funnels which are featured on a dashboard? You weren't the only one. That's why we redesigned them!


The new interface addresses the feedback we've heard, making funnels easier to use and clearer than ever. Check[the Funnels docs](https://posthog.com/docs/user-guides/funnels)


for more information about what's possible with funnels!


###


Improved: A more accessible color palette


Speaking of design, you may have noticed we've updated the color palette in most Insights (specifically:[Trends](https://posthog.com/docs/user-guides/trends)


,[Funnels](https://posthog.com/docs/user-guides/funnels)


,[Retention](https://posthog.com/docs/user-guides/retention)


, and[Stickiness](https://posthog.com/docs/user-guides/stickiness)


).


The new color palette is more pleasing to the eye, but the change isn't just aesthetic. It's also much more accessible for colorblind users. We want to continue improving accessibility in PostHog, so if you have specific feedback on this topic then please[let us know](https://app.posthog.com/home#supportModal)


or[create an issue](https://github.com/PostHog)


!


###


Improved: Switch between series in the persons modal


Thanks to the new series selector, the Persons modal is easier to use with multi-series Trends insights. In fact, you can now switch between series without ever having to close the window, quickly zooming in on particular user segments - just like in the GIF above!


###


Improved: Preflight checks


When self-hosting PostHog, it's crucial that all components are up and running, which is why the preflight checks screen is the first thing you see when setting up a fresh instance. Our new version of this screen makes initial setup of your instance all the more pleasant!


###


New: Restack joins the PostHog Marketplace


Restack is a platform for deployiong open-source software stacks on Kubernetes and is run by a team of open-source enthusiasts. It's a natural match for the PostHog Marketplace, where we've steadily grown the number of businesses offering services and support over the last few months.


Restack enables you to deploy and benefit from a self-hosted PostHog instance, but without the maintenance busy work. Best of all, Restack Console can ensure deployments are[HIPAA](https://posthog.com/docs/privacy/hipaa-compliance)


, FISMA and SOC 2 compliant — handy if you deal with a lot of[PII](https://posthog.com/blog/what-is-personal-data-pii)


. Find out more in[the Marketplace, or head to Restack's store listing for PostHog](https://www.restack.io/store/posthog)


to find more great videos like the one above!


###


Other improvements & fixes


Version 1.36 also adds hundreds of other improvements and fixes, including...


- **New** : Median conversion time is now presented in funnel insights, offering results that are more resistant to being skewed by outliers. The average continues to be available in detailed results.[#9810](https://github.com/PostHog/posthog/pull/9810)


- **Improved** : The in-app plugin source code editor has been divided into tabs, to make editing` plugin.json` and` index.ts` slightly more intuitive. Additionally, code is now automatically formatted with` prettier` on save.[#9795](https://github.com/PostHog/posthog/pull/9795)


- **Improved** : Large numbers are now presented with the comma as the thousands separator in Insights, greatly improving readability.[#9725](https://github.com/PostHog/posthog/pull/9725)


[#9733](https://github.com/PostHog/posthog/pull/9733)


- **Improved** : Properties table column sizing has been made smarter, making them scale better for various contents of properties.[#9502](https://github.com/PostHog/posthog/pull/9502)


- **Fixed** : User Paths with start/end points containing trailing slashes work now. It's also been made impossible to unselect all event categories for a more intuitive experience.[#9482](https://github.com/PostHog/posthog/pull/9482)


- **Fixed** : Trends insight displayed as a table can be sorted independently on dashboards.[#9625](https://github.com/PostHog/posthog/pull/9625)


- **Fixed** : Custom series names are included in CSV exports.[#9677](https://github.com/PostHog/posthog/pull/9677)


- **Fixed** : Automatic provisioning of PostHog users logging in with SSO has been made more reliable. For instance, users who first logged-in *before* automatic provisioning was turned on for the organization, will be automatically added to it when they log in again.[#9515](https://github.com/PostHog/posthog/pull/9515)


- **Fixed** : The UI again always shows the correct active license.[#9575](https://github.com/PostHog/posthog/pull/9575)


View the commit log in GitHub for a full history of changes:[release-1.35.0...release-1.36.0](https://github.com/PostHog/posthog/compare/release-1.35.0...release-1.36.0)


.


###


Deprecation and removal notices


- **For SAML admins:** The previous update,[PostHog 1.35.0](https://posthog.com/blog/the-posthog-array-1-35-0)


, changed SAML from being instance-based to domain-based. This means that SAML configuration takes place in the PostHog UI. You can have multiple SAML providers on the same instance (segment by domain, from the user's email address). Please review our[SSO docs](https://posthog.com/sso)


for more details.
- **For SAML admins:** If you use SAML on a self-hosted instance and have enabled SAML enforcement (previously` SAML_ENFORCED` environment variable) then this environment configuration has been deprecated too. You will now need to configure SSO enforcement via Authentication domains. Check the[SSO docs](https://posthog.com/sso#authentication-domains)


for more details.
- **For plugin app developers:**` onEvent` ,` onShapshot` and` onAction` event payloads have been cleaned up a bit. Specifically,` timestamp` ,` $set` , and` $set_once` are always provided now, while` now` ,` sent_at` ,` site_url` ,` offset` , as well as` kafka_offset` are no longer included. All apps available in our App Store are compatible with this change.` processEvent` is unaffected.


##


Share your feedback


We'd love to hear anything you have to say about PostHog, good or bad. As a thank you, we'll share some awesome[PostHog merch](https://merch.posthog.com/)


.


Want to get involved?Email us to schedule a 30 minute call


with one of our teams to help us make PostHog even better!


##


Contributions from the community


We always welcome contributions from our community and this time we want to thank the following people...


- @ParthGandhi for fixes to PostHog.com
- @JDConley for corrections to PostHog docs


Do you want to get involved in making PostHog better? Check out our[contributing resources](https://posthog.com/docs/contribute)


to get started, or head to[our community page](https://posthog.com/posts)


. We also have a[list of Good First Issues](https://github.com/PostHog/posthog/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)


for ideas on where you can contribute!


##


Open roles at PostHog


Want to join us in helping make more products successful? We're currently hiring for remote candidates in any of the following roles:


- [Growth Engineer](https://posthog.workable.com/jobs/2096622)


- [Site Reliability Engineer - Kubernetes](https://posthog.workable.com/jobs/2105790)


- [Senior Data Engineer](https://posthog.workable.com/jobs/2482259)


- [Senior Product Manager](https://posthog.workable.com/jobs/2490685)


- [Developer Advocate](https://posthog.workable.com/jobs/2422700)


- [Full Stack Engineer - Community Tools, Website & Docs](https://posthog.workable.com/jobs/2175512)


- [Full Stack Engineer](https://posthog.workable.com/jobs/2362038)


- [Marketing Content Writer - 3 Month FTC](https://posthog.workable.com/jobs/2501789)


Curious about what it's like to work at PostHog? Check out our[careers page](https://posthog.com/careers)


for more info about our all-remote team and transparent culture. Don’t see a specific role listed? That doesn't mean we won't have a spot for you.Send us a speculative application!


---


*Follow us on[Twitter](https://twitter.com/PostHog) or[LinkedIn](https://linkedin.com/company/posthog) for more PostHog news!*


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
