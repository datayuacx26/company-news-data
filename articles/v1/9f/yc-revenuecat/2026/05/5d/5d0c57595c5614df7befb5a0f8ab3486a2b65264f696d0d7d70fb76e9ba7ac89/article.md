---
schema_version: "1.0.0"
document_id: "5d0c57595c5614df7befb5a0f8ab3486a2b65264f696d0d7d70fb76e9ba7ac89"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/play-billing-8-migration"
published_at: "2026-05-08T00:41:42+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T22:15:23.630573+00:00"
content_hash: "sha256:447222d40443ff64d8585aaa3173b4e5baf3fe084b43b131105f8ddce6db258a"
---

# A complete guide to migrating from Google Play Billing v7 to v8 (and preparing for v9)

Every Android subscription team lives on the same two year clock. The Play Billing Library you shipped with last year is on a deprecation timer, and the Play Console will refuse new uploads the moment the timer runs out. v7 is the next version to hit that wall. After August 31, 2026, you can no longer publish a new app or update built against v7, and v8 has been generally available since June 30, 2025 with significant additions still arriving in v8.1 through v8.3. This guide walks you through the migration end to end, then sets you up for v9.


In this article, you’ll explore the v7 to v8 deprecation timeline and what each date actually means, the full list of removed APIs and their replacements, a step by step migration of the connection, query, and purchase flows, the new behaviors v8 adds that are worth adopting today, what v8.1 through v8.3 layer on top, and how to prepare for v9 before Google publishes a single line of its API surface.


## **The deprecation timeline that sets your deadline**


Google has run a two year deprecation cycle for the Play Billing Library across several major versions. Every major version gets two years of support after release, then the Play Console stops accepting new builds compiled against it. The deprecation FAQ publishes the exact dates:


Library version


Last date for new apps and updates


Extension request deadline


5.x


2024-08-31


2024-11-01


6.x


2025-08-31


2025-11-01


**7.x**


**2026-08-31**


**2026-11-01**


8.x


2027-08-31


2027-11-01


The phrasing in the FAQ is precise and worth reading carefully: “This deprecation prevents only new apps and updates from using older versions of the Play Billing Library. Existing apps that use a deprecated version of the library will continue to function as expected.”


That distinction matters. The deadline is a publishing gate, not a runtime kill switch. Already published v7 binaries keep transacting after August 31, 2026. What stops is your ability to ship a new release. If your app receives any updates at all, including security patches and bug fixes, you need to be on v8 before that gate closes. The extension request deadline of November 1, 2026 is for teams that need to file a formal request through the Play Console to keep shipping v7 builds for a brief additional window. It is not a free extension you receive automatically.


The Play Console signals this through a warning and an inbox message on the Policy status page once your app is on a deprecated version. If your` AndroidManifest.xml` does not contain the entry named` com.google.android.play.billingclient.version` , the Play Console cannot detect your library version, and the warning never reaches you. That entry is added automatically by the library, but manifest merging in multi module projects sometimes drops it. Verify it exists in your final merged manifest before you trust the absence of warnings.


## **What changed in v8: the removals at a glance**


v8 removed several APIs that had been deprecated for one or two prior versions. If your code still uses any of these, the v8 upgrade will fail to compile until you replace them.


Removed API


Replacement


` querySkuDetailsAsync()`


` queryProductDetailsAsync(QueryProductDetailsParams, ProductDetailsResponseListener)`


` queryPurchaseHistoryAsync()` (all overloads)


No direct client API. Track history server side.


` enablePendingPurchases()` (no arg)


` enablePendingPurchases(PendingPurchasesParams)`


` queryPurchasesAsync(String skuType, PurchasesResponseListener)`


` queryPurchasesAsync(QueryPurchasesParams, PurchasesResponseListener)`


` BillingClient.Builder.enableAlternativeBilling()`


` enableUserChoiceBilling(UserChoiceBillingListener)`


` AlternativeBillingListener`


` UserChoiceBillingListener`


