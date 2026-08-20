---
schema_version: "1.0.0"
document_id: "8050a7236e5fd13cd230587935c40fc0f3338bc8839f50fe342edc6e3009219e"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/posthog-cloud-eu"
published_at: "2022-10-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:8666ff7787967261d47bb4a471d3ea3a195371b35f727b044edf9f6b6ac7b4f4"
---

# Introducing PostHog Cloud EU

# Introducing PostHog Cloud EU


- [Andy Vandervell](https://posthog.com/community/profiles/30208)


Oct 11, 2022


- [Product updates](https://posthog.com/blog/product-updates)


#### Contents


-
-
-
-
-
-
-


It's no secret that some popular analytics tools, such as Google Analytics, have fallen foul of GDPR regulations in the EU. That's one reason why we've enabled users to self-host PostHog, or to anonymize user data in a way which complies with GDPR.


Today, we're making a third option available: hosting PostHog Cloud in Europe so that user data never has to leave the EU. Now, any business which needs to comply with GDPR regulations can get up and running in a few minutes, and without the ongoing upkeep required for a self-hosted deployment.


Oh, and PostHog Cloud EU also happens to be faster for users located in Europe, which is nice.


PostHog Cloud EU offers everything you'd expect of PostHog – analytics, session recording, experiments and more. The only difference is that you can now choose to host in a different region, at no extra cost.


Need analytics hosted in the EU?


[Try PostHog Cloud EU for free](https://eu.posthog.com/signup)


##


FAQ


###


Where are PostHog Cloud EU's servers based?


PostHog Cloud EU is hosted in the AWS` eu-central-1` region based in Frankfurt, Germany.


###


What data is transferred from the EU to the US?


None. PostHog Cloud EU is an entirely independent instance of PostHog. All event data, user data, and the product itself, is hosted on our EU-based infrastructure.


###


How do I use PostHog Cloud EU in compliance with GDPR?


Just deploying PostHog Cloud EU isn't enough – you still need to[acquire consent](https://posthog.com/tutorials/react-cookie-banner)


from your users and comply with all other provisions of the GDPR, such as the right to be forgotten. Please see our[GDPR guidance](https://posthog.com/docs/privacy/gdpr-compliance)


for more information.


###


What does PostHog Cloud EU cost?


For now, we're charging[the same as "regular" PostHog Cloud](https://posthog.com/pricing)


. The first 1 million events/month are free, then we charge monthly per event after that.[Get started here](https://eu.posthog.com/signup)


.


###


I'm an existing PostHog customer, can I migrate to PostHog Cloud EU?


It is possible to migrate event data from another PostHog instance to EU Cloud, but it is a time-consuming process likely to take several days or weeks. There is currently no way to migrate other information, including insights, dashboards, user accounts, feature flags or API keys.


We strongly recommend that users with large volumes of data begin by setting up and connect a new EU cloud instance and only attempt to migrate data later, if needed, by following[this migration tutorial](https://posthog.com/tutorials/migrate-eu-cloud)


.


##


GDPR compliance checklist


We recommend completing the following steps to ensure GDPR compliance when using PostHog Cloud EU, but please seek out expert advice. Further GDPR advice can be found on the[official GDPR.eu website](https://gdpr.eu/checklist/)


1.


**Deploy PostHog Cloud EU:** Deploying PostHog Cloud EU is easy – simply follow the steps in the onboarding process. More information is available[via our docs](https://posthog.com/docs/getting-started/cloud)


.


2.


**Review what data you're collecting:** It's important to understand what data you're collecting using PostHog, or any other tools you use. Please read our[privacy documentation](https://posthog.com/docs/privacy)


and[privacy policy](https://posthog.com/privacy)


for more information on what data PostHog collects.


3.


**Update your privacy policy and terms:** You must identify PostHog as a tool in your terms. When using PostHog Cloud EU, PostHog is the Data Processor and you are the Data Controller.


4.


**Acquire consent from users:** You must give users the option to opt out of cookies. If you don't already do this, we've created a tutorial for[creating a simple GDPR banner using React](https://posthog.com/tutorials/react-cookie-banner)


. It is also possible to run PostHog Cloud EU without cookies – see our[cookieless tracking guide](https://posthog.com/tutorials/cookieless-tracking)


.


5.


**Create "right to be forgotten" process:** A user must be able to request that their data be removed from PostHog. How you facilitate that request is up to you. Information on how to delete user data is[available in our docs](https://posthog.com/docs/privacy/data-deletion)


.


Need analytics hosted in the EU?


[Try PostHog Cloud EU for free](https://eu.posthog.com/signup)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
