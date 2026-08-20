---
schema_version: "1.0.0"
document_id: "32de6284f3f13c7b0f76a7a51ae18bd4a675e83037b25a9d3e7ce502d7fde38b"
company_key: "yc-release"
company: "Release"
source_id: "yc-release-news-import-be48e1d74beb"
canonical_url: "https://release.com/blog/introducing-supabase-integration-for-release"
published_at: null
first_seen_at: "2026-07-24T10:54:59.054180+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:662e4a018bca664d89211d5b7fabe6c670bb116b363d4af95a0058afbae0fd21"
---

# Introducing Supabase Integration for Release

Every ephemeral environment needs its own database. Without one, teams share databases across environments, leading to test pollution, flaky CI, and the constant fear of accidentally breaking someone else's work. Today, we're adding another option for teams that want clean, isolated databases for every pull request. Release now integrates with[Supabase](https://supabase.com/) , giving you dedicated PostgreSQL branches for each environment.


## Why Supabase


Supabase is an open-source Firebase alternative built on PostgreSQL. It gives you a full Postgres database, authentication, realtime subscriptions, edge functions, and storage—all from a single platform. For teams already building on Supabase, their database isn't just a data store. It's the backbone of their application, with Row Level Security policies, database functions, triggers, and extensions that are all part of the schema.


That's exactly what makes Supabase branching valuable for ephemeral environments. When Release creates a branch, it copies your entire schema—tables, views, functions, RLS policies, extensions, and all. Each environment gets a database that mirrors the structure of your production project, so your application behaves the same way it does in production.


Supabase's branching feature is available on the Pro plan and above. Branches provision in under a minute and are fully independent of each other, so there's no risk of one environment's tests affecting another.


## How It Works


The integration follows the same Instant Datasets pattern as our other database providers. If you've used Release with Neon or PlanetScale, the workflow will feel familiar.


Once you install the Supabase integration in Release, you can create an Instant Dataset backed by your Supabase project. Datasets are a core concept in Release—a pool of ready-to-use database instances that environments can claim instantly. You can learn more in the[Instant Datasets documentation](https://docs.release.com/instantdatasets/introduction) .


Here's the lifecycle:


1. You open a pull request.
2. Release creates a new environment for that PR.
3. A Supabase branch is created from your project's default branch, replicating the full schema.
4. Release provisions connection credentials using the Supabase session pooler for reliable IPv4 connectivity.
5. Your application services start up and connect to the branch database.
6. When the environment is torn down, the branch is deleted to clean up resources.


Connection details are injected automatically. For a dataset named` my-app` , Release provides:


- ` MY_APP_DB_POOL_HOST` — the Supabase session pooler hostname
- ` MY_APP_DB_POOL_USER` — the database username (in` postgres.<branch-ref>` format)
- ` MY_APP_DB_POOL_PASS` — the database password
- ` MY_APP_DB_POOL_DATABASE_NAME` — the database name
- ` MY_APP_DB_POOL_NAME` — the branch name created for the environment


Your application uses these variables the same way it would with any other dataset provider. No code changes required.


## Schema Without Data


One important distinction with Supabase branching: branches replicate your **schema but not your data** . This is a deliberate design decision by Supabase to protect production data. When a branch is created, you get all of your tables, views, functions, RLS policies, and extensions—but the tables start empty.


For many use cases, this is exactly what you want. Your application's test suite or seed scripts populate the data each environment needs, and you don't have to worry about production data leaking into development environments.


If your environments need seed data, you have two options:


- **Supabase seed file** : Add a` supabase/seed.sql` to your repository. Supabase runs this automatically when creating branches via the GitHub integration.
- **Application-level seeding** : Run your existing migration and seed scripts as part of the deployment process—the same way you'd set up a fresh database.


## What Makes Supabase a Good Fit


A few characteristics make Supabase particularly well-suited for ephemeral environments:


**Full PostgreSQL with batteries included.** Supabase isn't just a database—it's a platform. Branches include your Auth configuration, RLS policies, database functions, and extensions. Your ephemeral environment behaves like a miniature version of production.


**Fast provisioning.** Branches typically provision in 30-60 seconds. Combined with Release's database pooling, environments can claim a pre-provisioned branch instantly.


**Automatic connectivity.** Release handles the networking complexity. Supabase branch databases use IPv6 by default, which can be a challenge in IPv4-only Kubernetes environments. Release automatically routes connections through Supabase's session pooler, providing reliable IPv4 connectivity without any configuration on your part.


**Pro plan alignment.** Supabase branching requires the Pro plan ($25/month), which is the same plan most production Supabase projects are already on. There's no separate add-on or enterprise tier needed.


## Getting Started


Setting up Supabase with Release takes just a few minutes.


First, make sure your Supabase project is on the Pro plan—branching isn't available on the Free tier. Then generate an access token from your[Supabase account settings](https://supabase.com/dashboard/account/tokens) . Name it something like` release-integration` and set it to never expire.


Install the Supabase integration in Release under **Configuration** > **Integrations** , and provide your access token. That's all it takes to connect.


From there, create a new Instant Dataset and select your Supabase project from the dropdown. Choose how many branches to keep in the pool, and your environments will automatically start using Supabase branches for database provisioning.


For a detailed walkthrough, see the[Supabase Integration Guide](https://docs.release.com/integrations/integrations-overview/supabase-integration) .


## What's Next


Supabase joins Neon, PlanetScale, AWS RDS, Aurora, and Google Cloud SQL as a supported dataset provider in Release. Each provider has its strengths, and we believe in giving teams the flexibility to choose what works best for their stack.


If you're building on Supabase and want isolated database environments for every pull request, this integration is ready to use today. The combination of schema-level branching, automatic pooler connectivity, and zero infrastructure management makes it a natural fit for the ephemeral environment workflow.


If you're already using Release, head to **Configuration** > **Integrations** to get started. If you're new to Release,[sign up for free](https://release.com/signup) and see how isolated environments with production-like databases can transform your development workflow.
