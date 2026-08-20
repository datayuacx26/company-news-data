---
schema_version: "1.0.0"
document_id: "8014869d612f439410d7b64784ff769cd7d7c0c652e47a6d0b837c227aaa3e87"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-kotlin-new-multiplatform-memory-manager"
published_at: "2022-08-09T12:05:22+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:b7b9a987caf807e3db12f32019a554c5eb234dfebdf567184aaff6da913f40b8"
---

# Apollo Kotlin: New Multiplatform Memory Manager

[Back in May](https://www.apollographql.com/blog/mobile/kotlin/whats-new-with-multiplatform-in-apollo-kotlin/) , we shared about how Apollo Kotlin started supporting the Hierarchical project structure. Following the Kotlin Multiplatform ecosystem, here is some more news, this time about the new Memory Manager.


## Kotlin Native’s Memory Manager


Since its early days, Kotlin Native has had[specific rules](https://kotlinlang.org/docs/multiplatform-mobile-concurrency-overview.html) regarding multithreading and immutability:


- objects could only be accessed from the thread they were created in
- unless they had been “frozen”, in which case they could be accessed by multiple threads, but became immutable


These native-specific restrictions have been a source of friction in writing and sharing code among targets.


In Apollo Kotlin, following these rules meant that the library had to be accessed strictly from the Main thread on Apple targets, which was a hindrance.


Kotlin Native’s[new memory manager](https://blog.jetbrains.com/kotlin/2021/08/try-the-new-kotlin-native-memory-manager-development-preview/) ([now beta](https://kotlinlang.org/docs/whatsnew-eap.html#the-new-kotlin-native-memory-manager-is-enabled-by-default) ) removes these specificities and with it, native code can for the most part be written like JVM code when it comes to concurrency. After some months of being opt-in, it will be the default starting with[Kotlin 1.7.20](https://blog.jetbrains.com/kotlin/2022/08/kotlin-1-7-20-beta/) .


**Apollo Kotlin made the switch in[version 3.5.0](https://github.com/apollographql/apollo-kotlin/releases/tag/v3.5.0) .**


What does it mean for you?


- If your Multiplatform / Native project hasn’t yet switched to the new MM, you will need to[do so](https://github.com/JetBrains/kotlin/blob/master/kotlin-native/NEW_MM.md#enable-the-new-mm) before upgrading to 3.5.0:


```text
// build.gradle.kts


kotlin.targets.withType(KotlinNativeTarget::class.java) {
binaries.all {
binaryOptions["memoryModel"] = "experimental"
}
}
```


- Calling the library exclusively from the Main thread on Apple is no longer required *
- You can now use[Kotlin’s runTest](https://github.com/Kotlin/kotlinx.coroutines/blob/master/kotlinx-coroutines-test/README.md) in all unit tests
- Initial benchmarks[indicate](https://github.com/apollographql/apollo-kotlin/pull/4287/files?w=1#diff-aead75359419ef1647be74310bd7093e4cc2d9393917d17029d5fcc5e11ce1ef) that performance is improved by up to 45% under the new memory manager!


**Suspend functions still need to be run from the Main thread from Swift / Objective C code – see[KT-51297](https://youtrack.jetbrains.com/issue/KT-51297) for more information.*


## Conclusion


Kotlin Multiplatform continues to evolve, and with its new Memory Manager, it is becoming easier to use. By adopting it in its codebase, Apollo Kotlin 3.5.0 is able to get rid of a few threading and testing-related intricacies. We will continue to closely follow Kotlin Multiplatform’s evolutions so you can take advantage of it in your projects! As always, we love to hear your feedback, on[GitHub](https://github.com/apollographql/apollo-kotlin) , the[Apollo Community](https://community.apollographql.com/) , or the[Kotlin Slack](https://slack.kotl.in/) .


Written by


Benoit Lubek


[Read more by Benoit Lubek](https://www.apollographql.com/blog/author/blubek)
