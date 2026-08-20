---
schema_version: "1.0.0"
document_id: "72c393492a083cf8c279aedde7c2d3ed227bb969a407f18fc0793ff947e5b40b"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/rust-client-influxdb-3/"
published_at: "2026-08-13T08:00:00+00:00"
first_seen_at: "2026-08-13T07:50:15.173971+00:00"
fetched_at: "2026-08-13T08:00:01.087005+00:00"
content_hash: "sha256:0c8fe7616f9e30ce06fa1f80f5b88c925251eb9a495f2839c385c05ba23e52fd"
---

# A Rust Client for InfluxDB 3

Summary


A new async Rust client for InfluxDB. Built for the edge gateways, embedded systems, and high-throughput ingest pipelines where Rust already runs.


Table of Contents


Time series data shows up wherever the physical world meets software. A satellite constellation streams altitude, power, and thermal telemetry from every spacecraft on every pass. A factory floor running on Industry 4.0 principles instruments every line, every motor, every batch. And underneath all of it sits a humbler problem that anyone who has worked in operational technology knows well: getting telemetry out of the PLCs and edge controllers that actually run the machines, off the bus, and into a database that can enable real-time asset intelligence.


InfluxDB 3 is built for this class of workload. It is the latest generation of the InfluxDB time series engine, built on an open source stack: **Apache Arrow** for in-memory columnar data and **Apache DataFusion** as the query engine. In practice, that means InfluxDB 3 is a columnar, vectorized engine that speaks SQL, exchanges data over Arrow Flight, and interoperates with the broader Arrow ecosystem, rather than a closed world with its own bespoke query path. For high-cardinality telemetry (thousands of spacecraft channels, tens of thousands of sensor tags on a plant floor), that columnar foundation is what keeps both ingest and analytical queries fast.


