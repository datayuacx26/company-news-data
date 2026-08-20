---
schema_version: "1.0.0"
document_id: "ad3a4f693238f7d57e67fef4dc4ed32e9284a876a7b04c6d843d7b52569a4706"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/whats-new-with-multiplatform-in-apollo-kotlin"
published_at: "2022-05-25T10:25:23+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:820a30dd26985fb9e7452001c5d347c84c656684f5a3724bcb47330f859df027"
---

# What’s new with Multiplatform in Apollo Kotlin

*« Write once, run anywhere »* : that age-old dream is still alive today. That is why, when making Apollo Kotlin (then known as Apollo Android) a 100% Kotlin codebase, making it also[Multiplatform](https://kotlinlang.org/docs/multiplatform.html) was inevitable. The potential advantages for users would well be worth the overhead of writing agnostic code. Since then, the Multiplatform ecosystem has been evolving and Apollo Kotlin is keeping up with it.


## Hierarchical project structure


Let’s start with the[hierarchical project structure](https://kotlinlang.org/docs/multiplatform-hierarchy.html) (a.k.a. HMPP) which enables code-sharing among several platforms – for instance, having a unique source set for both iOS Arm64 and X64 is now achieved without any custom scripting. This was introduced[with Kotlin 1.4.0](https://kotlinlang.org/docs/whatsnew14.html#sharing-code-in-several-targets-with-the-hierarchical-project-structure) and is now enabled by default[since Kotlin 1.6.20](https://kotlinlang.org/docs/whatsnew1620.html#kotlin-multiplatform) .


Apollo Kotlin has made the switch in the[latest release, 3.3.0](https://github.com/apollographql/apollo-kotlin/releases/tag/v3.3.0) .


What this means for your project:


- If you’re already using Kotlin 1.6.20+, you’re good to go ✅! If not, we recommend you update to it.
- Otherwise, enable HMPP on your project:


```text
kotlin.mpp.enableGranularSourceSetsMetadata=true
kotlin.native.enableDependencyPropagation=false
```


When using Kotlin < 1.6.20, you’ll also need to disable the` KotlinMetadata` task due to[issue KT-51970](https://youtrack.jetbrains.com/issue/KT-51970) , like so:


```text
// Workaround for https://youtrack.jetbrains.com/issue/KT-51970
afterEvaluate {
afterEvaluate {
tasks.configureEach {
if (
name.startsWith("compile")
&& name.endsWith("KotlinMetadata")
) {
println("disabling ${this@subprojects}:$name")
enabled = false
}
}
}
}
```


This workaround is not applicable if your project is a library, though – in that case you’ll need to update to Kotlin 1.6.20+.


## New Memory Manager in Kotlin Native Projects


If you have been writing native code in Kotlin, you probably have encountered these messages:


- ` IncorrectDereferenceException: illegal attempt to access non-shared com.example.MyObj from other thread`
- ` InvalidMutabilityException: mutation attempt of frozen com.example.MyObj`


These are due to Kotlin Native’s[specific concurrency rules](https://kotlinlang.org/docs/multiplatform-mobile-concurrency-overview.html) .


The[new memory manager](https://github.com/JetBrains/kotlin/blob/master/kotlin-native/NEW_MM.md) (currently alpha) makes these messages a thing of the past and multiplatform development a definitely nicer experience! Apollo Kotlin is already compatible, so you can enable it on your project with this flag:


```text
kotlin.native.binary.memoryModel=experimental
```


Since the memory manager concerns executables rather than libraries, Apollo Kotlin’s code must abide by the old rules to stay compatible with projects using the old manager – which we’ll keep on doing in the short term.


Apollo Kotlin Your project Compatibility


Assumes old MM (today) Old MM ✅


Assumes old MM (today) New MM ✅


Assumes new MM (the future) New MM ✅


Assumes new MM (the future) Old MM 💥


What’s next? We will monitor feedback and the progress on the new manager and eventually adopt it in Apollo Kotlin’s code.


To help us decide on the right time to do this, we’d love your comments on[ticket #4100](https://github.com/apollographql/apollo-kotlin/issues/4100) , or hop on the` #apollo-kotlin` channel on the[Kotlin Slack](https://slack.kotl.in/) !


## Conclusion


Today, Kotlin Multiplatform is still considered experimental, but it is becoming solid and nice to use. Apollo Kotlin 3.3.0 has adopted the HMPP convention and we will continue to closely follow Kotlin Multiplatform’s evolutions so you can take advantage of it in your projects! As always, we love to hear your feedback, on[GitHub](https://github.com/apollographql/apollo-kotlin) , the[Apollo Community](https://community.apollographql.com/) , or the[Kotlin Slack](https://slack.kotl.in/) .


Written by


Benoit Lubek


[Read more by Benoit Lubek](https://www.apollographql.com/blog/author/blubek)
