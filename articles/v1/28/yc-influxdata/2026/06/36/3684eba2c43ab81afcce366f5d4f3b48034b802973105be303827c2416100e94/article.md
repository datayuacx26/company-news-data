---
schema_version: "1.0.0"
document_id: "3684eba2c43ab81afcce366f5d4f3b48034b802973105be303827c2416100e94"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/river-plugins-influxdb-3/"
published_at: "2026-06-04T08:00:00+00:00"
first_seen_at: "2026-07-20T23:23:58.693982+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:c18281e1e25d017154b92cc1fdca95e08e3b63cf4e619eacac717ea1aef04dd4"
---

# Anomaly Detection and Forecasting That Learns From Every Write in InfluxDB

Table of Contents


For many operational time series workloads, machine learning can’t operate in the historical way, where data is compiled once and models are trained offline. Sensor readings, infrastructure metrics, application telemetry, energy data, industrial measurements, and financial ticks all share a basic property: the next datapoint is more useful when the system can respond to it immediately (or at least close to immediately). When a model learns in the same flow that ingests data and reacts to incoming data as it’s written, things like anomaly detection, short-horizon forecasting, and adaptive thresholding all become a lot more useful.


Enter three new[River](https://riverml.xyz/dev/) -based plugins for InfluxDB 3:


- [River Anomaly Detector](https://github.com/influxdata/influxdb3_plugins/tree/main/influxdata/river_anomaly_detector/?utm_source=website&utm_medium=direct&utm_campaign=river_plugins_influxdb_3&utm_content=blog)
- [River Auto-Profiler](https://github.com/influxdata/influxdb3_plugins/tree/main/influxdata/river_auto_profiler/?utm_source=website&utm_medium=direct&utm_campaign=river_plugins_influxdb_3&utm_content=blog)
- [River Forecaster](https://github.com/influxdata/influxdb3_plugins/tree/main/influxdata/river_forecaster/?utm_source=website&utm_medium=direct&utm_campaign=river_plugins_influxdb_3&utm_content=blog)


These plugins are built for the InfluxDB 3 Processing Engine and leverage River, a Python library for online machine learning. If you’re unfamiliar with it, the Processing Engine is an embedded Python VM that runs inside InfluxDB 3 and can execute plugin code on writes, schedules, or HTTP requests; it also provides an in-memory cache for stateful applications. For write-triggered plugins, InfluxDB 3 can send batches of data to a plugin as data is flushed through the write-ahead log. River, meanwhile, is designed for models that learn from streaming data incrementally, including anomaly detection, drift detection, and time series forecasting.


That combination makes InfluxDB 3 and River a perfect match. These plugins bring small, per-series, constantly-updating models directly into the write path, then write the resulting profiles, anomalies, and forecasts back into InfluxDB tables where they can be queried like any other time series data or combined with other triggers and plugins to kick off informed actions. Better yet, they do this all within InfluxDB, eliminating the need for extra infrastructure, servers, and data pipelines to fuel your ML models. In this blog, we’re going to talk about all three new River plugins for InfluxDB, and how they can not only simplify your ML stack, but lead to faster insights through online modeling.


## River Anomaly Detector: multiple ways to detect problematic data


Let’s start with the River Anomaly Detector. At a high level, the plugin monitors numeric fields and writes anomaly rows to the table` _anomalies.{source_table}` . It supports rolling Z-score detection, seasonal detection, and[adaptive window (ADWIN)](https://riverml.xyz/dev/api/drift/ADWIN/) -based drift detection. Each unique combination of table, tags, and field has its own detector state, and models are updated incrementally as new observations arrive.


The rolling detector tracks an exponentially weighted mean and variance, then flags values that exceed the learned range by a configurable number of standard deviations. The seasonal detector maintains separate time-based buckets, either 24 hour-of-day buckets or 168 hour-of-week buckets, and it compares new values against the bucket that matches the current timestamp. The ADWIN detector watches for changes in the statistical properties of the stream, which is useful when the issue is a behavior shift rather than a single spike.


Detectors learn from every observation, and you can choose to enable one, two, or all three detector modes, but only the detectors active in the current mode can vote on whether a point is anomalous. This allows you to train the various forms of anomaly detection, then modify your trigger once enough data has been provided to the model. This way, a stream can accumulate enough seasonal history before the seasonal detector is allowed to participate in decisions, and ADWIN can keep learning even when the current mode is using a simpler rolling Z-score path. You don’t even need to specify which detector to use; run the plugin with a minimal trigger such as the following:


```text
influxdb3 install package river


influxdb3 create trigger \
--database mydb \
--plugin-filename gh:influxdata/river_anomaly_detector/river_anomaly_detector.py \
--trigger-spec "all_tables" \
anomaly_detector
```


With that trigger in place, the plugin starts monitoring numeric fields in all incoming tables. The default behavior is to “auto-tune” and read recommendations from the River Auto Profiling plugin, which we’ll dive into more in the next section. A typical query against the output might look like this:


```text
SELECT
time,
host,
field_name,
original_value,
detector_mode,
rolling_mean,
rolling_std,
rolling_deviation,
rolling_threshold,
seasonal_bucket,
drift_detected
FROM "_anomalies.cpu"
ORDER BY time DESC
LIMIT 20;
```


That output is deliberately detailed: you get the original value, the active detector mode, rolling statistics, seasonal statistics when available, and ADWIN drift information. The plugin only writes rows where a datapoint is determined to be an anomaly, so the anomaly table is an event stream rather than a full copy of the source data.


For a stable metric, the detector may run with a lower Z-score threshold because small deviations are meaningful. For a noisy metric, it may use a higher threshold to avoid producing noise. For a metric with a strong weekly pattern, seasonal buckets can separate “expected at 2 p.m. on Monday” from “expected in general.” For a metric that is drifting, ADWIN can participate so the detector can respond to changes in the stream rather than treating every shift as a one-off outlier.


You can also make the mode explicit. For example, if you only care about a specific table and you know it has strong daily or weekly seasonality on a couple fields you want to monitor, you can specify this and force the plugin to only use seasonal detection:


```text
influxdb3 create trigger \
--database mydb \
--trigger-spec "table:cpu" \
--trigger-arguments \
'include_fields=temperature humidity' \
'rolling_std_threshold=3.0' \
'enable_seasonal=true' \
anomaly_detector_cpu
```


The detector can encode understanding in the database itself, close to the data, while still leaving room for explicit configuration when you know exactly what you want. Whether you want adaptive detection, rolling Z-scores, seasonal detection, or some combination of the three, the River Anomaly Detector plugin can make it happen, and it’s truly as simple as defining the tables and fields you want it to operate on.


## River Auto-Profiler: the control plane for per-series tuning


The Auto-Profiler is the least flashy of the three plugins, but it is the one that makes the anomaly detector more practical at scale.


A static threshold is easy to understand and easy to deploy. It is also usually wrong somewhere. A metric that barely moves, a metric with high variance, a metric with a weekly pattern, and a metric undergoing a slow drift should not all use the same anomaly detection settings. You can tune each field by hand, but that does not scale well when your schema grows or when the behavior of a series changes over time.


The Auto-Profiler addresses that by incrementally profiling each numeric series and writing recommendations to` _meta.series_profiles` . It tracks exponentially weighted mean and variance, skewness,[kurtosis](https://en.wikipedia.org/wiki/Kurtosis) , write interval, and seasonal variance buckets. After a short calibration phase, it writes profile snapshots that include a pattern label, recommended detector mode, threshold, fading factors, seasonality strength, trend strength, and maturity flags.


Creating the trigger is intentionally simple:


```text
influxdb3 create trigger \
--database mydb \
--plugin-filename gh:influxdata/river_auto_profiler/river_auto_profiler.py \
--trigger-spec "all_tables" \
auto_profiler
```


Then you can inspect the current profile state:


```text
SELECT
time,
source_table,
host,
field_name,
observations,
pattern_label,
recommended_detector_mode,
recommended_threshold,
recommended_fading_factor,
seasonality_strength,
trend_strength,
profile_mature,
seasonality_ready
FROM "_meta.series_profiles"
ORDER BY time DESC
LIMIT 20;
```


The classifier is intentionally transparent. It labels streams as stable, noisy, trending, seasonal, or bursty and maps those labels to detector modes such as` zscore_low` ,` zscore_high` ,` zscore_adaptive` ,` zscore_conservative seasonal` , or` zscore_conservative adwin` . Seasonality detection uses hourly or weekly variance buckets, and trend detection compares fast and slow exponentially weighted means.


That transparency is useful. If a profile says a series is seasonal, you can look at the` seasonality_strength` and` seasonal_buckets_filled` fields. If it says a series is trending, you can inspect` trend_strength` . If the profile is not mature yet, downstream consumers can fall back to safer defaults.


The Auto-Profiler also tunes thresholds using observed exceedance behavior. It tracks how often observations fall outside` mean ± threshold × std` and adjusts the threshold toward an approximate target anomaly rate, with bounds to avoid unbounded sensitivity changes. That does not make anomaly detection “automatic” in the magical sense. It makes it adaptive in the operational sense: the system can use what it has learned about each series to pick a more appropriate starting point.


The most important integration is with the anomaly detector. With both plugins deployed, the profiler writes per-series recommendations, and the anomaly detector consumes those recommendations to adapt to your data, weed out anomalies, and minimize noise.


## River Forecaster: short-horizon forecasts from the write stream


The River Forecaster takes the same online-learning pattern and applies it to forecasting. The plugin uses River’s[SNARIMAX](https://riverml.xyz/dev/api/time-series/SNARIMAX/) model, an online[ARIMA](https://www.ibm.com/think/topics/arima-model) -style model, to learn from incoming values and periodically write multi-step forecasts to` _forecasts.{source_table}` . Like the anomaly detector, it keeps a separate model per table/tag/field series. Unlike the anomaly detector, it requires explicit table selection through` include_tables` , which is a necessary guardrail for forecast cardinality.


A trigger for the forecasting plugin might look like this:


```text
influxdb3 create trigger \
--database mydb \
--plugin-filename gh:influxdata/river_forecaster/river_forecaster.py \
--trigger-spec "all_tables" \
--trigger-arguments \
"include_tables=system_cpu system_memory" \
"include_fields=idle used available" \
"max_series=100" \
"default_horizon=24" \
"log_forecasts=true" \
forecaster
```


By default, the model waits for a warm-up period before producing forecasts. The forecast horizon is also derived from the stream. The plugin tracks each model’s write interval and uses it to choose how many future steps are needed to cover a target forecast window, which defaults to one hour. If a series is written every 10 seconds, the forecast horizon can be much longer than a series written every five minutes, because the time window is the same, but the step size is different.


You can query forecasts like this:


```text
SELECT
time,
host,
field_name,
horizon_step,
forecast_value,
horizon_total,
observations
FROM "_forecasts.system_cpu"
ORDER BY time DESC
LIMIT 12;
```


Each forecast row receives a future timestamp based on the last observed timestamp plus` horizon_step * write_interval_seconds` . The output includes the forecasted value, the step number, total horizon length, and the number of observations the model has learned from.


The forecaster also self-throttles per model. It produces a forecast, stores those predictions in memory, and then compares them step by step against incoming actuals. A new forecast is produced once the previous forecast horizon has been consumed, so evaluation covers the full horizon rather than a partial subset.


That makes the plugin useful for near-term operational questions. For the example above (forecasting CPU and system memory utilization), these questions might be:


- Is memory usage expected to cross a threshold in the next hour?
- Is a sensor trending toward a range that usually precedes maintenance?
- Is the current CPU-idle forecast consistent with what the anomaly detector is seeing?


These are not the only forecasting questions you might ask of time series data, but they are the kind that fit well in a write-triggered architecture.


## Using the River plugins with InfluxDB


With the simplicity of InfluxDB 3’s embedded Processing Engine, getting started with notoriously difficult or complicated ML tasks is now simpler, faster, and easier than ever. For all three of these River ML plugins, you can view the code and README files within the[InfluxDB 3 plugin repository](https://github.com/influxdata/influxdb3_plugins/?utm_source=website&utm_medium=direct&utm_campaign=river_plugins_influxdb_3&utm_content=blog) , with full documentation for configuration and recommended use. Installing them is as simple as defining and starting the triggers within the processing engine.


Make sure you’re intentional about which tables and fields you monitor. You should think about cold starts, profile maturity, and whether a given field has enough regularity for short-horizon forecasting. These questions exist whenever you’re deploying ML techniques on data—this isn’t magic.


Once you have the plugins up and running, everything should work seamlessly as described above, adapting and learning from the data written to your tables. All three plugins’ outputs remain in InfluxDB. Profiles are written to` _meta.series_profiles` . Any detected anomalies are written to` _anomalies.{source_table}` . Forecasts are written to` _forecasts.{source_table}` .


That means the operational burden is small, and you can query profiles, anomalies, forecasts, and raw data with SQL. You can visualize your River ML-generated data in dashboards alongside the raw and processed data you’ve written into InfluxDB, join different datasets together, and inspect behavior, anomalies, and forecasts without needing to involve any other services or platforms. You don’t need additional hardware or pipelines or self-written plugins to extract your time series data into an offline system for ML. It may just be the easiest way to leverage your time series data for machine learning out there. By embedding these plugins within InfluxDB, your training workflows and ML insights can now live closer to the raw data than ever before.
