---
schema_version: "1.0.0"
document_id: "4d4f083e662c06c8059380662396d3ff704493300ae97461245e010353c61164"
company_key: "yc-jitsu"
company: "Jitsu"
source_id: "yc-jitsu-news-import-65f0e2b767a6"
canonical_url: "https://jitsu.com/blog/data-warehouse-first"
published_at: "2023-08-23T00:00:00+00:00"
first_seen_at: "2026-07-22T00:57:29.784311+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:f1628dd17b02779e63d5f25ded3d10d72880ee5497eea532631fda8b8c192719"
---

# What is warehouse-first architecture?

## # Introduction


In recent years data warehouses have become increasingly accessible, even for smaller teams. Setting up a warehouse is literally a few clicks away. Now, it's time to reevaluate our approach to data collection pipelines.


In this blog post, we'll discuss the implications of these advancements and explore how we can optimize our data infrastructure to take advantage of these increasingly accessible data warehousing solutions.


## # What is warehouse-first?


Warehouse-first architecture places the data warehouse at the heart of the data infrastructure.


In this architecture, the data warehouse is the singular source of truth, where the data is initially collected before being distributed to other destinations via reverse ETL.


The primary advantage of this approach is the creation of a **single source of truth** for your data. If the data in any destination becomes out of sync with the data warehouse, you can always re-sync it. This is not feasible with a traditional architecture, where data is collected in multiple destinations, with the warehouse being just one of them.


Events


Connectors


Warehouse


CRM


Analytics


## # Layers


Warehouse-first architecture consists of three layers


### # Data Collection Layer


The layer responsible for collecting data from sources and event streams and loading it into data warehouse.


### # Transformation Layer


This layer is responsible for transforming raw data inside database and prepare models — SQL tables and views that contain data ready to use


Sometimes, transformation layer follows["Medallion"](https://www.databricks.com/glossary/medallion-architecture) architecture where data is split into 3 classes: Bronze, Silver, Gold. Bronze is raw data, Silver is curated data, Gold is ready-to-use


- **Bronze** (or *raw* ) tables contains raw data as it was collected by data collection layer
- **Silver** (or *curated* ) contains models that was transformed and cleaned up, but not aggredated and ready to use for business purposes
- **Gold** (or *ready-to-use* ) contains models that are ready to use either in BI visualization or in BI export


[DBT](https://getdbt.com/) is great open-source tool for transforming data with a SQL and Python scripts


### # Reverse ETL Layer


This layer is responsible for syncing data from data warehouse to destinations. It is called reverse ETL because it is the opposite of ETL: it takes data from data warehouse and pushes it to destinations.


Reverse ETL works with prepared data (Gold or Silver tables).


[Hightouch](https://hightouch.com/) and[Census](https://www.getcensus.com/) is the two most popular reverse ETL tools


## # Why go warehouse-first?


Data warehouse-first architecture has several advantages over legacy architecture


### # Replays


At any time it is possible to "reset" destination and replay data from data warehouse. Why it can be useful? For example, destination might be misconfigured and data becomes corrupted. Or you decided to change data structure and you want your changes to be applied retroactively.


### # Back-filling data into new destinations


If you decide to add new destination, you can easily back-fill data from data warehouse and make.


For example, you might decide to give a try to a new product analytics tool. With back-filling, you can easily start using the tool for analysing data from the beginning of your project.


### # Using historical data for building user profiles


For some destinations, like CRM, it is important to have historical data about users. This kind of data can't be inferred from a single event from event stream.


Example of such property will be activity of a user, like number of logins per month. Such property can be usefull for segmenting users inside CRM and building marketing campaigns on top of it.


With legacy architecture, it's not possible to update activity property when a next event for a particular user comes into pipeline. With warehouse-first, you can easily update user profile by querying data warehouse for historical data.


### # Single source of truth


Reports and dashboards build by various system can give a slightly different picture of the same data. It always happens because each system has its own data model and data is transformed differently. With warehouse-first, you always has a way to check if report look inconsistent because of the nature of destination, or because of error. Just run and SQL query against data warehouse and you will get the truth.


### # Downside


While warehouse-first architecture offers several benefits, there are some minor issues that could arise when dealing with data collection. One of the only problems that could be hard to fix is an error occurring in the event streams or data collection layer. This is due to the fact that modern data warehouses lack robust tooling for modifying data, and tracking such errors can be a challenge. However, it's important to note that such errors are rare and can often be corrected in the transformation layer. For instance, if there's a typo in a field value, it can be fixed in the transformation layer. To prevent such errors, it"s advisable to use reliable event streaming platforms that don"t drop any data events even if the warehouse is down. Additionally, applying fewer transformations in the collection layer and offsetting most of them to the transformation layer can also help mitigate these minor issues. Overall, these minor issues should not deter organizations from implementing warehouse-first architecture as the benefits far outweigh the minor challenges.


Here's a comparison table of the two approaches:


Warehouse-fist


Legacy


#### Can replay historical data


Easy to fix data quality issues in the past; easy to add new destinations.


#### Can utilize historical data


Possible to send user activity based properties to DB


#### Can work without Reverse-ETL


#### Low data delays


Data can be delayed by up to 1 hour because transformations and reverseETL usually runs in a batch manner.


Data is ready to use in destination right away. No need to wait for batch processing.


#### Single Source of Truth


## # Why I should collect data to warehouse from a first day?


Recently, I was approached by a friend who was looking for advice. His startup was using Google Analytics from the start, and now they outgrew it. They decided to switch to Amplitude, and they were looking for a way to migrate their Google Analytics data to Amplitude. I wish I could help, but in this case, it was impossible. Google Analytics *has* the data, but there's no API to access it.


Had he had set up data warehouse data collection from the first day of his project, he would solve this problem quickly by reverse-ETL'ing data to Amplitude.


I have a countless examples like that. People are looking to migrate away from Amplitude to Mixpanel or vice versa because of pricing. Or they need a very complicated report that can't be build with product analytics UI. Or they want apply ML-model to their data. All of those problems can be solved by having raw data in data warehouse.


If you are starting a new project, I highly recommend you to set up data warehouse from the first day. It will save you a lot of time and money in the future.


## # Conclusion


In conclusion, the warehouse-first architecture prioritizes the data warehouse as the central component of the data infrastructure. This approach enables building a single source of truth for the data, which helps in keeping all the destinations in sync with the data warehouse. Although implementing this architecture requires more effort, it provides several benefits, including the ability to backfill data and easily identify the source of any discrepancies between the data warehouse and the destinations. Ultimately, the decision to adopt the warehouse-first approach depends on the specific needs and goals of the organization.
