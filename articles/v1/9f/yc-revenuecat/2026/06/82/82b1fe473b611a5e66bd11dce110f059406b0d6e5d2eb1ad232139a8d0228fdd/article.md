---
schema_version: "1.0.0"
document_id: "82b1fe473b611a5e66bd11dce110f059406b0d6e5d2eb1ad232139a8d0228fdd"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/dont-prompt-ratings-during-onboarding"
published_at: "2026-06-18T15:06:00+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:a72a9fb465cf14423fd3e269ef25fcb2c0efe6eb8aec35712cbc356cd3fe97ff"
---

# Move your rating prompt out of onboarding — or risk rejection

A common pattern in many new and existing apps is to prompt users for a review during the onboarding process. However, next time you submit your app for review, Apple will reject it if you’re still using this pattern.


## Asking ratings too early is considered manipulation


Apple has started rejecting apps under[App Review Guideline 5.6.3](https://developer.apple.com/app-store/review/guidelines/) , which states:


> *…Manipulating any element of the App Store customer experience such as charts, search, reviews, or referrals to your app erodes customer trust and is not permitted.*


This isn’t a new guideline, but Apple hasn’t previously applied it to onboarding in this way. Prompting for a review early in the onboarding flow was a common, if questionable, tactic. Some apps were collecting thousands of five-star ratings before users had done anything meaningful with the app.


It is not clear if Apple will go through apps that are already in the App Store, but apps using this pattern will get rejected during the review process for new versions of the app.[It might also be that Apple was already ignoring ratings that occur during onboarding](https://x.com/arielmichaeli/status/2067287069819355338) by comparing the time between install and rating; nullifying the ASO benefits.


## What should you do instead?


The first step to fixing this is pretty simple: just remove all the rating and review prompts from your onboarding flows. The second step is more complicated; you should prompt only after the user has meaningfully engaged with your app. Ask for a review when there’s actually something to review. So when exactly?


Apple’s own Human Interface Guidelines say to prompt at “natural and happy moments”: after a user has completed something, achieved something, or had a genuine reason to form an opinion about your app.


That sounds straightforward, but in practice most apps don’t have a systematic way to know when that moment actually is. A user who just opened your app for the first time isn’t there yet. Neither is someone who made it through onboarding but hasn’t done anything meaningful.


Now, if you’re wondering how to do that, you’re in luck.[We’ve actually written about it before in this guide for ethically hacking your App Store ratings](https://www.revenuecat.com/blog/engineering/how-to-hack-your-app-store-ratings/) . In the guide, you learn how to build “a happiness engine” that tracks user engagement with the app, and prompts for a review when you’re most likely to get a positive one. The same guide applies to the Google Play Store as well (although Google is not yet limiting).


## Bottom line


Stop asking users to review your app during the onboarding flow. Apple will reject your app if they catch you doing it. Most likely they were already filtering out these ratings, by comparing the interval between app install and time of rating.


Instead prompt users to review the app after they’ve meaningfully engaged with it. Track those engagements and build an engine for prompting for a review when the user has both engaged with your app and is happy with their experience.


- [How to hack your app store ratings (ethically)](https://www.revenuecat.com/blog/engineering/how-to-hack-your-app-store-ratings/)