What has been missing for Influx users is a first-class **Rust** client. We just built one:[influxdb3-client](https://crates.io/crates/influxdb3-client) , an async Rust client for InfluxDB 3 Core and Enterprise that mirrors the feature set of the official[Go](https://github.com/InfluxCommunity/influxdb3-go) and[Python](https://github.com/InfluxCommunity/influxdb3-python) clients with an idiomatic Rust API.


## Why Rust, when Go and Python clients already exist?


The honest answer is that for a lot of jobs, you shouldn’t switch. If you’re exploring data in a notebook, the Python client is the right tool. If you’re writing a typical backend service, the Go client is mature and perfectly fast. Rust earns its place in the parts of a telemetry pipeline where the other two start to fight you. Here are some scenarios where it makes sense to switch:


**The edge box next to the PLC** . The machine that bridges OPC UA or Modbus to your historian is often an ARM gateway with a few hundred MB of RAM, no package manager you control, and a change window measured in months. Rust cross-compiles to a single static binary, with no Python interpreter to install and patch on the box and no runtime to ship.` scp` it over, run it for a year.


**Ingest where tail latency is the spec** . During a ten-minute satellite pass, the ground segment has to drain every frame the downlink produces; there is no catching up later. A garbage collector that pauses at the wrong moment turns into dropped telemetry. Rust’s lack of a GC doesn’t make your code faster on average; it makes the worst case boring, which is what you actually care about when the data source won’t wait.


**Backpressure you can reason about** . High-rate ingest lives or dies on flow control: how many batches are in flight, how much memory they pin, what happens when the database slows down. With tokio, that’s an explicit semaphore and bounded buffers checked by the type system, rather than a goroutine count you tune by load testing.


**The telemetry source is already Rust** . Increasingly, the code producing the telemetry is Rust: a ROS 2 node on an autonomous mobile robot, drone flight software, a soft-PLC runtime, or a protocol bridge that speaks OPC UA or MQTT-Sparkplug. When the producer is a Rust process, the historian client should be a library you embed in it: same binary, same async runtime, no sidecar process to deploy and monitor on every robot in the fleet. An` Arc"Client"` shared across your tokio tasks is the whole integration.


**Compile once, use anywhere** . The same binary often has to write to whatever InfluxDB the customer runs. As of 0.2, writes default to the V2` /api/v2/write` endpoint, so one client works unchanged against InfluxDB 3 Core and Enterprise as well as InfluxDB Clustered and Cloud Dedicated/Serverless. A config flag, not a code change, opts into the V3-only extras like` no_sync` when you control the server.


**You’re already in the Arrow ecosystem** . InfluxDB 3 itself is written in Rust on Arrow and DataFusion. With this client, query results come back as native` Arrow RecordBatch` es—the same types you’d hand to DataFusion, Polars, or your own analytics code, with no serialization boundary in between. The client and the server are speaking the same in-memory format end-to-end.


If none of these scenarios describe your situation, the Go and Python clients remain great choices. If one of them does, you should get familiar with the Rust client.


## Installation


The client is on[crates.io](http://crates.io/) and requires Rust 1.89 or later:


```text
cargo add influxdb3-client
```


Or add it to your` Cargo.toml` alongside an async runtime:


```text
[dependencies]
influxdb3-client = "0.2"
tokio = { version = "1", features = ["full"] }
```


There is an optional` polars` feature for DataFrame-based workflows, which we’ll come back to later.


#### Let Claude Write the Boilerplate


If you use[Claude Code](https://claude.com/claude-code) , there’s an **influxdb3 skill** that teaches it the InfluxDB 3 API surface: line protocol, the v3 SQL and InfluxQL query paths, token and database administration, and the client libraries—this one included. With the` claude-influxdb3` plugin installed, you can skip the docs-spelunking and ask things like:


```text
*"Write me a Rust snippet that writes a batch of sensor readings to my InfluxDB 3 cluster and reads the last hour back with SQL."*


```


You’ll get working code against this client’s actual API. It also knows the troubleshooting terrain: 401s from token scoping, line-protocol parse errors, and queries that silently return no rows. That’s most of what a first hour with any database consists of.


## Configuring a client


uuA client needs a host, a database, and (usually) an API token. This is the most explicit way is to build the configuration yourself:


```text
use influxdb3_client::{Client, ClientConfig};


#[tokio::main]
async fn main() -> influxdb3_client::Result"()" {
let client = Client::new(
ClientConfig::builder()
.host("http://localhost:8181")
.token("my-api-token")
.database("sensors")
.build()?,
)
.await?;
Ok(())
}
```


For deployments where configuration comes from the environment, such as a container or a systemd unit on an edge box, read` INFLUX_HOST` ,` INFLUX_TOKEN` , and` INFLUX_DATABASE` directly:


```text
let client = influxdb3_client::Client::from_env().await?;
```


Optional variables (` INFLUX_AUTH_SCHEME` ,` INFLUX_ORG` ,` INFLUX_PRECISION` ,` INFLUX_GZIP_THRESHOLD` , and the` INFLUX_WRITE_*` family) configure the same write defaults the builder exposes, so a deployed agent can be retuned without a rebuild.


Or, parse a single connection string, which is handy when configuration arrives as a single opaque value:


```text
"let client = influxdb3_client::Client::from_connection_string(
"https://cluster.example.io/?token=TOKEN&database=mydb",
).await?;
```


The Arrow Flight channel used for queries is opened lazily on the first query, so constructing a client never blocks on query connectivity, and a write-only ingest agent never pays for a query connection it won’t use.


## Writing data


` client.write(data)` returns a builder; chain the options you want, then` .await` it. The data argument is flexible—it can be a line-protocol string, a` Vec"Point"` , or (with the` polars` feature) a DataFrame.


#### Points


The` Point` builder is the most ergonomic way to construct measurements in code:


```text
use influxdb3_client::{Point, Precision};


let points = vec![
Point::new("temperature")
.tag("location", "office")
.tag("floor", "2")
.field("celsius", 22.5_f64)
.field("humidity", 48_i64)
.field("occupied", true),
];


client.write(points).precision(Precision::Millisecond).await?;
```


#### Raw Line Protocol


If you already have line protocol, say forwarded straight off a device, you can write it as is:


```text
client
.write("cpu,host=server01 usage_user=42.3,usage_system=1.2")
.await?;
```


#### Write Options


The builder exposes the knobs that matter for real ingest pipelines:


```text
client.write(points)
.precision(Precision::Nanosecond)
.batch_size(10_000)          // points per HTTP request
.max_inflight(8)             // concurrent in-flight requests
.default_tag("region", "us-east")
.tag_order(["region", "host"])
.await?;
```


Large inputs are split into batches and sent as multiple pipelined requests, with one batch buffer held in memory at a time, so memory stays bounded even on very large writes.


` tag_order` matters more than it appears to. The **first write defines the physical tag column order for a table, and that order** affects query performance; tags you filter on most should sort first.` .tag_order(...)` serializes the listed tags first, then appends any remaining tags in deterministic lexicographic order, so the machine that happens to boot first doesn’t accidentally pick a bad layout for everyone. (Background:[sort tags by query priority](https://docs.influxdata.com/influxdb3/core/write-data/best-practices/optimize-writes/#sort-tags-by-query-priority) .)


#### Which Write Endpoint?


As of 0.2, writes go to the V2` /api/v2/write endpoint` **by default** , which means the same client also works against InfluxDB Clustered and InfluxDB Cloud Dedicated/Serverless without changes. Opting into the V3 endpoint (` ClientConfig::builder().write_use_v2_api(false)` , or` INFLUX_WRITE_USE_V2_API=false` in the environment) unlocks two V3-only behaviours:` no_sync()` (acknowledge before the WAL is synced) and partial-write reporting, both covered below.


#### High-Throughput Ingest


For sustained, high-volume writes, such as a satellite pass or a full plant floor, the throughput levers are` batch_size` (points per request) and` max_inflight` (concurrent requests per call). On the V3 endpoint (` write_use_v2_api(false)` ),` no_sync()` adds a third: acknowledge before the WAL is synced, trading a little durability for speed.


A single` write` call serializes its batches on one task. To use more CPU cores and connections, run several` write` calls concurrently. A` Client` is cheap to share, and its HTTP connection pool is reused, so the idiomatic pattern is to wrap it in an` Arc` , spread chunks across tasks, and cap concurrency with a semaphore to keep in-flight buffers bounded:


```text
use std::sync::Arc;
use tokio::sync::Semaphore;


let client = Arc::new(client);
// cap concurrent writes
let gate = Arc::new(Semaphore::new(8));


// each chunk is a Vec"Point"
for chunk in chunks {
let permit = gate.clone().acquire_owned().await.unwrap();
let client = Arc::clone(&client);
tokio::spawn(async move {
// released when the write completes
let _permit = permit;
client
.write(chunk)
.batch_size(10_000)
.max_inflight(8)
.no_sync()


.await
});
}
```


To spread load across multiple ingest nodes, put a load balancer in front of the cluster, or construct one` Client` per node and distribute chunks across them.


## Querying data


InfluxDB 3 supports both **SQL** and **InfluxQL** , and the client exposes both through the same query-builder pattern:` client.sql(q)` or` client.influxql(q)` .


```text
let result = client
.sql("SELECT * FROM temperature ORDER BY time DESC LIMIT 10")
.await?;


for row in result {
let row = row?;
let loc = row["location"].as_str().unwrap_or("");
let c = row["celsius"].as_f64().unwrap_or(0.0);
println!("{loc}: {c}");
}
```


InfluxQL is called in exactly the same way, which makes it easy to bring existing InfluxQL queries forward:


```text
let result = client
.influxql("SELECT MEAN(celsius) FROM temperature WHERE time > now() - 1h")
.await?;
```


#### Parameterized Queries


Bind parameters with` .param()` rather than interpolating into the query string:


```text
let rows = client
.sql("SELECT COUNT(*) AS n FROM cpu WHERE host = $host")
.param("host", "server01")
.await?
.rows()?;


if let Some(r) = rows.first() {
println!("count: {}", r["n"]);
}
```


#### Working with Rows


A` QueryResult` can be iterated row by row, collected all at once with` .rows()` , or accessed as raw Arrow` RecordBatches` with` .record_batches()` if you want to hand the columnar data straight to another Arrow-aware library. A` Row` is indexed by column name (` row\["col"\]` ) or position (` row\[0\]` ), and yields a Value with typed accessors:` as_f64` ,` as_i64` ,` as_str` ,` as_bool` ,` is_null` .


One deliberate design choice: if a query returns an Arrow type the row API doesn’t support, you get an explicit` Error::UnsupportedArrowType` , rather than a silent null. Telemetry pipelines fail quietly often enough without the client library helping.


#### Streaming Large Results


For analytical queries whose results are too large to hold in memory, like a scan over a month of high-rate telemetry, stream the Arrow batches instead of collecting them:


```text
use futures_util::TryStreamExt;


let mut stream = client.sql("SELECT * FROM temperature").stream().await?;
while let Some(batch) = stream.try_next().await? {
println!("got {} rows", batch.num_rows());
}
```


#### Reliability


Telemetry pipelines run unattended, so the client retries transient failures automatically with exponential backoff and full jitter. Connection errors, timeouts,` 429` , and` 5xx` responses are retried, and` Retry-After` is honored when present. Deterministic failures (other` 4xx` responses and partial writes) are never retried. Retrying writes is safe because line-protocol writes are idempotent at the (` series` ,` timestamp` ,` field` ) level, i.e., re-sending the same point simply overwrites it.


```text
use influxdb3_client::RetryConfig;
use std::time::Duration;


// Per-request override.
client.write(points)
.retry(RetryConfig { max_retries: 5, base_delay: Duration::from_millis(100), ..RetryConfig::default() })
.await?;


// Disable retries for a single call.
client.write(points).no_retry().await?;
```


On the V3 endpoint, when a batch contains invalid lines, the server accepts the valid ones and reports the rest, which surfaces as` Error::PartialWrite` with the rejected lines attached:


```text
use influxdb3_client::Error;


if let Err(Error::PartialWrite(e)) = client.write(line_protocol).await {
for line_error in &e.line_errors {
eprintln!("line {}: {}", line_error.line, line_error.message);
}
}
```


If all-or-nothing semantics fit your pipeline better,` .accept_partial(false)` rejects the entire batch when any line fails.


## Polars integration


For data-engineering and analysis workflows, the optional` polars` feature lets you write a DataFrame directly and read query results back as one:


```text
influxdb3-client = { version = "0.2", features = ["polars"] }


use influxdb3_client::write_dataframe::DataFrameWrite;
use polars::prelude::*;
```


```text
let df = df![
"host"    => ["srv1", "srv2"],
"region"  => ["us-east", "us-west"],
"cpu_pct" => [42.5_f64, 71.0_f64],
"time"    => [1_700_000_000_000_000_000_i64, 1_700_000_001_000_000_000_i64],
]?;


client
.write(
DataFrameWrite::new(&df, "server_metrics")
.tags(&["host", "region"])
.timestamp_column("time"),
)
.await?;


let df_back = client
.sql("SELECT * FROM server_metrics")
.await?
.to_polars()?;
```


#### Backfilling from Parquet Files


Let’s look at a common migration task. Say you have historical telemetry sitting in Parquet files, exported from another system or batch dumped from a data lake, and you want it in InfluxDB where it can be queried alongside live data. File IO deliberately lives in your code rather than the client; read the file with Polars, then hand the frame to` DataFrameWrite` . Enable the Parquet reader on Polars in your own` Cargo.toml` :


```text
polars = { version = "0.53", features = ["Parquet"] }
```


```text
use std::fs::File;
use polars::prelude::*;
use influxdb3_client::write_dataframe::DataFrameWrite;


let df = ParquetReader::new(File::open("sensors.Parquet")?).finish()?;


client
.write(
DataFrameWrite::new(&df, "sensor_data")
.tags(&["site", "line", "machine_id"])
.timestamp_column("time"),
)
.await?;
```


Parquet files carry their schema, so dtypes arrive correct—floats stay floats, timestamps stay timestamps. Columns you name in` .tags(&\[...\])` become tags, the timestamp column sets each point’s time, and every remaining column becomes a field. The whole pipeline, from Parquet reader through DataFrame and client to server, is Arrow-native, so the data never leaves columnar form until the final encode.


For a multi-gigabyte backfill, read and write in file-sized chunks rather than one giant frame, and reuse the high-throughput pattern from earlier (` batch_size` ,` max_inflight` , one task per file) to keep the pipe full.


#### Loading CSV Exports


Most SCADA packages and historians will export CSV, so it’s often the format you’re handed. The same programming pattern works, with one caveat: CSV carries no schema, so Polars infers column types, and anything ambiguous infers as a **string** . A string column becomes a string field in InfluxDB, and you can’t` MEAN()` a string. Supply the dtypes explicitly and parse the timestamp at read time:


```text
polars = { version = "0.53", features = ["csv"] }
```


```text
use polars::prelude::*;
use influxdb3_client::write_dataframe::DataFrameWrite;


let schema = Schema::from_iter([
Field::new("machine_id".into(), DataType::String),
Field::new("rpm".into(), DataType::Float64),                       Field::new("spindle_load_pct".into(), DataType::Float64),
Field::new("alarm_active".into(), DataType::Boolean),
Field::new("time".into(), DataType::Datetime(TimeUnit::Nanoseconds, None)),
]);


let df = CsvReadOptions::default()
.with_schema(Some(Arc::new(schema)))
.try_into_reader_with_file_path(Some("plc_export.csv".into()))?
.finish()?;


client
.write(
DataFrameWrite::new(&df, "machine_telemetry")
.tags(&["machine_id"])
.timestamp_column("time"),
)
.await?;
```


If you’d rather let inference do the first pass, that works too. Just` cast()` the numeric and boolean columns before writing, or they’ll land as string fields and you’ll be wondering why your aggregation queries return nothing.


## FAQ


What is influxdb3-client? The Rust` influxdb3-client` is a Rust native client library for programmatically interacting with Influxdb. It simplifies writing Line Protocol (Influxdb’s native format) as well writing queries in SQL and Influxql.


What Rust version does it require? The crate requires Rust version 1.89 or later.


Does it work with InfluxDB Cloud Dedicated, Clustered, or Serverless, or only Core and Enterprise? Yes! The client library has a full backwards compatible api and integrates with any version of Influxdb that supports the v2 write API.


Does the client support InfluxDB 1.x or 2.x? Yes. Later versions of InfluxDB have forward compatibility APIs, and all versions of Influxdb support the v2 write API.


Why would I use this instead of the Go or Python client? Rust excels in embedded programing environments, or integrations with other Rust codebases.


Is the Polars integration required? No. Polars enables reading and writing to CSV and Parquet files, or if you want to leverage Polars Dataframes, but isn’t required.


Does it retry failed writes automatically? Yes, the client retries transient failures automatically with exponential backoff and full jitter.


Where do I report issues or request features? https://github.com/InfluxCommunity/influxdb3-rust


## Try it


The repository ships runnable examples in[examples/:](https://github.com/InfluxCommunity/influxdb3-rust/tree/main/examples) a` quickstart` that does an end-to-end write and query, a Cloud Dedicated connection example, and a Polars DataFrame round-trip. Point them at a running InfluxDB 3 instance and go:


```text
INFLUX_HOST=http://localhost:8181 INFLUX_TOKEN=token INFLUX_DATABASE=mydb \
cargo run --example quickstart
```


The client is[influxdb3-client on crates.io](https://crates.io/crates/influxdb3-client) , the source lives on[GitHub](https://github.com/InfluxCommunity/influxdb3-rust) , and the API docs are on[docs.rs](https://docs.rs/influxdb3-client) . It’s early, and feedback, issues, and pull requests are all welcome.
