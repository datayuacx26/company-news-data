---
schema_version: "1.0.0"
document_id: "876a0ba4c1e4bc89aa3532ee285721ee7c0a700774528df17b180c8b919b957a"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1-26-0"
published_at: "2021-05-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:b311bf12f09a34e5f1959d3f32e3296876470d29752c68f8dc75e681eb457b0a"
---

# Array 1.26.0

# Array 1.26.0


May 13, 2021


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
-
-


PostHog 1.26.0 is here!


We've raised another $15M, our libraries have leveled up, and we've brought on even more great people. Not bad if you ask me.


###


[We've raised a $15M Series B](https://posthog.com/blog/15-million-series-b)


Some exciting news in PostHog land![Read our blog post about this latest round](https://posthog.com/blog/15-million-series-b)


.


###


Community MVP 🏆


This release cycle's Community MVP goes to...


[imhmdb](https://github.com/imhmdb)


!


Mohamad submitted a[500+ line PR](https://github.com/PostHog/posthog-php/pull/12)


implementing feature flags in our[PHP library](https://posthog.com/docs/integrate/server/php)


. The PR is still under review but we anticipate it'll be merged soon.


Awesome work and thanks again Mohamad!


> If you haven't seen it yet, we have a[new page dedicated to our contributors](https://posthog.com/contributors)
>
>
> . Every contributor gets their own digital card, and we provide a leaderboard with stats on each person's contributions. We also have a bot that sends a gift card for PostHog merch to contributors for every PR merged, and we welcome all types of contributions!


**In this release:**


- **New:** Feature flags for Node.js and Go
- **New:** CSV download for users in a datapoint
-


- **New:** Funnel trends for analyzing conversion over time


- **New:** Request retries for posthog-js
- **New:** New plugins for Redshift, PostgreSQL, and PagerDuty
- **Improvement:** New querying experience


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


##


PostHog 1.26.0 Release Notes


> If you're self-hosting and want to upgrade for a better experience and new features, remember to[update your PostHog instance](https://posthog.com/docs/runbook/upgrading-posthog)
>
>
> .


###


Feature flags for Node.js and Go


- [Node.js](https://github.com/PostHog/posthog-node/pull/29)


- [Go](https://github.com/PostHog/posthog-go/pull/2)


You requested and we delivered!


` posthog-node` and` posthog-go` now both support feature flags.[Ruby](https://github.com/PostHog/posthog-ruby/pull/6)


and[PHP](https://github.com/PostHog/posthog-php/pull/12)


are coming next.


We're making our libraries world-class, and this cycle also saw significant improvements to` posthog-python` ,` posthog-js` , and` posthog-flutter` . We now have a dedicated team responsible for our libraries, so expect development to speed up!


Thank you to everyone in the community for supporting us with feature requests and PRs.


###


[Funnel trends](https://github.com/PostHog/posthog/pull/4419)


Following a few refactors, funnel trends are now available in beta for Cloud and self-hosted[Scale](https://posthog.com/pricing)


users.


Funnel trends let you see how conversion in a funnel changes over time, as well as specify the time taken between steps for a conversion to be counted.


###


[CSV download for users in a datapoint](https://github.com/PostHog/posthog/pull/4175)


If you didn't already know, almost all datapoints in PostHog's 'Insights' section are clickable and reveal the users that make up that datapoint.


Well, now those users can be exported in CSV format, so you can use them in other tools or even create a static PostHog cohort from them.


Thanks a lot for building this[c3ho](https://github.com/c3ho)


!


###


[Request retries for posthog-js](https://github.com/PostHog/posthog-js/issues/199)


Continuing on the libraries theme, a much-requested feature is now live for` posthog-js` : retries!


Requests that fail because of for example, the client's network connection, will now be retried up to 10 times within an hour, making sure you miss as few events as possible. So if your user's internet goes down and comes back up, you'll still receive the events that happened when they were offline.


Also, Neil fixed a bug that sent requests to a wrong endpoint (with no impact on tracking). You can read about how Neil solved this issue on his[blog](https://neilkakkar.com/debugging-open-source.html)


.


###


New apps for Redshift, PostgreSQL, Salesforce, and PagerDuty


We've just released 4 new integrations with major platforms to enhance your PostHog experience.


Export data to Redshift, Postgres, and Salesforce, and leverage the PagerDuty app to get alerts when metrics in PostHog cross thresholds you specify.


###


New querying experience


We've redesigned and significantly improved the performance of the query builder in PostHog 'Trends'!


[Let us know what you think about it](https://app.posthog.com/home#supportModal)


.


###


[User Interviews](https://calendly.com/posthog-feedback)


We’re working hard to improve PostHog and would love to talk to you about your experience with the product.


If you're interested in helping us out, you can schedule a quick 30 min call with us[on Calendly](https://calendly.com/posthog-feedback)


.


To thank you for your time, we're giving away some awesome[PostHog merch](https://merch.posthog.com/)


!


##


PostHog News


We onboarded so many people this cycle that we decided to make a table for it!


###


New joiners


Name Role 🍍 on 🍕? Interesting Fact


Joe Product Marketer 😍 *"Jobs I had before I moved into marketing include (in order): clown, waiter, hospital & morgue cleaner, chainsaw salesman, and videogame journalist."*


Marcus VP of Product 🤮 *"Jointly hold the world record for Most People Dressed As Santa Doing A Zip Line In One Hour."*


Alex Fullstack Developer 😍 *"I've visited all of NYC's secret subway stations without getting caught (yet)."*


Phil Developer Relations 😍 *"I made national press coverage (The Times, The Sun, The Mirror, Radio 5 Live... The Gainsborough Today :smirk:) in 2006 after creating a David Beckham website called 'Bring Back Beckham.'"*


##


Community Shoutouts


Big thanks to the following members of our community who have contributed to PostHog over this release cycle:


- [imhmdb](https://github.com/imhmdb)


🏆
- [tobiastornros](https://github.com/tobiastornros)


- [adrienbrault](https://github.com/adrienbrault)


- [sankalpdomore](https://github.com/sankalpdomore)


- [gesposito](https://github.com/gesposito)


- [taobojlen](https://github.com/taobojlen)


- [pietrodevpiccini](https://github.com/pietrodevpiccini)


- [weyert](https://github.com/weyert)


- [lharress](https://github.com/lharress)


- [bard](https://github.com/bard)


- [avorio](https://github.com/avorio)


- [DimitrisMazarakis](https://github.com/DimitrisMazarakis)


- [abhijitghate](https://github.com/abhijitghate)


- [c3ho](https://github.com/c3ho)


- [joesaunderson](https://github.com/joesaunderson)


- [mands](https://github.com/mands)


- [Rajakavitha1](https://github.com/Rajakavitha1)


- [lutangar](https://github.com/lutangar)


- [jonataslaw](https://github.com/jonataslaw)


##


Open Roles


If you'd love to help us build PostHog, we're currently hiring for the following roles:


- Front End Developer
- Senior Full Stack Engineer
- Staff Engineer - Fullstack
- Any role you think you'd be a great fit for


Check out our[Careers page](https://posthog.com/careers)


for more info.


##


Share your feedback


We'd love to hear anything you have to say about PostHog, good or bad. As a thank you, we'll share some awesome[PostHog merch](https://merch.posthog.com/)


.


Want to get involved?Email us to schedule a 30 minute call


with one of our teams to help us make PostHog even better!


##


Bug Fixes and Performance Improvements


In addition to the highlights listed above, we also merged a bunch of PRs improving PostHog's performance and fixing bugs. Here are some highlights:


- [PostHog/posthog](https://github.com/PostHog/posthog/commits/master)


- [PostHog/plugin-server](https://github.com/PostHog/plugin-server/commits/master)


- [PostHog/posthog-python](https://github.com/PostHog/posthog-python/commits/master)


- [PostHog/posthog-cloud](https://github.com/PostHog/posthog-cloud/commits/master)


- [PostHog/posthog-js](https://github.com/PostHog/posthog-js/commits/master)


- [PostHog/plugin-repository](https://github.com/PostHog/plugin-repository/commits/master)


---


*Enjoyed this? Subscribe to our[newsletter](https://newsletter.posthog.com/subscribe) to hear more from us twice a month!*


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
