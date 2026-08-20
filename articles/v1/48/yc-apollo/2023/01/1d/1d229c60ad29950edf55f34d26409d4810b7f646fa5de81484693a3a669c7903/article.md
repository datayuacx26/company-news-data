---
schema_version: "1.0.0"
document_id: "1d229c60ad29950edf55f34d26409d4810b7f646fa5de81484693a3a669c7903"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/4-new-years-resolutions-for-your-kotlin-apps"
published_at: "2023-01-10T09:11:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:dd3013dc9baf82c70edbfbc52c825951ad6881cbb2408cee8b58a532a6d32544"
---

# 4 New Year’s resolutions for your Kotlin apps

Happy New Year everyone 🥳. I hope 2023 brings you joy, happiness and loads of type safety!


To kickstart the year, here’s a small list of tips & tricks for working with GraphQL in a Kotlin app. These tips have nothing in common besides the fact that they are easy to implement and will make your life easier.


It’s a list of things that came up in GitHub issues, conferences and/or other discussions. Very often the GraphQL setup was been done some years ago and it’s easy to miss these quality of life improvements.


It might be that you’re doing some or all of them already. If that’s the case then congrats, you’ve earned yourself a good cup of coffee ☕! If not, let’s dive in (and grab a good cup of coffee too 😃!).


## 1. Use SDL for your schema


