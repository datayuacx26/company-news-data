---
schema_version: "1.0.0"
document_id: "bbc20053aa62df8cffc85afb57a82594eeba791f231bc2bb81f7baddf0cc1441"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/5-new-processing-engine-plugins/"
published_at: "2026-07-23T08:00:00+00:00"
first_seen_at: "2026-07-23T09:06:31.967054+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:238375dde7aed2969fa600d23c05ba2500235fea8246a829d13c1e83cb73f171"
---

# What’s New in InfluxDB 3: 5 New Processing Engine Plugins

Summary


The five most recent plugins from the InfluxDB team are live: Sagemaker, value counter, Chronos forecasting, simple data replicator, and a stock portfolio tracker.


Table of Contents


The InfluxDB team has released five new Processing Engine plugins. They range from making it easy to call a hosted ML model to pulling in stock market data in real-time. Every one of them can be activated with a few terminal commands. No external services or tools—they all run inside your existing InfluxDB instance.


Here’s what’s new, when you’d reach for them, and a few quickstart examples to get you going in minutes.


## Processing Engine primer


If you aren’t familiar, the[Processing Engine](https://docs.influxdata.com/influxdb3/enterprise/plugins/?utm_source=website&utm_medium=direct&utm_campaign=5_new_processing_engine_plugins&utm_content=blog) is an embedded Python runtime inside InfluxDB 3 that allows you to execute Python code on the following triggers:


- **Schedule** - Run code at scheduled intervals
- **Data write** - Runs whenever data is written to a table
- **Request** - A custom HTTP endpoint that handles incoming requests


Plugins can query InfluxDB, transform your data, and write it back to a new table. They can also be used to hit external services outside InfluxDB.


## SageMaker inference


The SageMaker plugin pulls rows of data, formats them how your SageMaker model expects, calls the model endpoint, and writes the prediction back to InfluxDB. It can build CSVs or 8 different JSON schema options, so it supports TensorFlow Serving, AWS’ built-in algorithms, and Hugging Face models.


This plugin allows you to use your deployed SageMaker models without having to create a custom plugin. Just point the plugin at your endpoint to get anomaly scores, classifications, forecasts, or whatever else your endpoint returns, and have it written directly to InfluxDB.


#### Example: Anomaly Scoring with Built-In AWS Model


```text
influxdb3 install package boto3
influxdb3 install package pandas


influxdb3 create trigger \
--database iot \
--path "sagemaker.py" \
--trigger-spec "every:30s" \
--trigger-arguments 'endpoint_name=rcf-anomaly,source_measurement=metrics,feature_order={cpu}|{mem}|{rps}|{p99_ms},json_shape=instances_features,output_fields=anomaly_score=scores[*].score,interval=2min,limit=20' \
rcf_score
```


With this setup, every 30 seconds the last 20 rows of metrics will get batched into a request to your Random Cut Forest endpoint. Each score comes back as a new row in the specified output table.


## Value counter


This plugin is a port of the Telegraf value counter aggregator plugin. Point it at a field, and it will count how many times each unique value shows up in your data. It can be used with a data write trigger that counts as rows arrive or a scheduled trigger that aggregates the time window since it last ran. Common use cases for this plugin are HTTP status codes or log-level counts.


## Chronos forecasting


The Chronos forecasting plugin allows you to use Amazon’s Chronos time series foundation models directly against your InfluxDB data. Chronos models don’t require a training step; you point the plugin at a measurement, and it returns a median forecast and prediction interval between 50% and 80%. This plugin works as a scheduled trigger for recurring forecasts and as an HTTP trigger for on-demand forecasting.


The benefit of using Chronos models is that you don’t need to mess around with training data or tuning per series. If you want to start getting forecasts on your data as soon as possible, this is the fastest path.


#### Example: On-Demand Forecast via HTTP


```text
influxdb3 install package chronos-forecasting
influxdb3 install package torch


influxdb3 create trigger \
--database mydb \
--path chronos_forecasting.py \
--trigger-spec "request:forecast_series" \
chronos_forecast_http


influxdb3 enable trigger --database mydb chronos_forecast_http


curl -X POST "http://localhost:8181/api/v3/engine/forecast_series" \
-H "Content-Type: application/json" \
-d '{"table": "sensor_data", "field": "temperature", "horizon": 28, "context_limit": 128}'
```


This will return your historical context and a 28-step forecast, with each point having a median value and 50%-80% confidence bounds.


## Simple data replicator


This plugin replicates data from your local InfluxDB 3 instance to a remote instance over HTTP, with built-in table/field filtering and renaming. It can be run on a schedule or on every data write. Data is buffered in a queue with automatic retries to support unreliable network connectivity.


The data replicator makes it easier to set up edge-to-cloud deployments, sync dev and staging environments, or deploy InfluxDB in multiple regions.


##### Example: Replicate Data on Every Write


```text
influxdb3 install package influxdb3-python


influxdb3 create trigger \
--database mydb \
--trigger-spec "all_tables" \
--plugin-filename gh:influxdata/simple_data_replicator/simple_data_replicator.py \
--trigger-arguments host=example.com,remote_token=apiv3_token,database=remote_db,tables="home home2",unique_file_suffix=wxyz5678 \
simple_data_replicator_trigger


influxdb3 enable trigger --database mydb simple_data_replicator_trigger
```


## Stock portfolio tracker


With the stock portfolio tracker plugin, you can fetch live stock prices from Yahoo Finance. Define the tickers you want to track in a TOML configuration file and monitor values over time. This plugin is mostly an example of how versatile the Processing Engine is, showing how you can make external API calls, aggregate data on a schedule, and roll up your data.


## Mix and match your plugins


These five plugins are just some of the most recent[created by the InfluxDB team](https://docs.influxdata.com/influxdb3/enterprise/plugins/library/official/?utm_source=website&utm_medium=direct&utm_campaign=5_new_processing_engine_plugins&utm_content=blog) . The official plugin library already has plugins for data transformation, downsampling, anomaly detection, alerting, and more. The biggest benefit of the Processing Engine is the ability to combine these out-of-the box tools. Here is some inspiration:


- **SageMaker Inference +[Notifier](https://docs.influxdata.com/influxdb3/enterprise/plugins/library/official/notifier/?utm_source=website&utm_medium=direct&utm_campaign=5_new_processing_engine_plugins&utm_content=blog)** - SageMaker writes anomaly scores to InfluxDB as time series data. You can then use the Notifier plugin to send alerts to Slack, email, or a webhook if a threshold is crossed without needing a separate alert stack.
- **Value Counter +[State Change](https://docs.influxdata.com/influxdb3/enterprise/plugins/library/official/state-change/?utm_source=website&utm_medium=direct&utm_campaign=5_new_processing_engine_plugins&utm_content=blog)** - If you are tracking the status of something, you can use the value counter to tell you how many times a value has appeared and combine it with the state change to tell you when it changed. Together you can track frequency counts and status-change events for alerting, giving you a complementary view of the same data.
- **Simple data replicator + Downsampler** - Downsampling your data locally before sending it to your remote InfluxDB instance will help to reduce bandwidth usage.
- **Simple data replicator + InfluxDB to Iceberg** - This combination allows you to move hot data from InfluxDB to another instance for redundancy or regional access, and then export cold data to Iceberg for long-term storage.


All five are live now in the[influxdb3_plugins repository](https://github.com/influxdata/influxdb3_plugins/tree/main/influxdata/?utm_source=website&utm_medium=direct&utm_campaign=5_new_processing_engine_plugins&utm_content=blog) . Grab one, wire up a trigger, and see what you can build with the InfluxDB 3 Processing Engine.


## Related resources


- [InfluxDB 3 download](https://www.influxdata.com/influxdb-signup/?utm_source=website&utm_medium=direct&utm_campaign=5_new_processing_engine_plugins&utm_content=blog)
- [Official plugins Github repo](https://github.com/influxdata/influxdb3_plugins/tree/main/influxdata/?utm_source=website&utm_medium=direct&utm_campaign=5_new_processing_engine_plugins&utm_content=blog)
- [Processing Engine docs](https://docs.influxdata.com/influxdb3/enterprise/plugins/?utm_source=website&utm_medium=direct&utm_campaign=5_new_processing_engine_plugins&utm_content=blog)
- [InfluxDB 3 Explorer](https://docs.influxdata.com/influxdb3/explorer/?utm_source=website&utm_medium=direct&utm_campaign=5_new_processing_engine_plugins&utm_content=blog)


## FAQs


##### **Q: Do I need to write Python to use these plugins?**


No, each plugin is ready to run after you install its dependencies with` influxdb3 install package` , then wire it up with` influxdb3 create trigger` . You only need to touch Python if you want to modify the plugin’s behavior or create a custom plugin.


##### Q: Do I need to write Python to use these plugins?


##### Q: Can I configure a plugin with a file instead of inline commands?


Yes, every plugin supports using a TOML configuration file via the` config_file_path` argument. Using a configuration file is recommended for setups that require more than a few parameters.


##### Q: Where do I see plugin errors or logs?


Every plugin writes to the` system.processing_engine_logs` table in the trigger’s database.


##### Q: Do these plugins work with all versions of InfluxDB 3?


All five run on both InfluxDB 3 Core and InfluxDB 3 Enterprise; you just need the Processing Engine enabled (` --plugin-dir /path/to/plugins` when you start the server).
