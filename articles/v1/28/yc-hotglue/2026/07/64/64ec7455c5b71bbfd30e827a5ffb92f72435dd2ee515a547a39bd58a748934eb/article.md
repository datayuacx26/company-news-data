---
schema_version: "1.0.0"
document_id: "64ec7455c5b71bbfd30e827a5ffb92f72435dd2ee515a547a39bd58a748934eb"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/hotglue-melt-march-2025"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:0c7cb6a6e4aec36a1c324c022eb0e7aa8da19902a379e3f7f750485f544aed5b"
---

# hotglue melt: March 2025

# 🚀 Product Updates


## ⚡10x faster discovers for CRM connectors


We have shipped major performance improvements on our core CRM connectors: Salesforce and HubSpot. Previously, refreshing a field map would take 2 minutes on average – with this update it’s down to **10 seconds** !


Note that the time will vary based on how many objects exist within the CRM instance, and this update is not live if you're using a forked version of a connector.


These improvements will be rolled out to more connectors in March.Let us know if there are specific connectors you want to see with this improvement.


## 🌓 Dark Mode


You asked and we listened! We have introduced a dark mode option in the hotglue admin dashboard – simply use the dark mode toggle in the navbar to switch. Here’s a quick preview:


## ⚙️ React 19+ support for the hotglue widget


We have introduced a new major release (` 1.5.x` ) of the` @hotglue/widget` package which introduces support for React 19+ and newer NextJS versions. This version is still in beta but is already available on[npm](https://www.npmjs.com/package/@hotglue/widget) .


Note that` 1.5.x` is not yet backwards compatible with React 18 or lower, and we will continue support for React 18 with updates on` 1.4.x` . If you run into any issues, don’t hesitate to reach out!


## 🔧 Automatic handling of stuck jobs


We have introduced improved automatic handling and detection of stuck jobs using a **heartbeat** .


With this release, hotglue automatically detects jobs that are hung and retriggers them. This typically comes up when jobs run out of memory or hit unexpected connector errors.


We expect this release will improve stability and result in fewer unexpectedly failed jobs. If you have any questions, let us know!


# 🔌 Connector Updates


## ⚠️ Shopify is migrating to GraphQL and deprecating REST


Shopify is in the process of[deprecating their REST APIs in favor of their newer GraphQL API](https://shopify.dev/changelog/public-apps-must-use-new-graphql-product-apis-to-be-accepted-in-the-shopify-app-store) . As of February 1st they require all Shopify apps to use their GraphQL endpoints for` products` , or they will be delisted. **No action is required** on your side – we added a “compatibility” layer in our _tap-shopify_which replaces our REST API calls for *products* with GraphQL queries and maps the data to the original REST schema. You can review the changes[on our GitHub repository](https://github.com/hotgluexyz/tap-shopify/pull/28) .


Shopify will be continuing to deprecate resources in REST and is encouraging all users to migrate to GraphQL. Additionally, any new fields/objects will only be made available in GraphQL. To help with this transition, we have developed a *tap-shopify-v2* which leverages the GraphQL API instead ([see GitHub](https://github.com/hotgluexyz/tap-shopify-v2) ). If you’re interested in learning more about migrating to the new connector, reach out to us.


## 🏎️ Microsoft SQL target is faster + more stable


We have done a significant rework of our target for Microsoft SQL Server, which has led to significantly better performance and throughput as well as improved stability.


Under the hood we are leveraging[bcp](https://learn.microsoft.com/en-us/sql/tools/bcp-utility?view=sql-server-ver16&tabs=windows) which is SQL Server’s bulk copy utility instead of sending records via SQL. If curious, you can[see the changes in GitHub](https://github.com/hotgluexyz/target-mssql/pull/6) .


That’s all for this month! Thanks for reading :)


Want to chat with our team?[Book a demo](https://hotglue.com/demo) .


TABLE OF CONTENTS


- 🚀 Product Updates
- ⚡10x faster discovers for CRM connectors
- 🌓 Dark Mode
- ⚙️ React 19+ support for the hotglue widget
- 🔧 Automatic handling of stuck jobs
- 🔌 Connector Updates
- ⚠️ Shopify is migrating to GraphQL and deprecating REST
- 🏎️ Microsoft SQL target is faster + more stable


RECOMMENDED BLOGS


[Custom Mapping for CRM Integrations: Why It’s Better and How to Do It CRMs revolve around custom data. With custom mapping you can make your CRM integration more useful to users while decreasing onboarding time.](https://hotglue.com/blog/custom-mapping-for-crm-integrations-why-its-better-and-how-to-do-it)


[Introducing Magic Link Imagine if you could just send a link to your users to connect all their integrations… now you can using our new Magic Link feature 🪄](https://hotglue.com/blog/introducing-magic-link)


[Announcing hotglue’s $4M Seed Round Announcing our $4 million seed round led by 8VC with participation from Y Combinator, Correlation Ventures, and others.](https://hotglue.com/blog/announcing-hotglues-4m-seed-round)
