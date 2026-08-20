---
schema_version: "1.0.0"
document_id: "6489789b5fe5d61dd976fc6d2fb37568c241de779b9a5b58041216f676a74be6"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/wwdc26-whats-new-for-apps"
published_at: "2026-06-09T18:01:53+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:1fd13b4abff734bf998a08de428663b68d410e85904dfddb24cb2e854452e55c"
---

# WWDC26: What’s new for subscription apps

WWDC26 has wrapped up and there’s a lot to dig into for subscription app businesses. We’ve been going through the session videos and release notes to figure out what Apple has changed this year that actually matters for developers building with RevenueCat.


Most of the keynote was focused on Siri AI and iOS 27’s design updates, but the StoreKit and App Store sessions had some of the most significant subscription business changes in years. Here’s everything worth knowing.


## **Annual subscriptions can now bill monthly**


(By the way, this is only available outside of the United States and Singapore for now, and requires iOS/iPadOS/macOS/tvOS/visionOS 26.5 or later.)


Apple has introduced a new pricing option that lets customers pay for an annual subscription in monthly installments. The customer still commits to a full year, but the cost is spread across twelve monthly payments instead of charged upfront.


This is a meaningful new tool for paywalls. Annual plans convert well because of the price-per-month math, but the upfront cost is a real barrier for a lot of users. Monthly-billed annual plans give you the retention benefits of an annual commitment without asking users to hand over a year’s worth of money at once.


You can add this billing option to new or existing one-year auto-renewable subscriptions in App Store Connect.


### **What changes in your code**


The entry point on the client side is a new pricingTerms property on a product’s subscription info. It returns an array of every billing plan available for that product. Every annual subscription has at least one entry with a` billingPlanType` of` .upFront` . If you’ve configured a commitment plan, a second entry shows up with a` billingPlanType` of` .monthly` .


There’s a new` preferredSubscriptionPricingTerms` view modifier for` SubscriptionStoreView` if you’re using StoreKit’s built-in UI. For custom paywall UI, read` pricingTerms` directly to get both the monthly price and the total 12-month commitment price so you can display both clearly to users.


To trigger the purchase, pass the billing plan as a purchase option:


```text
let result = try? await product?.purchase(options: [.billingPlanType(.monthly)])
```


### **What changes on your server**


The JWS transaction payload has new fields to be aware of:` billingPlanType` ,` renewalBillingPlanType` , and` commitmentInfo` . The` commitmentInfo` block tells you which billing period a customer is in, the total number of billing periods, the committed price, and the commitment expiration date.


This is important: a single annual product ID can now represent two completely different billing arrangements. Make sure your backend is reading and storing these new fields, or you’ll end up with incorrect entitlement data.


### **What this means for RevenueCat customers**


Your paywalls will need to show both the monthly price and the total 12-month commitment. “Pay $4.99/month, billed over 12 months” is not the same thing as “$59.99/year” and your users will expect to see both.


In analytics, committed monthly billing is not the same as a regular monthly subscription. If you don’t segment them, your LTV models will be wrong.


One testing caveat: there’s a known Xcode bug where` pricingTerms.commitmentInfo.price` returns an incorrect price for commitment plans in StoreKit Testing. Keep that in mind as you build and test.


