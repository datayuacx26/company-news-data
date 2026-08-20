---
schema_version: "1.0.0"
document_id: "d0bd9e383d4f33effa1c91739135056bfbe9b9a9c945a9c8c3c4599799604e57"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/mocking-postgresql-the-easy-way-simplifying-testing-with-speedscale-proxymock/"
published_at: "2025-11-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:25:08.967255+00:00"
content_hash: "sha256:1eef5d19610976424bec0855fd0dfbabde9ab981d7de8102da47e0a03a2f9908"
---

# Mocking PostgreSQL the Easy Way: Simplifying Testing with Speedscale Proxymock

Every developer who’s worked with PostgreSQL knows the pain: testing against a real database slows everything down.


You need the database running locally, loaded with the right data, and configured to match production as closely as possible. Every time you run a new test or build, you’re forced to repeat that setup migrate schemas, seed test data, and clean everything up again.


It’s time-consuming, brittle, and hard to scale across a team.


As Matt points out in[his video](https://youtu.be/hNbeoyiTmbs) , database testing often becomes the **biggest bottleneck in modern development pipelines** . Whether you’re running integration tests on your laptop or spinning up ephemeral environments in CI/CD, managing a real PostgreSQL instance just to run basic CRUD tests wastes time and resources.


---


## Typical Solution


Developers usually take the “just clone it” approach.


They copy the PostgreSQL database, sanitize production data, spin up a Docker container or local instance, and load the dataset. It works, but it’s inefficient. Each time the schema changes or a test modifies data, the process needs to start over.


Here’s what the typical workflow looks like:


1. Start PostgreSQL locally (often via Docker).
2. Run migrations.
3. Load sample or scrubbed production data.
4. Run the tests.
5. Reset the database and repeat.


It’s a familiar pattern — and a huge time sink.


This approach also doesn’t guarantee test stability. Two developers might run the same test with slightly different datasets and get different results. And in CI/CD pipelines, starting a PostgreSQL container and loading data for every job increases run times significantly.


In short, it’s slow, inconsistent, and expensive.


---


## Our Solution: Proxymock


With` proxymock` , you can replace all that overhead with a single, elegant solution.


Instead of running a local PostgreSQL instance, you can **record your real database interactions once** and then **mock them** during future runs.


Proxymock sits transparently between your application and PostgreSQL, recording every query, prepared statement, and result. Later, it replays those interactions — so your app behaves exactly as if it’s connected to a real database, but without PostgreSQL actually running.


You get the realism of live database responses without the complexity of maintaining one.


In practice, this means you can:


- Run local or CI tests instantly without starting PostgreSQL.
- Reproduce production-like results deterministically.
- Avoid data drift between runs.
- Simplify setup and teardown for developers and pipelines alike.


It’s the same power as having a lightweight, instant PostgreSQL clone that just works.


---


## Step 1: Record PostgreSQL Traffic


The first step is to record your app’s real interactions with PostgreSQL. Run this command in a dedicated terminal window:


```text
proxymock   record   --reverse-proxy   15432=localhost:5432   --app-port   8080


```


This tells` proxymock` to:


- Listen on port **15432** for PostgreSQL traffic.
- Forward all requests to the real PostgreSQL instance at **5432** .
- Capture every SQL query, result set, and timing metric in RRPair files (Request/Response Pairs).


Think of it as a “black box recorder” for your database — it records once so you can replay infinitely.


## Step 2: Configure Your Application


Now you’ll configure your application to route traffic through the proxy instead of directly to PostgreSQL.


In a new terminal window, run your app with modified environment variables:


```text
PGUSER  =<  user  >   PGPORT  =  15432   go   run   main.go


```


Here, we’re telling the app to connect through port` 15432` , where` proxymock` is listening. Your real PostgreSQL instance is still running at` 5432` , but` proxymock` now intercepts every command.


This setup works for any PostgreSQL client — not just Go. Python, Node.js, Java, Ruby — all follow the same pattern.


## Step 3: Inspect What You Recorded


Once you’ve generated traffic (by running queries, tests, or your app), inspect what` proxymock` captured:


```text
proxymock   inspect


```


You’ll see all the SQL statements your app executed, along with the data returned by PostgreSQL.


Behind the scenes, each captured interaction is stored as JSON, even though PostgreSQL uses a binary wire protocol. This makes it easy to review or edit.


A typical RRPair might include:


- The original SQL query or prepared statement.
- The response rows and metadata.
- Timing details like latency or execution duration.


You can even modify the JSON files manually to simulate different query results.


## Step 4: Run the Mock Server


Now for the fun part. Once recording is done, you can stop your local PostgreSQL server (to free up port 5432) and start` proxymock` in mock mode:


```text
proxymock   mock


```


Your application can now connect to` localhost:5432` like usual, but` proxymock` will serve the recorded responses.


From the app’s perspective, nothing has changed — but in reality, it’s running entirely without a database.


## Step 5: Modify or Transform Responses


Sometimes, you’ll want to modify what your mock returns. Maybe you want to simulate an error, or test how your app handles empty results.


You can edit the JSON manually, or automate the process using Speedscale’s transformation system:


```text
proxymock   cloud   push   snapshot
proxymock   cloud   pull   snapshot   <  i  d  >


```


In the Speedscale UI, you can configure transformations — like changing returned values, introducing latency, or injecting errors — and then pull those updates back into your mock.


---


More information about transforms can be found here:[https://docs.speedscale.com/guides/replay/mocks/edit-sig/](https://docs.speedscale.com/guides/replay/mocks/edit-sig/)


## Value


Let’s talk about what this means in real terms.


If it takes about **1 hour a day** for a developer to reset and reload their PostgreSQL database for tests, and your team has 8 engineers, that’s **8 hours wasted daily** — nearly **40 hours per week** .


With` proxymock` , recording a PostgreSQL session takes about five minutes, and then you can replay it endlessly.


That’s a **90% reduction** in setup time.


Multiply that across teams, CI runs, and staging environments, and you’re looking at hundreds of saved engineering hours each month — all while getting **more reliable, deterministic test results** .


You don’t just save time — you make testing faster, cleaner, and more consistent across environments.


---


## Conclusion


Database mocking doesn’t have to be complex or fragile. With` proxymock` , you can record once, replay infinitely, and eliminate one of the most persistent bottlenecks in testing.


If you’re ready to speed up your local and CI workflows:


- 🎥[Watch Matt’s video](https://youtu.be/hNbeoyiTmbs) to see how database mocking transforms testing speed.
- 📚[Read the full Proxymock docs](https://docs.speedscale.com/proxymock/guides/postgres/) for deeper details.
- ⚡[Install Proxymock](https://docs.speedscale.com/proxymock/getting-started/installation/) and try mocking your PostgreSQL database today.


Simplify your stack. Speed up your builds. Spend less time managing databases — and more time shipping great software.
