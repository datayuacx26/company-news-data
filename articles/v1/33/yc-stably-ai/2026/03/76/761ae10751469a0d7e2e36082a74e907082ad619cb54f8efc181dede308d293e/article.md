---
schema_version: "1.0.0"
document_id: "761ae10751469a0d7e2e36082a74e907082ad619cb54f8efc181dede308d293e"
company_key: "yc-stably-ai"
company: "Stably AI"
source_id: "yc-stably-ai-news-import-4d9fcc9f146f"
canonical_url: "https://www.stably.ai/blog/ensure-synthetic-data-for-e2e-testing"
published_at: "2026-03-16T00:00:00+00:00"
first_seen_at: "2026-07-24T02:44:25.851186+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:d5925c1f37fca04b740da595c49f96ff10dd54a73acec0515e1213def5f7c8cf"
---

# How to Ensure You Only Use Synthetic Data for E2E Testing

There is a dangerous, unspoken habit in software engineering: dumping production data into the Staging environment just to get automated tests to pass.


We all know it's a bad idea. It's a massive security vulnerability and a fast track to violating SOC2 or GDPR. Yet, teams still do it. Why? Because developers argue that basic "dummy" data doesn't catch complex edge cases, and that managing purely synthetic relational data is a nightmare.


They are half right. The industry is full of naive advice on how to handle synthetic data—advice that falls apart the second you introduce foreign keys, third-party APIs, or high-volume CI runs. But as a startup, you don't have the time to build massive, over-engineered data-management pipelines. You need solutions that are fast to implement, require zero maintenance, and don't slow down your CI.


If you want to enforce a strict, 100% secure data policy without your test suite collapsing under its own weight, you have to stop using the "easy" workarounds. Here are 3 pragmatic, startup-friendly architectural patterns that actually work.


---


### 1. Stop Relying on` NODE_ENV` for Backdoors (Use Hardened Test Routes)


**The Naive Approach:** To speed up test setup, teams often build a dedicated` POST /_internal/e2e-seed` API endpoint. They wrap it in a simple` if (process.env.NODE_ENV !== 'production')` check, assuming that's enough to keep it safe.


**Why it fails in practice:** Bundler misconfigurations happen. If that environment variable gets statically replaced or misconfigured during a messy deployment, your backdoor ships to Production. Suddenly, anyone can bloat or nuke your live database. While enterprise architects will tell you to "use a CLI script that connects directly to the DB," that is a networking nightmare for a startup whose GitHub Actions runners can't securely access a VPC-protected database. You often *need* an HTTP backdoor for E2E testing.


**The Pragmatic Solution:** If you must use an HTTP backdoor, secure it at the structural level.


First, require a cryptographically secure` E2E_ADMIN_SECRET` in the header of the request. This secret should only be injected into your CI environment and Staging secrets. Second, **physically separate the code** . Create a` server.test.ts` or a dedicated` test-routes/` directory that is *only* imported into the build step when creating a specific Docker image for Staging. If the code literally does not exist in the Production bundle, it cannot be exploited.


### 2. Stop Writing Teardown Scripts (Embrace` ON DELETE CASCADE` )


**The Naive Approach:** If you run 500 tests a day against a shared Staging database, it gets bloated quickly. The common advice is to write a custom "teardown script" that deletes the test data after the run.


**Why it fails in practice:** In a relational database, you can't just delete a User. You have to delete their Transactions, their Audit Logs, and their Organization first to avoid Foreign Key constraint errors. Writing a custom script to delete relations in topological order is a massive maintenance nightmare. Every time an engineer adds a new database table, the E2E teardown script breaks. Startups don't have time for that.


**The Pragmatic Solution:** Embrace **Tenant-Level` ON DELETE CASCADE`** .


Design your application so that *every* record belongs to a top-level` Tenant` ,` Workspace` , or` Organization` . Configure your database schema so that everything cascades from that root entity.


Your teardown process becomes exactly one line of SQL:` DELETE FROM workspaces WHERE id = 'test-workspace-123';` . The database handles the rest instantly and safely. No maintenance, no custom scripts, and no lingering orphaned data in your staging environment.


### 3. Stop Looping ORMs for Bulk Data (Use DB Branching)


**The Naive Approach:** For complex PR branches that require deep, realistic data (like a 5-year multi-currency ledger), teams realize they can't clone Production because anonymization scripts constantly leak PII. Instead, they try to use an ORM (like Prisma or TypeORM) with Faker.js to dynamically generate 10,000 records "on the fly" in a` beforeAll` block.


**Why it fails in practice:** ORMs are notoriously slow at bulk-inserting nested relational data. Looping` prisma.user.create()` 10,000 times will take minutes. It consumes massive amounts of memory and grinds your CI pipeline to a halt.


**The Pragmatic Solution:** Ditch ORM loops and use **Copy-on-Write Database Branching** .


Modern serverless databases (like Neon, Supabase, or PlanetScale) have made enterprise-grade DB branching accessible to startups. Here is the ultimate workflow:


1. Generate your massive synthetic dataset locally using raw SQL (` INSERT INTO ...` ) and save it as a` seed.sql` file.
2. Run this script *once* against a dedicated` base-seed` branch in your cloud provider.
3. In your CI pipeline, stop running seed scripts entirely. Instead, use your provider's API to create an instant **branch** of` base-seed` .


This takes milliseconds. It requires zero data insertion during the CI run, it provides a fresh, fully-populated, 100% synthetic database for every single PR, and you just throw the branch away when the tests finish. It is the holy grail of startup E2E testing speed.


---


### Stop Treating Test Data as an Afterthought


You cannot mandate a "no production data" policy by just writing it in a handbook. Engineers will always gravitate toward whatever data is easiest to use to get their tests passing.


To enforce security without sacrificing speed, you have to lean on practical, low-maintenance patterns. By hardening your test routes structurally, leaning into native database cascades, and leveraging modern DB branching, you make synthetic data not just safer, but significantly faster and easier to use than real data ever was.
