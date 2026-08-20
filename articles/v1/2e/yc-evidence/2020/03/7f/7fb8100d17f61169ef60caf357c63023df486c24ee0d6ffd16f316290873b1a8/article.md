---
schema_version: "1.0.0"
document_id: "7fb8100d17f61169ef60caf357c63023df486c24ee0d6ffd16f316290873b1a8"
company_key: "yc-evidence"
company: "Evidence"
source_id: "yc-evidence-news-import-47bf0dc75044"
canonical_url: "https://evidence.dev/blog/motherduck-connector"
published_at: "2024-06-11T00:00:00+00:00"
first_seen_at: "2026-07-21T19:04:43.661863+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:a242085cf22234b4434721605d5a588d5a43901502740308884b2cd85071ddcf"
---

# Evidence MotherDuck Connector

## Native MotherDuck Connector for Evidence


Today, we’re releasing a native MotherDuck Connector for Evidence and a new[template](https://github.com/evidence-dev/evidence-motherduck-template) which includes the connector.


Many of our community members already use MotherDuck via our existing DuckDB connector. This new native connector makes it more straightforward for teams to manage their MotherDuck connection and more manageable for users whose first experience with DuckDB is MotherDuck.


It also provides a platform for us to build MotherDuck-specific features in the future.


## Why it matters


Our community is very excited about MotherDuck, and so are we.


Like MotherDuck, Evidence[is built on DuckDB](https://evidence.dev/blog/why-we-built-usql) . No matter what data connector you are using, Evidence uses DuckDB to power its fast, interactive components and enable you to work with data from multiple sources.


DuckDB is increasingly being built *into* every data tool, but DuckDB isn’t designed to be used as a cloud data warehouse. MotherDuck is solving that problem, and they’re doing it in a delightful, uniquely DuckDB way.


If you have yet to try MotherDuck, we’d encourage you to do so. From the unbelievably fast[data profiling](https://motherduck.com/blog/introducing-column-explorer/) built into their SQL IDE to the shockingly good[AI SQL error fixer](https://motherduck.com/blog/introducing-fixit-ai-sql-error-fixer/) , there’s a lot to be excited about.


MotherDuck's Column Explorer


## Getting Started


### Adding the New Connector to an Existing Evidence App


1. **Update Evidence** : Ensure you have the latest version of Evidence installed.
2. **Install the MotherDuck Connector** :` npm install @evidence-dev/motherduck`
3. **Register the connector** :[Update evidence.plugins.yaml](https://docs.evidence.dev/plugins/source-plugins/#registering-source-plugins)
4. **Connect with your MotherDuck Token** : Navigate to settings, and add your MotherDuck token


### Starting a New Project with the MotherDuck Template


1. [Login to Evidence Cloud](https://www.evidence.app/)
2. Create a new project, and choose the MotherDuck Template
