---
schema_version: "1.0.0"
document_id: "4fb652c72b87f8856fee0b2c6348955b0a1d99ba88cc9290237fe2e75308d970"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1-3-0"
published_at: "2020-04-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:0a740ba943f0ec9ca411b54ad6fa445d25850530ef0a073c4d3d0971e81041c7"
---

# Array 1.3.0

# Array 1.3.0


Apr 29, 2020


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
-


Another shiny new integration - PostHog now plays nicely with Android!


Like what you see and self-hosting?[Update](https://posthog.com/docs/runbook/upgrading-posthog)


your instance.


##


Release notes


###


[PostHog Android Library](https://posthog.com/docs/integrate/client/android)


In addition to the work Marius put into the iOS integration, this week you can also capture events in your Android app and send them to PostHog. Like the iOS library you can automatically captures screen changes, and you can capture events as per usual


Check out our[Android docs](https://posthog.com/docs/integrate/client/android)


on how to install it on your app.


###


[Gatsby App](https://posthog.com/docs/libraries/gatsby)


This week also sees the first community contributed app – thank you[Ritesh Kadmawala](https://github.com/kgritesh/gatsby-plugin-posthog-analytics/)


.


It’s now very easy in Gatsby’s words to “make your blazing fast site even more awesome.”


###


[URL Wildcards](https://github.com/PostHog/posthog/pull/653)


A user came to Tim with a problem, due to the nature of their site pages could be the same but have different url/paths – wildcards felt like an easy way of overcoming this so Tim merged this pr.


You can now use % as a wildcard when setting up an action.


###


[Further updates to Trends design](https://github.com/PostHog/posthog/pull/648)


As you’ll see below we have continued with our AntD implementation but this comes with also considering how to improve the Trends page as we now[default to Trends](https://github.com/PostHog/posthog/pull/656)


when you login.


Moving the bar to the left allows users to easily engage with the Trends graphs as they filter by Actions and Events.


###


[Trends Hints](https://github.com/PostHog/posthog/pull/632)


Eric mentioned when we added the stickiness explanation in Trends it was one of the most transformative updates in that release as he had to check the docs to remind himself of what he was measuring.


After a customer interview Aaron and Tim realized this was true for trends in general so we added Trends hints.


###


[Sort events table by timestamp](https://github.com/PostHog/posthog/pull/626)


This is another PR this week that we have the community to thank for,[solnsubuga](https://github.com/solnsubuga)


felt that clicking the table header for the timestamp should sort the events in reverse order, and we agreed.


##


Performance updates


- [Eric](https://github.com/eLRuLL)


added a[strict flake8](https://github.com/PostHog/posthog/pull/660)


setup as well as improvements, laying the ground work for code linting and likely more prs regarding darker.
- [Upgraded](https://github.com/PostHog/posthog/pull/663)


Kea to 2.0.0-beta.5.
- We continue to implement AntD as above in[Setup](https://github.com/PostHog/posthog/pull/621)


.


##


Share your feedback


We'd love to hear anything you have to say about PostHog, good or bad. As a thank you, we'll share some awesome[PostHog merch](https://merch.posthog.com/)


.


Want to get involved?Email us to schedule a 30 minute call


with one of our teams to help us make PostHog even better!


##


Favorite issue


###


[Copy to clipboard feature for scripts/docs #661](https://github.com/PostHog/posthog/issues/661)


Thank you[sac0](https://github.com/sac0)


for opening this issue then after a quick discussion with Tim[closed it](https://github.com/PostHog/posthog/pull/662)


.


Great to see updates built on the work we have done so far that ensures PostHog is better to use for the entire community.


##


PR of the week


[Flake8 setup and improvements](https://github.com/PostHog/posthog/pull/660)


What makes it the PR of the week is the discussion it started in our repo, it was awesome to see an active discussion on how to make it easier to contribute, potential usage of[black](https://github.com/psf/black)


and[darker](https://github.com/akaihola/darker)


for code formatting and a resolution that improves the PostHog project.


##


Repo round up


- [Qrcp](https://github.com/claudiodangelis/qrcp)


(Transfer files over wifi from your computer to your mobile device by scanning a QR code without leaving the terminal.)
- [Uptoc](https://github.com/saltbo/uptoc)


(A cli tool for deploying files to the cloud storage.)
- [Gitland](https://github.com/programical/gitland)


(A multiplayer game controlled using GitHub.)


##


PostHog news


We were all pretty excited at PostHog when we hit 2k stars on GitHub, we know it’s a vanity metric but we are pleased that we have started to build the foundations of a strong community thank you all!


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
