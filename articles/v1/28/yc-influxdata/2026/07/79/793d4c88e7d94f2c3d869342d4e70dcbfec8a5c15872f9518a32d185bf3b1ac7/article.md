---
schema_version: "1.0.0"
document_id: "793d4c88e7d94f2c3d869342d4e70dcbfec8a5c15872f9518a32d185bf3b1ac7"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/telegraf-1-39-release-notes-influxdb/"
published_at: "2026-07-10T08:00:00+00:00"
first_seen_at: "2026-07-20T23:23:58.693982+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:0d972624a3be1e9cd330f995187f508f29efc53e64fdfed90e11a75f33b2f050"
---

# Telegraf 1.39 Release Notes

Table of Contents


A new feature-bearing release for Telegraf is now available:


- Telegraf 1.39 —[Release notes](https://docs.influxdata.com/telegraf/v1/release-notes/?utm_source=website&utm_medium=direct&utm_campaign=telegraf_1_39_release_notes_influxdb&utm_content=blog)


You can find the binaries for the latest Telegraf release on our Downloads page. Many thanks to all the open source community members who contributed to this effort!


## New plugins


These are the newest plugins, first available in this version:


- **GNMI dial-out input** (` inputs.gnmi_listener` )


- Receive GNMI dial-out telemetry data pushed by network equipment such as Nokia SR OS devices.
- Please open a[feature request](https://github.com/influxdata/telegraf/issues/new?template=FEATURE_REQUEST.yml/?utm_source=website&utm_medium=direct&utm_campaign=telegraf_1_39_release_notes_influxdb&utm_content=blog) to request support for your devices.
- Contributed by[srebhan](https://github.com/srebhan/?utm_source=website&utm_medium=direct&utm_campaign=telegraf_1_39_release_notes_influxdb&utm_content=blog)


## Important changes


Here are some changes to highlight:


- **OPCUA node discovery**


- The OPCUA input plugins, both` inputs.opcua` and` inputs.opcua_listener` , can now be configured to discover nodes based on filtering patterns. This allows Telegraf to work in dynamic environments where nodes are added or removed on the server side.
- If configured, the plugin will browse available nodes on the server and filter them according to your settings. It will then subscribe or listen to the remaining nodes to create metrics.
- Use the` browse` settings to specify the root node and depth for the nodes to discover and one or more` browse.paths` patterns to filter the nodes found.


- **Oracle SQL driver support**


- Use the` outputs.sql` plugin to stream metrics to Oracle databases.
- **Custom header support for Kafka**
- Set custom record headers for messages sent by` outputs.kafka` . The header values support templating, taking the metrics sent as input.


- **MongoDB custom metadata**


- Set custom metadata in` outputs.mongodb` for the MongoDB documents written by specifying a tag subset. This allows easier and more efficient querying while keeping the full metric information.


- **OpenTelemetry improvements**


- The` outputs.opentelemetry` plugin now allows using a proxy or authenticating with a token.


- **More system details**


- The` inputs.system` plugin provides more details on the host system, such as DMI hardware and operating system information.
- To include this information, please opt in by adding the respective` include` settings in your configuration.


## Downloads


Head to our[Downloads page](https://portal.influxdata.com/downloads/?utm_source=website&utm_medium=direct&utm_campaign=telegraf_1_39_release_notes_influxdb&utm_content=blog) to get the latest Telegraf release. If you have issues or questions, please join our[InfluxDB Community Slack](https://influxdata.com/slack/?utm_source=website&utm_medium=direct&utm_campaign=telegraf_1_39_release_notes_influxdb&utm_content=blog) or post them in our[InfluxDB GitHub Repo](https://github.com/influxdata/telegraf/issues/?utm_source=website&utm_medium=direct&utm_campaign=telegraf_1_39_release_notes_influxdb&utm_content=blog) or[Community Site](https://community.influxdata.com/c/influxdb2/?utm_source=website&utm_medium=direct&utm_campaign=telegraf_1_39_release_notes_influxdb&utm_content=blog) , and we will look into them.


## InfluxDB University


Learn more about collecting data with Telegraf by taking the free InfluxDB University[Data Collection with Telegraf course](https://university.influxdata.com/courses/data-collection-with-telegraf-tutorial/?utm_source=website&utm_medium=direct&utm_campaign=telegraf_1_39_release_notes_influxdb&utm_content=blog) .
