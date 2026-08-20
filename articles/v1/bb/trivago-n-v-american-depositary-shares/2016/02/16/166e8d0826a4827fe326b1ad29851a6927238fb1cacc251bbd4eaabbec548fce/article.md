---
schema_version: "1.0.0"
document_id: "166e8d0826a4827fe326b1ad29851a6927238fb1cacc251bbd4eaabbec548fce"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2016-02-23-protector/"
published_at: "2016-02-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:32.213667+00:00"
content_hash: "sha256:d780b37a6fd0fe366608d636a29613abbd670a1d61d047df3beec45beca0b901"
---

# Introducing Protector - a Circuit Breaker for Time Series Databases

At trivago we store a subset of our realtime metric data in InfluxDB and we are quite impressed by the load it can handle. Despite all the joy, we had to learn some lessons the hard way. It is pretty easy to overload the database or the web browser by executing queries that return too many datapoints. To prevent that, we wrote[Protector](https://github.com/trivago/Protector) - a circuit breaker for Time series databases that blocks malicious queries.


Time series databases are great! They can store heaps of metric data and allow realtime queries at the same time. At trivago we store a subset of our realtime metric data in InfluxDB 0.8 and we are quite impressed by the load it can handle. Since our[last post](https://tech.trivago.com/2015/04/14/timeseries_influxdb/) we tripled the amount of data we ingest.


## Preventing overload


Despite all the joy, we had to learn some lessons the hard way. One of InfluxDB’s strongest features was also a big source of frustration for us: the Query Engine. InfluxDB features an SQL-like syntax for data queries. Turns out you can run pretty expensive queries without a security net.


For example, to get all data from a series we could write:


```text
select * from myseries
```


This might seem harmless, but look twice: Since we didn’t set any limit here, this statement could potentially return millions of datapoints. Even a simple` list series` query (which returns a list of all time series in a database) could render a browser unusable with a big number of series names.


Here is what it looks like when InfluxDB runs out of memory. The dark orange part shows current memory usage. You can see that the process got killed at around 6pm (18:00) and needed to be restarted. Ouch.


With over a hundred engineers at trivago running InfluxDB queries, issues like these are quite common. Especially since Grafana, our current metrics dashboard, executes queries as you type.


We faced three options:


1. Tolerate service outages for our metrics backend.
2. Enforce a list of rules on all developers and hope that there will be no violations.
3. Make sure that potentially dangerous queries are blocked automatically.


The first two options were unacceptable, so we rolled up our sleeves and wrote a tool to block malicious queries. Today we Open Source[Protector](https://github.com/trivago/Protector) , our Circuit Breaker for Time Series Databases.


## What is a circuit breaker anyway?


A circuit breaker limits CPU and memory usage for each query. It prevents out-of-memory exceptions. Most prominently, Elasticsearch has that functionality built-in. They mention it in the[official documentation](https://www.elastic.co/guide/en/elasticsearch/guide/current/_limiting_memory_usage.html#circuit-breaker) :


> The circuit breaker estimates the memory requirements of a query by introspecting the fields involved (their type, cardinality, size, and so forth). It then checks to see whether loading the required fielddata would push the total fielddata size over the configured percentage of the heap. If the estimated query size is larger than the limit, the circuit breaker is tripped and the query will be aborted and return an exception. This happens before data is loaded, which means that you won’t hit an OutOfMemoryException.


InfluxDB had no such protection before. Some time ago we[asked for it on the mailing list](https://groups.google.com/forum/?utm_medium=email&utm_source=footer#!msg/influxdb/rkpz3Gv918g/BTI2ZsYUCgAJ) but to no avail. If we wanted it now, that meant we needed to build it ourselves.


## Architecture


Protector is a reverse proxy for InfluxDB. It sits in-between the client and the database and listens for queries. Currently it works with InfluxDB 0.8 with support for 0.10 coming soon.


Here is our current setup:


We had the following goals:


- Protect InfluxDB from running out of memory
- Prevent the browser from freezing because of too many incoming datapoints
- Provide developers with helpful hints to refine their queries in case of error.


To make this work, we needed to estimate if a query could be potentially dangerous before executing it.


## Rules


Each query gets checked by a number of simple rules. Each rule will block a certain kind of malicious request to the database. Of course, all these rules are highly opinionated. Therefore we make sure that each of them can be enabled and disabled separately.


Here are some interesting rules enabled by default:


**Prevent too many datapoints** :
We estimate the amount of data to be returned from every query. If it is above a certain threshold, the query gets blocked. The reason is that expensive requests can bring down the time series database or overload the client with too much data transferred over the wire. By default we allow up to 9000 datapoints, which is a bit more than 24 hours of data with a 10 second resolution. The number of datapoints can be reduced by setting a limit, increasing the group by time or setting a smaller date range.


**Prevent drop and delete queries** :
Deleting data can be a[very expensive operation](https://influxdb.com/docs/v0.8/api/query_language.html#deleting-data-or-dropping-series) . We block delete queries by default. In the same way, drop queries should only be executed by Administrators as they mean data loss. They are blocked as well.


**Prevent queries for short series names** :
The shorter the regex for the series name, the more series names get potentially matched. This is a huge performance hit for InfluxDB.


## Writing new rules


Writing a new rule to check a query is pretty straightforward.
We introduce a couple of terms to make things easier:


Term Description


duration Timespan (in seconds) for which the data should be returned.` select * from series where time > now() - 24h` specifies a duration of 24 hours (or 86400 seconds).


resolution Time interval (in seconds) between two data points. For example` group by time(1h)` would mean a resolution of 60*60 (=3600) seconds. If not specified, we assume a resolution of 10 seconds, which is the interval that many metrics collectors like collectd or telegraf are using to send data.


datapoints The number of datapoints returned from a query. The minimum of duration/resolution and limit.


` select * from series limit 10` would return at most 10 datapoints.


` select * from series where time > now() - 24h group by time(1h)` would return 24 datapoints (24h/1h).


If no duration or limit is given in the query, we expect that all datapoints since the beginning should be returned. You can set this "beginning date" (also called epoch) in the config.


All of these properties are parsed automatically for every query. You can access them in your rules. Here is an example rule that blocks negative resolutions, such as` group by time(-1s)` :


```text
class RuleChecker(Rule):
"""
Negative group by statements lead to undefined behavior.
They can even bring down the server. That's why they are forbidden.
"""


@staticmethod
def description():
return "Prevent negative group by statements"


def check(self, query):
"""
:param query: The query object to check
"""
if query.get_type() not in {Keyword.SELECT}:
# Bailing out for non select queries
return Ok(True)


if query.get_resolution() > 0:
return Ok(True)


return Err("Group by statements need a positive time value")
```


If the query is forbidden we return a HTTP 400 error code. This is also[how InfluxDB itself does it](https://github.com/influxdb/influxdb/issues/2560) .


## Support for other Time Series Databases


The terms above are also valid for other Time Series databases. As an example, here is how to[specify time ranges in Prometheus](http://prometheus.io/docs/querying/basics/#range-vector-selectors) :


```text
user_logins_total{page="homepage"}[5m]
```


This would get all user logins from the homepage during the last five minutes. Hence, the duration would then be 300 seconds (5 minutes).


Druid also uses similar terms but instead of resolution, they use the term[granularity](http://druid.io/docs/0.8.2/querying/granularities.html) .


## Why build a stand-alone tool?


In the end, shouldn’t such a security mechanism be part of InfluxDB itself? Yes, absolutely!


Still we might want to extend the tool to other time series databases as we see fit, such as Graphite, Druid and Prometheus. This way we are less dependent on a specific database and can switch to a different one if we need to.


Also, writing additional rules is pretty straightforward. This way we can react on new threats as we go along.


Lastly we are able to adjust our protection rules without restarting InfluxDB. That’s a nice extra if you ingest a lot of data at all times.


## Future work


We hope for community support to come up with new rules and help us fixing bugs.
If you want to have a look,[check out the code on Github](https://github.com/trivago/Protector) .
