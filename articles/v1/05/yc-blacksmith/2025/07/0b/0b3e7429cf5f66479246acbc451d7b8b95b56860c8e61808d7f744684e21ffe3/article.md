---
schema_version: "1.0.0"
document_id: "0b3e7429cf5f66479246acbc451d7b8b95b56860c8e61808d7f744684e21ffe3"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/logging"
published_at: "2025-07-02T00:00:00+00:00"
first_seen_at: "2026-07-23T03:46:48.319296+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:c92486a9a69cbfc0da2665c63a376740a69f68965ed6a430bd4dc2a7eba75e56"
---

# How we built a logging platform for GitHub Actions with ClickHouse

We’ve all had that experience: you push a new commit, wait for the CI build to go green, but instead all you see is *red* . You inspect the failure, and it doesn’t seem related to your change. But you’ve seen stranger things. Maybe it’s a flaky test. Maybe infra’s acting up again. You start clicking through other PRs, hunting for patterns. Something similar. Anything.


There has to be a better way.


When it comes to observability in CI, logs are still the main tool in a developer’s toolbox. Viewing the logs for a single failing job helps, but being able to search across *all* logs makes finding the root cause much easier. That’s why we built a logging platform for GitHub Actions.


## ‍ **Where do all the logs go?** ‍


Before building anything, we had to decide how to ingest logs and where to store them. We knew we wanted fast substring lookups over large datasets, plus support for aggregate queries, such as plotting how often a specific search term appears over time. And we wanted all of this to feel fast while keeping operational complexity and cost as low as possible.


ClickHouse was already a major part of our analytics stack, so it was the main option we evaluated. It checked a lot of boxes:


- Low operational overhead and team familiarity
- Costs scaled affordably with data volume
- Fantastic compression (about 12x reduction in size!)
- Strong performance in our initial load test, particularly for aggregate queries across millions of log lines


In comparison, a traditional ELK stack added operational complexity and cost that just didn’t make sense for our needs. With ClickHouse we don’t need to run a Kafka cluster to buffer intermediate messages, or manage an Elasticsearch cluster just for logs. With ClickHouse as the single home for all our observability data, it’s easy to join across tables as we build new features. For all these reasons, we chose the logs to be stored in ClickHouse.


## ‍ **Thinking in ClickHouse** ‍


To make this work, the first thing we needed to do was make sub-string text search on ClickHouse performant. The challenge here is narrowing down which rows might contain a given sub-string without scanning everything. If your query is already scoped by other filters — say, a specific job ID — then it’s not a big deal. But when you're scanning millions of log lines for an arbitrary snippet of text, you want a smarter strategy.


ClickHouse solves this with its n-gram Bloom filter index, which is surprisingly well-suited to this kind of problem. It breaks a text column into n-sized chunks, organizes rows into buckets (called *granules* ), and uses a Bloom filter to probabilistically check if the granule might container your target sub-string. These filters can return false positives, but never false negatives. So, if a granule’s Bloom filter says the string isn’t there, you can safely skip that entire chunk of data. This makes log search very performant. If you’re curious, this[blog post from Tinybird](https://www.tinybird.co/blog-posts/using-bloom-filter-text-indexes-in-clickhouse) goes deeper into how the Bloom filter index works.


There are a couple of other ClickHouse features that we’ve leaned on heavily and worth mentioning. One is materialized columns. We use this to infer the log level for each row based on a set of well-known failure patterns. This makes it easier to quickly spot which log lines are actually worth a look, and surface those up to customers. The other is their row-level TTL. It lets us control data retention at the row level, which is convenient for when different customers have different retention policies. **‍**


## **Log ingestion pipeline** ‍


Here’s what happens when a job runs on Blacksmith.


We start up a Firecracker virtual machine (VM). That VM runs the jobs and starts writing log file artifacts. For security reasons, the VM doesn’t have direct access to our infrastructure, so it can't send logs. We can’t give untrusted workloads the keys to our kingdom.


Instead, the host machine running the VM spins up an HTTP server. From inside the VM, a daemon process we call *blacksmithd* , combined with some coordination with GitHub’s runner process, watches the log files being written to disk. Once those writes complete, they’re securely streamed up to another daemon that runs on the host machine, which batches them. From there, they’re uploaded to ClickHouse.


You might be wondering where the queue is. Most systems like this have one. It acts as a buffer, giving durability guarantees and absorbing spikes of traffic. But in our case, ClickHouse handles millions of rows per second and has good support for[async inserts](https://clickhouse.com/docs/optimize/asynchronous-inserts) . Adding a queue would only complicate the system without enough benefit — at least for now.


To combat transient network failures and bulk insert operations that might fail, we do implement retries. But you want to make sure that data isn’t duplicated since nobody likes seeing duplicated log lines in the output. It’s important to deduplicate at ingestion time. De-duplicating at query time is costly in ClickHouse, since it’s a columnar store and doesn’t store data on a row-by-row basis. Fortunately, ClickHouse gives us a better option. By combining the ReplacingMergeTree engine with async inserts, we can let ClickHouse take care of deduplication for us. With the right tuning — specifically` non_replicated_deduplication_window_for_async_inserts` and` non_replicated_deduplication_window_seconds` — you can configure it to cache recently inserted rows and deduplicate them at ingestion time.


## ‍ **Query language design** ‍


Once logs are ingested into ClickHouse, we needed a flexible way to query them. From the get-go we knew that we wanted to build a lightweight query language that would support nearly all use-cases, and give Datadog a run for its money. Our requirements included:


- Substring matching
- Property matching (e.g. searching by job_id or branch)
- Logical and comparison operators like AND, OR, NOT


Since our logs were ingested into ClickHouse we knew that we would need to compile these queries down into SQL. Lucene’s query language is a common interface for search logs. So it became an obvious choice. But we opted to keep the initial version simple. So we skipped full compatibility, and only borrowed the best parts to meet the requirements. Still giving developers a familiar starting point. **‍**


```text
branch:main level:error,warn (failure OR panic) -"econn refused"
```


Once the language specification was fleshed out, we wrote a small parser that takes a query string, like the one above, and converts it into a parse tree.


From there, it’s transformed using a *toSql()* method. This design gives us flexibility and allows us to optimize certain SQL queries that we perform when doing lookups against other tables. For example, GitHub’s control plane doesn’t give us the job ID at execution time. At ingestion time, we only get an execution token. Later on, we have to do a look up and map that token back to a job ID. With this design, we can cleanly support that kind of lookup.


## ‍ **Aggregation queries** ‍


ClickHouse shines here. The kinds of aggregation queries we use for histograms and trends run smoothly even across millions of rows. That means you can look back over millions of logs to spot patterns, and see what’s changed with a particular workflow over time.


## ‍ **GitHub Actions, but observable**


We’re excited to continue improving observability for GitHub Actions. It’s been far too long waiting for GitHub to come up with something like this. This is already available to all our existing customers. Stay tuned for the next phase. But don’t wait, try it out.
