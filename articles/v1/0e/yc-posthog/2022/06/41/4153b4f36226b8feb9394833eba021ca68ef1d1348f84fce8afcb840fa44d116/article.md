---
schema_version: "1.0.0"
document_id: "4153b4f36226b8feb9394833eba021ca68ef1d1348f84fce8afcb840fa44d116"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/calculating-events-from-users"
published_at: "2022-06-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:e08acffd280ef0e1440eb29d4fea68aef516d2b2917f4b81873d78e4c79dd3df"
---

# The two ways to estimate your monthly event usage

# The two ways to estimate your monthly event usage


- [Simon Fisher](https://posthog.com/community/profiles/28895)


Jun 21, 2022


- [Guides](https://posthog.com/blog/guides)


#### Contents


-
-
-
-
-


When talking through our editions and pricing options with potential customers I'm often asked "How can I estimate my event count?"


If you're not already using an analytics tool, or it doesn't readily give you a count of tracked events, it can be quite tricky to figure out how much adopting an event-based platform like PostHog is going to cost you.


This guide explains why we price by event and offers some tips for estimating your usage.


##


Why we price based on events


Pricing software is hard. It's a delicate balance between being affordable for your[ideal customer profile](https://posthog.com/newsletter/ideal-customer-profile-framework)


, competitive in the marketplace, while still allowing you to grow and fund further development in the product.


We settled on event-based pricing for two main reasons:


1.


Events are the main thing we store in our Clickhouse database in PostHog Cloud. The more events we store, the higher our own infrastructure costs. It's only fair that we incorporate those running costs into our pricing.


2.


Product analytics is all about visualizing data about events which your users trigger. The more events available to you for analysis, the richer that analysis will be.


Although point one is mainly focused on our own Cloud, we wanted pricing parity between our Cloud and self-hosted editions so that it was easy to migrate between the two. Hence event-based pricing for all editions.


##


Ok, that makes sense, but how do I calculate my event count?


There are two ways to estimate your event count. One takes a bit of time but will give you a very accurate estimate; the second is much quicker but is more of a guestimate.


###


1. Start sending event data to PostHog Cloud


The most accurate way to figure out your event count is to take advantage of our[1 million event per month free tier](https://posthog.com/pricing)


on PostHog Cloud.


Simply use one of our[libraries](https://posthog.com/docs/integrate#libraries)


to send your event data to PostHog ([autocapture](https://posthog.com/docs/integrate/ingest-live-data#use-autocapture)


is easiest) and check your event usage on the[Billing](https://app.posthog.com/organization/billing)


page in the app.


Once you've sent a typical week's worth of data then you can do some multiplication to project your monthly event count.


If you get close to the 1 million event limit then you can stop sending events and project forward based on how many days worth of data has already been captured.


###


2. Estimate based on MAU and your product category


Most people who come to us not knowing their event count will have a handle on their monthly active user (MAU) number.


This can be a useful starting point, but user interaction patterns vary by type of product, industry and target persona:


-


For a banking app I might log in, check my balance, look at a few offers and then log out, generating a few events


-


For a social media app I might log in, check what my friends had for dinner, watch endless videos of cats jumping off things, find an appropriate GIF to send to my cousin for her birthday, all generating hundreds of events


-


For an infrastructure monitoring product I could be checking it in the morning and then only visiting the app if I'm alerted to a problem, generating events sporadically


Event counts also vary based upon whether you are using[autocapture](https://posthog.com/docs/integrate/ingest-live-data#use-autocapture)


,[custom capture](https://posthog.com/docs/integrate/ingest-live-data#capture-user-events)


or a combination of both.


As autocapture generates events for every pageview and click, it can start to get quite noisy, however there are things that can be done to limit that.


##


Example event counts from PostHog users


For a little more context, we took a look at PostHog Cloud customers to get a better understanding of how event counts map to MAUs.


As a rule of thumb, most fell within the range of 50-100 tracked events per MAU per month.


We then did a deeper analysis of the different types of customers and came up with the following list of product types and expected monthly events per MAU.


**Product** **B2B / B2C** **Monthly events per MAU** **Autocapture** **Platforms**


PostHog B2B 87 ✔


Web


Financial reporting B2B 44 ✖


Web


Cloud monitoring B2B 22 ✖


Web


Document management B2B 54 ✔


Web


Speech to text API B2B 583 ✖


API


Crypto wallet B2C 162 ✖


Browser extension


Meditation app B2C 118 ✖


Android, iOS


Fashion retail B2C 31 ✔


Web


Event booking B2C 8 ✖


React Native


Restaurant booking B2B2C 54 ✔


Web, Mobile


As you can see, event counts vary wildly across different types of products, but this should help you get closer to an estimated event count based on your product and MAU count.


Once you've got this figure you can visit the[pricing](https://posthog.com/pricing)


page and calculate your estimated costs for adopting PostHog.


And don't forget, PostHog Cloud and Scale are free for up to 1 million tracked events per month.


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
