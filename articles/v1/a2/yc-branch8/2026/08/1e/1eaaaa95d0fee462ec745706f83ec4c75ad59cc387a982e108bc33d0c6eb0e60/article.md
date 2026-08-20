---
schema_version: "1.0.0"
document_id: "1eaaaa95d0fee462ec745706f83ec4c75ad59cc387a982e108bc33d0c6eb0e60"
company_key: "yc-branch8"
company: "Branch8"
source_id: "yc-branch8-news-import-c52687a2f2d5"
canonical_url: "https://branch8.com/posts/omnichannel-retail-data-architecture-apac-guide-2026"
published_at: "2026-08-08T03:00:01+00:00"
first_seen_at: "2026-08-09T20:07:14.512134+00:00"
fetched_at: "2026-08-09T20:07:16.743686+00:00"
content_hash: "sha256:10b9511da64856360a77fcc86efce7d36dc908f7bd4cf978e4f512b7ae4de08d"
---

# Omnichannel Retail Data Architecture APAC Guide 2026: A Step-by-Step Blueprint

**Quick Answer:** A unified omnichannel retail data architecture for APAC connects marketplace (Shopee, Lazada), e-commerce (Shopify, Adobe Commerce), and POS channels through a central warehouse like BigQuery or Snowflake, with real-time inventory sync and identity resolution across markets.


---


Imagine a single customer view that reconciles a Shopee purchase in Jakarta, a WeChat Mini Program order in Shenzhen, a Shopify checkout in Sydney, and a walk-in at your flagship store in Hong Kong — all updating inventory, loyalty points, and margin calculations within 90 seconds. That is what a properly designed omnichannel retail data architecture delivers, and it is exactly what APAC retailers need heading into 2026.


