---
schema_version: "1.0.0"
document_id: "fed5dcd87bafbab8d051fa5aa7c4ee054cd17247cab4e38c5a01e0364491b713"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/free-revenue-tracking-for-apps"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:d06c95fb10ba0998163462185b0772b8bc262b26409a03ed12004e1ae5fc7880"
---

# Free Revenue Tracking for Apps

Keeping tabs on revenue is critical for apps. From identifying trends to seeing how price tests are performing, it's not something you can do without. When it comes to breaking down revenue, though, you have a lot of options. Whether you use another third-party service, or the first-party tools offered by App Store Connect — at Superwall, we also have a robust suite of revenue tracking tools.


Plus, they're also *free* .


Today, I'd love to show off what you get when you set this up with Superwall. I'll also briefly cover some of the options you have at your disposal to get up and running quickly. Currently, our revenue tracking supports iOS apps — but we're closing in on Google Play support, too. I'll update this post once we have it live.


## Revenue charts


Our best-in-class charts are the star of the show here. They let you slice and dice data in several different ways, making it easy to see and understand the impacts of any price test, paywall change, or similar tests. In terms of revenue, some of the primary ones to check out are:


- **Proceeds** : Revenue after refunds, store fees, and taxes. In essence, your take-home amount.
- **Sales** : Revenue before refunds, taxes, and fees.
- **New Trials** : Trial starts, organized by trial start date.
- **Refunds** : Ratio of refunds to gross proceeds, organized by first purchase date.


The proceeds revenue chart


Revenue charts support natural language in their date range filtering, too. You could type something like "Last four weeks" and Superwall will magically filter to that date range.


If you're a visual person, you can get a walk through of our revenue charts over on YouTube.


If you decide to integrate Superwall in your app, or already have, then you can enjoy our entire suite of charts. We go beyond just revenue, too. We have charts for paywall conversions, users, new trial starts, and more. Check out the[docs](https://superwall.com/docs/charts) for a complete list.


### Transactions


One of my favorite features is the[transactions view](https://superwall.com/docs/overview-metrics#recent-transactions) .


Here, you can see recent transactions such as trial conversions, trial starts and more. If you're using our no-code integration, your transactions will look a bit like this.


Transactions view without using the Superwall SDK


However, if you integrate Superwall altogther, or simply enable[Observer Mode](https://superwall.com/docs/observer-mode) , Superwall can fill in more information such as associating events to user identifiers:


```text
let   options   =   SuperwallOptions  ()
options.  shouldObservePurchases   =   true
Superwall.  configure  (  apiKey  :   "  your_api_key  "  ,   options  : options  )
```


The transactions view in Superwall


Then, your charts can display a bit more information, like you saw in the first transaction view. Having Superwall integrated or using observer mode for revenue tracking purposes gives you a bit more insight, and it's still entirely free.


### iOS client


Our[iOS client](https://apps.apple.com/us/app/superwall/id6523434157) is a great way to check out proceeds on the go. No matter if you have one or several apps, you can setup widgets to keep tabs on proceeds throughout the day.


Superwall for iOS


And, when you login at[superwall.com](https://www.superwall.com/login) , you'll see your proceeds by default in our dashboard.


## Setting up a revenue source


When it comes to revenue tracking, all you have to do is setup a source. If you've not integrated the[Superwall SDK](https://superwall.com/blog/getting-started-with-superwall-in-your-indie-ios-app) , no worries. As I mentioned previously, we offer a no-code integration, meaning you can do the setup without releasing an update or changing any code. Our[docs](https://superwall.com/docs/overview-settings-revenue-tracking) have a comprehensive walkthrough, so I'll cover the basics here.


Once you've created an iOS app in Superwall, head to **Settings -> Revenue Tracking** . From there, you have three options to choose from:


1. [App Store Connect](https://superwall.com/docs/overview-settings-revenue-tracking#method-1%3A-app-store-connect) : The most common option, here — you let Apple pipe us revenue data directly into Superwall via App Store Server Notifications.
2. [Event forwarding](https://superwall.com/docs/overview-settings-revenue-tracking#method-2%3A-event-forwarding) : Similar to above, but here — you forward events to Superwall that you receive from your own server implementations.
3. [RevenueCat](https://superwall.com/docs/overview-settings-revenue-tracking#method-3%3A-revenuecat) : If you're using[RevenueCat with Superwall](https://superwall.com/docs/using-revenuecat) , you can use their events and send them over to Superwall.


Once you've done that, Superwall will start receiving monetization events. Plus, if you setup Superwall with the[App Store Connect API](https://superwall.com/docs/overview-settings-revenue-tracking#app-store-connect-api) , we can pull additional information — such as product data. You can even get the[In-App Purchase Configuration](https://superwall.com/docs/overview-settings-revenue-tracking#in-app-purchase-configuration) file configured so Superwall can power things such as[refund protection](https://superwall.com/docs/overview-settings-refund-protection) if you'd like.


## Amazing revenue analytics, all for free


We're committed to making the best tools around for apps of all sizes, no matter if you're a massive company or a thriving indie developer. Superwall can make your life easier and your revenue higher, and our free revenue tracking features are just one of the many ways we do that.


Get started with everything mentioned here for free today by[signing up](https://superwall.com/sign-up) . And, if you're ready to grow your revenue, you can ship an update at any point to start running paywall experiments with our SDK.
