---
schema_version: "1.0.0"
document_id: "d9a937b3491388151177b38193932d76bc45f1cce7bbd91fd7aed167e244b815"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/how-to-build-comprehensive-customer-financial-profiles-elastic-cloud-google-cloud"
published_at: "2023-02-13T23:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:b2ed94a5f1a846b567fedb28be1f899f6305cbb5dc2992a7b5795a3bac99ecf4"
---

# How to build comprehensive customer financial profiles with Elastic Cloud and Google Cloud

# How to build comprehensive customer financial profiles with Elastic Cloud and Google Cloud


By


[Dimitri Marx](https://www.elastic.co/blog/author/dimitri-marx)[Eric Lowry](https://www.elastic.co/blog/author/eric-lowry)[Yang Li](https://www.elastic.co/blog/author/yang-li)


February 13, 2023


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


Financial institutions have vast amounts of data about their customers, but many of them struggle to use that data to their advantage. Data may be sitting in silos or trapped on costly mainframes. Customers may only have access to a limited quantity of data, and service providers may need to search through multiple systems of record to handle a simple customer inquiry. This creates a hazard for providers and a headache for customers.


Elastic and Google Cloud enable institutions to manage this information effectively. Powerful search tools allow data to be surfaced faster than ever — whether it's card payments, ACH (Automated Clearing House), wires, bank transfers, real-time payments, or another payment method. This information can be correlated to customer profiles, cash balances, merchant info, purchase history, and other relevant information to enable the customer or business objective.


This reference architecture enables these use cases:


**1. Offering a great customer experience:**


Customers expect immediate access to their entire payment history, with the ability to recognize anomalies — not just through digital channels, but through omnichannel experiences (e.g., customer service interactions).


**2. Customer 360:**


Real-time dashboards that correlate transaction information across multiple variables, offering the business a better view into its customer base and driving efforts for sales, marketing, and product innovation.


Customer 360: The dashboard above looks at 1.2 billion bank transactions and gives a breakdown of what they are, who executes them, where they go, when, and more. At a glance we can see who our wealthiest customers are, which merchants our customers send the most money to, how many unusual transactions there are based on transaction frequency and transaction amount, when folks spend money, and what kind spending and income they have.


**3. Partnership management:**


Merchant acceptance is key for payment providers. Having better access to present and historical merchant transactions can enhance relationships or provide leverage in negotiations. With that, banks can create and monetize new services.


**4. Cost optimization:**


Mainframes are not designed for internet-scale access. Alongside technological limitations, the cost becomes a prohibitive factor. While Mainframes will not be replaced any time sooner, this architecture will help to avoid costly access to data to serve new applications.


**5. Risk reduction:**


By standardizing on the Elastic Stack, banks are no longer limited in the number of data sources they can ingest. With this, banks can better respond to call center delays and potential customer-facing impacts like natural disasters. By deploying machine learning and alerting features, banks can detect and stamp out financial fraud before it impacts member accounts.


Fraud detection: The Graph feature of Elastic helped a financial services company to identify additional cards that were linked via phone numbers and amalgamations of the original billing address on file with those two cards. The team realized that several credit unions, not just the original one where the alert originated from, were being scammed by the same fraud ring.


## Architecture


The following diagram shows the steps to move data from Mainframe to Google Cloud, process and enrich the data in BigQuery, and then provide comprehensive search capabilities through Elastic Cloud.


This architecture includes the following components:


### Move data from Mainframe to Google Cloud


Moving data from IBM z/OS to Google Cloud is straightforward with the


[Mainframe Connector](https://github.com/GoogleCloudPlatform/professional-services/tree/main/tools/bigquery-zos-mainframe-connector) , by following simple steps and defining configurations. The connector runs in z/OS batch job steps and includes a shell interpreter and JVM-based implementations of gsutil, bq, and gcloud command-line utilities. This makes it possible to create and run a complete ELT pipeline from JCL, both for the initial batch data migration and ongoing delta updates.


A typical flow of the connector includes:


1. Reading the mainframe dataset


2. Transcoding the dataset to ORC


3. Uploading ORC file to Cloud Storage


4. Registering ORC file as an external table or loading as a native table


5. Submitting a Query job containing a MERGE DML statement to upsert incremental data into a target table or a SELECT statement to append to or replace an existing table


Here are the steps to install the BQ MainFrame Connector:


1. Copy mainframe connector jar to unix filesystem on z/OS


2. Copy BQSH JCL procedure to a PDS on z/OS


3. Edit BQSH JCL to set site specific environment variables


Please refer to the


[BQ Mainframe connector blog](https://cloud.google.com/blog/products/data-analytics/a-simple-way-to-migrate-mainframe-data-to-the-cloud) for example configuration and commands.


### Process and enrich data in BigQuery


[BigQuery](https://cloud.google.com/bigquery?utm_source=google&utm_medium=cpc&utm_campaign=na-US-all-en-dr-bkws-all-all-trial-e-dr-1011347&utm_content=text-ad-none-any-DEV_c-CRE_621957121377-ADGP_Desk%20%7C%20BKWS%20-%20EXA%20%7C%20Txt%20~%20Data%20Analytics%20~%20BigQuery_Big%20Query-KWID_43700073023085501-kwd-327307220781&utm_term=KW_gcp%20bigquery-ST_gcp%20bigquery&gclid=Cj0KCQiA37KbBhDgARIsAIzce14zp0ElbazcFfTROEdaXRU4GjF-xAEl_frGnil2TIYq4bXEUExBz68aAlnCEALw_wcB&gclsrc=aw.ds) is a completely serverless and cost-effective enterprise data warehouse. Its serverless architecture lets you use SQL language to query and enrich enterprise scale data. And its scalable, distributed analysis engine lets you query terabytes in seconds and petabytes in minutes. An integrated BQML and BI Engine enables you to analyze the data and gain business insights.


### Ingest data from BQ to Elastic Cloud


[Dataflow](https://cloud.google.com/dataflow) is used here to ingest data from BQ to Elastic Cloud. It’s a serverless, fast, and cost-effective stream and batch data processing service. Dataflow provides an


[Elasticsearch Flex Template](https://cloud.google.com/dataflow/docs/guides/templates/provided-batch#bigquery-to-elasticsearch) , which can be easily configured to create the streaming pipeline.


[This blog](https://www.elastic.co/blog/ingest-data-directly-from-google-bigquery-into-elastic-using-google-dataflow) shows an example of how to configure the template.


### Cloud orchestration from Mainframe


It's possible to load both BigQuery and Elastic Cloud entirely from a mainframe job, with no need for an external job scheduler.


To launch the Dataflow flex template directly, you can invoke the gcloud dataflow flex-template run


command in a z/OS batch job step.


If you require additional actions beyond simply launching the template, you can instead invoke the gcloud pubsub topics publish


command in a batch job step after your BigQuery ELT steps are completed, using the --attribute


option to include your BigQuery table name and any other template parameters. The pubsub message can be used to trigger any additional actions within your cloud environment.


To take action in response to the pubsub message sent from your mainframe job, create a


[Cloud Build Pipeline with a pubsub trigger](https://cloud.google.com/build/docs/automate-builds-pubsub-events) and include a Cloud Build Pipeline step that uses the


[gcloud builder](https://cloud.google.com/build/docs/cloud-builders#supported_builder_images_provided_by) to invoke gcloud dataflow flex-template run


and launch the template using the parameters copied from the pubsub message. If you need to use a custom dataflow template rather than the public template, you can use the


[git builder](https://cloud.google.com/build/docs/cloud-builders#supported_builder_images_provided_by) to checkout your code followed by the


[maven builder to compile and launch a custom dataflow pipeline](https://cloud.google.com/build/docs/building/build-java#using_the_maven_image) . Additional pipeline steps can be added for any other actions you require.


The pubsub messages sent from your batch job can also be used to trigger a


[Cloud Run service](https://cloud.google.com/run/docs/tutorials/pubsub) or a


[GKE service via Eventarc](https://cloud.google.com/eventarc/docs/gke/quickstart-pubsub) and may also be consumed directly by a Dataflow pipeline or any other application.


## Mainframe capacity planning


CPU consumption is a major factor in mainframe workload cost. In the basic architecture design above, the Mainframe Connector runs on the JVM and runs on zIIP processor. Relative to simply uploading data to cloud storage, ORC encoding consumes much more CPU time. When processing large amounts of data, it's possible to exhaust zIIP capacity and spill workloads onto GP processors. You may apply the following advanced architecture to reduce CPU consumption and avoid increased z/OS processing costs.


### Remote dataset transcoding on Compute Engine VM


To reduce mainframe CPU consumption, ORC file transcoding can be delegated to a GCE instance. A gRPC service is included with the mainframe connector specifically for this purpose. Instructions for setup can be found in the


[Mainframe Connector documentation](https://github.com/GoogleCloudPlatform/professional-services/tree/main/tools/bigquery-zos-mainframe-connector) . Using remote ORC transcoding will significantly reduce CPU usage of the Mainframe Connector batch jobs and is recommended for all production-level BigQuery workloads. Multiple instances of the gRPC service can be deployed behind a load balancer and shared by all Mainframe Connector batch jobs.


### Transfer data via FICON and Interconnect


Google Cloud technology partners offer products to enable the transfer of mainframe datasets via FICON and 10G ethernet to Cloud Storage. Obtaining a hardware FICON appliance and Interconnect is a practical requirement for workloads that transfer in excess of 500GB daily. This architecture is ideal for the integration of z/OS and Google Cloud because it largely eliminates data transfer-related CPU utilization concerns.


## Ready to get started?


Start putting your data to work with


[Elastic on Google Cloud](https://www.elastic.co/partners/google-cloud) . Or, learn more about how


[financial institutions can turn their data into a strategic asset](https://www.elastic.co/industries/financial-services) .


*We really appreciate Jason Mar from Google Cloud who provided rich context and technical guidance regarding the Mainframe Connector, Eric Lowry from Elastic for his suggestions and recommendations, and the Google Cloud and Elastic team members who contributed to this collaboration.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
