---
schema_version: "1.0.0"
document_id: "80719365f15331478b1fdf8a2a087996963d680887b4e16d452c3f7ab9cd9882"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-apollo-kotlin"
published_at: "2021-12-16T15:15:14+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:33f3dc26ccdf87616fa42881cefa3b9ecb81412cb46df6bd496e9dc347e1d1ae"
---

# Introducing Apollo Kotlin

We’re thrilled to announce Apollo Kotlin!


For the past while we’ve been heads down converting Apollo Android to be 100% Kotlin based, which means Apollo Android now works with any Kotlin based application, website or even server. Because of this, **we’re renaming the project to Apollo Kotlin** !


Apollo Kotlin is a type-safe, caching GraphQL client. It takes your GraphQL queries and generates models that you can use in your application without having to deal with parsing JSON or passing around` Map` s and making clients cast values to the right type manually.


When Apollo Android started in 2016, it supported Java code generation and a callbacks API.


Apollo Android 1.0.0[introduced](https://github.com/apollographql/apollo-android/releases/tag/1.0.0) Kotlin code generation.


Apollo Android 2.0.0[introduced](https://github.com/apollographql/apollo-android/releases/tag/v2.0.0) a separate` apollo-runtime-kotlin` artifact with support for multiplatform queries and subscriptions.


Apollo Kotlin 3.0.0 takes this one step further and unifies both artifacts into a single` apollo-runtime` artifact, written in Kotlin and supporting multiplatform cache, multiplatform file upload, multiplatform persisted queries, multiplatform query batching, and multiplatform everything else!


Apollo Kotlin 3 is[available today](https://github.com/apollographql/apollo-kotlin/releases/tag/v3.0.0) on Maven Central. If you’re coming from Apollo Android 2, check out the[migration guide](https://www.apollographql.com/docs/android/migration/3.0/) and[documentation](https://www.apollographql.com/docs/android/v2) .


## What’s new


We’re not just changing the name! Apollo Kotlin introduces multiple new features and improvements:


- [coroutine APIs](https://www.apollographql.com/blog/introducing-apollo-kotlin#coroutines) for easier concurrency
- [multiplatform support](https://www.apollographql.com/blog/introducing-apollo-kotlin#multiplatform) makes it possible to run the same code on Android, JS, iOS, MacOS and linux
- [responseBased codegen](https://www.apollographql.com/blog/introducing-apollo-kotlin#response-based) is a new optional codegen approach that models fragments as interfaces
- [SQLite batching](https://www.apollographql.com/blog/introducing-apollo-kotlin#sqlite-batching) makes reading from the SQLite cache significantly faster
- [Test builders](https://www.apollographql.com/blog/introducing-apollo-kotlin#test-builders) offer a simple APIs to build fake models for your tests
- [The @typePolicy and @fieldPolicy](https://www.apollographql.com/blog/introducing-apollo-kotlin#declarative-cache) directives make it easier to define your cache ids at compile time
- [The @nonnull](https://www.apollographql.com/blog/introducing-apollo-kotlin#nonnull) directive catches null values at parsing time, so you don’t have to deal with them in your UI code


### Coroutines APIs


Apollo Kotlin exposes coroutines APIs by default.


Queries and Mutations are` suspend` functions returning an` ApolloResponse` :


```text
val response = apolloClient.query(MyQuery()).execute()


println(response.data)
```


```text
val response = apolloClient.mutation(MyMutation()).execute()


println(response.data)
```


Subscriptions are` Flow<ApolloResponse>` :


```text
apolloClient.subscription(MySubscription()).toFlow().collect { response ->
println(response.data)
}
```


### Multiplatform support


Apollo Kotlin code is multiplatform by default with` expect` /` actual` implementations for platform dependencies. For example, HTTP is handled by the following components:


Platform HTTP Client


Android[OkHttp](https://square.github.io/okhttp/)


JavaScript[Ktor](https://ktor.io/)


Apple[NSURLSesssion](https://developer.apple.com/documentation/foundation/nsurlsession)


For persistence, Apollo Kotlin uses[SQLDelight](https://github.com/cashapp/sqldelight) .


The current feature matrix is:


` jvm`` apple`` js`` linux`


` apollo-api` (models) ✅ ✅ ✅ ✅


` apollo-runtime` (network, query batching, apq, …) ✅ ✅ ✅ 🚫


` apollo-normalized-cache` ✅ ✅ ✅ 🚫


` apollo-adapters` ✅ ✅ ✅ 🚫


` apollo-normalized-cache-sqlite` ✅ ✅ 🚫 🚫


` apollo-http-cache` ✅ 🚫 🚫 🚫


If your favorite platform isn’t listed above, feel free to[reach out](https://github.com/apollographql/apollo-kotlin/issues/new/choose) or[contribute](https://github.com/apollographql/apollo-kotlin/blob/main/CONTRIBUTING.md) it. We’re definitely planning on adding other targets.


### ` responseBased` codegen


Apollo Kotlin’s codegen has been refactored to support[Fragments as Interfaces](https://github.com/apollographql/apollo-android/issues/1854) .


` responseBased` models map 1:1 with the JSON response, meaning you don’t have to type` .fragments` anymore. Given the following query:


```text
query GetHero {
hero {
name
... on Droid {
primaryFunction
}
...humanFragment
}
}
fragment humanFragment on Human {
height
}
```


The codegen will generate 3 models that you can use like so:


```text
when (val hero = response.data?.hero) {
is DroidHero -> println("${hero.name} function is ${hero.primaryFunction}")
is HumanHero -> println("${hero.name} height is ${hero.height}")
is OtherHero -> println("${hero.name} is of type ${hero.__typename}")
}
```


While this looks simpler on the surface,` responseBased` codegen has to unroll all fragments to build models for each response shape. This generates code that grows exponentially as nested fragments are used (see[#3144](https://github.com/apollographql/apollo-android/issues/3144) ).


For this reason,` responseBased` codegen is not enabled by default. You can opt-in with the` codegenModels` Gradle property:


```text
apollo {
// Generate models that map 1:1 with the Json response
codegenModels.set("responseBased") // "operationBased" by default
}
```


- ` responseBased` generates more complex models that map the JSON response 1:1.
- ` operationBased` generates simpler models that map the GraphQL operation 1:1. (default)
- ` compat` is used for backward compatibility with Apollo Android 2.


` responseBased` models map the JSON response and merge fields, which means they can always use streaming parsers and never have to rewind. Since they merge fields however, they also need to unroll all the fragments, generating models that grow exponentially in size as nested fragments are used.


There are tradeoffs involved in picking the right codegen approach to use. If you make limited use of fragments,` responseBased` codegen will be faster and expose more type information. If you have a lot of fragments it can create a lot of generated source, leading to increased build times and increased class loading times.


Read more in[the documentation](https://www.apollographql.com/docs/android/advanced/response-based-codegen/) .


### SQLite batching


Apollo Kotlin batches SQL requests instead of executing them sequentially. This can speed up reading a complex query by a factor of 2x+ ([benchmarks](https://github.com/apollographql/apollo-android/blob/91ad93f7cddbb8f666f1e238893062334656165a/benchmark/README.md#current-results) ). This is especially true for queries that contain lists:


```text
{
"data": {
"launches": {
"launches": [
{
"id": "0",
"site": "CCAFS SLC 40"
},
...
{
"id": "99",
"site": "CCBGS 80"
}
]
}
}
}
```


Reading the above data from the cache would take 103 SQL queries with Apollo Android 2 (1 for the root, 1 for` data` , 1 for` launches` , 1 for each launch). Apollo Kotlin 3 uses 4 SQL queries, executing all the launches at the same time.


### Test builders


Test Builders allow generating fake data using a type safe DSL and provide mock values for fields so that you don’t have to specify them all.


Test builders are opt-in. To enable test builders, add the following to your Gradle scripts:


```text
apollo {
generateTestBuilders.set(true)
}
```


This will generate builders and add them to your test` sourceSets` . You can use them to generate fake data:


```text
// Import the generated TestBuilder
import com.example.test.SimpleQuery_TestBuilder.Data


@Test
fun test() {
// Data is an extension function that will build a SimpleQuery.Data model
val data = SimpleQuery.Data {
// Specify values for fields that you want to control
hero = droidHero {
name = "R2D2"
friends = listOf(
friend {
name = "Luke"
}
)
// leave other fields untouched, and they will be returned with mocked data
// planet = ...
}
}


// Use the returned data
}
```


You can control the returned mock data using the[TestResolver API](https://www.apollographql.com/docs/kotlin/advanced/test-builders/#configuring-default-field-values) :


```text
val myTestResolver = object: DefaultTestResolver() {
fun resolveInt(path: List<Any>): Int {
// Always return 42 in fake data for Int fields
return 42
}
}


val data = SimpleQuery.Data(myTestResolver) {}
// Yay, now every Int field in `data` is 42!
```


### @typePolicy and @fieldPolicy directives


@typePolicy and @fieldPolicy are two new directives that make it possible to define cache keys in a declarative, compile-time checked way.


You define` @typePolicy` by adding an extra[type extension](https://spec.graphql.org/June2018/#sec-Type-Extensions) to a` extra.graphqls` file next to your schema. If you have a` Book` type in your schema, and you want the` isbn` to be the cache key, you can do so like this:


```text
extend type Book @typePolicy(keyFields: "isbn")
```


Because this happens at compile type, the codegen will automatically query` isbn` on every` Book` field queried.


Symmetrically, you can use` @fieldPolicy` to tell the runtime how to compute a cache key from a field and query variables.


Given this schema:


```text
type Query {
book(isbn: String!): Book
}
```


you can tell the runtime to use the` isbn` argument as a cache key with:


```text
extend type Query @fieldPolicy(forField: "book", keyArgs: "isbn")
```


` @typePolicy` is used **after** a network request and will make sure your data is de-duplicated.` @fieldPolicy` is used **before** a network request. It is an optional optimization that will save a network roundtrip.


Read more in[the documentation](https://www.apollographql.com/docs/android/caching/declarative-ids/) .


### @nonnull directive


` @nonnull` turns nullable GraphQL fields into non-null Kotlin properties. This directive can be used when a field being null is generally the result of a larger error that you want to catch during parsing.


```text
query GetHero {
# data.hero will be non-null
hero @nonnull {
name
}
}
```


Like[@typePolicy and @fieldPolicy](https://www.apollographql.com/blog/introducing-apollo-kotlin#declarative-cache) ,` @nonnull` can also be specified on schema types if you want the same rule to apply to all the fields of a certain type. To do so, add a type extension in your` extra.graphqls` file:


```text
extend type Query @nonnull(fields: "hero")
```


Read more in[the documentation](https://www.apollographql.com/docs/android/advanced/nonnull/) .


## Java compatibility


While Apollo Kotlin is written in Kotlin and has Kotlin-first APIs, it can still generate Java models and work with Java codebases.


### apollo-api


Like Apollo Android 2, the` apollo-api` artifact depends on` kotlin-stdlib` . You can use your own HTTP client with no other dependencies ([documentation](https://www.apollographql.com/docs/android/advanced/no-runtime/) ):


```text
// build a body
Buffer buffer = new Buffer();
JsonWriter jsonWriter = new BufferedSinkJsonWriter(buffer);
Operations.composeJsonRequest(query, jsonWriter);
String body = buffer.readUtf8();


// send it to your backend
HttpResponse httpResponse = sendHttpRequest(
"POST",
"https://com.example/graphql",
"application/json",
body
);


// and parse the response
JsonReader jsonReader = BufferedSourceJsonReader(httpResponse.body.source());
ApolloResponse<MyQuery.Data> apolloResponse = Operations.parseJsonResponse(query, jsonReader);
```


### apollo-runtime


Unlike Apollo Android 2, the` apollo-runtime` artifact now depends on[kotlinx.coroutines](https://github.com/Kotlin/kotlinx.coroutines) . To call the Apollo APIs from Java, you can use the[RxJava](https://www.apollographql.com/docs/android/v3/advanced/rxjava/) bindings:


```text
ApolloCall<MyQuery.Data> queryCall = client.query(new MyQuery());
Single<ApolloResponse<MyQuery.Data>> queryResponse = Rx2Apollo.single(queryCall);
queryResponse.subscribe( /* ... */ );
```


We realize that working with coroutines from Java is not always ideal, especially if you’re using a lot of the more advanced APIs like interceptors, store, etc… If you’re in this case, we recommend you stay with Apollo Android 2 and upvote[#3694](https://github.com/apollographql/apollo-android/issues/3694) for more idiomatic Java APIs


## Get it!


To get started, add the following to your` build.gradle\[.kts\]` :


```text
plugins {
id("com.apollographql.apollo3").version("3.0.0")
}


dependencies {
implementation("com.apollographql.apollo3:apollo-runtime:3.0.0")
}


apollo {
packageName.set("com.example")
}
```


Download your schema:


```text
./gradlew downloadApolloSchema \
--endpoint="https://your.domain/graphql/endpoint" \
--schema="app/src/main/graphql/com/example/schema.graphqls"
```


Write a query in a` ${module}/src/main/graphql/GetRepository.graphql` file:


```text
query HeroQuery($id: String!) {
hero(id: $id) {
id
name
appearsIn
}
}
```


Build your project; this will generate a` HeroQuery` class that you can use with an instance of` ApolloClient` :


```text
// Create a client
val apolloClient = ApolloClient.Builder()
.serverUrl("https://example.com/graphql")
.build()


// Execute your query. This will suspend until the response is received.
val response = apolloClient.query(HeroQuery(id = "1")).execute()


println("Hero.name=${response.data?.hero?.name}")
```


## Join the community


Have a question? Want to discuss GraphQL, Kotlin, or the[ROADMAP](https://github.com/apollographql/apollo-kotlin/blob/main/ROADMAP.md) ? Feel free to reach out:


- on[Github](https://github.com/apollographql/apollo-android/issues)
- on the[community forums](http://community.apollographql.com/new-topic?category=Help&tags=mobile,client)
- or in[the #apollo-kotlin channel](https://app.slack.com/client/T09229ZC6/C01A6KM1SBZ) in the KotlinLang Slack (get your invite[here](https://slack.kotl.in/) ).


Written by


Martin Bonnin


[Read more by Martin Bonnin](https://www.apollographql.com/blog/author/martin)