` AlternativeChoiceDetails`


` UserChoiceDetails`


The signature of` ProductDetailsResponseListener.onProductDetailsResponse()` also changed. The callback now returns a` QueryProductDetailsResult` that splits the response into a fetched list and an unfetched list with per product status codes, replacing the older single list signature. Any class that implements this listener has to be updated, even if the rest of its logic is unchanged.


Two terminology changes accompany the API removals. “In app items” are now called “one time products” throughout the documentation and the API surface. The class names themselves did not change, the documentation simply uses the new term, but the rename carries a behavioral implication: one time products in v8 can have multiple purchase options and offers, the same way subscriptions did in v7.


## **Step 1: Bump the dependency and your minSdk**


The first change is the Gradle coordinate. v8.0.0 was the first release in the v8 line, but most teams should target the latest patch in the v8 series instead of pinning to 8.0.0:


```text
dependencies   {
val   billingVersion   =   "8.3.0"
implementation  (  "com.android.billingclient:billing:  $billingVersion  "  )
implementation  (  "com.android.billingclient:billing-ktx:  $billingVersion  "  )
}
```


The library raised its` minSdkVersion` to 23 starting at v8.1.0. If your app still supports API 21 or 22, you have two choices: pin to 8.0.0, which keeps the original v7 minSdk of 21, or raise your own minSdk to 23 and adopt the latest v8 patch. The latter is the right call for almost every team, since API 23 was released in 2015 and the device tail below it has been negligible for years. v8.1 is also built against Kotlin 2.2.0, which can affect projects on older Kotlin versions if you consume the` billing-ktx` artifact, though this is a small jump for any project already on a recent Kotlin Compose stack.


## **Step 2: Migrate enablePendingPurchases**


The no argument` enablePendingPurchases()` was deprecated in v7 and removed in v8. It used to be a single method that turned on pending purchase support for in app items implicitly. v8 replaces it with a builder that forces you to declare which product categories you want pending support for.


In v7, you had:


```text
val   billingClient   =   BillingClient.  newBuilder  (context)
.  setListener  (purchasesUpdatedListener)
.  enablePendingPurchases  ()
.  build  ()
```


In v8, the equivalent call is:


```text
val   pendingPurchasesParams   =   PendingPurchasesParams.  newBuilder  ()
.  enableOneTimeProducts  ()
.  build  ()


val   billingClient   =   BillingClient.  newBuilder  (context)
.  setListener  (purchasesUpdatedListener)
.  enablePendingPurchases  (pendingPurchasesParams)
.  build  ()
```


Notice that` .enableOneTimeProducts()` is not implicit. The deprecated zero argument version was, by Google’s own description, equivalent to a builder with` .enableOneTimeProducts()` set, so if you do not include this call you will silently lose pending purchase support for one time products. That regression is invisible in unit tests and in most countries with credit card based purchase flows. It surfaces as broken purchases in markets where users pay with cash at convenience stores or with bank transfers, primarily Japan, Germany, Brazil, Mexico, and Indonesia. Add` .enableOneTimeProducts()` even if you think you do not need it.


If you sell prepaid subscription top ups, also add` .enablePrepaidPlans()` to the builder. This was added in v7 specifically for prepaid plan pending transactions and carries over into v8 unchanged.


## **Step 3: Update queryProductDetailsAsync to the new callback shape**


` queryProductDetailsAsync` itself is not removed, but its callback was. The pre v8 listener received a` BillingResult` and a` List<ProductDetails>` . The v8 listener receives a` BillingResult` and a` QueryProductDetailsResult` that splits products into two lists. Any product that failed to fetch shows up in the unfetched list with its own status code, instead of being silently dropped.


The pre v8 callback looked like this:


```text
billingClient.  queryProductDetailsAsync  (params) { billingResult, productDetailsList   ->
if   (billingResult.responseCode   ==   BillingResponseCode.OK) {
productDetailsList.  forEach   { details   ->
renderProduct  (details)
}
}
}
```


The v8 version exposes both the successful fetches and the failures:


