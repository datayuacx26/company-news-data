---
schema_version: "1.0.0"
document_id: "0bf4f7818f0b45a9ff6c43585bf8e20c6444a77dceddb6ffb30ce9ef628b03d7"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-run-diffs-and-operation-checks-in-apollo-sandbox"
published_at: "2021-12-14T15:55:07+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:2aab7119f0e9b2a10d001acfd1ea864b5e5fb87a81865252d3d722680c072d55"
---

# How to run diffs and operation checks in Apollo Sandbox

When you’re making changes to your GraphQL schema, you probably make those changes locally before you publish them to a graph registry like Apollo Studio.


We created[Apollo Sandbox](https://www.apollographql.com/blog/announcement/platform/apollo-sandbox-an-open-graphql-ide-for-local-development/) so that you can use the best GraphQL IDE when working locally, but wouldn’t it be great if you could easily see the diff between your local schema and the existing schema in Studio?


And even better – what if you could run checks to see if your local changes could break operations against the production graph? Well, now you can do both right from Apollo Sandbox! Let’s take a look at how:


## Validate your schema changes before publishing them to Studio!


Want to feel confident about your schema changes before publishing them to Studio? Simply head over to[Apollo Sandbox](http://studio.apollographql.com/sandbox/) and connect to your localhost via the Sandbox endpoint field (or you can connect to a remote endpoint if you like).


### Compare your schema to a registered graph in Studio


Go to the Diffs tab and sign into Studio to select a registered graph to diff your local schema against and explore your diff! See a summary of how many types and fields have been added or removed and select to view it as a line wrap if needed.


### Make sure there are no breaking changes


Try running an operation check on your local schema by either:


1. Using the Run check button from the loaded diff view or
2. Going to the Checks tab and selecting a registered graph to check against


Once you see a summary of results, you can also view more details by navigating to the main checks page on Studio.


Remember, you can always move back and forth between Sandbox and the full Studio experience by clicking the box icon in the top righthand corner in the navigation bar.


⚠️ **Note:** For checks to work, you’ll need to have a Studio account with a registered graph that is receiving traffic and access to Checks, which is available on Team and Enterprise plans only.


## Try these new improvements out today!


[Head over to Apollo Sandbox](https://studio.apollographql.com/sandbox/) and compare your local changes with registered graphs and ensure there are no breaking changes! And as always, we love hearing from you, so if you have any feedback, please fill out this[feedback form](https://forms.gle/4G2cEumrh85jgDF1A) or drop a post on the[Apollo Studio GitHub Community](https://github.com/apollographql/apollo-studio-community/issues) !


Written by


Parul Schroff


[Read more by Parul Schroff](https://www.apollographql.com/blog/author/parul)
