---
schema_version: "1.0.0"
document_id: "254891715474ee6b196854f25fe2e2b3731a2eb0e35fbf13d033abac2057e0fa"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/hotglue-melt-june-2026"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:f2176de361bead73d03d014862269c20c7f6a72622c806a8a3dc960d0929810f"
---

# Hotglue Melt: June 2026

# 🚀 Product Updates


## 🔄 Access tokens on demand


We've added a new API endpoint that returns a valid access token for connectors that support OAuth refresh flows.


` GET /{env}/{flow}/{tenant}/connectors/{connector}/accesstoken


`


Previously, developers could retrieve refresh tokens from Hotglue but were responsible for implementing each provider's token refresh flow themselves.


Now, Hotglue handles the entire refresh lifecycle behind the scenes: invoking the connector-specific refresh logic, updating the stored credentials, and returning a fresh access token in a single API call.


Available today for HubSpot, QuickBooks, Dynamics 365 Business Central, and more.


Check out the full API reference:[https://docs.hotglue.com/api-reference/access-tokens/retrieve-access-token](https://docs.hotglue.com/api-reference/access-tokens/retrieve-access-token)


## 🛠️ Reproduce ETL issues locally


Debugging a failed ETL job just got a lot easier. We've added a new` hotglue etl setup-local-run` command to the Hotglue CLI that recreates a job's execution environment on your local machine so you can reproduce exactly what happened in production.


The command downloads the job's input files and generates a` .env` file containing the environment variables that were present when the job ran, allowing you to debug against the same inputs and configuration used by Hotglue.


Once downloaded, run your ETL script locally using your preferred tooling. The generated` .env` file works with VS Code launch configurations or can be sourced directly from your terminal.


Full docs:[https://docs.hotglue.com/cli/etl#etl-set-up-local-job-data](https://docs.hotglue.com/cli/etl#etl-set-up-local-job-data)


# ⚡ New Connectors


**New connectors now available:** Airwallex (payments), CMiC (construction ERP), 7shifts (restaurant operations), and Rillet and DualEntry (AI-native ERPs).


We're continuing to expand the Hotglue connector library to help you integrate with the systems your customers rely on every day.


That’s all for this month! Thanks for reading :)


Want to chat with our team?[Book a demo](https://hotglue.com/demo) .


TABLE OF CONTENTS


- 🚀 Product Updates
- 🔄 Access tokens on demand
- 🛠️ Reproduce ETL issues locally
- ⚡ New Connectors


RECOMMENDED BLOGS


[hotglue melt: December 2025 Improved job logs view, widget v3 updates, audit trail, and more!](https://hotglue.com/blog/hotglue-melt-december-2025)


[Introducing the new and improved widget Announcing the brand new version of the widget, featuring enhanced performance, customizability, and a refreshed design.](https://hotglue.com/blog/introducing-the-new-and-improved-widget)


[JavaScript & TypeScript Support for Hotglue Transforms Hotglue now supports writing transformation scripts in JavaScript or TypeScript in addition to Python!](https://hotglue.com/blog/js-ts-support-for-hotglue-transforms)
