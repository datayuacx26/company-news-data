---
schema_version: "1.0.0"
document_id: "1b882aff2abbe205dd67107aeafd32da2046a94373e6379cd2ec65b25d61d8f3"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/48235-migration-of-supabase-management-api-logs-all-analytics-endpoint-to-logs-endpoint"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:7841b85f41077b2952765732e5a81234ac92a9f8aa809f480b7ea70b13b7ea6e"
---

# Migration of Supabase Management API logs.all analytics endpoint to logs endpoint

The` logs.all` Management API endpoint is being removed on 23rd September 2026 (Wednesday), two months from this announcement. Log querying moves to a new ClickHouse-backed` logs` endpoint. The new endpoint accepts ClickHouse SQL only. It returns every source through a single unified` logs` table instead of a separate table per source.


## How Do I Know if This Impacts Me?#


You are impacted only if you query the` logs.all` Management API endpoint (` .../analytics/endpoints/logs.all` ). This includes any scripts, integrations, or tooling that call it directly.


If you do not call this endpoint, no action is needed. Using logs through the dashboard Logs Explorer is not affected.


## What Should I Do?#


1. **Update the endpoint path** from[analytics/endpoints/logs.all](https://supabase.com/docs/reference/api/v1-get-project-logs-all) to[analytics/endpoints/logs](https://supabase.com/docs/reference/api/v1-get-project-logs) .
2. **Convert your SQL to the ClickHouse dialect.** The endpoint only accepts ClickHouse SQL.
3. **Filter by` source_name` instead of selecting a source table.** All sources now live in one` logs` table. Filter to a specific source in the` WHERE` clause.


**Before** (query a source table directly):


`
_10


SELECT timestamp, event_message


_10


FROM edge_logs


_10


ORDER BY timestamp DESC


_10


LIMIT 100


`


**After** (filter the unified stream by` source_name` ):


`
_10


SELECT timestamp, event_message


_10


FROM logs


_10


WHERE source_name = 'edge_logs'


_10


ORDER BY timestamp DESC


_10


LIMIT 100


`


Any query that previously targeted a specific table must now add a` source_name` filter in its` WHERE` clause.


## Accessing Nested Fields#


Nested fields move from the` metadata` array to a flat` log_attributes` map. Before, you added one` CROSS JOIN unnest()` per level of nesting to reach a field. Now you read the field directly from` log_attributes` with map key access. The joins go away.


**Before** (unnest each level of` metadata` ):


`
_10


SELECT timestamp, request.method, header.x_real_ip


_10


FROM edge_logs


_10


CROSS JOIN unnest(metadata) AS m


_10


CROSS JOIN unnest(m.request) AS request


_10


CROSS JOIN unnest(request.headers) AS header


`


**After** (read from the` log_attributes` map):


`
_10


SELECT


_10


timestamp,


_10


log_attributes\['request.method'\] AS method,


_10


log_attributes\['request.headers.x_real_ip'\] AS x_real_ip


_10


FROM logs


_10


WHERE source_name = 'edge_logs'


`


## Timeline#


Date Change


23 July 2026 Changelog published


23 September 2026` logs.all` endpoint removed for all projects


## Learn More#


- [Management API reference](https://supabase.com/docs/reference/api/introduction) : the` logs` endpoint and its parameters
- [Logs & querying documentation](https://supabase.com/docs/guides/telemetry/logs) : available sources and ClickHouse SQL query examples
