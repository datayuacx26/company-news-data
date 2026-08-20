---
schema_version: "1.0.0"
document_id: "feaccafc3fe26a419195a89fe903796176c2472097e29b5298979ebe98bdef7a"
company_key: "yc-etleap"
company: "Etleap"
source_id: "yc-etleap-news-import-75d796be5177"
canonical_url: "https://etleap.com/blog/insights-in-minutes-how-tier-analysts-self-serve-rider-data-with-etleap"
published_at: null
first_seen_at: "2026-07-24T03:08:49.604130+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:8055ac7ff5716df5124b0a44b94414da03c2237312d678341419ae3f405a0faa"
---

# Insights in Minutes: How TIER Analysts Self-Serve Rider Data with Etleap

In less than five years, last-mile transportation company TIER Mobility has expanded from just a few cities to become the largest micro-mobility company in the world. Integrating robust ETL as the centerpiece of its modern data stack has been critical to meeting scalability challenges and managing exponential growth.


## Results with Etleap


-


Analysts can access query-able data in hours to minutes without engineering support, **accelerating time to insight**


-


Automated pipeline maintenance frees up engineering time, **prioritizing business-critical use cases**


-


Seamless integration of diverse data stacks eases strategic acquisitions, **spurring global expansion**


## TIER mobility stats


-


HQ: Berlin, Germany


-


350,000 e-scooters and e-bikes


-


560+ cities and communities


-


400+ million rides to date


*"For analysts, Etleap provides extensive out-of-the-box functionality in the Wrangler, where they can click a few buttons and be done. And for engineers who want the flexibility to build from the ground up, Etleap gives them the liberty to write their own custom functions and then executes the transformations.”*


*- **Kumar Aman, Data Engineering Lead at TIER Mobility***


## “Changing mobility for good” 400+ million rides at a time


TIER Mobility was founded in 2018 to offer car-addicted urbanites shared e-scooters and e-bikes as a sustainable alternative. Kumar Aman was there from the start.


“When I joined TIER as an intern, we had about fifty employees and just three people on the data team,” Aman recalls. “Now we employ 2,500 and the data team has grown by 10X. I’ve worked as a data analyst, data engineer, and analytics engineer, first leading the analytics platform team to currently leading the data engineering team. I think of myself as a full stack data guy.”Since then, Aman has had a front-row seat as TIER Mobility emerged as the dominant player in the micro-mobility market in Europe, growing from 10,000 vehicles to 350,000. Not only has the number of riders increased, but their riding patterns have become more dynamic—intensifying the demand for actionable, data-rich insights.


## Raw data is meaningless without transformation


TIER analysts leverage data to understand user behavior, undertake diligent customer segmentation and appeal


to the right audience. They create stories with data that answer the following questions for business stakeholders:


-


How many daily, weekly, and monthly active users do we have?


-


Are we retaining customers from one month to the next?


-


What areas of the city are underserved?


-


How many e-scooters and e-bikes should we have at each depot?


-


What cities offer potential for new market entry?


To this end, small IoT chips onboard TIER vehicles transmit critical real-time data points such as location


coordinates, battery level, and predictive repair diagnostics, while external APIs capture weather data, process payments, and manage customer care information.


But with data inflow that is both abundant and heterogenous, extensive engineering work is required to make it available for analysts. To bridge the gap between unstructured data inflow and business-critical decision-making,


TIER needed an ETL provider that offered the flexibility of custom transformations while also being analyst-


friendly. “From a business perspective raw data is meaningless,” Aman explains. “Etleap takes difficult-to-analyze data and transforms it into a meaningful form where key stakeholders can derive valuable insights.”


Aman points to Etleap’s data Wrangler as a standout feature, as it lets TIER’s data team parse, structure, de-


identify, and clean incoming rider data in an interactive and intuitive way. “When we do ETL,” he explains, “we


don’t want to do too much.”


*TIER’s data infrastructure resides in AWS cloud. The data team uses Etleap pipelines to automate the ingestion of files,APIs, event streams, and databases into Snowflake. Presently, TIER has seven source types, 100+ source connections and over 500 pipelines in Etleap, with analysts adding new pipelines daily. These capabilities inform a globally distributed team about rider engagement in real-time, unlocking business-critical insights for key stakeholders*


## Etleap autonomizes analysts and unshackles engineers


Without Etleap, analysts who require raw data transformation to underpin a business insight are stalled until they receive assistance from a data engineer. But with Etleap complementing a thorough internal onboarding process, TIER analysts get queryable tables in Snowflake for their machine-learning use cases in hours if not minutes.From there, they access data needed for reporting, analytics and ML use cases autonomously.


They simply select from where they want to fetch data and with a few clicks they can transform and send it to Snowflake, where it can be fed into a business intelligence platform like Looker.“Etleap’s default functionalities empower our non-engineers,” explains Aman. “Let’s say you need to explode aJSON list; that’s pretty complex. You either have to build a custom function or teach people how to do it. WithEtleap, it’s available as a drop-down point-and-click feature.”Simultaneously, Etleap manages TIER’s pipeline monitoring, freeing up any engineering work previously required of the TIER data team to identify and debug ETL issues.“Etleap has greatly optimized engineering resources,” says Aman. “It’s not necessarily about having a smallerteam, but rather freeing up engineering time so they can focus on building other pieces of critical datainfrastructure.”


## Etleap’s 100+ source connections ease strategic acquisitions


As TIER prospered, they acquired two micro-mobility companies—Spin and Nextbike—thus becoming the


world’s largest micro-mobility provider. Nextbike and Spin managed their data in different ways. While TIER was using Snowflake, Spin was using BigQuery and Nextbike was primarily based on MySQL and Postgres.


To smooth the transition, TIER’s data team treated Spin’s data warehouse (BigQuery) as a source, fetching the data using Etleap and depositing it into Snowflake. With Nextbike, they used the MySQL reporting database as a source then leveraged Etleap’s advanced CDC replication to feed that information into Snowflake.


“The seamless integration of these acquisitions from a data standpoint required robust and flexible ETL,” explains Aman. ”Without Etleap’s numerous source options, the ingestion workload would have been a nightmare.”


## ...While securing TIER’s market dominance now and into the future


Data-driven demand prediction, powered by a machine learning model, is crucial for how TIER manages its fleets. When customers open their phones to search for a TIER e-scooter and they don’t see one nearby, TIER receives data specific to that user’s location.


“We call it rebalancing,” says Aman. “Let’s say we have 100 vehicles, where should we put them? If we can find out where we have a surplus of e-scooters, we can move them into a place with unfulfilled demand.”The same applies when modeling habitual patterns such as the end of the workday or demand on a Saturday versus a Tuesday and dynamic, real-time factors like rain or snow. TIER ingests data required for these insights by taking advantage of Etleap’s S3, Google Sheets, and Event Streams connectors. Fleet optimization in real-time lets TIER serve more customers and maximize revenue. Demand and supply prediction thus became one of the differentiating factors in how TIER was able to compete against numerous small startups offering similar mobility services.“There was a lot of consolidation going on in the sector,” concludes Aman. “Our dominant emergence in Europe and the Middle East was a result of data-driven demand prediction and how we positioned our vehicles.


*"Our motto in the early days was ‘build fast, fail fast’. As the business grew, we prioritized data quality and having modern data processes in place. Throughout this journey, Etleap enabled us to confidently manage the scaling of data."*


*- **Kumar Aman, Data Engineering Lead at TIER Mobility***
