---
schema_version: "1.0.0"
document_id: "832b9053b5d3fb996cae617da9e082a0d3f7a29b22ff4d80efb92160d5ad92e6"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/how-to-reducto-parsing-elasticsearch-semantic-search"
published_at: "2025-06-05T12:00:00+00:00"
first_seen_at: "2026-07-22T11:10:28.155558+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:6eb64c5f5ab63acb18173eb8ad1379d21dd1629e5b6f6fd63fab63ef56177346"
---

# How to use Reducto parsing with Elasticsearch for Semantic Search

Parsing is the bottleneck in most[RAG](https://www.elastic.co/search-labs/blog/retrieval-augmented-generation-rag) pipelines. Scanned PDFs and spreadsheets can be messy, and poor inputs lead to incomplete retrieval, hallucinations, and brittle results. Nearly 80%** of enterprise knowledge is trapped in these formats, and traditional OCR flattens structure and meaning.


Addressing this challenge requires advanced parsing techniques, combining traditional OCR and vision-language models (VLMs) to interpret document layout and content, and generating structured, LLM-ready chunks. This post will explore such a hybrid approach and demonstrate how[Reducto's](https://reducto.ai/) document parsing API can be integrated with Elasticsearch for[semantic search](https://www.elastic.co/search-labs/blog/introduction-to-vector-search) .


## Why parsing is still a large challenge


Traditional OCR and basic text extraction methods aim to produce a "flat" view of a document. This might be fine for copying and pasting, but it’s disastrous for search.


Imagine flattening a financial report or a complex form into a single string of text.


- **Relationships between tables, headers, footnotes, and body text** vanish.
- **Contextual clues** about layout, importance, and hierarchy are lost.
- **Retrieval systems** that rely on embeddings or keyword search no longer have a strong signal to work with.


Rather than sticking to the conventional pipeline of "OCR everything → dump to Elastic,"[Reducto](https://reducto.ai/) designed a new approach: one that **preserves the structure, context, and meaning** of real-world documents.


## Reducto’s “vision‑first” approach


Instead of treating documents as just text,[Reducto’s Parsing API](https://docs.reducto.ai/parsing/options) treats them as **visual objects with contextual meaning** .


Our parsing pipeline combines:


- **Traditional OCR** : High-accuracy text extraction at the character level
- **Vision-Language Models (VLMs)** : Deep understanding of visual context—tables, multi-column layouts, embedded forms, and more


Reducto's hybrid approach leads to significant improvements over traditional text-only parsers in benchmarks (for example, some implementations show over 20 percentage points better performance on datasets like[RD-TableBench](https://reducto.ai/blog/rd-tablebench) , an open-source table parsing benchmark).


By analyzing both the content and the visual structure of documents, Reducto generates:


- Accurate bounding boxes (coordinates of a given element on the page, used for citations)
- Segmented layout types for each block (headers, tables, text, figures, etc.)
- LLM-ready chunks you can customize


The result is a parsed output that retains the full richness of the original document—ready for retrieval pipelines that require splitting and context embeddings.


### Introducing Agentic OCR: Multi‑pass self‑correction


Even with the best models, parsing errors happen. Complex scans, handwritten notes, or multi-layered tables can throw off a one-shot parser.


To solve this, we introduced **Agentic OCR** —a multi-pass self-correction framework​.


Think of it as a “human-in-the-loop” process—except there’s no human needed. This approach dramatically improves real-world robustness, ensuring that even complex, previously unseen documents are parsed cleanly and accurately.


## Example: Using Reducto APIs with Elasticsearch


Parsing isn’t the end goal. Search is.


Reducto customers often integrate parsed outputs directly into **Elastic Search** to power internal knowledge bases, intelligent document retrieval, or retrieval-augmented generation (RAG) systems.


Here’s a high-level outline:


1. **Parse** : Use[Reducto’s API](https://docs.reducto.ai/api-reference/parse) to parse documents into structured JSON, with customizable chunking options.
2. **Embed + Index** : We’ll use[ELSER](https://www.elastic.co/docs/explore-analyze/machine-learning/nlp/ml-nlp-elser) , a sparse retrieval model that comes built-in with Elasticsearch and makes running semantic search very simple, to generate weighted tokens in this example. We’ll store the embeddings and chunk metadata in Elasticsearch for semantic retrieval.
3. **Retrieve** : Given a query, fetch the most relevant chunks from the index.


If you’re interested in learning more about sparse vectors and why they are a great option for semantic search, read[this blog](https://www.elastic.co/search-labs/blog/sparse-vector-embedding) to learn more.


### **🧾 1. Parse & chunk with Reducto (via API)**


In order to get access to a Reducto API Key, head to[reducto.ai](http://reducto.ai/) and hit “Get started.” We’ll set you up with a free trial key.


Install our[Python SDK](https://github.com/reductoai/reducto-python-sdk) - the easiest way to interact with our APIs.


```text
python  pip install reductoai
```


Then, call our parsing function. There are some chunking-related[configurations](https://docs.reducto.ai/api-reference/parse#body-options) to take note of:


- [options.chunk_mode](https://docs.reducto.ai/api-reference/parse#body-options-chunking-chunk-mode) : choose how you’d like your chunks to be configured. For example, by pages, by block, by variable (char length), etc.
- [options.chunk_size](https://docs.reducto.ai/api-reference/parse#body-options-chunking-chunk-size) : how long in character length you’d like your chunks to be around. Defaults to 250-1500 characters.


We recommend that RAG use cases stick to Reducto’s default settings of “variable” chunk mode, as it is the most flexible for capturing and grouping data that is most semantically relevant. When defaulting, there is no need to set a specific configuration in your call.


Refer to Reducto’s[API documentation](https://docs.reducto.ai/api-reference/parse) for more details on configurations you can play with using the Parse endpoint. You can also try out our[UI playground](https://studio.reducto.ai/) and see all the options under “Configure Options”.


```text
python  from reducto import Reducto
from getpass import getpass


# Parse documents using Reducto SDK
reducto_client = Reducto(api_key=getpass("REDUCTO_API_KEY"))
upload = reducto_client.upload(file=Path("sample_report.pdf"))
parsed = reducto_client.parse.run(
document_url=upload,
options={ # Optional configs would go here
"chunking": {
"chunk_mode": "variable",
"chunk_size": 1000,
}
}
)


# Parsed output
print(f"Chunk count: {str(len(parsed.result.chunks))}")
print(parsed.result.chunks)
```


Using our sample equity research report, available[here](https://cdn.reducto.ai/JAZZ-Equity-Research-Report.pdf) , the parsed output would look similar to the below:


```text
python  Chunk count: 32


ResultFullResultChunk(blocks=[ResultFullResultChunkBlock(bbox=BoundingBox(height=0.022095959595959596, left=0.08823529411764706, page=1, top=0.04671717171717172, width=0.22549019607843138, original_page=1), content='Goldman Stanley', type='Title', confidence='high', image_url=None), ...])
```


### **📐 📦 2. Embed and index with ELSER**


Create your[Elastic Cloud Serverless](https://www.elastic.co/cloud/serverless) cluster or pick a deployment method of[your choice](https://www.elastic.co/elasticsearch) .


Install the Elasticsearch Python client.


```text
python  pip install elasticsearch
```


Initialize the client and connect to your Elasticsearch instance.


```text
python  from elasticsearch import Elasticsearch, exceptions


# Create Elasticsearch Client
es_client = Elasticsearch(
getpass("ELASTICSEARCH_ENDPOINT"),
api_key=getpass("ELASTIC_API_KEY")
)


print(es_client.info())
```


Create the ingest pipeline to generate embeddings using ELSER and apply to our new index.


```text
python  INDEX_NAME = "reducto_parsed_docs"


# Drop index if exists
es_client.indices.delete(index=INDEX_NAME, ignore_unavailable=True)


# Create the ingest pipeline with the inference processor
es_client.ingest.put_pipeline(
id="ingest_elser",
processors=[
{
"inference": {
"model_id": ".elser-2-elasticsearch",
"input_output": {
"input_field": "text",
"output_field": "text_semantic"
}
}
}
]
)


print("Finished creating ingest pipeline")


# Create mappings with sparse_vector for ELSER
es_client.indices.create(index=INDEX_NAME, body={
"settings": {"index": {"default_pipeline": "ingest_elser"}},
"mappings": {
"properties": {
"text": { "type": "text" },
"text_semantic": { "type": "sparse_vector" }
}
}
})


print("Finished creating index")
```


Index each Reducto chunk into Elasticsearch, for which the Elastic Inference Service will generate the embedding via the ingest pipeline.


```text
python  # Index each chunk
for i, chunk in enumerate(parsed.result.chunks):
doc = {
"text": chunk.embed
}
es_client.index(index=INDEX_NAME, id=f"chunk-{i}", document=doc)
```


### **🔍 3. Retrieval**


Perform a sparse vector query using the inference service to generate the embedding for the query. This executes a query consisting of the sparse vectors built by ELSER.


```text
python  # Use semantic text search
response = es_client.search(
index=INDEX_NAME,
query={
"sparse_vector":{
"field": "text_semantic",
"inference_id": ".elser-2-elasticsearch",
"query": "What was the client revenue last year?"
}
}
)


# Print results
for hit in response["hits"]["hits"]:
print(f"Score: {hit['_score']:.4f}")
print(f"Text: {hit['_source']['text'][:300]}...\n")
print(f"ELSER Embedding: {hit['_source']['text_semantic']}...\n")
```


When querying the chunks of our sample equity research report, the results might look something like this, containing a cosine similarity score along with the corresponding chunk content:


```text
python  Score: 13.0095
Text: # Operations (cont.)


Figure 2 - Jazz Historical and Projected Revenue Mix by Product...


ELSER Embedding: {'studio': 0.5506277, 'copyright': 0.29489025, 'def': 1.0905615, ... }


Score: 12.0828
Text: # Jazz Pharmaceuticals (JAZZ) (cont.)


## Strong Q1 FY14 & Multiple Catalysts Ahead (cont.)...


ELSER Embedding: {'##arm': 0.12544458, 'def': 1.4090042, '##e': 0.48080343, ... }
.
.
.
```


Elasticsearch is a leading[vector database](https://www.elastic.co/search-labs/blog/elastic-vector-database-practical-example) , delivering performance at scale for AI applications. It is particularly strong at handling vast embeddings datasets with its distributed architecture, and hybrid search quality which blends vector similarity with keyword relevance. Reducto complements Elastic's capabilities by providing structured, LLM-ready chunks, ensuring that Elasticsearch's powerful indexing and search retrieval operate on the highest-quality input, leading to superior AI agent performance and more accurate generative outputs.


## Conclusion


Enterprises have spent years building search systems on incomplete or flattened document data— and now, new technologies are emerging to unlock deeper insights from that information. Reducto’s APIs are suitable for companies across all industries, particularly those that require high accuracy, such as finance, healthcare, and legal.


Reducto’s vision-first, agent-driven parsing pipeline offers a way forward:


- Preserve context, structure, and meaning
- Feed richer, more accurate inputs into vector storage and retrieval systems like Elasticsearch
- Enable more reliable, hallucination-free RAG and search experiences


You can try[Reducto](https://reducto.ai/) today at the[Reducto Studio](https://studio.reducto.ai/) , or[get in touch](https://reducto.ai/contact) to learn more about full integrations. If you'd like to build this app and play with the code, you can find instructions in this[notebook](https://colab.research.google.com/drive/1IyhexwTSihjh-45zQGmy72yUmaRMnKFp?usp=sharing) .


With Reducto and an Elasticsearch vector database, enterprise knowledge is searchable and understandable.


**Source:[https://researchworld.com/articles/possibilities-and-limitations-of-unstructured-data](https://researchworld.com/articles/possibilities-and-limitations-of-unstructured-data)
