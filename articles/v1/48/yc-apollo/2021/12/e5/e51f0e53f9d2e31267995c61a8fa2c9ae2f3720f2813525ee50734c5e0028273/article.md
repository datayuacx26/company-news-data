---
schema_version: "1.0.0"
document_id: "e51f0e53f9d2e31267995c61a8fa2c9ae2f3720f2813525ee50734c5e0028273"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/embed-apollo-explorer-anywhere"
published_at: "2021-12-09T16:42:28+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:9f43261a147e57d81c46c4aff35daa63a952535f5088f32d5d63c46a2bfa86c2"
---

# Embed Apollo Explorer Anywhere

The Apollo Studio Explorer is an incredible GraphQL IDE that makes it quick and easy to browse schemas and run operations against them. But wouldn’t it be awesome if you could embed the Explorer into external pages so that you could use it inside your product documentation, internal wiki, or blog post? Now you can! Let’s take a look at how.


## Make the graph you want to embed publicly visible


Making your graph publicly visible is as easy as flipping a switch. Simply navigate to the Settings page for your graph in Apollo Studio and under the Access tab, set a variant of your choosing as “public.” That particular variant of your graph is now publicly visible in Apollo Studio! When a variant of a graph is set to public, anyone can navigate to the public Studio URL for that variant and access the README, schema reference, changelog, and Explorer (this is equivalent to[Consumer-level access](https://www.apollographql.com/blog/announcement/platform/sharing-your-graph-just-got-a-whole-lot-easier-announcing-unlimited-consumer-seats/) ).


## Copy the embed code snippet


Now that your graph is public, head over to Explorer for that graph. If you would like to embed Explorer with a default operation, enter the operation in the editor pane in Explorer before copying the embed snippet (also, maybe test it out first to make sure it works 😄). Then click on the meatball menu right of your operation and click on “Embed this operation.” You can also find the action to embed under the Explorer Settings tab in Explorer.


In the embed modal, you can also toggle any configuration options that you would like such as embedded Explorer with dark theme and keeping the docs panel closed by default. Once you have configured it to your liking, copy the code snippet to embed!


## Paste the embed code snippet in the parent page


Now all you have to do is take the embed code snippet and paste it in the code of any webpage where you would like to embed Explorer like we have done here! Try the embedded Explorer for yourself below.


## Embed your graphs with Explorer today!


Excited? We hope so! Sharing graphs with a broader audience on your own documentation portals using Apollo Explorer’s amazing experience could not get any easier.[Head over to Studio](https://studio.apollographql.com/sandbox/explorer?endpoint=https%3A%2F%2Fapi.spacex.land%2Fgraphql%2F&explorerURLState=N4IgJg9gxgrgtgUwHYBcQC4QEcYIE4CeABADICGMSUAFggM5HAA6SRRANhVbQ862xy40A%2BgQRk8LAUTgBLOnVkQkwpGURSiAXxZaQAGhAA3CbLIAjdvQwgQWoA) and get wider adoption of your graph today! And as always, we love hearing from you, so if you have any feedback, please drop a post on the[Apollo Studio GitHub Community](https://github.com/apollographql/apollo-studio-community/issues) !


Written by


Parul Schroff


[Read more by Parul Schroff](https://www.apollographql.com/blog/author/parul)
