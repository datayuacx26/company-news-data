---
schema_version: "1.0.0"
document_id: "55d8fd864ff8b58f261f85484ca79a102ebb1bdb229a3950126363aca3968473"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-kotlin-5-is-now-available"
published_at: "2026-05-13T12:00:25+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:13:13.812190+00:00"
content_hash: "sha256:62a9fa2e5783be0c6dfdd35f1538b336dea00580240914dc4faff9780277a5c5"
---

# Apollo Kotlin 5 Is Now Available

We’re excited to announce Apollo Kotlin 5 is now available.


Apollo Kotlin 3 was a full rewrite of Apollo Android for Kotlin multiplatform. Apollo Kotlin 4 reworked error handling, introduced the Apollo IDE plugin and semantic nullability.


Apollo Kotlin 5 is[GraphQL Golden Path](https://goldenpath.graphql.org/) ready, comes with a[new normalized cache](https://www.apollographql.com/blog/the-new-apollo-kotlin-normalized-cache) , a rework of the Gradle plugin, new compiler plugins APIs, agent skills,` linuxX64` and` watchosDeviceArm64` targets and more.


If you’re currently using Apollo Kotlin 4, you should feel right at home with Apollo Kotlin 5. Most APIs are untouched. For the others, read the[migration guide](https://www.apollographql.com/docs/kotlin/v5/migration/5.0) for details.


For a comprehensive list of all changes, please review the[full changelog](https://github.com/apollographql/apollo-kotlin/releases/tag/v5.0.0) or read on for a brief summary of the key updates.


## GraphQL Golden Path


Apollo Kotlin aims to support the latest version of the[GraphQL draft specification](https://spec.graphql.org/draft/) . Making a change to the specification draft is a long and near-irreversible process. Yet experimentation matters: it gives the community confidence that proposed changes are sound before they ship.


For this reason, Apollo Kotlin 5 supports a number of experimental[GraphQL RFCs](https://rfcs.graphql.org/) :


- [Fragment arguments](https://github.com/graphql/graphql-spec/pull/1081)
- [Service capabilities](https://github.com/graphql/graphql-spec/pull/1163)
- [Incremental 0.2](https://specs.apollo.dev/incremental/v0.2/) protocol for @defer and @stream
- [onError request parameter](https://github.com/graphql/graphql-spec/pull/1163)
- [Directives on directives](https://github.com/graphql/graphql-spec/issues/566)
- [Schema coordinates](https://github.com/graphql/graphql-wg/blob/main/rfcs/SchemaCoordinates.md)


Make sure to join an[upcoming working group](https://github.com/graphql/graphql-wg/) and share feedback if you’re using any of these.


## Normalized cache


Apollo Kotlin 5 comes with a new, separately versioned, normalized cache that supports:


- TTL
- Garbage collection
- Pagination
- Binary format
- Partial results


For more details, read the[dedicated blog post](https://www.apollographql.com/blog/the-new-apollo-kotlin-normalized-cache) .


## Modernization


The Gradle plugin now uses[Gratatouille classloader isolation](https://github.com/gradleup/gratatouille) , instead of[GR8 relocation](https://github.com/GradleUp/gr8) previously. This makes the plugin more robust and easier to debug.


Apollo Kotlin 5 uses KGP 2.3, with 2.1 compatibility for JVM and Android consumers. Native and JS consumers must compile with KGP 2.3+.


If you are using Apollo Kotlin with AI agents, the[Apollo Kotlin agent skill](https://github.com/apollographql/skills/tree/main/skills/apollo-kotlin) is now available. Agents can use it to discover the Apollo Kotlin best practices.


## Migration path


Previous` DeprecationLevel.WARNING` symbols are now` DeprecationLevel.ERROR` . Previous` DeprecationLevel.ERROR` symbols are removed.


Most of the runtime APIs as well as the package name are unchanged. If your build has no Apollo deprecation warnings on v4, the upgrade should require minimal changes.


The main breaking changes are in experimental Data Builders and Apollo Compiler Plugins.


See the[v5 migration guide](https://www.apollographql.com/docs/kotlin/v5/migration/5.0) for a complete upgrade walkthrough.


## Update today


Apollo Kotlin 5 is now available on Maven Central:


```text
plugins {
id("com.apollographql.apollo").version("5.0.0")
}


dependencies {
implementation("com.apollographql.apollo:apollo-runtime:5.0.0")
}
```


Any feedback?[Let us know what you think!](https://github.com/apollographql/apollo-kotlin/issues/new?template=3_other.yaml) The team is looking forward to seeing what you build!


Written by


Martin Bonnin


[Read more by Martin Bonnin](https://www.apollographql.com/blog/author/martin)
