---
schema_version: "1.0.0"
document_id: "9da6b796fe32978fe70a054f025810cabecda05176ceb7fd575850b487f9adbf"
company_key: "yc-ryvn"
company: "Ryvn"
source_id: "yc-ryvn-news-import-a6f556f77bcf"
canonical_url: "https://ryvn.ai/blog/introducing-ryvn-blueprint-marketplace"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-23T23:41:20.719716+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:445d0184307735b21a74544e88afe41d3bb3b3575023b698016395185f02017f"
---

# Introducing Ryvn Blueprint Marketplace

Your app has been running on Railway for a year. Everything works well—until you sign a customer whose security team requires that your app run in their AWS account. Now you're rushing to migrate your app to use Terraform and Helm charts to deploy it.


You have Claude generate a Postgres module, and it applies cleanly—everything looks good, and you now have a database. However, you don't have deletion protection; if someone inadvertently changes the database name, the entire database gets wiped. You don't have storage autoscaling configured, so the disk fills up when your data outgrows the default 20 GB. And Performance Insights isn't enabled, so you can't diagnose why database operations are slow.


There's a big gap between Terraform that applies cleanly and Terraform that is production-ready, and the worst place to learn that difference is after you've deployed to a customer environment.


## Official blueprints


A blueprint is a package of infrastructure with production defaults already set. You set a few inputs, install it, and get a running database or cache without writing any Terraform. Our SRE team maintains **official blueprints** for Postgres and Redis. When installed, official blueprints provision the managed offering on the cloud provider you deploy to: RDS on AWS, Flexible Server on Azure, and Cloud SQL on GCP.


Official blueprints come with deletion protection, backup snapshots, storage autoscaling, encrypted connections, and maintenance windows already configured. These are settings your customer's security team checks during a SOC 2 or on-prem infrastructure review—and we've already done that work, so you don't have to.


Postgres on AWS, Azure, and GCP are three different products with different configuration models. Official blueprints abstract that away. You configure Postgres once, in terms that matter most for your deployment: storage size, high availability, and backup retention. The blueprint translates that into the right resources on whichever cloud you're deploying to.


Here's an example of what it's like to install the Postgres blueprint in a production environment:


When cloud providers update their managed offerings, our team updates the blueprints and your installations pick up the changes on the next deploy.


## Custom blueprints


Official blueprints cover common cloud infrastructure, but your team likely has needs for third-party services such as ClickHouse or Inngest. Fortunately, you can also create your own custom blueprints to bundle and deploy these services across different environments.


For instance, below, we define a blueprint that creates a` clickhouse-operator` along with a` clickhouse` database. We define a single storage size input that is later passed as a configuration value. As an output, we expose the HTTP host, which is dynamically set based on the installation and namespace.


Whoever writes the blueprint decides what's configurable and what isn't. Consumers who install it only need to fill out this predefined input surface.


When dealing with many customer deployments, this consistency matters. Each customer gets the same infrastructure, regardless of who deploys it. The blueprint maintainer makes the decisions once, and new customer environments only require a minimal set of inputs.


## Connecting infrastructure to your application


Once your blueprint is deployed and the infrastructure is provisioned, the next step is to connect it to your application workloads. Normally, this is a tedious process: searching through Terraform modules to find the right outputs, saving those values, and then passing them to your services—or worse, copying them manually into environment variables.


To simplify this, blueprints provide typed outputs that your services can reference directly:


When these outputs change, any services that reference them are automatically redeployed with the updated values.


## Built for agents


While agents have drastically changed the development landscape, cloud platforms weren't built with agents in mind. Without guardrails, an agent can easily leave resources insecurely misconfigured, modify out-of-scope infrastructure, or create dangling resources.


Blueprints provide an interface for AI-driven infrastructure. Instead of giving an agent direct access to your cloud account, you give it access to the[Ryvn CLI](https://ryvn.ai/docs/experimental/cli) . This allows agents to search for available blueprints, review input and output schemas, and handle installing blueprints and wiring them to application workloads end-to-end.


---


Alongside PostgreSQL and Redis, we're launching the Blueprint Marketplace with popular Helm charts, including RustFS, ClickHouse, and LiteLLM. Your team can define custom blueprints for any cloud infrastructure you deploy across customer accounts. If you're standing up a new customer environment, are new to IaC, or want to test a new third-party OSS tool without the configuration overhead, check the marketplace to see if it offers a blueprint that suits your needs.
