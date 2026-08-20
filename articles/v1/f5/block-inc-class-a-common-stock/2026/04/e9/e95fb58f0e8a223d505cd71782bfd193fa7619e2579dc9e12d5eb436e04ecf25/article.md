---
schema_version: "1.0.0"
document_id: "e95fb58f0e8a223d505cd71782bfd193fa7619e2579dc9e12d5eb436e04ecf25"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/metro-migration-at-square-android"
published_at: "2026-04-14T12:00:00+00:00"
first_seen_at: "2026-07-20T03:32:06.876214+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:f54c5c2fff1b8be652fd28a483f1c1494b87544c15099ace7b87fdb4f3341e65"
---

# Metro Migration at Square Android

[Metro](https://zacsweers.github.io/metro) has replaced Anvil and Dagger 2 as the[dependency injection](https://en.wikipedia.org/wiki/Dependency_injection) framework in our Square Android monorepo. Cash Android published a great[blog post](https://code.cash.app/cash-android-moves-to-metro) about why they adopted Metro, the challenges they encountered, and how they solved them. This post builds on that write-up and focuses on the additional constraints and lessons specific to Square Android.


We'd like to sincerely thank[Zac Sweers](https://github.com/ZacSweers) , the author and maintainer of Metro, for partnering and spending many hours with us throughout this migration. Over many months, we worked closely with him to tackle problems unique to a codebase of our size. Zac helped us debug issues, resolve performance problems, and implement missing features. We couldn't have adopted Metro without his effort.


**Thank you, Zac!** ❤️


## What comes after Anvil


We developed[Anvil](https://github.com/square/anvil) at Square and open sourced the framework in June 2020. It was tremendously helpful to us: it simplified our dependency injection setup with Dagger 2 and[improved build times](https://github.com/square/anvil?tab=readme-ov-file#dagger-factory-generation) by a large margin. However, with the release of Kotlin 2.0 Anvil became a blocker for future upgrades, because it relied on compiler APIs specific to Kotlin 1, and there was no direct replacement in the Kotlin 2 compiler.


We considered rewriting Anvil with[KSP](https://kotlinlang.org/docs/ksp-overview.html) , which would have let us avoid depending directly on unstable compiler internals. Although we had a working prototype, the build-time overhead was too high to justify moving forward. We also started a rewrite using Kotlin 2 compiler APIs directly, but progress was slow because Anvil still depended on the same Dagger 2 and KAPT-based architecture. That meant we would still be carrying much of the original complexity forward while porting it to a new compiler backend.


Around the same time, Zac started work on Metro and brought in people from several companies to provide feedback on its design and APIs. Metro quickly became what we had always wanted "Dagger 3" to be. Once the first releases landed, we decided to[deprecate Anvil](https://github.com/square/anvil/issues/1149) , focus on contributing to Metro, and begin migrating Square Android.


## The migration


Our migration followed a similar path to Cash Android's. Metro's interop feature and a[build time flag](https://code.cash.app/cash-android-moves-to-metro#metro-interop) let us continue supporting Dagger 2 while incrementally migrating to Metro. We used a[source split](https://code.cash.app/cash-android-moves-to-metro#one-last-thing---instantiating-dependency-graphs) whenever Metro required code that was incompatible with Dagger 2, or vice versa. Many` @Component.Builder` annotations were[converted](https://code.cash.app/cash-android-moves-to-metro#converting-componentbuilder-to-componentfactory) to` @Component.Factory` so they could leverage the interop support. Metro also surfaced a number of legitimate[nullability issues](https://code.cash.app/cash-android-moves-to-metro#fixing-nullability-issues-on-injected-types) that we then fixed.


Given the size of our codebase and patterns unique to Square Android, we had to take a few extra steps compared to Cash Android.


### Shadow CI job


The first pull request related to the Metro migration was merged in July 2025, and the full migration took nine months to finish. To measure progress and catch regressions early, we set up two nightly CI jobs. The first attempted to build every Gradle module in the repository, more than 7,000 in total, with Metro enabled and published the results to an internal dashboard. That helped us identify modules that still needed attention.


The second job ran our entire CI suite with Metro enabled. We have more than 1,500 CI jobs, and most failed in the beginning. The goal was to drive that number to zero. The results of the CI job were shared daily in a Slack channel:


### Converting Java code to Kotlin


Metro's code generation is driven by a Kotlin compiler plugin, so the code Metro processes has to be written in Kotlin. Java sources could still participate in our old Dagger setup, which meant we had to migrate Java files that were still using Dagger or` javax.inject.*` annotations before those parts of the codebase could move to Metro. In practice, that often meant Dagger modules using` @Module` , for example:


```text
java    1
2   @Module
3   public     abstract     class     AndroidModule     {
4        @Provides
5        static     ContentResolver     provideContentResolver  (  Application   context  )     {
6            return   context  .  getContentResolver  (  )  ;
7        }
8   }
```


And classes with an` @Inject` constructor, for example:


```text
java    1  public     class     EmailScrubber     implements     InsertingScrubber     {
2        @Inject
3        public     EmailScrubber  (  )     {     .  .  .  }
4   }
```


### Removing` @ContributesBinding.rank`


[Anvil's](https://github.com/square/anvil/blob/main/annotations/src/main/java/com/squareup/anvil/annotations/ContributesBinding.kt#L117-L133)` @ContributesBinding` annotation has a` rank` parameter that can automatically deduplicate contributions for the same binding. This is helpful when Gradle modules don't depend on each other and therefore cannot use the strongly typed` replaces` parameter. For example, we use this feature in device tests to replace real implementations with fakes.


[Metro's](https://github.com/ZacSweers/metro/blob/main/runtime/src/commonMain/kotlin/dev/zacsweers/metro/ContributesBinding.kt)` @ContributesBinding` annotation doesn't support` rank` , so we had to migrate hundreds of usages. Our[Gradle module structure](https://ralf-wondratschek.com/presentation/android-at-scale-at-square.html) enforces dependency inversion, which means implementation modules aren't allowed to depend on other implementation modules. As a result, they can't use` replaces` to reference the implementation they supersede.


```text
kotlin    1  // :public module
2   interface   LoginHandler
3
4   // :impl-real
5   @ContributesBinding  (  AppScope  ::  class  )
6   class   RealLoginHandler   :   LoginHandler
7
8   // :impl-fake
9   @ContributesBinding  (  AppScope  ::  class  )
10   class   FakeLoginHandler   :   LoginHandler
```


In this example,` :impl-fake` can't use` @ContributesBinding(AppScope::class, replaces = \[RealLoginHandler::class\])` , because that would require the module to depend on` :impl-real` in order to reference` RealLoginHandler` . Instead, we had to exclude the real implementation explicitly from the test graph, for example:


```text
kotlin    1  @DependencyGraph  (
2      scope   =   AppScope  ::  class  ,
3      excludes   =
4            [
5              RealLoginHandler  ::  class  ,     // FakeLoginHandler is used in tests
6            ]  ,
7   )
8   interface   TestAppGraph
```


### Fixing bugs in Metro


During the migration, we ran into several bugs, performance issues, and missing features in Metro. That isn't surprising for a framework in active development, especially when it is used in a codebase of our size. We fixed some problems ourselves, but more often we relied on Zac's guidance and expertise to turn bug reports into production-ready fixes. Over the course of the migration,[our team merged 58 pull requests](https://github.com/search?q=repo%3AZacSweers%2Fmetro%20is%3Apr%20is%3Amerged%20author%3AvRallev%20author%3AJoelWilcox%20author%3Ajapplin%20author%3Atcmulcahy%20&type=pullrequests) into Metro.


### Anvil extensions


Anvil supported custom code generators, and we used these extensions heavily for patterns unique to Square Android. For example, we have custom` @ContributesRobot` and` @ContributesService` annotations so test robots and Retrofit services can be injected:


```text
kotlin    1  @ContributesRobot  (  LoggedInScope  ::  class  )
2   class   LogOutHandlerRobot   @Inject     constructor  (
3        private     val   logOutHandler  :   LogoutHandler
4   )     :   ScreenRobot  <  LogOutHandlerRobot  >  (  )     {
5        fun     forceLogout  (  )     {
6          logOutHandler  .  forceLogout  (  Normal  )
7        }
8   }
9
10
11   @ContributesService  (  AppScope  ::  class  )
12   interface   AccountStatusService   {
13        @GET  (  "/..."  )
14        fun     getAccountStatus  (  )  :   AccountStatusResponse
15   }
```


Metro doesn't currently provide stable extension points, so our first solution was to rely on KSP, as[Metro recommends](https://zacsweers.github.io/metro/latest/generating-metro-code/) . That unblocked the migration, but KSP added significant build-time overhead, which matched what we had already learned when we explored migrating Anvil to KSP. In a second step, we migrated those KSP processors to our own Kotlin compiler plugin. These extensions are open source:[square/metro-extensions](https://github.com/square/metro-extensions) .


Implementing our extensions as a compiler plugin carries some risk because Metro doesn't guarantee stability for the APIs we rely on. To catch regressions early, we set up a[nightly job](https://github.com/square/metro-extensions/actions/workflows/metro-compatibility.yml) that builds Metro from source and runs our integration tests against it. That setup has already helped us catch breaking changes, report them upstream, and address them.


## Build time improvements


One of Metro's main value propositions is[faster builds](https://zacsweers.github.io/metro/latest/performance/) . Compared with Dagger 2 and Kotlin annotation processing, Metro avoids entire build steps such as generating Java stubs from Kotlin code, running an annotation processor, and compiling the generated Java. Instead, Metro runs inline during the regular Kotlin compilation step that already has to happen:


We compared Dagger 2 + Anvil with Metro + our KSP processors and Metro + our own compiler plugin. We expected improvements based on results from industry partners, but seeing these big leaps in our own project was still surprising:


#### Development app


The table below shows the median build times behind this chart. Positive deltas indicate faster builds.


Scenario Dagger median (ms) Metro + KSP median (ms) KSP delta vs. Dagger Metro + Compiler Plugin median (ms) Compiler Plugin delta vs. Dagger


ABI Change in Account 10,317 6,523 36.8%


4,484 56.5%


ABI Change in Account Service 6,575 4,428 32.6%


3,893 40.8%


ABI Change in App 6,385 4,337 32.1%


5,074 20.5%


ABI Change in Utilities 13,110 9,841 24.9%


7,184 45.2%


Dagger Change in Common Wiring 7,343 4,286 41.6%


3,804 48.2%


Dagger Change in Login Wiring 6,278 4,124 34.3%


3,455 45.0%


Dagger Change in Utilities Wiring 6,694 4,164 37.8%


3,627 45.8%


Non-ABI Change in App 4,051 3,726 8.0%


4,401 -8.7%


Non-ABI Change in Utilities 2,108 1,979 6.1%


1,996 5.3%


#### Production app


Scenario Dagger median (ms) Metro + Compiler Plugin median (ms) Compiler Plugin delta vs. Dagger


ABI Change in Account Public 105,734 55,344 47.7%


ABI Change in Account Impl 63,960 45,691 28.6%


ABI Change in Account Wiring 68,668 47,373 31.0%


ABI Change in Utilities 116,568 68,432 41.3%


Non-ABI Change in Utilities 47,284 45,367 4.1%


Dagger Change in Wiring 56,887 44,720 21.4%


Dagger Change in Utilities Wiring 65,764 48,429 26.4%


We ran Gradle benchmarks on local MacBooks for the first graph, which represents a smaller development app, and on remote workstations for the second graph, which represents a large production app. Depending on the environment and scenario, we measured **total build-time improvements ranging from 5% to 56%** . Even if we assume a conservative average improvement of only 10%, that still **saves us more than 4,800 hours of Gradle build time in CI every week** .


It is rare to see a single change such as adopting Metro deliver gains this large. The build-time improvements alone should make any large Kotlin project seriously evaluate Metro over Dagger 2.


## Conclusion


Metro is an excellent dependency injection framework for Kotlin. It learned from predecessors such as Guice, Dagger, and Anvil, kept the best parts, and removed much of the friction. Zac is also an outstanding maintainer: he responds quickly to issues, ideas, and improvements, and he helps external contributions get over the finish line.


After nearly nine months and 850+ pull requests, we successfully migrated more than 7,000 Gradle modules, 1,500 CI jobs, over 300 development apps, and 22 production apps from Dagger 2 and Anvil to Metro. That unblocked the Kotlin 2 compiler adoption for Square Android, which otherwise would have prevented us from upgrading to Kotlin 2.4 or newer. Build times also dropped significantly, improving productivity both locally and in CI for humans and agents.


Reaching this point took the work of many people across the team, including former teammates whose work was essential to making it successful. We're proud of the result and excited about the next steps for Metro.
