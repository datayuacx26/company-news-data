---
schema_version: "1.0.0"
document_id: "69e66bf87ed2d97c1867029f1ed05bd43b59f078138b6847b78274e06271b211"
company_key: "thredup-inc-class-a-common-stock"
company: "ThredUp Inc."
source_id: "thredup-inc-class-a-common-stock-rss-cddb21541ec6"
canonical_url: "https://medium.com/connect-the-dots/thredups-journey-with-databricks-modernizing-our-data-infrastructure-e6ce8a6cc58a"
published_at: "2024-10-25T19:28:03+00:00"
first_seen_at: "2026-07-20T23:18:30.437987+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:340f8fa32504e413fd8d90ff5fcaacc32d5122e527a2849533a03bdcec083bb0"
---

# ThredUp’s Journey with Databricks: Modernizing Our Data Infrastructure

Databricks


Data Lakehouse


Data Engineering


Data Modernization


AI


# ThredUp’s Journey with Databricks: Modernizing Our Data Infrastructure


[Chintan](https://medium.com/@chintanspatel?source=post_page---byline--e6ce8a6cc58a---------------------------------------)


9 min read


·


Oct 25, 2024


--


Press enter or click to view image in full size


## Introduction


Founded in 2009, we at **ThredUp** have established ourselves as a leader in the **fashion resale market** , creating a platform for buying and selling secondhand clothing. As a company that originated in the **AWS cloud** , we’ve always been at the forefront of technological innovation, consistently adopting advanced solutions to streamline our operations. Our proprietary technologies, supported by multiple patents, have been instrumental in enhancing automation and operational efficiency. With advanced AI and *machine learning capabilities* , we’ve been able to process more than **220 million single SKU items** — where each item is treated as unique, a challenge specific to the resale market.


To address the growing complexity and volume of our data, our data platform needed to evolve, and we selected **Databricks** as a pivotal solution. This platform has enabled us to efficiently manage, process, and analyze vast datasets, further supporting our leadership in the resale space at scale. In this article, we demonstrate how this transformation took shape with Databricks, starting from our first use case in 2017 and progressing through *ongoing innovations* .


## The Initial Phase: Using Databricks Notebooks (2017)


In 2017, we began our Databricks journey with **Databricks Notebooks** , providing our data science team with a *flexible* and *collaborative environment* for complex data analysis and model development. At the time, **Redshift** was our dominant data warehouse for analytics. However, as the demand for more advanced data science and **ML workloads** grew, we started shifting some tasks to Databricks. For instance, we moved batch-based prediction models, such as user purchase probability, leveraging **Spark** and **SciKit-Learn** at that time.


The ease of use and powerful features of Databricks Notebooks allowed our data scientists to derive insights more efficiently. Coupled with a **managed Spark environment** , Databricks enabled us to experiment faster and bring new use cases to production more quickly.


## Building a Robust Data Infrastructure: Integration of Delta Lake (2018–2019)


Between 2018 and 2019, we integrated **Delta Lake** into our in-house **Event Bus System** — a centralized **Kafka** consumer that processed payloads from several topics, enriching audit information and optimizing performance before syncing to **S3** . This integration provided a reliable and scalable data storage solution, ensuring high-quality, consistent data for downstream processing by relying on Delta Lake’s **ACID transactions** and *schema enforcement capabilities* .


At this stage, our **Delta tables** on S3 were primarily used as **Spectrum views** in Redshift while also serving ML model development on Databricks.


## Expanding Databricks Usage: Data Engineering Adoption (2019–2021)


Between 2019 and 2021, **Databricks Notebooks** became the primary tool for our data engineers. While we incorporated **dbt** by 2020–21 primarily for **analytics engineering** tasks, Databricks Notebooks often proved indispensable for more complex workflows. The ability to integrate programming constructs and seamlessly switch between **Python** , **Scala** , and **R** — while incorporating **SQL processing** where necessary — made Databricks highly versatile.


During this period, we began using Databricks for large **ETL/ELT jobs** , gradually moving workloads away from **Talend** . By leveraging distributed data processing in Databricks, we were able to handle our most complex workloads more efficiently, giving us more control over data processing and pipeline management.


## Challenges with Redshift (late 2021)


By late 2021, we began encountering several challenges with our legacy **Amazon Redshift** system as our data needs expanded.


- **Performance bottlenecks** in BI, ETL, and ad-hoc workloads hindered scalability as data needs grew.
- Frequent **fine-tuning of WLM (Workload Management) queues** and regular **system restarts** due to deadlocks, disrupting operational stability.
- **Serializable isolation violations** caused by concurrent transactions led to conflicts and system restarts.
- **Slow query performance** for data on S3 via Spectrum tables, resulting in inefficiencies.
- To meet strict **SLAs** , data often had to be **duplicated between S3 and Redshift** , increasing maintenance overhead.
- Some gold-layer datasets critical for ML had to be **copied back to S3** for further processing.
- **Coupled storage and compute architecture** in Redshift limited scalability, requiring costly expansions and downtime, impacting overall productivity.


These challenges ultimately led to the decision to migrate data warehousing operations to **Databricks** .


## Transforming Data Warehousing: Implementing Databricks Lakehouse (2021–2022)


A significant milestone in our data journey was the implementation of the **Databricks Lakehouse architecture** . This project involved migrating our data warehouse from Redshift to a unified **data lakehouse** on Databricks, revolutionizing the way we handled data. The Lakehouse architecture enabled seamless access to both **structured and unstructured data** , breaking down silos and allowing for comprehensive analytics, business intelligence, and machine learning — all powered by a single platform with a single copy of data.


To support this transition, we established a self-managed **Hive Metastore** hosted on **Aurora MySQL** for cataloging object store data and providing essential governance. With the full adoption of Databricks Lakehouse, our core data teams — including Data Engineering, Data Science, and Operational Analytics — all became fully embedded in the platform.


This success led to wider adoption across the organization, with application and customer-facing teams incorporating Databricks into solution designs for use cases like pricing, promotions, and managing loyalty programs.


## Expanding Usage: Love and Hate (2022–2023)


As Databricks adoption continued to grow, it became a story of both love and hate. On one hand, we loved the power of the **Lakehouse architecture** , with its decoupled storage and compute, providing flexibility, scalability, and an improved experience for our engineers and data scientists. From large-scale data processing to ad-hoc analysis, Databricks proved incredibly versatile and fast.


But with this love came some challenges — especially around security and governance. As our usage expanded, we realized that our security architecture needed strengthening. Until 2023, we relied on **AWS instance profiles** and **ACLs on Hive Metastore objects** for security and governance, but this approach had its limitations. Users often had broad access to **S3 resources** based on cluster configurations and instance profiles, making it challenging to maintain **fine-grained security** and effective data governance.


## Enhancing Data Governance with Unity Catalog (2023–2024)


We first considered **Unity Catalog** in 2022, but it wasn’t until 2023 that we began seriously evaluating its potential to address our growing governance needs This also allowed the product to mature in the meanwhile, with learnings from early adopters.


The shift to **Unity Catalog** in 2023–2024 transformed our data governance framework, allowing us to manage access and security more effectively across the organization.


Key features that brought value to our governance include:


- **Centralized permission management** at the account level, ensuring consistent permissions across all workspaces.
- Simplified access provisioning to **S3 buckets** through Databricks groups, leveraging existing user groups for seamless integration.
- Managing access to **ML models** alongside data, ensuring comprehensive governance across machine learning workflows.
- **Centralized auditing** capabilities, providing much-needed visibility into **audit trails** and improving compliance with governance standards.
- Enhanced **data discovery** features that promoted collaboration across teams and helped reduce data silos.
- Standardizing our **Software Development Life Cycle (SDLC)** , with dedicated catalogs for each phase of data processing, improving governance management.


This is how our Lakehouse environment looks currently :


Press enter or click to view image in full size


## Continuous Improvements with Databricks


Databricks’ continuous investment in advancing its platform has been a driving force behind our confidence in adopting its services as the foundation of our data infrastructure. The company’s commitment to innovation, coupled with user-friendly and robust solutions, has enabled us to meet evolving data challenges head-on. A standout example of this ongoing investment is the **Databricks Notebook environment** , which has undergone significant enhancements over the years, transforming how our engineers and data scientists work.


Some of the notable advancements in Databricks Notebooks include:


- **Inbuilt Visualizations** : This feature allows users to visualize data trends directly within notebooks, reducing the need for external visualization tools.
- **Enhanced Search and Navigation** : With advanced search capabilities, users can quickly discover data, code, functions, and variables, making it easier to manage a large, complex ecosystem.
- **AI Assistant and Smart Autocomplete** : These features provide intelligent code suggestions and help fix errors as you type, reducing development time and minimizing mistakes, allowing teams to focus on higher-value tasks.
- **Collaborative Editing** : Real-time collaborative editing enables multiple users to work simultaneously within the same notebook, fostering better teamwork and faster problem-solving.
- **Git Integration and Version Control** : With **Repos** , Databricks offers seamless Git integration, allowing teams to manage version control directly from the notebook environment, making collaboration smoother and ensuring that code iterations are tracked efficiently.
- **Notebook Dashboards** : Users can transform notebooks into interactive dashboards, making it easy to share insights and visualizations with stakeholders in real-time.
- **Seamless Language Switching** : Databricks Notebooks support Python, Scala, SQL, and R, enabling teams to collaborate across different programming languages without switching environments.


## Our Approach to Adopting Databricks Features


At ThredUp, we take a *thoughtful approach* to adopting new Databricks features. While we constantly monitor new releases, we wait until features are *mature* and fit well with our ecosystem. For example, we didn’t adopt **Unity Catalog** when it launched in 2022. Instead, we waited until 2024, by which time it had matured into a robust, reliable governance solution.
This careful strategy ensures that we leverage the most stable tools available while avoiding early-adopter pitfalls.


## Future Directions: Embracing Serverless Architecture, Genie Spaces, and AI Advancements (2024–2025)


Looking ahead, we plan to explore several key advancements with Databricks:


- **Serverless Architecture** : Transitioning to a serverless architecture will help reduce operational overhead, enhance scalability, and optimize cost efficiency. It will allow us to dynamically scale resources based on demand, ensuring peak performance and cost savings during lower usage periods.
- **Conversational Analytics** : We’re excited to experiment with **Genie Spaces** , which offer environments tailored for natural language analytics. These spaces promise flexibility, particularly for teams working on specific domain-focused workloads. Genie Spaces will become part of our evolving data strategy as we continue experimenting with advanced analytics.
- **AI Advancements** : We look forward to expanding our use of **AI capabilities** within Databricks, including tools like **Model Serving** and the **AI Playground** . These features will allow us to experiment with AI-driven insights and manage machine learning models with ML Flow and Unity Catalog more efficiently as we integrate AI deeper into our workflows.xz


Together, these advancements will allow us to scale, optimize, and push our data strategy forward while continuing to innovate with Databricks.


## Being Vendor-Agnostic and Observing the Vendor Landscape


At ThredUp, we maintain a vendor-agnostic approach when it comes to selecting and evaluating our data platforms. We are continuously aware of the evolving landscape in the data analytics space, keeping a close eye on innovations, pricing models, and the offerings of other vendors. This vigilance allows us to remain adaptable and make informed choices that fit the unique needs of our platform.


Cost efficiency is a critical factor, and we recognize that what works today may not always be the best solution for tomorrow. By remaining open to different vendors and approaches, we ensure that we make decisions that align with our company’s long-term objectives — balancing performance, scalability, and cost-effectiveness. Ultimately, our commitment is to choose the solutions that are in the best interest of ThredUp, enabling us to continue scaling and innovating efficiently.


## Wrapping Up


Our journey with Databricks illustrates a transformation from traditional data management to a modern, scalable data platform. By continuously adopting and integrating new technologies, we’ve significantly enhanced our data capabilities, fueling our growth and supporting our mission of promoting *sustainable fashion* . The partnership with Databricks has been instrumental in keeping us *agile* and competitive, proving that a modern data infrastructure can drive *operational excellence* and *business success* . As we continue to innovate, Databricks promises to remain a cornerstone of our data strategy, helping us push the boundaries of what’s possible in the world of resale and sustainability.


## **Acknowledgments**


This transformation was made possible by the contributions of several talented individuals from the **Data Engineering** , **Data Science** , **Fintech, Operational Analytics** and **Product Engineering** teams at ThredUp, both past and present. Huge shout out to


[Aniket Mane](https://medium.com/u/f73ca4ca7545?source=post_page---user_mention--e6ce8a6cc58a---------------------------------------)


, Michael Santhanam and Satish Rane — your contributions, whether it was building the foundational systems or advancing our use of **Databricks** — have been instrumental in the success of our data journey. Special thanks to[Andrii Panfilov](https://medium.com/u/fa5146692f84?source=post_page---user_mention--e6ce8a6cc58a---------------------------------------)


,[Hitesh Goel](https://medium.com/u/4a4a8b313a8a?source=post_page---user_mention--e6ce8a6cc58a---------------------------------------)


,[Maksym Belko](https://medium.com/u/bb577e749eef?source=post_page---user_mention--e6ce8a6cc58a---------------------------------------)


,[Saurabh Mishra](https://medium.com/u/c6bf3fc4df7d?source=post_page---user_mention--e6ce8a6cc58a---------------------------------------)


,[Vignesh S](https://medium.com/u/7a90e38c634?source=post_page---user_mention--e6ce8a6cc58a---------------------------------------)


,[Yash Pandya](https://medium.com/u/8bed39deb8b1?source=post_page---user_mention--e6ce8a6cc58a---------------------------------------)


from **Data Engineering** for their outstanding work and commitment to evolving this platform.
