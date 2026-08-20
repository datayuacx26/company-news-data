---
schema_version: "1.0.0"
document_id: "0d631f5aeac6d7e10ff22bc357640571ba48d8cb653c4442fe88198a825e6e79"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/user-segmentation-recipes-to-jump-start-app-growth"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:0028089bb306ffcf5dc0ae9dbb4d686a0118ada4036a4bd8ab8e9d3ee0e23b79"
---

# User segmentation recipes to jump-start app growth

Most developers start monetizing their app using the same playbook: show a skippable paywall during onboarding and a passive upgrade button somewhere in the core experience or settings. And that's a terrific start!


Once developers cover the basic paywall scenarios they often expand into more sophisticated monetization experiences that you may also want to use.


Sophisticated doesn't mean complicated, though. You can follow simple user segmentation recipes to tactfully increase conversions without beating your users over the head.


## Segment by country or groups of countries


One of the most common ways developers segment users is by country or groups of countries. Localizing your app for different markets is a key growth lever. Localizing your paywalls means more than just translating copy, though. Price, introductory offer type, design style, and social proof are all considerations when localizing your paywall.


Segmenting by country can be done by using country code or locale filters.


Audiences Filter parameter Operator Value


India locale contains IN


Brazil locale contains BR


Mexico locale contains MX


United States locale contains US


It can be challenging to create an audience segment for every single market, especially if your app is localized in ten or more countries. When this happens, app developers will typically break down markets into tiers or a combination of groups with some specific countries.


For example, you might group markets into three tiers based on the price or offer you want to show. Take this a step further and create an audience for your top three to five markets and then a segment that includes all other markets.


Audiences Filter parameter Operator Value


Tier 1 locale contains US, GB, AU


Tier 2 locale contains MX, BR


Tier 3 All others


The good news is you can start simple and expand as your app business needs without having to nail it the first time.


## New versus returning users


When a new user first experiences your app, they're coming in fresh. Yes, they may have heard about it from a friend or searched the App Store, but the accessibility of the App Store and Play Store means the odds are your new users haven't done research before installing.


The data clearly shows that presenting a paywall in onboarding correlates with increased subscription signups and revenue.


But what about users who need to explore your app before subscribing? Or users who've been around a few months and finally have a use case for a premium feature?


A simple way to segment users is by *days since app install* to show different paywalls to new, recently installed, and returning users.


New users are those who installed today. Their total days, days since app install, is less than 1. Some organizations will refer to this as "day zero" (D0) install. In Superwall you would set up an audience using the filter` daysSinceInstall < 1` .


Audience Filter parameter Operator Value


New installs daysSinceInstall < less than 1


Recently installed users are typically those in their first week of using your app. They're still new, but are still in the new user activation stage where they're experiencing the "aha" moment, but haven't formed a habit of using your app yet. You want to try to convert these users with a free trial or introductory price to get them to experience the premium app and help drive those habit-forming experiences.


In this scenario, you want to show a specific paywall for those between their first and seventh day since app install. The audience filters you'd use in the platform are a combination of` daysSinceInstall ≥ 1` and` daysSinceInstall ≤ 7` .


Audience Filter parameters Operator Value


Recent installs daysSinceInstall ≥ greater than or equal to 1


daysSinceInstall ≤ less than or equal to 7


Users who don't subscribe during the first week typically fall neatly into a freemium user. These users aren't ready to pay for your app but may upgrade still. There are lots of experiments to run on this segment of users, from upgrade discounts to value prop messaging to personalized paywall experiences. These users are segmented using` daysSinceInstall ≥ 8` .


Audience Filter parameter Operator Value


Returning users daysSinceInstall ≥ greater than or equal to 8


## Canceled and expired users


At some point, every developer wants to try offering a discount to reactivate a customer, referred to as a winback offer. Both Apple and Google provide offer types to reactivate or retain customers with extended trials or discounted subscriptions.


But you don't want to show this to just anyone, especially your happy subscribers. To solve this, most Superwall customers[set a user parameter](https://superwall.com/docs/setting-user-properties) based on RevenueCat subscription status or their own entitlement tracking. Here's an example of what this might look like in your audience filters.


Audience Filter parameter Operator Value


Canceled users subscriptionStatus = equals canceled


Expired users subscriptionStatus = equals expired


## Users with zero paywall views


The first time a user sees a paywall doesn't necessarily happen during onboarding. Maybe your app doesn't show a paywall during onboarding, or maybe you have legacy users who signed up before you monetized your app. Improving conversions of these user segments can include experiments with special introductory offers or different types of messaging on your paywall designs.


Thankfully, it's easy to control this experience in Superwall using just one filter.


Audience Filter parameter Operator Value


No paywall views totalPaywallViews = equals 0


## Show a different paywall to users with many paywall views


Ever notice how you become blind to seeing the same thing over and over? This is called inattentional blindness. Your users experience this when they see the same kinds of notifications or paywall offers on repeat.


When you experience the diminishing value of showing the same paywall or offer, you can mix it up by targeting users who've seen the same paywall a certain number of times. The simple audience recipe looks like this.


Audience Filter parameter Operator Value


Many paywall views totalPaywallViews > greater than N


Let's say you want to only show the offer to users who declined a specific paywall placement, for example, on app launch. In that case, you would add an extra filter to this which will tell Superwall to show a paywall to the audience who previously viewed a paywall *N* times on app launch.


Audience Filter parameter Operator Value


Many paywall views totalPaywallViews > greater than N


presented_by_event_name = equals app_launch


## Freemium gated features


Most apps use a freemium model where the majority of users are free and a group of them upgrade to access premium features or content. There are plenty of tactics for upgrading free users and one of the most common is feature gating.


In this scenario, a user tries to access a premium feature but is blocked until they upgrade. To set this up in Superwall, create a campaign with the placement event set to your premium feature event. In your audience, add a paywall with the **Feature Gating** setting set to "Gated". It's that simple.


## Limited access gating


A more advanced version of this is called *limited access gating* where free users can access a premium feature a set number of times before having to pay, allowing them the opportunity to experience the value of that feature before deciding if it's worth the upgrade.


To experiment with this tactic, you'll want to set two audiences in the same campaign:


1. Audience A will use a paywall with the **Feature Gating** setting set to "Non Gated" and using the filter` totalPaywallViews` as less than the number of times you want a free user to access a premium feature.
2. Audience B will add a paywall with the **Feature Gating** setting set to "Gated" and the filter` totalPaywallViews` will be greater than or equal to your premium feature limit.


Audience Filter parameter Operator Value


Audience A (ungated) totalPaywallViews < less than 3


Audience B (gated) totalPaywallViews ≥ greater than or equals to 3


## Periodic freemium upgrade offers


We all want to convert as many of our free users to a paid subscriber as possible, but even the best-converting apps will only average 15-20% of installs converting on a trial offer. Yes, you want free users to upgrade *and* you don't want to show them a paywall so frequently that they abandon your app altogether.


So how do you control this in Superwall? You'll use a combination of audience filters with a limit in your campaign. The campaign placement and audience filter should be set to your own business logic. Meanwhile, your paywall **Present Paywall** setting should be set to "Check User Subscription" which means the paywall will only be shown to users who are *not* subscribed.


Below your audience filter, add a limit.


Superwall audience filter limits


Here are a few examples of limits we've seen customers use.


Up to Every Interval


1 time 2 days


5 times 1 month


1 time 60 minutes


10 times total


## Experimenting with audiences


Just like paywall design and pricing, audience experimentation is an important lever for unlocking growth. Start simple with your audience segments and expand as your experiments, both successes and failures, lead you to learn more about your users.