Early versions of Apollo Kotlin only had support for[introspection](https://graphql.org/learn/introspection/) JSON. This is convenient because you can get it directly from your GraphQL endpoint. It is **very** verbose though and reading it is **very** cumbersome. To give you and idea, here’s a small sample (full JSON available[here](https://github.com/apollographql/apollo-kotlin/blob/05f2a3295517fb7a64c2259c38b453a37f3c4d19/libraries/apollo-gradle-plugin/testProjects/multiplatform/src/commonMain/graphql/schema.json#L7) ):


```text
{
"data": {
"__schema": {
"queryType": {
"name": "Root"
},
"mutationType": null,
"subscriptionType": null,
"types": [
{
"kind": "OBJECT",
"name": "Root",
"description": null,
"fields": [
// ~500 lines skipped
]
},
// ~5000 lines skipped
],
// ...
}
}
}
```


All in all, the introspection JSON is 5843 lines long for ~50 actual GraphQL types. That’s way too much. Thankfully,[SDL](https://www.apollographql.com/tutorials/lift-off-part1/schema-definition-language-sdl) makes it a lot more concise. SDL is[the part of the GraphQL language](https://graphql.org/learn/schema/) that deals with type definitions. The same schema translated to SDL looks like this (full SDL available[here](https://gist.github.com/martinbonnin/e1d9773521e0e4033c829d9f743820c6) ):


```text
type Root {
allFilms(after: String, first: Int, before: String, last: Int): FilmsConnection


# ~30 lines skipped
}


"""
A single film.
"""
type Film implements Node {
"""
The title of this film.
"""
title: String


# ~50 lines skipped
}


# more types
```


Much better, right? The good news is you can convert your existing` schema.json` to a` schema.graphqls` SDL file:


```text
$ ./gradlew convertApolloSchema --from src/main/graphql/schema.json
--to src/main/graphql/schema.graphqls
$ rm src/main/graphql/schema.json
```


Moving forward, when updating your schema, you can ask your backend team for the SDL file or directly convert on the fly:


```text
$ ./gradlew downloadServiceApolloSchemaFromIntrospection
```


If your schema has a` .graphqls` extension, Apollo Kotlin will recognize it and convert it automatically to SDL.


## 2. Update your schema in GitHub actions


Talking about updating your schema, typing` ./gradlew downloadServiceApolloSchemaFromIntrospection` is verbose and it’s easy to forget doing it. You could tweak your build to download it every time but this has two major drawbacks:


1. it makes your builds longer as they now need to download a file all the time.
2. it makes your builds non-reproducible as the schema will depend on when you run your builds. If you checkout an older tag, it will download your recent schema.


For these reasons, we recommend committing your schema in source control and updating it in a cron job. We have created a GitHub action for this:[update-graphql-schema](https://github.com/apollographql/update-graphql-schema) . To use it, create a new GitHub workflow file named` update-graphql-schema.yaml` and copy paste the following contents (update endpoint and schema):


```text
name: Update GraphQL schema
on:
schedule:
# every night at midnight (GitHub actions time)
- cron: "0 0 * * *"


jobs:
update:
runs-on: ubuntu-20.04
steps:
- uses: actions/checkout@v2
- uses: apollographql/update-graphql-schema@4b4517cad731e2564488f203de81937dcc4ef92e #main
with:
schema: "src/main/graphql/schema.graphqls"


# With GraphOS
key: "service:fullstack-tutorial:abc123"


# With introspection
endpoint: "https://example.com/graphql"


```


Whenever your schema changes, a new pull request is created:


You can view in action in the Confetti repo ([yaml file](https://github.com/joreilly/Confetti/blob/201cc4cb23bd1cd61ba116a3e4bbb82919eb52ea/.github/workflows/update-schema.yml) ,[example pull request](https://github.com/joreilly/Confetti/pull/130) ). Not only does it save you from updating manually but if using SDL, it shows the changes in a nice visual way!


## 3. Fail on deprecated fields usages


When you update your schema, your new schema might contain newly[deprecated fields](https://www.apollographql.com/tutorials/lift-off-part5/field-deprecation) . That’s one of the nice things with GraphQL. You can evolve your API progressively. When a field is deprecated, it’s good to stop using it so that the backend team can ultimately remove it.


By default Apollo Kotlin does two things:


1. It displays a warning during codegen.
2. It marks generated classes with[Kotlin’s @Deprecated annotation.](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-deprecated/) This way, your IDE can show it as strikethrough and` kotlinc` can itself display a warning.


Warnings are easy to ignore though and if you do not edit the code that is using deprecated fields, you might continue using the field for longer than necessary.


If you want to be strict about not using deprecated GraphQL fields, set` failOnWarnings` to true:


```text
apollo {
service("service") {
packageName.set("com.example")


warnOnDeprecatedUsages.set(true)
failOnWarnings.set(true)
}
}
```


Whenever you’ll try to use a deprecated field your build will fail (and that’s a good thing!):


```text
> Task :shared:generateServiceApolloSources FAILED
w: /Users/mbonnin/git/Confetti/shared/src/commonMain/graphql/Queries.graphql: (17, 9): Apollo: Use of deprecated field `name`


FAILURE: Build failed with an exception.


* What went wrong:
Execution failed for task ':shared:generateServiceApolloSources'.
> Apollo: Warnings found and 'failOnWarnings' is true, aborting.
```


## 4. Try out the data builders


When it comes to testing, you had 2 options with early versions of Apollo Kotlin:


1. enqueue a fake JSON into a mock server like[OkHttp MockWebServer](https://github.com/square/okhttp/tree/master/mockwebserver) . This works but requires maintaining a set of` .json` Strings that are not easy to create manually. You could intercept the network traffic to get some real life samples but doing that and maintaining it is cumbersome
2. call the model constructors manually. This works too but for models that have a lot of properties, it’s tedious to pass them all. What’s more merge fields require to duplicate some properties and it’s easy to create inconsistent models by passing a wrong` __typename`


Apollo Kotlin 3.6.0 introduced[data builders](https://www.apollographql.com/docs/kotlin/testing/data-builders) . Data builders intend to solve both issues by generating builders for your schema types.


*Note:* contrary to the models, the data builders are based on the schema and not the operation. This means it is possible to set a value in your builders that is never going to be read by a certain operation. This is a tradeoff to avoid the exponential blowup issues that come with[responseBased codegen](https://github.com/apollographql/apollo-kotlin/issues/3144) and allows sharing builders amongst different operations.


To enable the data builders, set` generateDataBuilders` to true in your Gradle files:


```text
apollo {
service("service") {
// ...
generateDataBuilders.set(true)
}
}
```


That generates builders that you can then use with a` Data {}` function:


```text
val data = GetHeroQuery.Data {
hero = buildHuman {
firstName = "John"
age = 42
friends = listOf(
buildHuman {
firstName = "Jane"
},
buildHuman {
lastName = "Doe"
}
)
ship = buildStarship {
model = "X-Wing"
}
}
}
```


To get a Json out of it, you can use` Data.toJsonString()` (JVM/Android only):


```text
val data = GetHeroQuery.Data { ... }
mockServer.enqueue(data.toJsonString())
```


You can read more in the[official doc](https://www.apollographql.com/docs/kotlin/testing/data-builders)


## Conclusion


That’s it for now! I hope you enjoyed this little tour. As always,[feedback is very welcome](https://github.com/apollographql/apollo-kotlin/issues/new/choose) ! You can also take a look at the[ROADMAP](https://github.com/apollographql/apollo-kotlin/blob/main/ROADMAP.md) to get a glimpse of what’s coming in 2023.


Written by


Martin Bonnin


[Read more by Martin Bonnin](https://www.apollographql.com/blog/author/martin)