*Related reading:*[Data Engineering Team Structure for Mid-Market Retail: A Hiring Sequence Guide](https://branch8.com/posts/data-engineering-team-structure-mid-market-retail)


*Related reading:*[Marketing Attribution Modelling for APAC Multi-Market Brands: A Step-by-Step Guide](https://branch8.com/posts/marketing-attribution-modelling-apac-multi-market-brands-guide)


This guide walks you through how to build that architecture from scratch or refactor what you already have. I am writing from direct experience: at Branch8, we have built and operated unified data stacks for retailers like Chow Sang Sang (400+ stores across APAC), HomePlus, and Maxim's Group. The patterns here are not theoretical — they are production-tested across Hong Kong, Singapore, Taiwan, and Southeast Asia.


*Related reading:*[UK E-Commerce Brand Singapore Market Entry Checklist: 9 Steps](https://branch8.com/posts/uk-ecommerce-brand-singapore-market-entry-checklist)


According to NIQ's 2025 Consumer Outlook, 73% of APAC consumers now expect a consistent experience whether shopping online, in-app, or in-store. Miss that expectation and you lose the sale. Miss it at the data layer and you lose the ability to even measure the loss.


## Prerequisites: What You Need Before You Start


### Inventory of Current Data Sources


Before drawing a single architecture diagram, catalogue every system that generates retail data. For a typical APAC omnichannel retailer, that list looks something like this:


- **E-commerce platforms** : Shopify, Shopify Plus, Adobe Commerce (Magento 2.4.x), or SHOPLINE
- **Marketplace channels** : Shopee, Lazada, Tokopedia, Rakuten, Amazon SG/AU/JP
- **POS systems** : Oracle Retail Xstore, Lightspeed, or custom-built POS (common in Taiwan and mainland China)
- **Messaging commerce** : WeChat Mini Programs, LINE Official Accounts, WhatsApp Business API
- **ERP / back-office** : SAP S/4HANA, Oracle NetSuite, or Microsoft Dynamics 365
- **Loyalty and CRM** : Salesforce, HubSpot, or proprietary loyalty engines
- **Payments** : Stripe, Adyen, local gateways like PayMaya (PH), GrabPay (SG/MY), or Line Pay (TW/JP)


Document every system's API capabilities, data refresh frequency, and format (REST JSON, SOAP XML, CSV flat files, webhook-based). This inventory becomes your ingestion requirements specification.


### Choosing Your Central Analytical Layer


You need a cloud data warehouse as the single source of truth. The two realistic options for APAC deployments in 2026 are **Google BigQuery** and **Snowflake** . Both have regional data centres across APAC. Here is how to decide:


- **BigQuery** if you are already invested in Google Cloud Platform, need serverless scaling with minimal DBA overhead, and your analytics team prefers SQL + Looker Studio.
- **Snowflake** if you need multi-cloud portability (AWS + Azure + GCP), have strict data-sharing requirements with partners, or need Snowpark for Python/Scala ML pipelines.


*Related reading:*[Managed Squad vs In-House Team Total Cost 2026: A Full Breakdown](https://branch8.com/posts/managed-squad-vs-in-house-team-total-cost-2026)


For most APAC retailers doing under US$500M in GMV, BigQuery on a flat-rate plan offers the better cost-to-performance ratio. Snowflake edges ahead when you have complex data-sharing use cases with franchisees or wholesale partners — a pattern we see frequently in Hong Kong and Australian retail groups.


*Related reading:*[React Native vs Native iOS Android Cost Analysis: A Buyer's Decision Framework](https://branch8.com/posts/react-native-vs-native-ios-android-cost-analysis-apac-guide)


### Regulatory and Residency Constraints


APAC is not one market. It is a patchwork of data residency rules:


- **Indonesia** : Government Regulation 71/2019 requires certain data categories be stored on servers in Indonesia.
- **Vietnam** : Cybersecurity Law (2018) mandates local storage for user data of Vietnamese citizens.
- **Australia** : The Privacy Act 1988 (APP 8) requires informed consent before transferring personal information offshore.
- **Singapore** : PDPA allows cross-border transfers with contractual safeguards.
- **Taiwan** : PIPA restricts transfers to jurisdictions without adequate protections.


Your architecture must account for these from day one. We typically solve this with regional Snowflake accounts or BigQuery datasets pinned to specific GCP regions (` asia-southeast1` for Singapore,` asia-southeast2` for Jakarta,` australia-southeast1` for Sydney), then use secure views or Snowflake data shares to create a federated query layer.


```text
1  -- BigQuery: Create regional datasets with location binding     2   CREATE     SCHEMA     `  project_id.retail_id_data  `     3    OPTIONS   (  location   =     'asia-southeast2'  )  ;     -- Jakarta     4
5   CREATE     SCHEMA     `  project_id.retail_sg_data  `     6    OPTIONS   (  location   =     'asia-southeast1'  )  ;     -- Singapore     7
8   CREATE     SCHEMA     `  project_id.retail_au_data  `     9    OPTIONS   (  location   =     'australia-southeast1'  )  ;     -- Sydney
```


## Step 1: Design the Ingestion Layer for Marketplace and E-commerce Data


### Marketplace API Integration Patterns


Shopee, Lazada, and Tokopedia each have their own API ecosystems with different rate limits, authentication flows, and data models. A common mistake is building bespoke integrations for each. Instead, normalise at the ingestion layer.


We use a combination of **Airbyte** (open-source, self-hosted on GKE or EKS) for marketplace connectors and custom Python extractors for APIs that Airbyte does not cover natively (Tokopedia's v2 API, for example). The pattern:


1. **Extract** raw JSON from each marketplace API into a landing zone (GCS bucket or S3 bucket, partitioned by source and date).
2. **Load** raw JSON into your warehouse's staging schema using COPY or external tables.
3. **Transform** using dbt (data build tool) to normalise orders, products, and customers into a shared schema.


```text
1  # dbt model: stg_shopee_orders.sql     2   {  {   config(materialized='incremental'  ,   unique_key='order_sn')   }  }     3
4  SELECT    5    order_sn AS order_id  ,     6    'shopee' AS channel  ,     7    JSON_EXTRACT_SCALAR(raw  ,   '$.buyer_username') AS customer_id  ,     8    CAST(JSON_EXTRACT_SCALAR(raw  ,   '$.total_amount') AS NUMERIC) AS order_total  ,     9    TIMESTAMP_SECONDS(CAST(JSON_EXTRACT_SCALAR(raw  ,   '$.create_time') AS INT64)) AS ordered_at  ,     10    JSON_EXTRACT_SCALAR(raw  ,   '$.order_status') AS status  ,     11    _loaded_at    12  FROM   {  {   source('raw'  ,   'shopee_orders_raw')   }  }     13   {  % if is_incremental() %  }     14  WHERE _loaded_at   >   (SELECT MAX(_loaded_at) FROM   {  {   this   }  }  )    15   {  % endif %  }
```


### E-commerce Platform Webhooks and Event Streams


Shopify and Adobe Commerce both support webhooks for real-time event data. For Shopify Plus stores, we configure mandatory webhooks for` orders/create` ,` orders/updated` ,` inventory_levels/update` , and` customers/update` , routing them through **Google Pub/Sub** or **AWS EventBridge** before landing in the warehouse.


Adobe Commerce (Magento 2.4.7+) offers native event integration through Adobe I/O Events. For self-hosted Magento instances — still common across Taiwan and Southeast Asia — we deploy a lightweight event publisher module that pushes to RabbitMQ, then bridge to the cloud via a Kafka Connect sink.


According to Shopify's 2024 Commerce Trends report, merchants using real-time inventory sync across three or more channels see a 23% reduction in overselling incidents.


### Handling POS Data from Offline Stores


POS integration is where most omnichannel architectures fail. The challenge: many POS systems in APAC retail were designed for batch uploads, not streaming.


For clients running Oracle Retail Xstore (versions 19+), we use the Transaction Broker API to capture basket-level data in near-real-time. For legacy POS systems that only export CSV or XML end-of-day files, we deploy an edge agent — a lightweight Python service running on a local server at each store — that watches the export directory and pushes new files to cloud storage via a secure tunnel.


```text
1  # Simplified POS file watcher (production version includes retry logic and checksums)     2   import   watchdog  .  events    3   import   watchdog  .  observers    4   from   google  .  cloud   import   storage    5
6   class     POSFileHandler  (  watchdog  .  events  .  FileSystemEventHandler  )  :     7        def     on_created  (  self  ,   event  )  :     8            if   event  .  src_path  .  endswith  (  '.csv'  )  :     9              client   =   storage  .  Client  (  )     10              bucket   =   client  .  bucket  (  'retail-pos-landing'  )     11              blob   =   bucket  .  blob  (  f'store_hk_001/  {  event  .  src_path  .  split  (  "/"  )  [  -  1  ]  }  '  )     12              blob  .  upload_from_filename  (  event  .  src_path  )
```


At Branch8, we deployed this pattern across 12 stores for a Hong Kong jewellery retailer in under 3 weeks. The edge agents run on existing store hardware — no new infrastructure required.


Ready to Transform Your Ecommerce Operations?


Branch8 specializes in ecommerce platform implementation and AI-powered automation solutions. Contact us today to discuss your ecommerce automation strategy.


[Get Started](https://branch8.com/contact)


## Step 2: Build the Unified Data Model


### The Star Schema That Actually Works for Omnichannel


Forget the ivory-tower data vault debates. For omnichannel retail analytics in APAC, a well-designed star schema with a few bridge tables delivers 90% of what you need with far less complexity.


Core fact tables:


- ` fct_orders` — one row per order line item, with foreign keys to channel, customer, product, store, and time dimensions
- ` fct_inventory_snapshots` — daily inventory positions by SKU, location, and channel
- ` fct_customer_events` — clickstream, email opens, app sessions, and in-store interactions


Core dimension tables:


- ` dim_customers` — the golden customer record (more on identity resolution below)
- ` dim_products` — unified product catalogue with marketplace-specific attribute mappings
- ` dim_channels` — Shopee SG, Lazada MY, Shopify AU, POS HK, etc.
- ` dim_locations` — stores, warehouses, 3PL fulfilment centres
- ` dim_time` — standard date dimension with APAC fiscal calendars (Australian FY starts July; HK and SG follow calendar year)


### Identity Resolution Across Channels


This is the hardest technical problem in omnichannel data architecture. A customer who buys on Shopee uses their Shopee username. The same person visits your Shopify store with a different email. They walk into your store and provide a phone number for loyalty.


You need a probabilistic + deterministic identity graph. Here is the approach we use:


1. **Deterministic matching** : Email normalisation (lowercase, remove dots for Gmail, strip plus-aliases), phone number normalisation to E.164 format with country code.
2. **Probabilistic matching** : Name + address fuzzy matching using Jaro-Winkler similarity (threshold ≥ 0.88) combined with purchase pattern analysis.
3. **Graph construction** : Build an identity graph in BigQuery using SQL-based connected components or use a purpose-built tool like **Amperity** or **FullContact** .


```text
1  -- Simplified deterministic identity matching in BigQuery     2   WITH   normalised   AS     (     3      SELECT     4      source_customer_id  ,     5      channel  ,     6      LOWER  (  REGEXP_REPLACE  (  email  ,   r  '\+.*@'  ,     '@'  )  )     AS   norm_email  ,     7      CONCAT  (  '+'  ,   country_code  ,   REGEXP_REPLACE  (  phone  ,   r  '[^0-9]'  ,     ''  )  )     AS   norm_phone    8      FROM     `  project.staging.all_customers  `     9   )     10   SELECT     11    FARM_FINGERPRINT  (  COALESCE  (  a  .  norm_email  ,   a  .  norm_phone  )  )     AS   unified_customer_id  ,     12    ARRAY_AGG  (  STRUCT  (  source_customer_id  ,   channel  )  )     AS   source_ids    13   FROM   normalised a    14   GROUP     BY     COALESCE  (  a  .  norm_email  ,   a  .  norm_phone  )
```


A 2024 study by Segment found that retailers with unified customer profiles see 33% higher repeat purchase rates compared to those operating channel-siloed customer data.


### Currency and Taxation Normalisation


APAC means multi-currency. Your fact tables need both local currency amounts and a normalised currency (typically USD or the company's reporting currency). We use daily exchange rates from the European Central Bank's free API, loaded into a` dim_exchange_rates` table, and apply conversion at the dbt transformation layer.


Tax treatment varies wildly: Australia has 10% GST, Singapore 9% GST (increased from 8% in January 2024), Malaysia has a Sales Tax and Service Tax (SST) regime, and Indonesia applies 11% VAT. Your` fct_orders` table should store both gross and net amounts with the applicable tax rate as a column, not a hardcoded calculation.


## Step 3: Implement the Real-Time Operational Layer


### Why Batch Is Not Enough for Inventory


Batch processing (hourly or daily) works for analytics and reporting. It does not work for inventory. If your Shopee store in Malaysia and your Shopify store in Australia draw from the same warehouse in Singapore, a one-hour lag means overselling.


You need a real-time operational layer alongside your analytical warehouse. The reference architecture:


- **Event streaming** : Google Pub/Sub or Apache Kafka (we use Confluent Cloud's APAC clusters for clients needing cross-cloud compatibility)
- **Operational database** : Redis for inventory counters, PostgreSQL or Cloud Spanner for transactional state
- **Sync engine** : A microservice that consumes inventory events, updates the operational database, and pushes stock updates to each channel's API


According to McKinsey's 2024 State of Retail report, retailers with real-time inventory visibility across channels achieve 15-25% higher sell-through rates.


### Event Schema Design for Omnichannel


Standardise your event schema across all sources. We use a modified CloudEvents specification:


```text
1  {     2      "specversion"  :     "1.0"  ,     3      "type"  :     "retail.order.created"  ,     4      "source"  :     "shopee/sg"  ,     5      "id"  :     "evt_a1b2c3d4"  ,     6      "time"  :     "2026-01-15T08:30:00+08:00"  ,     7      "datacontenttype"  :     "application/json"  ,     8      "data"  :     {     9        "order_id"  :     "SHP-SG-20260115-001"  ,     10        "channel"  :     "shopee_sg"  ,     11        "customer_id"  :     "cust_unified_xyz"  ,     12        "line_items"  :     [     13          {  "sku"  :     "RING-AU-750-001"  ,     "qty"  :     1  ,     "unit_price"  :     2850.00  ,     "currency"  :     "SGD"  }     14        ]  ,     15        "warehouse_id"  :     "wh_sg_01"     16      }     17   }
```


Every system — POS, e-commerce, marketplace — publishes events conforming to this schema. The streaming layer handles routing, the operational layer handles state, and the warehouse handles history.


### Building the Reverse Sync to Channels


Data flows two ways. Your warehouse receives data from channels, but your operational layer must also push data back: updated stock levels to Shopee, price changes to Lazada, new product listings to Shopify.


We build this as a set of channel-specific "publisher" microservices, each consuming from a dedicated Pub/Sub topic. Rate limiting is critical — Shopee's API allows 10 requests per second for inventory updates; Lazada allows 50. We use token-bucket rate limiters in each publisher service.


Ready to Transform Your Ecommerce Operations?


[Get Started](https://branch8.com/contact)


## Step 4: Add the AI and ML Layer


### Demand Forecasting Across Channels


With unified data in your warehouse, you can build demand forecasting models that account for cross-channel cannibalisation — something impossible with siloed data.


We use **Google Vertex AI** or **Amazon SageMaker** (depending on the client's cloud) with the following feature set:


- Historical sales by SKU × channel × location (from` fct_orders` )
- Marketplace promotion calendars (Shopee's 6.6, 9.9, 11.11 mega sales drive 40-60% of annual GMV for some sellers, according to iPrice Group's 2024 analysis)
- Weather data (surprisingly predictive for fashion and F&B categories in tropical APAC)
- Competitor pricing signals (scraped or from marketplace APIs)


The model outputs feed back into the operational layer to drive automated inventory allocation and reorder triggers.


### LLM-Powered Product Attribute Enrichment


One practical application of LLMs in omnichannel retail data architecture: automated product attribute normalisation. Each marketplace has different category taxonomies and required attributes. A product listed on Shopee SG needs different attributes than the same product on Lazada MY or Amazon AU.


We use GPT-4o via the Azure OpenAI Service (available in the Australia East and Southeast Asia regions) to:


1. Parse unstructured product descriptions into structured attributes
2. Generate marketplace-specific titles optimised for each platform's search algorithm
3. Translate and localise product descriptions across APAC languages


```text
1  # Product attribute extraction using Azure OpenAI     2   from   openai   import   AzureOpenAI    3
4  client   =   AzureOpenAI  (     5      azure_endpoint  =  "https://retail-ai-sea.openai.azure.com/"  ,     6      api_version  =  "2024-10-21"     7   )     8
9   def     extract_attributes  (  product_description  :     str  ,   target_marketplace  :     str  )     -  >     dict  :     10      response   =   client  .  chat  .  completions  .  create  (     11          model  =  "gpt-4o"  ,     12          messages  =  [     13                {  "role"  :     "system"  ,     "content"  :     f"Extract structured product attributes for   {  target_marketplace  }  . Return JSON with: category_path, brand, material, dimensions, colour, weight."  }  ,     14                {  "role"  :     "user"  ,     "content"  :   product_description  }     15            ]  ,     16          response_format  =  {  "type"  :     "json_object"  }     17        )     18        return   response  .  choices  [  0  ]  .  message  .  content
```


### Personalisation Engines That Respect APAC Privacy Norms


With your unified customer profile and event data, you can power real-time personalisation. But APAC consumers — particularly in Singapore, Australia, and Japan — are increasingly privacy-conscious. The CBRE Asia-Pacific Real Estate Market Outlook 2025 notes that consumer trust is now a top-three factor in brand loyalty across the region.


Build your personalisation on first-party data and declared preferences, not third-party cookies. Use your warehouse's ML capabilities (BigQuery ML or Snowpark ML) to train recommendation models on purchase history and explicit preference signals.


## Step 5: Operationalise with Monitoring and Data Quality


### Data Quality Checks That Prevent Revenue Loss


Bad data in an omnichannel system does not just produce wrong reports — it causes overselling, missed fulfilments, and customer complaints. Implement automated data quality checks at every layer:


- **Ingestion layer** : Schema validation on incoming events. Reject malformed payloads to a dead-letter queue.
- **Transformation layer** : dbt tests for uniqueness, not-null, accepted values, and referential integrity.
- **Serving layer** : Anomaly detection on key metrics. If today's order volume from Shopee PH drops 80% versus the 7-day average, alert immediately — it likely means an API credential expired, not a demand collapse.


```text
1  # dbt test: check for duplicate orders (a common Shopee webhook issue)     2   version  :     2     3   models  :     4      -     name  :   fct_orders    5        tests  :     6          -     unique  :     7              column_name  :     "CONCAT(channel, '-', order_id)"     8          -     not_null  :     9              column_name  :   unified_customer_id    10        columns  :     11          -     name  :   order_total    12            tests  :     13              -     dbt_utils.accepted_range  :     14                  min_value  :     0     15                  max_value  :     1000000
```


### Dashboard and Reporting Architecture


Build three tiers of reporting:


- **Executive dashboards** (Looker or Tableau): GMV by channel and market, blended margin, customer acquisition cost by channel, LTV:CAC ratios
- **Operational dashboards** (Grafana or Retool): Real-time inventory positions, fulfilment SLA adherence, API health status
- **Self-service analytics** (BigQuery + Sheets, or Snowflake + Hex): For merchandising and marketing teams to run ad-hoc queries


Do not build 50 dashboards on day one. Start with five and iterate. According to Gartner's 2024 Data & Analytics Survey, organisations that focus on fewer than 10 core dashboards report 2.4x higher user adoption rates.


Ready to Transform Your Ecommerce Operations?


[Get Started](https://branch8.com/contact)


## Common Mistakes and How to Avoid Them


### Mistake 1: Building Marketplace Integrations Without Rate-Limit Resilience


Shopee, Lazada, and Tokopedia all throttle API calls aggressively. If your ingestion pipeline does not handle 429 (Too Many Requests) responses with exponential backoff, you will have data gaps during peak periods — exactly when accurate data matters most. Always implement circuit breakers and queue-based retry mechanisms.


### Mistake 2: Ignoring Marketplace Data Delays


Shopee's Order API can lag 5-15 minutes during mega-sales. Lazada's order webhooks sometimes arrive out of order. Your architecture must be idempotent — processing the same event twice should not create duplicate records. Use upsert patterns in your staging tables, keyed on order ID + channel.


### Mistake 3: Underestimating Currency Conversion Complexity


Using a single daily exchange rate works for reporting but fails for margin calculation. If you purchased inventory in CNY, stored it in a Singapore warehouse, and sold it in AUD via Shopify, you need the conversion rate at the time of each transaction — not today's rate. Store the actual rate used at each transaction point.


### Mistake 4: Treating the Data Warehouse as the Operational System


BigQuery and Snowflake are analytical databases. They are not designed for sub-100ms point lookups that your checkout flow requires. We have seen retailers try to run real-time inventory checks against BigQuery and wonder why their site slows during traffic spikes. Keep your operational layer (Redis, Cloud Spanner, DynamoDB) separate from your analytical layer.


### Mistake 5: Neglecting Data Contracts Between Teams


When your e-commerce team changes the Shopify theme and adds a custom checkout field, your data pipeline breaks. Establish data contracts: formal agreements about what each system will produce, in what format, and at what frequency. Use tools like **Soda Core** or **Great Expectations** to enforce contracts automatically.


## Getting This Right on a Realistic Timeline


Here is a realistic timeline based on our implementation work at Branch8. For a mid-market APAC retailer with 3-5 channels and 50-200 stores:


- **Weeks 1-3** : Source inventory, data residency assessment, cloud environment setup
- **Weeks 4-8** : Ingestion layer build for top 3 channels + POS
- **Weeks 9-12** : Unified data model, identity resolution, dbt transformations
- **Weeks 13-16** : Real-time operational layer for inventory sync
- **Weeks 17-20** : AI/ML layer, dashboards, data quality automation
- **Weeks 21-24** : Remaining channels, performance tuning, team training


Six months to production for the full stack. You can get a minimum viable architecture — two channels, batch only, basic dashboards — in 8-10 weeks.


The cost? For a BigQuery-based architecture on GCP, expect US$3,000-8,000/month in cloud spend for the analytical layer, plus US$1,500-4,000/month for the streaming and operational components. Snowflake will run 20-30% higher for equivalent workloads in APAC regions due to credit pricing.


This omnichannel retail data architecture APAC guide for 2026 reflects patterns we have refined across dozens of deployments. The technology is mature. The challenge is execution discipline — getting ingestion, identity, and inventory right before layering on AI. If your team needs a partner to accelerate this build,[reach out to Branch8](https://branch8.com/contact) and we will scope it together.


Ready to Transform Your Ecommerce Operations?


[Get Started](https://branch8.com/contact)


## Sources


- NIQ Consumer Outlook Guide APAC 2025: https://nielseniq.com/global/en/insights/consumer-outlook-apac/
- Shopify Commerce Trends 2024: https://www.shopify.com/research/commerce-trends
- McKinsey State of Retail 2024: https://www.mckinsey.com/industries/retail/our-insights
- Gartner Data & Analytics Survey 2024: https://www.gartner.com/en/data-analytics
- CBRE Asia-Pacific Real Estate Market Outlook 2025: https://www.cbre.com/insights/books/asia-pacific-real-estate-market-outlook
- iPrice Group E-Commerce Map of Southeast Asia: https://iprice.sg/insights/mapofecommerce/
- Segment State of Personalization Report 2024: https://segment.com/state-of-personalization-report/
- Indonesia Government Regulation 71/2019 on Electronic Systems: https://jdih.kominfo.go.id/
