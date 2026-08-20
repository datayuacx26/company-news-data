---
schema_version: "1.0.0"
document_id: "23e0a570fad8d932c360ac7e11cb33e45767c2c77500f55c9f85e6b20f75a841"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-the-new-graphql-ci-in-apollo-studio"
published_at: "2020-10-01T16:44:29+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:74018dfa63fbcaa482f5b000d0aa49ab93c6f2dc4ddcda02693c6084985118f1"
---

# Schema Checks: Introducing the New GraphQL CI Tool In Apollo Studio

A Better Way to do CI For GraphQL


When deploying code, you want to be sure that nothing will blow up when things go live. To prevent downtime, it’s a good practice to use CI (continuous integration) to run tests, checks, and ensure your app is fully ready to hit production. If you’re running GraphQL in production, the *last* thing you want is a **schema change** to break things for your users. Here at Apollo, we’ve taken stock of every way schema changes could cause downtime, and **we’re delighted to announce a new and improved schema check to ensure your GraphQL layer is evolving smoothly 🎉**


## Schema Checks


Using the[Apollo CLI](https://www.apollographql.com/docs/devtools/cli/) , you can ensure changes made to your schema are backwards compatible by checking it against *real production traffic* .


We’ve redesigned our schema checks experience to prioritize giving you the most important information. Schema checks help you understand how a change will affect your *real-life clients.* This release comes with a few long-awaited additions:


- **Better visibility** : Understand not only *which* clients are affected, but also *how* . Schema checks help you determine if an operation won’t run with the new schema.
- **Change-level and operation-level overrides** : For dealing with more nuanced cases.
- **Better UX** : A visual update that shows usage of your affected operation and provides valuable forensics.


## Centralized config


We’ve also made it possible for teams to configure key settings for schema checks in a central place (see the *Check Configurations* page in[Apollo Studio](https://www.apollographql.com/docs/studio/getting-started/) ). Using this central configuration, teams can align on best practices (such as a standard deprecation lifecycle) and share information about deprecated clients and operations.


## Get started


Want to safely evolve your graph in production? Read the docs on[schema checks](https://www.apollographql.com/docs/studio/schema-checks) to learn more and see how to try it out!


Written by


Caydie Tran


[Read more by Caydie Tran](https://www.apollographql.com/blog/author/caydie)
