---
schema_version: "1.0.0"
document_id: "8fce419f4064f33aab3a29c9594eb846360b47d2916538d77d1960b92d777fff"
company_key: "ibotta-inc-class-a-common-stock"
company: "Ibotta Inc."
source_id: "ibotta-inc-class-a-common-stock-rss-dcf741155171"
canonical_url: "https://medium.com/building-ibotta/the-medallion-architecture-at-ibotta-b26841d6c45f"
published_at: "2023-01-10T18:52:56+00:00"
first_seen_at: "2026-07-25T01:07:04.216753+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:91ef7cf7b580be50db73aea6369a41ca82a009b5a5c916e86f1a700b95a1f873"
---

# The Medallion Architecture at Ibotta

# The Medallion Architecture at Ibotta


[Jaylyn Stoesz](https://medium.com/@jaylynstoesz?source=post_page---byline--b26841d6c45f---------------------------------------)


6 min read


·


Jan 6, 2023


--


[Ibotta](https://home.ibotta.com/) has been working towards implementing the[Medallion Architecture](https://www.databricks.com/glossary/medallion-architecture#:~:text=What%20is%20a%20medallion%20architecture,Silver%20%E2%87%92%20Gold%20layer%20tables).) in our data platform since early 2021. The model simply states that each of three tiers represents a different stage of processing: Bronze data is raw data captured and stored as-is from producers; Silver data is lightly processed and optimized for analytics, data science, and machine learning use; and Gold data is aggregated for analytics and business use with heightened quality rules applied.


Ibotta goes a step further and sets strict governance requirements for each tier — a Gold dataset is not Gold just because it’s an aggregate, or because our exceptional analytics & data science teams built it. It also meets a long list of technical and business requirements, ensuring that it complies with substantially elevated quality, resiliency, and governance expectations. Another unique feature of our model is the inclusion of a fourth “Purple” tier for production datasets which provide accurate and reliable reporting on our performance, behavioral, and or operational needs, but don’t necessarily require the rigor that goes into certifying a Gold dataset.


This article will outline what the specifications for Gold data are and why they matter, as well as each organization’s role in producing this data.


Press enter or click to view image in full size


## Definition & reasoning


Datasets which are eligible for Gold-tier certification at Ibotta are business-critical tables that our senior leadership team, auditors, business users, and select external partners see. **This includes, but is not limited to, datasets which are considered the source of truth for financial reporting and regulatory filings** .


Gold-certified data, or just “Gold data” for short, is strictly governed. This means it is highly reliable, thoroughly vetted and documented, and closely monitored (more on that later). Business users should ideally use this data for most of their day-to-day work. External users should, with few exceptions, use this data exclusively.


These datasets must be used for at least one critical utility. Due to the high level of effort it takes to certify them as Gold-tier quality, there has to be a good reason for putting them through this amount of rigor. Moreover, they must be comprehensive enough that the business can use them to answer their own questions without additional support from our technical teams in the majority of cases.


The reasons for having clear expectations for these tables should be pretty self-evident: the company owes it to its stakeholders and its employees to make good, well-justified, and well-documented judgment calls. We are best equipped to do that when the quality of our core data is guaranteed. Certification is therefore mandatory at Ibotta for any dataset used to make high-level strategic and business value-impacting decisions.


## Requirements


Gold data by our definition is, in simple terms, just extremely well-governed data which answers our most important business questions. This means that they are guaranteed to be:


- **Secure:** follow the Principle of Least Privilege where compliance rules require it, and are preserved as long as is necessary for regulatory and operational needs
- **Correct:** results are predictable, repeatable, and proven to accurately describe the behavior of the system producing them
- **Timely:** reliably delivered during the window of time when they provide the most value
- **Accessible:** modeled intuitively for end users who may not (and shouldn’t need to) have a complete understanding of how the data is produced, and accessible via business-friendly interfaces
- **Cost-effective:** generate more value for the business than what they cost, including lifecycle costs from requirements gathering to maintenance
- **Auditable:** robust mechanisms are in place to ensure that the above standards are met. Data lineage and change history are documented and easy to follow


There are entire books on each and all of these facets of data governance written by people who can explain them much more eloquently than I can in this blog post, but this is a reasonable framework on which to center the conversation.


Committing to excellent data governance should communicate that Ibotta sees these datasets as key data products rather than afterthoughts. The industry has often treated data as exhaust — a by-product that’s too expensive or difficult to manage, so it gets buried in an underground nuclear waste facility until the day some unfortunate data scientist has to spend late nights and thousands of dollars digging it out to solve an existential business problem. Instead, we use our data proactively to identify and solve those problems before they lead to a meltdown. All of the governance facets above are necessary for doing this well, and we expect everyone across the company to participate in this effort.


Our data teams have developed an extensive and specific list of technical specifications and business rules to ensure that our core datasets adhere to these standards (we won’t go into in detail here). What you do need to know is that implementation of these standards is a cross-functional effort that includes teams across the entire spectrum of the business, not just data teams.


## Responsibilities


The ideal answer to the question of ownership for Gold datasets is that those who are closest to the business logic are responsible for maintaining it (in accordance with the ever-more-popular[Data Mesh](https://martinfowler.com/articles/data-mesh-principles.html) paradigm). The real answer is more complex: everyone, from engineering to analytics to product, has an important role to play in ensuring that a Gold dataset is up to spec.


> Gold data is not just a product. It’s a cultural paradigm that everyone must participate in for our efforts to be successful.


Subsequent articles will outline specific expectations for each of the groups below, but here’s the gist:


- **Engineering*:** produce and monitor well-modeled and battle-tested data which adheres to[data contracts](https://dataproducts.substack.com/p/an-engineers-guide-to-data-contracts) defined in collaboration with data consumers
- **Data Platform Engineering**:** provide intuitive and reliable mechanisms for capturing data produced by upstream systems, store that data properly, and build out effective tooling for stakeholders to discover and access that data securely. The majority of governance tooling is built and maintained by this team
- **Analytics & Data Science:** collaborate with producers and stakeholders to design, implement, and maintain efficient data sets which answer critical business questions, and provide interfaces which allow end users to effectively leverage them
- **Product & Business Operations:** use domain expertise in collaboration with technical colleagues to develop datasets, models, and dashboards which meet a variety of needs for years to come, and proactively communicate when those needs change


**Engineering includes but is not limited to Software Engineering, DevOps, Cloud Infrastructure, and Security teams*


***Data Platform Engineering at Ibotta includes Data Engineering, Analytics Engineering, and Machine Learning/MLops teams which specialize in various areas of our data platform software*


Gold data is not just a product. It’s a cultural paradigm that everyone must participate in for our efforts to be successful. Subsequent articles will discuss how Ibotta addresses the substantial technical challenges of maturing our existing data ecosystem, as well as the role of each organization in meeting the high expectations we have set for ourselves.


## Non-critical (but important!) data


Of course, as we discussed earlier, we only require Gold-tier controls for business-critical datasets. But this doesn’t mean that all the other data we produce shouldn’t also meet high quality expectations.


Ibotta’s approach to the Medallion Architecture specifies an additional tier for data which is important to the business, but doesn’t necessarily drive company-wide decision-making. We call this category the **Purple tier** , and it includes aggregate datasets which may or may not merit the rigor applied during development of Gold data as we defined it above. Purple datasets can be built using Silver, Gold, and other Purple data.


> All of the must-have specs for Gold datasets are should-have specs for Purple datasets.


All of the tools we use to build Gold datasets are available for building Purple ones as well. In fact, we encourage dataset producers across the board to make use of these tools and methodologies to whatever degree is feasible for their project. They are designed to make producing high-quality data as fast and simple as possible, besides generally making data teams’ jobs easier. Moreover, if a Purple dataset eventually reaches the level of importance that it should be certified as Gold then the transition becomes significantly easier if it was largely built to standard in the first place.


That said, the simple difference is that except for security requirements (which are mandatory), all of the *must-have* specs for Gold datasets are *should-have* specs for Purple datasets *.* Producers can build non-critical datasets with whatever degree of governance makes sense for that use case.


## What’s next


The teams driving this initiative are hard at work developing the tools and processes needed for meeting these standards easily. We dedicated the last couple of years to laying the groundwork for making this transition; we are dedicating the next year (and onward) to building paved-road tooling for creating Gold-standard datasets, and shifting Ibotta’s culture to represent a shining example of excellent data governance. Expect more articles from the Ibotta data team with details of what that looks like in practice soon.


*Interested in working at Ibotta? Check out our*[careers page](https://ibotta.com/careers) *!*
