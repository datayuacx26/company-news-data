---
schema_version: "1.0.0"
document_id: "f99db2e56db255b62b4c66daaacdde028a94fb918c7a57db576ab57633174142"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/creating-certified-datasets"
published_at: "2024-05-16T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:8214a69eb23d6075a9a66d8f26d7e6ae4891a0044397a351315e557c50751fed"
---

# Creating Certified Datasets

A part of the magic of Target is our unique data and how we use data across our enterprise. As data became even more central to Target’s business over the past years, we realized we had a challenge to solve in how to use our data effectively in business decisions. Though we had several petabytes of available data, we struggled to handle the full scope of our data and needed to look for new ways to work with our partners in the business to make data easily discoverable and actionable for them in their departments. We envisioned and implemented a new and structured data architecture that we rolled out to the teams we support to help make it easier to interpret and apply data to their decision-making. We called this our “


Analytical Platform Architecture


.”


In this post, we’ll detail how we defined the problem, how we managed and maintained our new platform, and how we see our architecture evolving to meet future needs of our business.


Defining the Problem


As we united as a team to begin the work to better meet our organization’s data needs, we decided to outline some of the symptoms we observed as part of the initial problem. For example, we noted inconsistent data, multiple copies of the same data, pipeline maintenance issues, data quality concerns, and lack of transparency, to name a few. We decided to categorize the symptoms we observe into three separate buckets for each of the main themes that stood out: curation, management, and consumption. You can see the full list of symptoms we outlined in the table below:


Curation Management Consumption


No standard way of ingestion or pipeline development No governance or controls Users spending a lot of time to find "where is their data"


Unsuitable for modern data sources No standard way of collecting metadata Consumption overhead to mash data together


No visibility to number of pipelines being managed No clear data ownership No notion of trust


No data architecture or data standards being followed No cost implications to the users of data Lack of persona-focused access to tooling and datasets


Inefficient code maintainability, observability, and testability Domain knowledge trumps technical know-how Users lacked data auditability and traceability


Limited scalability options as volumes grow


Table of issues we identified, categorized by type


Starting with


curation


, we noted that our platform lacked a standard approach for ingestion and pipeline development. This made it unsuitable for contemporary data sources. The absence of established data architecture standards made it inefficient to maintain code and difficult to scale storage and compute as our data volume increased.


Regarding


management


, our platform lacked the sufficient governance, controls, and metadata collection models to be truly effective. Ownership and cost implications for data users were not well defined. We noticed that users with domain knowledge superseded those with technical expertise.


Finally, with respect to


consumption


, users found it difficult to locate their data and navigate the combining of data sources. Persona-focused access tooling and data sets were unavailable at the time, making it difficult to consume data efficiently.


We also noted the positives of our analytical platform – it wasn’t all unsuitable. The platform did a good job of supporting a variety of users and functionality. Support was available for analytics, reporting, modeling, and ad-hoc analysis. Though available, the data was often duplicated, and it did appear that the various functions tended to become siloed as product teams and analysts often create similar datasets to address speed-to-market concerns. These were all concerns that we’d need to address in our solution.


Crafting the Solution


We determined that our best way forward was to prioritize three main pillars: accessibility, flexibility, and agility. To accomplish this, we would need to modernize our existing analytical data platform by architecting a system focused on data management and consumption. As the Analytical Platform was designed to be the enterprise source of truth for all data analytics, reporting, models, and ad-hoc analysis, our ability to deliver consistency and quality available data while reducing duplication was of the utmost importance.


We set out by defining some guiding principles for our work, listed below:


- Consistent data architecture


- Common vocabulary


- Governed program to avoid replication


- Pre-joining for domain-centric datasets


- Defined business metrics at the foundational layer


- Business-process based


We further detailed our plans and needs for our new architecture in the diagram below, and share definitions for some of these discrete steps in more detail following the diagram:


Atomic History –


data from various sources, including our Target Retail Platform and third parties. We use Atomic History to build new Core History or Aggregations or rebuild data sets as needed. All data within our platform is intended to exist in some form in Atomic History.


Core History –


data for a single domain aggregated from one or many Atomic History sources. Core History represents changes over time and is considered time series data. Examples of this include sales, margin, and profile data.


Aggregations


– data across multiple domains either materialized in a dataset or represented as a virtual dataset using an API. Aggregations will be built from Core History and/or Atomic History. Example aggregations include Inventory + Cost, Guest + Sales, and Guest + Contact History.


Analytical Datasets (ADS)


– representing both Core History and Aggregations, ADS are typically designed and structured to support analytical workloads. ADS may have varying degrees of content enrichment, denormalization, or other features to support analytical processes.


If a dataset is created adhering to these standards, it was


deemed to be certified


and usable for enterprise decision-making. This allowed us to establish process controls to complement the data architecture we were building.


Technology Considerations for Building


Working at “Target scale” presents unique challenges. We move approximately 8 TB of data across our enterprise


daily,


