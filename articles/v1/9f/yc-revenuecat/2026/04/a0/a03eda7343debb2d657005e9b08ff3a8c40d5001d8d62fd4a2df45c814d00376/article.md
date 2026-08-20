---
schema_version: "1.0.0"
document_id: "a03eda7343debb2d657005e9b08ff3a8c40d5001d8d62fd4a2df45c814d00376"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/cmp-subscriptions"
published_at: "2026-04-30T01:40:16+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T22:15:31.883696+00:00"
content_hash: "sha256:6080e6a451925fb9f12f65fadc7975df881945b407fa80a9f912d6c4c15ded6b"
---

# Compose Multiplatform subscriptions: single codebase for iOS and Android

You ship a subscription app on Android and your team starts the iOS port. Suddenly you are maintaining two paywall implementations for Android and iOS, two billing integrations, and two sets of receipt verification code with different APIs and different bugs. RevenueCat’s[purchases-kmp SDK](https://github.com/RevenueCat/purchases-kmp) collapses that duplication. You write your subscription logic once in` commonMain` , the SDK wraps Google Play Billing on Android and StoreKit on iOS, and a Compose Multiplatform paywall component renders the same UI on both platforms.


In this article, you’ll set up a Kotlin Multiplatform project with the RevenueCat KMP SDK, configure dashboard products and entitlements, initialize Purchases on Android and iOS, gate premium content from common code, run an in app purchase from` commonMain` , and drop in a server driven paywall built with the dashboard’s Paywall Editor. You’ll work directly with the same source layout used by[cat-paywalls-kmp](https://github.com/RevenueCat/cat-paywalls-kmp) , the official KMP demo app.


## **What you’ll build**


You’ll end up with a Compose Multiplatform app that lists premium articles, fades the body until the user is entitled, opens a server driven paywall, runs the purchase through the platform native dialog, and refreshes the entitlement state. The same screen runs on both iPhone and a Pixel without a single line of duplicated UI code.


The repository structure is a normal multi module KMP project: a` composeApp` module with` commonMain` ,` androidMain` , and` iosMain` source sets, a few` feature` modules (home, article, paywalls, subscriptions), and` core` modules for data, network, and design system. Every line of subscription logic in this tutorial lives in` commonMain` .


## **Why single codebase changes the math**


Building cross-platform subscriptions without a shared SDK means writing every subscription concept twice. You query Google Play with` BillingClient` and a purchase token, and you query StoreKit with` Product.products(for:)` and a signed JWS. You verify receipts with two completely separate server APIs, store them in two different shapes, and reconcile them with a backend mapping layer. None of this work is the part of your product that users care about.


The RevenueCat KMP SDK gives you four shared concepts that hide all of that:


- **Offerings** are the set of products you currently sell, configured in the dashboard rather than in your app.
- **Packages** are the buyable units inside an offering (monthly, annual, lifetime).
- **Entitlements** are the access levels your app cares about (` premium` ,` pro` ,` family` ), independent of which store the user paid through.
- **CustomerInfo** is a single object that aggregates every active entitlement for the current user, regardless of platform.


Once you have these four objects, your app stops asking “did this user buy through Google Play or App Store?” and starts asking “is` customerInfo.entitlements\["premium"\]` active?” That single property check works the same on iOS and Android.


## **Prerequisites and dashboard setup**


Before any code, you’ll set up four things in the[RevenueCat dashboard](https://app.revenuecat.com/) :


1. A **project** with one Android app and one iOS app, each linked to its store credentials.
2. **Products** imported from Google Play Console and App Store Connect.
3. An **entitlement** named` premium` (or whatever identifier you prefer).
4. An **offering** with one or more packages attached, marked as the current offering.


Two codelabs walk through the dashboard side end to end. If you have not configured products yet, go through them first:


- [RevenueCat Google Play Integration](https://revenuecat.github.io/codelabs/google-play.html)
- [RevenueCat App Store Integration](https://revenuecat.github.io/codelabs/app-store.html)


The two important screens to land on are the entitlement and the offering. The entitlement is the unit your app code checks:


The offering is what your app fetches at runtime. It bundles the packages you want this version of the app to display, and you can change its contents without shipping a new build:


After both apps are set up, copy the public SDK API keys from **Project Settings > API Keys** . There is one key per platform, and you will use both of them in Step 2.


## **Step 1: Add the RevenueCat KMP SDK**


The KMP SDK ships as two artifacts:` purchases-kmp-core` for the subscription logic and` purchases-kmp-ui` for the Compose Multiplatform paywall component. Both are published to Maven Central with a single version coordinate of the form` <sdk>+<hybrid-common>` . The number after the plus sign matters when you link the iOS pod, so do not strip it.


Start by adding the version and library entries to` gradle/libs.versions.toml` :


```text
[versions]
purchases  -  kmp   =   "2.10.2+17.55.1"


[libraries]
purchases  -  kmp  -  core   =   { module   =   "com.revenuecat.purchases:purchases-kmp-core"  , version.ref   =   "purchases-kmp"   }
purchases  -  kmp  -  ui     =   { module   =   "com.revenuecat.purchases:purchases-kmp-ui"  ,   version.ref   =   "purchases-kmp"   }
```


Then declare the dependencies inside the` commonMain` source set of your app module’s` build.gradle.kts` . In the cat-paywalls-kmp demo this lives in` composeApp/build.gradle.kts` :


```text
kotlin   {
sourceSets   {
commonMain.  dependencies   {
\  /  \  /   RevenueCat
implementation  (libs.purchases.kmp.core)
\  /  \  /   Compose Multiplatform paywall component
implementation  (libs.purchases.kmp.ui)
}
}
}
```


Notice that both Android and iOS pull the same` commonMain` dependency. There is no` androidMain.dependencies { implementation("com.revenuecat...") }` block. The KMP artifact carries platform specific bindings inside it.


### **Linking the iOS native framework**


The KMP SDK depends on` PurchasesHybridCommon` , a native iOS framework that wraps StoreKit. The cleanest way to bring it in is through the Kotlin CocoaPods plugin. Apply the plugin to your` composeApp` module:


```text
plugins   {
alias  (libs.plugins.kotlin.multiplatform)
alias  (libs.plugins.compose.multiplatform)
alias  (libs.plugins.compose.compiler)
kotlin  (  "native.cocoapods"  )
}
```


Then declare both pods inside the` kotlin { cocoapods { ... } }` block. Pin the pod version to the same major as your` purchases-kmp` artifact so the Kotlin and Swift sides agree on protocol shapes:


```text
kotlin   {
cocoapods   {
summary   =   "Cat Paywalls KMP App"
homepage   =   "<https:  \/\/  github.com  \/  revenuecat  \/  cat-paywalls-kmp>"
version   =   "1.0"
ios.deploymentTarget   =   "15.0"
podfile   =   project.  file  (  "..  \/  iosApp  \/  Podfile"  )


framework   {
baseName   =   "ComposeApp"
isStatic   =   true
}


pod  (  "RevenueCat"  ) {
version   =   "~> 5.21"
extraOpts   +=   listOf  (  "-compiler-option"  ,   "-fmodules"  )
}
pod  (  "RevenueCatUI"  ) {
version   =   "~> 5.21"
extraOpts   +=   listOf  (  "-compiler-option"  ,   "-fmodules"  )
}
}
}
```


Run` ./gradlew podInstall` once. Gradle generates a` .podspec` for your shared module and writes a` Podfile.lock` next to your iOS app. From now on, opening the iOS workspace in Xcode pulls everything down through CocoaPods.


The KMP SDK uses Kotlin/Native interop bindings that are still flagged as experimental, so opt in inside any iOS source set that touches them:


```text
kotlin   {
sourceSets   {
all   {
languageSettings   {
if   (name.  startsWith  (  "ios"  )) {
optIn  (  "kotlinx.cinterop.ExperimentalForeignApi"  )
}
}
}
}
}
```


That is the entire setup. No platform-specific source files yet. Everything else lives in` commonMain` .


## **Step 2: Initialize Purchases on each platform**


` Purchases` is a singleton. You configure it once early in the app lifecycle, then every other call goes through` Purchases.sharedInstance` . Initialization is the only place where Android and iOS code differ, and only because each platform has its own application entry point.


On Android, initialize from your` Application.onCreate` . The cat-paywalls-kmp demo does this in` CatArticlesApplication` :


```text
class   CatArticlesApplication   :   Application  () {


override   fun   onCreate  () {
super  .  onCreate  ()


Purchases.logLevel   =   LogLevel.DEBUG
Purchases.  configure  (
PurchasesConfiguration  (apiKey   =   REVENUECAT_ANDROID_API_KEY) {
appUserId   =   null   \  /  \  /   Anonymous user
},
)
}


companion   object   {
private   const   val   REVENUECAT_ANDROID_API_KEY   =   "your_android_api_key"
}
}
```


The` PurchasesConfiguration(apiKey) { ... }` builder is a small DSL. Inside the trailing lambda you can set` appUserId` ,` purchasesAreCompletedBy` ,` verificationMode` , and other options. Passing` appUserId = null` tells the SDK to generate an anonymous identifier in the form` $RCAnonymousID:<uuid>` . When the user later signs in to your backend, you call` Purchases.sharedInstance.logIn(userId)` and RevenueCat transfers any purchases to that account.


Do not forget to register the` Application` class and the` INTERNET` permission in` AndroidManifest.xml` :


```text
<  application
android  :  name  =  ".CatArticlesApplication"
android  :  label  =  "@string\/app_name"  >
<  activity   android  :  name  =  ".MainActivity"   android  :  exported  =  "true"  >
<  intent-filter  >
<  action   android  :  name  =  "android.intent.action.MAIN"   \/>
<category   android  :  name  =  "android.intent.category.LAUNCHER"   \/>
<\/intent-filter>
<\/activity>
<\/application>


<uses-permission   android  :  name  =  "android.permission.INTERNET"   \/>
```


On iOS, initialize from your SwiftUI` App` struct. You import the native` RevenueCat` pod here, not the KMP wrapper, because configuration runs in Swift before the Kotlin runtime starts:


```text
import   SwiftUI
import   RevenueCat


@main
struct   iosAppApp  :   App   {


init  () {
Purchases.logLevel   =   .debug
Purchases.  configure  (  withAPIKey  :   "your_ios_api_key"  )
}


var   body:   some   Scene {
WindowGroup   {
ContentView  ()
}
}
}
```


` ContentView` then hosts the shared Compose surface inside a` UIViewControllerRepresentable` :


```text
struct   ComposeView  :   UIViewControllerRepresentable   {
func   makeUIViewController  (  context  : Context)   ->   UIViewController {
MainViewControllerKt.  MainViewController  ()
}


func   updateUIViewController  (  _   uiViewController: UIViewController,   context  : Context) {}
}
```


The Kotlin side of that bridge is a one liner in` iosMain` :


```text
fun   MainViewController  ()  :   UIViewController   {
val appGraph   =   createGraph  <  AppGraph  >  ()
return   ComposeUIViewController   {
App  (appGraph   =   appGraph)
}
}
```


` App` is the same` @Composable` you call from` MainActivity` on Android. From this point on, every screen you build runs on both platforms.


The reason initialization is split is that` Purchases.configure` reaches into the platform billing client immediately. On Android it asks` BillingClient` to open a connection. On iOS it registers a StoreKit transaction listener. Both need to happen before any Compose composition starts, which is why you do not configure inside` commonMain` .


## **Step 3: Check entitlements from common code**


Once` Purchases` is configured, you can ask “is the current user entitled to premium?” from anywhere in` commonMain` . The KMP SDK exposes coroutine friendly suspend variants of every callback API. The one you want here is` awaitCustomerInfo()` :


```text
suspend   fun   isPremium  ():   Boolean   {
val   customerInfo   =   Purchases.sharedInstance.  awaitCustomerInfo  ()
return   customerInfo.entitlements[  "premium"  ]?.isActive   ==   true
}
```


` CustomerInfo.entitlements` is a map keyed by the entitlement identifier you set up in the dashboard. The value is an` EntitlementInfo` with an` isActive` flag, an` expirationDate` , a` willRenew` boolean, and a` store` enum that tells you which platform the underlying purchase came from. For access decisions, only` isActive` matters. The entitlement is active whether the user subscribed through the App Store, Google Play, Stripe, or a promotional grant from your support team.


Most apps wrap this in a repository so the rest of the app can collect a` Flow` and forget about callbacks. The cat-paywalls-kmp demo defines` PaywallsRepository` in` core/data` :


```text
interface   PaywallsRepository   {
fun   fetchOffering  ():   Flow  <  Result  <  Offering  >>
fun   fetchCustomerInfo  ():   Flow  <  Result  <  CustomerInfo  >>
fun   awaitPurchase  (packageId:   String  ):   Flow  <  Result  <  StoreTransaction  >>
}
```


The` fetchCustomerInfo` implementation is a thin Flow wrapper around the SDK call, with` Dispatchers.IO` to keep network work off the main thread:


```text
override   fun   fetchCustomerInfo  ():   Flow  <  Result  <  CustomerInfo  >>   =   flow   {
try   {
val   customerInfo   =   Purchases.sharedInstance.  awaitCustomerInfo  ()
emit  (Result.  success  (customerInfo))
}   catch   (e:   Exception  ) {
emit  (Result.  failure  (e))
}
}.  flowOn  (Dispatchers.IO)
```


A ViewModel collects this flow and exposes a` StateFlow<CustomerInfo?>` :


```text
class   CatArticlesDetailViewModel  (
articleId:   Long  ,
articlesRepository:   ArticlesRepository  ,
paywallsRepository:   PaywallsRepository  ,
) :   ViewModel  () {


val   customerInfo:   StateFlow  <  CustomerInfo  ?>   =
paywallsRepository.  fetchCustomerInfo  ()
.  map   { it.  getOrNull  () }
.  stateIn  (
scope   =   viewModelScope,
started   =   SharingStarted.  WhileSubscribed  (  5000  ),
initialValue   =   null  ,
)
}
```


The Compose Multiplatform UI then reads it like any other state and decides whether to render the article body or fade it behind a paywall prompt:


```text
private   const   val   ENTITLEMENT_PREMIUM   =   "premium"


@Composable
private   fun   CatArticlesDetailContent  (
article:   Article  ,
viewModel:   CatArticlesDetailViewModel  ,
navigateToPaywalls: ()   ->   Unit,
) {
val   customerInfo   by   viewModel.customerInfo.  collectAsState  ()
val   isEntitled   =   customerInfo?.entitlements?.  get  (ENTITLEMENT_PREMIUM)?.isActive   ==   true


DetailsContent  (
article   =   article,
onJoinClicked   =   navigateToPaywalls,
isEntitled   =   isEntitled,
)
}
```


` DetailsContent` applies a` fadingEdge` modifier when` isEntitled` is false. Subscribed users see the article through to the end. Free users see the first few paragraphs fade into a “Join Now” CTA. The same composable runs on both iOS and Android.


## **Step 4: Run a purchase from` commonMain`**


Triggering a purchase is two suspend calls: fetch the current offering, then call` awaitPurchase` with one of its packages. The SDK takes care of opening Google Play’s billing dialog or StoreKit’s purchase sheet, validating the receipt with the store, and posting it to RevenueCat for tracking.


The` awaitPurchase` helper from` purchases-kmp-core` accepts a` Package` :


```text
suspend   fun   purchaseMonthly  () {
val   offerings   =   Purchases.sharedInstance.  awaitOfferings  ()
val   current   =   offerings.current ?:   error  (  "No current offering configured"  )


val   monthly   =   current.monthly
?: current.availablePackages.  first  ()


val   transaction   =   Purchases.sharedInstance.  awaitPurchase  (monthly)


\  /  \  /   The SDK has already verified the receipt and updated CustomerInfo.
\  /  \  /   Your existing customerInfo flow will emit the new state on its own.
}
```


` Offering` exposes convenience accessors for common cadences (` monthly` ,` annual` ,` lifetime` ) and a generic` availablePackages: List<Package>` if you want to build a custom plan picker. The cat-paywalls-kmp demo wraps this in` PaywallsRepository.awaitPurchase(packageId)` :


```text
override   fun   awaitPurchase  (packageId:   String  ):   Flow  <  Result  <  StoreTransaction  >>   =   flow   {
try   {
val   offerings   =   Purchases.sharedInstance.  awaitOfferings  ()
val   pkg   =   offerings.current?.availablePackages?.  find   { it.identifier   ==   packageId }
?:   error  (  "Package not found:   $packageId  "  )


val   transaction   =   Purchases.sharedInstance.  awaitPurchase  (pkg)
emit  (Result.  success  (transaction))
}   catch   (e:   Exception  ) {
emit  (Result.  failure  (e))
}
}.  flowOn  (Dispatchers.IO)
```


Two things to know about the return value. First, the` StoreTransaction` is informational only. The receipt has already been validated server side by the time` awaitPurchase` resumes, and the user’s` CustomerInfo` has been updated on RevenueCat’s backend. Second, the next call to` awaitCustomerInfo()` will reflect the new entitlement, which means any UI bound to your` customerInfo` flow recomposes automatically. You do not need to manually invalidate state.


If the user cancels the dialog, the SDK throws a` PurchasesException` whose` code` is` PurchaseCancelled` . Catch it and treat it as a no op rather than an error.


## **Step 5: Drop in a server driven paywall**


You could build the package picker yourself, but RevenueCat’s Paywall Editor lets you design the entire screen in the dashboard and update it without shipping a new build. The` purchases-kmp-ui` artifact includes a` Paywall` composable that renders the configured paywall on both Android and iOS.


You design the paywall in the dashboard’s visual editor:


In your KMP code, the entire paywall screen is one` Paywall` call. The cat-paywalls-kmp demo lives in` feature/paywalls/CatCustomPaywalls.kt` :


```text
@Composable
fun   CatCustomPaywalls  () {
val   composeNavigator   =   currentComposeNavigator


Box  (
modifier   =   Modifier
.  fillMaxSize  ()
.  background  (Color.White),
) {
Paywall  (
options   =   PaywallOptions  (
dismissRequest   =   { composeNavigator.  navigateUp  () },
),
)
}
}
```


` PaywallOptions` is where you pass callbacks for dismissal, purchase completion, and restore. With no offering passed, the component renders the **current offering** assigned to the user in the dashboard. If you are running an A/B test, RevenueCat picks the variant for this user automatically and attributes any conversion to the right experiment arm.


This is the part that pays for itself. The entire visual design of the paywall, including which packages to show, how to highlight the recommended plan, what copy to use for the trial CTA, and which offering to display, is configurable from the dashboard. You can tweak headline copy or swap a one screen layout for a feature comparison layout in the morning, watch conversion metrics in the afternoon, and revert by clicking a button if the new variant underperforms. None of this requires a Play Store or App Store review cycle.


## **Putting it all together: the architecture**


Here is how all the pieces line up in a finished KMP project. In code, the cat-paywalls-kmp demo organizes responsibilities by module:


- **` composeApp`** is the only module with platform specific code. Its` androidMain` configures` Purchases` from the` Application` , its` iosMain` exposes a` MainViewController()` to SwiftUI, and its` commonMain` holds` App.kt` plus a Navigation Compose` NavHost` .
- **` feature/*`** modules are screens (home, article, paywalls, account, subscriptions). They depend on` core/*` and contain Compose Multiplatform UI plus ViewModels.
- **` core/data`** owns` PaywallsRepository` , the only place that calls into` Purchases.sharedInstance` . Everything else reads` Flow<Offering>` and` Flow<CustomerInfo>` from this repository.
- **` core/model`** , **` core/network`** , **` core/designsystem`** , **` core/navigation`** hold the data classes, Ktor client, theme, and navigation graph respectively.


The final UX you ship looks like this. Subscribed users see the full article and a subscription management screen sourced from the same` CustomerInfo` object:


The same` SubscriptionManagementScreen` reads` customerInfo.activeSubscriptions` ,` customerInfo.entitlements\["premium"\]?.isActive` , and` customerInfo.originalAppUserId` to render its content, and runs without modification on both platforms. There is no` if (Build.VERSION...)` and no` #if os(iOS)` . The code lives once, in` commonMain` , and the SDK does the platform translation underneath.


If you want to inspect the full source as a reference, every file shown in this article is in[cat-paywalls-kmp](https://github.com/RevenueCat/cat-paywalls-kmp) . The minimum surface to get a working integration is the four steps above plus the dashboard setup. Everything else is product specific UI.


## **Conclusion**


In this article, you’ve configured the RevenueCat KMP SDK in a Compose Multiplatform project, initialized` Purchases` on Android and iOS, gated a premium screen with` CustomerInfo.entitlements` , run a real purchase through` awaitPurchase` , and rendered a server driven paywall with the` Paywall` composable from` purchases-kmp-ui` . Every piece of subscription logic except platform initialization lives in` commonMain` , which means your iOS and Android apps stay in lockstep without any code duplication.


The thing worth internalizing is what the SDK takes off your plate. You no longer think in terms of purchase tokens versus signed transactions, two notification pipelines, or two receipt verification servers. You think in terms of offerings, packages, entitlements, and` CustomerInfo` , and the SDK collapses both stores into those four concepts. That same abstraction is what makes the visual Paywall Editor possible, since it is far easier to ship one server driven UI when there is only one shared state model underneath.


Whether you are porting an existing Android subscription app to iOS, starting a new product on KMP from scratch, or experimenting with paywall variants without burning a release cycle, this setup gives you the smallest surface area you can ship a cross platform subscription app on. The remaining engineering time goes back into the parts of your app that actually differentiate your product.


As always, happy coding!


—[Jaewoong](https://github.com/skydoves/) (skydoves)
