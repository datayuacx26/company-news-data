---
schema_version: "1.0.0"
document_id: "d2783f724b131526a29f51a198d927bb237563c5e4e2250f468d99e5af66d492"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/importance-of-data-quality-in-decision-making"
published_at: "2025-01-31T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:0dc523b404d217b9eb61b3b0aa880039a23f4c835a9d25e41cf50e7db24d8a0d"
---

# The importance of data quality in decision-making

Data has become an integral part of our society, permeating every aspect of our lives. The exponential growth of data and advancements in technology have revolutionized the decision-making process by revealing insights and patterns that lead to better decision-making and improved business performance.


In this data-driven world, it is crucial to maintain the quality of data for


accurate analytics, informed decision-making, and cost-effectiveness


. High-quality data plays a significant role in elevating sales, streamlining processes, creating reliable strategies, and encouraging good business decisions.


Whether in supply chain management, marketing, customer insights, compliance, or financial reporting, certified datasets provides Target with the accuracy, reliability, and trust needed to thrive in a competitive and data-driven retail environment. They are a cornerstone for achieving high-quality data and reliable analytics.


Here is a step-by-step process for building a data pipeline effectively.


What is a certified dataset


A dataset can be called “certified” when the data is accurate, secure, complete, reliable, consistent and follows proper compliance guidelines.


The purpose of creating certified data is to:


- Build one


dataset for the entire enterprise and ensure users can easily locate and access the data they need.


- Build trust


by instituting operational controls and governance to continually improve reliability and integrity of the data.


- Mitigate risk


that results from using insufficiently managed and controlled data when making operational and strategic decisions.


Understanding Business Requirements and Discovery


The first step in building a dataset is to gather business requirements and understand its use cases. Product managers will identify the source of the dataset for broad enterprise consumption, and a common template called a


data contract


is maintained between the source, data engineering, and business teams.


Then technical discovery is done by engineers to validate the information shared through the data contract and document observations and anomalies.


Design Data Ingestion & Processing Pipeline


At Target, we use an in-house data pipeline architecture called Kelsa.


High Level Flow of Ingestion and Processing Pipeline


Data Ingestion Pipeline


The ingestion job reads data from various sources like SFTP, Kafka, and HTTP APIs, and then persists the data to an ephemeral HDFS location. The data is then moved from ephemeral storage to atomic history.


Data Processing Pipeline


The data processing pipeline reads data from the atomic layer, performs transformations based on business rules, and then loads the data into a Hive table.


The processing steps are as follows:


- Read Data


: Read raw data from ephemeral location.


- Control Validation


: To check if complete data has been received or not. The count and amount of source data is compared against the control data received either through API or files.


- Validate Schema and Data Quality:


The fields are type casted to different data types based on requirements, and quality checks are performed.


- Data Transformations:


All the transformations are performed in this step based on use case and business rules.


- Data Decorations or Data Enrichment with other data streams:


The core table is joined with other shared tables like location, item, calendar, etc. based on the requirement. All the enrichments and decorations are performed at this stage.


- Post Processing Validations:


Validations are done to check if the data post join or aggregation is valid or not.


- Sink to Table:


Sink to table writes the processed data to the foundation table after all the validation and processing is completed.


- Capture and Publish Metrics:


All the metrics counts are captured during the processing of data—like InputRowsCount, DuplicateCheckCount, CDSRowsCount—and then pushed to grafana dashboard for monitoring


Development and Testing Best Practices


Development


- The code changes for the pipeline are pushed through Git to keep track of changes and ensure reproducibility.


- Separate lower and upper environment branches are maintained in Git that allows developers to work on new features or bug fixes without affecting the main branch.


- The processing code is written in Scala or Python language using Apache Spark to carry out the process like extraction, transformation, and data load.


- Best coding practices are followed for development, and the code should also pass through the quality gate with no code smells and no duplicate lines.


Testing


