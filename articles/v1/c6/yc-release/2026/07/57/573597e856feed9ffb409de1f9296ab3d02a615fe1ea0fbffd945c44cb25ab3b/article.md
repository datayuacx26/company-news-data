---
schema_version: "1.0.0"
document_id: "573597e856feed9ffb409de1f9296ab3d02a615fe1ea0fbffd945c44cb25ab3b"
company_key: "yc-release"
company: "Release"
source_id: "yc-release-news-import-be48e1d74beb"
canonical_url: "https://release.com/blog/introducing-planetscale-integration-for-release"
published_at: null
first_seen_at: "2026-07-23T22:44:42.193831+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:e38b3fe26e6101b08b45ea1a3d88d02c7f3b013aa8bdafd6b43c0077c82dc142"
---

# Introducing PlanetScale Integration for Release

Setting up isolated databases for every pull request environment has always been one of the harder parts of ephemeral infrastructure. While spinning up application services is fast, databases often introduce delays, complexity, and operational overhead. Today, we're making that easier. Release now integrates with[PlanetScale](https://planetscale.com/) , giving you a faster, simpler way to provision database environments for every PR.


## Why PlanetScale


If you've worked with traditional cloud databases, you're familiar with the tradeoffs. They're powerful and flexible, but provisioning new instances or restoring snapshots can take time. Networking and access configuration can also add friction, especially when you're trying to move quickly in a development workflow.


PlanetScale takes a different approach. It's a fully managed database platform that supports both MySQL and PostgreSQL, designed around a branching model. Instead of provisioning full database instances, you create lightweight branches that represent a copy-on-write view of your data. These branches can be created quickly and discarded just as easily.


This model maps naturally to how Release environments work. Each environment already represents an isolated slice of your application stack. With PlanetScale, your database can follow the same pattern—one branch per environment, created on demand and removed when no longer needed.


There's also no infrastructure to manage. Because PlanetScale is a managed service, you don't need to think about VPCs, subnets, or security groups. Your application environments can run in AWS, GCP, or anywhere else, while your database lives in PlanetScale and is accessed securely over the internet with SSL.


## How It Works


Let's take a look at how the integration fits into your existing workflow.


Once you install the PlanetScale integration in Release, you can create an Instant Dataset backed by a PlanetScale database. If you're new to datasets, they're a core concept in Release that lets you define a source database and maintain a pool of ready-to-use copies so environments can start instantly. You can learn more in the[Instant Datasets documentation](https://docs.release.com/instantdatasets/introduction) .


With PlanetScale, instead of provisioning full database instances or restoring snapshots, Release creates a new database branch whenever an environment is spun up. That branch is based on the current state of your source database and is isolated to that specific environment.


Here's what the lifecycle looks like in practice:


1. You open a pull request.
2. Release creates a new environment for that PR.
3. A corresponding PlanetScale database branch is created automatically.
4. Your application services start up and connect to that branch.
5. When the environment is torn down, the branch is deleted.


Because PlanetScale branches are lightweight and copy-on-write, this process is fast. There's no waiting for a database instance to provision or for a large snapshot to restore. Your environments come up quickly and consistently, with a database that reflects the state you expect.


Connection details are handled for you. When the environment is created, Release automatically injects the necessary credentials into your containers as environment variables. For a dataset named` my-app` , you'll see:


- ` MY_APP_DB_POOL_HOST` — the PlanetScale connection hostname
- ` MY_APP_DB_POOL_USER` — the database username
- ` MY_APP_DB_POOL_PASS` — the database password
- ` MY_APP_DB_POOL_DATABASE_NAME` — the source database name
- ` MY_APP_DB_POOL_NAME` — the branch name created for the environment


Your application can use these variables directly, just like it would with any other dataset provider. There's no additional configuration required to wire up connectivity.


From a developer's perspective, this means each PR environment has its own isolated database, created automatically and cleaned up when it's no longer needed. You don't have to coordinate shared staging databases or worry about test data collisions between environments.


## Getting Started


Getting started with PlanetScale on Release is straightforward.


First, create an account on[PlanetScale](https://planetscale.com/) and set up a database. From there, you'll generate a service token with the required permissions—database access, branch management, and connection capabilities.


Next, install the PlanetScale integration in Release and provide the service token. Once the integration is configured, you can create a new Instant Dataset that points to your PlanetScale database.


After that, your environments will automatically start using PlanetScale branches for database provisioning. There's no additional setup required in your application templates beyond referencing the dataset.


For a step-by-step walkthrough, see the[PlanetScale Integration Guide](https://docs.release.com/integrations/integrations-overview/planetscale-integration) .


## What This Unlocks


Adding PlanetScale as a dataset provider gives you another way to tailor your environment strategy based on your needs.


If you're already using PlanetScale, this integration lets you bring that workflow directly into Release without introducing additional infrastructure. If you're exploring options for faster or more flexible database provisioning, PlanetScale's branching model offers a compelling alternative that aligns well with ephemeral environments.


Because PlanetScale supports both MySQL and PostgreSQL, you also have more flexibility in choosing the right database engine for your application while still benefiting from the same environment lifecycle.


And since PlanetScale operates independently of your cloud provider, you're not constrained to a single ecosystem. Your application can run wherever it makes sense, while your database layer remains consistent.


## What's Next


We're continuing to expand the set of database providers and workflows supported by Release. PlanetScale is an important step in that direction, and we're looking forward to seeing how teams use it alongside existing dataset options.


If you're already using Release, you can start experimenting with PlanetScale today.
