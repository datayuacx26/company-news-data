---
schema_version: "1.0.0"
document_id: "c53415d77d2819dbb7f5162906bc3b423911017d8ab7cde3a0c3d0782a02899a"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/devrel-newsletter-august-2026"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T21:20:39.411797+00:00"
fetched_at: "2026-08-13T21:20:42.205031+00:00"
content_hash: "sha256:9f9f66da90815056fa92023d92451ca236ad7fd37c628c9b78a184e8afd319d0"
---

# Elastic community newsletter — August 2026

# Elastic community newsletter — August 2026


By


[Elastic DevRel team](https://www.elastic.co/blog/author/elastic-devrel-team)


August 13, 2026


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


Hello from the Elastic DevRel team! In this newsletter, we cover version 9.5 of Elasticsearch, the latest blogs and videos, and upcoming events.


## What’s new?


Version 9.5 of Elasticsearch and the Elastic Stack promotes native PromQL, Dashboards API, Cases as Data, and natural language authoring to general availability. 9.5 also introduces Columnar Mode, VectorDB index mode, a new multimodal


semantic


field, and Elastic Agent Builder tracing in technical preview.


### Columnar Mode: A new storage foundation


[Columnar Mode](https://www.elastic.co/search-labs/blog/elasticsearch-columnar-storage) is a new index mode available in technical preview. It stores each field once in the column store with no inverted index by default. The result is a smaller storage footprint and a faster foundation for analytical queries and long data retention.


Built on Columnar Mode, Columnar Logs is the first profile designed for log data. It keeps a single inverted index on the


message


field, so full-text search stays fast while storing all other fields in columnar format. Storage use is significantly lower than standard logs without changing the search experience. Both modes are opt-in and existing indices are not affected.


### Vector search: VectorDB index mode and auto-calibration


Two technical preview features reduce the setup and tuning work for vector search.


VectorDB index mode sets up a vector index with a single setting. It applies defaults already tuned for vector workloads, including quantization, merge policy, and cache loading, so there is nothing to configure manually:


```text
PUT my-index
{
"settings": {
"index": {
"mode": "vectordb_document"
}
}
}


```


[Auto-calibration for DiskBBQ vector search](https://www.elastic.co/search-labs/blog/vector-quantization-auto-calibration-diskbbq) automatically sets quantization depth, preconditioning, and oversampling by analyzing the actual vectors in the index. The tuning that normally requires expertise and repeated testing now happens on its own:


```text
PUT /my_vector_index
{
"mappings": {
"properties": {
"my_vector_field": {
"type": "dense_vector",
"dims": 1536,
"index_options": {
"type": "bbq-disk",
"auto_calibration": true
}
}
}
}
}
```


Together, these two features mean a vector search index can be created and tuned with minimal configuration.


### Multimodal semantic search: Images, audio, video, and PDFs


The new


semantic


field extends semantic search to images, audio, video, and PDF files. It works the same way as


semantic_text


for text: Define a field, index your content, and query. Embeddings are generated at ingest time automatically.


You can mix content types in a single field. A document can hold a text description, an image, and an audio clip together. A text query then retrieves the best match across all of them. To search by image rather than text, send the image as a query and Elasticsearch generates the embedding on the fly.


The feature is powered by the


jina-embeddings-v5-omni


model, which places text, images, audio, video, and PDFs into a shared vector space. It is available as a technical preview in Elasticsearch 9.5 and Elastic Cloud Serverless. Read the


[full blog post](https://www.elastic.co/search-labs/blog/semantic-field-multimodal-search-elasticsearch) for setup details and code examples.


### Agent Builder: Tracing, human approvals, and faster responses


Elastic 9.5 adds three new Agent Builder capabilities.


Agent Observability and Monitoring (technical preview) records every LLM call, tool invocation, and reasoning step as OpenTelemetry (OTel) in Elasticsearch. Teams can inspect token usage, find timing bottlenecks, and debug agent behavior using the same Kibana they already use for operational data. A built-in


agent-builder-traces


skill lets you query this data in plain language. Custom dashboards and cost-threshold alerts can be built directly against the trace index.


Human-in-the-loop approvals let an agent pause and wait for a person to confirm a sensitive action before continuing. Each approval or rejection is recorded in an audit trail. Approval requests can be sent to external tools, such as Slack, so the decision does not require the approver to be in Kibana.


Agent Builder also delivers faster responses at lower token cost by routing specific tasks to faster models and removing redundant LLM calls. These improvements apply automatically to all Agent Builder users with no configuration change needed.


### Prometheus and PromQL now generally available


Native Prometheus and PromQL support is now generally available (GA). Teams can point existing Grafana dashboards and Prometheus tooling at Elastic and run PromQL queries alongside ES|QL with no changes to their existing setup. A migration tool, also now GA, helps


[bring Grafana and Datadog dashboards and alerts into Kibana](https://www.elastic.co/observability-labs/blog/grafana-elastic-kubernetes-dashboard-migration) .


The new ES95 codec stores metrics at roughly three bytes per sample (about 20% lower than 9.4) and significantly more efficiently than a plain Prometheus storage backend. Elastic also now supports all OpenTelemetry metric temporalities, so any OTel-instrumented application can send metrics over OTLP exactly as emitted with no workarounds.


### Alert Zero, Workflows, and rule version history


The goal of Alert Zero is an alert queue that contains only what matters. In 9.5, Attack Discovery works toward that goal by investigating alerts the way an analyst would; it checks entity risk, searches raw events, and looks for supporting evidence before calling something an attack. When it finds a coverage gap, it drafts an ES|QL detection rule for analyst review. Attack Discovery can now run automatically via an Elastic Workflow on a schedule or manually.


Elastic Workflows in 9.5 adds versioning so that teams can see who changed a workflow, compare any two versions, and roll back in one click. A new visual mode shows the workflow as a graph with its triggers, steps, and logic visible alongside the YAML. Human-in-the-loop steps can now send approval requests to Slack, so the workflow can pause and wait for a decision from outside Kibana.


Detection rule version history (technical preview) keeps a full log of every saved rule state, including who made each change and when. Teams can view any past version, compare it to the one before, and restore it in a single action. This is useful for finding out when a rule stopped generating alerts and for compliance frameworks that require evidence of change control.


## Blogs, videos, and interesting links


-


**NVIDIA inference:**


Jan Kazlouski walks us through


[setting up NVIDIA-hosted models in Elasticsearch](https://www.elastic.co/search-labs/blog/elasticsearch-nvidia-inference) with one API key and a model ID.


-


**Jina On-Prem:**


Scott Martens introduces


[Jina On-Prem](https://www.elastic.co/search-labs/blog/on-prem-ai-jina-embedding-models) , a fully self-contained installation suite for Jina AI’s high-performance models.


-


**AutoOps:**


Explore the


[redesigned AutoOps experience](https://www.elastic.co/search-labs/blog/autoops-elasticsearch-cluster-monitoring-redesigned) for Elastic Cloud Hosted deployments and Cloud Connect clusters with Ori Shafir and Arnon Stern.


-


**Agent memory:**


Noam Schwartz explains


[how we built a persistent agent memory layer on Elasticsearch](https://www.elastic.co/search-labs/blog/agent-memory-elasticsearch) with 0.89 recall and zero tenant leaks.


-


**Agentic SOC:**


Discover the five-step optimization loop that allowed us to


[cut AI agent LLM calls by 60%](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale) with Aaron Jewitt.


-


**Kibana Workflows:**


Learn how to


[run daily ML forecasts on disk usage](https://www.elastic.co/observability-labs/blog/disk-capacity-forecasting-kibana-workflows) and get Slack alerts listing which hosts will hit capacity and when with Valeriy Khakhutskyy.


**Check out these videos:**


-


[Search any video by what's on screen (Python + Elasticsearch)](https://youtu.be/2fTtXYbhNI8?si=C2IPS10j_H26TK5_) by JP Hwang


-


[Every search algorithm explained in 30 minutes (beginner to advanced)](https://youtu.be/_dOWlgFXu1E?si=wWdKi1DCIsuv1_PK) by Jon Avezbaki


-


[7 ways to query a Kafka Stream (and the trade-offs of each)](https://youtu.be/l2mg3s_2QlE?si=JyS1EiFrZsq656cg) by Viktor Gamov


**Featured blogs from the community:**


-


[Managing Elasticsearch Reindex at Scale: Performance, Reliability, and Observability](https://blog.palantir.com/managing-elasticsearch-reindex-at-scale-performance-reliability-and-observability-cf948d0efd47) by Palantir


-


[The hidden journey of a JSON document inside Elasticsearch](https://www.linkedin.com/pulse/hidden-journey-json-document-inside-elasticsearch-debraj-maity-ofm9c/) by Debraj Maity


-


[Why Elasticsearch doesn't read your documents anymore](https://www.linkedin.com/pulse/why-elasticsearch-doesnt-read-your-documents-anymore-franchini-gumoe/) by Gautier Franchini


## Upcoming events


**Learn Elastic at no cost:**


Explore


[self-paced modules](https://www.elastic.co/training/) to build your Elastic skills.


**Elastic{ON} tour**


, the one-day Elastic conference series around the world, is back.


[Register and join us](https://www.elastic.co/events/elasticon) in:


-


**Mumbai**


—


September 30


-


**New York**


—


October 8


-


**Amsterdam**


—


October 20


-


**San Francisco**


—


November 4


-


**London**


—


February 25


-


**Singapore**


—


March 23


**Want to speak at Elastic{ON}?**


The call for presentations is now open! We're looking for 30-minute technical talks covering Elasticsearch, Kibana, search, observability, security, and real-world use cases. Whether you're new to Elastic or an experienced user, we'd love to hear your ideas.


[Submit your proposal today.](https://sessionize.com/elasticon-tour/)


Join your


[local Elastic User Group](https://community.elastic.co/) chapter for the latest news on upcoming events! You can also find us on


[meetup.com](https://www.meetup.com/pro/elastic/) . If you’re interested in presenting at a meetup, send an email to


meetups@elastic.co .


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all. In this blog post, we may have used or referred to third party generative AI tools, which are owned and operated by their respective owners. Elastic does not have any control over the third party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use. Elastic, Elasticsearch, and associated marks are trademarks, logos or registered trademarks of elasticsearch B.V. in the United States and other countries. All other company and product names are trademarks, logos or registered trademarks of their respective owners.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
