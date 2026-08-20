---
schema_version: "1.0.0"
document_id: "09d42a8e111b20d41e4ec7e34ed2d20cfc9d01b5b37200de6d6d2f7cc99546e2"
company_key: "yc-helixdb"
company: "HelixDB"
source_id: "yc-helixdb-news-import-f26087e13605"
canonical_url: "https://www.helix-db.com/blog/mcp-guide-native-agent-discovery-with-helixdb"
published_at: "2025-08-15T13:01:33.549+00:00"
first_seen_at: "2026-07-24T11:14:24.417435+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:67ebbb879b96002f2cb3fa7a474d33e85f6f0c8bdc92b5d36bb9f073a4e0589a"
---

# MCP Guide - Native Agent Discovery with HelixDB

# Introduction


Build agentic graph queries that evolve step-by-step without rebuilding from scratch.


HelixDB's MCP endpoints provide a stateful, incremental query session via a` connection_id` , so each call extends the same traversal.


This allows agents to chain and build complex traversal and filter queries over time while being able to view intermediate results.


Stream results with` mcp/next` or batch with` mcp/collect` .


You can also:


-


Dynamically pass lists created at runtime


-


Toggle **AND /** **OR** property groups


-


Filter by temporary traversals


-


Run multiple traversals concurrently and asynchronously using multiple connection IDs


Initialize each traversal with its own` connection_id` to keep them separate and easy to manage.


# MCP Tools


These tools are used to manage the MCP connection and interact with the MCP endpoints.


## Initialization` init`


**Endpoint:**` mcp/init`


Creates a new MCP connection, which is a blank traversal.


**Request Format**


```text
{"connection_id": "<connection-id>"}
```


**Response Format**


```text
"<connection-id>"
```


## Iterate Results` next`


**Endpoint:**` mcp/next`


Returns the next item in the MCP traversal results.


**Response Format**


```text
{<node-edge-data>}
```


## Collect Results` collect`


**Endpoint:**` mcp/collect`


Collects all the results from the MCP traversal.


**Request Format**


```text
{
"connection_id": "<connection-id>",
"range": {"start": <start-index>, "end": <end-index>},
"drop": <true-false>
}
```


**Optional Parameters**


-


` range` *(optional)* – range of results to collect (default:` {"start": 0, "end": -1}` )


-


` drop` *(optional)* – resets the traversal after collecting results (default:` true` )


**Response Format**


```text
[
{<node-edge-data>},
...
]
```


## Reset Connection` reset`


**Endpoint:**` mcp/reset`


Resets the MCP connection with the given connection ID to a blank traversal.


**Request Format**


```text
{"connection_id": "<connection-id>"}
```


**Response Format**


```text
"<connection-id>"
```


## Schema Context` schema_resource`


**Endpoint:**` mcp/schema_resource`


Returns the schema and query information of the database for LLM context.


**Request Format**


```text
{"connection_id": "<connection-id>"}
```


**Response Format**


```text
{
"schema": {
"nodes": [
{
"name": "<node-name>",
"properties": {
"<property-name>": "<property-type>",
...
}
},
...
],
"vectors": [
{
"name": "<vector-name>",
"properties": {
"<property-name>": "<property-type>",
...
}
},
...
],
"edges": [
{
"name": "<edge-name>",
"properties": {
"<property-name>": "<property-type>",
...
}
},
...
]
},
"queries": [
{
"name": "<query-name>",
"parameters": {
"<parameter-name>": "<parameter-type>",
...
},
"returns": [
"<return-variable-name>",
...
]
}
]
}
```


# MCP Traversals


These tools dynamically traverse the graph and retrieve nodes and edges.


They do **not** return results of the traversal directly — use` collect` or` next` for that.


## Retrieve Nodes` n_from_type`


**Endpoint:**` mcp/n_from_type`


Retrieves all nodes of a given type.


**Request Format**


```text
{"connection_id": "<connection-id>", "data": {"node_type": "<node-type>"}}
```


**Parameters**


- ` node_type` – the type of node to retrieve


## Retrieve Edges` e_from_type`


**Endpoint:**` mcp/e_from_type`


Retrieves all edges of a given type.


**Request Format**


```text
{"connection_id": "<connection-id>", "data": {"edge_type": "<edge-type>"}}
```


**Parameters**


- ` edge_type` – the type of edge to retrieve


## Out` out_step`


**Endpoint:**` mcp/out_step`


Traverses out from current nodes or vectors in the traversal with the given edge type to nodes or vectors.


**Request Format**


```text
{
"connection_id": "<connection-id>",
"data": {"edge_type": "<edge-type>", "edge_label": "<edge-label>"}
}
```


**Parameters**


-


` edge_type` – the type of edge to traverse out


-


` edge_label` – the target entity (` node` or` vec` )


## OutE` out_e_step`


**Endpoint:**` mcp/out_e_step`


Traverses out from current edges to their target nodes or vectors.


**Request Format**


```text
{"connection_id": "<connection-id>", "data": {"edge_label": "<edge-label>"}}
```


**Parameters**


- ` edge_label` – the target entity (` node` or` vec` )


## In` in_step`


**Endpoint:**` mcp/in_step`


Traverses into current nodes or vectors with the given edge type to nodes or vectors.


**Request Format**


```text
{
"connection_id": "<connection-id>",
"data": {"edge_type": "<edge-type>", "edge_label": "<edge-label>"}
}
```


**Parameters**


-


` edge_type` – the type of edge to traverse into


-


` edge_label` – the source entity (` node` or` vec` )


## InE` in_e_step`


**Endpoint:**` mcp/in_e_step`


Traverses into current edges to their source nodes or vectors.


**Request Format**


```text
{"connection_id": "<connection-id>", "data": {"edge_label": "<edge-label>"}}
```


**Parameters**


- ` edge_label` – the source entity (` node` or` vec` )


## Filter` filter_items`


**Endpoint:**` mcp/filter_items`


Filters the current state of the traversal.


**Request Format**


```text
{
"connection_id": "<connection-id>",
"data": {"filter": <filters>}
}
```


Where`` is a list of filters to apply.


The **property filter** uses OR for the outer list and AND for the inner list:


```text
"properties": [
[
{"key": "<property-name>", "operator": "<operator>", "value": "<property-value>"},
...
],
...
]
```


Operators:


Operator Description Operator Description


` ==` equals` !=` not equals


` >` greater than` >=` greater than or equal to


` <` less than` <=` less than or equal to


> **Note:** List-to-value and value-to-list comparisons use **OR** for each element in the list.
>
>
> This allows for using` contains` and` not_contains` operators via` ==` and` !=` with lists.


The **traversal filter** checks results of a traversal without modifying the current traversal:


```text
"filter_traversals": [
{
"tool_name": "<tool-name>",
"args": {
"<parameter-name>": "<parameter-value>",
"filter": { ... }
}
}
]
```


# MCP Search Tools


## SearchV` search_vector`


**Endpoint:**` mcp/search_vector`


Searches for vectors by their vector representation.


**Request Format**


```text
{
"connection_id": "<connection-id>",
"data": {
"vector": <array-of-floats>, "k": <limit>, "min_score": <min-score>
}
}
```


**Response Format**


```text
[
{<vector-data>},
...
]
```


## SearchV Text` search_vector_text`


**Endpoint:**` mcp/search_vector_text`


Searches for vectors by embedding the given text and performing HNSW search.


**Request Format**


```text
{
"connection_id": "<connection-id>",
"data": {
"query": "<text>", "label": "<vector-label>"
}
}
```


> **Warning:** Traversal will reset after search.


## Search BM25` search_keyword`


**Endpoint:**` mcp/search_keyword`


Searches for nodes, edges, or vectors by keyword representation.


**Request Format**


```text
{
"connection_id": "<connection-id>",
"data": {
"query": "<text>", "label": "<label>", "limit": <limit>
}
}
```


> **Warning:** Traversal will reset after search.


# Knowledge Graph Examples


We'll use the Python SDK to interact with MCP endpoints on a local HelixDB instance.


Schema:


```text
N::Triplet {
INDEX uuid: String,
summary: String,
predicate: String,
value: String,
created_at: Date DEFAULT NOW
}
E::Triplet_to_Embedding {
From: Triplet,
To: Triplet_Embedding,
Properties: {}
}
V::Triplet_Embedding {
embedding: [F64]
}
E::Triplet_to_Subject {
From: Triplet,
To: Entity,
Properties: {}
}
E::Triplet_to_Object {
From: Triplet,
To: Entity,
Properties: {}
}
N::Entity {
INDEX uuid: String,
name: String,
entity_type: String,
description: String,
created_at: Date DEFAULT NOW
}
```


Python SDK:


```text
from helix import Client
client = Client(local=True, port=6969)
```


> **Tip:** Click[here](https://docs.helix-db.com/documentation/sdks/helix-py) for more Python SDK info.


## Example 1 — Filter properties by a list of values


```text
# Initialize
connection_id = client.query('mcp/init', {})[0]
# Get all entities
client.query('mcp/n_from_type', {'connection_id': connection_id, 'data': {'node_type': 'Entity'}})
# Filter
client.query('mcp/filter_items', {
'connection_id': connection_id,
'data': {'filter': {
'properties': [
[
{'key': 'name', 'operator': '==', 'value': ['John', 'Jane']}
]
]
}}
})
# Collect
entities = client.query('mcp/collect', {'connection_id': connection_id})[0]
```


## Example 2 — Filter multiple properties by a list of values


```text
# Initialize
connection_id = client.query('mcp/init', {})[0]
# Get all entities
client.query('mcp/n_from_type', {
'connection_id': connection_id,
'data': {'node_type': 'Entity'}
})
# Filter by name and created_at date range
client.query('mcp/filter_items', {
'connection_id': connection_id,
'data': {'filter': {
'properties': [[
{'key': 'name', 'operator': '==', 'value': ['John', 'Jane']},
{'key': 'created_at', 'operator': '>=', 'value': "2025-01-01T00:00:00+00:00"},
{'key': 'created_at', 'operator': '<=', 'value': "2025-01-31T23:59:59+00:00"}
]]
}}
})
```


## Example 3 — Filter properties by multiple lists of values


```text
# Initialize
connection_id = client.query('mcp/init', {})[0]
# Get all entities
client.query('mcp/n_from_type', {
'connection_id': connection_id,
'data': {'node_type': 'Entity'}
})
# Filter by two separate created_at ranges (OR)
client.query('mcp/filter_items', {
'connection_id': connection_id,
'data': {'filter': {
'properties': [
[
{'key': 'created_at', 'operator': '>=', 'value': "2025-01-01T00:00:00+00:00"},
{'key': 'created_at', 'operator': '<=', 'value': "2025-03-31T23:59:59+00:00"}
],
[
{'key': 'created_at', 'operator': '>=', 'value': "2025-06-01T00:00:00+00:00"},
{'key': 'created_at', 'operator': '<=', 'value': "2025-09-30T23:59:59+00:00"}
]
]
}}
})
```


## Example 4 — Filter by traversal result properties


```text
# Initialize
connection_id = client.query('mcp/init', {})[0]
# Get all entities and filter by name
client.query('mcp/n_from_type', {
'connection_id': connection_id,
'data': {'node_type': 'Entity'}
})
client.query('mcp/filter_items', {
'connection_id': connection_id,
'data': {'filter': {
'properties': [[{'key': 'name', 'operator': '==', 'value': 'John'}]]
}}
})
# Traverse to triplets connected as subjects
client.query('mcp/in_step', {
'connection_id': connection_id,
'data': {'edge_type': 'Triplet_to_Subject', 'edge_label': 'node'}
})
```


## Example 5 — Filter by multiple traversal results


```text
# Initialize
connection_id = client.query('mcp/init', {})[0]
# Get all triplets
client.query('mcp/n_from_type', {
'connection_id': connection_id,
'data': {'node_type': 'Triplet'}
})
# Filter triplets by subject "John" AND object "Jane"
client.query('mcp/filter_items', {
'connection_id': connection_id,
'data': {'filter': {
'filter_traversals': [
{
'tool_name': 'out_step',
'args': {'edge_type': 'Triplet_to_Subject', 'edge_label': 'node'},
'filters': {'properties': [[{'key': 'name', 'operator': '==', 'value': 'John'}]]}
},
{
'tool_name': 'out_step',
'args': {'edge_type': 'Triplet_to_Object', 'edge_label': 'node'},
'filters': {'properties': [[{'key': 'name', 'operator': '==', 'value': 'Jane'}]]}
}
]
}}
})
```


## Example 6 — Collecting intermediate results


```text
# Initialize
connection_id = client.query('mcp/init', {})[0]
# Get all triplets
client.query('mcp/n_from_type', {
'connection_id': connection_id,
'data': {'node_type': 'Triplet'}
})
triplets = client.query('mcp/collect', {
'connection_id': connection_id, 'drop': False
})[0]
# Traverse to embeddings and collect
client.query('mcp/out_step', {
'connection_id': connection_id,
'data': {'edge_type': 'Triplet_to_Embedding', 'edge_label': 'vec'}
})
embeddings = client.query('mcp/collect', {
'connection_id': connection_id
})[0]
```


## Example 7 — Search by vector


```text
# Initialize
connection_id = client.query('mcp/init', {})[0]
# Traverse to embeddings
client.query('mcp/n_from_type', {
'connection_id': connection_id,
'data': {'node_type': 'Triplet'}
})
client.query('mcp/out_step', {
'connection_id': connection_id,
'data': {'edge_type': 'Triplet_to_Embedding', 'edge_label': 'vec'}
})
# Search for similar embeddings
client.query('mcp/search_vector', {
'connection_id': connection_id,
'data': {
'vector': [0.1, 0.2, 0.3, 0.4, 0.5],
'limit': 10,
'min_score': 0.7
}
})
# Traverse back to triplets
client.query('mcp/in_step', {
'connection_id': connection_id,
'data': {'edge_type': 'Triplet_to_Embedding', 'edge_label': 'node'}
})
```
