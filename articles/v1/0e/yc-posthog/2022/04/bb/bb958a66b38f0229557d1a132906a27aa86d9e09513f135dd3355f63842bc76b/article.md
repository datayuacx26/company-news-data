---
schema_version: "1.0.0"
document_id: "bb958a66b38f0229557d1a132906a27aa86d9e09513f135dd3355f63842bc76b"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1-35-0"
published_at: "2022-04-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:463cf5b77cf937785ff711a3adb0fe78bc445f52c4a671c3fad35d145f3fda33"
---

# Array 1.35.0: Introducing SAML, world map view and new plugins

# Array 1.35.0: Introducing SAML, world map view and new plugins


Apr 25, 2022


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


PostHog 1.35.0 introduces audit logs and a brand new world map view to help you visualize where your users are coming from. Additionally, we now support organization-level SAML login on both Cloud and Self-Hosted instances. Plus, check out your project homepage for a few additional goodies!


We've also recently launched a new newsletter, which we send once every two weeks![Subscribe today](http://newsletter.posthog.com/subscribe)


to hear about our latest news, what we're reading and how to get the most out of PostHog.


> **IMPORTANT!** If you use SAML, please read ourdeprecation notices below
>
>
> .


##


PostHog 1.35.0 release notes


> Wondering how to upgrade a self-hosted instance? Check out our[upgrade guide](https://posthog.com/docs/runbook/upgrading-posthog)
>
>
> .


**Release highlights:**


- Activity log


- World map


- Multi-tenant SAML


- Project homepage updates


- New Avo plugin


###


New: Activity log


Ever wondered who deleted that feature flag, or merged those two people? Wonder no more with the new activity log!


You can now view changes to feature flags and persons made in the UI and see who made them, when. Watch out for the activity log being added to more pages in future releases. Or[tell us](https://app.posthog.com/home#supportModal)


where we should add it next!


###


New: World map


A staple request of every PostHog hackathon has finally been realized, with the introduction of **world map** – a new Trends chart type which visualizes breakdowns by country (based on[GeoIP](https://posthog.com/integrations/geoip)


data). Intuitively compare how your traffic and usage differ across countries of the world!


###


New: Multi-tenant SAML


SAML has been a big focus point in this update (check the deprecation notices below for more info) and the upshot is that you can now configure SAML login on both self-hosted and PostHog Cloud instances!


You can configure up to one SAML provider per organization, but check out our[SSO docs](https://posthog.com/sso)


for more information on how to get started with SAML on PostHog.


> **IMPORTANT!** If you use SAML please read ourdeprecation notices below
>
>
> . SAML support is only available on Enterprise plans.


###


Improvement: Project homepage updates


In the last update, we introduced[project homepages](https://posthog.com/blog/the-posthog-array-1-34-0#new-homepage)


to help you find information quickly. Now, we've made them even better!


We've added a new homepage card which lists your recently viewed insights to the project homepage, as well as lists of recent recordings and newly identified persons. All of this is to help give you quick access to commonly-visited pages and recent insights.


###


New: Avo plugin


We're big fans of[the Avo data governance platform](https://www.avo.app/)


and were lucky enough to meet the team in person at our recent Iceland offsite. Now, we've launched a new plugin too!


The plugin sends event schemas to the Avo Inspector as they are ingested by PostHog, enabling you to detect inconsistencies in your schema without sharing any user data. Install it from the plugin tab to get started, or find out more in[Avo's documentation](https://www.avo.app/docs/workspace/connect-inspector-to-posthog#step-2-enable-avo-inspector-plugin-in-posthog)


.


###


Other improvements & fixes


Version 1.35.0 also adds hundreds of other fixes and improvements, including...


- **New** : Warnings before leaving an insight with unsaved changes
- **New** : Page titles that fully reflect your place in the PostHog app
- **New** : "Out-of-band" events shown in session recordings


###


Deprecation and removal notices


- From PostHog 1.35.0 onwards, SAML will change from being instance-based to domain-based. This means that SAML configurations will take place in the PostHog UI. You will be able to have multiple SAML providers on the same instance (segment by domain, from the user's email address). Please review our[SSO docs](https://posthog.com/sso)


for more details.
- If you use SAML on a self-hosted instance and have enabled SAML enforcement (previously` SAML_ENFORCED` environment variable) then this environment configuration has been deprecated too. You will now need to configure SSO enforcement via Authentication domains. Check the[SSO docs](https://posthog.com/sso)


for more details.


##


Share your feedback


We'd love to hear anything you have to say about PostHog, good or bad. As a thank you, we'll share some awesome[PostHog merch](https://merch.posthog.com/)


.


Want to get involved?Email us to schedule a 30 minute call


with one of our teams to help us make PostHog even better!


##


Contributions from the community


We always welcome contributions from our community and this time we want to thank the following people...


- [Michael Cavallaro](https://github.com/Cavallando)


, for Android bug fixes
- [Rory Crispin](https://github.com/RoryCrispin)


, for Docs improvements
- [Rahul](https://github.com/rahul3v)


, for website improvements
- [Alberto](https://github.com/albtsantos)


, for plugin enhancements
- [Joe Trollo](https://github.com/joetrollo)


, for Kafka and SASL support
- [Björn and the rest of the Avo team](https://github.com/bjornj12)


, for the Avo plugin


Do you want to get involved in making PostHog better? Check out our[contributing resources](https://posthog.com/docs/contribute)


to get started, or head to[our community page](https://posthog.com/posts)


. We also have a[list of Good First Issues](https://github.com/PostHog/posthog/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)


for ideas on where you can contribute!


##


Open roles at PostHog


Want to join us in helping make more products successful? We're currently hiring for remote candidates in any of the following roles:


- [Growth Engineer](https://apply.workable.com/posthog/j/F6B73AD2F6/)


- [Site Reliabliulity Engineer - Kubernetes](https://apply.workable.com/posthog/j/7A6F1142D0/)


- [Community Engineer](https://apply.workable.com/posthog/j/449572FD18/)


- [Full Stack Engineer](https://apply.workable.com/posthog/j/2682B00B76/)


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
