---
schema_version: "1.0.0"
document_id: "a3a40bd6148eabb2929901039d0ee35c86eaf30fe036de16f621edf736116b89"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/influxdb-3-mcp-server-v1-3/"
published_at: "2026-05-28T02:00:00+00:00"
first_seen_at: "2026-07-20T23:23:58.693982+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:449e1131f30dd28f85015599485bd188d9a7579ee64b9863917206fe887da360"
---

# InfluxDB 3 MCP Server v1.3.0: AI Access to Time Series Data

Table of Contents


*A more reliable agent that learns your schema, queries your data, and investigates alerts - that’s what’s possible with this release of our MCP server v1.3.0.*


## TL;DR


This release enhances the InfluxDB 3 MCP server—which already allows AI agents like Claude and ChatGPT to read from and write to your InfluxDB 3 instance using natural language—with key dependability improvements, including protocol compliance tests, integration tests, and a properly scoped npm package.


- **What it is** : an MCP server that exposes InfluxDB 3 tools, including query, write, schema discovery, database management, and token management, to any MCP-compatible client.
- **What changed in 1.3.0** : test coverage, CI, the updated MCP SDK (1.27.1),` @influxdata/influxdb3-client` 1.4.0, a Node 20.11+ baseline, and safer error responses.
- **Install** :` npx -y @influxdata/influxdb3-mcp-server` . No clone, no build.
- **See it work** : we ran an agent investigation against the[influxdb3-ref-bess](https://github.com/influxdata/influxdb3-ref-bess/?utm_source=website&utm_medium=influxdb_3_mcp_server_v1_3&utm_content=blog) reference architecture with prompts for schema discovery, current state, forcing a thermal runaway alert, and pivoting back to readings.
- **Where to get it** :[npm](https://www.npmjs.com/package/@influxdata/influxdb3-mcp-server) ·[Docker](https://hub.docker.com/r/influxdata/influxdb3-mcp-server) ·[GitHub](https://github.com/influxdata/influxdb3_mcp_server) ·[CHANGELOG](https://github.com/influxdata/influxdb3_mcp_server/blob/main/CHANGELOG.md)


## What is the InfluxDB 3 MCP server?


The Model Context Protocol is an open standard for connecting AI agents to external tools and data. Our MCP server is the official InfluxData implementation: it runs locally over stdio, accepts a connection from your MCP client (Claude Desktop, Claude Code, OpenAI Codex, Cursor, and others), and exposes InfluxDB 3 as a set of tools the agent can call on your behalf.


It works across all InfluxDB 3 editions—Core, Enterprise, Cloud Dedicated, Cloud Serverless, and Clustered—with tools adapted to each product. For example, admin token management is only available on Core and Enterprise; bucket retention is a Cloud Serverless concern.


For background and the original tool tour, see our previous posts:[Introducing the InfluxDB 3 MCP server](https://www.influxdata.com/blog/influxdb-mcp-server/?utm_source=website&utm_medium=influxdb_3_mcp_server_v1_3&utm_content=blog) and[Building with the InfluxDB 3 MCP server & Claude](https://www.influxdata.com/blog/influxdb-3-mcp-server-claude/?utm_source=website&utm_medium=influxdb_3_mcp_server_v1_3&utm_content=blog) .


## What’s new in v1.3.0?


This release makes the MCP server safer and more dependable:


- **Scoped npm package** . The official server is now` @influxdata/influxdb3-mcp-server` (the unscoped` influxdb-mcp-server` on npm is an unrelated community project); always use the scoped name.
- **Protocol-compliance test suite** . Verifies server startup, the MCP handshake, and tool/resource/prompt registration.
- **Integration tests against Core and Cloud Serverless** . A first set covering` health_check` ,` list_databases` , and` execute_query` .
- __Server-level errors now set` isError__: true` , matching handler-level responses. MCP clients see failures as failures, not as empty success responses.
- **Plain-text error responses (HTTP 500) from Core now pass through to the MCP client with their original message,** instead of being replaced by a generic “Internal Server Error.”
- **MCP SDK 1.12 → 1.27.1** and` @influxdata/influxdb3-client` **1.1 → 1.4.0** .
- **Node 20.11 minimum** : Node 18 reached end of life.


## How do I install the MCP Server for InfluxDB 3 Core?


If you haven’t already installed Core and created an admin token, follow the[Set up Core](https://docs.influxdata.com/influxdb3/core/get-started/setup/) guide (you’ll be running within 5 minutes).


The following example uses` npx` , which is the easiest way to get started.


1. **Add the server to your MCP client config** . For Claude Desktop, edit` claude_desktop_config.json` :


```text
{
"mcpServers": {
"influxdb": {
"command": "npx",
"args": ["-y", "@influxdata/influxdb3-mcp-server"],
"env": {
"INFLUX_DB_INSTANCE_URL": "http://localhost:8181/",
"INFLUX_DB_TOKEN": "YOUR_ADMIN_TOKEN",
"INFLUX_DB_PRODUCT_TYPE": "core"
}
}
}
}
```


1.


**Restart Claude Desktop** . The first launch downloads the package; subsequent launches are instant.


2.


**Sanity-check** . Open a new chat and ask: “Use the InfluxDB tools to list my databases.” Claude should call the` list_databases` tool and return whatever’s on your instance.


#### Other MCP Clients


The[README](https://github.com/influxdata/influxdb3_mcp_server#readme) includes specific config-file snippets for other MCP clients.


Prefer Docker? The v1.3.0 image is on Docker Hub at[influxdata/influxdb3-mcp-server](https://hub.docker.com/r/influxdata/influxdb3-mcp-server) . Our[January walkthrough](https://www.influxdata.com/blog/influxdb-3-mcp-server-claude/?utm_source=website&utm_medium=influxdb_3_mcp_server_v1_3&utm_content=blog) covers the Docker-based setup in detail.


## How do I install the MCP Server for InfluxDB 3 Enterprise?


Enterprise uses the same configuration shape, just replace the environment variable values:


```text
{
"mcpServers": {
"influxdb": {
"command": "npx",
"args": ["-y", "@influxdata/influxdb3-mcp-server"],
"env": {
"INFLUX_DB_INSTANCE_URL": "https://your-enterprise-host/",
"INFLUX_DB_TOKEN": "YOUR_ADMIN_TOKEN",
"INFLUX_DB_PRODUCT_TYPE": "enterprise"
}
}
}
}
```


Enterprise adds licensed multi-node and read/write replica features, but to the MCP client, the tools look the same as Core. The[Enterprise install guide](https://docs.influxdata.com/influxdb3/enterprise/install/) covers licensing and provisioning.


#### Cloud Variants


Cloud Serverless uses the same three-variable shape, just swap the URL and set` INFLUX_DB_PRODUCT_TYPE=cloud-serverless` . Cloud Dedicated and Clustered are slightly different: Cloud Dedicated takes a cluster ID instead of a host URL, and both support an optional separate management token. See the exact recipe for each in the MCP server[README](https://github.com/influxdata/influxdb3_mcp_server#readme) .


## Try it against a real running system


Run MCP against live data in one of our reference architecture demos to simulate a real environment. The ref arch demos run on InfluxDB 3 Enterprise by default.


[influxdb3-ref-bess](https://github.com/influxdata/influxdb3-ref-bess/?utm_source=website&utm_medium=influxdb_3_mcp_server_v1_3&utm_content=blog) is our reference architecture for Battery Energy Storage Systems on InfluxDB 3: a simulator writing 768 cells across 4 packs at ~2,000 points per second, in-database Python plugins performing thermal-runaway detection and daily SoH rollups, and a control-room UI.` make demo` brings the whole thing up in about two minutes.


Point your MCP server’s` INFLUX_DB_INSTANCE_URL` and` INFLUX_DB_TOKEN` at the ref-arch instance, and set` INFLUX_DB_PRODUCT_TYPE=enterprise` . An agent now has a real plant to investigate. Here’s a session we ran in Claude Code.


1.


**Orient** .


“I have InfluxDB 3 running locally with a BESS reference simulator writing data into it. List the databases on this instance, then, for the` bess` database, show me which measurements exist and the schema of each.”


The agent chose the tools and made seven calls:` list_databases` , then` get_measurements` and` get_measurement_schema` once per table. It returned this summary:` cell_readings` (per-cell voltage and temperature),` pack_readings` (pack-level current, SoC, SoH),` inverter_readings` (site power), and` alerts` (an event stream written by an in-database plugin when a cell crosses a threshold).


1. **Current state of the plant** .


“Show me the current state of each pack: the most recent` current_a` , SoC, and SoH per` pack_id` from` pack_readings` .”


```text
pack     current_a   SoC      SoH       as of
pack-0   -80.0       53.0%    98.1%     14:09:36
pack-1   -80.0       61.8%    97.9%     14:09:36
pack-2   -80.0       63.8%    98.1%     14:09:36
pack-3   -80.0       60.8%    98.0%     14:09:36
```


The agent used` DISTINCT ON (pack_id) ... ORDER BY pack_id, time DESC` , the DataFusion/Postgres idiom for “latest row per group,” which is typically cheaper than the more portable` ROW_NUMBER()` window-function pattern. It picked the SQL from the schema it discovered in prompt 1. The agent also flagged a simulator bug that we hadn’t asked about: all four packs were reporting the same current. Real BESS sites rarely dispatch evenly due to SoC balancing, thermal derating, and inverter routing.


1.


**Investigate alerts** .


“Any alerts in the last 15 minutes in the` alerts` table? If so, find the most recent alert’s` pack_id` and timestamp, then show me the` cell_readings` from that pack within a one-minute window around the alert time.”


The result: zero alerts. The agent ran one query for the 15-minute window, ran a second query to confirm the table had no rows, and then declined to invent any. It didn’t fabricate rows or an alert just because we’d asked for one. The MCP server’s error handling keeps errors distinct from success, and makes empty results easy to distinguish from silence.


1.


**Force the alert condition.**


“Force a hot cell in` bess` so that the thermal-runaway trigger fires.”


The agent read the plugin source to learn the threshold (` temperature_c > 70` ), then wrote one synthetic line-protocol row for` pack-0/module-0/cell-0` at 85 °C using` write_line_protocol` . The Processing Engine’s WAL trigger fired in-process on that write and emitted a row into` alerts` . No restart or orchestration is required because plugins run *inside* InfluxDB 3.


1.


**Now run the original investigation.**


“Show me the cell readings around that alert.”


The agent’s first attempt asked for ±1 minute and received 1,542 rows, which it then narrowed to the hot cell’s timeline:


```text
time            voltage   temperature_c
14:12:41.617    3.643     22.08
14:12:57.071    3.646     22.20
14:13:07.557    3.650     85.00   ← injected, fires alert
14:13:12.529    3.646     22.00
14:13:27.989    3.646     21.90
```


The temperature spike alone triggered the alert, but the agent also flagged what we should expect for a real thermal event: “in a real cell, an 85 °C excursion would correlate with voltage and current anomalies.”


Five prompts, no handwritten SQL: discover the schema, read the current state, search alert events, write a single line of synthetic data to fire the trigger, and tie the resulting alert back to the underlying readings.


## Links


- **npm** :[@influxdata/influxdb3-mcp-server](https://www.npmjs.com/package/@influxdata/influxdb3-mcp-server)
- **Docker Hub** :[influxdata/influxdb3-mcp-server](https://hub.docker.com/r/influxdata/influxdb3-mcp-server)
- **Repository** :[github.com/influxdata/influxdb3_mcp_server](https://github.com/influxdata/influxdb3_mcp_server)
- **Release notes** :[v1.3.0 on GitHub](https://github.com/influxdata/influxdb3_mcp_server/releases/tag/v1.3.0)
- **Issues / tool requests** :[GitHub issues](https://github.com/influxdata/influxdb3_mcp_server/issues)
- **Earlier posts** :[Introducing the MCP server](https://www.influxdata.com/blog/influxdb-mcp-server/?utm_source=website&utm_medium=influxdb_3_mcp_server_v1_3&utm_content=blog) ·[Building with Claude](https://www.influxdata.com/blog/influxdb-3-mcp-server-claude/?utm_source=website&utm_medium=influxdb_3_mcp_server_v1_3&utm_content=blog)


Try it and tell us what’s missing.
