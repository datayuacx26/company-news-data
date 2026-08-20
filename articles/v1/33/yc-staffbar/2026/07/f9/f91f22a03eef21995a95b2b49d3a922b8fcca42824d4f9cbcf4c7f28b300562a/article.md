---
schema_version: "1.0.0"
document_id: "f91f22a03eef21995a95b2b49d3a922b8fcca42824d4f9cbcf4c7f28b300562a"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/stripe-checkout-for-ios-apps-is-here"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:64d3d3918a5698300914dddbcc31482f89b81409c09a892caf1496f7ceae25d7"
---

# Stripe Checkout for iOS Apps is Here

Earlier this week, we shared[initial insights](https://superwall.com/blog/initial-data-is-in-app-to-web-conversion-rates-after-the-app-store-ruling) into how our customers were adopting web checkout flows following the Apple v Epic ruling.


Today, we're thrilled to announce more improvements. With Superwall's new iOS SDK 4.3.4, you can now directly link to Stripe checkout from your iOS paywalls.


Now, paywalls can directly open Stripe checkout flows


Here's how the streamlined flow works:


1. User taps the buy button on your iOS paywall.
2. Stripe checkout immediately opens.
3. After successful payment, Superwall deep links back into your app.


## Get started today


If you want to ship this today, getting started is easy. First,[follow our docs for web checkout setup](https://superwall.com/docs/web-checkout-overview) if you haven't yet. There, we'll get you up and running with a Stripe account linked to Superwall. Then, you can add Stripe products (among much more — you get[full web checkout links](https://superwall.com/blog/web-checkout-for-mobile-apps-is-here) once you're done there!).


Next, you'll add one of those Stripe products to an iOS paywall. When Superwall detects a Stripe app configured in the project, all Stripe products become available in the product selector:


Stripe products, in addition to your App Store products, are now selectable


It's as simple as adding one, and then initiating a checkout for that product. That's it — now you can open up Stripe checkout right from your paywall.


## Make it seamless with Universal Links


To further enhance the user experience, configure[Universal Links](https://superwall.com/docs/web-checkout-configuring-stripe-keys-and-settings#universal-links-optional) . Users will automatically be redirected back into your app post-checkout, making the entire end-to-end experience feel fantastic.


If you haven't set up Universal Links, no problem. We'll provide a friendly page prompting users to return to your app after a successful purchase.
