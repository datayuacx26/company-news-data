---
schema_version: "1.0.0"
document_id: "c40390d6d9f04d700f01a99880a7a8e9e6da5a3fca0b0f0ef411b208245b5698"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/improved-embedding-for-apollo-explorer"
published_at: "2022-06-07T10:44:27+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:457fc182768c9ee7ae073781509b446388983db510662e35946303e30513e1a8"
---

# Improved embedding for Apollo Explorer!

**Embed private graphs, use embeds to invite new users, and use improved Apollo Client Dev Tools.**


Explorer is an incredible GraphQL IDE that makes it quick and easy to browse schemas and run operations against them. It’s so awesome, that our community asked us to make it embeddable so that it could be used anywhere – and last year,[we did just that](https://www.apollographql.com/blog/announcement/platform/embed-apollo-explorer-anywhere/) ! But at the time, you could only embed public graph variants. Now, we are happy to announce that you can embed Explorer on **any website** with **any graph variant** – even private variants!


You can provide the experience of Explorer on any of your internal or external documentation, dev portals, and websites to onboard new consumers to your graphs.


## **Embedding the Explorer GraphQL IDE**


To embed Explorer for a specific graph, first open Explorer for that graph in Apollo Studio. Click on the chevron next to the share icon in the editor pane and select “Share as embed”. This will open a modal that gives you a few different options for embedding.


💡Embedded Explorer uses the graph ref from Apollo Studio. Since your graph is registered in Studio, embedded Explorer is always kept up to date with the schema changes in Studio. You can take your schema anywhere by embedding Explorer while Studio’s registry acts as its single source of truth behind the scenes. How neat is that?!


### How does embedded Explorer work for private graph variants?


If the graph variant you want to embed isn’t public (meaning it’s gated behind Studio authentication), then the embedded Explorer will request the visitor to authorize access before proceeding.


If the visitor has never interacted with Apollo Studio before – no problem! They can sign up and be invited from the embed itself.


If your Studio org is using SSO, users are invited with the role that is default selected for your org.


Otherwise, while copying the embed code from the modal in Apollo Studio, admins can configure whether they would like to invite visitors to the organization and select the role to invite with.


### Share operation collections with embedded Explorer


As the person embedding Explorer, graph producers can[share collections of example operations](https://www.apollographql.com/blog/announcement/platform/save-and-share-your-graphql-operations-in-apollo-explorer/) to get their consumers started. Any collection in the “Shared” workspace will be available as default once the embedded Explorer is made available. Visitors can also create their personal collections by signing in.


## **Improved Explorer in Apollo Client Dev Tools**


With this release, we have also improved the Apollo Client Dev Tools by enabling it to securely pull in a graph ref from Apollo Studio if it is unable to introspect the graph from the browser.


## **Start embedding your graphs with Explorer today!**


Sharing graphs with a broader audience using Apollo Explorer’s amazing experience could not get any easier.[Head over to Apollo Studio](https://studio.apollographql.com/sandbox/explorer?endpoint=https%3A%2F%2Fapi.spacex.land%2Fgraphql%2F&explorerURLState=N4IgJg9gxgrgtgUwHYBcQC4QEcYIE4CeABADICGMSUAFggM5HAA6SRRANhVbQ862xy40A%2BgQRk8LAUTgBLOnVkQkwpGURSiAXxZaQAGhAA3CbLIAjdvQwgQWoA) and get started! For detailed instructions on embedding Explorer, check out the[docs](https://www.apollographql.com/docs/studio/explorer/embed-explorer/) . As always, we love hearing from you, so if you have any feedback, please drop a post on the[Apollo Studio GitHub Community](https://github.com/apollographql/apollo-studio-community/issues) !


**⚠️ Note** : Make sure you are on the latest version of the UI. Click on the bell icon at the top right corner of Studio and if you see “Update available, click to relaunch” at the bottom left of the modal then click that to get the latest version.


Written by


Parul Schroff


[Read more by Parul Schroff](https://www.apollographql.com/blog/author/parul)
