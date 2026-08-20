---
schema_version: "1.0.0"
document_id: "dfd7e5bcf8b520071c0c2253e57ee252295d1e31d1cba313ed8062c7d7c4e996"
company_key: "upstart-holdings-inc-common-stock"
company: "Upstart Holdings Inc."
source_id: "upstart-holdings-inc-common-stock-rss-a598e0871937"
canonical_url: "https://medium.com/upstart-tech/upstarts-new-low-code-no-code-data-platform-aqueduct-282799dbdfed"
published_at: "2025-02-03T18:34:00+00:00"
first_seen_at: "2026-07-20T23:17:16.152971+00:00"
fetched_at: "2026-08-20T00:26:23.139930+00:00"
content_hash: "sha256:ea2fcdce3d64343fecfe66070b8996d8f01b6ae0ed5ab442f1c39da0227df63c"
---

# Upstart’s New Low-Code/No-Code Data Platform: Aqueduct

*By Srinivasan Hariharan, Puneet Bysani, Bhargavi Damodaran & Satheesh Boopathi*


Data is the fuel that powers machine-learning algorithms and enables AI systems to learn, adapt, and improve over time. The more reliable data points an AI system can access, the more accurate and effective it becomes.


That’s where Upstart’s Data Platform team comes in.[Upstart](https://www.upstart.com/) is dedicated to[expanding access to credit through the use of AI](https://www.upstart.com/our-story) , and our role is to provide fast access to high-quality data to enable quicker model iteration, regular improvements in model accuracy, the uncovering of new business insights, and increased confidence in financial reporting through low-code and easy-to-use tools.


In an effort to better serve other Upstart teams — engineers, product managers, data analysts — and help them overcome challenges in the development process, we built a new solution that allows them to create data engineering workflows at scale. It’s called Aqueduct, a low-code/no-code data platform that, we like to think, has changed the game when it comes to data management. Here’s how it came together:


**Speed Bumps to Productivity**


By conducting interviews across the organization, we were able to identify the main issues slowing productivity and efficiency for our colleagues:


**1. High Development Time:** The in-house requests for data from sources as varied as S3, Kafka, and third-party APIs (for use in the Lakehouse for model training and analytics) required us to build bespoke data pipelines to extract and transform that data. This led to a lot of extra dev time, redundant code across projects and teams, a lack of standardization and best practices, and inconsistent testing practices among teams, which ultimately resulted in an inefficient development lifecycle.


**2. Reliance on Specialized Data-Engineering Skills:** A dependency on data engineers for common ingestion patterns and transformations — along with the specialized skills needed to handle high volumes of complex data (e.g., flattening large JSON blobs or querying nested JSON structures) — became a bottleneck. This slowed down the development of new product insights and hampered rapid model iterations.


**3. Infrastructure and Setup Time:** Analysts and machine-learning scientists faced significant friction in setting up development environments, spending excessive time on non-core tasks like securing permissions for sensitive data (e.g., Personally Identifiable Information) and managing infrastructure, rather than focusing on developing and testing models.


**4. Support for Various Data Pipelines:** Data consumers inside Upstart increasingly requested access to near real-time data to support critical business processes such as online models, real-time analytics dashboards, fraud detection, and velocity controls. In response, we needed to build scalable and reliable infrastructure that could support a variety of pipeline types — batch, streaming, event-driven — Change Data Capture, and Single Message Transforms.


**Our North Stars**


Yes, there was more than just one. By developing scalable infrastructure and self-service tooling for these various pipeline types, we aimed to meet Upstart’s diverse business needs and unlock more value from our data ecosystem. To get there, we established a set of guiding principles that would shape how Aqueduct was built.


**1. Domain-Specific Language (DSL) for Data Engineering:** To reduce development time, we drew inspiration from well-established tools such as DBT and domain-specific languages such as SQL. We implemented DSL support in our platform, enabling users to interact with various SaaS and cloud provider tools — Databricks Spark Streaming, Delta Lake, S3, and Kafka — through simple commands defined in configuration files. This abstraction enables users to build complex data pipelines without needing to write extensive custom code, enabling them to focus on business logic rather than technical and infrastructure details.


**2. Low-Code/No-Code:** To eliminate the need for specialized data engineering skills and enforce best practices, we built our platform with a low-code/no-code approach. This automates critical tasks such as deployment, compute, and infrastructure provisioning, while enabling developers to inject custom data processing code using popular languages such as Python or SQL. This flexibility enables users to format data for model training or analytics while still adhering to enforced best practices, such as cost attribution tags to track resource consumption, table indexes to optimize read/write, and auto-scaling to manage resource efficiency. This approach ensures that performance and cost-related issues are minimized without burdening users with complex infrastructure management.


**3. Support for Various Types of Pipelines:** Our platform is designed to support a diverse range of data pipeline types in order to cater to different business needs. By supporting multiple types of pipelines, we enable users to build and manage data workflows that suit their specific use cases, from real-time analytics to large-scale batch processing.


**4. Extensible and Modular Design:** We aimed to modularize the platform’s architecture to enable seamless addition of source connectors (for pulling events) and sink connectors (for persisting events). This modular approach ensures that the platform remains highly extensible — making it easy to integrate new sources or sinks without major refactoring. By managing SaaS integrations as separate modules (e.g., Databricks Spark, Delta Lake, Confluent Kafka, AWS SQS), we avoid vendor lock-in, giving users the flexibility to switch or expand their toolset as needed. This design not only enhances flexibility but also supports scalability as new technologies emerge.


**5. Security and Deployment:** To simplify access management and enhance security, we abstracted the handling of credentials such as IAM roles, service accounts, and access tokens, along with role-based access controls (RBAC) to various SaaS and cloud provider services. This ensures that sensitive credentials are managed securely and automatically, reducing the operational burden on users. Also, we sought to streamline production deployments by offering pre-built continuous integration/continuous delivery (CI/CD) templates. These templates simplify the process of validating, submitting, and testing configuration changes, enabling teams to deploy their pipelines quickly and with greater confidence in production environments. This approach reduces manual overhead, enhances consistency across deployments, and promotes best practices for production readiness.


**Rolling It Out**


*Main Components of Aqueduct*


Aqueduct is designed to empower users to create data engineering workflows at scale. It uses a Domain Specific Language in JSON format to define workflows that ingest, transform, and write data while abstracting away infrastructure complexities and scheduling details. It supports a wide variety of use cases aimed at the needs of data engineers, software engineers, machine learning engineers, and analytics engineers.


Aqueduct is based on the Workflow Design Pattern. The main modules in Aqueduct are Workflow Engine and Connectors. Each workflow job has an input stream, processing logic, and output stream. An Aqueduct job can contain multiple workflows packaged with their own dependencies that can be executed in parallel. For example, a datalake_ingestion DSL can contain multiple workflows, and when translated into Databricks, it creates multiple jobs to scale out and optimize resources, typically managing up to 100 workflows per job. It also abstracts the job compute configuration, infrastructure management, scheduling, and workflow orchestration into the DSL configurations. This enables Aqueduct users to focus on defining their data workflows without worrying about the implementation details.


*Self-Serve Workflows*


Aqueduct users clone the template repository so that each team has their own DSL repo. Users configure the DSL to instruct the platform to perform tasks such as reading data from different sources, applying transformations, and writing data to various destinations. This declarative way of defining workflows makes Aqueduct easily accessible for both technical and non-technical users in Upstart. Below is a sample DSL to ingest Salesforce marketing cloud data from S3 to Databricks Lakehouse:


As seen above, users can specify different workflow types such as data_lake_ingestion, data_lake_transformation, streaming transformations, kafka_connect_sink, etc. Based on the workflow type, Aqueduct provides execution abstraction. For example, DSLs for data_lake_ingestion and data_lake_transformation are translated into Databricks jobs, while DSLs for kafka_connect_sink are translated into Kafka sink connectors. For the data_lake_ingestion workflow type, all users need to do is specify input properties, output properties, compute configuration, and schedule. Aqueduct handles everything behind the scenes to translate the DSL into a job and execute it in Databricks.


In some cases, users need to write custom transformations beyond just ingesting data from one source to a destination. For those types of transformations, Aqueduct supports custom processing built using Python code and PySpark DataFrames. Users can also specify these process scripts in the DSL.


This workflow as a config enables users to scale their data workflows for volume and latency requirements. For example, a data pipeline that ingests data from thousands of Kafka topics into thousands of corresponding Delta Lake tables translates into multiple Databricks jobs, complete with auto-scaling, optimal instance types, monitoring, and observability. Additionally, Aqueduct offers out-of-the-box performance optimizations, including auto-optimization, indexing, partitioning, and Z-ordering. These features enable users to focus on the business logic without the need to manage the complexities of the pipeline.


*Production Deployment*


Now that we understand how the pipeline is configured, let’s take a look at how CI/CD works. All services running in Aqueduct are stateless. Users commit the DSL through GitHub, and once the PR is approved and merged, the build process validates the DSL. Upon successful validation, the DSL is copied to an S3 bucket. The arrival of the DSL in the S3 bucket triggers an event in the Aqueduct Kafka topic. The Aqueduct translator service continuously listens to this topic. Once the new DSL arrives, the translator parses and submits it to Databricks or Confluent based on the specified workflow type. Once the job is created, it runs as a streaming or batch job based on the schedule.


**Conclusion**


Aqueduct has proven to be a transformative tool for streamlining data engineering workflows at scale in Upstart. While challenges in roll-out and early-stage use presented themselves, we are implementing new features to support better orchestration and dependency management, observability, and data governance. We continue to improve the platform to deliver a better experience for our teams. The following shows a summary of the overall impact of Aqueduct at Upstart:


- Empowers teams to create data pipelines without relying on data engineers.
- Accelerates development of data pipelines by reducing the effort from weeks to days. We observed a 50% reduction in average time to ingest raw data or transform data within the Lakehouse.
- Enables easy plugging of custom transformation code without any infrastructure overhead.
- Out-of-the-box handling for ingestion scaling and optimizing the written datasets for optimal read performances.
- Speeds up time-to-insight using reusable pipeline templates.
- Seamlessly integrates with popular tools like Databricks, Kafka, and AWS.
- Auto-scaling of infrastructure based on data volume.
- Automated alerting and notifications of workflow failures to the responsible teams.


There were other challenges encountered when adopting Aqueduct for niche use cases in Upstart, such as scaling of real-time data processing and dynamically generating DSLs to reduce manual intervention. We will dive into those, and discuss specific use cases where Aqueduct has provided significant value to the Upstart, in future blog posts.


*Read more about Upstart’s innovations in data, engineering, machine learning, and AI*[here](https://medium.com/upstart-tech) *.*


---


[Upstart’s New Low-Code/No-Code Data Platform: Aqueduct](https://medium.com/upstart-tech/upstarts-new-low-code-no-code-data-platform-aqueduct-282799dbdfed) was originally published in[Upstart Tech](https://medium.com/upstart-tech) on Medium, where people are continuing the conversation by highlighting and responding to this story.
