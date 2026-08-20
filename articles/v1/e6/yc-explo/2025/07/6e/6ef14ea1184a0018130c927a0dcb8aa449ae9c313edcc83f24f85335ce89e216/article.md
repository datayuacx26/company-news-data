---
schema_version: "1.0.0"
document_id: "6ef14ea1184a0018130c927a0dcb8aa449ae9c313edcc83f24f85335ce89e216"
company_key: "yc-explo"
company: "Explo"
source_id: "yc-explo-news-import-52334996d9df"
canonical_url: "https://www.explo.co/blog/building-embedded-real-time-analytics-with-explo-and-clickhouse-on-altinity-cloud"
published_at: "2025-07-08T00:00:00+00:00"
first_seen_at: "2026-07-27T02:15:57.545680+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:e1e0d49df277afac25e3525c6436e0b5ac2d6a408b03cf4097ca4c52c895d6ff"
---

# Building Embedded Real-Time Analytics with Explo and ClickHouse on Altinity.Cloud

In today’s fast-paced data world, customers demand access to their data to make better informed decisions. As SaaS companies look to externalize data to their end users, questions about the time it takes to create an analytics experience tend to come up. How long will it take to build helpful charts and reporting? How will we scale real-time analytics demands across hundreds of customers? Explo and Altinity work wonderfully together as a solution for real-time,[embedded analytics](https://www.explo.co/blog/what-are-embedded-analytics) based on popular Clickhouse analytic databases.


‍


With Explo and Altinity, teams can easily create embedded analytics solutions with hands-on support and simple-to-use interfaces. There is no need to spend months setting up custom frontends and backends to create engaging analytics experiences. The tools complement each other for a great customer-facing experience: Explo is an outstanding tool for offering easy-to-embed analytics for any product, whereas Altinity.Cloud® makes it easy to manage and scale ClickHouse® as an analytics database. This piece dives into the value of these tools and the important relationship between them.


## Explo for Real-Time Dashboard Analytics


[Explo](https://www.explo.co/) is a leading tool on the market for real-time analytics visualization designed to be embedded in an application. Teams are able to write SQL, create advanced reporting or charts in a no-code interface, then embed Explo in their application with just a few lines of code in a fully white-labeled, native-looking solution. Instead of spending months on charts, teams can build powerful charts and reporting for their end users in hours.


‍


Explo has a few major differentiators over the existing embedded solutions in this space. First, Explo is optimized for ease-of-use and minimizes the learning curve for a new embedded analytics tool. Users can quickly assess how to use the product and immediately dive into the click-and-drop interface. Second, Explo is highly optimized for customizability. With Explo’s[Report Builder](https://www.explo.co/products/report-builder) and editable sections offerings, Explo’s customers can enable end users to fully customize their analytics experiences. It used to be that only enterprises could offer these solutions, but now they’re available out-of-the-box with Explo. Finally, Explo has built state-of-the-art real-time analytics infrastructure, enabling immense scalability and speed. Typically the data warehouse is the best infrastructure in the stack, whereas the analytics offering isn’t properly optimized; with Explo, this isn’t a concern.


## ClickHouse for Real-Time Analytics and Business Intelligence


ClickHouse has made optimization decisions based around common real-time analytics needs. Specifically, ClickHouse was started with the goal of computing aggregates with filter and group by operations directly on very large source datasets. It can deliver real-time analytics as well as feed[business intelligence use cases](https://www.explo.co/blog/top-5-business-intelligence-use-cases) that use flexible slicing and dicing to obtain answers. To achieve this optimization, ClickHouse has based its storage and caching layers around the concept of column-oriented storage and vectorized query execution, meaning data is stored and processed with the column as the core entity. Additionally, ClickHouse’s architecture enables maximum CPU utilization, even for a single query. The ClickHouse architecture is impressive and enables a best-in-class analytics experience.


## Altinity ClickHouse Cloud: the Best Option for Clickhouse Hosting


[Altinity](https://altinity.com/) quickly embraced the ClickHouse open-source project, offering services to make ClickHouse work with enterprise. In 2020, Altinity expanded their services into a cloud-offering on AWS, followed by GCP a year later. Altinity.Cloud® is a perfect complement to the existing services capability of the team. As one of the first cloud offerings on the market for ClickHouse, Altinity has done a wonderful job of creating one of the most scalable and robust ClickHouse offerings. Moreover, Altinity maintains a strong commitment to the open source community, ensuring transparency with the[builds of ClickHouse](https://altinity.com/altinity-stable/) they use for their cloud production builds and continually contributing to the open source ecosystem. Altinity is a helpful and ethical partner in the ClickHouse cloud space.


## Explo and Altinity Together for ClickHouse Customer-Facing Analytics


Explo and Altinity are a perfect match for customer-facing analytics. As one of the premiere solutions for offering ClickHouse in the cloud, Altinity brings lightning-fast hardware and software for fast querying time, which is integral to a real-time analytics solution. Further, with Altinity’s built-in support, the data setup process is incredibly easy. In order to develop data apps, it is important that the data warehouse piece is simple yet scalable, which Altinity has done a great job in satisfying. After that initial setup with Altinity.Cloud®, the final steps with Explo are relatively easy: write SQL, create a chart, then embed the application. In the next section, we will explore the setup process of creating and embedding a dashboard with Explo and Altinity.


## Explo and Altinity ClickHouse Example Dashboard


The process to connect Explo and Altinity is fast and easy.


### Get Credentials and Connection Parameters for ClickHouse


Login to Altinity.Cloud and find the and create a new ClickHouse user by pressing CONFIGURE->Users->ADD USER. Fill in the dialog screen as shown in the example below and press OK to save.


‍


Next, get connection parameters by pressing the Connection Details link on the cluster dashboard.


### Establish a Connection


Go to the data section of Explo and select ClickHouse as your source. Note the host name and port for the connection as well as the sample connection strings. Connections to Altinity.Cloud are always encrypted.


From the connection details, you will need the host, HTTP port, database, username (admin), and password.


### Create a Dashboard


###


Write SQL


Write SQL for your visualization; in this case, we are selecting e-commerce data.


### Create a Chart


Drag on a visualization type, click the dataset, configure your chart, and watch your visualization!


### Embed in Your Application


‍


## **Altinity.Cloud® for ClickHouse and Explo for Embedded Analytics Conclusion**


Altinity.Cloud® for ClickHouse and Explo are two of the premiere solutions in the market for real-time embedded analytics solutions. Altinity.Cloud® ClickHouse provides a scalable way to run ClickHouse, an amazing real-time analytics offering, and Explo provides a wonderful visualization and optimization interface geared towards a customer-facing analytics use case. With these two solutions, a team can be confident in their ability to launch customer-facing analytics quickly and robustly.
