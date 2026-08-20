---
schema_version: "1.0.0"
document_id: "ccb8bdc3db9c9dc701193acdd7ddb8b01f7d370799b779313466546c5eadcace"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/create-histogram-metrics-at-ingest-with-openpipeline/"
published_at: "2026-08-14T10:21:17+00:00"
first_seen_at: "2026-08-14T11:50:02.253577+00:00"
fetched_at: "2026-08-14T11:50:03.747054+00:00"
content_hash: "sha256:f3b36cc2825ad7eeef162130b345343a3ec2ce1bc500594483ea75f857c0df89"
---

# Create histogram metrics at ingest with OpenPipeline

**Histogram metrics preserve the full distribution of your observability data, enabling accurate percentile analysis and service-level objective (SLO) measurement. Dynatrace now extracts histogram metrics from spans and logs during ingestion with[OpenPipeline](https://www.dynatrace.com/platform/openpipeline/) , so you can capture latency distributions and other high-value metrics without re-instrumenting your services. This post explains how histogram metrics work, why they matter, and how they help you measure performance beyond averages, minimums, and maximums.**


Picture a service that reports a 200 ms average response time. On the surface, it looks healthy. But if 5% of requests take more than 3 seconds, that average conceals the experience of your most affected users. As organizations adopt SLO-driven reliability practices, they need answers that summary statistics can’t provide: What percentage of requests complete within 500 ms? Where does p99 latency actually fall? Those questions require distribution data, not a single aggregated value.


## Why do averages hide important performance signals?


Single-value metrics simplify complex behavior into a single number, often obscuring the insights that matter most. Imagine a service where 80% of requests complete in 10 ms while the remaining 20% take 2 seconds. The average response time is 408 ms, but that number accurately represents neither group. Once you’ve stored only the average, the underlying distribution is lost—and with it, the ability to analyze percentiles, tail latency, or SLO compliance.


Histogram metrics preserve the distribution at write time:


- **Distribution preservation:** Histogram metrics record observations into buckets, keeping the full shape of your data intact. You see the fast cluster, the slow tail, and everything between.
- **Server-side percentile computation:** Pre-calculated summaries compute quantiles at the source and can’t be merged. Histograms let you calculate p50, p90, p95, or p99 server-side, across all instances.
- **Aggregability:** Exponential histograms aggregate cleanly even when there are resolution differences between sources. Summaries don’t aggregate cleanly.


The[OpenTelemetry](https://www.dynatrace.com/platform/open-standards/) specification defines exponential histograms as a first-class metric type, and the industry is converging on this representation. Dynatrace is adopting this format as a native metric type, and it is consistent with Prometheus open-source histograms so your OpenTelemetry instrumentation works without translation or loss of fidelity.


Figure 1: Load-action response time for the first action of each user visit, tracked as median (p50), 95th, and 99th percentiles across the day.


## How to extract histogram metrics from spans and logs at ingest


Computing a percentile used to mean scanning raw span or log data on every query. A request like this one ran against the full dataset each time:


` fetch spans`
` | makeTimeseries percentile(duration, 95), by:{endpoint.name}`


That works, but it reprocesses raw telemetry on every execution. With OpenPipeline, you configure a histogram metric extraction rule that converts span or log data into exponential histogram metrics at ingest. That same percentile analysis then becomes a straightforward metric query:


` timeseries percentile(span.duration, 95), by:{endpoint.name}`


The key detail is that this runs on data you’re already ingesting. You don’t re-instrument your applications, change your SDKs, or modify your OpenTelemetry collectors. OpenPipeline processes spans and logs flowing into Dynatrace and can automatically produce histogram metrics. The resulting metrics are stored in[Grail](https://www.dynatrace.com/platform/grail/) , the Dynatrace data lakehouse, where they’re available for Dynatrace Query Language (DQL) queries,[dashboards](https://www.dynatrace.com/hub/detail/dashboards/) , SLO definitions, and[anomaly detection](https://www.dynatrace.com/platform/artificial-intelligence/anomaly-detection/) under the same retention and access controls as any other metric.


The extracted metrics use the exponential histogram format, aligned with OpenTelemetry conventions. Bucket boundaries adapt dynamically to the data range, so there’s no upfront configuration of bucket sizes. That’s a real advantage over classic histogram approaches, which require you to guess the right boundaries before you’ve seen the data.


Figure 2: The OpenPipeline histogram metric extraction configuration. Define a metric key, select the source field, and OpenPipeline creates exponential histogram metrics.


For the full configuration walkthrough, see the[OpenPipeline documentation](https://docs.dynatrace.com/docs/platform/openpipeline/concepts/processing) .


## How do histogram metrics help with SLOs and reliability engineering?


Histogram metrics are more than a storage improvement. They support reliability use cases that single-value metrics can’t:


- **Accurate percentile monitoring:** Query p50, p75, p90, p95, or p99 directly from pre-aggregated histogram metrics, with no raw data scan and consistent results across all reporting instances.
- **Precise SLO measurement:** Answer questions like “What fraction of requests completed under 500 ms?” with real data. The new[percentRank()](https://docs.dynatrace.com/docs/platform/grail/dynatrace-query-language/commands/metric-commands) aggregation function also works in the other direction: given a threshold, it returns the estimated fraction of observations below that value. That makes histogram metrics a natural fit for SLOs based on latency distributions rather than averages.
- **Distribution visibility over time:** Spot bimodal response patterns, shifting baselines, and outlier clusters that averages would mask.
- **Smarter anomaly detection:**[Dynatrace Intelligence](https://www.dynatrace.com/platform/artificial-intelligence/) uses histogram data for rich baselining. Instead of learning a single expected value, it learns the expected distribution, so it detects a new latency mode or a widening tail that a threshold on an average would miss.


Whether you’re tracking API latency against an SLO, monitoring inference times for a machine learning model, or validating that a deployment didn’t introduce a new latency mode, histogram metrics give you the data fidelity you need for SLO-driven reliability.


## Get started with histogram metrics


Histogram metrics preserve the distribution your SLOs depend on. OpenPipeline makes them available from the spans and logs you’re already collecting with no re-instrumentation, and Grail, Dashboards, and Dynatrace Intelligence put them to work across your reliability workflows.


To configure your first histogram metric extraction rule, read the[histogram metrics documentation](https://docs.dynatrace.com/docs/shortlink/openpipeline-processing) . To explore the query experience first, try histogram metric queries on the[Dynatrace Playground](https://www.dynatrace.com/signup/playground/) . And if you have questions or want to share your use cases, join the conversation on the[Dynatrace Community](https://community.dynatrace.com/) .
