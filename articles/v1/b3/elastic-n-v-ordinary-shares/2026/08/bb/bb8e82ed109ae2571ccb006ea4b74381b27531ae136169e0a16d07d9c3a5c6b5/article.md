---
schema_version: "1.0.0"
document_id: "bb8e82ed109ae2571ccb006ea4b74381b27531ae136169e0a16d07d9c3a5c6b5"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/esql-subquery-source-commands"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-07T16:57:11.913468+00:00"
fetched_at: "2026-08-07T16:57:12.625932+00:00"
content_hash: "sha256:9e3295fa2d03886241207db6f0ce90980a8e6d9c514d1373577360c1c396d0d8"
---

# One query, three data sources: ES|QL subqueries get FROM, TS and ROW

[Elasticsearch Query Language (ES|QL)](https://www.elastic.co/docs/reference/query-languages/esql) subqueries now support three source commands in Elasticsearch 9.5:[FROM](https://www.elastic.co/docs/reference/query-languages/esql/commands/from) for indexed data,[TS](https://www.elastic.co/docs/reference/query-languages/esql/commands/ts) for time-series metrics, and[ROW](https://www.elastic.co/docs/reference/query-languages/esql/commands/row) for inline literal values. You can use them individually or combine all three in a single query, filtering log data by live metric behavior or mixing real and synthetic rows without any index setup.


If you've been exploring ES|QL, you might have noticed that a[subquery](https://www.elastic.co/docs/reference/query-languages/esql/esql-subquery) used to feel like it had exactly one front door:` FROM` . And it made sense. Most of the time, queries start with a simple directive to go fetch documents from an index. In an earlier[post](https://www.elastic.co/search-labs/blog/dynamic-filtering-esql-where-in-subquery) , we taught the` WHERE` command a new trick:` IN` subqueries. And before that,[subqueries showed up in the FROM command](https://www.elastic.co/search-labs/blog/esql-subquery-from) to combine data sources. In both instances, every subquery started the same way, with` FROM` .


But not every useful query starts with a bulk document fetch. Sometimes you need to evaluate time-series semantics. Other times, you just need to whip up a tiny inline row for testing. Sometimes, the absolute best input to a filter is a dynamic query that builds the list for you on the fly, rather than a static, hard-coded list.


**Source command**


**Reads from**


**Best for**


**Requirements**


FROM


Indexes, data streams, aliases, views


Stored document lookups, live filter lists


None (works with any index)


TS


Time-series data streams


Metric aggregations with counter-reset handling


TSDS with` index.mode: time_series`


ROW


Inline literal values


Test cases, seed values, synthetic placeholders


None (no index needed)


The` FROM` ,` TS` and` ROW` source commands are generally available (GA) in Elasticsearch 9.5, while the[WHERE IN subquery](https://www.elastic.co/docs/reference/query-languages/esql/esql-in-subquery) remains in technical preview in 9.5.


## How ES|QL subquery source commands work


Think of subqueries as having two specific placements and three different engines.


### Where subqueries go: FROM and WHERE placements


-


**Inside the**` **FROM**` **command:** The subquery is an independent result source, contributing its rows to the outer query. Fields that exist in one source but not the other are gracefully filled with null values.


-


**Inside the**` **WHERE**` **command:** The` IN` subquery contributes dynamic values to be used as a predicate or filter.


### Three source commands for starting a subquery


#### Door #1: FROM (the classic door)


` FROM` is the familiar workhorse. You use it when the subquery should read stored data from indices, data streams, aliases, or views. One of its best use cases is the "stop-copy-pasting-IDs" pattern. Instead of running one query, manually copying the output values, and pasting them into the filter of another query, the subquery becomes your live filter list.


```text
FROM employees
| WHERE emp_no IN (FROM high_value_accounts
| KEEP emp_no
)
| KEEP emp_no, first_name, last_name
```


#### Door #2: ROW (the tiny door)


` ROW` is the lightweight option that requires absolutely no index setup. It allows you to build rows completely out of literal, inline values. This makes` ROW` useful for small seed values, test cases, allow/deny lists, or one-off "what if?" scenarios. In the query below,` ROW` is the perfect way to staple a synthetic sentinel row or placeholder directly onto real data.


```text
FROM
(FROM access_logs
| WHERE status == 500
| KEEP cluster, status),
(ROW cluster = "synthetic", status = 0)
| SORT status
| KEEP cluster, status
```


#### Door #3: TS (the time-series door)


The` TS` command targets time-series data streams and enables time-series aggregation functions. Why not just use` FROM` for metrics?[TS](https://www.elastic.co/docs/reference/query-languages/esql/commands/ts) is uniquely optimized for time-series data, and it natively handles tricky scenarios, like counter resets on process restarts and uneven metric publish intervals. Using TS as a subquery lets you filter your application logs by what your metrics are saying. For example, imagine asking ES|QL: *Show me log events only from clusters whose peak metric throughput crossed 800* :


```text
FROM access_logs
| WHERE cluster IN (TS k8s_metrics
| STATS peak = MAX(bytes_in) BY cluster
| WHERE peak > 800
| KEEP cluster
)
| SORT cluster, path
| KEEP cluster, status, path
```


In this scenario, the filter is reacting to live metric behavior rather than being hard-coded.


## Combining FROM, TS and ROW in one query


Because subquery placements and source commands are independent, you can freely mix and match them. You can throw all three doors into a single` FROM` union to generate a cohesive table containing real logs, a live metrics summary, and a synthetic row.


```text
FROM
(FROM access_logs
| KEEP cluster, status),
(TS k8s_metrics
| STATS peak = MAX(bytes_in) BY cluster),
(ROW cluster = "synthetic")
| STATS log_events = COUNT(status), peak = MAX(peak) BY cluster
| SORT cluster
| KEEP cluster, log_events, peak
```


## ES|QL subquery constraints


A few constraints to keep in mind before using subqueries:


-


` **IN**` **subqueries demand one column:** If a subquery feeds an` IN` operator, it must project exactly one column. Use the` KEEP` command to make that explicitly clear.


-


` **TS**` **requires a time series data stream (TSDS):** The` TS` command only works on data stored in a[TSDS](https://www.elastic.co/docs/manage-data/data-store/data-streams/time-series-data-stream-tsds) , which uses` index.mode: time_series` .


## The takeaway: when to use each source command


Subqueries in ES|QL provide a structured way to compose queries by using the results of one query as the input to another. Choosing the appropriate source command (` FROM` ,` ROW` , or` TS` ) lets you combine data and generate inline values. It also lets you filter dynamically without duplicating query logic. For more details and additional examples, see the ES|QL subqueries documentation.


-


[Subquery in FROM command](https://www.elastic.co/docs/reference/query-languages/esql/esql-subquery)


-


[Subquery in WHERE command](https://www.elastic.co/docs/reference/query-languages/esql/esql-in-subquery)
