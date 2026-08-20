---
schema_version: "1.0.0"
document_id: "d93139190915c0e063c7a7a1f7ec3b76a63cf0db2c554f0e5b8131f7965c1739"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/build-checks-webhooks-apollo-studio"
published_at: "2021-05-27T17:17:07+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:c298a77a9625c2d54aeaa76eb8a4948fe29f9a978ace36ee3774506309b396b4"
---

# Introducing Build Checks and Webhooks In Apollo Studio

Federation Support in Apollo Studio Just Got a Whole Lot Better


Preventing breaking changes when working on a shared data graph is critical. Last year, we launched[schema checks in Apollo Studio](https://www.apollographql.com/blog/introducing-the-new-graphql-ci-in-apollo-studio/) to help you collaborate on your graph with confidence. Checks validate your ** schema changes against historical operations to ensure your changes *will not* break your clients. However, for[federated](https://www.apollographql.com/docs/federation/) graphs, in addition to checking that your operations won’t break, we also need to check to see whether the[supergraph](https://www.apollographql.com/docs/rover/supergraphs) schema is successfully built, meaning that it successfully composed the supergraph schema from the underlying subgraph schemas. Before today, our platform did build checks but didn’t expose the results in Studio, which required going into the CLI to see why a supergraph build failed.


Here at Apollo, we understand how important it is to have visibility of your federated graph’s composition, and **we’re delighted to announce[build checks](https://www.apollographql.com/docs/studio/schema-checks/#federated-composition-checks) and[webhooks](https://www.apollographql.com/docs/studio/build-status-notification/) to help you better understand the impact of schema changes, update your data graph faster, and allow more teams to contribute to the graph safely. 🎉**


## Build checks for federated graphs


With the addition of build checks, you will now have better visibility into why a build failed for your subgraph’s schema changes. With this release comes a few additions and improvements:


- ****Better visibility** :** If the build fails, the build page will display all errors that occurred when trying to compose your changes. You’ll also be able to download or copy the subgraph schema and the resulting supergraph schema when available. Use the downloaded schema for codegen, linting, or mocking!
- ****Better UX** :** The recent checks page now introduces filters. Use these filters to quickly find checks based on the check status, author, subgraph, or branch, helping you find what you’re looking for and surface root cause faster.


**Example of failed build check**


Example of a **successful** build check


New filtering capability with checks


## Build Webhooks


We’ve also made it possible to configure build webhooks for your graph so that you can be notified when a new schema is built and ready to ship to your running gateway. You can also use this to alert on broken builds so that you can more quickly resolve errors when they happen.


Set up a webhook to get notified when supergraph schema is built


Webhooks are part of the Studio Enterprise plan. Please[reach out to us](https://www.apollographql.com/contact-sales/?referrer=blog) if you are interested in this feature.


## Get started


Want to try out the new check experience today? See the[docs](https://www.apollographql.com/docs/studio/schema-checks/#federated-composition-checks) to learn more and see how to try it out!


Written by


Caydie Tran


[Read more by Caydie Tran](https://www.apollographql.com/blog/author/caydie)
