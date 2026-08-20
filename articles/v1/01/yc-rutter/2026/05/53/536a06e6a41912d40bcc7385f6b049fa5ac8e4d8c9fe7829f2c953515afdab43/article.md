---
schema_version: "1.0.0"
document_id: "536a06e6a41912d40bcc7385f6b049fa5ac8e4d8c9fe7829f2c953515afdab43"
company_key: "yc-rutter"
company: "Rutter"
source_id: "yc-rutter-news-import-c12c34cd87f8"
canonical_url: "https://www.rutter.com/blog/seller-data-api"
published_at: "2026-05-06T16:52:18.962+00:00"
first_seen_at: "2026-07-22T12:26:40.375919+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:43197651d99d36eef6d3c28988561338c91b8d28bafd2b80e657b00c839f9cd1"
---

# How ecommerce lenders verify revenue in real-time: a technical guide to seller data APIs

Revenue verification is the foundation of every ecommerce lending and financing product. Whether you're building merchant cash advances, revenue-based financing, or embedded lending within a payment platform, your ability to make accurate credit decisions depends on accessing reliable, granular, real-time seller data.


We've worked with some of the best ecommerce lenders in the market on exactly this problem. This guide covers the technical architecture for accessing and using seller data from ecommerce platforms, what data is available, how to structure your integration, and how leading lenders have built their pipelines.


## **Why bank statements aren't enough**


Bank statements show aggregate deposits and withdrawals. They don't tell you which product lines are growing, what the refund rate looks like, whether inventory is turning over, or how revenue is distributed across sales channels. For ecommerce businesses that sell across Shopify, Amazon, WooCommerce, and other platforms, the bank statement is a lossy summary of the actual business activity.


Modern ecommerce lenders need access to the source data: orders, refunds, products, customers, and transactions directly from the platforms where the revenue is generated. This data enables faster, more accurate credit decisions and supports ongoing monitoring that bank statements can't provide.


## **What seller data is available via API**


Each ecommerce platform exposes different data through its API, but the core objects relevant to lending and underwriting are consistent:


- **Orders:** The complete transaction record including order amount, line items, taxes, discounts, shipping, payment method, and timestamps. Order data is the most direct signal of revenue velocity.


- **Refunds and returns:** Critical for understanding net revenue and assessing product or service quality issues. A business with a 2% refund rate is a very different risk profile than one at 15%.


- **Products and inventory:** Product catalog data reveals the breadth and depth of a business's offering. Inventory levels indicate whether the business can fulfill future orders and how capital-intensive the operation is.


- **Customers:** Customer data reveals concentration risk. A business where a single customer represents 40% of revenue carries different risk than one with a diversified customer base.


- **Payouts and settlements:** The actual money flowing from the platform to the merchant's bank account. This ties the commerce data back to the financial reality.


When you combine this data across multiple platforms, you get a comprehensive, real-time picture of the business that would be impossible to assemble from bank statements alone. And when you layer in accounting data from QuickBooks or Xero alongside the commerce data, the picture gets even more complete. That cross-category view, commerce plus accounting through a single API, is something we're uniquely positioned to provide.


## **Technical implementation**


**Connecting to seller accounts**


The standard approach is to use an OAuth-based authentication flow where the merchant grants your application read access to their commerce platform data. A pre-built authentication widget handles the platform-specific OAuth flows and returns a connection that your application can query programmatically.


**Pulling historical data**


For underwriting, you typically need 6 to 24 months of historical order and transaction data. The initial data sync pulls this history into your pipeline. Depending on the volume of orders and the platform's API rate limits, this can take anywhere from minutes to a few hours. Because we store the data rather than acting as a passthrough, our customers aren't subject to rate limit constraints when querying historical data after the initial sync.


**Normalizing across platforms**


This is where a unified API provides significant value. A Shopify order has a different structure from an Amazon order, which differs from a WooCommerce order. Field names, data types, and nested object structures vary across platforms. A unified API normalizes these into a consistent schema so your underwriting model works the same regardless of which platform the merchant uses.


**Computing risk signals**


With normalized data in your pipeline, you can compute the signals your model needs. Leading ecommerce lenders typically focus on monthly and trailing-twelve-month revenue, month-over-month revenue growth rate, net revenue after refunds and chargebacks, average order value trends, customer concentration metrics, seasonal revenue patterns, and product category diversification. These signals feed directly into credit scoring models and can be updated in real time as new data flows in.


**Ongoing monitoring**


Webhooks enable real-time monitoring after the initial credit decision. When a new order is placed, a refund is issued, or a payout is processed, your system receives a notification and can update the merchant's risk profile. This is essential for revolving credit products and portfolio monitoring.


## **How leading lenders have built this**


Ramp built their commerce sales-based underwriting pipeline using our Commerce API to connect to Shopify, Stripe, Etsy, and other platforms. Before adding commerce data to their underwriting process, some businesses were getting lower credit limits than they qualified for because Ramp didn't have a full picture of their financial health. Adding real-time seller data allowed them to offer more accurate limits while maintaining risk controls.


Uncapped spent a year building commerce integrations in-house before switching to a unified API. After rolling out Shopify and Stripe integrations themselves, they faced a roadmap of Amazon, WooCommerce, Magento, and others that would have consumed their engineering team for months. Using a unified API, they rolled out new integrations five times faster.


Outfund saved approximately 960 hours of engineering time by using a unified API instead of building each commerce integration individually. As an early-stage startup, those engineering hours redirected to core product development were the difference between hitting and missing product milestones.


Kickfurther needed SKU-level data and inventory data that wasn't available from bank-level data sources. Connecting directly to commerce platforms gave them the granularity they needed to assess businesses on their marketplace, resulting in a reported tenfold improvement in efficiency and quality of their underwriting process.


## **Getting started**


If you're building a lending or financing product for ecommerce businesses, the architecture is straightforward: connect to the merchant's commerce platforms, pull historical and real-time data, normalize it into a consistent schema, and feed it into your underwriting model. A unified commerce API handles the first three steps so your team can focus on the model and the product experience.


We work with every customer to map the specific data flows and API calls required for their use case, so if you're evaluating this architecture, reach out and we can walk you through how other lenders have implemented it.


‍
