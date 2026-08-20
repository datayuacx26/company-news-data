---
schema_version: "1.0.0"
document_id: "d630a9d6b2b6baef17786f50a1e2ecf31aba7600b199f90ec2ad592058d85a6c"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/mysql-mocking-with-speedscales-proxymock-a-complete-guide/"
published_at: "2025-10-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:55:41.466450+00:00"
content_hash: "sha256:03e165c1a55cd80f753b4f15557557520bb0a3efc3c66d37e1a5965bd008684c"
---

# MySQL Mocking with Speedscale's Proxymock: A Complete Guide

## **Introduction**


Testing database-driven applications is notoriously painful.


If your app depends on MySQL, you’ve probably spent hours setting up local databases, running migrations, loading data, and then cleaning everything up just to rerun your tests. This repetitive cycle slows development, breaks pipelines, and introduces inconsistency between local and production environments.


Matt explains this perfectly in[this video](https://youtu.be/ScXVxLfFXe8?si=2JfTZ3Pt98mOxZ4U) : when your testing process depends on a live database, you’re constantly fighting setup issues rather than building features. Every developer wants accurate tests, but not at the cost of productivity.


This is where Speedscale’s` proxymock` comes in. Proxymock allows you to record and mock MySQL traffic, enabling your application to behave as if it’s connected to a real database without requiring one to be running. This blog will guide you through what MySQL mocking is, its benefits, and how to implement it step-by-step using proxymock.


## **What is MySQL and Why Mock It?**


MySQL is one of the world’s most popular open-source relational database management systems (RDBMS). It powers countless applications, from small web apps to enterprise-grade systems.


But when it comes to local development and CI/CD testing, running a full MySQL instance can be unnecessary or inefficient. Mocking solves this problem by allowing you to:


- Simulate database responses without an actual running database.
- Reproduce production scenarios consistently for debugging.
- Speed up local testing by removing database setup time.
- Run CI pipelines faster without containerized or external MySQL dependencies.
- Avoid waiting for data refreshes or shared environment resets.
- Work with your **own isolated database instance** without stepping on other developers’ data.


Proxymock makes this easy by recording real MySQL traffic and then replaying it in subsequent tests.


## Typical Solution


The traditional approach is to replicate production as closely as possible. You make a copy of your database, clean the data, spin up MySQL locally, and load the copy into it. Then you point your application at this database and start testing.


That works… until you realize you have to do it again every time your schema or data changes. Each test run becomes a mini DevOps project.


It looks something like this:


This approach is functional but inefficient. Resetting the database, reloading data, or reproducing complex transactions across multiple tables takes time. On large teams, that delay compounds fast.


Every engineer ends up babysitting their test data instead of building or debugging code.


## **Our Solution: Service Mocking**


At Speedscale, we built proxymock to make **service mocking** effortless.


Instead of running a real database, you can **record and replay** actual MySQL interactions. proxymock captures the communication between your app and a live MySQL service once, then simulates that same behavior later — just like the real thing, but faster and isolated.


It’s like having a local clone of your database that always responds the same way, without sharing infrastructure or wrestling with seeded data.


Key benefits include:


- **Realistic behavior** : all queries and responses match real-world traffic.
- **Repeatable results** : your tests are consistent across runs and machines.
- **Instant startup** : no waiting for MySQL to boot or data to load.
- **Simplicity** : setup takes minutes, not hours.


## How It Works


Here’s what the workflow looks like in practice.


## **Recording MySQL Traffic with Proxymock**


When debugging or testing database-dependent services, being able to replay real MySQL interactions can save hours of setup and guesswork. Proxymock makes this possible by recording live database traffic into **Request/Response Pair (RRPair)** files.


### **Start the Recorder**


In a dedicated terminal, run:


```text
proxymock   record   --map   13306=localhost:3306   --app-port   8080
```


This command tells Proxymock to:


- Listen on port **13306**
- Forward all traffic to the real MySQL server at **3306**
- Capture each request and response as an RRPair


You can learn more about how this works in the[proxymock docs](https://docs.speedscale.com/proxymock/) .


### **Configure Your MySQL Connection**


In a separate terminal, configure your application to route traffic through the proxy:


```text
export   MYSQL_HOST  =  localhost
export   MYSQL_PORT  =  13306
export   MYSQL_DATABASE  =  auth_db
java   -jar   target/auth-1.1.7.jar
```


Normally, apps connect directly to localhost:3306.


By switching to 13306, you route all database queries through Proxymock, enabling it to record them.


If you’re using the demo app, make sure MySQL is running and the demo database exists (see the demo README).


### **What Gets Recorded**


After interacting with your app, inspect the captured data:


```text
proxymock   inspect
```


Each RRPair file includes:


- **Request data:** SQL queries, prepared statements, and connection handshakes
- **Response data:** Result sets, errors, and metadata
- **Timing info:** Query execution times and latency


Although MySQL uses a binary protocol, Proxymock converts it into **readable JSON** .


You can even edit these files to simulate different responses or test edge cases.


### **Starting the Mock Server**


Before running the mock server, **stop your local MySQL instance** to prevent port conflicts.


You can now run your app against the standard MySQL port (3306) — Proxymock will act as your simulated database.


Start the mock server with:


```text
proxymock   mock
```


Your MySQL client or app will now connect to Proxymock on port 3306 just like a real database.


### **Modifying Responses**


You can manually edit the RRPair files (Markdown or JSON) to change returned values, inject errors, or tweak timing.


To automate transformations, use **Speedscale Enterprise transforms** :


```text
proxymock   cloud   push   snapshot
```


This uploads your snapshot to the Speedscale Cloud.


A link will be provided — open it in the UI to add transforms such as:


- Randomized IDs or timestamps
- Latency injection
- Error simulation


Click **Save** , then download the modified snapshot:


```text
proxymock   cloud   pull   snapshot   <  i  d  >
```


You’ll see a new .metadata directory containing your transform definitions. Next time you run:


```text
proxymock   mock
```


Those transforms will automatically apply to your mock.


Proxymock gives you a full loop — **record, inspect, and replay** — for realistic database simulation.


Perfect for testing, troubleshooting, or CI/CD environments without needing a live database.


## Value of Proxymock


The ROI of mocking MySQL with` proxymock` is immediate and measurable.


Let’s do some quick math:


- Setting up and reloading a local MySQL database typically takes about 1 hour per developer per day (between migrations, data loads, and cleanup).
- Using` proxymock` , you can record once and replay indefinitely. Setup time drops to under 5 minutes.


If you have a 10-person dev team:


- Traditional setup: 10 hours/day spent on database maintenance.
- With` proxymock` : <1 hour/day total.


That’s 45+ hours saved per week, or nearly two full work weeks per month reclaimed for actual development.


And the best part? You get repeatable, production-accurate test environments that don’t require babysitting.


For CI/CD pipelines, the savings are even more dramatic. You can remove MySQL service dependencies entirely, reducing build times and cloud costs while improving reliability.


## **Troubleshooting Recording Issues**


If you encounter problems while recording traffic, check the following:


- Ensure your MySQL driver supports **SOCKS5 proxy** or use reverse proxy mapping.
- Verify the` tcp_proxy` environment variable is set correctly.
- Make sure your MySQL server is actually accessible by proxymock.,


```text
proxymock   cloud   push   snapshotproxymock   cloud   pull   snapshot   <  i  d  >
```


## **When to Use MySQL Mocking**


MySQL mocking is especially valuable for:


- CI/CD pipelines where you want deterministic test results.
- Load testing without setting up massive amounts of test data.
- Feature development when backend teams are still building the actual database schema.
- Resilience testing to ensure your app handles database downtime gracefully.


## **Conclusion**


Proxymock makes it simple to replace a real MySQL database with a lightweight, simulated environment. By recording real interactions and replaying them later, you can dramatically simplify local development and testing.


With this setup, you’ll be able to:


- Record MySQL traffic once.
- Run your app in mock mode without a real database.
- Customize responses for advanced testing scenarios.


This approach not only saves time but also ensures consistency and reliability across your dev and CI environments.


### **Next Steps**


If you’re tired of database setup slowing down your testing workflow, it’s time to give` proxymock` a try.


[Watch Matt’s video](https://youtu.be/ScXVxLfFXe8?si=2JfTZ3Pt98mOxZ4U) - he walks through the core pain points and why mocking is a game-changer.


[Read the MySQL Docs](https://docs.speedscale.com/proxymock/guides/mysql/) - for detailed instructions, architecture visuals, and advanced examples.


[Download Proxymock](https://speedscale.com/proxymock/) - and start mocking your MySQL traffic in minutes.


Simplify your stack. Speed up your tests. Get back to building.
