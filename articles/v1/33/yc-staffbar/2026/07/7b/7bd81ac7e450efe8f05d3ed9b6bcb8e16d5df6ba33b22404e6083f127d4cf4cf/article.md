---
schema_version: "1.0.0"
document_id: "7bd81ac7e450efe8f05d3ed9b6bcb8e16d5df6ba33b22404e6083f127d4cf4cf"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/introducing-next-gen-superwall-sdks-entitlements-standalone-purchases"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:aff4a39009201134df4048d43f95cb5d02f46aa0e5e207d80c15dd250948ad54"
---

# Introducing Next Gen Superwall SDKs - Entitlements, Standalone Purchases, StoreKit 2 & More

Superwall just got a whole lot better. With our latest SDK updates — iOS SDK v4 and Android, Flutter, and React Native v2 — we’re introducing:


1. Cross-platform entitlements.
2. Enhanced campaign filters.
3. Streamlined product management flow with entitlements.
4. Observer mode and direct purchasing APIs.
5. Plus, full StoreKit 2 support, along with other enhancements.


These updates give you more control, better segmentation, and a smoother monetization experience. Building experiences like[mult-tier paywalls](https://superwall.com/blog/how-to-build-multi-tiered-paywalls) , and targeting the right user, at the right time, is easier than ever.


Here's a quick rundown of what's new.


### Cross-platform entitlements: unlock more flexibility


Now, products are tied to entitlements, letting you segment access to features across different tiers of service. If you're not offering multiple tiers of service, then you don't even have to think about them (we assign every product a default entitlement). But, if you have a setup where you might be offering something like a Silver, Gold, or Platinum tier, Superwall ensures your users get the right access — on any platform:


- Assign products to entitlements for better user segmentation.
- Easily manage subscription tiers across iOS, Android, React Native, and Flutter.
- Works seamlessly with Superwall’s paywall and campaign filtering.


### Entitlements in campaign filters: next-level targeting


Our powerful new campaign filtering now includes entitlements, giving you fine-grained control over who sees what. Now, growing LTV by showing the right upsell at the *perfect* time in the user journey is trivial. For example, following our example above, you can now:


- Target users with the "Silver" entitlement but not yet on "Platinum" entitlement.
- Exclude specific entitlements from an upsell campaign.
- Combine with AND/OR filters for ultra-specific targeting.


All of this is built right into our audience filtering tools. Simply pick which entitlements should be checked, and you're done:


Creating filters using entitlements


### Updated products: entitlements out of the box


We've made product setup easier than ever for entitlements. Now, every product is associated with an entitlement by default. You don't have to do anything with existing products, we've taken care of it for you. When you visit your products page now, you'll see entitlements, and a new button to add your own:


Add entitlements to seamlessly offer tiers of service


### More quality-of-life tweaks


Superwall now defaults to StoreKit 2 (in most cases, check out the[docs](https://superwall.com/docs/migrating-to-v4#2-storekit-2) for more), giving you a more reliable, modern way to handle purchases on iOS. Beyond that, we've added:


- **Direct purchasing APIs using Superwall:** Skip edge cases, StoreKit and the Google Play Billing Library altogether — purchase products in one line of code.[Docs](https://superwall.com/docs/direct-purchasing) .
- **Use Observer Mode to integrate with your existing setup:** Superwall can associate users with transactions using the new observer mode. Get setup with the best revenue tracking around.[Docs here](https://superwall.com/docs/observer-mode) .


Those are some highlights, but there's a lot of stuff to discover. To learn more, check out our[new docs](https://superwall.com/docs/getting-started-with-our-sdks) . We hope these updates make your life easier, and your ARR higher 🫡.
