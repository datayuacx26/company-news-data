---
schema_version: "1.0.0"
document_id: "a3d2f1cc7b7c47e0347ed7b3526a955e3903c708919bd4edb55420fd404cb350"
company_key: "vtex-class-a-common-shares"
company: "VTEX"
source_id: "vtex-class-a-common-shares-rss-fffff04a1e70"
canonical_url: "https://vtextech.substack.com/p/vtex-scales-to-150-million-metrics"
published_at: "2024-03-10T11:23:00+00:00"
first_seen_at: "2026-07-20T23:19:02.969240+00:00"
fetched_at: "2026-07-28T22:26:08.375343+00:00"
content_hash: "sha256:ae436050823b3f403c90567b676b76de266f2389c08c6d2d7a557997c5ae918a"
---

# VTEX scales to 150 million metrics using Amazon Managed Service for Prometheus

# VTEX scales to 150 million metrics using Amazon Managed Service for Prometheus


### A cost-effective strategy with a 41% investment reduction


[VTEX Tech Blog](https://substack.com/@vtextech)


,[Rodrigo Moraes](https://substack.com/@rodrigocmoraes)


,[Gustavo Pantuza](https://substack.com/@pantuza)


, and[Samuel Aquino](https://substack.com/@samuelaquinoo)


Mar 10, 2024


VTEX is a multi-tenant platform with a distributed engineering operation. Observing hundreds of services in real-time in an efficient manner is a technical challenge for the business. In this article, we will show how VTEX created a resilient, open source-based architecture aligned with a sharding strategy, using Amazon Managed Service for Prometheus (AMP) to ingest and store metrics. This solution increased the visibility of engineering teams, reduced the average incident resolution time, and made the investment in cloud infrastructure needed to implement observability within VTEX more efficient, reaching ~41% of reduction on investments in observability.


## Introduction


VTEX started its operation in 2000 as a B2B software focused on the textile sector. Since then, there have been several changes that have influenced the company’s culture. Starting in 2007, VTEX entered the e-commerce market, resulting in the VTEX product that we know today. Along the way, there were several evolutions to achieve an increasingly efficient, resilient, and innovative product for the customer. As VTEX grew, it moved from monolithic to microservices, as it migrated its computing model from data centers to the cloud, boosting efficiency and scalability. Today, VTEX is a global company with more than 3,400 active customers in over 38 countries that trust VTEX to accelerate and transform their retail businesses.


Thanks for reading VTEX’s Tech Blog! Subscribe for free to receive new posts and support my work.


## Problem


Since VTEX is a global company with hundreds of services, it has invested in monitoring and observability to detect application errors, bottlenecks, and anomalous behaviors that may indicate fraud, among other things. For many years, the company used a single software vendor to provide observability. It has served its purpose over the years. However, due to the market momentum experienced by VTEX and its constant engineering innovation, this centralized observability model no longer made sense for the company.


When this centralized tool experienced failures due to unavailability or delayed telemetry ingestion, the VTEX engineering team was consequently left without visibility of its metrics, logs, and application traces. This had a direct impact on the efficiency of VTEX’s incident recovery time. In addition, these telemetry signals (logs, metrics, and tracks), individually, require different configurations. A centralized tool cannot be the best tool for all telemetry signals. Therefore, it’s not logically efficient to rely on a single tool for all of these types of data.


In addition, the lack of control made the cost of this tool unpredictable. Imagine that any major sales event, such as Black Friday, in any country can make the VTEX platform scale. As the systems scale, the volume of telemetry increases linearly with the number of resources being monitored. The consequence was a cost disproportionate to the investment made in the tool. In this blog, we will discuss how VTEX looked at available solutions to help it scale its metrics pipeline.


## Options


To solve the issues mentioned in the previous section, VTEX considered three community solutions and one managed by AWS: Cortex, VictoriaMetrics, TimescaleDB, and Amazon Managed Service for Prometheus.


Cortex continued to be an attractive option due to its maturity level as an open-source CNCF project and its features for using Prometheus at scale, such as remote write/read, PromQL, recording rules and Alertmanager support. In addition, its integration with Prometheus would allow VTEX to easily monitor its systems and receive alerts in case of problems. There is another factor in the balance, which is to have an infrastructure running internally with Cortex. At the end of the day, VTEX could assess whether the engineering investment could mitigate the problems found and avoid going to a new tool.


VictoriaMetrics is a popular alternative to Cortex, with advantages that include high scalability, support for different data formats, high performance, high availability support, and low resource consumption. However, there are some drawbacks to consider, such as limited compatibility with other solutions and lower adoption. It uses its own custom database engine. It is a high-performance, column-oriented time series database designed to handle large volumes of data with high ingestion rates and query performance. The database engine is optimized for efficient compression and provides a number of advanced features, such as fast indexing, data retention policies, data replication, and high availability.


TimescaleDB is a relational time series database integrated with PostgreSQL. It is scalable, resilient, and can support complex SQL queries. TimescaleDB is well suited for high-volume data environments where real-time and historical data analysis is required. One of the main advantages of TimescaleDB is that it allows users to use standard SQL to perform queries on their time series data.


## Why Amazon Managed Service for Prometheus?


Amazon Managed Service for Prometheus is an efficient solution for monitoring and alerting systems, applications, and services. Here are the reasons VTEX chose AMP for its monitoring solution:


-


**Integration with AWS** : Because the solution is integrated with the AWS platform, it can ingest metrics from native AWS services and resources, which can help simplify the configuration and collection of monitoring data.


-


**Scalability** : AMP is highly scalable and can handle large volumes of real-time metric data, which is important for cloud environments with a lot of resources and services running.


-


**Flexibility** : Prometheus is highly flexible through its Long-Term Storage (LTS) compatible with the Prometheus time series bank, which makes it integrable with tools such as Grafana.


-


**Tool Ecosystem** : Prometheus is part of a broader ecosystem of cloud monitoring tools, including data visualization tools, such as Grafana, other alert tools, such as Alertmanager, and a big community that constantly creates new tools.


-


**High cardinality metrics** : Each AMP environment built within a VPC can support up to 500 million active time series. This is crucial for VTEX in its need to add business rules within its metrics.


-


**Service managed by AWS** : AMP is a service managed by AWS and reduces the operational load on the VTEX Observability team of having to administer services.


## Architecture


Data collection is centralized on telemetry services installed on Amazon EKS clusters on private AWS networks. The architecture includes two forms of data collection: push and pull. Push is intended for service metrics that are sent via the OpenTelemetry Protocol. Pull is mostly aimed at collecting infrastructure metrics.


In push-based data collection, the collection is done in shards, where each shard is a grouping of services that correspond to an internal division of the company’s business. This division increases the resilience of the metric system like everything else, by isolating services, which in the event of faults occur, they occur locally and not globally.


In Pull-based data collection, the collection is made on services, where the grouping of services in the same Prometheus is based on the divisions of the engineering team. Each team has its own Prometheus. The filter for collecting the services is done via tags and the collection is carried out in all areas where the services are installed, thus centralizing the collection of data.


Both the Prometheus and the OpenTelemetry collectors are managed by Amazon EKS. The Node Groups where the metric collectors are installed have nodes in 3 Availability Zones.


*Figure 1 – Architecture for Sharding to AMP*


## Results


#### **Global Results**


As VTEX expanded its telemetry ingestion management capacity to hundreds of services and microservices using AMP, stability has been one of its biggest benefits. One consequence of this is the increase in the resilience of monitoring services. In addition, observability management was uncomplicated by allowing focus on more important issues for VTEX, such as ingestion control, education with the use of telemetry, and improvements in the metrics pipeline. All of this directly implies an increase in reliability for VTEX engineers and their systems, in addition to improving risk and crisis management. Using AMP, VTEX gained confidence in telemetry data that allowed the company to trust, debug, and correctly observe systems in real time.


By separating workspaces from AMP, VTEX was able to better isolate and allocate teams and services. This division minimized the failure of the VTEX Observability systems. Now, when there are faults, they occur locally and not globally. Local faults affect only a subset of the teams belonging to a workspace but never affects the entire VTEX engineering team.


#### **Results in the observability team**


Going a little deeper, we know that one of the main objectives of a reliability engineer is to automate processes and reduce toil such as repetitive and manual tasks. By moving to the fully managed Amazon Managed Service for Prometheus from Cortex, VTEX was able to focus on providing support to the VTEX teams, and gain control of its OpenTelemetry pipeline by using observability libraries and leveraging Grafana to visualize and query telemetry.


The reliability of the services offered by the observability team increased due to the use of AMP. Using Cortex for this type of telemetry, we had a fundamental problem: unavailability for many hours. Using Cortex required management, and VTEX faced constant problems maintaining the service to keep it running. In addition, VTEX faced limitations such as limited number of time series and the problem of high cardinality of parameters.


#### **Results for VTEX engineering and services**


One of the biggest improvement points in terms of results for VTEX engineering teams is the reliability of the metrics generated by the services. VTEX teams could rely on their alarms and views based on metrics that come from AMP, not only because VTEX didn’t have unexpected interruptions in the metric telemetry service, but also because of AMP’s support for high volume capacity and greater cardinality of parameters in metrics.


## Next Steps


This entire migration to Amazon Managed Service for Prometheus is part of a larger scope of modernizing the VTEX Observability stack. Thinking of a long-term architecture, without vendor lock-in and with flexible operation, VTEX adopted the OpenTelemetry protocol as the basic tool for this entire new architecture.


Once modernized, this architecture will allow VTEX to extract information and intelligence from all its telemetry data, such as correlation between logs, metrics, and traces, anomaly detection, SLOS/SLIs, better alerts, and data enrichment.


## Conclusion


In this blog, we showed how VTEX managed observability using Amazon Managed Service for Prometheus to maximize operational engineering efficiency by expanding the volume and quality of the observed metrics. The new architecture reduced investment in observability by 41%. Amazon Managed Service for Prometheus allowed the use of high-cardinality metrics while keeping clusters highly available, thus ensuring that VTEX engineering had visibility into their metrics.


For more information, see the following references:


-


[AWS Observability Best Practices Guide](https://aws-observability.github.io/observability-best-practices/guides/operational/gitops-with-amg/gitops-with-amg/)


-


[One Observability Workshop](https://catalog.workshops.aws/observability/en-US/aws-managed-oss/gitops-with-amg)


-


[Terraform AWS Observability Accelerator](https://github.com/aws-observability/terraform-aws-observability-accelerator)


-


[CDK AWS Observability Accelerator](https://github.com/aws-observability/cdk-aws-observability-accelerator)


> *This article was produced in collaboration with AWS. **[Original post](https://aws.amazon.com/blogs/mt/scaling-to-150-million-metrics-using-amazon-managed-service-for-prometheus/)** co-written with Augusto Ferronato and João Borges*


---


Thanks for reading VTEX’s Tech Blog! Subscribe for free to receive new posts.


A guest post by


[Rodrigo Moraes](https://substack.com/@rodrigocmoraes?utm_campaign=guest_post_bio&utm_medium=web)


Computing and electronics lover. Enjoys understand computational thinking and how to use it to solve society problems.


A guest post by


[Gustavo Pantuza](https://substack.com/@pantuza?utm_campaign=guest_post_bio&utm_medium=web)


https://blog.pantuza.com


[Subscribe to Gustavo](https://pantuza.substack.com/subscribe?)


A guest post by


[Samuel Aquino](https://substack.com/@samuelaquinoo?utm_campaign=guest_post_bio&utm_medium=web)


An SRE engineer at VTEX and a knowledge enthusiast. He holds a computer science degree from the Federal University of Ceara and is also a technician in computer networks by E.E.E.P Gov. Luiz de Gonzaga Fonseca Mota.
