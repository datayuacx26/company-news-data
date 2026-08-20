---
schema_version: "1.0.0"
document_id: "fe460f9ce42abd18740da32f21e494509a9e734470aa2c3cb2ed4a4f889916d8"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/optimizing-guidewire-cloud-data-bulk-loads"
published_at: "2025-09-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:579131273738fd727904508d17b63fcca75b48098abf6f42d20188b488a258b0"
---

# Optimizing Guidewire Cloud Data (CDA) Bulk Loads: A 50% Reduction in Ingestion Time

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


- Optimizing Guidewire Cloud Data (CDA) Bulk Loads: A 50% Reduction in Ingestion Time


For developers and technical leads working with Guidewire Cloud Data (CDA), efficient data ingestion is paramount. Whether you're a prospective customer evaluating CDA for large datasets or an existing customer needing to redeploy, the time it takes to get your data into the cloud directly impacts project timelines and operational efficiency. Traditionally, bulk data ingestion for large datasets on CDA has been a time-consuming process, with a baseline rate of approximately 24-28 hours per terabyte (TB) for PostgreSQL-based data products.


In this post, we'll dive into the significant improvements Guidewire has implemented to tackle this challenge. We'll explore the three key optimization phases that have collectively reduced bulk mode ingestion time by approximately 50%, enabling faster data availability and improved efficiency in your data pipelines. We'll also highlight a crucial feature, blocklist customization, that allows you to further optimize ingestion and streamline your CDA experience.


## The Bulk Load Challenge


Imagine needing to onboard several terabytes of data onto CDA. At a rate of 24-28 hours per TB, this can quickly add up to days of waiting, impacting development cycles and business readiness. This inherent challenge of bulk data ingestion, often referred to as a snapshot process during CDA deployment, underscored the need for a faster and more efficient solution. Developers like you often need to quickly access critical information to plan and deliver quality work, and slow data ingestion directly impedes this crucial process.


## The Solution: Phased Optimizations for Faster Ingestion


Guidewire has introduced three strategic optimization phases to reduce end-to-end data ingestion time. These improvements address common developer pain points, such as the need for easier access to information and enhanced training resources, by making the data platform more responsive and efficient.


### Phase 1: Multi-threaded Processing and Optimized Resource Management


The first phase focuses on maximizing compute and memory utilization during the data snapshot process.


- Multi-threaded Processing: We've enabled multiple read threads per data product, which significantly speeds up the snapshot process. Read operations are now split across these threads, leading to increased throughput.
- Optimized Instance Types: To prevent bottlenecks and enable faster processing, we've moved to larger instance types, improving CPU and memory capacity.


Impact: These enhancements result in faster data reads and improved resource utilization, directly contributing to a more efficient bulk load.


### Phase 2: Blocklist Customization for Improved Efficiency


The introduction of the blocklist customization feature empowers you with granular control over your ingestion volume. This feature allows you to exclude up to ten large, low-value tables that aren't required for curation downstream processing from bulk ingestion.


- Granular Control: You can now define specific tables to exclude, reducing unnecessary data ingestion.
- Real-time Data Availability: While historical data for previously blocked tables won't be available via bulk load, these tables can be included later in streaming mode, ensuring continuous data flow from that point forward.
- Flexibility: This provides flexibility to optimize ingestion per tenant and per data product, leading to variable improvements depending on which tables are excluded.


Impact: Blocklist customization directly reduces unnecessary data ingestion, saving valuable time and storage.


### Phase 3: Parallel Connectors (Available in Olos)


Looking ahead to the O release, we are introducing parallel connectors, which will further revolutionize bulk ingestion. This phase will enable us to:


- Parallelize Reads: Use multiple connectors to parallelize reads directly from the database.
- Parallelize Writes: Parallelize writes to Kafka for significantly better throughput.


Impact: This will deliver an additional 40% reduction in bulk ingestion time. Parallel connectors are designed to handle high-volume workloads more efficiently and eliminate bottlenecks caused by a single connector, offering a robust solution for managing large datasets.


## Final Impact: A 50% Reduction in Bulk Load Time


Collectively, these three phases are projected to reduce bulk load time by approximately 50% per TB. Consider a baseline where a 1TB PostgreSQL database bulk load took 24 hours to complete. After the N release, with these optimizations, the bulk load time will be reduced to 12 hours or even less, depending on your blocklist customization. This is a significant improvement that directly translates to faster data availability and enhanced operational efficiency for your Guidewire Cloud Data initiatives.


## How to Get Started


The good news is that multi-threaded processing, optimized resource management, and parallel connectors are automatically leveraged during CDA data product bulk load in production. You don't need to take any specific action to benefit from these system-level improvements.


However, to truly optimize your bulk load times, we highly encourage you to use the blocklist customization feature. By strategically excluding unnecessary tables in bulk mode, you can gain valuable hours back in your data ingestion process. You can find more information about currently block-listed tables in our documentation:
[https://docs.guidewire.com/cloud/dataplatform/topics/r_pc-blocklist.html](https://docs.guidewire.com/cloud/dataplatform/topics/r_pc-blocklist.html) .


## Closing & Next Steps


The continuous improvement of Guidewire Cloud Data bulk load capabilities underscores our commitment to providing powerful and efficient tools for developers. By significantly reducing ingestion times, we empower you to deliver quality work faster and focus on what truly matters: building innovative solutions on the Guidewire platform. This ongoing effort reflects our role as a long-term business partner, investing in solutions that align with your strategic goals.


We encourage you to:


- Use the blocklist customization option to optimize bulk load time in your data products as per your needs.
- Test the impact of system changes and blocklist configuration on your bulk load times.
- Provide feedback to your Customer Success Manager to help us refine and enhance the feature further.


Explore other foundational posts in our blog library to stay updated on the latest Guidewire advancements by subscribing to our **Developer Newsletter** .


[Subscribe Here](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