- The pipeline is tested thoroughly in local and stage environments by replicating real-life scenarios.


- Strict separation of code and config (variable by env.), for automated promotions.


- Test cases are written for each module, and the code coverage should be a minimum of 80%.


- Automated unit, integration, and functional testing is completed before pushing the code to the main branch.


A data engineering code quality platform can help developers ensure the quality of their code.


Deployment & Scheduling


Continuous Integration and Continuous Deployment (CI/CD)


- CI/CD process is used to deploy the pipelines in stage or production environment as it helps


decrease manual intervention, increase efficiency, and streamline workflows.


[Vela](https://go-vela.github.io/docs/) is Target’s official CI/CD tool for


all software e


ngineers.


The production deployment takes place only when the testing is completed thoroughly in lower environments and the pull request for code changes are reviewed by team members.


Shepherd


- Defining the sequence of operation in the pipeline is an important part of automation. This includes specifying the order of tasks, managing dependencies between tasks, handling errors, and setting up retries or notifications in case of failure. We use an in-house scheduler and orchestrator that abstracts the underlying infra from the workflows by providing a robust orchestration capability.


Observability and Performance Monitoring


The purpose of monitoring is to detect potential issues or discrepancy in the system and resolve it proactively to make it more reliable. Dashboards are created to monitor data, platform, and pipeline health. They play a key role in proactively detecting issues or trends that need prompt action or resolution.


Types of Monitoring Dashboards


- Pipeline Observability:


This dashboard captures important metrics related to running the pipeline like number of records ingested, records written and run time, to name a few.


- Data Quality Observability:


It gives a holistic view of all the data quality checks performed on datasets and alerts the team of any abnormality.


- Platform Observability:


The platform dashboard gives a historical or real-time view to detect potential issues or anomalies.


- Service Level Agreement: This


is maintained to keep track of service level agreement (SLA) and timeliness of all the certified datasets.


We also communicate through Slack or email to alert the team of any problems identified during production runs.


Data governance and compliance


Data governance is the process of managing the quality, integrity, reliability, and security of data based on a set of principles and standards throughout the lifecycle of data.


Key elements of data governance include:


- Data Cataloging:


Enable


data to be discovered, shared, understood, and managed within the organization.


- Data Classification


: D


ata is classified based on its sensitivity, value, and criticality.


- Auditing Data Entitlements and Access:


Data access is centrally audited with alerts and monitoring capabilities to promote accountability and security.


- Data discovery:


Well-defined datasets are important for easy data discovery to enable data scientists, analysts, engineers, and stakeholders to quickly discover and reference relevant data.


- Data sharing and collaboration:


Access to data is regulated with access controls across platforms.


- Data Retention:


It


helps businesses comply with data protection and privacy policy by ensuring that sensitive or expired data is securely removed.


- Data lineage:


Data lineage to get the visibility of end-to-end process into how data flows from source to destination.


- Data Security


:


Dataset security complies with Target’s internal information security requirements.


Effective data access management is crucial for data security and governance, and a good data security governance program should include access controls that define which groups or individuals can access what data.


- Data Quality


: To maintain the sanctity of the data, there are multiple checks that are performed at various stages in the pipeline so that there is no or less chance of undetected data quality issues.


Approvals and Reviews


The dataset is reviewed and approved by stakeholders and is updated on the element roster. The engineering checklist is approved by the data engineering director, and all the product certification criteria are approved by the data product director. The data management team approves the dataset and updates the element roster with a certified flag.


## RELATED POSTS


### Creating Certified Datasets


By Natarajan Ramamurthy, Brad Thompson, and Vinay Joshi, May 16, 2024


How Target data scientists built a new data pipeline framework


### Modernizing Data Sources Using Shims


By Janine Mechelke, October 26, 2021


At Target we’re always evolving our business to meet the needs of our guests and team members — which means we’re also always evolving how we build technology.
