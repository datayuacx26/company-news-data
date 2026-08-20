---
schema_version: "1.0.0"
document_id: "923be93c80006cb7d9c79071f9e9b248f61a0eae8ccc0022e5dabacfdc8d2635"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/android-codegen"
published_at: "2026-06-11T16:00:00+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:23eb39eb42ec5ca4783fa45d7f3cd3a71822f67d039b6e30526750573ec964f1"
---

# Introducing the RevenueCat Codegen Gradle Plugin: type safe entitlements and offerings on Android

Every RevenueCat integration shares the same quiet liability: the string keys. Your entitlements, offerings, and packages live in the RevenueCat dashboard, and your Kotlin code reaches for them with raw strings like` entitlements\["premium_access"\]` . The compiler cannot verify those strings, the IDE cannot autocomplete them, and a single typo ships as a runtime bug instead of failing the build. The new[RevenueCat Codegen Gradle Plugin](https://github.com/RevenueCat/purchases-android/tree/main/codegen) closes this gap by talking to the RevenueCat API at build time and generating type-safe Kotlin accessors for everything you’ve configured in the dashboard.


In this article, you’ll explore what the plugin generates and why, how its two Gradle tasks fetch and cache your project schema, how to set it up with a version catalog, how naming styles turn dashboard lookup keys into Kotlin identifiers, how caching and offline mode keep builds working without a network, and the trade-offs to weigh before adopting it.


## **The fundamental problem: Dashboard keys as raw strings**


Consider the typical code for gating a premium feature and resolving a package to purchase:


```text
val   isPremium   =   customerInfo.entitlements[  "premium_access"  ]?.isActive   ==   true
val   offering   =   offerings.  getOffering  (  "perplexity"  )
val   monthly   =   offering?.  getPackage  (  "  \$  rc_monthly"  )
```


This code compiles whether or not` "premium_access"` exists in your project. If someone renames the entitlement in the dashboard, or a teammate types` "premium_acess"` in a new screen, nothing fails until a user hits that code path: the entitlement lookup silently returns` null` , and` getPackage` throws at runtime. The usual mitigation is a hand maintained constants file, but that file drifts out of sync with the dashboard the moment anyone forgets to update it.


With the codegen plugin applied, the same logic becomes:


```text
val   isPremium   =   customerInfo.isPremiumAccessActive
val   offering   =   offerings.perplexity
val   monthly   =   offering?.perplexityMonthly
```


The difference is more than aesthetics:


- **IDE autocomplete** : every entitlement, offering, and package from your dashboard surfaces as a typed property, so you discover them by typing a dot instead of switching to a browser tab.
- **Compile time safety** : a typo or a removed entitlement becomes a build error, not a runtime` null` .
- **No drift** : the plugin fetches your latest dashboard state at build time, so there is no constants file to maintain by hand.


## **Setting up the plugin**


The plugin is published to Maven Central as part of the RevenueCat Purchases SDK. The plugin ID is` com.revenuecat.purchases.codegen` , and its version always matches the Purchases SDK version, so you reuse the version string you already have.


### **Step 1: Apply the plugin**


Add it to your version catalog in` gradle/libs.versions.toml` :


```text
[versions]
purchases   =   "PURCHASES_VERSION"


[plugins]
revenuecat  -  codegen   =   { id   =   "com.revenuecat.purchases.codegen"  , version.ref   =   "purchases"   }
```


Declare it in your root` build.gradle.kts` so Gradle resolves it for all subprojects:


```text
plugins   {
alias  (libs.plugins.revenuecat.codegen) apply   false
}
```


Then apply it in your app module’s` build.gradle.kts` :


```text
plugins   {
alias  (libs.plugins.android.application)
alias  (libs.plugins.revenuecat.codegen)
}
```


### **Step 2: Configure the extension**


Only three properties are required:` apiKey` ,` projectId` , and` packageName` . Everything else has a default.


```text
revenuecat   {
apiKey.  set  (  "sk_your_v2_secret_key"  )
projectId.  set  (  "proj_your_project_id"  )
packageName.  set  (  "com.example.app.rc"  )
}
```


**` apiKey`** must be a v2 secret key (it starts with` sk_` ) with read permissions, created in the RevenueCat dashboard under Project Settings > API Keys. This key is used only at build time and is never compiled into your app binary. You’ll see how to keep it out of version control in a later section.


**` projectId`** is your project’s identifier, also found in Project Settings.


**` packageName`** is the package the generated code lands in, and it must be a package you own, such as` com.myapp.rc` . Avoid anything under` com.revenuecat.*` . The Purchases SDK ships a consumer ProGuard rule,` -keep class com.revenuecat.** { *; }` , which prevents classes in that namespace from being shrunk or obfuscated. If your generated code lands there, R8 retains all of it in release builds even when unused, which bloats your APK for no benefit.


The full set of optional properties looks like this:


```text
import   com.revenuecat.purchases.codegen.NamingStyle
import   com.revenuecat.purchases.codegen.OfflineMode


revenuecat   {
apiKey.  set  (  "sk_your_v2_secret_key"  )
projectId.  set  (  "proj_your_project_id"  )
packageName.  set  (  "com.example.app.rc"  )


cacheTtlMinutes.  set  (  30L  )
offlineMode.  set  (OfflineMode.USE_CACHE_OR_SKIP)
namingStyle.  set  (NamingStyle.CAMEL_CASE)


generateEntitlements.  set  (  true  )
generateOfferings.  set  (  true  )
generatePackages.  set  (  true  )
generateCustomerInfoExtensions.  set  (  true  )
}
```


The four` generate*` flags each control one category of output. If you iterate` offering.availablePackages` dynamically rather than accessing packages by name, for example, set` generatePackages.set(false)` to keep the generated surface small. The caching and offline options are covered in their own section below.


### **Step 3: Build**


Run any standard build task:


` ./gradlew assembleDebug`


Generation runs automatically before compilation. After the first successful build, do File > Sync Project with Gradle Files in Android Studio so the IDE picks up the new source directory and autocomplete starts working.


If you ever want to fetch or regenerate without a full build, both tasks are invokable directly:


` ./gradlew :app:rcFetchSchema`
` ./gradlew :app:rcGenerateCode`


## **What gets generated**


Suppose your project has a` premium_access` entitlement and a` perplexity` offering containing the standard` $rc_monthly` and` $rc_annual` packages. The plugin generates four categories of output.


### **Entitlement ID constants**


A single` RCEntitlementId` object holds one constant per entitlement:


```text
object   RCEntitlementId   {
const   val   PREMIUM_ACCESS:   String   =   "premium_access"
}
```


These constants are useful at the boundaries where you still need the raw key, such as logging or analytics events, while keeping the compiler in the loop.


### **Type safe` EntitlementInfos` extensions**


For each entitlement, two extension properties land on` EntitlementInfos` : an accessor that returns the` EntitlementInfo` or` null` , and a boolean shortcut for the common` isActive` check.


```text
val   EntitlementInfos.premiumAccess:   EntitlementInfo  ?
get  ()   =   this  [  "premium_access"  ]


val   EntitlementInfos.isPremiumAccessActive:   Boolean
get  ()   =   this  [  "premium_access"  ]?.isActive   ==   true
```


### **Convenience` CustomerInfo` extensions**


The same active check is also generated directly on` CustomerInfo` , so gating a feature behind a single entitlement skips the` entitlements` lookup entirely:


```text
val   CustomerInfo.isPremiumAccessActive:   Boolean
get  ()   =   this  .entitlements[  "premium_access"  ]?.isActive   ==   true
```


### **Offering and package accessors**


Each offering gets a constant in` RCOfferingId` and a typed accessor on` Offerings` that wraps` getOffering()` :


```text
object   RCOfferingId   {
const   val   PERPLEXITY:   String   =   "perplexity"
}


val   Offerings.perplexity:   Offering  ?
get  ()   =   this  .  getOffering  (  "perplexity"  )
```


Packages need one extra design decision. Multiple offerings commonly share the same package lookup key, since` $rc_monthly` exists in nearly every offering. Generating a bare` monthly` extension on` Offering` would collide the moment a second offering defines the same package. The plugin resolves this by prefixing each package property with its offering name and scoping the ID constants into a per offering object:


```text
object   RCPerplexityPackageId   {
const   val   MONTHLY:   String   =   "${'$'}rc_monthly"
const   val   ANNUAL:   String   =   "${'$'}rc_annual"
}


val   Offering.perplexityMonthly:   Package  ?
get  ()   =   this  .  getPackage  (  "${'$'}rc_monthly"  )
```


The` ${'$'}` sequence might catch your eye. Because` $` begins a string template in Kotlin, KotlinPoet escapes the literal dollar sign in` $rc_monthly` this way so the generated file always compiles. At runtime the string is exactly` $rc_monthly` .


Every generated property also carries a KDoc comment with the display name from your dashboard and a reminder that the code reflects the dashboard at build time, so the documentation popup in the IDE tells you what each key actually is.


## **Naming styles: From lookup keys to Kotlin identifiers**


Dashboard lookup keys are arbitrary strings, so the plugin runs each one through a pipeline before it becomes an identifier: strip the` $rc_` prefix, apply the configured naming style, replace characters that are invalid in identifiers, prefix an underscore when the key starts with a digit, and escape Kotlin reserved words with backticks. The result always compiles, even if someone names an entitlement` when` or` 2024_promo` in the dashboard.


The` namingStyle` option controls the middle step:


Lookup key


` CAMEL_CASE` (default)


` SNAKE_CASE`


` AS_IS`


` premium_access`


` premiumAccess`


` premium_access`


` premium_access`


` ProPhoto`


` prophoto`


` pro_photo`


` ProPhoto`


` $rc_monthly`


` monthly`


` monthly`


` monthly`


` CAMEL_CASE` fits most Kotlin codebases because it follows the standard property naming convention.` AS_IS` preserves the key exactly after prefix stripping, which is useful when your lookup keys are already well formed identifiers and you want a one to one mapping. The constants inside ID objects like` RCEntitlementId` are always` UPPER_SNAKE_CASE` regardless of the style, because constants follow their own convention in Kotlin.


## **Caching and offline mode: Builds that don’t depend on the network**


A Gradle plugin that hits the network on every build would be a hard sell, so the fetch step is built around a local cache.` rcFetchSchema` writes the fetched schema and a timestamp to` build/revenuecat/cache/revenuecat-schema.json` . On the next build, it checks the file’s age against` cacheTtlMinutes` (default 30) and skips the network call entirely if the cache is still fresh. Setting the TTL to` 0` forces a fetch on every build.


When a fetch does run and your project has many offerings, the client paces itself: it follows the API’s pagination, waits 500 milliseconds between requests, and retries up to five times with exponential backoff when it hits a rate limit response. This makes the first build slower on large projects, but every subsequent build reads from the cache.


The interesting question is what happens when the fetch fails: no network on a flight, a connection timeout, or a key that was revoked. The` offlineMode` option offers two behaviors:


- **` USE_CACHE_OR_SKIP`** (default): if a stale cache exists, it is used and a warning logs the cache age. If no cache exists at all, generation is skipped with a log message and the build continues without generated sources. Local builds keep working with no intervention.
- **` FAIL`** : the build fails immediately with a` GradleException` describing the error. This is the right choice for a release pipeline where you need a hard guarantee that the generated code reflects the latest dashboard state.


A practical pattern applies each mode based on the environment:


```text
import   com.revenuecat.purchases.codegen.OfflineMode


val   isCI   =   providers.  environmentVariable  (  "CI"  ).isPresent


revenuecat   {
apiKey.  set  (providers.  environmentVariable  (  "REVENUECAT_API_KEY"  ).  getOrElse  (  ""  ))
projectId.  set  (providers.  environmentVariable  (  "REVENUECAT_PROJECT_ID"  ).  getOrElse  (  ""  ))
packageName.  set  (  "com.example.app.rc"  )
offlineMode.  set  (  if   (isCI) OfflineMode.FAIL   else   OfflineMode.USE_CACHE_OR_SKIP)
}
```


With this setup, a local build falls back to the cache when the network is unavailable, while CI refuses to ship a build whose generated code might be stale.


Note that the fallback applies to fetch failures, not to missing configuration. An empty` apiKey` fails the build immediately with “revenuecat.apiKey must be configured in your build script.” regardless of the offline mode, so every developer needs the key configured even when a cache is present.


One more consequence of` USE_CACHE_OR_SKIP` is worth calling out: if your very first fetch fails, generation is skipped and references to the generated code fail to compile with unresolved symbols. If you hit “No RevenueCat schema cache found. Skipping code generation.” in the build log, temporarily set` offlineMode.set(OfflineMode.FAIL)` to surface the underlying error, which is usually a wrong` apiKey` or` projectId` .


### **Refreshing after dashboard changes**


The generated code reflects your dashboard at the time of the last fetch. After you add or rename an entitlement, a normal build picks the change up once the TTL expires. To pick it up immediately, wipe the cache and rerun the tasks:


```text
rm   -  rf app\  /  build\  /  revenuecat\  /  cache
.\  /  gradlew :  app  :  rcFetchSchema   :  app  :  rcGenerateCode
```


Then sync the project in Android Studio so the IDE reindexes the regenerated sources.


## **How it works: Two Gradle tasks and a schema cache**


The plugin registers a` revenuecat { }` extension and two Gradle tasks under the` revenuecat` group:


1. **` rcFetchSchema`** calls the RevenueCat API v2 (` /projects/{projectId}/entitlements` ,` /projects/{projectId}/offerings` , and` /projects/{projectId}/offerings/{offeringId}/packages` ), follows pagination, and writes the result to a JSON cache at` build/revenuecat/cache/revenuecat-schema.json` along with a timestamp.
2. **` rcGenerateCode`** reads that cache and generates Kotlin source files into` build/generated/revenuecat/kotlin/` using KSP.


You never invoke either task by hand during normal development. The plugin wires` rcGenerateCode` into your compile tasks, so any standard build runs generation first.


## **Keeping the secret key out of version control**


The` apiKey` is a v2 secret key, so even though it never reaches your app binary, it should not be committed in` build.gradle.kts` . The generated files contain only plain lookup key strings like` "premium_access"` , never the key itself, but the build script is checked in. For local development, read the key from` local.properties` , which stays out of version control:


```text
val   localProps   =   java.util.  Properties  ().  apply   {
rootProject.  file  (  "local.properties"  ).  takeIf   { it.  exists  () }?.  inputStream  ()?.  use   {   load  (it) }
}


revenuecat   {
apiKey.  set  (localProps.  getProperty  (  "REVENUECAT_API_KEY"  ,   ""  ))
projectId.  set  (localProps.  getProperty  (  "REVENUECAT_PROJECT_ID"  ,   ""  ))
packageName.  set  (  "com.example.app.rc"  )
}
```


Then add the values to` local.properties` :


```text
REVENUECAT_API_KEY=sk_your_v2_secret_key
REVENUECAT_PROJECT_ID=proj_your_project_id


```


For CI, inject both values as environment variables, as shown in the offline mode example above.


## **Trade offs: When generated accessors fit and when they don’t**


The plugin fits best with stable identifiers. Entitlement IDs rarely change once set, and turning every entitlement check into a compile time verified property removes a whole class of bugs. Offerings and packages benefit the same way when their IDs are stable, especially in a large codebase where autocomplete and rename refactoring matter.


For highly dynamic offerings, keep one trade off in mind. The generated code is a snapshot of your dashboard at build time. Adding a new package or renaming an offering in the dashboard requires a new build and a new app release before the generated accessors reflect it. If you iterate` offering.availablePackages` at runtime instead, an already shipped app picks up new packages without a release, because the package list comes from the RevenueCat backend at runtime.


In practice this matters less than it sounds. Adding a new package that you intend to reference by name in code requires writing new code anyway, which means a new release regardless. But if you drive your paywall presentation entirely from the dashboard without touching code, the runtime API keeps that flexibility, and you can disable` generatePackages` while keeping the entitlement and offering accessors. The two approaches compose: use the generated properties where identifiers are stable, and the dynamic API where the dashboard is the source of truth.


## **Conclusion**


In this article, you’ve explored the[RevenueCat Codegen Gradle Plugin:](https://github.com/RevenueCat/purchases-android/tree/main/codegen) the raw string problem it removes, the` rcFetchSchema` and` rcGenerateCode` tasks that fetch your dashboard schema and generate typed Kotlin accessors, the setup from version catalog to first build, the naming pipeline that turns lookup keys into identifiers that always compile, and the caching and offline modes that keep builds fast and network independent. If your entitlement checks are still string lookups, applying the plugin turns the next typo into a build error instead of a support ticket.


As always, happy coding!


—[Jaewoong](https://github.com/skydoves/) (skydoves)
