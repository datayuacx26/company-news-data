---
schema_version: "1.0.0"
document_id: "3e8ca16e9bae41c08ab251ae0a3ae1831b4e6e63748f41734346d9c6fb899053"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/get-to-know-cloud-data-access-webinar-guide-and-highlights"
published_at: "2025-08-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:682c20c048fbe346ee5f0d88a334fe98b161a6f43245dc4617ecd569def3ebae"
---

# Get to Know Cloud Data Access (CDA): Webinar Guide and Highlights

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Developers](https://www.guidewire.com/resources/blog/developers)


- Get to Know Cloud Data Access (CDA): Webinar Guide and Highlights


Guidewire’s Get to Know the Platform Better webinar series recently spotlighted Cloud Data Access (CDA) – a transformative feature for insurers looking to maximize the value of their data in the cloud. In this recap, we’ll walk through the key takeaways presented by Brady Haggstrom, Product Marketer at Guidewire, and Subha Sathiam, Senior Product Manager at Guidewire. The webinar agenda covered CDA’s background, the challenges it solves, its primary features and benefits, a product demo, a look at the roadmap, and how you can get started. You can[watch the full video here.](https://explore.guidewire.com/c/_sziynwhuo0?x=DgFSF3)


## The Origins of CDA: Meeting Evolving Data Needs


CDA was born from Guidewire’s early cloud journey, recognizing that insurers needed robust, flexible access to their InsuranceSuite (IS) data for analytics and downstream systems. From the very first cloud release (Aspen) of the Guidewire Cloud Platform (GWCP), the focus was on building a data streaming service that delivers high-quality, accessible data to support evolving business needs. While multiple data access options exist, CDA has emerged as the recommended approach for most cloud customers.


## The Problem: Legacy Data Access Limitations


Insurers migrating to the cloud often face three major challenges:


- **Delayed Data Access** : Traditional methods only allow data refreshes at set intervals, creating lags for downstream analytics.
- **Limited Historical Change Data** : Capturing and tracking all historical changes requires specialized skills and infrastructure.
- **Preserving Existing Investments** : Many insurers have significant investments in on-premises data warehouses and analytics platforms, which they want to retain as they move to the cloud.


CDA addresses these pain points by enabling near real-time access, comprehensive change data capture, and seamless integration with existing data ecosystems.


## Introducing CDA: How It Works


CDA is a data streaming service within the Guidewire Data Platform, designed to provide simplified, near real-time access to all historical changes from InsuranceSuite systems.


How it works:


- When an agent enters claim data in ClaimCenter, change records are created in the InsuranceSuite database.
- CDA processes these records, performing deduplication, schema evolution, and operational checks.
- The processed data is delivered to an AWS S3 bucket, ready for ingestion into the customer’s data warehouse or analytics platform.


## Key Benefits of CDA


1. **Easy, Quick Access to Change Data Capture (CDC)** :


- Self-service UI for provisioning in non-production environments.
- Delivers InsuranceSuite data to S3 exactly once, with the latest schema.


2. **Continuous, Near Real-Time Streaming** :


- Keeps analytical and reporting systems in sync with the latest data.
- Enables faster, more granular insights and decision-making.


3. **Efficient Data Integration** :


- Supports integration with existing data warehouses and tools.
- Preserves prior investments and fits seamlessly into enterprise data strategies.


## Who Uses CDA?


CDA’s value spans multiple roles throughout its lifecycle:


- **Cloud Engineers** : Set up authentication and authorization.
- **Data Integration Specialists** : Analyze CDC data and define requirements for downstream consumption.
- **Data Engineers** : Architect and instrument data pipelines, ensuring compatibility with existing systems.
- **Business and Data Analysts** : Derive insights, build reports, and develop analytics solutions using CDA-delivered data.


## Demo Highlights: Self-Service Provisioning and New Features


The webinar demo showcased how easy it is to provision CDA via Guidewire Home’s self-service interface. Users can select their IS application, choose a CDA profile, and monitor the provisioning process—all from a unified dashboard.


Two recent feature highlights:


- **Operational Metrics** : Now available in the UI, these metrics provide visibility into CDA performance (e.g., batch write time, latency, throughput) and will soon integrate with Datadog for advanced observability.
- **Data Integrity Service (DIS)** : Ensures the integrity of data delivered to S3 by detecting any data loss during replication. If discrepancies occur, Guidewire’s operations team is alerted and coordinates remediation with the customer.


## Looking Ahead: Roadmap Priorities


Guidewire continues to invest in CDA, focusing on:


- Resiliency: Enhancing availability and the ability to recover from regional failures or disasters.
- Scalability: Allowing users to select data pipelines with increased throughput to match operational data volumes as needs grow.


## Getting Started with CDA


Ready to explore CDA? Here are your next steps:


- **Spin up a non-production plant** : Use Guidewire Home to set up CDA and start experimenting.
- **Consult Guidewire documentation** : Access in-depth guidance and best practices at[docs.guidewire.com](http://docs.guidewire.com/) .
- **Reach out to your Guidewire representative** : Get personalized support for CDA adoption.


CDA is helping insurers unlock the full potential of their data in the cloud—delivering speed, flexibility, and confidence as they modernize their operations. We look forward to seeing how you leverage CDA to drive your data strategy forward.


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
