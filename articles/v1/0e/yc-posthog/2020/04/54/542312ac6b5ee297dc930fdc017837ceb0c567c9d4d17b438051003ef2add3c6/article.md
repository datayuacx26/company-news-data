---
schema_version: "1.0.0"
document_id: "542312ac6b5ee297dc930fdc017837ceb0c567c9d4d17b438051003ef2add3c6"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1-2-0"
published_at: "2020-04-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:17aec82fda8eec1e9357d29cdac5ccac1d0a5ad37f9f8def33c827351390aa51"
---

# Array 1.2.0

# Array 1.2.0


Apr 22, 2020


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


Big new integration - PostHog now has a library for iOS!


Like what you see and self-hosting?[Update](https://posthog.com/docs/runbook/upgrading-posthog)


your instance.


##


Release notes


###


[PostHog iOS Library](https://posthog.com/docs/integrate/client/ios)


You can now capture events in your iOS app and send them to PostHog. It automatically captures screen changes, and you can capture events as per usual


Marius has been working on a lot of our libraries (starting with[Ruby](https://github.com/PostHog/posthog-ruby)


) but we knew that our mobile support was a big gap.


See our[iOS docs](https://posthog.com/docs/integrate/client/ios)


for how to install it on your app.


###


[Session Information](https://github.com/PostHog/posthog/pull/586)


You can now see exactly how much time people are spending on your app using sessions. There’s two modes: “Average session length”, which shows you how long sessions are and how many, and “distribution” which makes it super easy to spot whether sessions are uniformly distributed or whether there are outliers.


So far engagement metrics have focused on repeated actions or the volume of page views – we still think Stickiness, Active users are very valuable but this adds another dimension.


###


[Filtering funnels by properties](https://github.com/PostHog/posthog/pull/628)


In addition to the[changes](https://github.com/PostHog/posthog/pull/506)


last[week](https://github.com/PostHog/posthog/pull/561)


on funnels so that they work like /trends you can also apply properties to your funnels to narrow down conversion metrics by anything you like.


##


Performance updates


- [Added indexes](https://github.com/PostHog/posthog/pull/630)


so loading /trends is super fast, even with millions of events.
- We have[offloaded storing events](https://github.com/PostHog/posthog/pull/615)


to workers, so that calls to our events API are non-blocking, and you can scale insertion of events independently from the rest of PostHog.
- Removed drf-yasg in favor of[our own hosted docs](https://github.com/PostHog/posthog/pull/596)


.
- As part of our[design push](https://github.com/PostHog/posthog/pull/619)


Eric got the next branch out with the Ant design layout tweaking some of those aspects.


##


Favorite issue


###


[Running locally with docker](https://github.com/PostHog/posthog/issues/636)


We had instructions ready to run PostHog locally but had not prioritized doing local development with Docker.


Thank you[Viperfx](https://github.com/viperfx)


for requesting this.


We are very keen to see enhancements that are not part of our parity project, please keep them coming.


##


PR of the week


###


[Removing drf-yasg in favor of new docs website](https://github.com/PostHog/posthog/pull/596)


Thank you to[SanketDG](https://github.com/sanketdg)


for another pr that has helped ensure we fixed an[issue](https://github.com/PostHog/posthog/issues/574)


raised by another user[maximmarakov](https://github.com/maximmarakov)


, it’s great to see the community fix ad hoc issues especially ones that might not directly be related to new features but ensuring our docs and instructions are up to date for other users.


##


Share your feedback


We'd love to hear anything you have to say about PostHog, good or bad. As a thank you, we'll share some awesome[PostHog merch](https://merch.posthog.com/)


.


Want to get involved?Email us to schedule a 30 minute call


with one of our teams to help us make PostHog even better!


##


Repo round up


- [98.css](https://github.com/jdan/98.css)


(Whilst looking at a new design system, there was one vote to go super old school on the UI)
- [Avatarify](https://github.com/alievk/avatarify)


(More fun to be had with video call being the norm, photorealistic avatars for Skype and Zoom)
- [Teonite T-shirts](https://github.com/teonite/t-shirts)


(Aaron has been creating PostHog T-shirts this week and was intrigued by an Open Source T-shirt project – although the PostHog design is very different)


##


PostHog news


PostHog gets bigger every week! Not just in terms of new users and feature updates – we’re also excited to have[Marius](https://twitter.com/mariusandra)


join – he was one of the first contributors but has done a stunning amount in a short time.


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
