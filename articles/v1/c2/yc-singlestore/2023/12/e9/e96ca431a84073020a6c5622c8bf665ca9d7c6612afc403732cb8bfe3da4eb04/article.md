---
schema_version: "1.0.0"
document_id: "e96ca431a84073020a6c5622c8bf665ca9d7c6612afc403732cb8bfe3da4eb04"
company_key: "yc-singlestore"
company: "SingleStore"
source_id: "yc-singlestore-news-import-7744c8297ba3"
canonical_url: "https://www.singlestore.com/blog/how-to-get-started-with-singlestore/"
published_at: "2023-12-04T18:00:00+00:00"
first_seen_at: "2026-07-22T13:37:02.744795+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:409c39b9709266f41dd8b1cd929735eadd4d01d8a00f38c97b6f14f81928a3ba"
---

# Getting Started With SingleStore Helios® Cloud

# Getting Started With SingleStore Helios® Cloud


Dec 4, 2023


•


14 min read


•


[Philipp Schock, Director, Customer Success — International](https://www.singlestore.com/blog/author/philipp-schock/)


New to SingleStore? We’ve got everything you need to know about getting started in this step-by-step guide — here’s how to sign up and start building real-time applications.


Hey tech enthusiasts — I’m thrilled to present my inaugural blog for SingleStore! As a Success Manager, comprehending our product is key. In my first post, I'll guide you through signing up and setting up your free SingleStore Helios cloud trial. I'll cover the initial steps for ingesting your first dataset and delve into the fundamentals of harnessing SingleStore's SQL query engine. Join me on this journey!


SingleStore Database Cheat Sheet


- SingleStore core concepts


- SQL + Kai commands


- Vector operations tips


[Try Notebook](https://www.singlestore.com/spaces/singlestore-cheat-sheet/)


##


Part 1. Setting up SingleStore Helios Cloud


This first part will guide you through seven simple steps to create your first Singlestore Helios environment and Workspace.


**Step 1.1. Let’s get started**


To embark on this adventure, head over to


[our website](https://www.singlestore.com/) and give that “Try Free” button a click.


You’ll be directed to a signup page that’s pretty standard. Here, you can either simplify signing up by using your Google or Microsoft account, or you can go the classic route by providing your name and email to sign up for your free trial.


**Step 1.2. Setting things up**


Now that you’ve got an account, the next thing to do is set your password and verify your email. Simple, right?


**Step 1.3. Almost there**


All that’s left is to accept the Terms of Services and Privacy Policy, and you’re in! You now have access to your very own Singlestore Helios platform.


**Step 1.4. Understanding SingleStoreDB credits**


Now let’s dive a bit deeper into the platform, shall we? We’ll be exploring the Singlestore Helios UI and digging into some of the specific terminology you’ll come across. But first, let’s talk about something cool: credits!


In SingleStoreDB, credits are like currency. They measure computational resources and their usage. In your production environment you can purchase credits. However in this free trial, you earn credits by completing tasks — like describing your use case, creating a workspace or acing a tutorial. These credits will come in handy as you start exploring Singlestore Helios.


**Step 1.5. Find a fun use case**


For getting started, I’ve got an exciting use case in mind: diving into the world of the iconic video game franchise, Mario. We affectionately refer to it as the “S2 Mario Challenge” within the SingleStore CSM team. While it might not be the most complex technical puzzle for some of our tech-savvy readers, it’s a fun and engaging way to explore the product and deepen your understanding of SingleStoreDB.


**Step 1.6. Set up your workspace**


In SingleStoreDB, a workspace is like your personal tech playground. It’s an independent deployment of compute resources where you can run all sorts of fun stuff — workspaces can even share a database through read/write (R/W) and read-only (R/O) attachments. Plus, you can have multiple workspaces organized into workspace groups. Neat, right? Let’s take on the “Create a Workspace” challenge to earn credits and set up our first workspace.


Click on “+Workspace” to get started. Name your workspace group, and select your cloud provider and data center region.


A workspace group in SingleStoreDB is used to manage and configure workspaces. Each organization can have multiple workspace groups and conversely, a single workspace group can contain up to five workspaces.


Besides creating and managing multiple workspaces, you can also use workspace groups to configure access, updates and data sharing. When it comes to the utilization of databases, it allows databases to be attached or detached from any workspace within a workspace group. Each workspace group is deployed in a specific cloud provider and region, which gives the flexibility to have many workspace groups deployed across different cloud providers and regions.


To manage a workspace group in SingleStoreDB, you can use the Management REST API, which facilitates operations like creating, updating and deleting workspace groups.


**Step 1.7. Define your workspace name and size**


In SingleStoreDB, workspace size refers to the amount of computational resources assigned to a workspace, indicating the performance and capacity of the workspace. The size selection is made up of individual nodes that distribute jobs evenly across the underlying cloud resources.


Users can select a size — for example, S-64 — and SingleStoreDB automatically provisions the necessary resources. Size consideration should be based on the required compute, memory, and storage when selecting workspace size.


The workspace size can be adjusted as needed to align with workload requirements through scale up or scale down operations. When scaling up, SingleStore adds compute resources to the existing workspace, thereby increasing its performance and capacity. Conversely, when scaling down, a portion of the available resources is removed without impacting the workspace’s availability.


The workspace size is all about performance and capacity; it's based on vCPUs and RAM. For our small analytical use case, I’m going with an S-00 Workspace size (2 vCPUs, 16GB RAM).


Last but not least, enable the preview feature and Sample Datasets.


Activate “SingleStore Kai™” and turn on “Sample Datasets” to unlock even more features in Singlestore Helios. It’s like getting extra levels in a game! Finally, hit “


**Create Workspace."**


****


Singlestore Helios will now set up your first workspace group and workspace.


## Part 2. Ingesting data into SingleStore Helios


Next, we’ll guide you through a data ingestion tutorial, making the Mario dataset readily available in SingleStore Helios.


Before we start, please keep in mind that this is for learning and experimentation purposes only. SingleStoreDB is a multi-generational platform that can do much more with your data, allowing you to transact, analyze and contextualize data in real time within a single product.


For this segment, we will focus on the data analysis aspect of SingleStoreDB.


**Step 2.1. Find and store your sample data**


Our goal is to analyze the release date, sold items and generated revenue for our favorite plumber’s adventures. You can find a solid dataset on this[source](https://vgsales.fandom.com/wiki/Mario) . Extract the data and store it in your preferred location. For our brief experiment, the data is stored in an AWS S3 bucket in a CSV file. Setting up your AWS S3 and related IAM role won’t be covered in detail. However, you only need a simple read-only access permission which involves a key, secret and the region where your data is stored.


**Step 2.2. Create a database**


Navigate to your previously created workspace group (e.g., “Philipps_Blog”), where you’ll find your main workspace (e.g., “blog1”). Click the “+Create Database” button.


Provide a name for your database, choose the desired workspace for connection and click “Create Database.”


You have now created a new database named “MarioSales” linked to the workspace “blog1.”


Note: You can attach a database to multiple workspaces simultaneously. Each workspace can have its own attachment to the database, with one workspace having a Read/Write (R/W) attachment and many other workspaces having Read/Only (R/O) attachments.


The R/W attachment allows that workspace to make changes (write) to the data, while the R/O attachments only permit data reads. Changes made in the workspace with the R/W attachment are instantaneously reflected in workspaces with R/O attachments. This enables efficient data sharing across isolated compute workloads.


It’s important to note that attaching a database in the Read/Write mode to another workspace requires you to first detach it from the current workspace. A database cannot have more than one R/W attachment. Alternatively, you can attach an existing database in the Read/Only mode, without needing to detach it from any workspace.


Remember to assess your workspace capacity when attaching databases, especially with large datasets. Workspaces with insufficient resources may not effectively process reads against a large database.


**Step 2.3. Start to load data**


It’s important to note that there are multiple ways to ingest data into SingleStoreDB. The simplest method is to use the SingleStore portal UI, but you could also leverage SingleStoreDB’s Pipelines functionality — or a combination of Python and Pandas — for batch ingestion. For the sake of simplicity, we’ll use the SingleStore portal UI.


To populate the empty database, click the three dots on the right side of the database box, and choose “Load Data.”


The “Tutorial” box will guide you through the data ingestion process. Select “Load Your Data” since we are loading our own data.


**Step 2.4. Define your source system and file format**


Our data is stored in AWS S3, so choose “Load from Cloud Storage.”


Specify the cloud provider as AWS S3 and select the data format — in our example, it’s csv.


Click “Next” to proceed with the tutorial, which will guide you to set up your AWS S3 credentials. If you have previously created and saved credentials, select them from the list. Since we are starting from scratch the list is empty, so click “Add Credentials.”


**Step 2.5. Set up a cloud connection**


On the new screen for AWS S3 credentials, give your credentials a meaningful name and description. This will help you distinguish them when managing multiple connections later.


Enter your AWS Access Key ID, AWS Secret Access Key, AWS Session Token and specify the AWS Region where your data is stored (I won’t display sensitive information here, of course!).


Click “Save." SingleStoreDB will test your credentials and display a quick status popup upon successful connection.


Choose your created credentials and click “Next."


**Step 2.6. Specify your data format**


Let’s define the S3 bucket path and file format for the data you want to load. If you choose “Custom,” you can specify fields terminated by, fields escaped by, fields enclosed by, lines started by and lines terminated by characters.


**Step 2.7. Use the preview section to review your setup**


Click “Next.” If your connection and file specifications are correct, you’ll see a preview of your loaded dataset.


Click “Next” again to reach the final “Review” step, where you can get an overview of your ingestion setup. You can also view the ingestion queries created by the SingleStoreDB UI by clicking on “Queries.”


**Step 2.8. Submit data ingestion**


Finally click “Ingest” to execute the data ingestion. After a quick loading process, SingleStoreDB will open the SQL Editor to query your data.


## Part 3. Basic SQL queries in SingleStore Helios


In the third part, we'll leverage our sample dataset to introduce fundamental SQL queries directly accessible from your Singlestore Helios portal. While we're aware that there could be more efficient methods to achieve the same outcomes, our primary focus in this blog is simplicity — aiming to provide newcomers with an entry point into the thrilling world of data exploration.


SingleStore Database Cheat Sheet


- SingleStore core concepts


- SQL + Kai commands


- Vector operations tips


[Try Notebook](https://www.singlestore.com/spaces/singlestore-cheat-sheet/)


****


**Step 3.1. Let’s see how our data looks**


As we're acquainted with our loaded sample data brimming with release and sales figures featuring our iconic mustachioed plumber, Mario and his lanky brother, Luigi embarking on quests to save a princess (or the entire world), it's time to get a closer look at our dataset.


Understanding your data starts with examining the available fields, and a crucial initial step is to inspect the data schema. Navigate to the left-hand side control panel and select your database — ours is named "MarioSales." Click on the ingested table, "MarioSalesDetails."


Now, you'll gain visibility into all available field names alongside their respective stored data types. In our case, these fields include:


- Title


- Year


- Platform


- Sales revenue


A noteworthy observation is that barring "Year," all fields are stored as text data types. Understanding these nuances lays the groundwork for further exploration and analysis.


**Step 3.2. Retrieving data with simple SELECT statements**


The simplest approach to retrieve data using SQL is to select everything via


***SELECT ****


. However, in the realm of big data, executing


***SELECT ****


without additional constraints poses risks as it fetches all rows from all fields. This type of statement could strain your system if it's unprepared for such a large data load. An effective workaround involves limiting the number of retrieved rows by adding a


***LIMIT***


clause.


Let's demonstrate this in Singlestore Helios. Navigate to the SQL Editor in the left-hand control panel and select your active workspace containing your ingested database — ours is named MarioSales.


Next we we can write our first SQL query:


**


**


```text
1  SELECT     *    2   FROM   MarioSalesDetails   3   LIMIT     5  ;
```


This query retrieves all available fields from the 'MarioSalesDetails' table, limited to the first five rows. This straightforward SELECT statement offers insights into our data structure. For instance, it sheds light on why the 'Sales' and 'Revenue' fields are formatted as 'text.' This format includes commas as thousand delimiters and the presence of the '$' sign for currency. Understanding this informs potential adjustments in our ingestion or querying methods within SingleStoreDB to interpret these fields as numeric values. This adjustment is crucial for aggregations, like calculating the sum of all 'Titles' over the last five years.


**Step 3.3. Simple data transformation**


As mentioned earlier, adjusting data formats in Singlestore Helios offers two routes: refining the ingestion process to enable a different data interpretation by SingleStoreDB, or modifying our SQL Select statement accordingly.


Choosing between these options hinges on priorities — expediting data ingestion while potentially elongating the select statement's runtime, or prioritizing swift select statement returns while tolerating some delay during ingestion.


In our example, for simplicity, we'll address the data format issue during SQL Select time. Let's outline the steps required to rectify our data format.


The 'Sales' column signifies the number of copies sold per title. The issue lies in the comma serving as a thousands delimiter. Our strategy involves replacing the commas with spaces and subsequently removing any remaining spaces. We achieve this using nested functions REPLACE() and CAST():


```text
1  CAST  (  REPLACE  (  Sales  ,     ','  ,     ''  )     AS     DECIMAL  (  10  ,     0  )  )     AS   Sales
```


The addition of DECIMAL() in the SQL statements defines the specific data type for numerical values, indicating precision and scale.


The 'Revenue' column represents the dollar amount generated by these sales, including the '$' sign and commas as thousands delimiters. Employing a similar strategy, we convert this string into a number using nested REPLACE() functions:


```text
1  CAST  (  REPLACE  (  REPLACE  (  Revenue  ,     '$'  ,     ''  )  ,     ','  ,     ''  )     AS     DECIMAL  (  12  )  )     AS   Revenue
```


Incorporating this into a complete SQL Select statement yields:


```text
1  SELECT    2    Title  ,    3      Year  ,    4    Platform  ,    5    Revenue  ,    6    Sales  ,    7    CAST  (  REPLACE  (  REPLACE  (  Revenue  ,     '$'  ,     ''  )  ,     ','  ,     ''  )     AS     DECIMAL  (  12  )  )     AS   Revenue  ,    8    CAST  (  REPLACE  (  Sales  ,     ','  ,     ''  )     AS     DECIMAL  (  10  ,     0  )  )     AS   Sales   9   FROM   MarioSalesDetails  ;
```


This statement transforms 'Sales' and 'Revenue' into numeric formats, providing a solid foundation for subsequent calculations and aggregations. Previously, our approach involved transforming data during selection, necessitating this transformation each time we conducted an aggregation. This required additional nested statements or subqueries.


Our current aim is to aggregate all \[Sales\] and \[Revenue\] figures from 2010 to 2020.


We'll leverage our existing query, converting \[Sales\] and \[Revenue\] into numeric fields via an inner query (subquery) from the MarioSalesDetails table. The outer query will then sum TotalRevenue and TotalSales for the specified period (2010 to 2020).The output will be a single row containing three fields:


1. \[Year\]: a string representation as '2010 to 2020'


2. \[TotalRevenue\]: the cumulative sum of \[Revenue\] from 2010 to 2020


3. \[TotalSales\]: the cumulative sum of \[Sales\] from 2010 to 2020


```text
1  SELECT    2      '2010 to 2020'     AS     Year  ,    3      SUM  (  Revenue  )     AS   TotalRevenue  ,    4      SUM  (  Sales  )     AS   TotalSales   5   FROM     (    6      SELECT    7        Year  ,    8      CAST  (  REPLACE  (  REPLACE  (  Revenue  ,     '$'  ,     ''  )  ,     ','  ,     ''  )     AS     DECIMAL  (  12  )  )     AS   Revenue  ,    9      CAST  (  REPLACE  (  Sales  ,     ','  ,     ''  )     AS     DECIMAL  (  10  ,     0  )  )     AS   Sales   10      FROM   MarioSalesDetails   11   )     AS   Subquery   12   WHERE     Year     BETWEEN     2010     AND     2020  ;
```


Mastering these SQL fundamentals empowers you to unravel the analytical mysteries concealed within your data. Whether you seek the average price per copy over the last five years or aim to identify the most successful game by copies sold or revenue generated, SQL equips you to tackle these inquiries and many more. Start exploring today with the potent capabilities of Singlestore Helios.


## **Conclusion**


In conclusion, embarking on the journey to explore Singlestore Helios opens doors to a world of possibilities in data management and analysis. Starting from the initial setup through data ingestion and diving into fundamental SQL queries, this walkthrough provides a foundational understanding of utilizing SingleStoreDB's capabilities.


The setup process introduces users to the platform's interface, the concept of credits and the creation of workspaces, offering a hands-on approach to familiarize oneself with the ecosystem. The data ingestion segment illustrates various methods of loading data into Singlestore Helios, emphasizing the simplicity of the process while hinting at the platform's versatility in handling diverse data sources.


Moreover, delving into basic SQL queries within Singlestore Helios unravels the power to analyze, transform and retrieve insights from the ingested data. From understanding the data schema to performing simple data transformations and aggregations, this walkthrough equips users with the foundational skills to explore and derive valuable insights from their datasets.


By mastering these fundamental steps, users can unlock the potential of Singlestore Helios, enabling them to delve deeper into complex analyses, data manipulations and intricate queries, ultimately harnessing the platform's capabilities to derive meaningful business insights and drive informed decisions. The journey into Singlestore Helios serves as a gateway to a dynamic world of data exploration and management, empowering users to navigate the realm of data with confidence and efficacy.


Activate your


[free Singlestore Helios trial](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Fhow-to-get-started-with-singlestore%2F) today to get started.


Ready to dive deeper? Use[Spaces](https://www.singlestore.com/spaces/) , a set of quick start Notebooks to s olve common problems in a few minutes with end-to-end applications using SingleStoreDB.


SingleStore Database Cheat Sheet


- SingleStore core concepts


- SQL + Kai commands


- Vector operations tips


[Try Notebook](https://www.singlestore.com/spaces/singlestore-cheat-sheet/)


Frequently Asked Questions


## On this page


- Part 1. Setting up SingleStore Helios Cloud
- Part 2. Ingesting data into SingleStore Helios
- Part 3. Basic SQL queries in SingleStore Helios
- Conclusion


## Start building now


Get started with SingleStore Helios today and receive $500 in credits.


[Start free](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Fhow-to-get-started-with-singlestore%2F)


Last modified - June 4th, 2026


[Product](https://www.singlestore.com/blog/category/product/)


---


Share


### Don’t miss a thing.
Get the SingleStore newsletter.


## Related reading


[Blog Iceberg Needs an Open Catalog, Not a Walled Garden Product](https://www.singlestore.com/blog/iceberg-needs-an-open-catalog-not-a-walled-garden/)


[Blog Security Engineering, Not Just Security Features Product](https://www.singlestore.com/blog/cloud-database-security-engineering-sdlc/)


[Blog Running SingleStore on Kubernetes in Production Product](https://www.singlestore.com/blog/running-singlestore-on-kubernetes-in-production/)


[Blog Neon vs. SingleStore: Free Plan Comparison (3.5M Rows Benchmark) Product](https://www.singlestore.com/blog/neon-vs-singlestore-free-plan-comparison-35m-rows-benchmark/)


[Blog Turning Your Business Intelligence Stack Into an AI Engine Product](https://www.singlestore.com/blog/turning-your-business-intelligence-stack-into-an-ai-engine/)


[Blog Snowflake Too Slow? Unlock Real-Time Performance at Scale Product](https://www.singlestore.com/blog/snowflake-too-slow-unlock-real-time-performance-at-scale/)
