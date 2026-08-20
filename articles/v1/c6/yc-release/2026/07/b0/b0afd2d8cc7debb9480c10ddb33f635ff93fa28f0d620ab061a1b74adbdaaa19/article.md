---
schema_version: "1.0.0"
document_id: "b0afd2d8cc7debb9480c10ddb33f635ff93fa28f0d620ab061a1b74adbdaaa19"
company_key: "yc-release"
company: "Release"
source_id: "yc-release-news-import-be48e1d74beb"
canonical_url: "https://release.com/blog/introducing-neon-integration-for-release"
published_at: null
first_seen_at: "2026-07-24T10:54:59.054180+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:54df2f18e00cc8d55eeeafc82e8bfa0b1e93243a1b95e2b445663806c42a6463"
---

# Introducing Neon Integration for Release

Databases have always been the bottleneck in ephemeral environments. Application containers spin up in seconds, but databases often lag behind—waiting for snapshots to restore, instances to provision, or networking to configure. Today, we're eliminating that bottleneck for PostgreSQL users. Release now integrates with[Neon](https://neon.tech/) , giving you instant, isolated database branches for every pull request environment.


## Why Neon


Neon is a serverless PostgreSQL platform built around a storage architecture that separates compute from storage. This design enables something powerful: database branching that works like Git branching. Creating a new branch is a copy-on-write operation against the storage layer, which means it completes in seconds regardless of how large your database is. A 500GB production database branches just as fast as a 50MB development database.


This is a fundamentally different model from traditional cloud databases, where provisioning a new instance means allocating compute, restoring data, and configuring networking. With Neon, none of that overhead exists. Branches share the underlying storage with their parent, so you only pay for the data that actually changes.


For ephemeral environments, this is ideal. Each environment gets a complete copy of your database, branched from the current state of your project, with its own compute endpoint that scales to zero when idle. There's no infrastructure to manage, no VPCs to configure, and no waiting.


## How It Works


The integration follows the same pattern as our other dataset providers, but leverages Neon's branching model for speed and simplicity.


Once you install the Neon integration in Release, you can create an Instant Dataset backed by your Neon project. If you're new to datasets, they're a core concept in Release—a pool of ready-to-use database copies so environments can start instantly. You can learn more in the[Instant Datasets documentation](https://docs.release.com/instantdatasets/introduction) .


With Neon, instead of restoring snapshots or provisioning instances, Release creates a new branch from your project's default branch whenever an environment spins up. Here's the lifecycle:


1. You open a pull request.
2. Release creates a new environment for that PR.
3. A Neon branch is created from your default branch, along with a dedicated compute endpoint.
4. A database role is provisioned with connection credentials.
5. Your application services start up and connect to the branch.
6. When the environment is torn down, the branch and its compute endpoint are deleted.


Because Neon branches are copy-on-write, this entire process is fast. There's no data copying, no snapshot restoration, and no waiting for compute to warm up. Your environment comes up with a database that reflects the exact state of your project.


Connection details are injected automatically. For a dataset named` my-app` , Release provides:


- ` MY_APP_DB_POOL_HOST` — the Neon connection hostname
- ` MY_APP_DB_POOL_USER` — the database username
- ` MY_APP_DB_POOL_PASS` — the database password
- ` MY_APP_DB_POOL_DATABASE_NAME` — the database name
- ` MY_APP_DB_POOL_NAME` — the branch name created for the environment


Your application uses these variables the same way it would with any other dataset provider. No code changes required.


## What Makes Neon Different


A few characteristics of Neon make it particularly well-suited for ephemeral environments:


**Instant branching at any scale.** Because branching is a storage-layer operation, it doesn't matter how large your database is. A branch from a terabyte database is created in the same time as a branch from an empty one.


**Scale-to-zero compute.** Neon compute endpoints automatically suspend after a period of inactivity and wake up on the next connection. For PR environments that sit idle between reviews, this means you're not paying for compute you're not using.


**Storage efficiency.** Branches share storage with their parent through copy-on-write. You only pay for the delta—the data that actually changes in each branch. Running 20 PR environments doesn't mean paying for 20 copies of your database.


**PostgreSQL compatibility.** Neon runs standard PostgreSQL. If your application works with PostgreSQL, it works with Neon. There's no proprietary protocol or driver to worry about.


## Getting Started


Setting up Neon with Release takes just a few minutes.


First, create an account on[Neon](https://neon.tech/) and set up a project. Neon projects come with a default branch and a` neondb` database out of the box.


Next, generate an API key from your Neon account settings. Install the Neon integration in Release and provide the API key—that's all it takes to connect.


From there, create a new Instant Dataset and select your Neon project from the dropdown. Your environments will automatically start using Neon branches for database provisioning.


For a detailed walkthrough, see the[Neon Integration Guide](https://docs.release.com/integrations/integrations-overview/neon-integration) .


## What's Next


Neon joins PlanetScale, AWS RDS, Aurora, and Google Cloud SQL as a supported dataset provider in Release. Each provider has its strengths, and we believe in giving teams the flexibility to choose what works best for their stack.


If you're running PostgreSQL and want the fastest possible database provisioning for your ephemeral environments, Neon is worth a look. The combination of instant branching, scale-to-zero compute, and zero infrastructure management makes it a natural fit for the ephemeral environment workflow.


If you're already using Release, you can start using Neon today. If you're new to Release,[sign up for free](https://release.com/signup) and see how isolated environments with production-like data can transform your development workflow.
