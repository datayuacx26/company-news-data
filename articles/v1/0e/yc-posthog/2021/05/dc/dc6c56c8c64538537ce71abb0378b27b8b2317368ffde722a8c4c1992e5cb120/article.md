---
schema_version: "1.0.0"
document_id: "dc6c56c8c64538537ce71abb0378b27b8b2317368ffde722a8c4c1992e5cb120"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/the-posthog-array-1-25-0"
published_at: "2021-05-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:7b86bce998567e09e12183e3308f44f430c176698dd986590a18ca571159b512"
---

# Array 1.25.0

# Array 1.25.0


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


PostHog 1.25.0 is here!


Since the last release, we've now onboarded 6 new team members, all of them awesome.


For our users, what this means is:


```text
Awesome new team members == Better product
```


###


100x more, for free


We have increased our free volume on[PostHog Cloud](https://app.posthog.com/)


to 1 million events per month for free, instead of the previous 10k.


That means your next PostHog bill will be up to 225$/month cheaper!


It's important to us that you have enough room to determine if PostHog is the right fit for you, before committing to the platform.


This change is also retroactive, so existing PostHog users have already had this change applied to their accounts.


Enjoy!


###


Community MVP 🏆


This release cycle's Community MVP goes to...


[Rajakavitha1](https://github.com/Rajakavitha1)


!


Rajakavitha dug into our codebase and helped us document 2 new API endpoints, as well as submitting other suggestions and changes to our documentation pages.


Thanks a lot for all your help - hope you're enjoying your PostHog merch!


> If you haven't seen it yet, we have a[new page dedicated to our contributors](https://posthog.com/contributors)
>
>
> . Every contributor gets their own digital card, and we provide a leaderboard with stats on the contributions made by each contributor. We also have a bot that sends a gift card for PostHog merch to contributors for every PR merged, and we welcome all types of contributions!


**In this release:**


- **New:** App logs
- **New:** 'Trends' legends
- **New:** Lifecycle toggles
- **New:** Resizable table columns
- **New:** Job queues for apps
- **Improvement:** Fuzzy search for properties


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


##


PostHog 1.25.0 Release Notes


> If you're self-hosting and want to upgrade for a better experience and new features, remember to[update your PostHog instance](https://posthog.com/docs/runbook/upgrading-posthog)
>
>
> .


###


[Legends for charts in 'Trends'](https://github.com/PostHog/posthog/pull/3434)


This feature isn't new to all of you, because we've been testing it out with a[feature flag](https://posthog.com/manual/feature-flags)


. However, legends for charts in 'Trends' are now enabled for everyone!


With legends, you're able to determine with more clarity the different sections/lines you see on a graph, see the exact values for each datapoint, and disable sections with one click. You can find them under your graph in 'Trends'.


###


[App Logs](https://github.com/PostHog/posthog/pull/3482)


Apps are now able to use the JavaScript` console` API to specify errors that will be shown to users in the PostHog UI. This makes it easier to both debug your own plugins as a developer, and understand what's wrong about your configuration as a user.


###


[Lifecycle Toggles](https://github.com/PostHog/posthog/pull/3961)


Li joined us this cycle and started making an impact from day 1!


As a result of her work, you can now toggle different sections of lifecycle graphs on and off, in order to dig into the metrics that matter most to you.


This change also came with an addition of more in-product hints about the lifecycle functionality.


###


[Resizable Table Columns](https://github.com/PostHog/posthog/pull/3927)


Sam is another one of our new team members who's been smashing it from the moment he joined!


This cycle, in addition to picking up a variety of product fixes and improvements, he also shipped resizable columns for our tables, allowing you to easily get more details from an event, session, or feature flag without having to click on it.


###


[Job queues for apps](https://github.com/PostHog/plugin-server/pull/325)


Apps keep getting more and more powerful every new release, and this cycle was no exception.


App developers can now leverage job queues to implement a variety of asynchronous tasks, including retry mechanisms.


In addition, apps can now leverage have two more functions:` onEvent` and` onSnapshot` .


These are read-only functions that run on processed events and are particularly useful for export apps.` onSnapshot` handles session recording events while` onEvent` handles all other events.


For more information about this, check our[Building Your Own Plugin page](https://posthog.com/docs/plugins/build)


.


###


[Fuzzy search for properties](https://github.com/PostHog/posthog/pull/4091)


In addition to making significant changes to improve the experience of users with massive amounts of event names and properties, we have also implemented fuzzy search for properties.


This means that to find a property on a filter, you no longer have to type an exact subset of its name, as our search mechanism will still be able to identify what you mean even if you have a few typos or forgot the *exact* name of the property.


##


Share your feedback


We'd love to hear anything you have to say about PostHog, good or bad. As a thank you, we'll share some awesome[PostHog merch](https://merch.posthog.com/)


.


Want to get involved?Email us to schedule a 30 minute call


with one of our teams to help us make PostHog even better!


##


PostHog News


We onboarded so many people this cycle that it makes sense to make a table for it!


###


New joiners


Name Role 🍍 on 🍕? Interesting Fact


Buddy Fullstack Developer 🤮 *"I practice AcroYoga."*


Li Fullstack Developer 😍 *"I'm in the top 3% of all North American League of Legends players."*


Mo Content Marketer 😍 *"I was a radio presenter in a previous life. A producer walked past me at a career fair, heard me speak, asked me to follow her to the studio, and put a mic in front of me. The rest is history."*


Neil Fullstack Developer 🤮 *"There was a time I travelled using nothing but paper maps for directions. As a result, I developed excellent sense of direction and photographic memory for maps. Now I subconsciously try to figure out which way is North, wherever I am, so I can navigate using the map in my brain. Thanks to this I never get confused between taking an Eastbound or Westbound tube 😂"*


Sam Fullstack Developer 😍 *"In my apartment, I have a self-sustaining compost bin with worms inside. It keeps food scraps out of the landfill and makes excellent soil!"*


Tiina Fullstack Developer 😍 *"I spent a summer selling books door-to-door in Utah. I appreciate sales/customer service folks much more since then - their job is so hard."*


##


Community Shoutouts


Big thanks to the following members of our community who have contributed to PostHog over this release cycle:


- [Rajakavitha1](https://github.com/Rajakavitha1)


- [leggetter](https://github.com/leggetter)


- [gesposito](https://github.com/gesposito)


- [adrienbrault](https://github.com/adrienbrault)


- [sankalpdomore](https://github.com/sankalpdomore)


- [gagantrivedi](https://github.com/gagantrivedi)


- [thedeveloperr](https://github.com/thedeveloperr)


- [bard](https://github.com/bard)


- [swong194](https://github.com/swong194)


- [afterwind-io](https://github.com/afterwind-io)


- [kevinhu](https://github.com/kevinhu)


- [j-fuentes](https://github.com/j-fuentes)


- [daviddanielarch](https://github.com/daviddanielarch)


- [angelahuang89](https://github.com/angelahuang89)


- [akshayagarwal](https://github.com/akshayagarwal)


- [JeffreyQ](https://github.com/JeffreyQ)


##


Open Roles


If you'd love to help us build PostHog, we have a lot of openings available!


We're looking for:


- Customer Success Lead
- VP Customer Success
- Ex-Founders (Engineering)
- Front End Developer (Remote)
- Senior Full Stack Engineer
- Senior Full Stack Engineer - EMEA
- Senior Mobile Engineer
- Site Reliability Engineer
- Recruitment & Operations Manager
- Senior Product Designer
- Any role you think you'd be a great fit for


Check out our[Careers page](https://posthog.com/careers)


for more info.


##


Bug Fixes and Performance Improvements


In addition to the highlights listed above, we also merged a bunch of PRs improving PostHog's performance and fixing bugs. Here are some of the highlights:


- ` PostHog/posthog` :[228 PRs merged / commits](https://github.com/PostHog/posthog/commits/master)


- ` PostHog/plugin-server` :[66 PRs merged / commits](https://github.com/PostHog/plugin-server/commits/master)


- ` PostHog/posthog-python` :[13 PRs merged / commits](https://github.com/PostHog/posthog-python/commits/master)


- ` PostHog/posthog-cloud` :[7 PRs merged / commits](https://github.com/PostHog/posthog-cloud/commits/master)


- ` PostHog/posthog-js` :[7 PRs merged / commits](https://github.com/PostHog/posthog-js/commits/master)


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