```text
billingClient.  queryProductDetailsAsync  (params) { billingResult, queryProductDetailsResult   ->
if   (billingResult.responseCode   ==   BillingResponseCode.OK) {
queryProductDetailsResult.productDetailsList.  forEach   { details   ->
renderProduct  (details)
}
queryProductDetailsResult.unfetchedProductList.  forEach   { unfetched   ->
logUnfetched  (unfetched.productId, unfetched.statusCode)
}
}
}
```


The reason this matters is that pre v8, if you queried five products and one of them was misconfigured in the Play Console, the whole call could return successfully with that product silently missing from the list. Your paywall would render four products and you would have no idea the fifth was even attempted. The new shape makes the partial failure explicit. Each unfetched entry carries a status code that tells you whether the product is unavailable in the user’s country, has not yet been published, or hit a transient error.


This is also the moment to rebuild any abstraction layer that wraps` queryProductDetailsAsync` in a coroutine or flow. The return type of your suspending wrapper has to change from` List<ProductDetails>` to a sealed class or pair that carries the unfetched information forward. If you have a typealias for the listener type itself, the typealias has to be updated, since the lambda signature is different.


## **Step 4: Replace the deprecated queryPurchasesAsync overload**


v8 removed the overload of` queryPurchasesAsync` that took a raw SKU type string. The replacement uses a` QueryPurchasesParams` builder that wraps the same product type with the new typed enum.


Before:


```text
billingClient.  queryPurchasesAsync  (BillingClient.SkuType.SUBS) { result, purchases   ->
handlePurchases  (result, purchases)
}
```


After:


```text
val   params   =   QueryPurchasesParams.  newBuilder  ()
.  setProductType  (BillingClient.ProductType.SUBS)
.  build  ()


billingClient.  queryPurchasesAsync  (params) { result, purchases   ->
handlePurchases  (result, purchases)
}
```


The behavior is identical when invoked correctly. The` SkuType` constants are gone, replaced by` ProductType.SUBS` and` ProductType.INAPP` . If you have a single shared method that accepts a string for the SKU type, refactor it to accept a` String` that matches the new enum values, or convert it to use` BillingClient.ProductType` directly.


## **Step 5: queryPurchaseHistoryAsync has no client side replacement**


` queryPurchaseHistoryAsync` was deprecated in v7 and fully removed in v8. There is no equivalent client API. This is the only v8 removal that does not have a direct one to one replacement, and it tends to surprise teams the most.


Google’s reasoning is straightforward, even if undocumented in the migration page itself: purchase history is not authoritative source of truth on the client. Any history a client builds up can be incomplete, since the device may have been offline during a refund or revocation. The Play Developer API on your server has the authoritative record, and any analytics, audit, or reconciliation flow that depends on knowing past purchases should query it server side instead.


If you previously used` queryPurchaseHistoryAsync` for one of these reasons, you now have two paths. The first is to call the Play Developer API endpoint` purchases.subscriptionsv2.get` from your backend whenever you need to inspect a subscription’s full history, including its` linkedPurchaseToken` chain. The second is to lean on Real Time Developer Notifications and persist the events as they arrive, building your own purchase history table. Most teams already do the latter for analytics and revenue reporting, in which case the v8 removal is a no op.


If you used` queryPurchaseHistoryAsync` for entitlement restoration on a fresh install or after a reinstall, switch to` queryPurchasesAsync` with the new params builder.` queryPurchasesAsync` returns active purchases tied to the current Google account, which is what restoration actually needs. Active purchases is a smaller set than full history, but it is the correct set for entitlement granting. Note that v8 also stops returning consumed one time products and expired subscriptions from` queryPurchasesAsync` , so configure your one time products as non consumable in the Play Console if you need them to survive a reinstall.


## **Step 6: Move from alternative billing to user choice billing**


