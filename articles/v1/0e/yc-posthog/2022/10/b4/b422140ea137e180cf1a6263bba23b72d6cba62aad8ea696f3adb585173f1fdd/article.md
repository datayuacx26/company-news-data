---
schema_version: "1.0.0"
document_id: "b422140ea137e180cf1a6263bba23b72d6cba62aad8ea696f3adb585173f1fdd"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/analytics-tips-for-customer-success-teams"
published_at: "2022-10-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:0f38499d4715190e4f9f880b67fc33198dab38a1a3ecf83b2ff5eee9062fbcc9"
---

# 5 essential tips for Customer Success teams on PostHog

# 5 essential tips for Customer Success teams on PostHog


- [Joe Martin](https://posthog.com/community/profiles/29070)


Oct 10, 2022


- [Guides](https://posthog.com/blog/guides)


#### Contents


-
-
-
-
-


While PostHog is obviously useful for product managers, engineers and analysts, there’s a lot it can do for other teams too — including customer success. Internally, for example,[our CS team](https://posthog.com/blog/customer-success-at-posthog)


uses PostHog to track a variety of relevant metrics and to preemptively identify organizations that would benefit from their support.


You can find out more about[how our customer success team works](https://posthog.com/handbook/people/team-structure/customer-success)


in the PostHog docs, but in the meantime here are five tips to help *your* CS team get started with PostHog…


##


1. Use funnels to find out where customers get stuck


Using a combination of[autocapture](https://posthog.com/docs/data/autocapture)


and[custom or calculated events](https://posthog.com/tutorials/event-tracking-guide#using-custom-events-to-track-advanced-behaviors)


, PostHog makes it possible to track every part of your product experience. This makes it trivial to track where users are getting stuck by using[funnel insights](https://posthog.com/manual/funnels)


to visualize the user journey.


At PostHog, for example, new users have to go through several common steps to be successful — they must create an account, login, ingest events and then make a discovery through an insight. Using funnels, we’re able to identify which stage users get stuck on and then monitor how successful we are in our attempts to fix the problem.


> 💡 **PostHog Tip:** Want to delve deeper into the root causes behind a drop off? Use[breakdowns](https://posthog.com/manual/funnels#identify-possible-causes-of-failure-and-success)
>
>
> to explore what groups of users have in common.


##


2. Use trends to find out which features are popular


If you, like PostHog, charge based on usage then you’ll want to keep a close eye on your usage metrics. This will both help you spot trends in declining usage so that you can react early, and also proactively reach out to customers with increasing usage too.


The simplest way to monitor usage is via a[trends insight](https://posthog.com/manual/trends)


. Just create an insight which tracks your usage events on a reasonable timeline and you’ll be able to see how it changes over time. At PostHog, we actually monitor usage for each tool on the platform, as well as the overall events ingested — this helps us not only understand usage trends but also see which features are most popular.


> 💡 **PostHog Tip:** If you’ve only got a small number of users, your results may get swayed by internal users or test accounts.[Here’s how to filter them out](https://posthog.com/tutorials/filter-internal-users)
>
>
> .


##


3. Use lifecycle charts to find users about to churn


Regardless of whether or not your overall usage is growing, it’s a good idea to monitor your overall retention and identify which customers are falling dormant and are at risk of churning. The[lifecycle insight](https://posthog.com/manual/lifecycle)


exists exactly for this purpose.


Lifecycle insights categorize users based on whether they have completed any event within a specified period of time, then compare those results to the previous interval to tell you whether they’re falling dormant, returning, resurrecting, or active. Dormant users are the ones to watch out for, as they’re showing poor engagement and are at risk of churning — just click the relevant bar on the insight to get a list of them.


> 💡 **PostHog Tip:** Curious about the difference between returning and resurrecting users? Check[the lifecycle docs](https://posthog.com/manual/lifecycle#interpreting-your-lifecycle)
>
>
> for a full explanation.


##


4. Install apps to integrate PostHog with your stack


We like to think PostHog works brilliantly on its own, but to get the most out of the platform you may want to connect it with other software in your stack. We have a library of over 50 apps to help you do this, so you can connect PostHog with tools such as[Hubspot](https://posthog.com/apps/hubspot-connector)


or[Salesforce](https://posthog.com/apps/salesforce-connector)


.


Best of all, because PostHog is entirely open source, you can build your own apps to enhance your data even further — all you need is a little Javascript or Typescript knowledge and you’re good to go!


##


5. Build and share a customer success dashboard


Once you’ve built the insights you need, an essential step is adding them all to a PostHog dashboard so that you don’t need to check them one-by-one. Once you’ve pinned all your insights to a dashboard, you can easily check everything in one place and export it as an image if you need to include it in reports.


At PostHog, we’re not big fans of lengthy meetings and reports — instead, we’ve set up a subscription which sends the insights to a thread in a shared Slack channel every week. Don’t use Slack? You can subscribe by email too.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