with more than a million queries and more than 4,000 pipelines executed daily. Trying to develop clean datasets from this volume can be time-consuming for many reasons. For example, team members ended up implementing similar functionalities in multiple pipelines repeatedly. Also, each team member would create and interpret datasets in their own way, making it a challenge to standardize data that confirms to specific parameters. These combined with other use cases resulted in inconsistent user experience while accessing datasets, along with an element of doubt with some users wondering if the data could be trusted?


Before getting started, our team needed to outline the key objectives, principles, and concerns. Starting with objectives, we had four main considerations as we set out to build our framework.


Valued Quality Attributes We Held as Objectives:


- Reusability –


drives easy adoption and creates a common library of reusable assets


- Consistency –


allows different product teams to contribute to each other's codebase, and share support tasks with a shared understanding


- Shared scaffolding –


enables the team to focus on business concerns, allowing engineers to react faster to platform updates and changes


- Minimal code –


minimizes the amount of code that developers must write to create a pipeline, resulting in faster time to market


These four elements would need to be top of mind for us, along with our guiding principles for the work we had ahead of us. We defined four key principles to help us ensure that we were building the most effective and scalable solution. First, we wanted to work using the


single responsibility principle


and


separation of concerns


. This means that every module, class, or function we build should have responsibility over a single part of the functionality provided by the software, and that responsibility should be entirely encapsulated by the class. Next, we wanted to also prioritize the


Don’t Repeat Yourself (DRY) principle


, aiming to reduce any repetitive patterns and ensure that every piece of knowledge must have a single, unambiguous, authoritative representation within a system. We needed to also ensure that anything we built was


maintainable,


allowing other developers to easily maintain and extend the application. Finally, we wanted to build something


expressive


, using meaningful names that express intention. Names we used would need to be distinctive and not mislead users.


As our last step in the planning process, we defined our concerns about the pipeline prior to development. We knew our focus would need to address the concerns of movement, fidelity and quality of data, timeliness, measurement, scalability, auditability and traceability, and monitoring and alerting.


Building our Data Pipeline Framework


Once we had fully defined these objectives, principles, and concerns we were ready to create a spark-based data pipeline framework. Built in-house by our team of dedicated engineers in less than four months, our framework enables development of certified datasets in a more standardized and consistent manner. Speed is another key factor of the data pipeline framework, as we wanted to ensure data is quickly and easily accessible to diverse teams across the enterprise.


Data pipeline framework architecture and process flow


We wanted to be sure to build something that could accelerate the development of certified datasets while also standardizing them through a logical pipeline structure. We also wanted to bust through the silos that were a natural extension of the previous methods, which caused team members to develop their own inconsistent and non-standard datasets in multiple pipelines repeatedly.


By focusing on our “how” of development, and standardizing repeatable tasks, our new framework gives precious problem-solving time back to team members. This time can now be better spent in gleaning insights from the data and arriving more quickly at well-informed business decisions that ultimately enhance guest experience. The framework also provides out-of-the-box observability and data provenance, allowing our data consumers to get a transparent view of the pipeline, all while enabling quick and targeted troubleshooting.


Is the Team Ready?


Like most technology organizations, our big data team is comprised of engineers who are skilled in data warehousing concepts and many database paradigms. While this gave our engineers deep insights into data design and domain understanding, it was not enough to move to a modern tech stack and an agile product execution model for a few reasons. First, the team primarily worked in procedural SQL, the paradigm of which differed from the modern tech stack needs of an object-oriented paradigm. At the time, modern tech concepts like observability and testability were not as well understood. Finally, SQL-based pipelines didn’t lend themselves to collaborative development, which meant our engineers were tightly coupled to the pipelines on which they worked.


To ensure that the team was ready to take on such a massive effort, we took a few specific steps to support them to ensure the smoothest process possible. We started by building our data pipeline framework. While the framework took care of most of the heavy lifting for our team, individual engineers could employ their existing data expertise to contribute meaningfully to the product. The team leveled up their Scala programming language skills. In combination with knowledge of our pipeline framework, ensuring that our engineers were familiar with Scala allowed them to contribute atomic functions within the framework and ultimately deliver the required pipeline.


We also looked outside of our organization when needed and hired strategic influencers on the modern tech stack. These new hires helped drive the hands-on Apache Spark and Scala certification program at Target and helped engineers use real world use cases to solve our current dilemma. With these team members in place, we expanded our programming for internal team members, ramping up innovation and hack days. We noticed increased enthusiasm amongst the team and more curiosity around the technological shift we were undertaking to build our new framework. Finally, we inner-sourced the framework to encourage engineers to contribute to it, and to add to their knowledge. We accelerated this effort by rewarding and recognizing team members who contributed to the framework.


With all of that, we recognize that the journey is constantly evolving. While we are seeing significant progress and good outcomes, we are by no means done.


Working through Challenges


As we worked in an iterative development process, concerns began to emerge as soon as a certain number of datasets were released reaching a critical mass of users. We saw cross-cutting concerns like parallel processing where multiple pipelines processed huge volumes of data – sometimes as many as billions of records – and were finding it difficult to meet our SLA. Data quality issues began to pop up, as each team had their own way of implementing their reports, resulting in the proliferation of inconsistent and non-standard implementation. There was no standard method for providing end-to-end visibility of the health of the data pipeline which meant no standard observability.