v7 already deprecated the` enableAlternativeBilling` builder method, and v8 removed it entirely. The replacement is` enableUserChoiceBilling` , which takes a` UserChoiceBillingListener` instead of an` AlternativeBillingListener` and exposes a` UserChoiceDetails` payload instead of` AlternativeChoiceDetails` .


```text
val   billingClient   =   BillingClient.  newBuilder  (context)
.  setListener  (purchasesUpdatedListener)
.  enableUserChoiceBilling   { userChoiceDetails   ->
recordExternalSelection  (userChoiceDetails)
}
.  enablePendingPurchases  (pendingPurchasesParams)
.  build  ()
```


The functional contract is the same: when the user picks your billing system over Google Play’s at checkout, the listener fires with the purchase details so your backend can complete the transaction outside of Google Play’s payment flow. Only the type names changed. If you carry a per region build flavor where user choice billing is enabled in some regions and disabled in others, the build flavor logic does not need to change, only the type references.


## **One time products and the multi offer surface**


v8 renamed in app items to one time products and gave them the same multi offer capability subscriptions had since v5. A single one time product can now carry multiple purchase options, each with its own price, regional availability, and offers attached. This is the change that motivates the bulk of v8’s surface, and it is also the reason the unfetched product list now exists. With multiple offers per product, the partial failure modes multiplied, and a flat list could no longer represent what happened.


The class names did not change.` ProductDetails` is still` ProductDetails` . What changed is the` oneTimePurchaseOfferDetails` accessor, which can now expose multiple offers and pre-order details starting at v8.1.0. If your paywall renders a single price for an in app item, you are still on the v7 mental model. v8 lets you expose a discounted offer or a regional offer on the same product without creating a duplicate SKU. This is opt in: existing one time products continue to surface a single offer until you configure additional options in the Play Console.


## **New behaviors worth adopting in v8**


Two of v8’s additions are not strictly required but are worth turning on at migration time, since they reduce code you would otherwise carry yourself.


The first is` enableAutoServiceReconnection()` . The Play Billing service can disconnect for a number of reasons: the user installs an update to Google Play, the device wakes from a long sleep, the system reclaims the bound service. Pre v8, you had to implement` onBillingServiceDisconnected()` and call` startConnection()` again with appropriate backoff. v8 ships an opt in builder method that handles this for you:


```text
val   billingClient   =   BillingClient.  newBuilder  (context)
.  setListener  (purchasesUpdatedListener)
.  enablePendingPurchases  (pendingPurchasesParams)
.  enableAutoServiceReconnection  ()
.  build  ()
```


When auto reconnection is enabled, your` onBillingServiceDisconnected()` override can be a no op, and the library handles retry with backoff internally. Auto reconnection is opt in rather than the default. If you want to manage connection lifecycle yourself, leave it disabled and continue handling` onBillingServiceDisconnected()` directly.


The second addition is` BillingResult.subResponseCode` . Pre v8, the` BillingResult` returned for purchase failures grouped many cases under` BILLING_UNAVAILABLE` or` ERROR` . v8 adds a sub response code on` BillingResult` that distinguishes specific failure cases:


- ` PAYMENT_DECLINED_DUE_TO_INSUFFICIENT_FUNDS` : the user’s payment method does not have sufficient funds.
- ` USER_INELIGIBLE` : the user is not eligible for the offer they tried to redeem, typically because they have already used a one time intro offer.
- ` NO_APPLICABLE_SUB_RESPONSE_CODE` : the failure does not map to a more specific sub code.


The ineligibility code is the one you almost certainly want to handle, because it lets you steer the user back to a fallback offer instead of leaving them at a failed purchase screen. The sub code arrives in the` BillingResult` delivered to your` PurchasesUpdatedListener.onPurchasesUpdated` callback:


```text
override   fun   onPurchasesUpdated  (result:   BillingResult  , purchases:   List  <  Purchase  >?) {
if   (result.responseCode   ==   BillingResponseCode.ITEM_UNAVAILABLE) {
when   (result.subResponseCode) {
SubResponseCode.USER_INELIGIBLE   ->   showFallbackOffer  ()
SubResponseCode.PAYMENT_DECLINED_DUE_TO_INSUFFICIENT_FUNDS   ->   showFundsError  ()
else   ->   showGenericError  ()
}
}
}
```


