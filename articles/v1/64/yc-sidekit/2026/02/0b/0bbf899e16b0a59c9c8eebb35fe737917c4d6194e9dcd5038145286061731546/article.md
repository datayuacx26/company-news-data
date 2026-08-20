---
schema_version: "1.0.0"
document_id: "0bbf899e16b0a59c9c8eebb35fe737917c4d6194e9dcd5038145286061731546"
company_key: "yc-sidekit"
company: "SideKit"
source_id: "yc-sidekit-news-import-e36cf5c25e24"
canonical_url: "https://appsidekit.com/blog/app-metrics-that-matter-solo-developers"
published_at: "2026-02-24T00:00:00+00:00"
first_seen_at: "2026-07-22T13:24:11.973593+00:00"
fetched_at: "2026-07-28T21:26:27.452652+00:00"
content_hash: "sha256:e9ed303dae59b090baa13ecfd12ae889c394cb1f7a4cd595a15a4fdefa7026e8"
---

# App Metrics That Matter for Solo Developers

# Metrics


Publishing an app can often feel like sending it into the void and App Store Connect isn't great at surfacing insights. Knowing where your users are and what parts of your app they actually care about is essential to growing your app and improving your reviews.


But, there are a ton of analytics SDKs out there with most of them targetting big companies with millions in revenue and an army of data scientists. For simple apps there are usually only a few questions you need answers to.


# The 3 questions every solo dev needs answered


## 1. Is anyone using my app?


It's insane that in 2026 this is still something that doesn't work out of the box on the App Store. Equally insane is how easy it is to set up. All you need is something that fires every time your app is opened. This is what analytics tools love to call an event or a signal. An app open signal like this answers a ton of questions instantly.


- Are people using my app?
- Is usage going up or down or the last X days/months?


If you end up posting your app to Reddit or somewhere else and want to know in real-time if that's actually leading to installs/usage, this is how you can get that.


### Now how do you set that up?


App Store Connect takes 24-48 hours to update with the day's analytics so it's out of the question. Instead you're going to need an analytics SDK and it's your lucky day because SideKit tracks this out of the box. Add[one line of code](https://docs.appsidekit.com/ios/getting-started/configure-sidekit) to your app and it's one of the default things we track along with new users. Set up an account and you should be able to see this in < 5 minutes, promise.


# 2. Can I drop that version?


There are 2 things that keep all app developers up at night:


- old app versions
- old operating system versions


## 2a. Raising your app's minimum OS requirement


Every year, development frameworks release exciting new features but the issue is that they only ever support the last two years of major OS versions. There's a constant battle around orphaning users on older devices/older operating systems and bringing the latest system features to your apps. Your analytics provider should have some dead simple way to show you this data at a glance. Generally it's safe to drop versions that account only for < 5% of usage.


This is what SideKit shows:


## 2b. Getting users off old app versions


Equally frustrating is supporting the long tail of app versions. Every update you push leaves some stragglers behind. You need a quick way to see what versions your users are on.


This is what SideKit shows:


P.S. SideKit also offers a way to nudge users to update out-of-the-box with[version gating](https://docs.appsidekit.com/ios/version-management/set-a-minimum-version) ;)


## 3. Are users using features?


Understanding what features your users actually use is vital to prioritizing bug fixes and deciding what to build. Most people suffer through with the App Store's built-in analytics and this is the thing that usually pushes them to finally integrate an analytics SDK.


On top of just helping you roadmap things, it's also really motivating to see people actually using the things you build. Prior to SideKit, I had a sudoku app called Not Evil Sudoku and seeing over 1.5k games being played daily was like cocaine.


SideKit not only lets you[track features by sending a signal](https://docs.appsidekit.com/ios/analytics/track-a-signal) everytime a page is opened/feature is used but also lets you have custom values for signals. That means that for a signal like "purchase" you can track where it came from with custom values. So if the user clicked the buy button on the home page you can send value "home" or if it was from the upgrade banner on the settings page you can send value "settings" and in the dashboard you can see a clear breakdown of the different signal values and understand where users usually purchase from.


## Analytics without messing up your privacy policy


The issue with most analytics providers is that they track a whole firehose of user data and if you're a smaller developer, understanding how this affects your privacy policy or your App Transparency Report on the App Store can be a soul sucking experience. Trust me, I know, that's why I made SideKit. We track no user data so you should be able to add the SDK with zero impact on your privacy policy.


Hopefully SideKit can help. You can create an account[here.](https://appsidekit.com/dashboard)
