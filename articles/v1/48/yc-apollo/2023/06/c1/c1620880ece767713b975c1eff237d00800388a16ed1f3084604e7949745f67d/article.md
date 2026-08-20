---
schema_version: "1.0.0"
document_id: "c1620880ece767713b975c1eff237d00800388a16ed1f3084604e7949745f67d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/announcing-the-apollo-kotlin-plugin-for-android-studio-and-intellij-idea"
published_at: "2023-06-20T12:22:59+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:8c4d814a55eb255c561af60588be30c958740243cad044a08df5223c7052409c"
---

# Announcing the Apollo Kotlin plugin for Android Studio and IntelliJ IDEA

We’re happy to introduce an[Apollo Kotlin](https://github.com/apollographql/apollo-kotlin) plugin for Android Studio and IntelliJ IDEA, to help you work with the library and improve your productivity.


This preview release can be tried out today.


## Features


### On-the-fly code generation


Apollo Kotlin is a strongly-typed client library that generates models from your GraphQL queries. This works thanks to a Gradle plugin that triggers the code generation at build time. Normally, after touching a graphql file, to be able to use the up-to-date generated models in your Kotlin code, you would need to manually run the codegen Gradle task.


With the IDE plugin, this is done automatically: the codegen is run in the background whenever you touch your GraphQL queries, so you can use the models immediately.


On-the-fly code generation


### Migration helpers


Apollo Kotlin 3 was a major version that introduced quite a few changes compared to version 2. While some of[these changes](https://www.apollographql.com/docs/kotlin/migration/3.0/) require manual tweaks to the code, a lot of these can be automated. Which is what the “Migrate to Apollo Kotlin 3” action does. If you are still on version 2, now is a good time to migrate!


Migrate to Apollo Kotlin 3


If you are using Apollo Kotlin 3 with[the compat codegen](https://www.apollographql.com/docs/kotlin/migration/3.0/#codegen) , which was meant to ease the migration from version 2 (and will be discontinued in the upcoming version 4 release), the plugin can also help with this: “Migrate to` operationBased` Codegen”.


Migrate to operationBased codegen


### Navigation


When editing Kotlin code that references GraphQL queries, it is often useful to go to their definition – for instance, if you need to add a field to the selection. The natural way to do that in Android Studio is to` Cmd/Ctrl + click` (or` Cmd/Ctrl + b` on the keyboard) on the symbol. However without any special knowledge of Apollo Kotlin or GraphQL, this would go to the generated code, which is not as useful. Not anymore! The plugin adds target to go to the GraphQL definition:


Go to GraphQL definition


Note that the generated code destination is still there, as it is sometimes handy to have a look at it.


We’ve also added these nifty GraphQL icons in the gutter next to operation and fragment references. Click on them to go to the definition.


To go to the Schema instead, use “Go To Type Declaration” (` Cmd/Ctrl + Shift + click` , or` Cmd/Ctrl + Shift + b` ):


Go to the schema


The reverse works too if you need it: “Find Usages” on your GraphQL operation will list Kotlin files referencing them:


Find code usages


We hope to add “Find Usages” on fields as well in the future.


### More to come


We are working on adding more features and your ideas are very welcome! Please don’t hesitate to submit feature requests on[the issue tracker](https://github.com/apollographql/apollo-kotlin/issues/new/choose) .


## Try it out!


To install the plugin:


1. During the preview phase, you will need to add a dedicated plugin repository: –` Settings` >` Plugins` >` ⚙️` >` Manage Plugin Repositories` >` ➕` >` https://plugins.jetbrains.com/plugins/preview/20645` – If you want the cutting edge, use this “snapshots” URL instead (new versions once a week):` https://raw.githubusercontent.com/apollographql/apollo-kotlin/main/intellij-plugin/snapshots/plugins.xml`
2. ` Marketplace` > Search for “Apollo GraphQL” >` Install`


Detailed instructions are available[here](https://github.com/apollographql/apollo-kotlin/tree/main/intellij-plugin) .


## Conclusion


We hope this plugin will make your experience with Apollo Kotlin a delightful one and will help you build great apps. Stay tuned for more, and as always, your feedback is welcome! You can reach out on[GitHub](https://github.com/apollographql/apollo-kotlin) , the[Apollo Community](https://discord.gg/graphos) , or the[Kotlin Slack](https://slack.kotl.in/) !


Written by


Benoit Lubek


[Read more by Benoit Lubek](https://www.apollographql.com/blog/author/blubek)
