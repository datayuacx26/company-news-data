---
schema_version: "1.0.0"
document_id: "894971eef6b7033e44f7c7863d005a415e51f3fb792430eb70902607eb4c1676"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/explorer-1-8/"
published_at: "2026-04-30T01:00:00+00:00"
first_seen_at: "2026-07-20T23:23:58.693982+00:00"
fetched_at: "2026-07-28T22:15:31.883696+00:00"
content_hash: "sha256:e84c44b3b23e59a517a5e3ad58945acd7d55e020f922712d85c753715b8a2dc6"
---

# What's New in InfluxDB 3 Explorer 1.8: Streaming Subscriptions, Smarter Sample Data, Line Protocol Validation, and Retention Controls

Table of Contents


InfluxDB 3 Explorer 1.8 is all about writing data and keeping it under control. You can now subscribe to MQTT, Kafka, and AMQP streams directly from Explorer, generate custom sample datasets, stream live sample data continuously into your database, and validate your line protocol and preview the resulting schema before you write it. You can now also view and edit retention periods on both databases and individual tables.


## Data Subscriptions: stream from MQTT, Kafka, and AMQP


InfluxDB 3 Explorer now includes a **Data Subscriptions** page (powered by the[MQTT](https://github.com/influxdata/influxdb3_plugins/blob/main/influxdata/mqtt_subscriber/README.md) ,[Kafka](https://github.com/influxdata/influxdb3_plugins/blob/main/influxdata/kafka_subscriber/README.md) , and[AMQP subscriber](https://github.com/influxdata/influxdb3_plugins/blob/main/influxdata/amqp_subscriber/README.md) plugins) that lets you wire a streaming source directly into a database.


Pick a provider, fill in configuration details, and Explorer installs and activates the right Processing Engine plugin behind the scenes. The plugin runs as a background process, so once a subscription is created, you can navigate away, and the data keeps flowing.


The MQTT configuration contains: a subscription name, target database, broker host and port, client ID, optional authentication and TLS, and the topics you want to subscribe to (one per line, with` #` and` +` wildcards supported). The **Message Format** section allows you to map your data to your schema. If your messages already arrive as` Line Protocol` format, you’re good to go. However, if necessary, you can also parse` JSON` to map keys onto tags and fields, or extract from` Text` using regex patterns.


Kafka and AMQP work the same way, with the connection details specific to each protocol. Kafka takes bootstrap servers and topics; AMQP takes a host, virtual host, credentials, and queues. Once you’ve created a subscription, the **Stream Status** tab gives you a single place to monitor your running subscriptions. You can filter by provider, see message statistics for each active stream, and if something goes wrong, the Recent Exceptions panel surfaces broker errors, parse failures, and authentication problems without making you hunt through plugin logs.


A note on requirements: Data Subscriptions need InfluxDB 3 Core or Enterprise running version **3.9.0 or higher** .


## Sample data, three ways


The Write Sample Data page existed in earlier versions of Explorer, but it was thin. Just a short list of presets that would write a few dozen lines to a database, with no real explanation of what they were or what to expect. In 1.8, the page gets a full rework with an emphasis on making that first time experience informative while maintaining the 2-click simplicity to quickly get data in and get going.


#### Static Sample Data Presets


The previous preset datasets (Air Sensor, Bird Migration, Bitcoin, NOAA Weather) are still present, but selecting one now opens a details panel that shows you exactly what you’re about to write before you commit. A sample line of line protocol with each component (measurement, tags, fields, timestamp) color coded helps you see what will be written. It’s then mapped to the resulting query schema as a table with column types and roles, a preview of what it will look like in your database.


The presets also generate a more realistic volume of data than before. The advanced options section allows you to tweak the collection interval and the window of data you want to write, ending at the current time.


#### Custom Datasets (with a Dash of AI)


The preset datasets aren’t your only option for quick sample data anymore. If you have an AI provider configured under Configure → Integrations, you can make use of the **Custom dataset (AI)** option. Describe what you want in natural language (e.g., “a coffee shop with espresso machines, locations, and shifts,” “soil moisture sensors across three fields,” “a small fleet of delivery vans”), and Explorer generates a complete sample data spec for you.


The output is a realistic, ready to use schema with appropriate measurement names, tags, fields, and types. After the initial generation, you can refine the spec with the` Refine schema` with AI input, where you can say things like “drop the locations tag” or “let’s make this about a tea shop instead,” and the spec updates in place, highlighting your changes. Just as with the preset sample data, the **Advanced options** panel lets you set the interval and time window.


When you’re happy with it, click Write Sample Data, and Explorer creates a new database with your data ready for querying.


## Live data plugins, for real-time sample data


Static datasets are great for poking around with queries and exploring schema, but a lot of what makes InfluxDB interesting (alerts, transformations, automation) requires new data showing up over time. The new **Live Data** tab on the Sample Data page solves that.


Live Data uses the Processing Engine to continuously write data to your database on a schedule. Explorer 1.8 ships with two plugins out of the box: the[System Metrics Collector](https://github.com/influxdata/influxdb3_plugins/blob/main/influxdata/system_metrics/README.md) (host CPU, memory, disk, and network metrics from` psutil` ) and the[US Weather Sampler](https://github.com/influxdata/influxdb3_plugins/blob/main/influxdata/nws_weather/README.md) (live observations pulled from National Weather Service stations).


The layout follows the same pattern as the static page: pick a plugin, see the schema preview and a few rows of line protocol, choose a database, and click Activate. From there, it just runs, regularly writing data to your database. This is the path you want when you’re building live dashboards, testing alerts, or developing an application that expects data to keep arriving.


## Line protocol validation and schema preview


The **Write Line Protocol** page (under Write Data → Dev Data) now validates Line Protocol as you type, and shows a live **Schema Preview** of what your data is about to look like in your database. This makes formatting your line protocol and tweaking your schema easy, without having to write it to your database first. Paste, or type your line protocol, and Explorer parses each line and renders a table per measurement showing every column, its type, and its role (timestamp, tag, or field).


When something is wrong, you don’t have to wait for the server to tell you. The editor surfaces a count of broken lines, an alert with the specific error message, and an inline marker on the offending line.


The same applies if you upload a file using` Upload file` —Explorer will read it in, validate every line, and tell you exactly which lines need fixing before you write a single one. There’s also a **Line Protocol Reference** panel pinned to the right of the page covering the format, allowed types, escaping rules, and timestamp precision, so you don’t have to flip back to the[line protocol docs](https://docs.influxdata.com/influxdb3/enterprise/reference/line-protocol/) every time you forget whether integers take an` i` suffix.


## Database and table retention


InfluxDB 3 has supported per-database and per-table retention for a while, but until now, you had to set them through the API or CLI. In 1.8, retention shows up everywhere it should in the UI.


There’s a new **Retention Period** column on both the Manage Databases and Manage Tables pages, so you can see at a glance how long each database or table is keeping its data:


When you create a new database, the dialog now has a Retention Period field (tables previously had this available on create). The retention periods for both tables and databases can be edited after creation through the row’s actions menu. Tables follow the standard inheritance behavior: set a retention period, and the table uses it; set it to **None** , and the table inherits from the database.


If you’re new to how retention works in InfluxDB 3, the[data retention reference](https://docs.influxdata.com/influxdb3/enterprise/reference/internals/data-retention/) covers the underlying behavior.


## Get started with Explorer 1.8


If you’ve been wanting to get streaming data into Explorer without standing up a separate connector, or you’ve been doing the “let me eyeball this line protocol and hope it parses” dance, this release should make those quite a bit smoother. As always, the previous post—[What’s New in InfluxDB 3 Explorer 1.7: Table Management, Data Import, Transforms, and More](https://www.influxdata.com/blog/influxdb-explorer-1-7/) —is worth a look if you skipped that one and want to catch up on table-level schema management, the InfluxDB-to-InfluxDB import flow, and the Transform Data pages.


To update InfluxDB 3 Explorer, pull the latest Docker image:` docker pull influxdata/influxdb3-ui`
