---
schema_version: "1.0.0"
document_id: "a92909ac46154cc769ee9c6afbe1daf03edba48978f604c8823f054014a84a8a"
company_key: "yc-cargo"
company: "Cargo"
source_id: "yc-cargo-news-import-f4a8864e899b"
canonical_url: "https://www.getcargo.ai/blog/why-snowflake-is-the-new-salesforce"
published_at: "2024-01-01T00:00:00+00:00"
first_seen_at: "2026-07-21T12:34:39.888673+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:04843b68b9671c45c76eb94df576879c483cc2e917abcc63fc830d0a9ace5081"
---

# Snowflake vs Salesforce - The system of record battle

Salesforce has been considered for a long time as the source of truth. However, in the last 5 years, the number of SaaS tools used by companies has been multiplied by 10. Data are disparate across more tools than it was before (analytics data, product data, marketing data, financial data …) and Salesforce became less and less the reference for business and customer data.


The first generation of tools called CDPs tried to solve these problems.


CDP first generation


Their main value proposition was simple: Reconsolidate data (mainly analytics data and customer data) to have a clear overview of the customer journeys and sync them back into your company’s CRM.


They were promising to own the source of truth again. But they actually made the same mistake than CRMs did. They couldn’t easily adapt and fit the data complexity of your organization. You ended up with an infinite loop, data you couldn’t unmerge and automatic workflows you couldn’t control.


The second generation of tools called Reverse ETL tackled the problem in a smarter way taking advantage of the rise of cloud data warehouse adoption.


As the data warehouse was becoming central in companies for data visualization, they leveraged the work done by analytics engineers to sync it back into your CRM.


Composable CDP


But finally, if we step back, what are these 2 tool categories are trying to do? … They are just patching CRM limitations.


Let’s list some of these limitations here:


-


**Opinionated** - CRMs enforce a proprietary data model to represent your business entities and you will have to hack it to match your own definition or push other than customer or sales data


-


**Inflexible** - Models can’t be easily recomputed as your organization grows. You have to be cautious when starting to build your CRM properly otherwise you’ll end up with messy and unusable data and have to start again from the beginning


-


**Locked up** - APIs were great for the old world but limited when we are talking about synchronizing/updating a big amount of data. Data is managed and owned by our CRM and can’t be easily accessible for other providers/services.


And who is the best applicant to solve all these limitations? … Your data warehouse. Instead of trying to push it and format it in another tool you don’t master/control. Why not just use these models you or your team built while remaining in the warehouse.‍


That’s why we truly believe that the data warehouse and especially Snowflake is going to be a dangerous competitor for Salesforce in the coming years.


And both of them know their strengths and weaknesses. Snowflake owns the data and Salesforce, the sales interface. On one side, Salesforce signed a partnership for easy data sharing. On the other side, Snowflake announced the launch of Unistore and the Native Application Framework that show their interest in not only being considered as a data warehouse anymore.


Their ambition is to become a data operation system with a marketplace and to define a new standard for software to communicate together and access the data.


Seamantic layer atop the data warehouse


Semantic layer will become a new standard for SaaS apps to communicate together. Data engineering will provide a unified data business layer that apps could consume. APIs won’t be necessary anymore to sync data between tools. SaaS tools read and write data in the data warehouse and won’t own your data.


CRM was the first software to be a SaaS, it was the first software to own a marketplace, and the first software to provide a public API.


We can split a CRM into two main functions: The system of record, how you catalogue all the related customer data in a single place, and the system of engagement, helping you to interact with your customers. That’s where Cargo comes into place.


By building the engagement system on top of your Data Warehouse, we want to lead this revolution and define the future of CRMs.


## Key Takeaways#


- **Salesforce lost “system of record” status as SaaS proliferated** : 10x more tools in 5 years fragmented data across analytics, product, marketing, finance, CRM became one source among many, not THE source of truth
- **Two generations tried patching CRM limitations** : CDPs (1st gen) reconsolidated data but repeated CRM mistakes (inflexible, locked); Reverse ETL (2nd gen) leveraged data warehouses smartly, but both just patch CRM’s opinionated, inflexible, locked-up architecture
- **Three core CRM limitations data warehouses solve** : Opinionated (enforces proprietary data model vs. your business entities), Inflexible (models can’t recompute as org grows, messy data requires restart), Locked (APIs slow for bulk sync, vendor owns data)
- **Snowflake’s ambition: become data operating system with marketplace** : Unistore + Native Application Framework position Snowflake beyond warehouse, semantic layer as new standard for SaaS communication (apps read/write directly to warehouse, no APIs needed)
- **Future CRM = System of Record (warehouse) + System of Engagement (Cargo)** : Salesforce owns sales interface, Snowflake owns data, partnership acknowledges this. New architecture: Data warehouse as record system, engagement layer on top (Cargo), SaaS apps consume unified business layer via semantic layer


## Frequently Asked Questions#


Join the movement 👉[https://www.getcargo.ai](https://www.getcargo.ai/)
