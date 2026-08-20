---
schema_version: "1.0.0"
document_id: "c21d5c9716a29d8bac1585e19f74e27b711e6f079e96221abc5b49836dd10a38"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/play-billing-v9"
published_at: "2026-05-20T01:39:35+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T22:13:02.720629+00:00"
content_hash: "sha256:efc1395b1df811a3d23d1cc9d76482214f3bff0e88af79167005a984dea833be"
---

# Google I/O 2026: What’s New in Google Play and Play Billing Library 9.0

If you spent keynote day refreshing the[Play Billing release notes](https://android-developers.googleblog.com/2026/05/io-2026-whats-new-in-google-play.html) wondering when v9 would land, same. It dropped on May 19, 2026, the same day as the I/O announcements, and the rest of the Play story landed in the same window. v9 itself is a smaller surface than v8 was last year (breathe out), but it stacks on top of a bigger Play story this year: AI infused discovery, churn cutting billing changes, and a Play Console that drafts your store listings for you.


## TLDR


- **Play Billing Library 9.0.0 (May 19, 2026)** : in app messaging for opt in price increases, blocked Play Store now returns` BILLING_UNAVAILABLE` ,` getLinkUri` is` @Nullable` , target SDK 35.
- **Discovery moves into Gemini** : 450,000 plus movies and shows surface in the Gemini app.[Engage SDK Collections](https://android-developers.googleblog.com/2024/07/introducing-collections-powered-by-engage-sdk.html) hits 30M MAU and 45% YoY lift in app opens.
- **Play Shorts and Ask Play** : short form video on store, plus a Gemini backed search overlay answering 95% of queries.
- **Sidekick** : AI overlay for games. No SDK work. Global rollout in summer 2026.
- **AI in Play Console** : CSV or Sheets driven listing pre population, agentic catalog management, AI Q and A on metrics.
- **Billing aimed at involuntary churn** : delayed charging for low risk users, account recovery extended 30 to 60 days, new in app subscription management API.
- **Protected with Play** : one dashboard for integrity, distribution, and fraud. 160M spam reviews and $3.2B in fraud blocked last year.


In this article, you’ll explore each of these in turn: Gemini and Ask Play, Play Shorts and Sidekick, the AI assisted Play Console, the three billing changes that target involuntary churn, the four code level changes in Play Billing Library 9.0.0, the analytics, and Protected with Play surfaces.


## Discovery moves into Gemini and conversational surfaces


The single biggest shift announced at I/O 2026 is that Play discovery no longer ends at the Play Store. Google is pushing app and content recommendations into the Gemini app on Android and on the web, so a query that used to land a user on a generic answer page can now deep link them into your app’s relevant screen. The first wave covers entertainment, with over 450,000 movies and TV shows surfaced through Gemini, including live sports streaming links that route directly into the partner app.


For developers, this means your store listing is no longer the only place Google’s recommendation engine reads.[The Engage SDK](https://play.google.com/console/about/programs/EngageSDK/) , which already exposed in app content to Play surfaces like[Collections](https://android-developers.googleblog.com/2024/07/introducing-collections-powered-by-engage-sdk.html) (the widget on the Android home screen that resurfaces your in app content), has grown to over 30 million monthly active users and is driving a 45% year over year lift in the number of app opens for participating titles.


Store listing integration ships next month, with new tablet surfaces including home screen Collections, and every Engage SDK surface now scales across 80 plus Play markets. If you publish content with structure beyond the package name (articles, episodes, products, levels), the cost of adopting Engage SDK is the same as last year and the surface you reach is now substantially larger.


The pattern to internalize is that any single piece of content inside your app has a real chance of being its own discovery entry point. Build deep links for everything that has a stable identifier, keep them indexed through Engage SDK, and assume the user lands on a screen mid app rather than at your home tab.


## Play Shorts and Ask Play change the on store experience


Inside the Play Store itself, two surfaces are new. Play Shorts is a full screen, portrait, short form video feed rolling out to US users and select developers, with broader market expansion in the coming months. The feed sits next to traditional app browsing and lets you place short form promotional video against an audience that is already in store and primed to install.


The second surface is Ask Play, a conversational search overlay backed by Gemini. The on store Q and A feature that has been quietly running for a year already answers 95% of user queries, and Ask Play extends that with summary recommendations rendered as natural language responses to questions like “what app should I use to learn guitar.” For developers, the implication is that the keywords in your store listing are no longer the only thing that determines whether your app shows up. The descriptions, screenshots, and review text all feed Gemini’s answer construction, so listing copy now has to read well for both a human user and an LLM.


## Sidekick: an AI overlay for games


For games specifically, Play Games Sidekick is the biggest games announcement at I/O 2026. Sidekick is an in game overlay that surfaces AI generated tips, rewards, and achievements without requiring any new SDK work from participating titles. Over 100 games have launched with Sidekick already, and Google is opening eligibility to all participating titles globally in summer 2026, along with new social features that let players track friends, share achievements, and discover what friends are playing.


Sidekick lives at the platform layer rather than inside each game. You opt in through Play Console rather than wiring up an SDK, and the AI tips draw on Google’s own model of the game rather than on data you ship. This keeps the integration cost low and explains how Google can promise the feature works for any participating title rather than only those who invest in custom integrations.


## AI in the Play Console: localization and catalog


The Play Console itself got a substantial AI pass focused on two operations that consume disproportionate time for monetization teams: localizing store content and managing SKU catalogs.


For localization, you can now upload a CSV or Google Sheet and have the Play Console pre populate the multi language fields, with Gemini handling the translations. A separate capability turns your keyword set into listing copy automatically. Subscription benefits, which are notoriously easy to mistranslate when handed to a generic translation service because the source phrasing is short and context light, are now translated by a model that has been tuned on Play’s own corpus of subscription copy. The practical effect is that adding a new market goes from a week of agency turnaround to an afternoon of review.


For catalog management, the Play Console adds agentic bulk price changes, SKU import, and metadata configuration automation. The word “agentic” is literal here: the system chains operations on your behalf, ingesting a SKU list, generating regional pricing recommendations, applying them across markets, and surfacing the result for approval. Teams that maintain hundreds of SKUs across dozens of regions can move from manual entry to a review and approve workflow.


## Billing changes that reduce involuntary churn


The billing announcements at I/O 2026 are the parts of the keynote that translate most directly into recovered revenue. Three changes work together to reduce involuntary churn, the kind that happens when a renewal payment fails and the subscription expires before the user has a chance to fix it.


The first is delayed charging. When a renewal payment fails and Google’s risk model classifies the user as low risk, Google Play continues to retry the charge in the background while granting the user continued access. The user does not see a payment failure dialog, the app does not lose the entitlement, and a successful retry resolves the situation transparently. From the app’s perspective the purchase stays in the` PURCHASED` state during the retry window. If a final retry fails, the standard grace period and account hold flow takes over. The risk model gates this behavior because granting access on a failed payment carries fraud risk, so the feature applies only when the model is confident the user is genuinely going to pay.


The second is the extended account recovery window. Account hold, the state a subscription enters when grace period runs out without a successful payment, used to last 30 days. I/O 2026 extends that window to 60 days. Google’s published figures from the extension test are an 18% reduction in involuntary churn and a 9% reduction in total churn for top developers, so smaller apps should expect more modest effects. The longer recovery window costs nothing on the developer side: the subscription state is identical, the same RTDN notifications fire, and recovery surfaces through the existing` SUBSCRIPTION_RESTARTED` event the same way it did at 30 days. Your app keeps the user’s data available longer, and your backend gets more time to surface payment fix prompts before the subscription expires.


The third is flexible subscription management. A new in app subscription management API lets you offer plan changes and downgrade offers at the moment of cancellation without bouncing the user out to the Play Store subscription settings screen. Prorated refunds for downgrades are handled through replacement modes on the existing subscription update flow, so a user who downgrades from annual to monthly receives the correct credit automatically.


The three changes compound. If your subscription business sees 5% involuntary churn per month and you adopt the extended recovery window, even a fraction of the published 18% reduction takes that to roughly 4.1%. Over a year, that translates into a larger active subscriber base for the same gross adds.


## Play Billing Library 9.0.0: what actually changed in code


Play Billing Library 9.0.0 shipped on May 19, 2026, and if you already migrated to v8 last year, you can relax: the API surface this time is intentionally small. v8 was the breaking release. It retired the` SkuDetails` era methods (` querySkuDetailsAsync` , the no argument` enablePendingPurchases` , the legacy` queryPurchasesAsync` overload), introduced sub response codes on` BillingResult` , and renamed alternative billing to user choice billing. v9 builds on that foundation and adds a single new in app messaging capability, two compatibility adjustments, and a target SDK bump. Sub response codes like` PAYMENT_DECLINED_DUE_TO_INSUFFICIENT_FUNDS` and` USER_INELIGIBLE` carry forward from v8 unchanged.


The dependency update is straightforward:


```text
dependencies   {
val   billingVersion   =   "9.0.0"
implementation  (  "com.android.billingclient:billing:  $billingVersion  "  )
implementation  (  "com.android.billingclient:billing-ktx:  $billingVersion  "  )
}
```


The v9 changes that actually require code attention are in three areas.


### In app messaging for opt in price increases


The main addition in v9 is a new category on the in app messaging API. Pre v9,` showInAppMessages` with` InAppMessageCategoryId.TRANSACTIONAL` displayed Google’s recovery prompts for users in grace period or account hold. v9 extends the same surface to display an outstanding opt in price increase, letting the user confirm the new price without ever leaving your app.


You call the same method you would for payment recovery:


```text
val   params   =   InAppMessageParams.  newBuilder  ()
.  addInAppMessageCategoryToShow  (InAppMessageCategoryId.TRANSACTIONAL)
.  build  ()


billingClient.  showInAppMessages  (
activity,
params,
object   :   InAppMessageResponseListener   {
override   fun   onInAppMessageResponse  (result:   InAppMessageResult  ) {
when   (result.responseCode) {
InAppMessageResponseCode.NO_ACTION_NEEDED   ->   {
\  /  \  /   Nothing to show   this   session
}
InAppMessageResponseCode.SUBSCRIPTION_STATUS_UPDATED   ->   {
\  /  \  /   Payment fixed or price increase confirmed
refreshSubscriptionStatus  (result.purchaseToken)
}
}
}
}
)
```


The` SUBSCRIPTION_STATUS_UPDATED` response code covers both outcomes: payment recovery and price increase acceptance. The purchase token in the result tells you which subscription changed, and a refresh from your backend (or RevenueCat) tells you what the new state is. The two messages have different frequency caps. The price increase message shows at most once every 7 days, starting the first day the user can accept the new price. The payment issue message during grace period and account hold shows at most once per day. Either way, calling` showInAppMessages` on every app launch is safe, and the library returns` NO_ACTION_NEEDED` when there is nothing to show.


The practical impact is on the opt in price increase flow that previously required users to navigate to the Play Store subscription settings screen to accept a new price. Spoiler: most of them never made that trip, and their subscriptions auto canceled at the first renewal at the new price. The in app surface keeps the user inside your app and gives you a higher acceptance rate. If you have a planned price increase, adopt this first.


### Updated error code for a blocked Play Store


The second v9 change is a reclassification of one error code. The Play Store app can be blocked by the system. This happens in OEM customized kids modes, on managed devices with parental controls, and on enterprise devices with policies that disable the store. In v8 and earlier, billing calls in this state returned a` BillingResult` with response code` ERROR` and no specific debug message. v9 reclassifies these cases as` BILLING_UNAVAILABLE` and attaches a “Play Store is blocked” debug message.


The migration step is a one liner in your error handling code:


```text
when   (result.responseCode) {
BillingResponseCode.BILLING_UNAVAILABLE   ->   {
if   (result.debugMessage.  contains  (  "Play Store is blocked"  )) {
showBlockedStoreFallback  ()
}   else   {
showBillingUnavailableFallback  ()
}
}
BillingResponseCode.ERROR   ->   {
showGenericError  ()
}
}
```


Detecting this case requires` androidx.core` 1.9 or later, which most modern apps already pull transitively. If your app supports kids tablets or enterprise distribution, this reclassification lets you show a more specific message than a generic billing error and avoids a confusing dead end at the paywall.


### DeveloperProvidedBillingDetails.getLinkUri is now nullable


The third v9 change is a nullability adjustment on developer provided billing.` DeveloperProvidedBillingDetails.getLinkUri()` now returns` @Nullable` rather than non null. The direct link URI for external payments is not always available at the payment selection stage. It sometimes resolves later in the flow. The old non null guarantee forced the library to return an empty string in those cases, and apps that tried to parse the empty string as a URI crashed at the call to` Uri.parse` .


The migration step is to handle both null and empty string before launching a browser intent:


```text
val   linkUri   =   details.linkUri
if   (  !  linkUri.  isNullOrEmpty  ()) {
val   intent   =   Intent  (Intent.ACTION_VIEW, Uri.  parse  (linkUri))
startActivity  (intent)
}   else   {
showExternalPaymentUnavailableState  ()
}
```


If you do not use developer provided billing, this change has no effect on your code. If you do, the migration takes a single nullness check and a fallback branch.


### targetSdkVersion bumped to 35


v9 targets API 35 (Android 15). If your app builds against an older compile or target SDK and consumes the billing library directly, you may see manifest merger warnings until you bump your own` targetSdkVersion` to match. The library itself runs on the same` minSdk 23` floor that v8.1 established, so this is purely a target side bump.


## Analytics and AI powered insights


The analytics surface inside Play Console got the matching upgrade. The new metrics fall into three buckets. The first is reach measurement: total visibility, store listing indirect value, and traffic source breakdowns split across engagement, retention, and monetization. The second is conversion granularity: cart conversion rates, subscriber tenure distributions, and churn reason data. The third is AI assisted interpretation: chart descriptions on the Reach and Devices and Store Performance pages, interactive Q and A on metrics you select, and proactive monetization recommendations surfaced inline.


The interactive Q and A is the surface most likely to change how teams use Play Console day to day. Instead of clicking through to a docs page to understand why a metric moved, you can ask the question directly in the console and Gemini answers with the chart context already loaded. This collapses the loop from observation to hypothesis to corrective action.


The new churn reason data deserves separate attention. Cancellation surveys have always returned reason codes, but Play Console previously surfaced them only as a flat list. The v9 console adds tenure aware breakdowns, so you can see which reasons dominate at month one versus month twelve. If your top cancellation reason at month one is “too expensive” and your top reason at month twelve is “not using it anymore,” those map to two entirely different retention interventions.


## Protected with Play


The security side of I/O 2026 introduces Protected with Play, a centralized dashboard that consolidates integrity configuration, distribution defense, and monetization fraud controls into a single Play Console surface. The numbers Google published for the year are large: 160 million spam ratings and reviews blocked, and $3.2 billion in fraudulent and abusive transactions blocked automatically. The dashboard does not change those automated protections. What it changes is your ability to see them and configure where the thresholds sit for your app.


One performance related change is worth flagging. The Play Integrity API warm up latency has been reduced for latency sensitive user journeys. If you call Play Integrity at the start of a purchase flow, a sign in, or a checkout, the latency you pay for the integrity verdict is now lower. The reduction matters most for short sessions where every hundred milliseconds at the start of the flow risks losing the user.


## What v9 means for the v7 sunset


Google’s[Play Billing Library deprecation page](https://developer.android.com/google/play/billing/deprecation-faq) shows the August 31, 2026 deadline for new apps and updates built against v7. v7 binaries already published continue to function, but no new release built against v7 can be uploaded after that date. v9 does not change this calculation. If you are still on v7, the v9 migration guide is a single page that walks through every cumulative removal:` querySkuDetailsAsync` , the no argument` enablePendingPurchases` , the legacy` queryPurchasesAsync` overload, the user choice billing rename, and the v9 polish on top. The v7 to v8 work is the bulk of the effort. The additional v9 step is the four changes covered above and adopting the in app messaging extension for price increases.


> Still on v7? August 31, 2026 is your hard deadline. There is no shortcut from v7 straight to v9: you do the v8 work first. Pair this article with the[complete v7 to v8 migration guide](https://www.revenuecat.com/blog/engineering/play-billing-8-migration/) , which walks through the six migration steps, every removed API and its replacement, and the v8.1 to v8.3 additions you also pick up along the way. The four v9 changes covered above are what you tack on at the end.


For teams already on v8, the v9 jump is small enough to fit into a single sprint. The new in app messaging category is opt in, the error code reclassification only requires touching code that already handles` BILLING_UNAVAILABLE` , and the nullability change only affects developer provided billing integrations. The` targetSdkVersion` bump is the only forced change, and it is one line.


## Conclusion


In this article, you’ve explored the I/O 2026 wave of Play updates: Gemini powered discovery and Ask Play moving recommendations beyond the Play Store grid, Play Shorts and Sidekick reshaping on store and in game engagement, AI assisted localization and catalog management in the Play Console, three billing additions that target involuntary churn directly, the four code level changes that make up Play Billing Library 9.0.0, and the new analytics and security surfaces.


The release sits in clear contrast with v8: where v8 reshaped the API surface, v9 picks up a single new capability worth adopting and consolidates the v8 foundation. For the official source material, refer to Google’s[I/O 2026 Play announcement](https://android-developers.googleblog.com/2026/05/io-2026-whats-new-in-google-play.html) , the[Play Billing Library release notes](https://developer.android.com/google/play/billing/release-notes) , and the[Play Billing Library 9 migration guide](https://developer.android.com/google/play/billing/migrate-pbl-latest) .
