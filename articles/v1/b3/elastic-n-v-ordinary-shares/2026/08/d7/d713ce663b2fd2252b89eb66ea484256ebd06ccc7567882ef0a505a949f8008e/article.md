---
schema_version: "1.0.0"
document_id: "d713ce663b2fd2252b89eb66ea484256ebd06ccc7567882ef0a505a949f8008e"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/otel-metrics-cumulative-delta-elasticsearch"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-14T20:14:06.247167+00:00"
fetched_at: "2026-08-14T20:14:08.046658+00:00"
content_hash: "sha256:b31153c8cb5a75f083d8896daf622054032fe69f255368b395c440db93ecb03f"
---

# Skip the stateful OTel Collector: Elasticsearch 9.5 natively stores both metric temporalities

Elasticsearch 9.5 natively stores both cumulative and delta OpenTelemetry (OTel) counters and histograms, even when mixed for the same metric name. You ingest via OpenTelemetry Protocol (OTLP) and Elasticsearch preserves the temporality metadata automatically.[ES|QL TS](https://www.elastic.co/docs/reference/query-languages/esql/commands/ts) and[PromQL](https://www.elastic.co/docs/reference/query-languages/promql) queries detect the temporality per series and interpret the data correctly, without new syntax, configuration changes to your OTel SDKs or stateful OTel Collector conversion. Existing queries and downsampled data continue to work as expected.


## What is metric temporality in OpenTelemetry?


Metrics stores usually receive client-side, pre-aggregated metrics. For example, if an application records request response times, it won’t send each individual response time as a single data point to your metrics back end. Instead, the application (or rather the OTel SDK) pre-aggregates those raw response times into counters or histograms. These pre-aggregated values are then exported at a periodic interval, dramatically reducing the number of data points. *Temporality* is about how this pre-aggregation works. There are two temporality models: *cumulative* and *delta* .


### Cumulative temporality in OTel metrics


With *cumulative temporality* , each data point represents the total **** amount of change in the metric value since the process started. Values monotonically increase, with occasional reset to 0 (for example, when the process restarts).


Take a counter tracking the total CPU time consumed by a Java Virtual Machine (JVM):


**Timestamp**


**Value**


**Meaning**


10:01


12.4s


12.4s total CPU time since start


10:02


13.1s


13.1s total CPU time since start


10:03


13.9s


13.9s total CPU time since start


To compute the rate of change between 10:01 and 10:02, we subtract:` 13.1 - 12.4 = 0.7s` of CPU time was consumed in that interval. Dividing by the time range of the interval gives us the` rate` . This is the default temporality for counters in both Prometheus and OTel.


### Delta temporality in OTel metrics


With *delta temporality* , each data point represents the change since the last measurement. Values are independent of each other. In other words, after each export, the OTel SDK resets all values for all series.


The same raw observations from the cumulative example above would look as follows with delta temporality.


**Timestamp**


**Value**


**Meaning**


10:01


0.5s


0.5s of CPU time in this interval


10:02


0.7s


0.7s of CPU time in this interval


10:03


0.8s


0.8s of CPU time in this interval


To compute the rate or increase, we can use the value directly, without any subtraction.


### Trade-offs between cumulative and delta OpenTelemetry metrics


Both temporalities have practical trade-offs:


-


**Resilience to data loss:** Cumulative counters are self-describing: If you miss an export, the next data point still gives you the correct total. Delta values are incremental, so a lost data point means that the corresponding increase is lost.


-


**Metric producer memory footprint:** For cumulative temporality, the OTel SDKs need to keep a state for every series in memory. For delta temporality, the footprint is much lower. There, the SDKs only need to keep track of counters or histograms which changed since the last export. If there are a lot of counters or histograms and many of them don’t increase each period, this difference can be quite substantial.


-


**Aggregation across restarts:** Cumulative counters require reset detection logic, which in edge cases can fail: If the metric value decreases, it’s detected as a reset. We assume that the application was restarted and the counter started from 0 again. This can be missed if the first reported counter value after the restart is higher than before the restart. A concrete example:


Delta values don't have this problem since each value is independent.


If you’re using histograms, the trade-offs have an even bigger effect:


**Trade-off**


**Cumulative**


**Delta**


Histogram size


Buckets accumulate across exports, consuming more storage


Buckets reset each export, producing smaller histograms


Min/max accuracy


Approximated from buckets for custom time ranges (tracked values represent extremes since process start)


Exact per-export minimum and maximum values


Query performance


Faster: only the first and last value in a time range plus resets are needed


Slower: all histograms in the queried range must be combined


OpenTelemetry supports both models and lets you choose per SDK via the` OTEL_EXPORTER_OTLP_METRICS_TEMPORALITY_PREFERENCE` environment variable.


## Why native temporality support eliminates OTel Collector workarounds


[Prometheus](https://prometheus.io/docs/concepts/metric_types/#counter) and most other metrics back ends pick a side: All metrics have to be either cumulative or delta. Elasticsearch previously followed that pattern, too, with native storage of cumulative counters and delta histograms, and workarounds for everything else. Delta counters were stored as gauges, functional but without native counter semantics for rate queries. And cumulative histograms were unsupported.


One workaround for unsupported temporalities is to configure your metric producers (for example,[OTel SDKs](https://opentelemetry.io/docs/languages/) ) to produce data with the temporality that your back end supports. In large-scale deployments, this can be a very challenging task. And sometimes this isn’t even possible (for example, if you consume OTLP metrics from third-party services).


Another workaround is to convert the temporality prior to ingestion. In the OTel Collector, you would typically use the[cumulative-to-delta processor](https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/processor/cumulativetodeltaprocessor/README.md) , which comes with a big warning sign about *statefulness* . The conversion is inherently stateful, requiring ordered delivery of metric series to the same collector and persisted state across restarts. In practice, it works, but at scale, it comes with a lot of deployment headaches.


With Elasticsearch 9.5, you can skip the conversion pipeline entirely. Elasticsearch natively stores and queries metric data with both temporalities. It doesn’t require any stateful conversion required or explicit configuration of your OTel SDKs.


## Demo: ingesting cumulative and delta OTel metrics side by side


To demonstrate the temporality support, we'll reuse a demo setup from our[OTel histogram metrics ES|QL blog post](https://www.elastic.co/search-labs/blog/otel-histogram-metrics-esql) : a Java[Renaissance](https://github.com/renaissance-benchmarks/renaissance) benchmark instrumented with the[OTel Java agent](https://opentelemetry.io/docs/zero-code/java/agent/) . The twist this time: We run two instances of the benchmark, each configured with a different temporality:


-


****` **renaissance-delta**` **:** Exports metrics with delta temporality.


-


` **renaissance-cumulative**` **:** Exports metrics with cumulative temporality.


Both instances report the same metrics under the same service name` renaissance` , but with different` service.instance.id` values. Here’s the relevant section of the[docker-compose.yml](https://github.com/elastic/elasticsearch-labs/blob/main/supporting-blog-content/elasticsearch-temporality-demo/docker-compose.yml) that can be found in[the companion code](https://github.com/elastic/elasticsearch-labs/tree/main/supporting-blog-content/elasticsearch-temporality-demo) :


```text
renaissance-delta:
environment:
OTEL_SERVICE_NAME: renaissance
OTEL_RESOURCE_ATTRIBUTES: "service.instance.id=delta-instance"
OTEL_EXPORTER_OTLP_METRICS_TEMPORALITY_PREFERENCE: delta
OTEL_EXPORTER_OTLP_METRICS_DEFAULT_HISTOGRAM_AGGREGATION: BASE2_EXPONENTIAL_BUCKET_HISTOGRAM


renaissance-cumulative:
environment:
OTEL_SERVICE_NAME: renaissance
OTEL_RESOURCE_ATTRIBUTES: "service.instance.id=cumulative-instance"
OTEL_EXPORTER_OTLP_METRICS_TEMPORALITY_PREFERENCE: cumulative
```


To run the demo yourself, you'll also have to fill out the[managed OTLP endpoint URL](https://www.elastic.co/docs/reference/opentelemetry/managed-inputs/managed-otlp-endpoint) and the corresponding API key:


```text
OTEL_EXPORTER_OTLP_ENDPOINT: https://<cluster-endpoint>
OTEL_EXPORTER_OTLP_HEADERS: "Authorization=ApiKey <base64 api key>"
```


After starting the demo with` docker compose up --build` , both instances will start reporting metrics to Elasticsearch.


### Querying OTel counter metrics with ES|QL and PromQL


Let's query the first few raw data points of` jvm.cpu.time` for both instances to see the different temporalities in action:


```text
After starting the demo with `docker compose up --build`, both instances will start reporting metrics to Elasticsearch.
Querying OTel counter metrics with ES|QL and PromQL
Let's query the first few raw data points of `jvm.cpu.time` for both instances to see the different temporalities in action:
```


This gives us the first five data points for each service instance:


The benchmark consumes CPU at a nearly constant rate. This is directly visible based on the delta temporality data: The values are nearly constant between exports. In contrast, the cumulative temporality values grow over time, as they represent the total CPU usage of the benchmark instance.


Now let's have a look at how to properly query this metric using PromQL:


```text
PROMQL sum by (service.instance.id) (rate(jvm.cpu.time))
```


The screenshot shows that both benchmark instances consume a nearly constant of 1 to 1.2 number of CPU cores with some variance. This query works because we made our` rate` implementation respect the temporality: Every time series (so every service instance in our case) stores the temporality as a metric dimension. The` rate` implementation looks at this dimension and interprets the data accordingly: For delta temporality, values are summed up; for cumulative temporality, a difference computation is done. This all happens automatically in the background, without requiring any changes to your queries.


We’ve adapted` rate` ,[increase](https://www.elastic.co/docs/reference/query-languages/esql/functions-operators/time-series-aggregation-functions#increase) , and[irate](https://www.elastic.co/docs/reference/query-languages/esql/functions-operators/time-series-aggregation-functions#irate) to work this way. The same applies when using those functions in ES|QL TS queries:


```text
TS metrics-*
| STATS SUM(RATE(jvm.cpu.time)) BY TBUCKET(100), service.instance.id
```


Because Elasticsearch tracks the temporality as a dimension, you can have multiple series with different temporalities for the same metric, just like in the demo use case. Aggregating across series also works as expected, because at that point` rate` ,` increase` , or` irate` already took care of normalizing the data:


```text
PROMQL sum(rate(jvm.cpu.time))
```


*PromQL query showing total CPU time aggregated across both instances*


### Querying OTel histogram metrics across temporalities


Metric temporality applies to histograms in the same way it applies to counters: histogram buckets are effectively a set of counters, each tracking values in a specific range.As in our histogram demo, we use exponential histograms, where bucket boundaries adapt automatically to minimize relative error.


Due to this similarity, histograms can also be cumulative or delta. Either the counter per bucket is reset after each metric export or the cumulative count carries over between exports.


Let's query the median major garbage collection (GC) duration for our benchmark instances, which is a histogram metric:


```text
PROMQL histogram_quantile(0.5,  sum by (service.instance.id) (increase(jvm.gc.duration{jvm.gc.action=~".*major.*"})))
```


Or the equivalent ES|QL query:


```text
TS metrics-*
| WHERE jvm.gc.action LIKE "*major*"
| STATS MEDIAN(jvm.gc.duration) BY TBUCKET(100), service.instance.id
```


Again, both queries will automatically load the temporality per series and interpret the histograms accordingly. In PromQL, this is handled by the` increase` function. Note that in ES|QL, you don't explicitly call` increase` on histograms. The` TS` command automatically handles the temporality-aware merging of histograms when you use aggregation functions, like` PERCENTILE` ,` MEDIAN` , or` AVG` .


## How Elasticsearch stores metric temporality in TSDB


Elasticsearch's time series database (TSDB) stores metric temporality in a dedicated dimension field on each document. The[index.time_series.temporality_field](https://www.elastic.co/docs/reference/elasticsearch/index-settings/time-series#index-time-series-temporality-field) index setting lets you specify which field carries the temporality information. The field must be a` keyword` field with` time_series_dimension: true` and the permissible values` "delta"` or` "cumulative"` .


As soon as this setting is present on a time series index, ES|QL and PromQL will load the corresponding field when performing temporality-dependent aggregations. If the field isn’t present or has no value on a document, we fall back to defaults based on the type of the corresponding metric: counters default to cumulative, and histograms default to delta. This matches the historical behavior and ensures existing queries and existing data continue to work without changes.


When you ingest metrics via the OTLP endpoint, Elasticsearch automatically adds a` temporality` dimension field to each document, populated from the[OTLP AggregationTemporality](https://opentelemetry.io/docs/specs/otel/metrics/data-model/#temporality) metadata. For custom (neither OTLP nor[Prometheus remote write](https://www.elastic.co/docs/manage-data/data-store/data-streams/tsds-ingest-prometheus-remote-write) ) ingestion, you’ll have to manually set up the` index.time_series.temporality_field` setting and populate your temporality dimension.


The temporality is also respected during downsampling: As it’s a dimension, it’s preserved automatically and used to compute the aggregate values.


## Getting started with mixed-temporality OTel metrics in Elasticsearch


With Elasticsearch 9.5, cumulative versus delta is no longer a decision you have to get correct at the start. Ingest both temporalities side by side, even for the same metric name, and let ES|QL and PromQL handle the rest. You can switch between both without having to touch your queries. For more details, see the[metric temporality documentation](https://www.elastic.co/docs/manage-data/data-store/data-streams/metric-temporality) .
