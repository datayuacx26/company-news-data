---
schema_version: "1.0.0"
document_id: "28b97f18b88f6b74d16a6fa6e39f544288cdf4c243d39c4cd54a2b31a13739e0"
company_key: "draftkings-inc-class-a-common-stock"
company: "DraftKings Inc."
source_id: "draftkings-inc-class-a-common-stock-rss-016c40719db2"
canonical_url: "https://medium.com/draftkings-engineering/migrate-hundreds-of-microservices-to-the-cloud-with-zero-downtime-part-1-781a4b8a21e1"
published_at: "2024-10-02T06:20:12+00:00"
first_seen_at: "2026-07-20T04:35:13.112015+00:00"
fetched_at: "2026-07-28T20:59:23.866660+00:00"
content_hash: "sha256:675ecd8bb87f197e9431b0b6d5087f25da9c79828b1f1bef77ea4c61a17bc214"
---

# Migrate hundreds of microservices to the cloud with zero downtime — Part 1

# Migrate hundreds of microservices to the cloud with zero downtime — Part 1


[Valerii Golovko](https://medium.com/@nevalikne?source=post_page---byline--781a4b8a21e1---------------------------------------)


6 min read


·


Oct 2, 2024


--


Press enter or click to view image in full size


## Introduction


Imagine the situation when a mature high-load distributed system, already serving millions of customers, needs to be changed so that a significant part of it can be moved from an On-Premise solution to the Cloud with Zero Downtime. DraftKings faced that challenge as a business in a highly competitive market looking for ways to improve efficiency and scalability.


This article aims to provide an overview of such migration and brings valuable insights if you’re also considering a transition:


- From/to an On-Prem solution
- Move from one Data Center provider with IT support and certain control on your side to another Cloud/Data Center
- Switch from one Cloud provider to another
- Try to achieve certain Multi-Cloud or Hybrid-Cloud strategies
- Modernize legacy systems for performance improvements and cost optimization
- Plan disaster recovery and failover scenarios for business continuity during such migration


The decision to move to the cloud or build an On-Prem solution varies depending on a business’s strategies and available resources. There is no silver bullet solution for what to choose a Cloud or On-Prem environment. At DraftKings, the decision to modify the topology, where some parts of the system reside On-Prem and others in the Cloud, was a strategic choice aimed at leveraging the best of both environments while also considering factors such as regulatory requirements.


This is not an exhaustive list of possible reasons why you would need to do a topology modification of your system:


**Scalability strategy** — in a sports-related business, where load varies much throughout the year, even just one single game could result in X-times more load in comparison to the regular level.


With On-premise approach, there is full control in requesting required resources, limited only by company demand. However, it’s pointless and not-efficient to request resources for a peak load and keep it running or leave it idle. Cloud is able to provide better flexibility in requested compute, where scale could be adjusted on demand and mostly in automated way. However, it is worth pointing out that it’s applicable for certain parts of the system with such variable load and not applicable for the whole system.


**Operational strategy** — in case of On-Premise environment, there is a high level of control and flexibility over the hardware that is in use. At the same time, it’s fully up to the company to manage this hardware and keep such expertise in-house or out-source such activities like:


- DevOps support and management of requested hardware.
- Integrating new hardware when needed, both for increasing business demand and maintenance reasons.


**Resiliency strategy** — defining disaster recovery strategies requires proper analysis and planning and should take into consideration many aspects of the system, like geo-location presence, system scale, persistence recovery and replication, services automatic recovery, etc. There is no silver bullet, so choosing a suitable approach based on an On-Premise or Cloud solution requires taking into consideration all these aspects and nuances of the system.


## Background


The part of the system that required a topology modification was hosted by a reputable Data Center provider, offering sufficient resources and an acceptable level of control.


The system is built on the principles of Microservice Architecture and consists of various service types and communication channels between them. Here is a brief overview:


### Service Types


- **Pipeline Services** : These are ETL-like services that take in data, process it, and produce outputs that flow downstream until they reach the end user, typically through a browser or mobile app.
- **API Services** : These are HTTP services that provide certain APIs for serving data or execute commands.
- **Processor Services** : These are job-like services that perform certain repetitive workloads and produce an output that is used by other types of services.
- **Hybrid Services** : Some services combine the features of the above types to meet specific needs.


### Internal Communication


Services communicate through various channels including but not limited to:


- **Kafka** : This is one of the main communication channels between services.
- **HTTP** : Various flows involve request/response communication to execute a certain CRUD operation, trigger a business flow, run a job, etc.
- Others — out of the scope of the article.


### Persistence storages


For persistence, there are in use variety of technologies including classic SQL as well as NoSQL databases and storages for Business Intelligence like:


- MSSQL server
- PostgreSQL
- MongoDB
- Aerospike
- Snowflake


## Migration Strategies


Given the complexity and size of the system, the fastest strategy is to stop it On-Prem and start it in the Cloud. However, this approach is unacceptable from the business perspective and causes confusion for the customers due to the downtime. Additionally, it creates incredibly high risk in case some services won’t be able to operate in the new setup on the first try.


Therefore, a better approach would be to gradually roll out different pieces of the system to the Cloud with minimal downtime.


However, during the transition phase when the system is not fully migrated, it’s important to minimize two things:


- **Inter-service communication latencies** : During the migration, it’s inevitable that some parts of the system will be in the new Cloud environment and some in the old On-Prem solution. This setup results in additional inter-service communication latencies. So, to define the move of a certain piece of the system, it’s important to minimize the amount of additional latencies.
- **Backward Data flow** : Once a service is moved to the Сloud, it’s inevitable that it will need to consume outputs or call services in the old On-Prem (Forward Data Flow). However, it’s better to plan the migration in a way that minimizes the opposite — Backward Data Flow. In other words better to avoid a setup when a service located in On-Prem needs to consume output or call an API of a service located in the Cloud. For example, if there are circular dependencies between services, it’s better to move that circle of services in one batch to the Cloud to avoid double latencies inside the circle.


As a result, the following strategies came up:


### From right to left


In the describing system, the majority of services are pipelines. So, it makes sense to start moving services closer to the end-user first, then move upstream services, and so on. However, this approach can be complicated if there are circular dependencies between services. In such case better to migrate the whole circle on one turn when possible.


### APIs go last


The majority of inter-service communication of the describing system is done via Kafka. However, there are services that provide certain HTTP APIs. Based on the described “avoid **Backward Data flow** ” **** principle, it makes sense to move all API consumers to the Cloud first. Once that’s done, move the API services.


### Database goes last


Based on the same “avoid **Backward Data flow** ” **** principle better to migrate all the services with database interaction first and their databases last when it’s not possible to migrate all consuming services and their databases in one turn.


To highlight those strategies, you could imagine a system as a graph, with nodes representing services and arrows showing the Data Flow between them:


Press enter or click to view image in full size


In the diagram above, numbers inside nodes represent the potential order of the service migrating to the Cloud.


- Nodes without color represent services that are not part of any circle from the Data Flow point perspective
- Such nodes which have the same number could be migrated in parallel without waiting for each other
- Nodes with a color represent services that are in a circle from the Data Flow point perspective
- It is better to move such a circle of services altogether if possible to avoid double latencies. It will be discussed below what can be done when that is not an option.
- Number in such cases represents the migration order of a circle as a whole.


## What’s next


[The second](https://medium.com/draftkings-engineering/migrate-hundreds-of-microservices-to-the-cloud-with-zero-downtime-part-2-b15ad75a0729) and[third part](https://medium.com/draftkings-engineering/migrate-hundreds-of-microservices-to-the-cloud-with-zero-downtime-part-3-075197dea5fb) of the article will be focused on how to apply described strategies using Pipeline and HTTP API service types as examples.
