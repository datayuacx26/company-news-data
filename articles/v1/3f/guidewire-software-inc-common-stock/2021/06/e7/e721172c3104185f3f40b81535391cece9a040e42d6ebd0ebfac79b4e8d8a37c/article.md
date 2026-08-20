---
schema_version: "1.0.0"
document_id: "e721172c3104185f3f40b81535391cece9a040e42d6ebd0ebfac79b4e8d8a37c"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/technology/cortina-cloud-faster-data-and-analytics-smarter"
published_at: "2021-06-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:75fb5caf30243ab4b9e35efeabf6b27cf54c67a28f2a703305a590c36ca31b6d"
---

# Cortina. Cloud Faster. Data and Analytics Smarter.

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


- [Technology](https://www.guidewire.com/resources/blog/technology)


- Cortina. Cloud Faster. Data and Analytics Smarter.


Companies are making significant adjustments to their tech stacks as technology trends[evolve](https://a16z.com/2021/04/12/how-tech-stacks-up-in-b2b/) , finding new opportunities to engage, innovate, and grow efficiently. One trend I am particularly excited about is that *data is the new software* , which points to the rise of data infrastructure and a data development toolchain.


Anyone who works with data knows that making it useful and accessible is a challenge. We often hear that up to 80% of the time in an[analytics](https://www.guidewire.com/products/analytics) project is spent getting and organizing the data. This is why so many projects never get started in the first place—and why insurers have armies of frustrated data scientists and actuaries who want to be modeling and exploring new techniques but instead are spending most of their time wrangling data. This is also why the heads of the businesses are wondering, “Where is the return on analytics investment?”


Guidewire is uniquely positioned to solve this problem for our customers. It all starts with our updated cloud marketecture that we unveiled with the Cortina release.


The foundational component in the marketecture is[Guidewire Cloud Platform (GWCP)](https://www.guidewire.com/products/technology/guidewire-cloud) —a collection of value-added infrastructure and common services that powers Guidewire’s applications and solutions. This includes Data and Analytics Services that enable insurers to leverage their own data, accelerate time-to-insight with data curation, and power closed-loop analytics to turn innovative ideas into results. To better understand this, let’s drill down into this layer. This reveals the most complete P&C data platform delivered as a cloud service.


The data platform gathers data from a number of sources, such as core systems and external and third-party data sources, as well as contributory data from the Guidewire customer community. Data is collected into both a data lake–type repository through streaming and a data warehouse–type repository through batch ETL.


The data is then curated and organized into an operational data store (ODS)–type format:


1.


First, the data is auto-curated using the metadata to derive more business-friendly values, then it is denormalized based on relationship metadata.


2.


Next, the platform adds additional curation transformation and derivative metrics for reusable consumption.


3.


Finally, the data is organized into business-ready data sets.


The value-added services use the curated data to support various customer data usage scenarios and power Guidewire applications and solutions. Each release builds on the value-added services. Let’s drill down further into this layer of value-added services to see the data platform in action and what’s new with the Cortina release.


**Data Access Services**


These are services to enable easy access to data in both synchronous and asynchronous manners. For data access in near real time at a low latency, use the replicated data streaming (Cloud Data Access) and the curated data access (Data Studio) services. Additional services enable data access in batch mode and through APIs for external data collected through the data listening engine. The Integration Gateway provides synchronous subscription and consumption of business events.


Cortina adds the following capabilities across the various Data Access Services:


1.


Data Studio—a new[Jutro](https://www.guidewire.com/products/technology/guidewire-jutro-digital-platform) -based service—available as[Early Access](https://www.guidewire.com/products/technology/guidewire-early-access-program) . Data Studio is a business data set creation and management tool with a familiar SQL editor interface that provides unified access to both raw and pre-curated data.


2.


Provisioning of Cloud Data Access for customers in Australia, New Zealand, and Canada.


3.


Data retention policies for regulatory data protection compliance with Cloud Data Access.


4.


Business-ready data sets for Initial Segmentation for Auto/Motor Claims solution in our[claims management software](https://www.guidewire.com/products/core-products/insurancesuite/claimcenter-claims-management-software) , ClaimCenter, and Underwriting Submission Prioritization for Business Owners Policy (BOP) solution in[PolicyCenter](https://www.guidewire.com/products/core-products/insurancesuite/policycenter-insurance-policy-administration) .


5.


API call to the NHTSA (National Highway Transportation Safety Administration) to append vehicle identification number (VIN) data to modeling data sets.


**Data Science and Analytics Services**


These services support a diverse range of analytic workloads. The Real-Time Operational Business Intelligence (BI) (also called Explore) service provides numerous claims and policy visualizations and dashboards. The Predictive Modeling (also called Predictive Analytics) service enables the build, import, deployment, integration, and monitoring of predictive models. The Benchmarking service (also called Compare) enables[claims analysis](https://www.guidewire.com/products/analytics/compare) and peer comparisons for ClaimCenter users. The Risk Assessment service (also called[Cyence](https://www.guidewire.com/products/analytics/cyence) for Small Business) enables risk differentiation and continuous underwriting by leveraging non-obvious and behavioral data.


Cortina adds the following capabilities across the various Data Science and Analytics Services:


1.


Expanded text-mining capabilities to convert unstructured text data into structured features for modeling. Specifically, Cortina adds the ability to use fastText via Amazon Sagemaker BlazingText, BERT, and TF-IDF techniques.


2.


Integration of[Cyence](https://www.guidewire.com/products/analytics/cyence) risk assessment and risk factors with[InsuranceNow](https://www.guidewire.com/products/core-products/insurancenow) .


3.


Import models built using Python's scikit-learn libraries through BYOM (Bring Your Own Model).


4.


New personal auto segmentation use case that models for total loss, injury severity, and likelihood of an unreported injury. This use case is being leveraged by Guidewire Claims Autopilot.


5.


Enable modelers to set thresholds for feature drifts and get automated alerts through visual cues.


As customers use these value-added services and Guidewire keeps adding more, the data platform becomes more and more powerful. We are excited to share these fantastic Cortina features with you, and we are already working on future releases to deliver even more improvements and innovations.


To learn more, please watch these breakout sessions from our most recent[Connections Reimagined](https://em.guidewire.com/MTQwLUxIWC02ODMAAAF9CCU8NpZIXLMns-Kq_WTcvLreNj_2u51Kz4EJuM8Cri9NAnWas2sU8YhAp_Mdix9QKhUn8WE=) : “Smart Claims Assignment: The Amica Story” and “Liberate Your Data with Data Studio.” And be sure to read our[press release](https://www.guidewire.com/about-us/news-and-events/press-releases/20210511/guidewire-announces-cortina-release-introducing-new-integration-framework) and previous blog posts about[Cortina](https://view.ceros.com/guidewire-software/cortina-cloud-faster/p/1) :


-


[Cortina. Cloud Faster.](https://www.guidewire.com/blog/technology/cortina-cloud-faster)


-


[Deliver Business Value Faster and Easier with Guidewire Cloud Infrastructure](https://www.guidewire.com/blog/technology/deliver-business-value-faster-and-easier-with-guidewire-cloud-infrastructure)


-


[Application Services Brings the “Chatter” to Cortina](https://www.guidewire.com/blog/technology/application-services-brings-the-chatter-to-cortina)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
