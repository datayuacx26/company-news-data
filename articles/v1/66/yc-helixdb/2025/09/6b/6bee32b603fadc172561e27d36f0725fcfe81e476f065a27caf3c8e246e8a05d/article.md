---
schema_version: "1.0.0"
document_id: "6bee32b603fadc172561e27d36f0725fcfe81e476f065a27caf3c8e246e8a05d"
company_key: "yc-helixdb"
company: "HelixDB"
source_id: "yc-helixdb-news-import-f26087e13605"
canonical_url: "https://www.helix-db.com/blog/developing-a-db-in-prod-hardware"
published_at: "2025-09-22T16:42:08.763+00:00"
first_seen_at: "2026-07-24T11:14:24.417435+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:1818c2ed95662acd69694ac8fe5a51e88d4538c7234ac43f7ad59b1f1850bb37"
---

# Developing a DB in Prod (hardware)

When you're a startup, you usually sacrifice performance in exchange for speed of development.


When you're developing a database, you cannot sacrifice performance **ever** .


Just as we maximise our database's CPU time doing work and minimise the time spent per task, we must maximise the time our engineers spend achieving that and minimise the time spent per improvement.


For those who don't know, HelixDB is an open-source graph-vector database.


## The Friction


Cloud hardware is very different to the machine that you're reading this post on.


The friction is in the feedback loop being much slower as you have to push to some sort of a benchmarking pipeline to validate your performance assumptions.


This could be through CI or through a custom CLI, but both of those add overhead:


- **CI:** you must commit and push, and if you don't have benchmarks on every push you also need to invoke that.


- **CLI:** you now have another tool to maintain and engineers have to get used to the flow.


On top of that, both solutions require the project to compile on the engineers' machines for LSP. We're only targetting Linux and so we'd also have to maintain a local development container.


**We cut all that out.**


## Developing on EC2 instances


We provision each engineer their own workspace with everything set up for them, on the same hardware that runs our database in Production.


We can now run benchmarks at any point:


-


without needing to commit


-


without sending the code to another instance


-


without creating and learning any new tools


-


just as if we were benchmarking locally


We can also instantly inspect the compiled binary for vectorisation and rely on` -march=native` .


## Managing the Cost


Each workspace's schedule can be configured by the engineer - auto-start in the morning, auto-shutdown after periods of inactivity, and of course just manual start and stop.


The cost of running the instance is worth the productivity increase of the engineer.


As for the cost of setting this up, we'd like to give a huge shoutout to[Coder](https://coder.com/) , who's open-source product did 99% of the work and made this project quick and viable.
