---
schema_version: "1.0.0"
document_id: "07ffddd18e37bbaaa0e639028e355ca582a2c212a27453a7e8db11aeb84217d1"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/initial-data-is-in-app-to-web-conversion-rates-after-the-app-store-ruling"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:2b99e9b05942ec6fe4ac4fe8cbf0e94f6dfff4014b6755da5c9d2b4af382e55d"
---

# Initial Data Is In: App2Web Conversion Rates After the App Store Ruling

## Overview


With the recent Apple v Epic injunction, developers now have the unprecedented opportunity to link directly from their apps to external web-based payment systems in the US. In the seventeen year history of the App Store, this has *never* been allowed.


Simply put — we are in new territory, and there's no way to know how this will all shake out. But, what we *can* do is look at the data, and Superwall has some of the industry's first, and robust, data on this particular scenario. Today, we're going to peel it back so you can figure out what this all means for you.


The data is promising, but if you're expecting to boost your revenue instantly by 30%, you'll be disappointed. Reality paints a more nuanced picture. Here's what the numbers are actually telling us.


## The user flow our data is based on


Our early tests were set up like this:


1. **Control:** Users see a standard in-app paywall and purchase via Apple's in-app purchases (IAP). This is the "classic" setup we've all been using.
2. **Variant:** Users encounter the same exact paywall, only its C.T.A. button redirects them to an external browser using our web checkout links. In the control, this is the button that would kick off the standard StoreKit payment flow. After completing payment, users are directed back into the app via a deep link.


We initially assumed trial conversion rates would remain consistent with existing in-app purchase trials, but early trends suggest web-based trials might actually convert at a higher rate.


## Early conversion and revenue impact


So far, here's how the early data is breaking down:


Metric Change


Initial Conversion Drop **−11.9% decrease**


Net Proceeds Increase (approx.) **+19.85% increase**


While initial conversions are **lower** (likely due to friction in switching from the app to web), net proceeds **significantly** improve due to bypassing Apple's 30% fee. Again, this is a situation where you have to consider the entire end-to-end flow and what it means for you. Trading a slightly lower initial conversion rate for more take-home in the end might make sense for a lot of apps.


## The primary KPIs we're watching


While trying to get a sense of what how "succesful" this test is, we realized that there are two significant unknown factors which could further impact the long-term effectiveness of app-to-web funnels. Let's break those down:


### 1. Trial-to-paid conversion rates


Our current model conservatively assumes no change in the trial conversion rate. But, given the less familiar and slightly more complicated user experience of canceling web purchases, we anticipate the trial-to-paid conversion rate may actually increase. This wouldn't be surprising — users on the App Store have managed subscriptions using a similar flow for many years now. Any change to that flow will inevitably mean that it'll take time for consumers to adapt to.


But, what's the actual impact here? Well, if your trial conversion rate improves by just **10%** , then your net proceeds could jump by approximately **32%** .


### 2. Refund and chargeback rates


Web checkout brings different challenges when it comes to billing. Though StoreKit isn't without its issues, developers may come to find out that it handles a bevy of issues they'd rather not deal with. Chief among them? Refunds and chargebacks.


These two things are critical metrics yet to be fully understood. The data will need a bit more time to mature before we can understand their true impact. Again, it's possible these rates will increase with web-based checkouts, due to the more cumbersome cancellation process I mentioned earlier. We're closely monitoring these, though, and we'll be back with a follow up once we understand them more.


## Apple Small Business participants should carefully consider new flows


If your app currently benefits from Apple's Small Business Program (and its reduced 15% App Store fee), you might want to hold off before fully transitioning to web-based payments. If we run the numbers from our current model using a 15% commission instead of 30%, here's what we saw:


- **At current trial conversion rates:** Moving to web payments could actually *reduce* your net proceeds by about **1.30%** according to our early data.
- **However, if your trial conversion rate increases by 20%:** In this scenario, your net proceeds could rise by **17.74%** .


At Superwall, we say test everything — and those figures above show you exactly why. It may make sense for your app, or it could lead to reduced revenue. These figures also demonstrate why we tell developers and product managers to closely track their trial-to-paid conversion rates and refund rates before making sweeping changes.


## Want to get started?


It's up to you to figure out whether testing the new "app to web" flow makes sense for your app. But, having some early data should help lead you in the right direction. The landscape *is* going to change, and at Superwall — we're excited to be there with you every step of the way.


If you want to get started, here's the test that I recommend you try:


1. **Start Small:** Test your existing in-app paywall against a variant with web-based payments. This is the test we based our own data on.
2. **Set up in Minutes with Superwall:** Use Superwall's web checkout integration to quickly create these A/B tests without needing app updates.


It's early days, and this is a new world for all of us. You should always try to optimize your revenue streams, and app to web could help you do just that. But, remember, you have to monitor results carefully. And then, adapt as more data emerges (which is always easy using Superwall).


Good luck, and happy testing!
