---
schema_version: "1.0.0"
document_id: "2e5f59c396694056a973f74963713363622cadb299071fa2df2c521e3dd8baa4"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/make-your-graphql-schema-publicly-visible-without-introspection"
published_at: "2021-10-14T12:00:02+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:e7b44713f4253e524d4086394f843e2f356bd6b4bd395fb411ac0da504aa65b9"
---

# Make your GraphQL schema publicly visible without introspection

Ever faced the conundrum of how to share your GraphQL schema with users outside of your organization, perhaps with partner or vendor developers? If so, Apollo now has a simple solution for you! We recently launched the ability to make graphs in Apollo Studio publicly visible so that you share your graph with anyone, even those without a Studio login.


## How to share graphs outside of your Apollo organization


Making your graph publicly visible is as easy as flipping a switch. Simply navigate to the Settings page for your graph in Apollo Studio and under the Access tab, set *a variant of your choosing as “public.”* That particular variant of your graph is now publicly visible in Apollo Studio!


Toggle a variant to public in Graph Settings → Access


## What’s visible when a graph is public?


When a variant of a graph is set to public, anyone can navigate to the public Studio URL for that variant and access the README, schema reference, changelog, and Explorer (this is equivalent to[Consumer-level access](https://www.apollographql.com/blog/announcement/platform/sharing-your-graph-just-got-a-whole-lot-easier-announcing-unlimited-consumer-seats/) ). You can preview how the public view will appear to external users by clicking the` Public ↗︎` icon in the top nav.


You can share the public link with any user, even outside of the Studio organization to explore and consume your graph!


**⚠️ Note:** When a specific graph variant is marked as “public”, any unauthenticated user on the internet with the link to the public variant can access its schema. However, accessing endpoint to query data could require authentication depending on the GraphQL endpoint configuration.


## Give all developers a great UX for your graph


You may be wondering, “GraphQL already has introspection, why shouldn’t I just use that?” Well, in addition to the many[security considerations](https://www.apollographql.com/blog/graphql/security/why-you-should-disable-graphql-introspection-in-production/) that make disabling introspection in production a good idea, there are also many UX benefits that you miss out on with introspection alone.


### **A better onboarding experience for new developers**


By making your graph public in Apollo Studio, every developer automatically gets access to the README, schema docs, changelog, and Explorer, all in one central location. No more building custom documentation portals and changelogs that have to be manually updated.


### **Be specific about what you share externally**


A key problem with introspection is that it makes your entire graph visible, including things like experimental fields that you may not want external developers to use yet. With Apollo Studio, you can make an approved, production variant public and keep private, pre-production variants to create and test new fields. In the near future, we’ll be adding more features to make sharing even more customizable!


## Share your graphs today!


Excited? We hope so! Sharing graphs with a broader audience could not get any easier. And don’t forget to update the[README](https://www.apollographql.com/blog/announcement/platform/create-a-readme-to-onboard-developers-to-the-graph/) of your graph before sharing.[Head over to Studio](https://studio.apollographql.com/sandbox/explorer?endpoint=https%3A%2F%2Fapi.spacex.land%2Fgraphql%2F&explorerURLState=N4IgJg9gxgrgtgUwHYBcQC4QEcYIE4CeABADICGMSUAFggM5HAA6SRRANhVbQ862xy40A%2BgQRk8LAUTgBLOnVkQkwpGURSiAXxZaQAGhAA3CbLIAjdvQwgQWoA) and get wider adoption of your graph today! And as always, we love hearing from you, so if you have any feedback, please drop a post on the[Apollo Studio GitHub Community](https://github.com/apollographql/apollo-studio-community/issues) !


Written by


Parul Schroff


[Read more by Parul Schroff](https://www.apollographql.com/blog/author/parul)
