---
schema_version: "1.0.0"
document_id: "006cb422bda3b3183ebd9ae0843149ae4e3f4e2b3ab55ac3f016e04de09ee940"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-eu-us-regions-for-mastra-platform"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-24T10:56:34.426085+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:a2baf24efc37ff60284859efe00b98028003026230823bd26fb5ec4d268550b2"
---

# Introducing EU & US Regions for Mastra Platform

When you’re deploying your AI application, where you deploy it matters. Not just the platform that you’re trusting to run your code, but the geographical region where that code is deployed matters almost just as much. Now with[Regions](https://mastra.ai/docs/mastra-platform/regions) , the Mastra platform allows you to ship your code closer to your users and in the jurisdiction of your choice.


Mastra platform environments now support regions both in the web platform as well as the CLI. When you create an environment, select a region and then everything that environment runs — your application server, any hosted databases attached to it, and the observability data it emits — is placed in that region. We currently support three regions in the US and one in the EU.


Now with a choice of where to deploy your Mastra project, teams with a requirement to deploy their applications and host their data in the EU can easily isolate their entire project in a single EU data region. This ensures that not only your application code and database are shipped in the EU, but also all your observability data (traces, logs, metrics, etc.) are stored in the same region.


An added perk of picking a single geographical region is that your application code will run close to your database and, importantly, close to your users. Especially for conversational agents, latency can add up across messages and tool calls, so picking the right region for your project can also be a massive performance improvement.


Today, we’re supporting a handful of regions in the US and one in the EU, but we’re looking to expand into additional regions soon (let us know where you’d like us to go next!). One additional note when choosing a region: it’s immutable after the environment is created, so make sure you’ve got the right region selected when creating a new project or environment.


## Get started with the CLI


From your project directory, create an environment pinned to a region using the us or eu shorthand, then deploy to it:


Create a new EU-based production environment:


Terminal


```text
mastra   env   create   production-eu   --region   eu
```


Or set the region the first time you deploy to production:


Terminal


```text
mastra   deploy   --region   eu
```


Run` mastra env list` to see each environment's region and active deploy.


For more information and full configuration options, read our updated platform docs on[environments](https://mastra.ai/docs/mastra-platform/environments) ,[regions](https://mastra.ai/docs/mastra-platform/deploy#choose-a-region) , and[deploys](https://mastra.ai/docs/mastra-platform/deploy) .
