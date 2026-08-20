---
schema_version: "1.0.0"
document_id: "bd6bb29afdbf7f33de5fcb0ac34bca36ecc16f6ab82ff205169b80e47cf622c9"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-environments-for-mastra-platform"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-24T10:56:34.426085+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:c41b57259b6499029d867be793bc460b62866882bcaab7dc3389cd6ace376ee6"
---

# Introducing Environments for Mastra Platform

One of the oldest patterns in shipping software is to test production-like deployments in a test or staging environment. Now, with[Environments](https://mastra.ai/docs/mastra-platform/environments) on the Mastra platform, you can spin up multiple environments for your projects to make testing your Mastra apps even easier.


Your browser does not support the video tag.


Environments also bundle Studio and Server into a single deployment command for even easier deployments. Just run` mastra deploy` .


Every Mastra platform project can now run multiple environments. You can create and manage environments for your project in the platform or via the CLI. When you create a project, your first deploy automatically creates a` production` environment. From the CLI, running` mastra env create <env_name>` gives you a new deploy target inside the same project with its own URL, environment variables, deploy history, and optionally, its own hosted database and workspace.


Until now, projects on the platform were by default single environments. If you wanted to test a change before your users saw it, you either deployed over production and hoped, or you stood up a duplicate project and hand-copied every variable into it. But now, you can share environment variables between multiple environments and safely test changes before shipping to production.


Environments can also pin resources to specific geographic regions. You can create separate production environments for users in different geographic[regions](https://mastra.ai/docs/mastra-platform/regions) and use a load balancer to send traffic based on a user’s location. Use regional deployments to colocate resources like your app server, database, and observability pipeline close to your users for optimal performance.


Each environment is isolated along four lines:


- **Its own URL** . Run experiments in your staging Studio. Route traffic to your production Server. Mix-and-match as needed.
- **Its own variables** . Use variables scoped to environments or across multiple environments.
- **Optionally, its own database** . Share a managed LibSQL or Postgres Database across multiple environments or deploy a database for each environment.


## Requirements


Environment-based deploys require version 1.44 or higher of` @mastra/core` . For older projects, we’ll continue to support the legacy deployment flows through` mastra server deploy` and` mastra studio deploy` for the next 90 days, and you should expect to see a deprecation notice in your deployment flow during that time period,


Adding multiple environments is currently limited to the Team and Enterprise plans.


## Get started in the CLI


From your project directory, create an environment and deploy to it — or skip the first command and let the deploy offer to create it for you.


The first deploy creates a production environment:


Terminal


```text
mastra   deploy
```


Create a new environment:


Terminal


```text
mastra   env   create   staging
```


Deploy to a specific environment:


Terminal


```text
mastra   deploy   --env   staging
```


To create an environment in a specific region, pass` --region` at creation. Currently` us` and` eu` are supported:


Terminal


```text
mastra   env   create   eu-preview   --type   preview   --region   eu
```


Then check what's running where:


Terminal


```text
mastra   env   list
```


The output shows each environment's name, region, active deploy status, and managed variables. Pass` --json` for CI.


For more information and full configuration options, read our updated platform docs on[environments](https://mastra.ai/docs/mastra-platform/environments) and[deploys](https://mastra.ai/docs/mastra-platform/deploy) .
