---
schema_version: "1.0.0"
document_id: "afd5532209f6bbd44144ff13f3f4116f73c85ba6be91d4b29b57a087fe9ead11"
company_key: "yc-logarithm-labs"
company: "Logarithm Labs"
source_id: "yc-logarithm-labs-news-import-55126c19cebb"
canonical_url: "https://www.logarithmlabs.com/blog/reverse-etl-is-only-a-portion-of-the-solution"
published_at: "2021-12-16T00:00:00+00:00"
first_seen_at: "2026-07-24T09:54:22.724632+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:42f0318ae1e1962ba9ebfc5d1c6869294d9cd60380decaadf7c10132b2b63664"
---

# Reverse ETL is only a portion of the solution

The past few years have seen a phenomenal evolution of data infrastructure. Increasingly, cloud technologies have been dominating the data landscape. According to research, about[50% of all business data](https://www.computerweekly.com/news/2240241926/Prepare-for-two-modes-of-business-intelligence-says-Gartner) is stored in the cloud, and this number is only growing.


Availability of platforms like Snowflake and BigQuery have helped standardize the way data engineering is done, and though some terms like “Modern Data Stack” have reached buzzword status, the momentum behind such approaches is undeniable.


Source: Twitter ([@soobrosa](https://twitter.com/soobrosa) )


However, there still exist major gaps in being able to use that data to run business operations. In this post, we’ll take a look at challenges that remain, and some of the emerging solutions.


## The “Glue” Layer: Data Integration


“Modern” or not, data integration is a key component within any data solution. Data integration is the process of getting data in (and now out) your data warehouse, so that it can be put to use.


This used to be primarily in one direction, from data sources like transactional systems, SaaS apps, and other operational systems into a centralized data warehouse or data lake. using ETL (Extract, Transform, Load) tools or, more recently, EL tools like Fivetran and AirByte to perform batch ingest of data and using the data warehouse for transformations.


A Vendor-Specific Modern Data Stack (source:[Sequoia Capital Medium Blog](https://medium.com/sequoia-capital/census-the-rise-of-operational-analytics-464052918353) )


## Using data for business operations


While this approach of using data warehouses are a centralized repo is great for reporting and analytics, putting that data to use to run your operations remains an issue.


For example, you might have a churn prediction model built in BigQuery tying together all of your customer touch points, but triggering personalized outreach for high risk customers or alerting customer success teams to execute a customer retention playbook off that model requires additional plumbing.


You can check out other use cases highlighted in the graphic below:


Examples of operational use cases for data ([source: Census blog](https://blog.getcensus.com/announcing-our-series-a-from-sequoia/) )


## The Reverse ETL Landscape


Reverse ETL tools are vying to fill this gap, and 2021 saw multiple Reverse ETL vendors such as[Census](https://blog.getcensus.com/announcing-our-series-a-from-sequoia/) and[Hightouch](https://hightouch.io/blog/series-b/) announce sizable funding rounds.


Reverse ETL Landscape (source:[Memory Leak Medium Blog](https://medium.com/memory-leak/reverse-etl-a-primer-4e6694dcc7fb) )


We have also seen BI platforms like[Looker](https://looker.com/platform/actions/) , Integration Platform (iPaaS) vendors like[Workato](https://www.workato.com/) and[tray.io](https://tray.io/) , as well as Customer Data Platforms (CDPs) like[rudderstack](https://rudderstack.com/) jump on this bandwagon to help power operations with data. Even vendors like Airbyte who have primarily played in the ELT space now have[Reverse ETL as part of their roadmap](https://docs.airbyte.io/project-overview/roadmap) .


## Unaddressed Gaps


While they are great steps in the right direction, there’re still pieces missing.


Unlike data analysts, operational teams aren’t in the business of wading through data. A Digital Marketer is trying to generate more MQLs and SQLs, and data is simply the means to getting there. Similarly, back office teams are trying to execute on specific business workflows, and while data can help, more data doesn’t necessarily help them.


That workflow and decision automation layer becomes critical when trying to solve for operational use cases.


## What Comes Next?


We expect the Reverse ETL landscape to evolve as they go deeper into specific operational workflows.


We are already seeing vendors starting to specialize on teams and use cases (eg: Reverse ETL for digital advertising).[Hightouch Audiences](https://hightouch.io/solutions/marketing/) for example has a workflow component baked in, and it clear that they’re targeting the traditional CDP market with a modern data stack approach.


Lots more yet to be done, but certainly feels like the right step in the direction of[data-driven workflows](https://www.logarithmlabs.com/) .


‍
