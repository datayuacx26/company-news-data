---
schema_version: "1.0.0"
document_id: "4f8093f44129a01c0cec4c8d7106c3c258278b28d7b9bc4114f60de73dadbbfa"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/streamline-data-consumption-with-cda-lifecycle-events-and-aws-eventbridge"
published_at: "2025-09-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:7d3e71dc8e7eef3028981c75917a5157a0b3e5726ca979f2b4b7d499e277b86d"
---

# Streamline Data Consumption with CDA Lifecycle Events and AWS EventBridge

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


- Streamline Data Consumption with CDA Lifecycle Events and AWS EventBridge


Updated: September 18, 2025


Modern data pipelines require more than just access to information. They demand timely, automated, and scalable data movement. For organizations using Guidewire Cloud Data Access (CDA), CDA Lifecycle Events represent a powerful new capability to streamline downstream data consumption, reduce operational overhead, and unlock seamless data ingestion.


In this post, we’ll explore what CDA Lifecycle Events are, how they work, why they matter, and how your team can integrate with AWS EventBridge to build smarter, event-driven data consumer service.


## What are CDA Lifecycle Events?


CDA Lifecycle Events are event messages emitted by Guidewire’s Cloud Data Access service when significant milestones occur in its data processing pipeline.


Instead of constantly polling your S3 buckets and parsing manifest files, you can subscribe to events delivered via AWS EventBridge and respond programmatically to changes in your data.


Here’s how it works:


- CDA generates event messages following the CloudEvents specification - an open, standardized format for describing events.
- Events include metadata such as the affected table, batch type, timestamps, and S3 file paths.
- When enabled, these events are delivered to your AWS EventBridge event bus, allowing downstream systems to take action.


## Types of CDA Lifecycle Events


CDA emits different event types based on whether the system is operating in **bulk mode** or **streaming mode** .


**Event Type** **Mode** **Description**


batchModeTableWrittenOut


Bulk


Emitted when a table has been successfully written during the initial batch process.


batchModeCompleted


Bulk


Indicates the entire initial bulk batch is finished and streaming mode will begin.


streamingBatchCompleted


Streaming


Signals the completion of a streaming micro-batch.


tableSchemaChanged


Streaming


Triggered when CDA detects schema changes in one or more tables.


Each event includes[metadata](https://docs.guidewire.com/cloud/cda/guide/latest/cda/r_cda-lc-events.html) like table name, tenant ID, batch ID, and the S3 file path where records reside.


## 5 Reasons Why to Use CDA Lifecycle Events


Integrating CDA Lifecycle Events can unlock major efficiencies for data teams. Here’s how:


1. **Reduce Operational Overhead**
Without events, you’d need to poll S3 buckets, fetch the latest manifest, and determine what changed which is a manual, error-prone process. Lifecycle Events improve this model by pushing changes to you in real time.
2. **Streamline and Automate Downstream Consumption**
Build actions based on the batch completion event. These events include all relevant metadata needed to consume the data and, most importantly, the S3 file path.


This allows you to directly access the contents of a folder, instead of scanning through numerous micro-batch timestamp folders to determine what changed.
3. **Accelerate Integration Development**
EventBridge makes it easy to connect to services with minimal custom code. Event sources and targets are decoupled, meaning your architecture stays clean and scalable.
4. **Gain Observability**
Each streamingBatchCompleted event includes operational metrics, such as:


- Last update time
- Batch latency
- Number of records processed


These metrics help downstream systems monitor performance and detect anomalies.


5. **Improve Auditability**
Need to track exactly how many records were delivered and when? Lifecycle Events offer batch-level auditing right out of the box.


## Key Concepts to Understand


Before integrating, it’s useful to understand the terminology behind CDA and EventBridge:


- **Tenant:** Your organization’s instance in Guidewire Cloud (one per insurer).
- **Planet:** A Guidewire environment (e.g., dev1, qa2, prod) associated with one or more applications.
- **Event:** A structured notification that a lifecycle milestone has occurred in CDA.
- **Partner Event Source:** An AWS EventBridge source created by Guidewire for your account and environment.


## Step-by-Step: Integrating with CDA Lifecycle Events using Guidewire


1. **Enable Lifecycle Events**
If your tenant has access to the CDA Self-Service feature, you can enable lifecycle event delivery yourself by following the instructions in the CDA documentation.
If Self-Service is not enabled for your tenant, please submit a request to Guidewire to enable it on your behalf. By default, CDA does not send lifecycle events.


[Step 1: Request CDA lifecycle events](https://docs.guidewire.com/cloud/cda/guide/latest/cda/c_lifecycle_events.html)
* *Please log in to the portal for direct access to resource.*
2. **Accept the Event Source in AWS**
This step creates an event bus that is associated with CDA's integration for that planet.


[Step 2: Accept CDA events in Amazon EventBridge](https://docs.guidewire.com/cloud/cda/guide/latest/cda/c_lifecycle_events.html)
3. **Create Event Rules & Targets**
Set up EventBridge rules to filter event types (e.g., streamingBatchCompleted) and direct them to AWS services like CloudWatch and SNS topics.


[Step 3: Create rules and configure a target](https://docs.guidewire.com/cloud/cda/guide/latest/cda/c_lifecycle_events.html)
4. **Update Your CDA Client**
Incorporate event metadata into your downstream data consumption logic — no need to scan the S3 bucket manually.


## New: AWS EventBridge Partner Event Sources Integration


We are excited to announce a significant enhancement to our integration capabilities. Guidewire is now part of the AWS EventBridge Partner Event Sources ecosystem. This new collaboration simplifies the integration process, as customers can now find Guidewire directly within the AWS EventBridge Partner Event Sources console and quickly navigate to our documentation to complete the integration steps. This partnership not only makes it easier for you to connect your Guidewire data but also enhances our visibility and credibility as a trusted data platform partner within the AWS ecosystem.


For more information, you can visit the following links:


- [Main AWS EventBridge partners integration page](https://aws.amazon.com/eventbridge/integrations/)
- [AWS Public docs page where Guidewire is listed](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-saas.html)
- [Guidewire on the AWS EventBridge Partner Event Sources console](https://console.aws.amazon.com/events/?page=overview#/partners/guidewire.com)


## Conclusion


CDA Lifecycle Events help data teams modernize how they interact with their Guidewire Cloud data — moving from **polling and parsing** to **event-driven automation** . With rich metadata, native AWS integration, and support for real-time use cases, this capability reduces overhead, boosts observability, and enables scalable, decoupled architectures.


If you’re a CDA user ready to simplify your data workflows, now’s the time to enable CDA Lifecycle Events and build smarter integrations.


**Give it a try:**[Explore the official documentation](https://docs.guidewire.com/cloud/cda/guide/latest/cda/c_lifecycle_events.html) and enable lifecycle event delivery in your own environment. Don’t have Self-Service access? Contact your Guidewire representative to get started.


## References


- [Guidewire CDA Lifecycle Events Documentation](https://docs.guidewire.com/cloud/cda/guide/latest/cda/c_lifecycle_events.html)
- [AWS EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html)
- [CloudEvents Spec](https://cloudevents.io/)
- [AWS EventBridge Targets](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-targets.html)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
