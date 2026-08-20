---
schema_version: "1.0.0"
document_id: "57ecdcc6947dc1f3fe509bdc7e16f04430906400a6f0adacf4a044c5e5e41ce5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-kotlin-4-is-now-available"
published_at: "2024-08-02T07:52:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:d9e5a645dadb9b9e50d13c8b3d31f18ad9b1fbfd57e0b9ac15bedb8a84ba6283"
---

# Apollo Kotlin 4 is now available

Apollo Kotlin 4 is the new major version of Apollo Kotlin. It builds on top of Apollo Kotlin 3 and brings a number of tooling and API improvements:


- A brand new IDE plugin for all your refactoring needs.
- Improvement to the error handling APIs, making it easier to handle errors in a single place.
- WasmJs support.
- A multiplatform GraphQL parser.
- A preview of semantic nullability.
- Network monitoring API, new WebSocket APIs and more …


For a comprehensive list of all changes, please review the[full changelog](https://github.com/apollographql/apollo-kotlin/releases/tag/v4.0.0) .


If you’re currently using Apollo Kotlin 3, refer to the[migration guide](https://www.apollographql.com/docs/kotlin/v4/migration/4.0/#moved-artifacts) for step-by-step migration instructions.


Or read on for a brief summary of the key updates and a preview of what’s on the horizon.


## Android Studio/IntelliJ plugin


If you’re using Android Studio or IntelliJ,[you can now download the Apollo IntelliJ plugin in the IntelliJ marketplace](https://www.apollographql.com/docs/kotlin/v4/testing/android-studio-plugin/) and get even more help from your IDE.


Apollo IDE plugin core features include:


- On the fly code generation
- GraphQL <-> Kotlin navigation
- Unused field inspection
- High Field latency inspection
- Normalized cache viewer
- Schema download
- Automatic migration


That last point means you can now use the plugin to automate most of your v3 -> v4 migration.


## API consolidation


With the release of Apollo Kotlin 4, we’ve achieved a significant milestone where our core APIs—including code generation, parsers, and interceptors—are now stable. This stability means we don’t anticipate any major overhauls like the transition from version 2 to 3, though the landscape of Kotlin and GraphQL continues to evolve rapidly. Developments such as[union types for errors](https://youtrack.jetbrains.com/issue/KT-68296) or[semantic nullability schemas](https://github.com/graphql/nullability-wg/discussions/58) may necessitate breaking changes in the future.


Looking ahead, our goal is to roll out smaller, more frequent updates, including major versions. This approach will keep the core APIs consistent, enabling us to introduce new features and adapt more swiftly to changes in the environment. To facilitate this, we’ve updated the package name from` com.apollographql.apollo3` to` com.apollographql.apollo` . With Apollo Kotlin 4, we are also introducing an evolution policy. This policy will steer the development of future versions and ensure that we provide clear deprecation notices, helping you to transition smoothly and confidently.


## Multiplatform GraphQL parser


Previously, Apollo Kotlin relied on[graphql-js](https://github.com/graphql/graphql-js) for parsing GraphQL documents. In version 2, we transitioned to using[Antlr](https://www.antlr.org/) to generate parsers from` .g4` grammars.


With Apollo Kotlin 4, we’ve taken a significant leap forward by developing our[own recursive descent](https://github.com/apollographql/apollo-kotlin/pull/5047) parser.


The new parser not only performs up to[twice](https://github.com/apollographql/apollo-kotlin/pull/5047#issue-1780385717) as fast as the previous Antlr-based parser in certain scenarios, but it’s also entirely written in Kotlin, ensuring full multiplatform support. This means you can seamlessly integrate it into your Kotlin multiplatform projects just like any other KMP dependency.


Here’s how you can add the new parser to your project:


```text
kotlin {
sourceSets {
getByName("commonMain") {
dependencies {
implementation("com.apollographql.apollo:apollo-ast")
}
}
}
}
```


And here’s how to utilize it at runtime:


```text
// Parse a document
val document = "query { hello }".toGQLDocument()


// transform/inspect it
// And serialize it again
println(document.toUtf8())
```


## Introducing WasmJs support


Apollo Kotlin 4 now includes support for the WasmJs target, expanding your capabilities to use the generated models and` apollo-runtime` in a Wasm application. Experience this new feature firsthand by trying out the Confetti wasm demo at[https://wasm.confetti-app.dev/](https://wasm.confetti-app.dev/) ([GitHub](https://github.com/joreilly/Confetti) ).


## Enhancing nullability handling


In GraphQL,[fields are traditionally nullable by default](https://graphql.org/learn/best-practices/#nullability) . However, the standards set in 2015 don’t always align with the needs of 2024, especially as[the GraphQL community seeks to enhance](https://github.com/graphql/nullability-wg/) type safety for languages like TypeScript, Swift, and Kotlin.


Apollo Kotlin 4 introduces two experimental directives,` @semanticNonNull` and` @catch` , designed to simplify interactions with nullable fields.


The` @semanticNonNull` directive is a server-side directive, indicating that a field is guaranteed to be non-null unless an error happens. This directive can be incorporated into your` .graphqls` schema extensions, enhancing the clarity and reliability of your data models:


```text
# name is never null unless an error happens
extend type User @semanticNonNullField(name: "name")
```


On the code generation front, Apollo Kotlin recognizes` @semanticNonNullField` and accordingly generates non-null Kotlin properties, ensuring that your application code remains type-safe and concise.


Additionally, the` @catch` directive is a client-side directive for opting into specific error handling strategies during operations. By using` @catch` in your` .graphql` operations, you can manage how errors are handled directly within the data context—whether by mapping errors to` null` , re-throwing them, or using another method:


```text
query GetUser {
user {
# map name to FieldResult<String?> instead of stopping parsing
name @catch(to: RESULT)
}
}
```


For a deeper understanding of these enhancements and their implications on your projects, please refer to our detailed[nullability documentation page](https://www.apollographql.com/docs/kotlin/v4/advanced/nullability#handle-errors-and-receive-partial-data-with-catch) .


## Looking ahead


Beyond the updates already discussed, Apollo Kotlin 4 introduces several exciting features including support for[@oneOf](https://github.com/graphql/graphql-spec/pull/825) , new WebSockets APIs, new low-level Apollo compiler plugin APIs, and new Gradle APIs. For a detailed list of all the enhancements, make sure to read the full changelog.


One significant area poised for further development is the normalized cache, which continues to present challenges. While Apollo Kotlin 3 introduced[@typePolicy and @fieldPolicy for declarative ids](https://www.apollographql.com/docs/kotlin/caching/declarative-ids/) , certain scenarios still demand the use of complex programmatic APIs.


Additionally, issues such as garbage collection and invalidation remain unresolved. We aim to address these in version 5.


With the modularization of Apollo Kotlin development has shifted to[a separate repository](https://github.com/apollographql/apollo-kotlin-normalized-cache-incubating/) , enabling more agile iteration and improvement. For the latest updates and future plans, please refer to[the ROADMAP](https://github.com/apollographql/apollo-kotlin/blob/main/ROADMAP.md) document on what is being worked on.


## Join the community


Have a question or wish to discuss GraphQL, Kotlin, or the[ROADMAP](https://github.com/apollographql/apollo-kotlin/blob/main/ROADMAP.md) ? We encourage you to connect with us and join the conversation:


- [Github](https://github.com/apollographql/apollo-android/issues)
- [Community forums](http://community.apollographql.com/new-topic?category=Help&tags=mobile,client)
- Join the[Apollo KotlinLang slack channel](https://app.slack.com/client/T09229ZC6/C01A6KM1SBZ) (click[here](https://slack.kotl.in/) for an invite).


We’re here to support you and look forward to your insights and contributions!


Written by


Martin Bonnin


[Read more by Martin Bonnin](https://www.apollographql.com/blog/author/martin)
