---
schema_version: "1.0.0"
document_id: "65c7a6fc414b1e742d9744e67b9d0be81a14c92b61953ecbec165b25b2047a5f"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1"
published_at: "2020-03-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:eaa4d3d0beab55ed4cdf53d8877e8aba5e6234973fa49efc250ed2873f704f5b"
---

# Array 1.0.0

# Array 1.0.0


Mar 09, 2020


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


This is the first of (what we hope are many) PostHog weekly roundup posts. We want to let the community know what we have been up to, a few of our favorite comments, issues, and pull requests as well as some key repos and projects we have noticed over the course of the week.


If you would like us to change the format or include something new, create an[issue](https://github.com/PostHog/posthog)


.


##


Release details


We’ve made a lot of exciting new features this week (to update, please see[https://github.com/PostHog/posthog/wiki/Upgrading-PostHog](https://github.com/PostHog/posthog/wiki/Upgrading-PostHog)


):


###


Filtering action trends graphs


This is a more intuitive way of filtering action trends. You can also look at both the total and DAUs with this update.


###


Exact/contains matching for URLs in actions


This does exactly what it says on the tin, it will allow you to be able to specify specific pages to track in your app/website.


###


Filtering paths by date


We got a lot of requests for this update – paths are super interesting but if you’re making changes to your website based on your findings you should be able to then refine your paths by date.


and lastly…


###


Graphs show numbers


We took way too long to realize this was much simpler than guessing the number on the Y axis.


We’ll try to ensure we don’t make such oversights again. In the meantime, at least our dashboards are easier to read.


##


Favorite issue


###


[Library Request: integrate with Elixir](https://github.com/PostHog/posthog/issues/227)


We’ve wanted to increase the number of integrations. This is why we’re highlighting this issue – having people request something helps us prioritize better.


[Integrations-wise](https://posthog.com/docs/integrate/overview)


, we already have JS, Node, Python, Ruby, PHP and Go – let us know if we’re missing something that you would like.


Thank you[victorbordo](https://github.com/victorbordo)


for raising it.


##


PR of the week


###


[Filter paths by timestamp #272](https://github.com/PostHog/posthog/pull/272)


Part of a larger[issue](https://github.com/PostHog/posthog/issues/223)


but hats off to[mariusandra](https://github.com/mariusandra)


for the excellent contribution which made it into our product update above.


##


Repo round up


These are things that we thought were cool in the last week:


- [Newscatcher](https://github.com/kotartemiy/newscatcher)


(we were lucky enough to be included in[Python Weekly](https://www.pythonweekly.com/)


and this caught our eye at the same time)
- [7 days golang apps from scratch](https://github.com/geektutu/7days-golang)


(No not Craig David’s terrible follow up to his 2000 hit, we came across this as we finished our Go integration)
- [openpilot](https://github.com/commaai/openpilot)


(having recently moved to SF we see loads more Cruise cars and autonomous training vehicles than we ever would in London – it’s pretty cool that this is an open source project)


##


PostHog news


- We wrote a blog about[moving to SF](https://posthog.com/blog/moving-to-sf)


. James was delighted it made it to the front page of HN.
- We also started a[Youtube channel](https://www.youtube.com/channel/UCn4mJ4kK5KVSvozJre645LA)


– we’re not going to be big time vloggers anytime soon but we did want to make it easier for our users to understand our features.


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
