---
schema_version: "1.0.0"
document_id: "67c327371f877573df24a86e5bcd2c836c23f35d3c7ec7a712d9cba8e4e8bf5a"
company_key: "yc-sidekit"
company: "SideKit"
source_id: "yc-sidekit-news-import-e36cf5c25e24"
canonical_url: "https://appsidekit.com/blog/why-does-sidekit-exist"
published_at: "2025-01-17T00:00:00+00:00"
first_seen_at: "2026-07-22T13:24:11.973593+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:17d331aae1a479eafa2faabae937a24132a7bad6f5ac55de212542daa73f6a9e"
---

# Why Do We Exist?

## Why does SideKit exist?


It's now been two months of working on this idea every waking hour and this is a question that's been on my mind a lot. It's also a question I've been asked every time I describe what we're building to other founders.


I'm writing this article to organize my thoughts more than anything else, this isn't to argue our right to exist. If anything, it questions it.


First, the pitch: SideKit is an all-in-one package to run your mobile app in production, i.e. product analytics, feature flags, version management out of the box in one package.


If that still doesn't mean anything to you:


- product analytics: see how many users you have/what they're doing on your app.
- version management: nudge/force users to update when they open your app and see what usage is like across versions
- feature flags: roll out features to cohorts of users at a time and turn them off if you need to without pushing app updates


*Now for the existential questions:*


- Aren't there already a ton of analytics packages out there? Yes.
- Aren't there a ton of feature flag SaaS products out there? Yes.


## Analytics


As of this article, the main product we offer is analytics, so this is usually what most people ask me about. Why are you building analytics, why not just use X?


Analytics and I suppose observability as a whole is one of the most crowded categories of software out there and the incumbents are formidable. Google Analytics, PostHog etc. have pretty much built out any feature you can possibly imagine. And given the maturity of this category, Ethan and I cannot hope to match their feature set in the next few months even if we wanted to.


Building yet another analytics platform has absolutely no allure to me but analytics is a means to something greater that I think we have a shot at.


I also think that software observability has gone so far that it has forgotten why it came to exist in the first place.


The purpose of analytics is simple: understand your users. And I think in a race to add the most features and win as many enterprise customers as possible, analytics platforms have kind of lost this.


Do you really need to track users across sessions? Do you really need to track every click and every mouse movement? It feels like a lot of companies believe that the right way to go about this is to ingest petabytes of usage data for $XX millions of dollars and then pay $XX millions of dollars for software to help them make sense of it.


I genuinely and earnestly believe that simple usage-based analytics to track feature usage is all that 80% of apps need. And that's what we've built. But again, we aren't hoping to be the next big analytics platform, it's just a means to an end for us.


## The Origin


As with most of my projects, SideKit started out as a problem I wanted to solve for myself. As the proprietor of a mildly popular sudoku app I wanted a "not evil" analytics service that was dead simple, just lightweight usage based tracking for my very simple needs.


Building for myself has worked out pretty well in the past. Not Evil Sudoku started that way, most of my friends thought it was a stupid idea given the hundreds of existing sudoku apps but it turned out that almost 200,000 people shared my frustration with what was on the market. This is all despite the fact that the app doesn't even let you play more than one game a day. If it were fully free I'm sure it could've gone farther.


I ended up really enjoying building the little analytics service and it became one of my most used apps. So I started to think: what if we could just build a lot of tooling tangential to app development and provide it in a lightweight package? Something like Firebase but that was privacy-first out of the box with some straightforward 80% tools. In my mind something like PostHog is a 100% analytics tool, it takes care of 100% of analytics needs - if you desire to make sense of every mouse click it can do that for you. An 80% tool on the other hand like ours is one that's good enough for 80% of use cases. For analytics, that's understanding who your users are (OS, language, country, etc.) and what parts of your app they use.


However, the thing with B2B SaaS is that it's not enough to build for yourself. When you build for consumer, taste matters, you can carve a niche for yourself solely by differentiating on design and UX.


Perhaps there is no market for this. It's totally possible that most people are either at a scale where they require none of this or at a scale where they require some of those 100% tools that allow them to make micro-optimizations that produce tangible results at their scale.


## Something Greater


Throughout this article I've mentioned a greater purpose so let me elaborate by example.


Version nudges are something that comes out of the box with SideKit. You can let users know about updates and link them to the App Store. We let you do this with a forced gate that doesn't let them use the app until they update or a dismissible one. This lets you block arbitrary app versions by name or just set a minimum requirement for your app:


This is a nice feature to have and not difficult to build if you're a software engineer. We just provide a polished interface that works out of the box which you'd probably appreciate. Especially because this is one of those things you don't bother building until one day something breaks in production and you wish you could just move all your users off the bad version.


Analytics means that we can show you what the usage on that app version looked like in the last 7 days which is pretty nice to know before you decide to block a version.


There's a lot of tooling that I think would be amazing to have out of the box in a simple package that you could add without worrying about breaking GDPR or complicating your privacy policy. Something like a go-to you could just add before you release your app into the wild.


The idea is that with SideKit, instead of having a 100% analytics tool, a 100% feature flag tool, a 100% app configuration tool, you have one 80% tool that'll get you pretty far before you hit the scale where you need a 100% tool for any of these single things.


Time will tell if this thesis holds any water.