RevenueCat supports committed monthly billing out of the box. We parse and store the new` billingPlanType` ,` renewalBillingPlanType` , and` commitmentInfo` fields automatically, so you don’t need to build custom backend logic to handle them. See the[docs](https://www.revenuecat.com/docs/subscription-guidance/apple-monthly-with-commitment) for implementation details.


## **Retention Messaging lets you save subscribers at the moment they cancel**


Retention Messaging is one of the most interesting business tools Apple has shipped in a while. When a subscriber goes to cancel, the App Store can now show them a message or offer that you’ve configured. This is access to a moment in the subscription lifecycle that developers previously had no visibility into.


There are two ways to use it.


The App Store Connect path requires no custom server infrastructure — you configure messages and offers directly in App Store Connect and Apple handles the rest. The real-time path is more powerful but optional.


**App Store Connect configuration** is the simpler path. You set up messages in App Store Connect, optionally attach a retention offer and creative assets from the new Asset Library, and Apple displays them during the cancellation flow. No additional server infrastructure required.


**Real-time Retention Messaging** is more powerful. When a user tries to cancel, the App Store makes a server-to-server request to an endpoint you configure, passing fields like` originalTransactionId` ,` productId` , and` userLocale` . Your server responds with what to show: a message, an alternate product, or a promotional offer.


This opens up subscriber-aware save flows that weren’t possible before. You can offer a discounted annual plan to a loyal monthly subscriber, suggest a lower tier to someone on a premium plan, skip the offer entirely for someone who already redeemed one recently, or show different messaging by market.


Apple shared early data in the WWDC session (session 309): subscriptions using Retention Messaging saw an average save-rate lift of 1.4 percentage points, equivalent to an 82% increase. Promotional offer messages had the strongest lift at 5.5 percentage points. Apple noted that results vary by developer.


One important technical requirement: real-time Retention Messaging requires a fast server response. Apple runs a sandbox performance test and your endpoint has to pass before you can use real-time messaging in production. If your server doesn’t respond in time, the App Store falls back to your App Store Connect configuration. Plan for that fallback path.


Real-time retention also works with the new commitment billing plans. You can respond to a cancellation request by offering a switch to the monthly-billed annual plan, which is a useful save option for users who are cancelling because of the upfront cost.


### **What this means for RevenueCat customers**


Retention offer transactions have their own identifiers in the JWS payload:` offerType` :` 5` ,` offerIdentifier` , and` offerDiscountType` . Make sure your analytics pipeline tracks these separately from other promotional offers so you can measure actual save rates, post-save retention, and which offer types are performing.


RevenueCat handles the real-time Retention Messaging server side for you. The dashboard lets you configure all four message types — text, text + image, switch plan, and promotional offer — and manages the response window requirement automatically. You’ll still need to complete Apple’s approval form to register the endpoint, but the infrastructure is taken care of. Full setup steps are in the[docs](https://www.revenuecat.com/docs/platform-resources/apple-platform-resources/apple-retention-messaging-api) .


## **Subscriptions can now be sold to groups and organizations**


Apple has introduced two new ways to sell subscriptions beyond the individual consumer model.


**Group purchases** let a single subscriber buy multiple seats and invite others to join from inside your app. Apple handles the invitation flow; each person joins using their own Apple Account. This is available for in-app purchases you build yourself with the StoreKit 2 purchase flow.


**Volume purchasing** puts your subscription in front of enterprise and education buyers through Apple Business Manager and Apple School Manager. Seat assignments are handled through existing device management workflows, which means IT can deploy your app to a fleet of devices without users needing to do anything themselves.


Volume purchasing is available this fall. Group purchases arrive this winter. Both are configured in App Store Connect and are on by default for most auto-renewable subscriptions (Family Sharing subscriptions are opted out).


Volume pricing bands are configurable: you can set up to five price tiers with reduced per-seat pricing for larger purchases.


On the StoreKit side, Apple added` Transaction.OwnershipType.assigned` and` Transaction.RevocationType.assignmentRevoked` enum values to support volume purchases. Transaction query methods also now return transactions assigned to a Managed Apple Account.


### **Entitlement design is the hard part**


If your app has any team, education, or organization use case, think through these before you enable group subscriptions:


- The buyer is often not the end user
- One transaction can produce multiple entitled seats
- Seats can be assigned, revoked, or reassigned
- Volume purchases may arrive through device management rather than the app itself
- Volume pricing changes your per-seat revenue metrics


This is genuinely new territory for most subscription apps. The purchasing layer is simpler now, but the entitlement logic is more complex.


One implementation detail worth flagging now: both group purchases and volume purchasing require StoreKit 2. If your app hasn’t migrated yet, it’s worth prioritizing that before these features become available later this year to avoid delays.


We’ll have updates later this year on how to implement this in RevenueCat.


## **Cross-developer Bundles and Suites**


This is the App Store change getting the most attention in the broader developer community, and it’s worth understanding what it actually is.


**App Store Bundles** let developers from different companies package their subscriptions together and offer them at a combined discount. Users can still buy each subscription individually; the bundle is an additional offering alongside the standalone products.


**App Store Suites** are different: a collection of subscriptions that only exist as a package and can’t be purchased separately. They’re designed for tightly related apps from different developers that make more sense together than apart.


Both formats address something developers have been asking for for years: cross-developer bundling without requiring the same parent company.


The StoreKit API for Bundles and Suites is available to prototype in Xcode 27 today. New` Product.ProductType` values represent Bundles and Suites, and` Product.SubscriptionInfo.BundledSubscription` lets you fetch merchandising data about subscriptions in a bundle.` Transaction` and` RenewalInfo` also have new fields for Bundle and Suite status.


That said, Apple has said full program details are coming later in 2026. Prototype against the API now, but don’t plan a ship date yet.


### **What this means for your RevenueCat Dashboard**


Bundle and Suite subscribers will look different in your analytics from direct subscribers. Revenue attribution, churn analysis, and per-app subscriber counts will all need to account for the bundle layer. We’ll have more to share on RevenueCat support as the program details become clearer.


## **App Store Merchandising: new placements for Creative Assets**


Apple is adding new creative placements on the App Store this year. Developers can now supply rich images and videos that appear in product page headers and search results, and these assets also work with custom product pages and product page optimization tests. They’re managed through a new Asset Library in App Store Connect.


A few things worth noting about how this works:


- Assets can be submitted for App Review independently from an app update, so you can refresh seasonal creative or coordinate with an Apple Ads campaign without shipping a new build
- The Asset Library centralizes all your creative across custom product pages, In-App Events, and now these new placements, so you’re not uploading the same assets in multiple places
- A new product page preview lets you see exactly how your page looks across languages, Dark Mode, and device orientations before you publish


The same Asset Library also feeds Retention Messaging. That means your creative team can manage App Store acquisition assets and cancellation-save assets from the same place. That’s a meaningful workflow improvement for teams that have historically treated those as completely separate functions.


## **Personalized Collections and App Notes**


Apple is rolling out two new discovery surfaces on the App Store. Personalized Collections are curated lists that appear on the Apps, Games, and Search tabs, surfacing apps based on each user’s interests and behavior. App Notes are short explanations that appear alongside a recommendation, telling users why a specific app is being suggested to them.


Both are already rolling out in English (US), with more languages and regions coming later this year.


There’s no developer action required to participate. That said, apps with complete, accurate metadata — clear descriptions, relevant keywords, up-to-date screenshots — will be better positioned to surface in relevant collections. It’s a good time to audit your App Store listing if you haven’t recently.


## **Offer Code Redemption gets a proper result**


A smaller but welcome StoreKit update: the offer code redemption sheet now returns a` VerificationResult` when redemption completes, rather than firing and forgetting.


On success, you get a` VerificationResult` containing a Transaction to verify and finish. On failure, you get a descriptive error. The API also now takes a set of` RedeemOption` values to configure the flow.


This brings offer code redemption in line with how the rest of StoreKit 2 works. If you have a custom “redeem code” button in your app, it’s worth updating the transaction handling path to use the new result.


## **Unified App Review submissions**


A useful operational change: you can now group In-App Purchase products into a single App Review submission alongside other item types like in-app events, custom product pages, and product page optimizations.


Previously you had to submit each product individually and track them separately. Now you select everything together, submit once, and track review status from one view. The App Store Connect API’s` reviewSubmissions` collection now supports IAP, subscription, and subscription group resources. Apple is deprecating the older per-resource submission endpoints in favor of this unified path.


If you have tooling or automation built around the old individual submission flow, start migrating now rather than waiting.


## **App Review Guidelines: saturated categories now face removal, not just rejection**


Alongside the new monetization features, Apple updated its App Review Guidelines in a way that’s worth knowing about.


Previously, guideline 4.3(b) warned developers not to submit new apps in categories that were already crowded with low-effort entries. Apple would reject new submissions in categories like flashlight apps, fortune telling apps, and dating apps unless they offered something meaningfully different.


The updated guidelines go further. Apple now says it may remove existing apps in well-established saturated categories if they are not “updated, improved, or attracting customers.” The list of targeted categories has also been expanded to include wallpaper apps, simple timers, and sound effects. Apple also added language calling repeated submissions of low-effort apps “mediocre” and “low-quality,” with a warning that developers who keep submitting them may lose Apple Developer Program access entirely.


This is a meaningful shift from “we won’t accept new ones” to “we may remove the ones already there.”


It connects directly to Apple’s new Personalized Collections discovery feature. Apple is building better ways for users to find quality apps, and low-quality clutter degrades those surfaces. The two moves go together.


For existing developers building legitimate subscription businesses, this is more tailwind than headwind. A cleaner App Store, optimized for apps that are actively maintained, is a welcome addition!


## **Summary**


WWDC26 brought more than just the usual updates—it was a really exciting year for subscription apps! Apple has introduced some fantastic new ways to help your business grow and connect with users. Here’s a quick look at the highlights:


- **Committed annual billing** gives you a new middle-ground pricing option that could convert users who balk at the annual upfront cost
- **Retention Messaging** gives you a save opportunity at the moment of cancellation that didn’t exist before
- **Group purchases and volume purchasing** open up B2B and team sales paths without building your own billing infrastructure
- **Cross-developer Bundles and Suites** are coming later this year, with an API available now to prototype
- **Creative Assets** expand your merchandising surface on the App Store
- **Updated App Review Guidelines** raise the quality floor across the store


We know there’s a lot to dig into here, especially regarding analytics and how you design your entitlements. We’re already hard at work figuring out the best ways for RevenueCat to support you through these changes, and we’ll be sharing more updates as we learn more. We’re excited to see what you build!
