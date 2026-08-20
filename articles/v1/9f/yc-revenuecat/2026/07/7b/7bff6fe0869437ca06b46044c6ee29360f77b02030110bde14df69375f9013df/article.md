---
schema_version: "1.0.0"
document_id: "7bff6fe0869437ca06b46044c6ee29360f77b02030110bde14df69375f9013df"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/company/win-back-campaigns-for-web"
published_at: "2026-07-07T13:02:58+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:e52a10611a02e622964f6c393de35a6aeb1d73adffd99ab5dcd3041f82464be7"
---

# Recover churned subscribers automatically with win-back campaigns for web

Every app loses subscribers. And while both Apple and Google have native win-back offers, they take real work to set up: StoreKit 2 integration, App Store Connect configuration, eligibility rules, image assets, App Review. On Android, win-back offers can only be redeemed inside your app, so the user has to come back on their own.


What if you could reach churned subscribers via email, the moment their access expires? And what if the re-subscription happened on the web, where you keep more of the revenue?


That’s what win-back campaigns for web does. It’s now in public beta.


## 2 settings, 60 seconds, done


You configure a campaign in the RevenueCat dashboard in about 60 seconds. There are two settings: pick the app you want to monitor for subscription expirations, and choose a Web Purchase Link with your win-back offer. That’s it.


When a subscriber’s access expires (not when they cancel, but when they actually lose access), RevenueCat sends them a transactional email with a “Claim offer” button. That button takes them to your web checkout, where they re-subscribe at whatever discounted price you’ve configured. For example, a first year at $39.99 instead of $99.99 that auto-renews at the standard price.


The trigger fires only on genuine end-user churn. If you revoke access or cancel a subscription from your side, no email gets sent.


The email is branded to your app. It pulls in your app icon, name, and appearance settings automatically. The template includes a clear “Claim offer” call to action linking to your Web Purchase Link. You don’t need to design, write, or host anything. In the current beta, the template uses a standard layout and English copy. Email customization is on the roadmap.


## Reach users that native offers can’t


Native win-back offers from Apple and Google are powerful, but they rely on the user returning to the App Store, opening your app, or checking their subscription settings. They’re passive. Your offer sits there and waits.


Win-back campaigns reach out at the exact moment access expires, via email, regardless of whether the user still has your app installed or what platform they originally subscribed on. A churned iOS subscriber, an Android subscriber, or a web subscriber all get the same email.


And because the re-subscription happens on the web, you bypass the 15%–30% platform commission entirely. You can offer a generous discount and still improve your effective revenue per recovered subscriber. It’s money from users who would have otherwise left completely.


If you already have Apple or Google win-back offers configured, this works alongside them. More places for a churned user to resubscribe is a good thing.


## Send them anywhere: paywall or checkout


The “Claim offer” link points to the Web Purchase Link you’ve configured. Depending on how you’ve set that up, the user can land on a paywall, a package selection screen, or go directly to checkout. You control the experience.


You can pair this with[Flexible Discounts](https://www.revenuecat.com/docs/web/web-billing/discounts) to create the right offer: introductory pricing on a yearly plan, percentage-off codes, or whatever makes sense for your audience.


## Their account picks up where it left off


The email link includes the user’s existing app ID. When they complete checkout on the web, their premium access is restored inside your mobile app immediately. The purchase shows up in their existing customer history, attributed to the same user, with the same entitlements.


## One campaign covers iOS, Android, and web


Win-back campaigns work with all three of RevenueCat’s supported web billing engines: RevenueCat Billing, Stripe Billing, and Paddle Billing.


The app you monitor for expirations can be iOS, Android, or web. This means the feature supports both **app-to-web** (a mobile subscriber churns and re-subscribes on web) and **web-to-web** (a web subscriber churns and re-subscribes on web) flows.


## What you need before creating a campaign


Win-back campaigns build on top of RevenueCat Web. Before you create one, you’ll need:


- **A web configuration.** Connect a billing engine (RevenueCat Billing, Stripe Billing, or Paddle Billing) by following[Getting started with RevenueCat Web](https://www.revenuecat.com/docs/web/overview) .
- **A Web Purchase Link with your win-back offer.** Create an offering with a discounted product (like a yearly plan with an introductory price), then create a Web Purchase Link for it. This is what the email links to.
- **Your customers’ email addresses.** RevenueCat sends the win-back email to the address stored in the` $email`[subscriber attribute](https://www.revenuecat.com/docs/subscriber-attributes) . If your app already collects email during signup or onboarding, you’re set. If not, you’ll need to start setting it via the SDK.


## Getting started


Once those are in place, creating a campaign takes seconds:


1. Go to **Lifecycle → Win-back** in your RevenueCat dashboard
2. Click **Create campaign**
3. Select the app to monitor for subscription expirations
4. Select the Web Purchase Link containing your win-back offer
5. Click **Create campaign**


The campaign starts monitoring immediately.


[Set up your first win-back campaign →](https://www.revenuecat.com/docs/web/winback-campaigns)
