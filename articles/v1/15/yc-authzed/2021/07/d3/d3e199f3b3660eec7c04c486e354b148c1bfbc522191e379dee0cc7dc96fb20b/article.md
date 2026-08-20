---
schema_version: "1.0.0"
document_id: "d3e199f3b3660eec7c04c486e354b148c1bfbc522191e379dee0cc7dc96fb20b"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/what-to-build"
published_at: "2021-07-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:2561816609bccd103d089c8201230ded6fc7f62d8f5cd8bc8acc1eef3790ee94"
---

# What should my startup build next?

Time, money, and resources are finite so you want to best utilize them as you build your product but how do you know what to build next? In this post, we’ll review a few techniques we use at Authzed to answer this question, using a product feature we’re currently exploring as an example. (Spoiler alert: we’ve initially named the feature[Authzed Connect](https://authzed.com/connect/) and want your feedback!)


First for some background, Authzed is a permissions database that enables applications to store, query, and validate application permissions all in one place. The fundamental building blocks of the product are in place: we handle the traditionally tricky parts of application permissions for our customers so that their engineers don’t have to reinvent the wheel. But are there ways to improve the dev experience around permissions? Yes, lots! But where to start? Here are a few techniques we are using to decide:


## Time to First "Wow!" (TTFW)


This is an informal but important metric we track anecdotally for our customers: how long before a customer tells us they’ve experienced a "wow" moment while using our product. The "wow" moment can be a profound realization about our product’s utility or a simple, unexpected convenience. During a recent retrospective meeting, we asked ourselves, "How can users start seeing their data relationships in Authzed sooner?" The current process requires reading API docs, configuring a client, and making a few write calls. While this is a common process of[getting started](https://docs.authzed.com/guides/first-app) for many services, it is an experience we feel could be improved for our TTFW.


Instead of going through our existing getting started process, what if a new user could import existing relationships from the web *immediately* after signing up? Applications often utilize external services that require custom data syncing via webhooks or other replication strategies. If we could provide integrations with these services built into Authzed, our users could access their external services’ data relationships without writing code and eventually query that data for their permissions decisions.


How would this affect TTFW? Our minds began to race with possibilities. A new user could connect to external services with the click of a few buttons, view relationships from those services in a UI, and also immediately incorporate those services’ object definitions into their permissions schema. Even existing customers could potentially have new "wow" moments by utilizing the same integrations.


*Improving your "time to first wow" often involves refining your product’s onboarding experience but can also be creating new potential wow moments for existing customers.*


## Scratch Your Own Itch


Since resources can be tight, fulfill an existing need as you explore. The benefit to scratching your own itch is that you are your own customer and therefore have inherent knowledge that can be used to tease out nuances about the potential feature. At the end of the exploration, if it’s determined that the feature isn’t needed, you’re left with something functional that meets an internal need.


For the ability to connect to external services (which we’ll refer to as[Authzed Connect](https://authzed.com/connect/) from here), we identified our existing Stripe usage as a useful place to test our ideas. Because we use Stripe to manage subscriptions and billing, we built a proof of concept that syncs our Stripe subscription data into our own permissions system and have been testing the integration.


This approach also aligns with 2 values we hold:[dogfooding](https://en.wikipedia.org/wiki/Eating_your_own_dog_food) and "building knowledge by doing". As we built the proof of concept, we’ve identified the things we’ll need to improve and as we continue to dog food the integration, we hope to experience any pain points that might exist and have the chance to address them before any of our customers do.


*Can you identify an internal need that can be used as a proving ground for the new feature?*


## Get Outside Feedback


While scratching your own itch is a good way to start, getting outside feedback is absolutely critical to accurately assessing your idea. Common pitfalls of *not* getting outside feedback are:


- Overestimating how many customers have the same problem.
- Overestimating how well you understand the problem.
- Overestimating the actual usefulness of your idea.
- Building something no one else actually wants.


Get feedback from your current customers as well as potential customers and as you’re listening to the feedback, look for the issues that are repeat offenders. Also, gathering good feedback from these audiences may not be as easy as it seems. We’re big fans of[The Mom Test](https://www.amazon.com/Mom-Test-customers-business-everyone/dp/1492180742) and it includes detailed strategies for how to elicit good feedback.


For Authzed Connect, we’ve interviewed some current customers using a script we designed using principles from The Mom Test. We’ve also launched an[interactive feedback page](https://authzed.com/connect) that introduces Authzed Connect and provides potential customers a way to signal which integrations would be most useful. This will serve as an input into our decision for what to build.


*Get outside feedback and don’t overlook the importance of good questions.*


## What’s Next?


By now, you’ve found a potential feature to build considering TTFW, gathered insights through building something useful, and received inputs through feedback from outside sources. So what to do next?


This is the exciting part and an exercise left to you. Define what success is for your idea and continue building! There are numerous product design and prioritization frameworks companies have used successfully. See Amazon’s[Working Backwards](https://www.quora.com/What-is-Amazons-approach-to-product-development-and-product-management) , Intercom’s[RICE](https://www.intercom.com/blog/rice-simple-prioritization-for-product-managers/) , and Gusto’s[Apples and Oranges](https://gusto.com/blog/growth/product-manager-prioritization-framework) to get started.


If this has been helpful in your own process of figuring out what to build next, let us know. And check out[Authzed Connect](https://authzed.com/connect/) to learn more about how we’re currently thinking of improving application permissions.


On this page


- Time to First "Wow!" (TTFW)
- Scratch Your Own Itch
- Get Outside Feedback
- What’s Next?


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)
