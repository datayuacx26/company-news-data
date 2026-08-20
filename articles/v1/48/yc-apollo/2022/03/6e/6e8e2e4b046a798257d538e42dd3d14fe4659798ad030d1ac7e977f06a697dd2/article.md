---
schema_version: "1.0.0"
document_id: "6e8e2e4b046a798257d538e42dd3d14fe4659798ad030d1ac7e977f06a697dd2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-subgraph-support-in-apollo-sandbox"
published_at: "2022-03-02T12:28:22+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:df6fb35b370620a410d37c6db058df36f44476029ae5865ee3785665d2b31d99"
---

# Introducing subgraph support in Apollo Sandbox

When you’re a developer working on a federated graph, you likely work on a single subgraph locally before publishing it to Apollo Studio as part of the supergraph. We created[Apollo Sandbox](https://www.apollographql.com/blog/announcement/platform/apollo-sandbox-an-open-graphql-ide-for-local-development/) to give developers a great tool for working on locally-running graphs, but wouldn’t it be great if could get the same functionality when working with subgraphs? And even better – what if you could use Sandbox to run checks to check that your local changes won’t break composition or operations against the federated graph? Well, now you can!


## Load a subgraph in Apollo Sandbox


When working off a subgraph, head over to[Apollo Sandbox](http://studio.apollographql.com/sandbox/) and connect to your subgraph localhost via the Sandbox endpoint field (or you can connect to a remote endpoint if you like). You will notice that a` SUBGRAPH` badge appears next to the endpoint. Your subgraph schema is loaded and ready to be queried!


Explore your subgraph schema on the Schema page and run operations in Explorer. As you make changes to your subgraph schema, changes are automatically updated in Sandbox without any manual refresh.


## Compare your subgraph schema to a registered subgraph in Studio


Navigate to the Diffs tab and sign into Studio to select a registered subgraph to diff your local schema against. Without going anywhere, you can see how your changes compare to the existing subgraph. You can also see a summary of how many types and fields have been added or removed.


## Check supergraph composition and prevent breaking changes


You can ensure that your changes won’t break the supergraph schema composition and or affect any operations to the federated graph by clicking on the “Run a check” button in the loaded diff view or by heading over to the checks tab. If all of your checks pass, you can publish your changes to the registered schema in Apollo Studio with complete confidence!


If you want to see even more details about checks, you can navigate to the main checks page on Studio. You can always move back and forth between Sandbox and full Studio experience using the box icon on the top right corner in the navigation bar.


**⚠️** **Note:** For checks to work, you’ll need to have a Studio account with access to Checks, which is available on Team and Enterprise plans only.


## Start working on your subgraphs in Sandbox today!


[Head over to Apollo Sandbox](https://studio.apollographql.com/sandbox/) , point it to a subgraph and try out these new features! And as always, we love hearing from you, so if you have any feedback, please fill out this[feedback form](https://forms.gle/4G2cEumrh85jgDF1A) or drop a post on the[Apollo Studio GitHub Community](https://github.com/apollographql/apollo-studio-community/issues) !


Written by


Parul Schroff


[Read more by Parul Schroff](https://www.apollographql.com/blog/author/parul)
