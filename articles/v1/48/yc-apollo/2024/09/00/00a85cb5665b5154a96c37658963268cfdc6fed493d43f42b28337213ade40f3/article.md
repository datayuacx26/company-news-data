---
schema_version: "1.0.0"
document_id: "00a85cb5665b5154a96c37658963268cfdc6fed493d43f42b28337213ade40f3"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/more-expressive-schemas-with-oneof"
published_at: "2024-09-25T07:10:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:0ffd31d3cea6eedacf7d7a06078577cb6bb5dd30c40bc6c5f610dc2f261ed037"
---

# More expressive schemas with @oneOf

GraphQL, the *lingua franca* of APIs, offers a palette of expressive tools: interfaces, nullability, deprecation, unions, etc.


Occasionally though, a backend’s contract would be reflected more precisely, if only it could be represented in the schema. Take unions for example: they can be used for output types, but not input types, so a search API accepting different kinds of criteria can be implemented like this:


```text
type Query {
searchByName(name: String!): User
searchById(id: ID!): User
searchBy...
}
```


This works, but is a bit tedious, and what we *really* mean is something like` search(name: String! | id: ID! | …)` . What can we do to improve the situation? GraphQL is an open standard, with a working group and[processes](https://github.com/graphql/graphql-spec/blob/main/CONTRIBUTING.md) in place to support its evolution: the RFCs (requests for comments).


## The OneOf RFC


Seeing this possible room for improvement, the GraphQL community came up with[this RFC](https://github.com/graphql/graphql-spec/pull/825) , which proposes a new built-in directive:` @oneOf` . It can be applied to input types to indicate that one and only **one of** the fields must be provided.


In our example above, we can take advantage of it like this:


```text
type Query {
search(criteria: SearchCriteria!): User
}


input SearchCriteria @oneOf {
name: String
id: ID
#...
}


```


With that, a client can query` search({ name: “John” })` or` search({ id: “42” })` but` search({ name: “John”, id: “42” })` would result in a validation error.


**Backwards compatibility**


A nice property of the proposal is that it doesn’t introduce any new syntax and therefore should be transparent for any client libraries that have no knowledge of the new directive.


If you provide more than one field to a OneOf input type, a “OneOf aware” library will warn you early, before executing the operation. An older library will treat the input type as a regular one, and will allow its execution, but the **backend** will then provide an error, as expected. To achieve this compatibility, all the fields of a OneOf input type must be declared as if they were **nullable** (without an exclamation mark).


```text
input SearchCriteria @oneOf {
name: String
id: ID! # Illegal!
type: Type! # Illegal!
}
```


Without that requirement, pre-OneOf client libraries would require to provide **all** non-nullable fields – which wouldn’t make sense since only **one** field must be provided.


Note: although the fields are declared in the schema as nullable, passing the value null is not allowed. This may seem counterintuitive at first but the reason lies within GraphQL’s type system –[this RFC comment](https://github.com/graphql/graphql-spec/pull/825#issuecomment-1835978233) explains it all.


## Support in Apollo Kotlin


If you are an Apollo Kotlin user and want to try @oneOf today, you’re in luck! We’ve added experimental support for it in[version 4](https://www.apollographql.com/blog/apollo-kotlin-4-is-now-available) :


- [Fetching a schema](https://www.apollographql.com/docs/kotlin/advanced/plugin-recipes#downloading-a-schema) via introspection on a OneOf-aware server will include the` @oneOf` directives.


- You’ll get a runtime error if you build a oneOf input type and provide more than one field:


```text
SearchCriteria.Builder()
.name("Luke")
.id("42")
.build() // Throws! Must provide only one field.
```


- When using[the IDE plugin](https://www.apollographql.com/docs/kotlin/testing/android-studio-plugin/) you’ll get errors in the editor:


## Support in Apollo Server


With[Apollo Server](https://github.com/apollographql/apollo-server) , you can use` @oneOf` in your schemas starting from version 4.


## What’s next?


The GraphQL RFC process is an iterative one. The OneOf RFC is currently in Stage 2 (“Draft”), which[means](https://github.com/graphql/graphql-spec/blob/main/CONTRIBUTING.md#stage-2-draft) it is thought to be ready to be accepted into the specification but awaiting feedback from the community. Try it today with Apollo Kotlin and Apollo Server, and make your voice heard on[the ticket](https://github.com/graphql/graphql-spec/pull/825) and in the Apollo[issue](https://github.com/apollographql/apollo-kotlin/issues)[trackers](https://github.com/apollographql/apollo-server/issues) .


Written by


Benoit Lubek


[Read more by Benoit Lubek](https://www.apollographql.com/blog/author/blubek)
