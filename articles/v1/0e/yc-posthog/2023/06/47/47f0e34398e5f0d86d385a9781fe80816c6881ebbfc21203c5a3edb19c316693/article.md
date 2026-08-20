---
schema_version: "1.0.0"
document_id: "47f0e34398e5f0d86d385a9781fe80816c6881ebbfc21203c5a3edb19c316693"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/introducing-housewatch"
published_at: "2023-06-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:d978ae015b56e31ef8d0f72d732ed900d8cf457c8e60bca9d07ffbeb65cb4e1c"
---

# Introducing HouseWatch: An open-source toolkit for ClickHouse

# Introducing HouseWatch: An open-source toolkit for ClickHouse


- [Ian Vanagas](https://posthog.com/community/profiles/29296)


- [Yakko Majuri](https://posthog.com/community/profiles/28694)


Jun 12, 2023


- [Product updates](https://posthog.com/blog/product-updates)


,
- [Release notes](https://posthog.com/blog/release-notes)


,
- [ClickHouse](https://posthog.com/blog/clickhouse)


#### Contents


-
-
-
-
-
-
-
-
-


We are big fans of ClickHouse. We rely on it heavily to store and retrieve the massive amount of data we process every day. In doing this at scale for multiple years now, we’ve built a lot of expertise and systems related to ClickHouse.


To formalize and share these, we’ve recently built and launched[HouseWatch](https://github.com/PostHog/HouseWatch)


, an open-source suite of tools for monitoring and managing ClickHouse. HouseWatch is free and works with your existing ClickHouse instance. You can[clone it from GitHub](https://github.com/PostHog/HouseWatch)


and deploy it via Docker Compose.


##


Why we built HouseWatch


We started using ClickHouse in August 2021 when we[moved away from Postgres](https://posthog.com/blog/how-we-turned-clickhouse-into-our-eventmansion)


.


ClickHouse provides tons of easily queryable metadata about your system, but knowing how and what to query is difficult. From our usage of ClickHouse, we’ve built an intuition for this – some of which we’ve documented in our[ClickHouse manual](https://posthog.com/handbook/engineering/clickhouse)


.


We’ve also built many systems and processes for managing clusters. These include:


- Tracking metrics via Grafana
- Querying via Metabase
- Running operations on nodes through` ssh`
- Managing async migrations with[custom-built tools](https://posthog.com/blog/async-migrations)


To share our expertise, formalize these tools, and centralize them in one place, our engineers Li, Yakko, and CTO Tim built the first version of HouseWatch at our[Aruba offsite](https://posthog.com/blog/aruba-hackathon)


. We felt it would be useful to us, and support other ClickHouse users as well.


##


Features


HouseWatch provides a central location for the tools we use to monitor and manage ClickHouse.


###


Query performance and analysis


To help understand the performance of all the queries on your ClickHouse clusters, we provide a list of normalized queries and their performance metrics, with an emphasis on active and slow queries.


Each query includes metrics on average run time, calls per minute, percentage of all IOPs, total IOPs, and percentage of run time. It also includes details on the query itself and the` EXPLAIN` statement. From this, you can sort, monitor, and improve important, slow, or high-stress queries.


###


Schema stats


HouseWatch provides stats for all the tables for your cluster and lets you dive into the details for each of them. For each table, you can see columns, parts, compressed and uncompressed disk space sizes, disk usage, and more.


###


Query editing and benchmarking


With the size of data ClickHouse stores and processes, optimizing your queries is critical. To help you do this, you can run and edit queries in HouseWatch, as well as test and compare them.


You can run queries on your ClickHouse cluster and receive results visualized in HouseWatch. For query comparisons, you can write two queries, run them, and then get metrics on performance across the duration, read bytes, CPU usage, memory usage, and more.


We’ve also built a natural language query editor that uses GPT to create ClickHouse queries based on the table and instructions you provide.


###


Logs and errors


Like any good monitoring tool, HouseWatch provides access to logs and errors from ClickHouse. For both, you can search for specific ones, as well as see the number of and most recent occurrences.


###


Operations


Last but not least is our operations tool. This is inspired by our[async migrations tool](https://posthog.com/handbook/engineering/databases/async-migrations)


, which we've used in production for over a year. It enables you to run and monitor long-running operations like migrations or SQL commands. You can monitor their status, pause or stop them, and retry them if they fail. Failures come with automatic rollbacks as well.


##


Deploying HouseWatch


First, clone the repo.


Terminal


```text
git    clone https://github.com/PostHog/HouseWatch
```


Next, create a` .env` file and add the following environment variables.


```text
CLICKHOUSE_HOST=localhost \    CLICKHOUSE_CLUSTER=mycluster \    CLICKHOUSE_USER=default \    CLICKHOUSE_PASSWORD=xxxxxxxxxxx \
```


Finally, run Docker Compose.


Terminal


```text
docker    compose -f docker-compose.yml up
```


##


Future plans


We aspire for HouseWatch to be like[pganalyze](https://pganalyze.com/)


for ClickHouse. There is more to build to make this a reality including:


- An index advisor tool
- A visualizer for` EXPLAIN` statements
- Support for monitoring multiple instances
- Automatic surfacing of known system issues


> You can see our full to-do list, suggest a feature, find installation details, or contribute by going to the[HouseWatch repo](https://github.com/PostHog/HouseWatch)
>
>
> .


Subscribe to our newsletter


#### build mode


Read by 75,000+ founders and builders


We'll share your email with Substack


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