## **What v8.1, v8.2, and v8.3 add on top**


v8 has been moving fast since GA. Three minor releases between November and December 2025 added significant capabilities that are easy to miss if you only read the v8.0 migration guide.


v8.1.0, released November 6, 2025, introduced suspended subscription handling. A subscription enters the suspended state when Google detects abuse or a payment dispute and pauses delivery of entitlements without canceling the subscription. Pre v8.1, suspended subscriptions were invisible to client side queries. v8.1 adds an` isSuspended` flag on` Purchase` and a parameter to` queryPurchasesAsync` to include suspended purchases in the result. The pattern is to filter purchases by` isSuspended` before granting entitlement, and route suspended ones into a separate UI branch that explains the hold to the user:


```text
val   params   =   QueryPurchasesParams.  newBuilder  ()
.  setProductType  (BillingClient.ProductType.SUBS)
.  build  ()


billingClient.  queryPurchasesAsync  (params) { result, purchases   ->
purchases.  forEach   { purchase   ->
if   (purchase.isSuspended) {
showSuspendedState  ()
}   else   if   (purchase.purchaseState   ==   Purchase.PurchaseState.PURCHASED) {
grantEntitlement  (purchase)
}
}
}
```


v8.1 also reworks subscription replacement. The old` SubscriptionUpdateParams.setSubscriptionReplacementMode()` is now deprecated in favor of` SubscriptionProductReplacementParams` set on the per product builder. The new shape supports a` KEEP_EXISTING` replacement mode that lets you upsell a second subscription on the same account without canceling the existing one, which is useful for bundled offerings.


v8.2.0 and v8.2.1, released in December 2025, generalize what used to be the External Offers Program into a broader Billing Program surface. The old methods (` enableExternalOffer` ,` isExternalOfferAvailableAsync` ,` createExternalOfferReportingDetailsAsync` ,` showExternalOfferInformationDialog` ) are deprecated and replaced by` enableBillingProgram(int)` ,` isBillingProgramAvailableAsync` ,` createBillingProgramReportingDetailsAsync` , and` launchExternalLink` . v8.3.0, shipped December 23, 2025, adds the developer provided billing flow on top, with` BillingProgram.EXTERNAL_PAYMENTS` ,` EnableBillingProgramParams` , and` DeveloperBillingOptionParams` . This consolidation is Google’s response to the regulatory pressure that emerged from the Epic Games ruling and similar requirements in other jurisdictions, where Google is required to support developer provided billing and out of app payment links.


If your app uses the External Offers Program today, the deprecation is a clear signal to start migrating. The methods still work in v8.2 and v8.3, but they will likely be removed in v9. Migrating to the Billing Program surface now is a one time cost that buys you out of a forced migration later.


## **Preparing for v9 before its API surface exists**


At the moment, the release notes top out at v8.3.0. The deprecation FAQ does not list a v9 row. There is no public alpha, beta, or release candidate. So how do you prepare?


By understanding the cadence and the direction of change. Google has shipped a new major version of the Play Billing Library roughly once a year, with v7 released in May 2024 and v8 reaching GA on June 30, 2025. On that pattern, v9 is expected to land in mid 2027, lining up with the v8 sunset deadline of August 31, 2027.


The direction of change is also predictable from the rapid v8.1 through v8.3 releases. Google is consolidating the external billing surface (External Offers Program, user choice billing, developer provided billing) into a unified Billing Programs API. v9 is likely to remove the deprecated External Offers methods, formalize the Billing Programs surface, and possibly extend it further to handle additional regulatory regimes.


Three things you can do today to be ready:


