---
schema_version: "1.0.0"
document_id: "d018f7147d4aa9616a9a9553811c506cf2fb2ed8f2d628da186b4e3433533251"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/launch-week-universe-of-new-features"
published_at: "2022-03-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:4df0e9faf6bb76c5525c8453e37e3c21fff9501599c17cb9d6a4d60a06a51533"
---

# PostHog Launch Week I: A Universe of New Features

# PostHog Launch Week I: A Universe of New Features


- [Andy Vandervell](https://posthog.com/community/profiles/30208)


Mar 23, 2022


- [Product updates](https://posthog.com/blog/product-updates)


#### Contents


-
-
-
-
-


PostHog made huge strides in 2021. To name just a few landmarks, we:


- Raised a[$15 million Series B ahead of schedule](https://posthog.com/blog/why-we-raised-a-15m-series-b-ahead-of-schedule)


- Became one of[YC's top-valued companies](https://posthog.com/blog/yc-top-companies)


- Rebranded our website[in just four weeks](https://posthog.com/blog/postmortem-rebrand)


- Migrated from[postgresql to ClickHouse](https://posthog.com/blog/how-we-turned-clickhouse-into-our-eventmansion)


- Achieved product market fit for PostHog Scale


All of this (and more) lead to huge growth. Users grew by 4x, the number of open source contributors doubled and we acquired[numerous reference customers](https://posthog.com/customers)


.


But this is just the beginning. We've also been busy making PostHog a broader and more powerful tool for making great products.


To celebrate, we're officially launching a new feature or initiative every day this week. Welcome to *PostHog Launch Week I: A Universe of New Features* .


##


Chapter I: Data Management


- **Starring:** Alex Kim
- **Introducing:** New Data Management UI, event definitions, definition tagging and many more


As the biggest dogfooder of our own product, we were beginning to see the consequences of the lack of in-house tools to manage our data. All the use cases we saw boiled down to a few common pain points:


1. It is hard to understand events, actions, and properties that you yourself didn't set up.
2. Data decays quickly, and most of the time fails to stay up-to-date with instrumentation.
3. Event-level context in insights and dashboards is difficult to share with external people and new team members.


We set out to solve these pain points and the result is the new Data Management experience we're introducing today on PostHog Cloud and next week for self-hosted.


**Read:**[Introducing Data Management for PostHog](https://posthog.com/blog/data-management-feature)


##


Chapter II: Project Sparkle


- **Starring:** Karl-Aksel Puulmann, Harry Waye and Guido Iaquinti
- **Introducing:** Sharded ClickHouse and easier self-hosted deployments


We created 'Project Sparkle' to address two challenges: to **make PostHog scalable for anyone** and to **make self-hosting PostHog easier** .


In[The secrets of PostHog query performance](https://posthog.com/blog/secrets-of-posthog-query-performance)


, Karl-Aksel Puulmann details the work we've done to make PostHog faster and more scalable, including (among other things) a massive 55% improvement to query performance on PostHog Cloud.


In[How we’re making PostHog deployments easier](https://posthog.com/blog/improving-posthog-deployments)


, Harry Waye and Guido Iaquinti explain how PostHog's architecture has evolved since its launch in 2020, and the steps we're taking to make self-hosted deployments easier, including multi-layered testing and improved monitoring.


Finally, we're delighted to officially confirm our[partnerhship with Altinity](https://posthog.com/blog/posthog-altinity-announce)


and the launch of[PostHog Marketplace](https://posthog.com/marketplace)


. You may know[Altinity](https://altinity.com/)


as experts in all things ClickHouse and data infrastructure. Altinity will be offering a variety of services to help give large organizations on PostHog an increased level of support for self-hosted deployments.


Going forward, the marketplace is where you’ll be able to find third-party services which layer on top of PostHog, such as companies offering support, integration or development services. Altinity is the first such company, but there are more to come in the future.


##


Chapter III: Experimentation


- **Starring:** Neil Kakkar
- **Introducing:** A/B testing, multivariate tests and statistical significance


Everyone has an opinion about user experience, and that's fine. But it's vital to test those opinions and validate what works, which is why we built our Experimentation suite.


Experiments allow you to choose a target metric, choose specific people to run this experiment on, and set how long the experiment runs for.


Thanks to Feature Flags, you can then easily validate whether each variant looks good, launch your experiment, and wait for data to come in. We run a Bayesian analysis on the data to give a probability for each variant being the best, show a graph of how things are looking for each variant, and whether the results are statistically significant or not. It's awesome.


Today, Neil Kakkar shares three things we've learned about running effective A/B tests. If you want to build better products, we seriously recommend giving it a read.


**Read:[What launching Experimentation taught us about running effective A/B tests](https://posthog.com/blog/experiments)**


##


Chapter IV: Collaboration


- **Releasing:** Thursday, March 24
- **Starring:** Rick Marron and Paolo D'Amico
- **Introducing:** Granular dashboard permissions, automatic insight naming and much more


95% fresh on Tasty Earthworms


##


Chapter V: Session Recording


- **Releasing:** Friday, March 25
- **Starring:** Rick Marron
- **Introducing:** The most addictive feature you'll ever know


"So good, I wrote a 1,000 word Twitter thread about it" - A Twitter user, probably


*Enjoyed this? Subscribe to our[newsletter](https://newsletter.posthog.com/subscribe) to hear more from us twice a month!*


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
