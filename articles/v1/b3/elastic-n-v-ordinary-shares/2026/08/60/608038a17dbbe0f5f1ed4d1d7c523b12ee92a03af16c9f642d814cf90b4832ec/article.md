---
schema_version: "1.0.0"
document_id: "608038a17dbbe0f5f1ed4d1d7c523b12ee92a03af16c9f642d814cf90b4832ec"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/ai-index-building-context-agents"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-11T22:28:52.768378+00:00"
fetched_at: "2026-08-11T22:28:54.327701+00:00"
content_hash: "sha256:5cef5465974970556ba65378e4a128b6f468e8fbf63bc7295bc9c5d9067cfa28"
---

# Building context in Elasticsearch: how AI Indices power smarter agents using fewer tokens

Agents burn tokens exploring your data before they answer anything, inspecting mappings, sampling documents, probing which index to use. Elasticsearch AI Indices let you precompute that work once and store it as a Knowledge Indicator (KI): a structured, searchable record agents retrieve directly instead of rediscovering from scratch. This walkthrough shows you how to build the full pipeline: create an AI Index, generate routing KIs with a Kibana Workflow, and wire them to any agent harness via a portable ES|QL skill. We've also provided a[notebook](https://github.com/elastic/elasticsearch-labs/tree/main/supporting-blog-content/building-context-technical-walkthrough-part-1) if you'd like to run it yourself end to end as you go through the examples in this blog. This is Part 1 in a blog series providing a technical walkthrough to managing your context through KIs and AI indices.


While AI indices will be included in future Stack releases, today we recommend using Serverless.


## How it works: AI Index, Kibana Workflows, and the query-ki skill


Building context in this walkthrough has three moving parts:


1.


An **AI Index** , where KIs live. It's a regular Elasticsearch index or data stream with a specific naming convention triggering component templates to configure the right mappings automatically.


2.


**Kibana Workflows** , which read from your data sources, run an LLM to structure content into KIs, and write those KIs into the AI Index.


3.


A` **query-ki**` **skill** , a skill that queries KIs directly from the AI Index using ES|QL, and that a chat agent can call as a tool.


### Prerequisites


This tutorial assumes you have:


1.


