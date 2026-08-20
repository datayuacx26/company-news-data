---
schema_version: "1.0.0"
document_id: "bc3600e73c44e3f3e83853ed099b12717b23ae88cf3f36cce338ea67edd51da5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/journey-to-k2-using-the-new-compiler-in-apollo-kotlin"
published_at: "2024-06-19T08:26:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:06701f611eb56e92be8202b6d713051cd902fd9c054c02b9a7e07c62c606a004"
---

# Journey to K2: using the New Compiler in Apollo Kotlin

Apollo Kotlin is now built with K2, the new compiler that has recently been released with Kotlin 2. Here are a few details on the migration process.


## **What is K2?**


K2 is a complete rewrite of the Kotlin compiler frontend, « *the part of the compiler that parses your code and performs the semantic analysis, data flow analysis, call resolution, and type inference »* ([source](https://blog.jetbrains.com/kotlin/2023/02/k2-kotlin-2-0/) ). The result is a faster, more robust and reliable compiler that should also be easier to evolve in the future.


## **Switching to K2 in Apollo Kotlin**


On Apollo Kotlin, we started looking at K2 early on, so we could be ready when it’s stable. A nice practice that helps catching issues early is to set up[shadow jobs](https://slack.engineering/shadow-jobs/) – in our case though that wasn’t practical, since it took a while before we could reach a state where the project would build. Instead we just maintained the necessary changes in a[dedicated branch](https://github.com/apollographql/apollo-kotlin/tree/keep-k2) in parallel to our main one. Towards the end, we merged all non-breaking changes to the main branch, so as to keep the diff minimal.


In a perfect world, using one compiler or another should have no impact for a precisely specified language. In reality though, while this should be the case for most projects, the chances of stumbling upon behavior differences increase with the project size.


In practice, when switching the project to K2, we got compilation errors that needed tweaks in our code. This could be due to constructs that were allowed in K1 but shouldn’t have been and that K2 had fixed. Sometimes they were just small differences in the way the language could be used. We’ve also seen a few actual bugs in the new compiler.


This process – an iterative task with each beta and rc release – took a while and we’ve been blocked a few times, but in the end, the transition has been smooth overall. For the curious, a list of the few misc issues we’ve hit is available[here](https://gist.github.com/BoD/29ff99fb695a3ce5e82b28159cdc5268) . These were all fixed or had workarounds in the stable release of Kotlin 2.


On the Compose side, one notable change with Kotlin 2 is that the Compose compiler now resides[in the Kotlin repository](https://github.com/JetBrains/kotlin/tree/master/plugins/compose) . This should help homogenize the release cycles and version numbers between Kotlin and the Compose compiler. A[dedicated Gradle plugin](https://developer.android.com/develop/ui/compose/compiler) is now available, which automatically imports the right version whereas this used to be a manual configuration.


## **Build performance**


The main promise of K2 is improved build speeds. We’re happy to report that we’ve noticed some improvement when building the whole library:


- with K1: **1 minute 40 seconds**
- with K2: **1 minute**


*(Measured by running time* *./gradlew –no-build-cache –rerun-tasks publishToMavenLocal* *on a 2021 MacBook Pro with a 10 core M1 Max chip).*


That’s a **40%** improvement!


If we look at build scans ([K1](https://ge.apollographql.com/s/qc442j4pu6uvq/timeline?view=by-type) ,[K2](https://ge.apollographql.com/s/co2mdwfrfh6pw/timeline?view=by-type) ), we can see that most of the time is spent in the various compilation tasks, Native being the most time consuming. The JS compilation is where most improvement is achieved.


**Task Type** **Duration (K1)** **Duration (K2)** **Improvement**


KotlinNativeCompile 1m 33s 1m 10s 25%


KotlinCompile (JVM) 26s 18s 30%


Kotlin2JsCompile 14s 8s 42%


KotlinCompileCommon (Metadata) 5s 3s 40%


*(Parallelization disabled for a better understanding of time spent per task.)*


## **Compatibility**


On Android and the JVM, the Kotlin compiler has[N+1 forward compatibility](https://kotlinlang.org/docs/kotlin-evolution.html#evolving-the-binary-format) . The documentation states it is a best effort but in our experience, it has always worked. So a library compiled with K2 and languageVersion of 2.0 (such as Apollo Kotlin 4, coming soon!) can be used on projects using Kotlin 1.9+.


For native[and JS](https://github.com/apollographql/apollo-kotlin/issues/5801) , where the ecosystem is still moving fast, that level of forward compatibility is not there yet, so Kotlin 2 is needed.


(A discussion about this can be seen in[this issue](https://github.com/apollographql/apollo-kotlin/issues/5856) ).


## **What’s next?**


The **Kotlin Symbol Processing** tool (KSP) has a[version 2](https://github.com/google/ksp/blob/main/docs/ksp2.md) specifically tailored to work with K2. At the time of writing, we’re hitting[an issue](https://github.com/google/ksp/issues/1823) that prevents us from making the switch just yet.


For its refactoring, inspection and navigation features, the[Apollo IDE plugin](https://www.apollographql.com/docs/kotlin/v4/testing/android-studio-plugin) interacts with the Kotlin IDE plugin, which currently uses K1 under the hood. New[APIs based on K2](https://blog.jetbrains.com/idea/2024/03/k2-kotlin-mode-alpha-in-intellij-idea/) are coming, and we’ll update our plugin code to use them when it is time.


Finally, one goal of K2 has been to ease the evolution of the language. A few **future language features** have been announced:[Guards](https://github.com/Kotlin/KEEP/blob/guards/proposals/guards.md) ,[Context sensitive resolution](https://github.com/Kotlin/KEEP/issues/379) ,[Explicit backing fields](https://github.com/Kotlin/KEEP/blob/explicit-backing-fields-re/proposals/explicit-backing-fields.md) ,[Data arguments](https://youtrack.jetbrains.com/issue/KT-15471) ,[Union types for errors](https://youtrack.jetbrains.com/issue/KT-68296) . We’ll keep an eye on them!


## **Conclusion**


Switching to the new K2 compiler in Apollo Kotlin has been an interesting journey, which has brought improvements to our build performance.


We recommend[upgrading](https://kotlinlang.org/docs/k2-compiler-migration-guide.html) your own project to benefit from enhancements and to stay in line with future developments.


As always, feedback is welcome – don’t hesitate to reach out on[GitHub](https://github.com/apollographql/apollo-kotlin) , the[Apollo Community](https://discord.gg/graphos) , or the[Kotlin Slack](https://slack.kotl.in/) !


Written by


Benoit Lubek


[Read more by Benoit Lubek](https://www.apollographql.com/blog/author/blubek)
