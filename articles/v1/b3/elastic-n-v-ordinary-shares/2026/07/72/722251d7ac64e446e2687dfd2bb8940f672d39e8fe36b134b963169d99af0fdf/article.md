---
schema_version: "1.0.0"
document_id: "722251d7ac64e446e2687dfd2bb8940f672d39e8fe36b134b963169d99af0fdf"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/on-prem-ai-jina-embedding-models"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-23T17:43:30.841265+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:a77024d6401602f75b7076f7f7bbfc1ac88fe8286f2448a49b16ea00de36622f"
---

# On-prem in under 5 minutes: Jina embedding models now available for on-prem deployment

Get hands-on with Elasticsearch: Dive into our sample notebooks in the[Elasticsearch Labs repo](https://github.com/elastic/elasticsearch-labs?tab=readme-ov-file) , start a[free cloud trial](https://www.elastic.co/cloud/cloud-trial-overview) , or try Elastic on your[local machine now](https://github.com/elastic/start-local) .


All 28[Jina AI](https://www.elastic.co/jina-search-models)embedding and[reranking](https://www.elastic.co/search-labs/blog/elastic-semantic-reranker-part-1) models now ship as fully offline Docker containers for on-prem deployment, including[jina-embeddings-v5-omni](https://www.elastic.co/search-labs/blog/jina-embeddings-v5-omni-all-media-one-index) and[jina-reranker-v3](https://www.elastic.co/search-labs/tutorials/jina-tutorial/jina-reranker-v3) . Download one, transfer it to an on-premises air-gapped or firewalled system, and local[inference](https://www.elastic.co/docs/explore-analyze/elastic-inference) is running in under five minutes. The containers are completely self-contained and make no external connections. There’s no call to Hugging Face or any model registry. There’s also no license server or telemetry or logging endpoints. For regulated industries, data sovereignty requirements or environments where internet access is unreliable or simply unavailable, this removes the dependency on third-party AI services. Jina On-Prem supports[Elastic Inference Service](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis) (EIS), OpenAI, Cohere, Voyage AI, and Gemini API schemas, so existing applications work without code changes.


The most powerful AI models run on remote cloud installations with access via a web API, meaning that you have to trust your AI service provider for security, service availability, and stable prices. You can’t easily align reasonable demands for reliability, privacy, manageable costs, and good data governance with increasingly powerful, sophisticated, and resource-intensive AI usage.


Government regulation, court rulings, and business considerations made in someone else’s interest have all recently resulted in restricting access to specific services. And even if you can switch to other services, AI models aren’t components that can just be swapped out whenever you want. Applications that use semantic[embeddings](https://www.elastic.co/what-is/vector-embedding) depend on having access to the same models atquery time as at data ingestion time. To lose access to yourembedding model means your search system comes to a halt.


AI pricing models compound that risk. Recent financial disclosures from major AI vendors give customers good reason to be concerned about potential price hikes. Reliance on products with unpredictable costs adds more risk to capital-intensive AI investments that may not produce clear returns.


Jina On-Prem is Elastic’s answer to these challenges.


## Who needs on-premises AI?


Local hosting and direct control over your AI models support a variety of technical demands, industry requirements, and business interests.


Local installation reduces what you pay your AI service providers, but it puts the cost of hardware and reliable access on your organization. Depending on your volume of use, it may simply be cheaper. But there are additional pressing reasons to consider running your own AI. If any of the issues described below concern your enterprise, consider a local AI solution like Jina On-Prem. This list is not exhaustive.


Use case Why on-prem Example


Air-gapped / high-security No outbound data transmission; complete network isolation Defence, intelligence, classified research


Regulatory compliance Data sovereignty; no cross-border transmission or third-party exposure Healthcare (Health Insurance Portability and Accountability Act \[HIPAA\]), finance, EU enterprises (General Data Protection Regulation \[GDPR\])


Latency-critical Zero network dependency; no tolerance for connection failures Robotics, edge computing, vehicles, ships


Cost predictability Fixed infrastructure cost vs. per-token pricing with uncertain future rates High-volume continuous inference workloads


Liability reduction No third-party data exposure; maintains legal privilege and duty of care Law firms, government agencies


### Why air-gapped and firewalled systems need on-prem AI


Air-gapped and firewalled systems cannot use external AI APIs. Jina On-Prem runs entirely within your infrastructure with no outbound connections.


For organizations managing especially sensitive data, security and privacy considerations are paramount. It does little good to invest in protecting your sensitive data if you promptly turn it over to some remote third party that may have insufficient security in place or might be subject to the demands of a foreign government.


Employees in organizations that handle sensitive data often receive some training in secure data handling, but this isn’t very effective when they all have web browsers that may be open to any page on the internet while they handle that data. Isolation is the most effective security measure available, either through air-gapping or very restrictive firewalls, but that makes it difficult to use external services of any kind.


### On-prem AI for latency-sensitive and high-availability systems


Software as a service and cloud computing represent a compromise between the cost of offering highly accessible, reliable services on your own computers and outsourcing the problem to someone else. But they come with variablelatency , outages, and a complete loss of control when things go wrong. AI services aren’t the exception. If your search system goes offline when you can’t access your embedding model, it may no longer look like a good compromise.


Furthermore, relying on external AI will always involve risks that you can’t easily foresee or manage. Internet access and network latency can degrade without notice, as a result of political events, bad weather, or ships dragging their anchors over underwater fiber-optic cables. Governments can, and recently have, used export bans to suddenly block access to AI models. AI service providers sometimes withdraw models to induce you to switch to newer ones. The flexibility and managed costs of external services have to be balanced against the risks of dependency.


### On-prem AI for GDPR, HIPAA, and data sovereignty compliance


Organizations that collect personal data are subject to increasingly stringent regulations which often differ between jurisdictions and may have contradictory requirements. Notably,[HIPAA rules](https://www.hhs.gov/hipaa/for-professionals/privacy/laws-regulations/index.html) place very strict data protections on American healthcare providers, and strong general data protection laws in[Canada](https://laws-lois.justice.gc.ca/eng/acts/p-8.6/) , the[European Union](https://gdpr-info.eu/) , and[many Asian jurisdictions](https://www.japaneselawtranslation.go.jp/en/laws/view/4241) require all enterprises that handle personal information to do so securely and to limit the transmission of that data to other parties or other jurisdictions. These rules can even impose obligations on foreign entities if they have any customers in those jurisdictions. Financial institutions are frequently subject to even stricter rules and bear the same direct liability for information security that they have to protect against other forms of criminal activity.


Regulatory compliance can be incompatible with third-party AI services, especially if using them involves cross-border data transmission.


Furthermore, recent events show that rules restricting the physical location of data stores may not be a reliable source of protection when international cloud operators are subject to pressure from foreign governments. Local laws may conflict between jurisdictions, requiring local data storage and processing and making third-party services impossible to use. In some cases, the only solution is to take all the parts of your processes in house, including your AI systems.


### AI liability risks from third-party data transmission


Data protection laws and recognized duties of care toward sensitive data routinely have liability implications, sometimes very severe ones. You can be liable for third-party service providers’ handling of your data. While courts and legal procedures might provide some retrospective protections from insecure service providers, those remedies are not available nor generally effective against national security actors, law enforcement, or criminal hackers.


For governments, there have already been instances of cross-border cloud service providers releasing sensitive state information to foreign actors.


But even if you don’t worry about foreign governments or hackers, and if your external AI service providers are themselves secure, just the fact that they’re external can create liabilities.


For example, in most jurisdictions, lawyers’ communications with their clients enjoy special legal protections, and law offices have strict liabilities when recording or storing this information. In the United States, this “attorney-client privilege” is so famous, it’s central to movie and TV plots. But one of the ways that privilege can be lost is by communicating information with someone who is not privileged, and recent developments suggest that external AI service providers might qualify.


It’s possible, at least in the United States, that just using third-party AI services over an internet API, like embedding models that provideindexing services, might violate critical confidentiality rules. A law firm might be sued, disciplined, or disbarred just for using externally hosted software, even if no security breach occurs.


### On-prem AI for offline, edge, and physically isolated systems


Computer systems aren’t just isolated for security reasons. For example, moving vehicles cannot rely on internet access for any essential functions. Ships and aircraft have very extensive onboard computer systems that have to function without internet connections and therefore cannot use external AI services. Offshore platforms, remote facilities in wilderness areas, computer services in the Arctic, Antarctic and on small islands without adequate physical connections to global networks are all examples of installations that benefit from locally hosting all the services they need. As AI’s role in enterprise computing grows, these limitations become more important to address.


Emerging applications of AI to physical systems (robotics and other spatially confined or external-world–focused use cases, like logistics management systems or even supermarket checkouts) may be connected to the global internet, but they have no tolerance for connection failures or spikes in latency. If they rely on an AI system to operate, that AI system needs to be as local and reliable as possible.


## Who doesn’t need on-premises AI?


Remote software services and off-site AI do have benefits. Running AI models can require expensive, power-hungry processors with notoriously short lifespans. Access to high-quality hardware is particularly difficult right now due to market factors and external economic shocks. Under the circumstances, it may make sense to pay by thetoken to use an external API instead of supporting the steep capital costs of local AI.


External APIs make the most sense for intermittent users. If you use AI models primarily to batch process data for analysis, rather than running a search system that has to be online all the time, it makes little sense to invest in capital-intensive hardware and local installations.


Furthermore, when your data processing is already cloud-based, for example, an[ecommerce](https://www.elastic.co/enterprise-search/ecommerce) website hosted in the cloud for reliability and accessibility reasons, using AI services located in the same cloud infrastructure may provide a better value for money than introducing your own licensed AI model deployment. You’re already dependent on your cloud service provider, so being dependent on its AI services doesn’t add much risk.


If your use case sounds like it fits that description, Jina AI models are available on[EIS](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis) ,[AWS Marketplace](https://aws.amazon.com/marketplace/seller-profile?id=seller-stch2ludm6vgy) , and the[Google Cloud Platform](https://console.cloud.google.com/marketplace/browse?q=jina) specifically to meet your needs.


The table below summarizes the key factors. Your answer depends on your data, infrastructure and usage pattern.


Factor On-prem favored Cloud API favored


Usage pattern Continuous or high-volume inference Intermittent or batch processing


Data sensitivity Regulated, sovereign, or classified No cross-border or third-party restrictions


Network environment Air-gapped, firewalled, or unreliable Stable, always-on internet


Existing infrastructure Own or can procure GPU hardware Already cloud-hosted with colocated AI


Cost model Fixed hardware + license; predictable at scale Per-token; lower up-front, variable long-term


Latency tolerance None (robotics, edge, real-time) Network variability is acceptable


Operational responsibility Your team manages hardware and availability Provider manages hardware and updates; you manage integration


You have to consider the costs and benefits in light of your particular circumstances and use cases, taking into account the issues highlighted in the previous section that apply to you. The cost-benefit analysis will doubtless change over time. We can’t predict the future of the AI industry or hardware prices even in the short term.


## Introducing Jina On-Prem


For users who can benefit from local AI services, we’re introducing[Jina On-Prem](https://github.com/jina-ai/jina-on-prem/wiki/) , a fully self-contained installation suite for Jina AI’s high-performance models.


Jina AI’s models match the accuracy of embedding models[many times their size](https://mteb-leaderboard.hf.space/benchmark/MTEB(Multilingual%2C%20v2)) , reducing compute costs, memory footprints, and hardware requirements. This makes them an ideal choice for users who want or need to keep their AI on-premises. Commercial licenses are available with scalable, proportionately priced solutions for use cases of all sizes.


Massive Multilingual Text Embedding Benchmark (MMTEB) leaderboard, as of July 8, 2026.


### What API schemas does Jina On-Prem support?


- Available as a complete collection of dependencies for local installation or as a[Docker container](https://www.docker.com/) that you can install and run in minutes.
- Jina On-Prem installations *do not* call out to outside systems.


- No call to Hugging Face Hub or any model registry (` HF_HUB_OFFLINE=1` and` TRANSFORMERS_OFFLINE=1` are baked in).
- There’s no license server.
- There are no telemetry or logging endpoints.


- Supports both CPU and GPU hardware, with GPU autodetection.
- All 28 Jina AI models available, including the latest[jina-embeddings-v5-omni](https://www.elastic.co/search-labs/blog/jina-embeddings-v5-omni-all-media-one-index)multimodal embedding models and[jina-reranker-v3](https://www.elastic.co/search-labs/tutorials/jina-tutorial/jina-reranker-v3) .
- Access via standard AI API schemas:[Jina API](https://jina.ai/api-dashboard) , OpenAI, Cohere, Voyage AI, and Gemini. Jina On-Prem is a drop-in solution for applications built on those schemas.
- Drop-in replacement for models served by the[EIS](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis) . Jina On-Prem integrates directly with[air-gapped Elastic deployments](https://www.elastic.co/blog/deploy-elastic-air-gapped-disconnected-environments) .


## Hardware requirements for Jina AI on-prem models


The hardware requirements vary for different Jina models. The table below shows the recommendations for the most recent models using GPU settings. You don’t need anything more powerful than an NVIDIA L4 GPU, although an A100 is recommended for the v5 embedding models. Our latest embedding model currently requires a minimum of 8 GB of VRAM.


Model Minimum VRAM Recommended GPU


jina-embeddings-v5-text-nano 2 GB T4 / L4


jina-embeddings-v5-text-small 3 GB L4 / A10G


jina-embeddings-v5-omni-small 8 GB L4 / A10G / A100


jina-reranker-v3 3 GB L4


jina-clip-v2 4 GB L4


jina-code-embeddings-1.5b 4 GB L4


ReaderLM-v2 4 GB L4


If you use more than one model at a time, the VRAM requirements will increase. Please see the[Sizing and Hardware page](https://github.com/jina-ai/jina-on-prem/wiki/Sizing-And-Hardware) for more information.


## How to install Jina On-Prem with Docker


The quickest way to get started is to[install Docker](https://www.docker.com/get-started/) (if you haven’t already) and follow the instructions on the[Jina On-Prem Quick Start](https://github.com/jina-ai/jina-on-prem/wiki/QuickStart) page.


There are pre-composed Docker containers for all 28 Jina models. Download one and transfer it to your installation target, and you can have Jina AI models running in under five minutes.


For multimodal or custom builds, or to download the complete dependency set for installation outside of a container, follow the steps outlined in the[bundling guide](https://github.com/jina-ai/jina-on-prem/wiki/Bundling-Guide) .


You’ll need a GitHub account and access token to download Jina On-Prem. To create a free account, go to[https://github.com/signup](https://github.com/signup) . To generate or manage your access tokens, follow the instructions in the[GitHub documentation](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) .


Your Jina On-Prem installation supports all Jina API and EIS functionality and embedding generation via OpenAI, Cohere, Voyage AI, and Gemini APIs, so it can integrate into preexisting applications using standard interfaces. See the[API documentation](https://github.com/jina-ai/jina-on-prem/wiki/API-Reference) for more information.


Jina models, including models installed with Jina On-Prem, are available on various licensing terms, with the latest models free for noncommercial use under a[CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.en) license. To license Jina On-Prem for commercial use, please contact[Elastic Sales](https://www.elastic.co/contact) .


#### How helpful was this content?


Not helpful


Somewhat helpful


Very helpful


[Report an issue](https://discuss.elastic.co/c/elastic-community-ecosystem/elasticsearch-labs/101)


## Related Content


[Agentic AI](https://www.elastic.co/search-labs/blog/category/agentic-ai)[Integrations](https://www.elastic.co/search-labs/blog/category/integrations)


July 28, 2026


#### [One prompt, a complete workflow: Elastic's AI agent writes your automation for you](https://www.elastic.co/search-labs/blog/ai-workflow-automation-natural-language)


Elastic Workflows takes a plain-text prompt and generates YAML you can inspect, version and run against your Elasticsearch data. Now GA, with human-in-the-loop workflows in Slack, parallel execution, and 10 new connectors.


TESG


By:[Tinsae Erkailo](https://www.elastic.co/search-labs/author/tinsae-erkailo)


and[Shahar Glazner](https://www.elastic.co/search-labs/author/shahar-glazner)


[Jina AI](https://www.elastic.co/search-labs/blog/category/jina-ai)[Relevance](https://www.elastic.co/search-labs/blog/category/relevance) +1


July 27, 2026


#### [56% faster, up to 50% better retrieval performance: What's inside Jina's new 600 million parameter listwise reranker](https://www.elastic.co/search-labs/blog/jina-reranker-35-legal-medical-structured-data)


Jina Reranker 3.5 beats v3 by 50%+ on case law, closes the gap with models 7x its size on legal, medical, and financial benchmarks, and beats them outright on structured data. It's a drop-in replacement for v3, with no API changes.


FWSM


By:[Felix Wang](https://www.elastic.co/search-labs/author/felix-wang)


and[Scott Martens](https://www.elastic.co/search-labs/author/scott-martens)


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)[Relevance](https://www.elastic.co/search-labs/blog/category/relevance) +1


July 16, 2026


#### [A picture is worth 1.5x the words: What we learned benchmarking product search embeddings](https://www.elastic.co/search-labs/blog/multimodal-embeddings-ecommerce-product-search)


We benchmarked two embedding models on 5,000 real products and found that combining image and text beats either alone by up to 50%. Here's the data and the model that won.


SV


By:[Sofia Vasileva](https://www.elastic.co/search-labs/author/sofia-vasileva)


[Integrations](https://www.elastic.co/search-labs/blog/category/integrations)[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database) +1


July 21, 2026


#### [4 NVIDIA AI tasks, 1 Elasticsearch API: Embeddings, chat, completion, and rerank](https://www.elastic.co/search-labs/blog/elasticsearch-nvidia-inference)


Set up NVIDIA hosted models in Elasticsearch with one API key and a model ID. No custom integration code needed.


K


By:[Jan Kazlouski](https://www.elastic.co/search-labs/author/jan-kazlouski)


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)[Jina AI](https://www.elastic.co/search-labs/blog/category/jina-ai) +1


July 10, 2026


#### [How BBQ shrinks Jina v5 embeddings by 29x without losing recall in Elasticsearch](https://www.elastic.co/search-labs/blog/bbq-quantization-jina-embeddings-v5)


A hands-on test comparing BBQ and float32 vector indices in Elasticsearch, measuring memory, disk and recall@10 across five languages.


JR


By:[Jeffrey Rengifo](https://www.elastic.co/search-labs/author/jeffrey-rengifo)