An Elasticsearch Serverless project. You can[sign up for a trial](https://cloud.elastic.co/registration?onboarding_token=search&cta=cloud-registration&tech=trial&plcmt=article%20content&pg=search-labs) if you don't have one.


2.


An API key to access your Elasticsearch project.


## Create sample indices for agent routing


First, we’ll need some sources. Sources can be data that already exists in your Elasticsearch indices, or external data accessed via connectors or ES|QL data sources.


For this blog, we’ll create some indices with example data. We’ll start with an example using three datasets:[BEIR/fiqa](https://huggingface.co/datasets/BeIR/fiqa) (financial),[beir-nfcorpus](https://huggingface.co/datasets/BeIR/nfcorpus) (biomedical/nutrition), and[beir-scifact](https://huggingface.co/datasets/BeIR/scifact) (scientific fact-checking). Each index is populated with its own` _meta.description` .


Here are the mappings we define for these indices:


```text
{
"beir-fiqa": {
"mappings": {
"_meta": {
"description": "FiQA: financial question answering corpus from StackExchange Finance community posts and web crawls. Covers investments, banking, taxes, and market analysis. BM25-only index."
},
"properties": {
"text": {
"type": "text",
"meta": {
"description": "Full document body text."
}
},
"title": {
"type": "text",
"meta": {
"description": "Document or article title."
}
}
}
}
}
}


{
"beir-nfcorpus": {
"mappings": {
"_meta": {
"description": "NFCorpus: biomedical information retrieval corpus from NutritionFacts.org. Contains nutrition science and medical research documents on diet, disease, and health interventions. BM25-only index."
},
"properties": {
"text": {
"type": "text",
"meta": {
"description": "Full document body text."
}
},
"title": {
"type": "text",
"meta": {
"description": "Document or article title."
}
}
}
}
}
}


{
"beir-scifact": {
"mappings": {
"_meta": {
"description": "SciFact: scientific fact-checking corpus of biomedical research abstracts used to verify factual claims in peer-reviewed literature. BM25-only index."
},
"properties": {
"text": {
"type": "text",
"meta": {
"description": "Full document body text."
}
},
"title": {
"type": "text",
"meta": {
"description": "Document or article title."
}
}
}
}
}
}
```


Then, using the above convenience scripts, load a handful of documents into each index with the[_bulk](https://www.elastic.co/docs/api/doc/elasticsearch/operation/operation-bulk) API.


Now imagine an agent with a question and the indices we’ve just created. The agent has no idea which one is relevant at the start. Without pre-computed context, it either performs exploratory lookups (mappings, test searches) to figure out which source to use, or searches all three and hopes the merged results contain something useful. Either approach costs tokens, and if you multiply that inefficiency across every query an agent makes, it adds up.


## Create your AI Index


Before generating any KIs, you need an index to store them. We call this an **AI Index** .


The naming convention is what triggers automatic configuration. Any index whose name starts with` ai-index-idx-` is a regular index;` ai-index-ds-` is a data stream. You’ll want to choose data streams for observability use cases, time series data, and when recency is important. Conversely, standard indices are a good choice for static data that will exist for a long while, where recency is not as much of a concern, and may need to occasionally be updated on demand. This naming convention is required for AI indices.


When Elasticsearch sees the` ai-index-` prefixes, it automatically applies component templates that configure the right mappings and settings.


Creating an AI Index is a single call:


```text
PUT ai-index-idx-my-corpus
```


To see exactly what the component templates applied, inspect the mappings:


```text
GET ai-index-idx-my-corpus/_mapping
```


The response shows the fields every AI Index gets out of the box:


```text
{
"ai-index-idx-my-corpus": {
"mappings": {
"properties": {
"@timestamp": {
"type": "date"
},
"attributes": {
"type": "flattened"
},
"content": {
"type": "text",
"fields": {
"semantic": {
"type": "semantic_text",
"inference_id": ".jina-embeddings-v5-text-small"
}
}
},
"description": {
"type": "text",
"fields": {
"semantic": {
"type": "semantic_text",
"inference_id": ".jina-embeddings-v5-text-small"
}
}
},
"references": {
"properties": {
"uri": {
"type": "keyword"
}
}
},
"tags": {
"type": "keyword"
},
"title": {
"type": "text",
"fields": {
"semantic": {
"type": "semantic_text",
"inference_id": ".jina-embeddings-v5-text-small"
}
}
},
"type": {
"type": "keyword"
}
}
}
}
}
```


` title` ,` description` , and` content` are each a` text` field with a` .semantic` sub-field of type[semantic_text](https://www.elastic.co/docs/reference/elasticsearch/mapping-reference/semantic-text) , supporting hybrid retrieval.


Data stream indices (` ai-index-ds-*` ) additionally carry a default 90-day data retention policy. This blog uses a standard index (` ai-index-idx-*` ).


## Index Metadata as a Knowledge Indicator


The target use case for this example is how the` query-index-metadata-ki` skill can route an agent to the correct Elasticsearch index, even when index or field names are vague. This reduces mistakes from choosing the wrong index or formulating queries based on incomplete schema exploration.


Since we're creating KIs for our own indices, we can give the LLM a head start: annotate index mappings with human-written` _meta.description` content. The workflow generates better KIs with more context to work from.


To address this, we'll manually create a[Kibana Workflow](https://www.elastic.co/docs/reference/kibana) that profiles each index and writes routing KIs into the AI Index. The workflow chains four steps:


**Step**


**Type**


**What it does**


` get_mapping`


` elasticsearch.request`


Read the mapping, including` _meta.description` and per-field descriptions.


` sample_docs`


` elasticsearch.search`


Pull a few real documents so the profile reflects actual value shapes.


` profile_index`


` ai.agent`


Generate a structured index profile as structured output.


` sink_index_ki`


` elasticsearch.bulk`


Write the profile into the AI Index as a KI.


Paste the following YAML into the[Workflows](https://www.elastic.co/docs/explore-analyze/workflows) editor:


```text
version: '1'
name: beir-index-profile-ki
description: Profile an index into an index-selection Knowledge Indicator.
enabled: true
tags:
- context-management
- index-selection


triggers:
- type: manual


consts:
indices:
- beir-fiqa
- beir-nfcorpus
- beir-scifact


steps:
- name: loop_indices
type: foreach
foreach: '{{ consts.indices | json }}'
iteration-on-failure:
continue: true
steps:
- name: get_mapping
type: elasticsearch.request
with:
method: GET
path: '/{{ foreach.item }}/_mapping'


- name: sample_docs
type: elasticsearch.search
with:
index: '{{ foreach.item }}'
size: 3
query:
match_all: {}


- name: profile_index
type: ai.agent
timeout: 120s
with:
message: >
You are a data steward building an INDEX PROFILE for an enterprise
data catalog. Downstream, an AI agent uses these profiles to decide
WHICH Elasticsearch index to query for a given user question -- this
is an index-SELECTION aid, not a place to answer the question itself.


You are given (a) the index name, (b) its Elasticsearch mapping
including human-written descriptions in `_meta.description` and each
field's `meta.description`, and (c) a few sample documents. Produce a
faithful, decision-useful profile. Rules:
- Ground everything in the provided mapping + samples. Never invent
fields, values, or purpose. If unknown, use an empty string/array.
- Optimize for routing: make it obvious what kinds of questions this
index can authoritatively answer, and what it canNOT.
- Prefer concrete field names and real example values from the
samples over vague phrasing.
- For joins, surface shared keys (e.g. *_id fields) that link this
index to sibling indices, since cross-index questions hinge on them.


Index name: {{ foreach.item }}


Elasticsearch mapping (JSON):
{{ steps.get_mapping.output | json }}


Sample documents (JSON):
{{ steps.sample_docs.output.hits.hits | map: '_source' | json }}
schema:
type: object
properties:
display_name:
type: string
description: A concise human-readable name for what this index represents (<= 8 words).
purpose:
type: string
description: 2-4 sentences describing what this index stores and its role. PRIMARY semantic surface for matching a question to this index.
answers_questions:
type: array
items:
type: string
description: 3-7 representative natural-language questions this index can authoritatively answer.
does_not_contain:
type: array
items:
type: string
description: 1-4 things a searcher might wrongly expect here but that live elsewhere, to prevent mis-routing.
key_fields:
type: array
items:
type: string
description: 3-10 of the most query-relevant fields as "field_name - what it is".
when_to_use:
type: string
description: A single crisp routing heuristic - when should an agent pick THIS index? (<= 30 words).
example_esql:
type: string
description: One realistic, runnable ES|QL query against this index answering one of answers_questions.
required:
- display_name
- purpose
- answers_questions
- key_fields
- when_to_use


- name: sink_index_ki
type: elasticsearch.request
with:
method: PUT
path: '/ai-index-idx-my-corpus/_doc/{{ foreach.item | url_encode }}'
body:
'@timestamp': '{{ "now" | date: "%Y-%m-%dT%H:%M:%S.%LZ" }}'
type: index_metadata_entry
title: '{{ steps.profile_index.output.structured_output.display_name | default: foreach.item }}'
tags:
- index-profile
- '{{ foreach.item }}'
attributes:
display_name: '{{ steps.profile_index.output.structured_output.display_name }}'
purpose: '{{ steps.profile_index.output.structured_output.purpose }}'
when_to_use: '{{ steps.profile_index.output.structured_output.when_to_use }}'
answers_questions: '{{ steps.profile_index.output.structured_output.answers_questions | json }}'
does_not_contain: '{{ steps.profile_index.output.structured_output.does_not_contain | json }}'
key_fields: '{{ steps.profile_index.output.structured_output.key_fields | json }}'
example_esql: '{{ steps.profile_index.output.structured_output.example_esql }}'
source_index: '{{ foreach.item }}'
content: >
=== SOURCE / PROVENANCE ===
This is an INDEX PROFILE for routing/index-selection.
Backing Elasticsearch index: {{ foreach.item }}
Inspect it directly with ES|QL:
FROM {{ foreach.item }} | LIMIT 10
=== WHAT THIS INDEX IS ===
{{ steps.profile_index.output.structured_output.purpose }}
Questions this index can answer: {{ steps.profile_index.output.structured_output.answers_questions | join: " | " }}
When to use this index: {{ steps.profile_index.output.structured_output.when_to_use }}
Example query:
{{ steps.profile_index.output.structured_output.example_esql }}
description: >
Index profile: {{ steps.profile_index.output.structured_output.display_name }}.
Does NOT contain: {{ steps.profile_index.output.structured_output.does_not_contain | join: "; " }}.
Key fields: {{ steps.profile_index.output.structured_output.key_fields | join: "; " }}.
```


Let's walk through what this workflow does. We loop over three specified indices with a` foreach` loop. For each:


1.


` get_mapping` fetches the Elasticsearch index mappings, including any` _meta.description` annotations we added earlier.


2.


` sample_docs` pulls 3 real documents. Concrete examples give the LLM much better signal than schema alone.


3.


` profile_index` calls` ai.agent` with the index name, mappings, and sample documents. The LLM returns structured output describing the index's purpose, key fields, and an example ES|QL query showing how to use it.


4.


` sink_index_ki` writes the result into the AI Index as a KI of type` index_metadata_entry` , keyed on the index name so re-runs are idempotent.


A few things to point out:


-


This workflow hard-codes a specific set of indices. In practice, you could derive the list from an index pattern or a dynamic source.


-


The` foreach` loop also runs iterations sequentially, which is fine for this guide but slow in production because each iteration involves an LLM call. For scale, use[workflow.executeAsync](https://www.elastic.co/docs/explore-analyze/workflows/steps/composition) or native parallel support. The[cheat sheet](https://www.elastic.co/docs/explore-analyze/workflows/reference/cheat-sheet) has tips on both.


-


In the` profile_index` step, the agent prompt is the special sauce. This is what shapes the accuracy and usefulness of the KIs.


-


Using[ai.prompt](https://www.elastic.co/docs/explore-analyze/workflows/steps/ai-steps#ai-prompt) can improve workflow efficiency (and cost) if you don’t need to load other tools.


-


Cost can be controlled in multiple ways. Richer prompts and structured output often result in higher token utilization, and of course the model you choose significantly impacts total costs. The[Elastic Inference Service](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis) (EIS) can be a great playground to test different models against the` profile_index` ’s` ai.agent` step to compare how different models stack up against each other when generating KIs.


### Query your AI Index to verify Knowledge Indicators


Once the beir-index-profile-ki workflow runs, query the AI Index directly in the Discover tab using the following ES|QL query to confirm what got written:


```text
FROM ai-index-idx-my-corpus
| WHERE type == "index_metadata_entry"
| KEEP title, content, description, attributes, tags
| LIMIT 10
```


This will result in the following output:


### Build a portable skill to retrieve AI agent context


Retrieval is a critical component in an AI index. A KI is a document in the AI Index, and finding one is a single ES|QL query. We package that query as a small, portable skill so any agent can call it, regardless of the harness it runs in.


We write the skill as a SKILL.md: a YAML header with a name and description, followed by markdown instructions. This is the same Agent Skills format that many harnesses, including Claude Code, LangChain's Deep Agents, and others, load directly.


The harness reads the header content up front, and only pulls in the full instructions when a question matches the description. The one thing the skill asks of the harness is a way to run ES|QL against Elasticsearch.


Here is a sample` query-index-metadata-ki` skill:


```text
---
name: query-index-metadata-ki
description: >-
Retrieve Knowledge Indicators (pre-computed context) from the Elasticsearch AI
Index before answering. Use it to find which index to search (routing profiles).
Trigger on any question that depends on choosing a data source.
allowed-tools: esql_query
---


# Retrieving Knowledge Indicators


Knowledge Indicators (KIs) live in Elasticsearch indices named `ai-index-*`.
Retrieve them by calling the `esql_query` tool with the query below. Substitute
the user's question for `<query>`, and `index_metadata_entry` as the `<ki_type>` for routing profiles.


```esql
FROM ai-index-idx-* METADATA _id, _index, _score
| WHERE type == "<ki_type>"
| FORK
(WHERE MATCH(content, "<query>") OR MATCH(description, "<query>")
| SORT _score DESC | LIMIT 20)
(WHERE MATCH(content.semantic, "<query>") OR MATCH(description.semantic, "<query>")
| SORT _score DESC | LIMIT 20)
| FUSE
| SORT _score DESC
| KEEP title, content, description, tags
| LIMIT 5
```


Ground your answer in what the query returns, and cite the KI titles you used. If
nothing relevant comes back, say so rather than guessing.
```


Let’s break down what the skill is doing:


-


We’re defining` index-metadata-entry` as a KI type/use case.


-


We’re performing a hybrid ES|QL search on our AI indices, filtering by the appropriate` type` using RRF as the default method to fuse results.


-


The KI results will directly ground the agent’s answer when determining what indices are relevant to the query.


Because the skill is just instructions plus a query, it travels wherever your agent does. You can point the same file at a Kibana Workflow agent, Claude Code, LangChain Deep Agents, or any other harness without changing a line of it.


### Connect your AI Index to an agent harness


We want to demonstrate how you can use AI indices to query your data with any harness. For these examples, we’ll use LangChain Deep Agents and an OpenAI-compatible key, but any other agent harness can be easily substituted in, including Elastic Agent Builder.


First, let’s create a baseline to see how an agent will perform without using KIs:


```text
# Example question: Is there scientific evidence that vitamin D supplementation prevents cancer?
import os
import sys
import time
from elasticsearch import Elasticsearch
from langchain_core.messages import AIMessage
from langchain_core.tools import tool
from langchain_openai import ChatOpenAI
from deepagents import create_deep_agent


if len(sys.argv) < 2:
sys.exit(f'Usage: python {sys.argv[0]} "your question"')


es = Elasticsearch(os.environ["ES_URL"], api_key=os.environ["ES_API_KEY"])


@tool
def esql_query(query: str) -> list[dict] | str:
"""Execute an ES|QL query against Elasticsearch and return the matching rows.


Args:
query: A complete ES|QL query string, e.g. 'FROM beir-fiqa | LIMIT 5'.
Full-text search syntax: WHERE MATCH(field, "value") — not field MATCH "value".
"""
try:
resp = es.esql.query(query=query, format="json")
cols = [c["name"] for c in resp["columns"]]
return [dict(zip(cols, row)) for row in resp["values"]]
except Exception as e:
return f"ES|QL error: {e}"


@tool
def get_mapping(index: str) -> dict:
"""Return the field mapping for an Elasticsearch index or pattern."""
return es.indices.get_mapping(index=index).body


baseline_agent = create_deep_agent(
model=ChatOpenAI(  # any OpenAI-compatible endpoint; configure via LLM_* env vars
base_url=os.environ.get("LLM_BASE_URL", "https://openrouter.ai/api/v1"),
model=os.environ.get("LLM_MODEL", "anthropic/claude-sonnet-4.5"),
api_key=os.environ["LLM_API_KEY"],
),
tools=[esql_query, get_mapping],
system_prompt=(
"You are a research assistant with access to three Elasticsearch indices: "
"beir-fiqa, beir-nfcorpus, and beir-scifact. "
"You do NOT know which index is relevant for a given question. "
"Use get_mapping to inspect an index's description and fields, "
"then query the most relevant one with esql_query. "
"Ground your answer strictly in what the queries return."
),
)


start = time.perf_counter()
result = baseline_agent.invoke(
{
"messages": [
{
"role": "user",
"content": sys.argv[1],
}
]
}
)
latency = time.perf_counter() - start


print("\n--- Tool calls ---")
for m in result["messages"]:
if isinstance(m, AIMessage) and m.tool_calls:
for tc in m.tool_calls:
print(f"  [{tc['name']}] {str(tc['args'])[:120]}")
total = sum(
len(m.tool_calls)
for m in result["messages"]
if isinstance(m, AIMessage) and m.tool_calls
)
print(f"Total: {total}\n")


print("--- Usage ---")
input_tokens = sum(
(m.usage_metadata or {}).get("input_tokens", 0)
for m in result["messages"]
if isinstance(m, AIMessage) and m.usage_metadata
)
output_tokens = sum(
(m.usage_metadata or {}).get("output_tokens", 0)
for m in result["messages"]
if isinstance(m, AIMessage) and m.usage_metadata
)
print(f"Tokens: {input_tokens + output_tokens} (input {input_tokens}, output {output_tokens})")
print(f"Latency: {latency:.2f}s\n")


print("--- Answer ---")
print(result["messages"][-1].content)
```


Here’s a modified example that could run the same agent, but now with the ability to search AI indices to return KIs:


```text
import os
import sys
import time
from elasticsearch import Elasticsearch
from langchain_core.messages import AIMessage
from langchain_core.tools import tool
from langchain_openai import ChatOpenAI
from deepagents import create_deep_agent
from deepagents.backends.filesystem import FilesystemBackend


if len(sys.argv) < 2:
sys.exit(f'Usage: python {sys.argv[0]} "your question"')


es = Elasticsearch(os.environ["ES_URL"], api_key=os.environ["ES_API_KEY"])


@tool
def esql_query(query: str) -> list[dict] | str:
"""Execute an ES|QL query against Elasticsearch and return the matching rows.


Args:
query: A complete ES|QL query string, e.g. 'FROM beir-fiqa | LIMIT 5'.
"""
try:
resp = es.esql.query(query=query, format="json")
cols = [c["name"] for c in resp["columns"]]
return [dict(zip(cols, row)) for row in resp["values"]]
except Exception as e:
return f"ES|QL error: {e}"


backend = FilesystemBackend(root_dir=".", virtual_mode=False)


agent = create_deep_agent(
base_url=os.environ.get("LLM_BASE_URL", "https://openrouter.ai/api/v1"),
model=os.environ.get("LLM_MODEL", "anthropic/claude-sonnet-4.5"),
api_key=os.environ["LLM_API_KEY"],
),
tools=[esql_query],
skills=["skills"],
backend=backend,
system_prompt=(
"You are a research assistant with access to several Elasticsearch indices. "
"You do NOT know which index is relevant for a given question. "
"Before searching, always use the query-ki skill with type 'index_metadata_entry' "
"to retrieve the routing profile for the right index, then query that index directly. "
"Ground your answer strictly in what the queries return and cite the KI you used for routing."
),
)


start = time.perf_counter()
result = agent.invoke(
{
"messages": [
{
"role": "user",
"content": sys.argv[1],
}
]
}
)
latency = time.perf_counter() - start


print("\n--- Tool calls ---")
for m in result["messages"]:
if isinstance(m, AIMessage) and m.tool_calls:
for tc in m.tool_calls:
print(f"  [{tc['name']}] {str(tc['args'])[:120]}")
total = sum(
len(m.tool_calls)
for m in result["messages"]
if isinstance(m, AIMessage) and m.tool_calls
)
print(f"Total: {total}\n")


print("--- Usage ---")
input_tokens = sum(
(m.usage_metadata or {}).get("input_tokens", 0)
for m in result["messages"]
if isinstance(m, AIMessage) and m.usage_metadata
)
output_tokens = sum(
(m.usage_metadata or {}).get("output_tokens", 0)
for m in result["messages"]
if isinstance(m, AIMessage) and m.usage_metadata
)
print(f"Latency: {latency:.2f}s\n")


print("--- Answer ---")
print(result["messages"][-1].content)
```


This agent will always query the KI indices to get the answer.


## How much do Knowledge Indicators reduce agent token usage?


Since we’re using agents, the results of these scripts are non-deterministic. However, when I ran these results against the query` Is there scientific evidence that vitamin D supplementation prevents cancer?` , both agents led to the same conclusion, but they took different paths to get there:


Baseline (No AI Index)


With AI Index


Total tool calls


12


8


` read_file` calls


0


2


` get_mapping` calls


3


0


` esql_query` calls


9


6


Total indices queried


2 (bounced between` beir-scifact` and` beir-nfcorpus` )


1 (` beir-nfcorpus` )


Tokens consumed


167,763


92,711


Latency


39.58s


36.15s


Answer


Grounded, correct


Grounded, correct


The KI answers were both grounded and correct, but an interesting datapoint is the fact that the overall tool usage and token utilization was smaller when using KIs (latency was roughly equivalent). Here’s how both paths went, side by side:


## Run the full AI Index pipeline in Serverless


This walkthrough offered a deep dive into the build-it-yourself version of AI indices and KIs. In production, you wouldn't hand-write these workflows; a setup agent would generate them, and a feedback loop would refine KIs from the agent's own traces. But the primitives are exactly what you just used: extract KIs with a workflow, store them in an AI Index, and retrieve them with a skill.


Managing context is key to a relevant and efficient agentic search system, and AI indices are a way to manage this context with the full power of the Elastic stack. Try it out in Serverless and let us know what you think in our[Discuss forums](https://discuss.elastic.co/top?period=monthly) or the` #stack-kibana` channel in our[Community Slack](https://elasticstack.slack.com/signup#/domain-signup) !
