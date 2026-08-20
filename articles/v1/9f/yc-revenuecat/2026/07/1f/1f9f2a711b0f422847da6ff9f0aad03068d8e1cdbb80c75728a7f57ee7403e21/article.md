---
schema_version: "1.0.0"
document_id: "1f9f2a711b0f422847da6ff9f0aad03068d8e1cdbb80c75728a7f57ee7403e21"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/growth/minimum-ios-version"
published_at: "2026-07-14T08:53:06+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:f1de835dac3c1c22e36b0a95bafd20162b782ef1cfa53057ba551cfe85b1231d"
---

# Almost everyone who pays for subscriptions is already on iOS 26

Right before WWDC, Apple published its final adoption numbers for the iOS 26 cycle:[79% of all iPhones, and 86% of iPhones introduced in the last four years, are running iOS 26](https://www.macrumors.com/2026/06/09/ios-26-adoption-stats-wwdc/) .


Those numbers convinced me to[post a hunch on Twitter](https://x.com/drbarnard/status/2064817113580363856) : for most subscription apps, the share of paying users on iOS 26 is probably more like 95%. And if that’s anywhere close to true, it’s time to start thinking about iOS 26 as the minimum requirement. Adding new features faster could easily pay for the small drop in paying users.


App industry folks pushed back, added data, and sharpened the argument in ways I didn’t expect. So here’s the fuller case: when cutting older iOS versions makes business sense, when it doesn’t, and how to run the numbers on your own app.


## What the data says about who actually pays


I made up the 95% number. Baran Toppare, a data scientist here at RevenueCat, came back with the receipts.


> late to the party but I had a look at our data for paid subscribers who opened the app during the last 90 days and iOS26 share is ~88%. iOS18 is not that small, still worth supporting for most folks[pic.twitter.com/f9h71aORgz](https://t.co/f9h71aORgz)
>
>
> — Baran Toppare 🧟‍♂️ (@toppare)[June 11, 2026](https://x.com/toppare/status/2065160596132184231?ref_src=twsrc%5Etfw)


Across apps on RevenueCat, about 88% of paid subscribers who opened an app in the last 90 days are on iOS 26. iOS 18 still accounts for 9.4% of subscribers and 9.6% of revenue. New transactions tell the same story: 85% happen on iOS 26.


So, not 95%. But that platform-wide average includes massive apps like ChatGPT that pull it toward the broadest possible audience. Your app probably isn’t ChatGPT. When I checked Weather Up (my side-project weather app), roughly 94% of paying users active in the past 90 days were on iOS 26.


## The trade you’re actually making


Raising your minimum OS costs you two things. Existing users on older versions keep the app, but frozen at the last compatible update. And new users on older versions can’t download it at all. That second one is the real cost, because it compounds: every new customer you can’t acquire is gone for good.


David Smith wrote[the best conservative case](https://www.david-smith.org/blog/2025/06/27/requiring-26/) I’ve read. Nine months into the iOS 18 cycle, requiring the latest OS would have cut about 9% of Widgetsmith’s new downloads. As he put it: “If I could do something which boosted my downloads by 9% I’d be delighted”. His threshold: wait until older versions fall to about 1% of new downloads.


That’s a defensible line. Mine is more aggressive, and 2026 is a big part of why:


- Since April 28,[every App Store submission must be built with the iOS 26 SDK](https://developer.apple.com/news/?id=ueeok6yw) . Your app already renders Liquid Glass for iOS 26 users, so supporting iOS 18 means shipping and QA-ing two design systems in one binary.
- At WWDC26, Apple confirmed[iOS 27 runs on the exact same devices as iOS 26](https://www.macrumors.com/2026/06/08/ios-27-supports-iphone-11-newer/) (iPhone 11 and newer). Requiring iOS 26 doesn’t strand anyone whose hardware could go further. Everyone excluded is on a 2018-or-older iPhone.
- The cost shrinks every month. iOS 18 was down to[about 10% of active devices by the end of June](https://telemetrydeck.com/survey/apple/iOS/majorSystemVersions/) , per TelemetryDeck. An iOS 26 minimum costs you a bit today, less in the fall, and almost nothing by next spring.


## Why I required iOS 26 for Grit Method


My latest side project app,[Grit Method](https://gritmethod.app/) , requires iOS 26. The UI leans heavily on Liquid Glass, and I didn’t want to check every animation and UI element against older simulators, then invent workarounds for a shrinking audience. One code path, the latest best practices, no backward-compatibility debt on day one.


There’s also a more practical problem: I no longer own a device running iOS 18. My whole family upgraded in the fall. Supporting an OS version you can only test in a simulator is its own quiet risk, especially when the people on it are paying you.


To be clear about my bias: I’m an indie moving fast, and the tradeoff is much smaller for me. Moving faster is worth more to my business than the single-digit percentage of paying users I’m giving up. The bigger your app, the more carefully you should run this math.


## When you shouldn’t cut older versions


Some apps shouldn’t take this trade:


- **Apps where the audience skews toward older devices.** Kids apps are the standout, since parents hand down old phones and buy used ones. Games with older demographics fit here too. As one reply on the thread put it, the Candy Crush crowd on iPhone 14s is still a market worth acquiring.
- **Freemium apps that need reach.** Free users skew toward older OS versions. If your model depends on a large free tier feeding a small paid one, cutting old versions taxes the top of your funnel hardest.
- **Apps people need, not just want.** If your app touches health, safety, or money, “they can buy a newer phone” isn’t an acceptable answer for the people who can’t.


The biggest companies in the world land all over this spectrum, and the split tracks business model.[WhatsApp still supports iOS 15.1](https://faq.whatsapp.com/1150261202542208) , a 2021 release, because reach is the product. Airbnb and Gmail already require iOS 18. Neither is wrong. They’re optimizing for different things.


One more nuance from the thread that stuck with me:


> After digging for a decade over dozens of apps for intent patterns, the clearest isnt OS version, age, traffic source etc but just the current price of the device. Valid across platforms too, with a tiny share of Android premium worth gold.
>
>
> — Thomasbcn (@Thomasbcn)[June 10, 2026](https://x.com/Thomasbcn/status/2064828064597491744?ref_src=twsrc%5Etfw)


OS version is a proxy for device age, and device age is a proxy for purchase intent.


## The best counterargument


> I just gracefully degrade the experience if I use newer APIs, but I suppose it highly depends on the type of app you're building. For the ones I do, I support all the way back to iOS 16, and it's basically no work for me.
>
>
> — Dan Loewenherz (@dwlz)[June 12, 2026](https://x.com/dwlz/status/2065237333549478028?ref_src=twsrc%5Etfw)


> And I don't really care about testing on a real device since the absolute numbers are so small. I'm doing a service to the world by supporting it at all. In my opinion, it's better to have it be available (and maybe buggy) than not available at all.
>
>
> — Dan Loewenherz (@dwlz)[June 12, 2026](https://x.com/dwlz/status/2065237587535532306?ref_src=twsrc%5Etfw)


Dan’s right that for some apps, maintaining backward compatibility is cheap. AI coding tools have made the #available dance easier than it used to be. If old-version support costs you almost nothing, keep it.


But “almost nothing” is doing a lot of work in that sentence. Two design systems isn’t nothing. An untestable device matrix isn’t nothing. And Dan’s own framing (better available and maybe buggy than not available) is a harder sell when the person hitting the bug is a paying subscriber.


## How to run your own numbers


Don’t inherit last year’s minimum by default, and don’t inherit mine. Check four things:


**1. New users by OS version:** In RevenueCat, go to the Customers tab and click on “New audience”. Add a filter for “First seen” with the last 90 days. Click the “Export all” button and then have your favorite AI tool analyze the data.For Weather Up, 83% of new users in the past 90 days were on iOS 26 or newer.


**2. New purchases by OS version:** In RevenueCat, go to the Customers tab and select “New audience”. Add a filter for “First purchase date” with the last 90 days. Click the “Export all” button and then have your favorite AI tool analyze the data.For Weather Up, 85% of new purchases in the past 90 days were on iOS 26 or newer.


**3. Active subscribers by last seen OS version:** In RevenueCat, go to the Customers tab and select “Active subscribers”. Click the “Export all” button and then have your favorite AI tool analyze the data.For Weather Up, 94% of active subscribers are on iOS 26 or newer. Interestingly, 8.5% of active Weather Up subscribers are already on the iOS 27 beta.


**4. What the floor buys you.** If requiring iOS 26 doesn’t change what you can ship or how fast, there’s no trade to make. For a Liquid Glass-heavy UI or anything built on iOS 26-only frameworks, the velocity gain is real.


So, did the data change my mind about Weather Up? No. But this is a judgment call: potentially losing 15% of new purchases is a real cost, and more than I expected when I tweeted.


Two things tip the decision anyway. We’re deep into a 4.0 update that fully embraces Liquid Glass, and dragging that redesign through a second, older design system is exactly the tax this whole post is about.


The second: the past 90 days of data is somewhat skewed by traffic source. Most of Weather Up’s current traffic comes from App Store search and being featured, which reach the broadest (and least bleeding-edge) audience. When the 4.0 update launches alongside iOS 27 in the fall, press and launch attention should skew new downloads back toward early adopters, closer to the 94% of active subscribers. Your new-payer mix isn’t a fixed property of your app; it follows your traffic.


So pick your line on purpose, and treat it as a line, not a law. David Smith’s is 1% of new downloads. Mine is “under 10% of new paying users, if the velocity gain is real”, and Weather Up just showed you both halves of that sentence doing work. What’s not defensible is picking a minimum on vibes (or this post) without ever looking at your own data.


## Keep reading


[WWDC26: What’s new for subscription apps](https://www.revenuecat.com/blog/engineering/wwdc26-whats-new-for-apps/)