1. **Opt in early** : adopt every opt in API v8 ships, including` enableAutoServiceReconnection()` and the new` SubscriptionProductReplacementParams` . The opt in surface in one major version is the default surface in the next.
2. **Migrate off External Offers** : the Billing Programs surface in v8.2 and v8.3 is the forward path, and it gives you a non breaking migration window before v9 forces it.
3. **Move history server side** : v8 removed` queryPurchaseHistoryAsync` for a reason, and v9 will not reintroduce it. Build your purchase audit trail server side from RTDN events and the Play Developer API.


If you do these three things in your v8 migration, the v9 migration in mid 2027 becomes incremental rather than disruptive.


## **How RevenueCat absorbs the v7 to v8 transition**


The recurring nature of Play Billing Library migrations is the kind of work that taxes a small team disproportionately. Every two years, someone has to read the migration guide, refactor the connection layer, retest pending purchases in cash payment markets, audit the` queryPurchaseHistoryAsync` removal, and verify that no listener signature change broke a downstream module. The work is rarely interesting, and it always lands on the team that least wants to do it.


RevenueCat absorbs this migration on your behalf. The RevenueCat Android SDK 9.x ships with Play Billing Library 8 already integrated, and the SDK API surface most apps interact with did not change between RC SDK 8.x and 9.x. The only meaningful changes for app code are a Kotlin minimum of 1.8.0 and the removal of` data class` modifiers from a handful of public types, which means` copy()` and component destructuring no longer work on those types.` equals()` and` hashCode()` are preserved. The migration path is documented in the[RevenueCat 8.x to 9.x Android migration guide](https://www.revenuecat.com/docs/sdk-guides/android-native-8x-to-9x-migration) , and the rationale and feature set are walked through in the[Play Billing Library 8 support in Purchases SDK v9.0.0](https://www.revenuecat.com/blog/engineering/google-play-billing-v8/) engineering post.


The practical implication is that the v7 to v8 migration steps in this article reduce to: bump your RevenueCat SDK from 8.x to 9.x, bump your Kotlin to 1.8 or later, replace any` copy()` or destructuring on the affected RC types with explicit constructors, and verify your one time products are configured as non consumables in the RevenueCat dashboard (since v8 cannot query consumed one time products). That is the entire migration. The v8 specific work, including the new` PendingPurchasesParams` builder, the` queryProductDetailsAsync` callback shape, the` queryPurchasesAsync` overload swap, the user choice billing rename, and the unfetched product handling, is already done inside the SDK.


The same property holds for v9. When v9 ships, RevenueCat will release a new SDK that integrates it, and your app code moves forward with the same pattern: bump the SDK, possibly bump your Kotlin or minSdk, and run your test suite. The recurring Play Billing Library migration stops being something your team has to schedule.


## **Conclusion**


In this article, you’ve explored the full v7 to v8 migration: the deprecation timeline that sets your August 31, 2026 deadline, the removed APIs and their replacements, the six concrete migration steps from Gradle bump through user choice billing, the new behaviors v8 ships with` enableAutoServiceReconnection` and` BillingResult.subResponseCode` , the three minor releases that added suspended subscriptions and the unified Billing Programs surface, and a pattern based read on what v9 will likely require.


Understanding the structural reasons behind each removal helps you make better decisions during the migration itself.` queryPurchaseHistoryAsync` is gone because purchase history was never authoritative on the client. The new` enablePendingPurchases` builder is explicit because the implicit version silently regressed cash payment flows in some markets. The unfetched product list exists because partial failures had to become visible once one time products gained multiple offers. Each change reflects a problem the prior surface failed to handle, and the v9 changes will follow the same logic.


Whether you’re migrating to v8 this quarter, planning the v9 transition for 2027, or evaluating whether to outsource billing infrastructure to a platform like RevenueCat, this foundation lets you ship Android subscriptions without losing weeks every two years to a forced upgrade. For the official source material, refer to Google’s[migrating to Play Billing Library 8](https://developer.android.com/google/play/billing/migrate-gpblv8) page, the[deprecation FAQ](https://developer.android.com/google/play/billing/deprecation-faq) , and the[release notes](https://developer.android.com/google/play/billing/release-notes) .
