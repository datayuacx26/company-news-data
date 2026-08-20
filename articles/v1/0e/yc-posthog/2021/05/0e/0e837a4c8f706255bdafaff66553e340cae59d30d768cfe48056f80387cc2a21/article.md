---
schema_version: "1.0.0"
document_id: "0e837a4c8f706255bdafaff66553e340cae59d30d768cfe48056f80387cc2a21"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/100-times-more-events"
published_at: "2021-05-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:0e3bca80e657bd66a416f564ccf6c133ff7b5656e6c3acb4a175778b943aa785"
---

# Why we're giving away 100 times more cloud usage, free

# Why we're giving away 100 times more cloud usage, free


- [James Hawkins](https://posthog.com/community/profiles/27732)


May 17, 2021


- [Product updates](https://posthog.com/blog/product-updates)


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


Today, we're announcing that we're giving away 1 million ingested events/month on[Cloud](https://app.posthog.com/signup)


, for free. That's up from 10k/month that we were offering before.


##


More free stuff?! You're already pretty generous!


Gee, thanks subtitle!


The magic of an[open core business model](https://www.youtube.com/watch?v=L1Ovbzs7vyo)


is that giving away stuff for free often *helps* us make money.


Behold the virtuous circle:


1. Create a valuable free product and a community around it. Collaborate with the community. The free product gets more valuable and more popular as a result. The community grows faster and faster.
2. Larger companies using the free product think the paid product looks valuable. They email asking to buy[PostHog Scale](https://posthog.com/pricing)


or they increase their usage to 10s or 100s of millions of events/month.
3. Get money from paid product, to get[even](https://posthog.com/blog/raising-3m-for-os)


[more](https://posthog.com/blog/posthog-announces-9-million-dollar-series-A)


money from venture capitalists, go to step 1.


##


Who we made this change for


We noticed in user interviews that a subset of our user base were self-hosting product analytics *purely* for cost reasons. Typically:


- Small startups/hobby projects/charities
- Up to a few thousand users monthly
- Very little budget for product analytics or hosting product analytics
- No need to self-host product analytics for privacy reasons


If the above is you, you can of course keep going with PostHog Open Source.


However, for most users that fit the above criteria, we'd recommend[Cloud](https://app.posthog.com/signup)


because:


- No need to run updates
- No hosting costs at all
- You're likely to get an experience with fewer bugs if your usage is a little bit higher/grows


##


What's the catch?


If we're doing our jobs right, *your* (!) product may get really popular. Oh no!


We sell to you when you get to this "champagne problem". Word of caution: it qualifies as humble bragging if you come to us because you're getting too popular ;)


When your product is successful, you'll usually have more event volume, and a larger team - you will need features around collaboration or even compliance if you become a big enterprise one day. At that point, you'll want[PostHog Scale](https://posthog.com/pricing)


.


Serious sidenote: we don't lock you in - we're working on making our Open-Source product much more scalable, so you can at least do the basic analytics even with very high usage, without spending anything. That'll come later in the year.


##


Card needed, why?


We require a card to unlock the full 1m events/month.


It costs quite a bit to provide this free usage to people, so this felt fair to us. A little friction here ensures people place some value on product analytics so we don't end up with very poorly engaged users - this would create a lot of noise for us in judging how our own retention is performing, which makes it hard to judge what to build. Some users will go past 1m events/month in which case we don't want extra friction in our own sales process, although really for PostHog - we focus on PostHog Scale to drive revenue.


##


How easy is it to move from one edition of PostHog to another?


###


Current open source user -> PostHog Cloud?


If you are currently hosting PostHog Open Source and having read this would rather use PostHog Cloud for greater simplicity and enhanced reliability, then it may make sense to move. First of all, check above if you are one of the users we are targeting with this change.


We *haven't* built a way to automatically migrate your data over - that would have been non-trivial to build, and we didn't want to bottleneck releasing this change. However, after talking with many of you - our recommended way to migrate is this:


- [Set up](https://app.posthog.com/signup)


PostHog Cloud now
- Send events to[PostHog Cloud](https://app.posthog.com/signup)


and your self-hosted instance in parallel
- Switch off your self-hosted instance once you've enough data built up to be useful in Cloud (around 3 months is what most people do)


###


PostHog Cloud -> PostHog Open Source in future


In future, you may want to self-host your analytics again as your product takes off - when increased volume and privacy needs both drive you to want to self host.


We don't have plans in the immediate future to build a feature for this as we've other urgent items to get built first that will benefit more users. You could use our APIs, but in reality - we'd recommend running both systems in parallel for a couple of months then switching one off.


The exception is if you wish to move to[PostHog Scale](https://posthog.com/pricing)


(paid), in which case we provide extra support to migrate your data.


###


PostHog Cloud -> PostHog Scale


You can do this - we provide a little manual support in this case. Just[contact us](https://posthog.com/talk-to-a-human)


.


##


Does this mean PostHog is focusing more on cloud then?


Oh subtitle, you couldn't be more wrong.


A key goal is to provide a more reliable PostHog for *all* users.


We view[Cloud](https://app.posthog.com/signup)


as a way of generating awareness (charging high volume cloud users to cover its hosting costs) and improving scalability, and Self-Host as a way of generating high volume users that we retain very well and who may end up paying one day.


For those that self-host, we are putting a ton of engineering work into enabling ClickHouse in our open source product. That means users that self-host will be able to use PostHog at much greater scale, bug free, for free or paid depending on which features they need.


For teams with smaller volume and less of a focus on user privacy, this is where today's change comes in.


##


Existing paying Cloud customer? We've got your back


Existing customers should get treated just as well (or better, from time to time!) than new customers.


We've therefore already applied this increase to any customers who are paying per event on[Cloud](https://app.posthog.com/signup)


. Yep, that cost us a bunch of money. But it's the right thing to do.


After all, PostHog's mission is to increase the number of successful products in the world.


> PostHog is an open source developer platform that helps people build successful products. We help you debug and ship your product faster.


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
