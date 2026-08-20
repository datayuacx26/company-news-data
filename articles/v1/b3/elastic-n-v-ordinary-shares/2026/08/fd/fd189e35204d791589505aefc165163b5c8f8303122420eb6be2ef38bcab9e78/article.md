---
schema_version: "1.0.0"
document_id: "fd189e35204d791589505aefc165163b5c8f8303122420eb6be2ef38bcab9e78"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/whats-new-elastic-9-5-0"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T16:15:42.235326+00:00"
fetched_at: "2026-08-04T17:31:37.256081+00:00"
content_hash: "sha256:40d2b9eb5f961d77ea3e30e6e58a4d3d45b75b1f36994d440d7adfdccf8f13ca"
---

# Elastic 9.5: Columnar, VectorDB index mode & auto-calibration, and AI-driven alert triage

# Elastic 9.5: Columnar, VectorDB index mode & auto-calibration, and AI-driven alert triage


By


[Sarah Leslie](https://www.elastic.co/blog/author/sarah-leslie)


August 4, 2026


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


Today, we are pleased to announce the general availability (GA) of Elastic 9.5 as the latest version of the Elasticsearch Platform. This release includes a range of new features aimed to help developers do more with their data, build AI agents more confidently, and sharpen security operations with the introduction of new capabilities across


[Elasticsearch](https://www.elastic.co/enterprise-search) ,


[Elastic Observability](https://www.elastic.co/observability) , and


[Elastic Security](https://www.elastic.co/security) .


The Elasticsearch Platform


## What’s new in Elastic 9.5?


Elastic 9.5 continues the advancement of Elasticsearch in efficiency, visibility, and performance to help make enterprise data more accessible and useful. With Elastic 9.5, users can:


-


Store more, query faster, and retain longer with


[Columnar Mode](https://www.elastic.co/blog/whats-new-elastic-9-5-0#the-multi-signal-datastore-for-any-workload) ****


— Elasticsearch as a native columnar database


-


Skip the setup and index tuning with


[VectorDB index mode and auto-calibration](https://www.elastic.co/blog/whats-new-elastic-9-5-0#vector-database) —


****


vector search that works out of the box


-


Migrate Prometheus workloads to Elasticsearch with the GA of


****


[native Prometheus and PromQL support](https://www.elastic.co/blog/whats-new-elastic-9-5-0#native-prometheus-and-promql-support,-now-ga)


-


Help reach


[Alert Zero](https://www.elastic.co/blog/whats-new-elastic-9-5-0#alert-zero:-from-alert-queue-to-validated-threats) ;


****


the SOC's version of inbox zero — a queue worked down to what actually matters, reached by agents and analysts together


-


Build smarter AI agents with the latest Elastic


[Agent Builder enhancements](https://www.elastic.co/blog/whats-new-elastic-9-5-0#more-agent-builder-enhancements!) , including Agent Observability and Monitoring and advanced human-in-the-loop approvals


Read more about these and additional feature highlights below.


### Elasticsearch


Elastic 9.5 brings advancements designed to help every user move faster, operate with greater confidence, and get more value from their data with less friction. This release innovates on multiple aspects of the Elasticsearch Platform from storing and querying data to building and governing AI agents alongside Kibana dashboard enhancements. Read on for details.


#### The multi-signal datastore for any workload


Elastic is pleased to announce the technical preview of


[Columnar Mode](https://www.elastic.co/search-labs/blog/elasticsearch-columnar-storage) , an opt-in indexing mode that stores each field once in a column store with no inverted index by default, delivering meaningfully smaller storage footprints and a foundation for faster indexing, analytical queries, and longer data retention. It ships alongside existing index modes with zero changes to APIs, dashboards, or integrations. Building on that foundation,


**Columnar Logs**


is the first specialized profile purpose-built for log data; it keeps a single inverted index on the message field, so full-text search stays fast while storing everything else fully columnar. The result is significantly less storage than logs use today without changing the search experience teams rely on. Both modes are opt-in and existing indices are untouched.


#### Vector database


Teams building retrieval for retrieval augmented generation (RAG) applications and agentic AI usually have to make many configuration decisions at index time, and that work grows at production scale. We're introducing these capabilities to take the setup and index tuning off your plate, so vector search works out of the box.


**VectorDB index mode**


delivers high-performance vector search with a single setting; there’s no manual configuration required. It applies defaults already optimized for vectors, tuning quantization, merge policy, and cache loading for you, so vector search is simpler to stand up and performs out of the box.


[Auto-calibration](https://www.elastic.co/search-labs/blog/vector-quantization-auto-calibration-diskbbq) **for DiskBBQ vector search**


automatically configures quantization depth, preconditioning, and oversampling based on statistical analysis of the vectors in the index. The tuning that normally takes expertise and experimentation now happens on its own, using unique algorithms developed at Elastic.


[Multimodal semantic](https://www.elastic.co/search-labs/blog/semantic-field-multimodal-search-elasticsearch) **search makes image search easier for developers.**


Searching images normally means setting up embeddings, ingesting and embedding images, and embedding the query. The new


semantic


field simplifies these steps, so users can search images as easily as they search text with


[semantic_text](https://www.elastic.co/search-labs/blog/elasticsearch-semantic-text-ga) .


#### More Agent Builder enhancements!


Elastic 9.5 gives developers greater visibility and control when building AI agents for production.


[Agent Observability and Monitoring](https://www.elastic.co/search-labs/blog/opentelemetry-tracing-agent-builder) , now available in technical preview, traces large language model (LLM) calls, tool invocations, and reasoning steps as OpenTelemetry (OTel) in Elasticsearch while


**human-in-the-loop approvals**


gate sensitive actions and record each decision in an audit trail. Developers can also


**create skills, tools, and workflows directly from chat**


by describing the capability they need, such as a workflow to generate a dashboard. Agent Builder then drafts, names, and saves the configuration without requiring them to leave the conversation, reducing setup time and the need to understand the underlying configuration model.


#### AI-native Kibana


With 9.5, our


**Dashboards & Visualizations API**


becomes generally available, providing platform teams with a stable and supported way to create, update, and manage dashboards and visualizations in code. Additionally,


**dashboards in chat**


is now GA with improved chat quality as well as controls creation and delivers higher speed. These advancements dramatically reduce time-to-insight and eliminate the need for users to build dashboards manually, expediting incident investigation, where speed to a clear visual directly shortens time-to-resolution. And, for use cases where speed is paramount, toggle-on


**Fast Mode UI**


allows STATS-based queries in Dashboards and Discover to run on a sampled dataset instead of scanning full data, extrapolating results back to real scale while keeping accuracy very close to exact.


#### Enhanced automation where your data lives


Elastic Workflows in 9.5 makes automations faster to build and shows exactly what a workflow will do before it runs.


[Natural language authoring](https://www.elastic.co/search-labs/blog/ai-workflow-automation-natural-language) is now generally available and on by default, so teams can describe the automation they want in plain language that’s generated automatically. Versioning is built in, tracking every change, letting teams compare any two versions, and rolling back to a working one in a click, so there’s always a record of who changed what and when. Visual mode shows a workflow as a graph with its triggers, steps, branches, and logic visible at a glance alongside the YAML; drag-and-drop editing is coming next. And human-in-the-loop now reaches outside Kibana. When a workflow needs a person to approve or weigh in, it pauses and sends the request to tools like Slack. Automation handles the routine while teams stay on top of the decisions that need judgment. These are Elasticsearch Platform capabilities, available across Search, Observability, and Security.


### Elastic Observability


Elastic 9.5 delivers observability upgrades designed to make unified monitoring faster to adopt and easier to scale. Whether you're migrating from Prometheus, onboarding Kubernetes, or connecting cloud and SaaS data, it's easier to get started with less operational overhead. Improved SRE workflows, managed integrations, and AI-ready context help teams move from detection to diagnosis faster.


#### Native Prometheus and PromQL support, now GA


With 9.5, it is easier than ever to consolidate observability with best-in-class metrics. With the


[Prometheus remote-write endpoint and native PromQL support](https://www.elastic.co/observability-labs/blog/prometheus-metrics-elasticsearch-getting-started) embedded directly in ES|QL, teams can point existing Grafana dashboards and queries at Elastic with minimal migration effort — no need to abandon the query language and workflows they've spent years building. A new


[migration tool](https://www.elastic.co/observability-labs/blog/migrate-datadog-grafana-dashboards-alerts-to-kibana) is also now GA, helping bring Grafana and Datadog dashboards and alerts into Elastic automatically and making it easier for users to benefit from Elasticsearch's storage efficiency (up to 2.5x better than Prometheus) and faster query performance (up to 30x faster than Prometheus) without starting from scratch. In 9.5, a new codec (ES95) builds on


**Elasticsearch's**


already substantial efficiency gains


[with columnar metrics](https://www.elastic.co/search-labs/blog/elasticsearch-columnar-metrics-engine-30x-faster-prometheus) to push storage costs down a further ~20% to roughly 3 bytes per sample, so users can monitor more services and retain metrics longer without ballooning the bill. Along with


[dashboards-as-code](https://www.elastic.co/blog/whats-new-elastic-9-5-0?#ai-native-kibana) and dashboards in chat, these updates let users unify metrics, logs, and traces on a single platform and correlate across all three for faster root cause analysis while keeping the workflows and standards they already rely on.


#### Complete integrations fully loaded with AI


Elastic's latest out-of-the-box integrations for Kubernetes and AWS (tech preview) mean you can go from zero to fully monitored in minutes.


[Kubernetes monitoring](https://www.elastic.co/observability-labs/blog/kubernetes-dashboards-alerts-anomaly-detection) is now GA and ships complete with preconfigured dashboards, alerts, SLOs, and machine learning jobs. Plus,


[Agent Skills](https://github.com/elastic/agent-skills) and an


[Observability MCP app](https://github.com/elastic/example-mcp-app-observability) make health monitoring, anomaly detection, incident investigation, and remediation available through any MCP-enabled AI tool where SREs already work. These comprehensive integration packages enable teams to get immediate value from metrics from the moment they connect.


#### Easier onboarding of cloud native data


[Elastic managed integrations](https://www.elastic.co/docs/reference/integrations/managed_integrations) eliminate the operational burden of ingesting cloud data sources. With just a few clicks, users can now ingest and start getting value without deploying or maintaining on-prem agents. In the spirit of the “easy button,” we’ve also


**simplified Kubernetes and AWS**


onboarding by streamlining both setup flows to default to the recommended OTel path, meaning platform engineers and SREs can get up and running without navigating multiple screens and decisions.


#### Enhanced APM and LLM observability


9.5 delivers a major quality of life improvement for SREs and developers investigating service incidents.


[Dependency analysis in alerts and dashboards](https://www.elastic.co/observability-labs/blog/service-map-apm-dependency-analysis) ****


introduces


****


a faster route to triage, eliminating context-switching by surfacing service maps directly in an alert or custom dashboard. Better


[surfacing of anomalies](https://www.elastic.co/observability-labs/blog/red-metrics-trace-breakdown-discover) in APM service health views makes ML-based degradation signals easier to spot.


Elastic’s new


[Anthropic integration](https://www.elastic.co/observability-labs/blog/anthropic-claude-api-monitoring) **for LLM observability**


polls Anthropic's Admin APIs to ingest organization-wide telemetry from the Claude API platform, token usage, cost, and rate limit configuration into Elasticsearch with prebuilt Kibana dashboards and out-of-the-box alerts.


Identify dependencies directly on your alert detail pages.


### Elastic Security


Elastic Security 9.5 gives analysts more time to decide and fewer alerts to sort. Stronger endpoint protection stops more threats before they ever raise an alert. Attack Discovery works the alerts that do fire the way an analyst would, surfacing the real attacks so that teams don't start their day buried in raw alerts. And automation runs underneath it all, triggering those investigations and handling the mechanical work, so the decisions stay with analysts.


#### Alert Zero: From alert queue to validated threats


[Alert Zero](https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero) is the SOC's version of inbox zero, a queue worked down to what actually matters, reached by agents and analysts together. It’s a goal teams move toward. It doesn't mean zero alerts, and it doesn't mean replacing the analysts. Attack Discovery is what gets a SOC closer to Alert Zero. It now investigates like an analyst by threat-hunting raw events, checking entity risk, and corroborating beyond the alerts that first fired before it calls anything an attack, so teams open a short list of validated attacks instead of a wall of raw alerts. When it finds something the rules missed, it drafts an ES|QL rule to close the gap with an analyst approving before anything is saved.


Every run goes through the same investigation now, whether kicked off manually, set on a recurring cadence, or triggered from an Elastic Workflow. A separate alert analysis workflow works the noise from the other side, classifying alerts as true or false positives so that analysts stop losing hours to low-fidelity ones, leaving Attack Discovery a cleaner set to investigate.


#### Enhanced endpoint protection


With Elastic Security 9.5, new


[endpoint capabilities](https://www.elastic.co/security-labs/vulnerable-driver-detection-elastic-defend-byovd) make protection stronger and extend coverage across more devices. For vulnerable drivers, which attackers bring in already signed and trusted to reach the kernel, our threat research team monitors public disclosure sources like VirusTotal, loldrivers.io and Microsoft's blocklist. Through an always-on process, Elastic automatically generates and instantly deploys YARA rules as new drivers are disclosured, so protection keeps pace instead of waiting on a release cycle. That speed matters when AI-driven attacks can move from one machine to the next in under a minute, faster than any response workflow can react. Windows on ARM is now fully covered in Elastic Defend, bringing Surface and other ARM-based laptops into full coverage. And a new endpoint troubleshooting skill in Agent Builder flags policy and performance issues, so teams spend less time chasing endpoint problems.


#### Automation across your SOC


[Elastic Workflows](https://www.elastic.co/blog/whats-new-elastic-9-5-0?#enhanced-automation-where-your-data-lives) brings native automation to the SOC, so a detection can trigger enrichment, case creation, and response automatically with no separate SOAR to buy, integrate, or maintain. Humans stay on the decisions that need judgment.


### In case you missed it …


In between stack releases, we’re not just resting on our laurels. The time between Elastic 9.4 and 9.5 was anything but quiet. Here's a roundup of some key announcements ICYMI:


-


**Jina AI updates:**


-


[jina-embeddings-v5-omni](https://www.elastic.co/search-labs/blog/jina-embeddings-v5-omni-all-media-one-index) extends the v5-text model family with native image, audio, and video support, carrying nearly 100 languages across all four modalities in a single embedding space with no reindexing required.


-


[Jina On-Prem](https://www.elastic.co/search-labs/blog/on-prem-ai-jina-embedding-models) is now available for teams with strict data residency requirements. It packages the full Jina AI model lineup (e.g., reader, embedding, and reranking) to run entirely on your own hardware, fully air-gapped with zero outbound calls once deployed. Costs are fixed and predictable, not metered per token. The embedding models search text, images, audio, and video across nearly 100 languages in a single embedding space with accuracy that rivals much larger models on commodity hardware.


-


We've released


[jina-reranker-v3.5](https://www.elastic.co/search-labs/blog/jina-reranker-35-legal-medical-structured-data) , a drop-in successor to jina-reranker-v3.


jina-reranker-v3.5


, at just 600M parameters, beats its predecessor by 50%+ on case law with further gains on medical and financial reranking and runs up to 56% faster on long documents. Against Qwen3-Reranker-4B, a widely used open source reranker more than seven times its size, it wins outright on general-purpose search and one structured-data benchmark and closes most of the remaining gap everywhere else.


-


[Reindex from remote](https://www.elastic.co/search-labs/blog/elasticsearch-reindex-node-relocation-pit-serverless) is now generally available in Elastic Cloud Serverless, giving teams a seamless way to migrate indices from any Elastic Cloud Hosted deployment or Serverless project, regardless of region, directly into their Serverless environment. The operation is built for the realities of Serverless infrastructure; it automatically resumes through node shutdowns triggered by scaling events or software deployments, so your migration keeps moving without manual intervention.


## Start here now


Elastic 9.5 is packed to the brim with new and enhanced features ready to help organizations get the most out of their data.


Elastic 9.5 is


[now available on Elastic Cloud](https://cloud.elastic.co/registration) , the hosted Elasticsearch service that includes all of the new features in this latest release.


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


*In this blog post, we may have used or referred to third party generative AI tools, which are owned and operated by their respective owners. Elastic does not have any control over the third party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use.*


*Elastic, Elasticsearch, and associated marks are trademarks, logos or registered trademarks of elasticsearch B.V. in the United States and other countries. All other company and product names are trademarks, logos or registered trademarks of their respective owners.*


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
