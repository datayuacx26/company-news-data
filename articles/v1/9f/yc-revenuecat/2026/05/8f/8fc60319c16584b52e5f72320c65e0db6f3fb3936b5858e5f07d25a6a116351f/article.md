---
schema_version: "1.0.0"
document_id: "8fc60319c16584b52e5f72320c65e0db6f3fb3936b5858e5f07d25a6a116351f"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/testing-subscription-cmp"
published_at: "2026-05-12T00:22:09+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T22:14:43.061733+00:00"
content_hash: "sha256:f4b3587256b75e6672d4f1f18a863fd94ff2103e9a54962bc21bee082d77a3d7"
---

# Testing subscriptions on Compose Multiplatform: one test suite for iOS and Android

You wrote your subscription logic once in` commonMain` , your Compose Multiplatform paywall renders the same on iPhone and Pixel, and your` CustomerInfo` flow works on both platforms without a single` expect/actual` . Then you sit down to test it and find that the same product still has two sandboxes, two test account systems, two CI jobs, and two completely different ways for the platform to say “the purchase succeeded.” The unified codebase from[Compose Multiplatform Subscriptions](https://www.revenuecat.com/blog/engineering/cmp-subscriptions/) stops at the test boundary, and most teams either skip purchase testing entirely or maintain two parallel test suites that drift apart.


In this article, you’ll work through a complete subscription testing setup for a Compose Multiplatform app, exploring why Google Play and StoreKit sandboxes resist unification, how RevenueCat’s Test Store collapses both into one sandbox, how to wire the Test Store into a KMP build with a single` BuildConfig` field, why the right testability boundary is a` PaywallsRepository` interface in` commonMain` , how to write coroutine and` StateFlow` tests in` commonTest` so a single suite runs against both targets, why fakes beat mocks for KMP subscription code, and how to layer instrumented tests on top so the same code path is exercised end to end.


Every snippet uses the same source layout as[cat-paywalls-kmp](https://github.com/RevenueCat/cat-paywalls-kmp) , the official CMP demo, so you can drop these patterns into a real project without renaming anything.


## **The fundamental problem: One product, two sandboxes**


Subscription testing on a native Android app is already an undertaking. The[ultimate guide to Android subscription testing](https://www.revenuecat.com/blog/engineering/the-ultimate-guide-to-android-subscription-testing/) lists license testers, internal tracks, closed tracks, the` android.test.purchased` static response, and a handful of` BillingClient` quirks before you can reliably run a single purchase end to end. The equivalent path on iOS adds StoreKit configuration files, sandbox tester accounts, accelerated renewal cycles, and TestFlight rate limits that change throughout the year.


When the same product ships on both stores, you do not get to pick one of these paths. You run both, and you reconcile the differences yourself. A purchase that goes through in the Google Play closed track does not appear in App Store Connect. A StoreKit` Transaction` that arrives over the iOS testing pipeline does not flow through your Android` PurchasesUpdatedListener` . Even when the entire app is one Kotlin codebase, you write two` @Test` annotations, two CI matrices, and two test data setups.


There is also the unit test gap. Google Play does not expose any way to simulate a purchase inside a JVM unit test. StoreKit configuration files run in the iOS simulator, not in` kotlinx-coroutines-test` . So even before you worry about cross platform reconciliation, you cannot get a working purchase flow inside` commonTest` at all. The standard advice is to push everything below the SDK boundary into integration tests, which means most teams end up testing only the parts of their code that do not actually touch` Purchases` .


Look at the` CatArticlesDetailViewModel` from the cat-paywalls-kmp demo and you can see the shape of the problem:


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


This class has real business logic. It decides whether to fade an article body, whether to surface a “Join Now” CTA, what to do when the network call fails, and when to recompose. None of that logic depends on whether the underlying receipt came from Google Play or StoreKit. But if` PaywallsRepository.fetchCustomerInfo()` only emits values when a real billing SDK is connected to a real store, you cannot reach any of this logic from a unit test. The KMP win disappears at exactly the boundary where you most need it back.


The rest of this article is about getting it back.


## **The Test Store: A sandbox built into RevenueCat itself**


The shortest path out of the two sandbox problem is to stop using the two sandboxes for development purchases. RevenueCat’s[Test Store](https://www.revenuecat.com/docs/test-and-launch/sandbox/test-store) is a sandbox built into the platform itself, not into Google Play or the App Store. You configure it once in the dashboard, set a different API key in your app, and the SDK routes every purchase through RevenueCat’s own purchase modal instead of asking the native store to open its dialog.


The modal it shows is small and deliberate. When the user taps “Subscribe” inside the` Paywall` composable, the SDK overlays a three button sheet: **Test valid purchase** , **Test failed purchase** , and **Cancel** . Tapping the first one is what a successful purchase looks like from your app’s perspective. The SDK marks the receipt valid on the backend,` awaitPurchase` resumes with a` StoreTransaction` , the` CustomerInfo` flow emits a new value with` entitlements\["premium"\].isActive == true` , and your gated UI unlocks the same way it would in production. The other two buttons exercise the failure and cancellation paths without you having to fake an exception by hand.


There is no Play Console setup, no App Store Connect tester account, and no license tester opt in URL. The Test Store works in the simulator, the emulator, debug builds on a real device, and CI runners. Auto renewal still happens, just on accelerated cycles: a monthly subscription renews every five minutes, an annual subscription renews every hour, both stop after five renewals so a test session never runs longer than around five hours. Every Test Store purchase shows up in the same RevenueCat dashboard as production data, which means the same` CustomerInfo` shape, the same webhook payloads, and the same entitlement transitions you ship against.


Two practical rules. First, you need` purchases-kmp` 2.2.2 or newer for the Test Store path; the cat-paywalls-kmp demo pins` 2.10.2+17.55.1` . Second, a Test Store key is a debug only artifact. It looks like` test_...` instead of` goog_...` or` appl_...` and is rejected by the server if the SDK is configured against a production project. Treat it the same way you would treat a debug signing key: useful for development, never in a release.


## **Setting up the Test Store in a KMP project**


The whole setup is one extra` BuildConfig` field on the Android side and one decision in your` Application.onCreate` about which key to use. iOS does not need a separate change because the same Kotlin code reads the same configuration when the shared module initializes on the Swift side.


Start with the Gradle build. In` composeApp/build.gradle.kts` , read the Test Store key from` local.properties` and expose it through` buildConfigField` . Pulling the key from a local property file (which is already gitignored by Android Studio) keeps it out of source control by default:


```text
import   java.util.Properties


val   localProperties   =   Properties  ().  apply   {
val   file   =   rootProject.  file  (  "local.properties"  )
if   (file.  exists  ()) file.  inputStream  ().  use   {   load  (it) }
}


android   {
namespace   =   "com.revenuecat.catpaywalls"
defaultConfig   {
applicationId   =   "com.revenuecat.catpaywalls"
buildConfigField  (
"String"  ,
"REVENUECAT_TEST_API_KEY"  ,
"  \"  ${localProperties.  getProperty  ("revenuecat.test.api.key", "")}  \"  "  ,
)
}
buildFeatures   {
compose   =   true
buildConfig   =   true
}
}
```


Notice the default value of an empty string. If` local.properties` does not contain` revenuecat.test.api.key` , the field is still defined and compiles cleanly. That matters for two reasons: release builds pulled from a clean CI checkout do not see the key at all, and any developer who has not opted into the Test Store keeps the regular production path without any source changes.


The` local.properties` entry on a developer machine is one line:


```text
revenuecat.test.api.key=test_YOUR_KEY_HERE


```


The key selection happens in` CatArticlesApplication` , which is the Android entry point for the shared Compose surface. The pattern is “prefer Test Store key when present, fall back to the production key otherwise”:


```text
class   CatArticlesApplication   :   Application  () {


override   fun   onCreate  () {
super  .  onCreate  ()


Purchases.logLevel   =   LogLevel.DEBUG
val   apiKey   =   BuildConfig.REVENUECAT_TEST_API_KEY
.  takeIf   { it.  isNotBlank  () } ?: REVENUECAT_API_KEY


Purchases.  configure  (
PurchasesConfiguration  (apiKey   =   apiKey) {
appUserId   =   null
},
)
}


companion   object   {
private   const   val   REVENUECAT_API_KEY   =   "your_revenuecat_api_key"
}
}
```


Two design decisions here are worth pulling out. The first is that the same` Purchases.configure` call routes either to the Test Store or to production. The KMP SDK does not have a separate “test mode” flag; it picks the backend based on the key prefix and behaves identically otherwise. Your` commonMain` code never has to ask which backend it is talking to. The second is that this is a debug only ergonomics, not a build flavor. Every developer can flip on Test Store for their personal build by adding one line to their own` local.properties` , without any branches in the source tree.


The iOS side does not need an analogue. When the Kotlin runtime starts on iOS, it shares the same` Purchases.sharedInstance` configured by the platform` App` struct. If you want the iOS app to also use the Test Store during development, configure the same` test_...` key in the SwiftUI entry point:


```text
@main
struct iosAppApp:   App   {
init  () {
Purchases.logLevel   =   .debug
Purchases.  configure  (withAPIKey:   testStoreApiKey   ??   "your_ios_api_key"  )
}
var   body:   some   Scene   {
WindowGroup   {   ContentView  () }
}
}
```


Read` testStoreApiKey` from an` .xcconfig` , a` Info.plist` entry, or a build setting. The shape is the same as on Android: take the key from a local source of truth, fall back to a production constant.


## **The testability boundary: wrapping Purchases behind a repository**


Even with the Test Store wired up, you do not want every unit test to depend on a running` Purchases.sharedInstance` . The SDK assumes a platform context that is not present in` commonTest` : an Android` Context` , a` BillingClient` , an iOS StoreKit transaction listener. The cat-paywalls-kmp demo solves this by funneling every call into the SDK through a single repository interface in` core/data` :


```text
interface   PaywallsRepository   {
fun   fetchOffering  ():   Flow  <  Result  <  Offering  >>
fun   fetchCustomerInfo  ():   Flow  <  Result  <  CustomerInfo  >>
fun   awaitPurchase  (packageId:   String  ):   Flow  <  Result  <  StoreTransaction  >>
}
```


There are three things to notice about this interface. First, every method returns a` Flow<Result<T>>` instead of a suspend function. That choice gives` StateFlow` consumers something to` .map` and` .collect` without paying for an extra` viewModelScope.launch` , and it lets the implementation choose between cold flow semantics and` stateIn` caching without changing callers. Second, the types in the return positions are all from` com.revenuecat.purchases.kmp.models.*` . They are pure Kotlin data classes that exist in` commonMain` , which means a test can construct or hold them without expect/actual gymnastics. Third, there is no platform specific code in the signature, which is what lets the same interface back both` androidMain` and` iosMain` consumers.


The production implementation wraps` Purchases.sharedInstance` with` suspendCancellableCoroutine` and a small` Flow` builder. The shape is straightforward:


```text
class   PaywallsRepositoryImpl   :   PaywallsRepository   {


override   fun   fetchCustomerInfo  ():   Flow  <  Result  <  CustomerInfo  >>   =   flow   {
try   {
val   customerInfo   =   Purchases.sharedInstance.  awaitCustomerInfo  ()
emit  (Result.  success  (customerInfo))
}   catch   (e:   Exception  ) {
emit  (Result.  failure  (e))
}
}.  flowOn  (Dispatchers.IO)


override   fun   fetchOffering  ():   Flow  <  Result  <  Offering  >>   =   flow   {
try   {
val   offerings   =   Purchases.sharedInstance.  awaitOfferings  ()
val   current   =   offerings.current ?:   error  (  "No current offering"  )
emit  (Result.  success  (current))
}   catch   (e:   Exception  ) {
emit  (Result.  failure  (e))
}
}.  flowOn  (Dispatchers.IO)
}
```


This is the only place in the entire codebase that imports` Purchases.sharedInstance` . Every` ViewModel` , every Compose function, every test is on the other side of the interface. That is the boundary you need to make` commonTest` viable, because anything above this line can be exercised against a fake.


## **Unit testing in commonTest: the patterns that work on both platforms**


A Kotlin Multiplatform unit test cannot use MockK, Mockito, or any reflection based mocking framework. Those libraries are JVM only. They will compile in` commonTest` if MockK is on the version catalog, but the` iosTest` target will fail to link when Kotlin/Native tries to resolve the bytecode generation runtime. The cat-paywalls-kmp demo even declares MockK in` gradle/libs.versions.toml` and then never uses it, because the moment you adopt the convention plugin that runs` commonTest` against both targets you cannot import it from shared code.


What does work in` commonTest` is` kotlin-test` ,` kotlinx-coroutines-test` , and Turbine. The cat-paywalls-kmp` KmpLibraryConventionPlugin` wires them into every feature module at the source set level:


```text
sourceSets.  apply   {
commonMain.  dependencies   {
implementation  (libs.  findLibrary  (  "kotlinx-coroutines-core"  ).  get  ())
}
commonTest.  dependencies   {
implementation  (libs.  findLibrary  (  "kotlin-test"  ).  get  ())
implementation  (libs.  findLibrary  (  "kotlinx-coroutines-test"  ).  get  ())
implementation  (libs.  findLibrary  (  "turbine"  ).  get  ())
}
}
```


The reason this is in a convention plugin and not in each feature module’s` build.gradle.kts` is that you want this to be free. Every module that hosts a` ViewModel` or a state holder needs the same test dependencies, and forcing each module to opt in by hand is exactly the kind of friction that ends with someone writing logic they cannot reach from a test. The plugin makes “write a unit test” the default, not a setup task.


A feature module then only declares what is unique to its tests. For` feature/subscriptions` , the only extra dependency is` core/data` , because the test uses the real` PaywallsRepository` interface:


```text
plugins   {   id  (  "catpaywalls.kmp.feature"  ) }
android   { namespace   =   "com.revenuecat.catpaywalls.feature.subscriptions"   }
kotlin   {
sourceSets   {
commonTest.  dependencies   {
implementation  (projects.core.  data  )
}
}
}
```


That is the entire test wiring. From here, you can write a` class SubscriptionManagementViewModelTest` in` feature/subscriptions/src/commonTest/kotlin/...` and it will be picked up by` androidUnitTest` and` iosTest` automatically. One file, both targets.


## **Why fakes beat mocks for KMP subscription code**


With MockK off the table, your options are hand written fakes or expect/actual mock factories. The cat-paywalls-kmp demo picks fakes for every test, and the choice is more deliberate than it looks. Subscription code has two properties that make fakes the better fit even in a JVM only project. First, the surface area of` PaywallsRepository` is small. Three methods, all returning` Flow<Result<T>>` . There is nothing to mock that a small data class with a few setters cannot already express. Second, the same fake is reused across many tests with different setup, which makes a stateful builder more readable than a stack of` every { ... } returns ...` lines.


Here is what the canonical fake from` FakePaywallsRepository.kt` looks like:


```text
class   FakePaywallsRepository   :   PaywallsRepository   {
private   var   offeringResult:   Result  <  Offering  >?   =   null
private   var   customerInfoResult:   Result  <  CustomerInfo  >?   =   null
private   var   purchaseResults:   MutableMap  <  String  ,   Result  <  StoreTransaction  >>   =   mutableMapOf  ()


fun   setCustomerInfoResult  (result:   Result  <  CustomerInfo  >?) {
customerInfoResult   =   result
}


fun   setPurchaseResult  (packageId:   String  , result:   Result  <  StoreTransaction  >) {
purchaseResults[packageId]   =   result
}


fun   simulateOfferingError  (message:   String   =   "Failed to fetch offering"  ) {
offeringResult   =   Result.  failure  (  Exception  (message))
}


fun   simulateCustomerInfoError  (message:   String   =   "Failed to fetch customer info"  ) {
customerInfoResult   =   Result.  failure  (  Exception  (message))
}
```


The constructor takes no arguments. The fake starts in a deliberately empty state where every flow emits a “not configured” failure. A test then sets the slots it cares about and leaves the others alone. That asymmetry is the value of fakes over mocks: you do not have to enumerate every method up front, you only describe the scenario you are testing.


The flow methods themselves are one line each, with the empty case routed through a recognisable exception so a misconfigured test fails loudly rather than hanging on an empty flow:


```text
emit  (customerInfoResult ?: Result.  failure  (  IllegalStateException  (  "No customer info configured"  )))
}


override   fun   awaitPurchase  (packageId:   String  ):   Flow  <  Result  <  StoreTransaction  >>   =   flow   {
emit  (purchaseResults[packageId] ?: Result.  failure  (  IllegalStateException  (  "Package not found:   $packageId  "  )))
}
}
```


The` IllegalStateException` here is not for production semantics. It is a test ergonomic. When a future contributor adds a test that calls a method without setting it up, the test fails with a message that names the missing slot, which is much faster to debug than an empty` Flow` that just never emits.


One limitation to call out. The fake holds and returns` Offering` and` CustomerInfo` values, but it does not construct them. Those types come from` purchases-kmp` and do not currently expose public constructors. That means you can test the failure paths through the fake directly, but a successful “purchase completed and entitlement is now active” assertion has to be exercised against a real` Purchases.sharedInstance` . That is exactly the seam the Test Store fills, and the next two sections show how.


## **Writing tests against the fake**


A ViewModel test in` commonTest` looks almost identical to its androidx counterpart. The only differences are the` kotlinx.coroutines.test.StandardTestDispatcher` instead of` TestCoroutineDispatcher` , and Turbine instead of` LiveData` observers. Here is the setup pattern that the cat-paywalls-kmp` SubscriptionManagementViewModelTest` uses:


```text
@OptIn  (ExperimentalCoroutinesApi::  class  )
class   SubscriptionManagementViewModelTest   {


private   val   testDispatcher   =   StandardTestDispatcher  ()
private   lateinit   var   fakeRepository:   FakePaywallsRepository


@BeforeTest
fun   setup  () {
Dispatchers.  setMain  (testDispatcher)
fakeRepository   =   FakePaywallsRepository  ()
}


@AfterTest
fun   tearDown  () {
Dispatchers.  resetMain  ()
}
}
```


` Dispatchers.setMain` replaces the main dispatcher that` viewModelScope` uses with the test dispatcher. Without that swap, any` StateFlow.stateIn(viewModelScope, ...)` would post to the real main thread and the test would hang, because there is no Android main looper in` commonTest` .` Dispatchers.resetMain` after each test is the symmetric tear down. Together they give you deterministic scheduling: nothing runs until you ask it to.


The actual test reaches into the fake, instantiates the ViewModel, and asserts on its state through Turbine. The pattern for an error path:


```text
@Test
fun   whenCustomerInfoFetchFails_stateIsError  ()   =   runTest   {
fakeRepository.  simulateCustomerInfoError  (  "Failed to fetch customer info"  )
val   viewModel   =   SubscriptionManagementViewModel  (fakeRepository)


viewModel.uiState.  test   {
assertIs  <  SubscriptionManagementUiState  .  Loading  >(  awaitItem  ())
testDispatcher.scheduler.  advanceUntilIdle  ()
val   errorState   =   awaitItem  ()
assertIs  <  SubscriptionManagementUiState  .  Error  >(errorState)
assertEquals  (  "Failed to fetch customer info"  , errorState.message)
cancelAndIgnoreRemainingEvents  ()
}
}
```


Three details earn their keep here. The first` awaitItem()` returns the initial` Loading` value because` StateFlow` always replays its current state on collection.` testDispatcher.scheduler.advanceUntilIdle()` then drains every queued coroutine in the test scope, which is what causes the repository’s flow to emit, the ViewModel’s` combine` chain to run, and the next state to land in the` StateFlow` . The second` awaitItem()` picks that state up, and the assertion confirms the error message survives the pipeline.


If you write four or five tests like this, you cover the entire surface of the ViewModel’s state machine. The initial loading state, the offering error path, the customer info error path, the combined error path, and the cancellation path are each one fake setter plus an` awaitItem()` . None of these tests need a billing client, none of them need a network, and the same suite runs against the JVM target and the iOS target without modification.


What this suite cannot cover is the actual purchase. Because the fake cannot construct an` Offering` with real packages, you cannot run a test that says “the user taps subscribe, the SDK validates the receipt, and the entitlement flips to active.” For that path you need the Test Store, and the right place for it is an instrumented test on top of the unit suite, not inside it.


## **Integration testing with the Test Store on Android**


The[Testing Test Store](https://www.revenuecat.com/blog/engineering/testing-test-store/) post on the RevenueCat engineering blog walks through this end to end, and the same approach applies to a CMP project because the Android target compiles down to a normal` androidTest` source set. Put the integration test in` composeApp/src/androidTest/kotlin/...` . Configure` Purchases` once with the Test Store key, launch a small activity that drives a real purchase, and use Espresso to tap one of the three buttons that the Test Store modal presents.


The setup uses` InstrumentationRegistry.getInstrumentation().targetContext` because the test runs in a real Android process, not in a JVM unit test. The configuration call is the same one your` Application` makes, but pinned to the Test Store key:


```text
@Before
fun   setup  () {
val   context   =   InstrumentationRegistry.  getInstrumentation  ().targetContext
Purchases.logLevel   =   LogLevel.DEBUG
Purchases.  configure  (
PurchasesConfiguration.  Builder  (context, BuildConfig.REVENUECAT_TEST_API_KEY)
.  purchasesAreCompletedBy  (PurchasesAreCompletedBy.REVENUECAT)
.  build  ()
)
}
```


The actual purchase test then resolves the test offering, launches the activity, and waits for the Test Store modal to appear before tapping the “Test valid Purchase” button:


```text
@Test
fun   successfulPurchaseUpdatesEntitlements  ()   =   runBlocking   {
val   offerings   =   Purchases.sharedInstance.  awaitOfferings  ()
val   pkg   =   offerings.all[  "test-offering"  ]  !!  .availablePackages.  first  ()


activityScenario   =   ActivityScenario.  launch  (TestPurchaseActivity::  class  .java)
activityScenario.  onActivity   { activity   ->
activity.  launchPurchase  (pkg) { result, _   ->   purchaseResult   =   result }
}


delay  (  2_000  )
onView  (  withText  (  "Test valid Purchase"  )).  perform  (  click  ())


withTimeout  (  30  .seconds) {
while   (purchaseResult   ==   null  )   delay  (  500  )
}


assertTrue  (purchaseResult  !!  .customerInfo.entitlements.active.  isNotEmpty  ())
}
```


Two things to call out. The` delay(2_000)` before the Espresso tap exists because the Test Store modal is rendered through the host activity, and Espresso needs the view hierarchy to settle before it can find the button. The 30 second timeout on the assertion accounts for the round trip to the RevenueCat backend that validates the receipt and updates` CustomerInfo` . Both numbers can be tuned for your CI environment, but they are intentionally generous because the test is doing real network work.


The companion tests are the other two buttons. Tapping “Test failed Purchase” lets you assert that the entitlement stays inactive and the error propagates through your domain layer. Tapping “Cancel” lets you assert that the` PurchaseCancelled` exception is mapped to a no op rather than surfaced as a user facing error. Three tests, three modal buttons, one Test Store key, no Play Console.


## **The iOS side: StoreKit configuration files and what the Test Store unifies**


On iOS, the equivalent of an instrumented test is a StoreKit configuration file driving an XCUITest. Apple introduced[StoreKit configuration files](https://www.revenuecat.com/docs/test-and-launch/sandbox/apple-app-store) in Xcode 12, and they remain the canonical way to test purchases inside the iOS simulator without setting up a sandbox account. You describe the products in a` .storekit` file, attach it to a scheme, and run the app under that scheme. Purchases then resolve through the configuration file instead of the App Store backend, and you can use XCUITest to drive the resulting native StoreKit modal.


This works, and for an iOS only app it is often the right answer. In a Compose Multiplatform project, the trade off looks different. The StoreKit configuration file only covers the iOS side. It does not interact with your Android tests, it does not flow data through your` PaywallsRepository` , and a successful purchase against it does not show up in the RevenueCat dashboard the way a real purchase does. You end up with two parallel integration suites that test the same Kotlin code through two completely different driver layers.


The Test Store collapses this. When the same` commonMain` code is configured against a Test Store key on both platforms, an iOS purchase goes through the same RevenueCat modal as an Android purchase, the same backend validates the receipt, and the same` CustomerInfo` flow emits the same shape on both sides. Your iOS integration test can then drive the same three button modal with XCUITest:


```text
func   testSuccessfulPurchase  ()   throws   {
let app   =   XCUIApplication  ()
app.  launch  ()
app.buttons[  "Subscribe"  ].  tap  ()
app.buttons[  "Test valid Purchase"  ].  tap  ()


let unlockedHeader   =   app.staticTexts[  "Premium Article"  ]
XCTAssertTrue  (unlockedHeader.  waitForExistence  (timeout:   30  ))
}
```


The pattern mirrors the Android Espresso test almost line for line. Find the subscribe button, find the “Test valid Purchase” button, wait for the gated content to unlock. The same three Test Store buttons cover the same three scenarios. The same` CustomerInfo` update unlocks the same composable. You still write two driver layers because the OS testing frameworks are different, but the data plane is shared, the network calls are the same, and the dashboard sees both runs as the same Test Store activity.


For most teams, this is enough. If you also want to exercise the native StoreKit error paths (parental controls blocking a purchase, sandbox account expired, region restrictions), keep a small` .storekit` based suite for those edge cases. The bulk of your purchase path coverage moves to the Test Store and shrinks the size of the iOS only suite to a handful of platform specific tests.


## **A three layer testing strategy**


Putting it all together, a Compose Multiplatform subscription app has three useful test layers, and each one answers a different question.


The first layer is` commonTest` unit tests against` FakePaywallsRepository` . This is where you cover your ViewModel state machine: initial loading, error paths, cancellation handling, derived UI states, and any business logic that lives above the SDK boundary. These tests are fast, deterministic, run on both targets from a single source set, and never touch the network. They are the layer you run on every PR, in pre push hooks, and on every CI build.


The second layer is instrumented tests against the Test Store. On Android this is` androidTest` with Espresso. On iOS this is XCUITest. Both drive the same RevenueCat purchase modal, both produce real` CustomerInfo` updates, and both verify the end to end flow from subscribe button to unlocked content. These tests are slower and a little flakier because they include real network calls, but they are the only layer where you can prove that the purchase path works for a real receipt. Run them on merges to main, on release branches, and as part of your release qualification.


The third layer is optional and platform specific. A StoreKit configuration file based suite for iOS edge cases that the Test Store does not model. A Google Play closed track for the rare bugs that only reproduce against the real billing client. Both are valuable, but neither is the workhorse. They exist to plug specific gaps, not to cover the core purchase flow.


The numbers shift the right way too. The unit tests are tens of milliseconds each and run on every commit. The instrumented Test Store tests are seconds each and run on merge. The native edge case suites are minutes each and run on release. Most of your purchase coverage ends up in the layer that runs most often, which is the opposite of what happens when you only have a` closedTrack` and a StoreKit configuration file.


## **CI considerations: one matrix, three jobs**


The CI shape that falls out of this is straightforward. One job runs` commonTest` for both targets:


```text
-   name  : Run unit   tests   (  JVM   +   iOS)
run  : .\  /  gradlew allTests
```


` allTests` is the umbrella task that Kotlin Multiplatform creates when you have both Android and iOS targets configured. It dispatches to` jvmTest` ,` iosX64Test` ,` iosSimulatorArm64Test` , and any other target you have enabled. A failure in any one fails the job. Run this on every push, every pull request, and every nightly build.


The second job runs the Test Store instrumented tests on Android. The blog’s CI snippet works as written:


```text
-   uses  : reactivecircus\  /  android  -  emulator  -  runner@v2
with  :
api  -  level  :   30
target  : google_apis
arch  : x86_64
script  : .\  /  gradlew   :  composeApp  :connectedAndroidTest
```


Two practical notes. The Test Store key needs to be available to this job, but not to forks or to release builds. The cleanest way is a repository secret that the job writes to` local.properties` at the start of the run, so the same` buildConfigField` pattern picks it up. The job should also run on merges to main and on release branches, not on every PR, because it includes real network calls and consumes Test Store renewals.


The third job runs the iOS XCUITests. A standard` xcodebuild test` invocation against the same scheme that ships the app, with the Test Store key injected through the same mechanism your local builds use. Both Android and iOS integration jobs read from the same RevenueCat project, which means a single dashboard view shows test activity from both platforms.


The result is a CI surface that mirrors the test layers. Fast feedback from the unit suite on every commit. Slower but realistic feedback from the Test Store suite on merge. The expensive native suites only when you cut a release. None of these jobs require coordinating two sandboxes, and the same` PaywallsRepository` and` ViewModel` code is exercised end to end at every layer.


## **Conclusion**


In this article, you’ve worked through a complete subscription testing setup for a Compose Multiplatform app. You configured the RevenueCat Test Store from` local.properties` and a single` buildConfigField` , drew the testability boundary at a` PaywallsRepository` interface so the SDK never leaks into your ViewModels or your tests, wrote` commonTest` unit tests against a` FakePaywallsRepository` using` kotlin-test` ,` kotlinx-coroutines-test` , and Turbine, then layered instrumented tests on top with Espresso on Android and XCUITest on iOS that drive the same three button Test Store modal. The same shared code path is exercised from` commonTest` all the way through to the receipt validation backend, on both platforms, without two parallel test suites.


The thing worth internalizing is what changed at each layer. Subscription testing usually fragments because the sandboxes do. By moving the development sandbox up into RevenueCat itself, the Test Store turns the two platform problem back into one. By putting a small repository interface between` Purchases.sharedInstance` and the rest of your app,` commonTest` becomes a viable home for the bulk of your business logic tests. By using fakes instead of mocks at the unit layer, the same suite runs against both Kotlin targets without expect/actual or a JVM only mocking framework. Each of these choices is small. Together they collapse what used to be two test suites into one.


Whether you are porting an Android subscription app to iOS, starting a new CMP product from scratch, or trying to get purchase testing into CI for the first time, this layered setup gives you the smallest surface area you can ship a cross platform subscription app on with real confidence. The full source for every snippet in this article lives in[cat-paywalls-kmp](https://github.com/RevenueCat/cat-paywalls-kmp) , and a free RevenueCat project with a Test Store key is the only thing you need to run all three layers locally.
