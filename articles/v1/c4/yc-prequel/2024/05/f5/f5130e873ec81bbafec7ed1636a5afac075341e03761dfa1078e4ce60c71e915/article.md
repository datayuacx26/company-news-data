---
schema_version: "1.0.0"
document_id: "f5130e873ec81bbafec7ed1636a5afac075341e03761dfa1078e4ce60c71e915"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/how-to-build-data-sharing-products-using-mongodb-atlas/"
published_at: "2024-05-30T00:00:00+00:00"
first_seen_at: "2026-07-24T10:10:51.593417+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:3a895c1ccfed7433e079b5686a14190fa0ac1f2d527ee366bbd59a092fca5001"
---

# How to build data export products using MongoDB Atlas

In recent years, leading software companies such as Stripe, Segment, LogRocket, and Modern Treasury have all launched data export products. These products go by a range of names, including Data Pipelines, Push to Data Warehouse, and Streaming Exports. While the packaging is different, their core function is the same: Giving customers direct access to the data that software companies have collected and saved on their behalf.


Now, software companies that run on[MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database) , the popular multi-cloud developer data platform, can build data export products using Prequel’s Data Export Platform and safely share data with customers on[20+ data platforms, including Snowflake, BigQuery, Databricks, and MongoDB](https://www.prequel.co/connections) .


## How Prequel’s Data Export Platform Works


Companies decide which tables they are open to sharing, and customers choose which tables they want access to. For example, a payment processing platform may share tables related to invoices, transactions, and success codes with their customers. Customers can then choose the tables they want to receive and how often they want them updated.


[Data is replicated](https://docs.prequel.co/docs/transfer-logic) using an ephemeral server, which reads data from the source and then upserts that data into the destination. Data is reformatted in flight so that it’s compatible with the destination. No data remains on Prequel’s servers once the data is transferred.


Prequel’s Data Export Platform can replicate up to 100 million rows of data per destination in just 15 minutes. It supports[multi-tenancy](https://docs.prequel.co/docs/multi-tenancy) ,[eventual consistency](https://docs.prequel.co/docs/transfer-logic#incremental-transfers) , and more.


## Why won’t an API work?


APIs are a great way to pass event-based data between two systems, but both sides need to do a lot of work to set them up, changes to data structures are complicated, and they aren’t built for frequent, high-volume transfers.


Data export, by contrast, is a great fit when:


- Your company doesn’t have time to build an API,
- Your customers don’t have time to build an ingest pipeline,
- Your customers want access to a large quantity of information that gets updated frequently, or
- The data model may change, and supporting those changes might not be a priority.


With Prequel’s Data Export Platform, customers can easily access all the data they need, with far less work for everyone at every point in the product lifecycle.


## A Thoughtful Customer Experience


Prequel puts up-to-date, analysis-ready data right where enterprise customers need it — directly into their existing database, data warehouse, or object storage.


The entire customer experience is white-labeled from start to finish.


Customers can sign up in a couple of clicks. They select the data platform they use from a drop-down menu, choose the tables they want to receive, and how often they want the data updated, from every 15 minutes to once daily.


## Launch Data Export in One Day


Prequel is engineered to make deployment easy. After[becoming a Prequel customer](https://www.prequel.co/demo) , all teams need to do is:


1. **Connect MongoDB Atlas to Prequel** — Follow the 4-step[directions here](https://docs.prequel.co/docs/sources-mongodb) .
2. **Outline the data to be shared** — Use version-controlled schemas to[outline the data](https://docs.prequel.co/export/concepts/models) .
3. **Connect Prequel** —[Confirm the connection with test data](https://docs.prequel.co/docs/sources-mongodb) .


Once a connection has been made, companies can begin sharing data with customers on 20+ platforms.


Onboarding customers is simple. Companies can send[magic links](https://docs.prequel.co/reference/get_magic-links) to customers, allowing them to set up their destinations online. Companies can also choose to[embed a sign up form in a website](https://docs.prequel.co/docs/react-sdk-overview) or set up destinations manually.


Prequel also automatically generates a[data type map](https://docs.prequel.co/docs/data-types) that outlines how data types change across data stores, simplifying the documentation process for companies.


## Security and Deployment Options


Companies can use Prequel’s cloud-based service or deploy Prequel privately, depending on the company’s needs. When Prequel is deployed privately, no customer data passes through Prequel’s servers.


Prequel also supports a wide range of[connection modalities](https://docs.prequel.co/docs/connection-modalities) , including SSH tunneling, SSO, and role-based authentication.


## Further information


For more information on Prequel for MongoDB, please set up a time to[talk to one of our engineers](https://www.prequel.co/demo) or visit[prequel.co](https://www.prequel.co/) .