We also noted historical data restatements being triggered by dimension changes, recalculations, data corrections, and changes to both attributes and hierarchies. These restatements required a significant time investment to rectify and could sometimes take from 2-3 weeks to complete. Our process was inefficient. Our code was insufficiently reusable, requiring dedicated engineer time to monitor during restatement processing. We lacked source system standards, meaning downstream analytical data product teams were handling issues that would be better addressed at the source.


Finally, source systems were being modernized while the data platform was being built. This meant changes to the schema, and in attribute definitions, along with underlying changes to the data. Comparing new datasets to legacy ones was complex and not always easy to do.


Luckily, working in a product model provided teams with a structured approach to solving cross-cutting concerns in a consistent and systematic way. By creating a common vocabulary and using frameworks like layered parallel processing, our processing time was improved by 80%. Using standardized observability and data quality frameworks, we had a common framework for monitoring and ensuring data quality. Our restatement framework reduced the history restatement time from two weeks to just thirty-six hours. Finally, using an open source cross-platform graphical differencing application helped in comparing large datasets, improving data quality, and establishing standards with source system teams, holding them accountable for the quality of data they send. Solving these concerns in a timely manner as they emerged helped us accelerate our journey.


How Else Did We Accelerate?


We used a few key tactics to accelerate our journey towards building our platform. Beginning with ruthless prioritization, we identified the most critical and impactful datasets that needed to be migrated first and prioritized these. We used cluster analysis to make these identifications quickly. We also knew that automation would be a key element, so we built automation tools and frameworks to accelerate our process. Restatement frameworks, dataset swaps, open source graphical applications, standardized observability, and Grafana were all tools and frameworks that we used in this case to help us move faster.


Through our investment in our own team’s engineering culture, our team members were able to employ framework-based thinking and innovation. This resulted in further acceleration through domain-specific templatization of the pipelines on which we were working. While we were modernizing our team’s skills, we were simultaneously working on modernizing our own tech stack, reducing tech debt and employing a “lift and land” strategy to avoid the situation of merely shifting one tech debt to another. Focusing on a product mindset was also critical to accelerating our speed, using like-for-like datasets to reduce discovery time and enable iterative development for our teams. We tried new tactics like internally dedicated events that allowed the team the time and space to focus on experimentation with new tools and techniques together. We saw great success with these events, noting the team quickly grasped a better understanding of pipeline development velocity and common bottlenecks.


Maintaining What We Built


As we built more datasets, our focus had to shift to include running effective operations for the datasets already in production. To tackle this, we first stood up a Data Site Reliability Engineering (SRE) team. Their focus would be on reliability and performance of the platform, ensuring all data pipelines functioned correctly, and addressing any issues promptly and effectively. We also needed to develop clear data governance policies to ensure that all data is stored, managed, and accessed appropriately. We made sure we established clear data ownership, access controls, and data retention policies at the outset and conducted regular audits to ensure we complied with legal and regulatory requirements. Finally, we prioritized strong observability, including building out effective monitoring and alerting systems in our pipelines and our data platform.


All the steps detailed above helped us prioritize precise performance tuning and optimization of the data pipelines to ensure they could handle increasing data volumes and meet the evolving needs of our business.


What’s Next?


Our learnings were incredibly helpful to us throughout this whole process. In principle, our initial hypothesis and new data architecture proved itself to be correct. However, we noted a need to consciously violate some of our standard principles in certain scenarios we discovered throughout the process of reimagining our new architecture.


For example, not all of the new pipelines were built in compliance with analytical dataset standards, which led to exceptions in data ingress patterns like using files instead of Kafka. Source modernization was not aligned with the data platform migration, leading to new strategies like lift and land to avoid completely rebuilding data pipelines. Large datasets were difficult to join efficiently, resulting in data being persisted multiple times in various technologies to provide efficient access for teams. This eventually led to off-platform copies being made of large datasets, and subsequent inconsistent metrics being calculated from these copies. We also noted several uncertified datasets being built, leading to issues with data quality and governance.


Overall, our use of frameworks accelerated our build but also caused this federated model which resulted in a lack of systemic governance. As we look forward to our next evolution of this effort, we plan to go beyond frameworks toward centralized platforms. We hope that platformization will enable us to democratize our dataset creation while enforcing minimum standards of build, observability, quality, and governance. We also anticipate a centralized provision of data access will simplify access for both humans and tenants moving forward. We hope that sharing our learnings will help other organizations as they look to modify and improve their data architecture.


## RELATED POSTS


### Solving for Product Availability with AI


By Brad Thompson and Meredith Jordan, October 24, 2023


Read about how Target uses AI to improve product availability in stores.


### Elevating Guest Repurchasing Behavior Using Buy It Again Recommendations


By Amit Pande and Rankyung Park, November 2, 2023


Target’s Data Science team shares an inside look at our Buy It Again model.
