---
schema_version: "1.0.0"
document_id: "4ad8665f4d0e3a55df5db21053ee096ea980ddce69b962abe1075df9a3a0047a"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/sdl-support-in-apollo-android"
published_at: "2020-11-18T15:12:04+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:37.862197+00:00"
content_hash: "sha256:8afa8b3e2131c6b3127e69148086812bb34fa38acb30b77792d43bd767242b0e"
---

# SDL Support in Apollo Android

If you’re using Apollo Android, the chances are that you are using a` schema.json` file: an[introspection](https://graphql.org/learn/introspection/) schema. While introspection schemas work well, GraphQL has[SDL](https://graphql.org/learn/schema/) (Schema Definition Language), which is another, more concise, way to describe a schema.


The excellent news is that Apollo Android supports SDL!


Read below to learn more about SDL and how to enable it in your project.


## Why SDL?


Since Apollo Android already supports introspection schemas, why bother with a new schema format? Well, there are a ton of reasons for that:


### 1. SDL is more concise


Taking a look at the[commit](https://github.com/apollographql/apollo-android/pull/2543) that moved the Apollo Android test schema from introspection to SDL:


The SDL file has almost ten times fewer lines. It’s also significantly smaller ([9KB](https://github.com/apollographql/apollo-android/blob/5d8338338cff6217a73f5c62db2443bf1d760577/apollo-compiler/src/test/graphql/schema.sdl) vs.[106KB](https://github.com/apollographql/apollo-android/blob/02d77acd9d84dc7fb4cf8d78e2b602beec7cf185/apollo-compiler/src/test/graphql/schema.json) ), making your repository faster to download.


### 2. SDL is easier to read (and write!)


SDL being smaller also means it’s easier to read. Let’s take an example from our[favorite schema](https://github.com/apollographql/starwars-server) (simplified here):


```text
# SDL
"""A character from the Star Wars universe"""
interface Character {
"""The ID of the character"""
id: ID!


"""The name of the character"""
name: String!


"""The date character was born."""
birthDate: Date!
}
```


The same thing in JSON (with only the “id” field or it would take too much space!):


```text
// Introspection
{
"kind": "INTERFACE",
"name": "Character",
"description": "A character from the Star Wars universe",
"fields": [
{
"name": "id",
"description": "The ID of the character",
"args": [],
"type": {
"kind": "NON_NULL",
"name": null,
"ofType": {
"kind": "SCALAR",
"name": "ID",
"ofType": null        }
},
"isDeprecated": false,
"deprecationReason": null    },
{
[REDACTED],
}
]
}
```


Adding a new field to an SDL type is a few lines, and you won’t have to worry about creating a valid JSON document or nesting the` LIST` and` NON_NULL` types; this makes the experience of writing unit tests for Apollo Android way more enjoyable.


### 3. Introspection isn’t always an option


While introspection is a useful tool during development, it exposes your schema’s structure to anyone to access your GraphQL endpoint. Unfortunately, exposing your schema can have security repercussions, and a lot of server implementations like apollo-server[will disable introspection in production](https://www.apollographql.com/docs/apollo-server/testing/graphql-playground/) , making it harder to obtain an introspection schema.


### 4. SDL carries more information


Last but not least, SDL carries more information than plain introspection JSON. Custom directives definition, for example, are stripped out in introspection responses. While Apollo Android does not use this, having this information gives client developers more context about the schema they are using. There are other useful ways we could utilize it in the future too!


## Using SDL in Apollo Android


Using SDL in Apollo Android is **as simple as replacing your` schema.json` file with a` schema.sdl` file** . You might sometimes see this file named schema.graphql. We decided to go with the explicit` schema.sdl` name to distinguish from operation files, which also have a .graphql extension.


We also built a few tools in the Gradle plugin to make it easier to work with SDL files:


### Transforming an introspection schema to a SDL schema


The Gradle plugin in Apollo Android can automatically translate your` schema.json` to` schema.sdl` :


` ./gradlew convertApolloSchema --from schema.json --to schema.sdl`


Note that it works both ways if for some reason you want to get a JSON file:


` ./gradlew convertApolloSchema --from schema.sdl --to schema.json`


### Downloading a SDL schema from introspection


Since the plugin can translate between both formats, it can save the results of an introspection query as SDL.


` ./gradlew downloadApolloSchema --endpoint https://example.com/graphql --schema app/src/main/graphql/com/example/schema.sdl`


### Downloading an SDL schema from Studio


Finally, if you have a Studio integration, you can also download the SDL directly from[Apollo Studio](https://www.apollographql.com/docs/studio/) . Get your[API key](https://www.apollographql.com/docs/studio/api-keys/) and type:


` ./gradlew downloadApolloSchema --key $API_KEY --graph $GRAPH --variant current`` --schema app/src/main/graphql/com/example/schema.sdl`


## Conclusion


The initial SDL support was[committed](https://github.com/apollographql/apollo-android/commit/8fccd52b01b7f8e9a7d9dffca0438cb8c97da141) a couple of months ago and we’re still working on improving it.` convertApolloSchema` , for example, is a very recent addition.


It’s still the early days of SDL support for Apollo Android but we’ve been using it successfully to write a lot of unit tests and it really made our life better. We hope it can make yours better too!


Try it out and[let us know](https://app.slack.com/client/T09229ZC6/C01A6KM1SBZ) how you liked it!


Written by


Martin Bonnin


[Read more by Martin Bonnin](https://www.apollographql.com/blog/author/martin)
