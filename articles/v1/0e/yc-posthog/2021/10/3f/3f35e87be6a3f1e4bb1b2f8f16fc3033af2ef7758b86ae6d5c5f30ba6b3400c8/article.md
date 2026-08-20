---
schema_version: "1.0.0"
document_id: "3f35e87be6a3f1e4bb1b2f8f16fc3033af2ef7758b86ae6d5c5f30ba6b3400c8"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1-29-0"
published_at: "2021-10-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:bd76d1733d30c22a7267ddff8cd1aef55a5ebf08b7bf21310e30a0a0375ea23a"
---

# Array 1.29.0

# Array 1.29.0


Oct 21, 2021


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


> 💡 This version contains a patch release (1.29.1), which fixes a migration lock when upgrading to 1.29.0. If you have issues upgrading to 1.29.0, please try upgrading directly to 1.29.1.


PostHog 1.29.0 includes a fully revamped Paths experience which enables you to better explore what actions your users take. New features include multivariate feature flags, private projects and DAU/WAU/MAU graphs.


##


PostHog 1.29.0 release notes


> For a better experience and new features, remember to[update your PostHog instance](https://posthog.com/docs/runbook/upgrading-posthog)
>
>
> .


**Release highlights:**


- PostHog 1.29.0 release notes


- Explore and deep dive with Paths


- Multivariate support in feature flags


- Private projects


- Trailing DAU/WAU/MAU graphs


- Other improvements \\& fixes


- Deprecation notice


- Share your feedback


- PostHog News


- Community


- Community MVP 🏆


- Community shoutouts


- Open roles


###


Explore and deep dive with Paths


Continuing our quest to help you understand why your users convert or don't convert, we fully revamped our Paths feature to help you explore the actions users are taking.


From jumping from a conversion drop-off in a funnel to identifying paths ending in a desired action, you will be able to fully understand the paths of your users. We're introducing a lot of additional features such as:


- The ability to select up to 20 steps
- Fine-grained controls on what paths to show
- Grouping paths through wildcards


> 🎁 Some of these new features require a PostHog Scale or Enterprise license.[Learn more](https://posthog.com/pricing)
>
>
> .


###


Multivariate support in feature flags


Feature flags just got a lot more powerful! Forget A/B testing, introducing A/B/C/D/... testing. You will now be able to create feature flags with multiple variants to allow for more comprehensive testing and feature releases.


###


Private projects


Extra concerns on privacy or compliance? Private projects now allow you to limit access to select team members. Learn more on[our docs](https://posthog.com/docs/settings/access-control)


.


> 🎁 Private projects is a premium feature and requires a PostHog Scale or Enterprise license.[Learn more](https://posthog.com/pricing)
>
>
> .


###


Trailing DAU/WAU/MAU graphs


If you're interested in better measuring your user engagement, DAU/WAU, WAU/MAU & DAU/MAU ratios can provide great signals. Create graphs like this to better understand your engagement.


###


Other improvements & fixes


- **Renaming filters** - You will now be able to set custom names for your graph series. Your teammates will now be able to better understand your graphs.
- **UI improvements** - We're introducing significant UI improvements across the board to better match our[new branding](https://posthog.com/founders/postmortem-rebrand)


.
- **Column configurator** - You will now be able to select the columns you want to see in your events table.


- **Password reset revamp** - We've shipped a lot of improvements to the password reset experience, including a new UI, API processing, and even new email designs 🎨. If you run with SAML-only login, password reset is now disabled.
- **Fixes to dashboard loading** - Previously loading huge dashboards could be quite slow and in some scenarios even cause some backend strain, this is now fixed.
- **Improved navigation in Insights** - A lot of under-the-hood improvements which will make navigation in Insights significantly smoother. Clicking back in your browser will now work as expected.
- **Automatically load new events** in the events table.
- **Fixed colors in insight table** - We've fixed a pesky bug in which graph series were being displayed with different colors on the graph versus on the table below.
- **Fixes to tooltips and person deep dive** - We've improved the way we display information on tooltips and the person deep dive modal.
- Plus **350+ more** improvements & fixes.


###


Deprecation notice


1.


We're deprecating the sessions page and fully removing it in PostHog 1.30.0. Read more about it,[in this blog post](https://posthog.com/blog/sessions-removal)


. If you have any feedback on this change, please[reach out](https://posthog.com/posts)


.


2.


In PostHog 1.30.0 we will be introducing major improvements to the experience of using PostHog with multiple projects and that requires us to rework part of the API structure. Hence, in PostHog 1.29.0 the following API paths are deprecated, with straightforward replacements:


- ` /api/action/` becomes` /projects/<project_id>/actions/`
- ` /api/annotation/` becomes` /projects/<project_id>/annotations/`
- ` /api/cohort/` becomes` /projects/<project_id>/cohorts/`
- ` /api/dashboard/` becomes` /projects/<project_id>/dashboards/`
- ` /api/dashboard_item/` becomes` /projects/<project_id>/saved_insights/`
- ` /api/element/` becomes` /projects/<project_id>/elements/`
- ` /api/event/` becomes` /projects/<project_id>/events/`
- ` /api/feature_flag/` becomes` /projects/<project_id>/feature_flags/`
- ` /api/insight/` becomes` /projects/<project_id>/insights/`
- ` /api/paths/` becomes` /projects/<project_id>/paths/`
- ` /api/person/` becomes` /projects/<project_id>/persons/`
- ` /api/plugin_config/` becomes` /projects/<project_id>/plugin_configs/`
- ` /api/sessions_filter/` becomes` /projects/<project_id>/session_filters/`


In a future PostHog version the deprecated paths will be removed. At the same time we will also have to remove the special` project_id` value` @current` (representing the currently selected project).


##


Share your feedback


We'd love to hear anything you have to say about PostHog, good or bad. As a thank you, we'll share some awesome[PostHog merch](https://merch.posthog.com/)


.


Want to get involved?Email us to schedule a 30 minute call


with one of our teams to help us make PostHog even better!


##


PostHog News


Welcome Harry Waye! Harry is joining us as a Full Stack Engineer on the Platform Team.


##


Community


###


Community MVP 🏆


Thanks to all our community members for helping move PostHog forward! This release cycle's Community MVP goes to[jyuvaraj03](https://github.com/jyuvaraj03)


!


Yuvaraj implemented the highly requested feature to let you duplicate dashboards! This way, you will now be able to use a dashboard as a template for other dashboards. The feature will be fully available on the next release (1.30.0).


###


Community shoutouts


We want to thank each and every community member that contributed to this release of PostHog!


- [jyuvaraj03](https://github.com/jyuvaraj03)


🏆
- [tirkarthi](https://github.com/tirkarthi)


- [geary](https://github.com/geary)


- [dakshshah96](https://github.com/dakshshah96)


- [terrymunro](https://github.com/terrymunro)


- [stefnnn](https://github.com/stefnnn)


- [csykes](https://github.com/csykes)


- [skabbes](https://github.com/skabbes)


- [banagale](https://github.com/banagale)


- [leoMehlig](https://github.com/leoMehlig)


- [asherf](https://github.com/asherf)


- [steveyackey](https://github.com/steveyackey)


##


Open roles


Join us in helping make more products successful! We're currently hiring for the following roles:


- Developer Educator
- Software Engineer
- Technical Content Marketer
- Technical Customer Success Manager


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
