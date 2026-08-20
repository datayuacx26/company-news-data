---
schema_version: "1.0.0"
document_id: "e3d1262561b3977e523a1f21e5ff198ba25c2c8426d92e722aeb881ec2dc5097"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1-34-0"
published_at: "2022-03-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:c7789d8cba2a2e6cabe4e46b42a455d835d607bad6de44cac607bb63faef5d98"
---

# Array 1.34.0

# Array 1.34.0


Mar 30, 2022


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


PostHog 1.34.0 focuses on supporting collaboration and helping you to find and share insights on PostHog. New features in this update include a new default homepage for projects, smart names for all insights and an entire Data Management tool!


> **IMPORTANT!** Before upgrading, make sure to run all async migrations on your instance. This version will not run until async migrations are completed. Please check out the[async migrations](https://posthog.com/docs/runbook/async-migrations)
>
>
> docs for details.


> **IMPORTANT!** If you use SAML please read ourdeprecation notices below
>
>
> .


##


PostHog 1.34.0 release notes


> Wondering how to upgrade a self-hosted instance? Check out our[upgrade guide](https://posthog.com/docs/runbook/upgrading-posthog)
>
>
> .


**Release highlights:**


- Project homepages


- Data Management


- Property group filtering


- Smart insight naming


- Staff user controls


- Sharding and replication support for ClickHouse


###


New: Project homepages


We've added default homepages for each project in PostHog, to help you explore new data and keep track on important metrics. You can pin your favorite dashboard directly to this new homepage to easily access those insights and we'll also surface popular insights for you too.


Best of all, if your team uses[Session Recording](https://posthog.com/product/session-recording)


, we'll also add your most recent recordings to the homepage too. We've found users who watch these recordings make 3x more useful discoveries, on average!


###


New: Data Management


We've also added a new Data Management system! This new tool helps you organize your PostHog data in-house through event definitions, definition tagging and many more features. This is especially useful in large teams, where it can sometimes be difficult to know if your insights are using the right events.


Check out our[blogpost which explains more about Data Management on PostHog](https://posthog.com/blog/data-management-feature)


or dive straight into the[Data Management docs](https://posthog.com/docs/user-guides/data-management)


for more information.


> 🎁 Definition creation, tagging, and volume metrics are premium features which require a PostHog Scale or Enterprise license.[Learn more](https://posthog.com/pricing)
>
>
> .


###


Improvement: Smart insight naming


Previously, we generated random default names for insights based on animals. It was fun and ensured each insight had a unique name, but they weren't very descriptive and didn't help if you wanted to browse existing insights.


Now, we've added smart insight naming to automatically generate descriptive names for every insight you create. This makes it a *lot* easier to find the insights you need - thought you can of course add custom animal names, if you want.


###


New: Property group filtering


One of the most requested features for PostHog, property group filters enable you to mix and match your favourite property filters with` OR` s and` AND` conditions. This makes it much simpler to create complex filters when building insights, though this is currently only available as a global filter for insights.


###


New: Staff user controls


Self-hosted users can now manage staff users on their instance settings. Staff users are a special instance-level permission which have access to instance-wide settings - and you probably want to have multiple staff users enabled to ensure your instance can always be properly maintained.


You can manage staff users either via the PostHog UI, or the API if you prefer - check out[the staff user docs](https://posthog.com/docs/self-host/configure/instance-settings#staff-users)


for more information.


###


New: Sharding and replication support for ClickHouse


Self-hosted users can now shard and replicate their data on ClickHouse to scale horizontally. This can help improve fault tolerance and improve query performance in certain situations.


Check out the[horizontal scaling runbook](https://posthog.com/docs/runbook/services/clickhouse/sharding-and-replication)


for more information.


###


Other improvements & fixes


Version 1.34.0 also adds hundreds of other fixes and improvements, including...


- **Improved** : Dashboard "add insight" flow
- **Improved** : SSO login redirect
- **New:** Experiments secondary metrics
- **New** : Shorter insight links


###


Deprecation and removal notices


- Starting from PostHog 1.35.0 SAML will change from being instance-based to domain-based. This means that SAML configurations will take place in the PostHog UI. You will be able to have multiple SAML providers on the same instance (segment by domain, from the user's email address). Please review our[SSO docs](https://posthog.com/sso)


for more details.
- If you use SAML on a self-hosted instance and have enabled SAML enforcement (previously` SAML_ENFORCED` environment variable) then this environment configuration has been deprecated too. You will now need to configure SSO enforcement via Authentication domains (see[SSO docs](https://posthog.com/sso)


for more details.)


##


Share your feedback


We'd love to hear anything you have to say about PostHog, good or bad. As a thank you, we'll share some awesome[PostHog merch](https://merch.posthog.com/)


.


Want to get involved?Email us to schedule a 30 minute call


with one of our teams to help us make PostHog even better!


##


Contributions from the community


Want to help improve PostHog? We always welcome contributions from our community! Check out our[contributing resources](https://posthog.com/docs/contribute)


to get started, or head to[our community page](https://posthog.com/posts)


.


We would also specifically like to thank PostHog team members[@hazzadous](https://github.com/hazzadous)


,[@guidoiaquinti](https://github.com/guidoiaquinti)


and[@liyiy](https://github.com/liyiy)


for their efforts on this particularly stubborn release.


###


Community shoutouts


This month we also want to say thank you to the following people...


- [Bnomei](https://github.com/bnomei)


for providing[feedback on our PHP docs](https://github.com/PostHog/posthog.com/issues/3190)


- [Hspotorno](https://github.com/hspotorno)


for additions to[Is Google Analytics Illegal?](https://isgoogleanalyticsillegal.com/)


- [Bla](https://github.com/Bla)


for submitting[ideas and feedback](https://github.com/PostHog/posthog/issues/1)


##


Open roles at PostHog


Want to join us in helping make more products successful? We're currently hiring for remote candidates in any of the following roles:


- [Community Engineer](https://apply.workable.com/posthog/j/449572FD18/)


- [Site Reliability Engineer - Kubernetes](https://apply.workable.com/posthog/j/7A6F1142D0/)


- [Full Stack Engineer - Growth](https://apply.workable.com/posthog/j/2682B00B76/)


- [Tech Talent Partner](https://apply.workable.com/posthog/j/AB22DA7D5F/)


Curious about what it's like to work at PostHog? Check out our[careers page](https://posthog.com/careers)


for more info about our all-remote team and transparent culture. We also welcome speculative applications for roles from exceptional candidates.


---


*Follow us on[Twitter](https://twitter.com/PostHog) or[LinkedIn](https://linkedin.com/company/posthog) for more PostHog news!*


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
