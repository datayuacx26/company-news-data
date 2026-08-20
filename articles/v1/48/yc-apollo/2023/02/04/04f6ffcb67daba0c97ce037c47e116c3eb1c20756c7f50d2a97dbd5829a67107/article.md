---
schema_version: "1.0.0"
document_id: "04f6ffcb67daba0c97ce037c47e116c3eb1c20756c7f50d2a97dbd5829a67107"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/nullability-in-graphql-and-apollo-kotlin"
published_at: "2023-02-27T08:10:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:88d03033dec08167ee039ac19dc94d57badfbaab3c3398a55f69470b4f0717cd"
---

# Nullability in GraphQL and Apollo Kotlin

The concept of` null` is sometimes called a “[billion dollar mistake](https://en.wikipedia.org/wiki/Null_pointer#History) ”. Mistake or not,` null` is here to stay and there are ways to coexist peacefully!


In fact, nullability is one of the[key features of GraphQL](https://graphql.org/learn/schema/#object-types-and-fields) , allowing API authors to clearly specify which fields of a type are nullable.


As for Kotlin, the language[supports nullability in its type system](https://kotlinlang.org/docs/null-safety.html) , significantly reducing the risk for errors – for example assigning a potentially` null` value to a non-nullable field won’t even compile!


At the intersection of these two worlds lies[Apollo Kotlin](https://github.com/apollographql/apollo-kotlin) , the GraphQL typesafe client library for Kotlin. Let’s look at a few ways the library deals with nullability.


## Null safety on fields


Apollo Kotlin generates models based on your project’s GraphQL operations and the type information found in the schema. When selecting a field whose type is nullable in the schema, the corresponding field in the generated Kotlin model will also be nullable.


💡 By the way, notice how the GraphQL and Kotlin syntaxes are quite similar, but an important difference is how the default in GraphQL (no` !` ) means nullable, whereas the default in Kotlin (no` ?` ) means **non-nullable** .


Schema:


```text
type Query {
person(id: ID!): Person  # ← nullable
}


type Person {
id: ID!  # ← non-nullable
email: String  # ← nullable
}
```


Query:


```text
query GetPersonById(id: ID!) {
person(id: $id) {
id
email
}
}
```


Generated models:


```text
data class Data(
val person: Person?,  // ← nullable
)


data class Person(
val id: String,  // ← non-nullable
val email: String?,  // ← nullable
)
```


There are cases where non-nullable fields will still be generated as nullable, can you guess which?


That’s right (you guessed!): when using the` @include` or` @skip` directives which control whether a field (or fragment) will be returned by the server. In that case the field will be` null` when not present.


**What about fragments?**


With the` operationBased` codegen ([the default](https://www.apollographql.com/docs/kotlin/advanced/response-based-codegen/) ), one field is generated for each fragment spread and inline fragment. When polymorphism is involved, the fragment’s fields may or may not be returned, depending on the object’s type. That is why, here too, the fragment field will be nullable – if necessary.


Schema:


```text
interface SearchResult {
rank: Int!
}


type ProjectSearchResult implements SearchResult  {
project: Project!
rank: Int!
}


type UserSearchResult implements SearchResult  {
user: User!
rank: Int!
}
```


Query:


```text
query Search(q: String!) {
search {
... on SearchResult { rank }
... on ProjectSearchResult { project { name } }
... on UserSearchResult { user { email } }
}
}
```


Generated models:


```text
data class Search(
val onSearchResult: OnSearchResult,
val onProjectSearchResult: OnProjectSearchResult?,  // ← nullable
val onUserSearchResult: OnProjectSearchResult?,  // ← nullable
)
```


👀 Notice` onSearchResult` is generated non-nullable. The code generator figures out that the type condition “on SearchResult” is always true for` search` , so these fields will always be present in the response – hence never` null` .


## Why so many nulls?


As a Kotlin developer, you may wonder why so many schemas make most of their fields nullable. And why is nullable the default in the schema SDL language anyway?


There can be multiple reasons but an important one has to do with error handling. In GraphQL, when an error happens, a response can be partial: fields that **could** be resolved will be returned. Fields that **couldn’t** will be` null` , **if the field’s type is nullable** . If not, then the field’s whole parent will be` null` , if possible, and so on, until we reach the root field,` data` , which is nullable. This is known as errors[“bubbling up”](https://spec.graphql.org/draft/#sel-FAPHRPDTAACVAn-e) .


There it is: if you want partial results,` null` is unavoidable. …However there are times when partial results won’t be sent, or certain field being nullable doesn’t make much sense. In those cases, you can enforce non-nullability on the client.


## Enforcing non-nullability


Dealing with nullable fields can be tedious:


```text
firstName = data.results?.get(0)?.user?.firstName ?: ""
// or
firstName = data.results!![0]!!.user!!.firstName!!


```


` results` ,` user` , and` firstName` may be nullable according to the schema, but if you happen to know it can’t happen here, it would be nice to tell the machine *“Actually, this field will never be null. Trust me, I know what I’m doing!”* 😅.


That’s what Apollo Kotlin’s[@nonnull directive](https://www.apollographql.com/docs/kotlin/advanced/nonnull) does: with it, you instruct the codegen to *act as if* the fields were non-nullable:


Query:


```text
query FirstName {
results @nonnull {
user @nonnull {
firstName @nonnull
}
}
}
```


🔍 Notice that` results` is non-nullable but the type parameter` Result?` still is. At the moment non-nullability can’t be enforced on the *elements* of a list.


Generated models:


```text
data class Data(
val results: List<Result?>  // ← non-nullable
)


data class Result(
val user: User  // ← non-nullable
)


data class User(
val firstName: String  // ← non-nullable
)
```


You can also mark fields as` @nonnull` at the **schema level** , to avoid repeating the directive in all your queries:


extra.graphqls:


```text
extend type Query @nonnull(fields: "results")
extend type Results @nonnull(fields: "user")
extend type User @nonnull(fields: "firstName")
```


💡 If you like this feature, you’re not alone – that is why a similar feature should soon be part of the official GraphQL spec.[“Client Controlled Nullability”](https://github.com/graphql/graphql-wg/blob/main/rfcs/ClientControlledNullability.md) , currently an RFC, will allow you to mark fields in queries as non-nullable, with a new` !` based syntax. Non-nullable list elements will also be supported!


## Input values: null vs absent


In GraphQL, when an **argument** or an **input object field** is of a nullable type, it actually[means 2 things](https://spec.graphql.org/draft/#sel-GAFdRHABABsC0pB) :


- it can be passed as` null` (present, with a value of` null` )
- it can be omitted altogether (absent)


And these two things can have a different meaning for your backend. For instance:


Schema:


```text
type Mutation {
updateUser(id: ID!, user: UpdateUserInput!): Boolean!
}


input UpdateUserInput {
firstName: String
nickname: String
}
```


Mutations:


```text
mutation UpdateFirstNameAndResetNickname {
updateUser(id: 1, user: {firstName: "Robin", nickname: null})
}


mutation UpdateFirstName {
updateUser(id: 1, user: {firstName: "Robin"})
}
```


In the first mutation, we explicitly set nickname to` null` ; in the second one, we don’t pass a value for nickname: the backend won’t touch it.


Apollo Kotlin reflects this in the generated input models by using the` Optional` class:


```text
data class UpdateUserInput {
val firstName: Optional<String?> = Optional.Absent,
val lastName: Optional<String?> = Optional.Absent,
val nickame: Optional<String?> = Optional.Absent,
}
```


Nullable fields will be **absent** by default, but you can explicitly set them to` null` with` Optional.present(null)` .


## Operation variables


Just like input values, operation variables can be nullable, and when they are, they can be passed as` null` or be omitted. Here too, they will therefore be generated with` Optional` .


However this is often not desirable: after all, if you wrote your operation to accept an argument, it is (usually!) because you want to pass a value for it, even if that value is` null` . And wouldn’t it be nice to be able to write` MyQuery(null)` rather than` MyQuery(Optional.present(null))` ?


We’ve got you covered! Use Apollo Kotlin’s` @optional` directive:


Mutation:


```text
mutation SetNickname($id: ID!, $nickname: String @optional(if: false)) {
updateUser(id: $id, user: {nickname: $nickname})
}
```


Generated code:


```text
data class SetNicknameMutation(
val id: String,
val nickname: String?,  // ← no Optional
) {
// ...
}
```


This can also be configured[once globally](https://www.apollographql.com/docs/kotlin/advanced/operation-variables/#making-nullable-variables-non-optional) if preferred.


## What about Java?


In addition to Kotlin, Apollo Kotlin can also generate Java code. Java doesn’t have a strong support for nullability, but there are tools that can help:


- annotating the fields with` @Nullable` or` @NotNull`
- wrapping nullable types in` Optional`


Apollo Kotlin[can do both](https://apollographql.github.io/apollo-kotlin/kdoc/apollo-gradle-plugin-external/com.apollographql.apollo3.gradle.api/-service/nullable-field-style.html) , with several flavors to choose from for each.


💡 With input values and operation arguments, you may encounter things like` Optional<Optional<String>>` . This can be surprising at first, but it makes sense when you think about it: the 1st Optional represents the presence or not of the value, the 2nd one represents nullability. It’s the Java equivalent of` Optional<String?>` .


## In a nullshell


GraphQL and Kotlin are a good fit: they are both strongly typed and have nullability built-in. Their syntax is quite similar even if the defaults are different and if the details of handling null vs absent can get complicated for input values.


I hope this post cleared some of the confusion around this and gave you more tools to deal with nullability in your codebase and avoid those costly` NullPointerException` !


As always, feedback is welcome. don’t hesitate to reach out on[GitHub](https://github.com/apollographql/apollo-kotlin) , the[Apollo Community](https://discord.gg/graphos) , or the[Kotlin Slack](https://slack.kotl.in/) !


Written by


Benoit Lubek


[Read more by Benoit Lubek](https://www.apollographql.com/blog/author/blubek)
