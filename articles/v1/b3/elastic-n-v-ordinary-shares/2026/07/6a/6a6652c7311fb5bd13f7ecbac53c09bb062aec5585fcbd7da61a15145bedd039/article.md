---
schema_version: "1.0.0"
document_id: "6a6652c7311fb5bd13f7ecbac53c09bb062aec5585fcbd7da61a15145bedd039"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/elastic-cloud-now-available-madrid-tokyo-sweden-sao-paulo"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-28T15:43:33.113789+00:00"
fetched_at: "2026-07-28T20:31:35.420648+00:00"
content_hash: "sha256:2ac434d44e471bb4cf9c8f24424323f74c7aebf04b99fa3db58254e95f20f651"
---

# Elastic Cloud Serverless: Ongoing multicontinent expansion

# Elastic Cloud Serverless: Ongoing multicontinent expansion


Coverage reaches 33 regions around the globe with additional feature enhancements


By


[Brian Bergholm](https://www.elastic.co/blog/author/brian-bergholm)


July 28, 2026


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


###### Summary


- Elastic Cloud Serverless eliminates infrastructure bottlenecks, shifting you from capacity management to solution building.


- Powered by Elasticsearch, it’s the fastest way to deploy search, observability, security, and RAG applications.


- Elastic Cloud Serverless has expanded even further to 4 new regions across 3 continents — total coverage of 33 regions globally across hyperscalers.


We’re pleased to announce the availability of


[Elastic Cloud Serverless](https://www.elastic.co/cloud/serverless) on Google Cloud and Microsoft Azure in the following regions starting July 28, 2026:


-


Europe Southwest 1 (Madrid), gcp-europe-southwest1


-


Asia Northeast 1 (Tokyo), gcp-asia-northeast1


-


Sweden Central (Sweden), azure-swedencentral


-


Brazil South (São Paulo), azure-brazilsouth


This expansion brings our total coverage to


[33 regions around the globe](https://www.elastic.co/docs/deploy-manage/deploy/elastic-cloud/regions) . Elastic Cloud Serverless is where you can find the newest features and incremental improvements included in


[Elastic releases](https://www.elastic.co/blog/whats-new-elastic-9-4-0#elastic-9.4:-workflows-ga,-agent-builder-updates,-and-prometheus/promql-support) .


## Serverless benefits


With Elastic Cloud Serverless, you just create a project and start building. Elastic runs all the underlying infrastructure on your behalf. The platform comes pre-tuned for Search AI workloads with infrastructure optimized for vector search, hybrid retrieval, and high-throughput ingest out of the box. There are no clusters to size, no shards to balance, no upgrades to schedule, and no capacity to plan. Search and ingest scale independently on a decoupled architecture, so a spike on one side doesn't impact the other. The result: Teams move faster from idea to production for context-aware search, observability, security, and retrieval augmented generation (RAG) applications.


## Example use cases


Serverless is relevant for any cloud-based Elastic use case. Below are a few examples where the operating model is especially impactful.


### High-variability ecommerce and catalog search


Retailers frequently experience massive, unpredictable traffic spikes during events like flash sales, forcing them to over-provision infrastructure and pay for unused compute during idle periods just to avoid downtime or poor customer experience. With Serverless, the decoupled architecture automatically scales compute up during traffic surges and scales it down when traffic normalizes. Customers only pay for the exact compute used, ensuring high-performance catalog retrieval.


### High-density SaaS multitenancy


ISVs and SaaS platforms face significant challenges balancing secure data isolation with cloud costs, as managing dedicated clusters for thousands of individual tenants creates a massive operational burden. Serverless provides a robust foundation for multitenancy by allowing organizations to rapidly provision discrete projects for new customers natively through the API. This isolates data efficiently without the need to manually manage node allocation or shard routing.


### Application observability and log analytics


During a system outage, applications can generate a massive spike in error logs. In a coupled architecture, this sudden ingest spike may consume all cluster resources, causing the actual search queries used to find the root cause to slow down. Serverless independently separates indexing compute from search compute, meaning a massive spike in log ingestion will dynamically scale the indexing tier without any scale up for the search tier. This allows SREs to query their observability data in real time during a critical incident.


## Getting started with Elastic Cloud Serverless on Google Cloud


To try Elastic Cloud Serverless on Google Cloud, visit


[Google Cloud Marketplace](https://console.cloud.google.com/marketplace/product/elastic-prod/elastic-cloud?inv=1&invt=AbzKmQ&project=elastic-product)


and follow the steps below. You can either start a seven-day free trial or employ any of your existing credits. Your Elastic subscription will appear in your Google Cloud consolidated billing, and your spend counts toward your Google Cloud cloud commitment. You can easily monitor and analyze your Elastic usage and costs right from the Elastic Cloud console.


1.


Create a


**Serverless project**


.


2.


For


**Elasticsearch**


,


**Elastic Observability**


, or


**Elastic Security**


projects, select


**Google Cloud**


as the cloud provider and then choose your preferred Google Cloud region.


3.


Click


**Create project**


, and your Elastic Cloud Serverless project will be provisioned.


If you have any questions or want to learn more, visit our


[documentation](https://www.elastic.co/docs/current/serverless) .


## Getting started with Elastic Cloud Serverless on Azure


To try Elastic Cloud Serverless on Azure, visit


[Microsoft Marketplace](https://marketplace.microsoft.com/en-us/product/elastic.ec-azure-pp?ocid=cloud-serverless-recap2025) and follow the steps below. You can either start a seven-day free trial or employ any of your existing credits. Your Elastic subscription will appear in your Microsoft Azure consolidated billing, and your spend counts toward your Azure cloud commitment. You can easily monitor and analyze your Elastic usage and costs right from the Elastic Cloud console.


1.


Create a


**Serverless project**


.


2.


For


**Elasticsearch**


,


**Elastic Observability**


, or


**Elastic Security**


projects, select


**Azure**


as the cloud provider and then choose your preferred Azure region.


3.


Click


**Create project**


, and your Elastic Cloud Serverless project will be provisioned.


If you have any questions or want to learn more, visit our


[documentation](https://www.elastic.co/docs/current/serverless) .


## Forward into the future


Elastic Cloud Serverless is a fully managed operating model that provides the most efficient way to build and scale search, observability, and security applications. When Elastic manages all your behind-the-scenes operations like version upgrades, sharding, and scaling, it allows your developers to focus entirely on their applications.


For more information, check out our


[ambitious roadmap](https://www.elastic.co/cloud/serverless/roadmap) or read why


[Vectorize.io](https://www.elastic.co/customers/vectorize-io) and


[Docusign](https://www.elastic.co/customers/docusign) chose Elastic Cloud Serverless.


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


*In this blog post, we may have used or referred to third party generative AI tools, which are owned and operated by their respective owners. Elastic does not have any control over the third party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use.*


*Elastic, Elasticsearch, and associated marks are trademarks, logos, or registered trademarks of Elasticsearch N.V. in the United States and other countries. All other company and product names are trademarks, logos, or registered trademarks of their respective owners.*


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
