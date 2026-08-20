---
schema_version: "1.0.0"
document_id: "82a620bff2dd046002342c40ecf6224fb9b1a73b3157250cd2f5574e86bd1ed8"
company_key: "yc-fivetran"
company: "Fivetran"
source_id: "yc-fivetran-news-import-dafa91bc199f"
canonical_url: "https://www.fivetran.com/blog/how-the-fivetran-approach-to-data-normalization-cuts-compute-costs"
published_at: "2023-03-06T00:00:00+00:00"
first_seen_at: "2026-07-23T09:42:24.979492+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:4bb3d5bfc84c91b31fcfb4463ddc51d46fa14a64dcd265d45f83c165929cf218"
---

# How the Fivetran approach to data normalization cuts compute costs

In a challenging economy, many data teams are looking to improve efficiency across their analytics stack — including their[ELT](https://www.fivetran.com/blog/etl-vs-elt) tool or platform. One potential[data movement](https://www.fivetran.com/blog/beyond-data-integration-why-data-movement-is-the-future) inefficiency you might not have considered relates to data normalization — namely, where an ELT provider performs it.


Ideally, you want your ELT provider to:


1. Automatically provide thoughtful, well-designed[schemas](https://www.fivetran.com/blog/what-is-a-database-schema) , freeing up engineering time and accelerating analytics
2. Normalize your data within its own systems, so the process doesn’t drive up your data warehouse costs


If you’re using warehouse-native data integration tools like AWS Glue or Azure Data Factory, you’ll have to normalize the landed data yourself, a process that can eat up substantial compute bandwidth and increase costs. For example, those tools load all the data, including duplicate records, and you’ll need to use transformation compute power to identify and omit the duplicates.


If you’re using a third-party data movement tool that offers[normalization](https://www.fivetran.com/blog/why-normalization-is-critical) , it’s important to know whether the provider performs the normalization within its own systems or within your data warehouse or destination. Nearly all data integration tools or platforms — including Stitch, Matillion, Domo, Hevo and[Airbyte](https://www.fivetran.com/blog/fivetran-vs-airbyte-features-pricing-services-and-more) — normalize data in your warehouse or lake, which can quickly get expensive.


Fivetran is the exception. We normalize your data within our own virtual private cloud (VPC), so you’ll never have to worry about data ingestion processes devouring your warehousing compute bill. **** We’ve made that decision specifically to support the most efficient data stack possible — and save you costs in the process.


> “Fivetran uses its own VPC to normalize our clients’ data, which is relatively unique among ELT tools and can cut our clients’ ingest compute costs substantially.”


> – Scott Breitenother, Founder and CEO, Brooklyn Data Co.


\[CTA_MODULE\]


## Real-world examples of ingest compute savings


Here’s what it looked like when a current Fivetran customer simultaneously used Fivetran, Matillion and Domo to load the same data into a data warehouse. Unlike Fivetran, Matillion and Domo used the customer’s warehouse instance to normalize the data. *Figures are for monthly usage* .


DATA MOVEMENT PROVIDER WAREHOUSE CREDITS USED PERCENTAGE OF COMPUTE COST ESTIMATED COST*


Matillion 143.69 61% $287–$575


Domo 69.33 30% $139–$277


Fivetran 6.74 3% $13–$27


* *Assuming default pay-as-you-go pricing for customer’s cloud data warehouse.*


We’ve repeatedly heard from new customers that their ingest compute costs drop significantly after they start to use Fivetran, and customers using two or more data integration tools note the differences as well.


“The compute differences between Fivetran and Stitch can be 10X,” a Fivetran customer in the healthcare sector reported. Plus I used both Fivetran and Stitch at my last company for two years and saw the difference anytime I would test a new connector.”


The customer noted that, in a week when both tools loaded roughly the same amount of data, Stitch consumed **53.6 credits** while Fivetran consumed **6.8 credits** .


Usage differences convert into meaningful savings for data teams and businesses. Head of Data Ken MacMann at access management company[ButterflyMX](https://butterflymx.com/) reported that his team saved **20 percent** on ingest compute costs after switching from Stitch to Fivetran — which translated into **$3,600 in savings per year** .


> “Fivetran is the superior tool. It's going to cost a little more than Stitch, but you'll also be incurring immediate cost savings on the warehouse side. We're happy to pay more for a tool that gives us more control and flexibility, especially when it presents permanent, long-term savings for other parts of our data stack.”


> - Ken MacMann, Head of Data, ButterflyMX


Customers using DIY and open-source data connectors who switch to Fivetran also report large reductions in compute, with many decreasing usage by **80–90 percent** .


## Test normalization efficiency before you commit


There’s a good way to figure out exactly how much you could save on ingest compute costs with Fivetran as opposed to another ETL provider — just test the tools yourself. Most ELT tools offer free trials, so you can simply load the same data across different providers.


If you sign up for a Fivetran trial, you’ll have 14 days of free access to data connectors for 700+ sources — including[Salesforce](https://www.fivetran.com/connectors/salesforce) ,[Hubspot](https://www.fivetran.com/connectors/hubspot) ,[Facebook Ads](https://www.fivetran.com/connectors/facebook) ,[Stripe](https://www.fivetran.com/connectors/stripe) ,[Shopify](https://www.fivetran.com/connectors/shopify) and[Google Analytics](https://www.fivetran.com/connectors/google-analytics-4) . The trial doesn’t begin until your initial historical sync has completed, and from there you can explore how efficiently Fivetran loads data into your destination.


Fivetran has[multiple pricing tiers](https://www.fivetran.com/pricing) — including a[free plan](https://www.fivetran.com/pricing/free-plan) for smaller organizations — so you’ll be able to limit your initial financial commitment if necessary. You’ll also benefit from free historical syncs and priority-first syncs, which allow you to access your most recent data without having to wait for the initial sync to complete.


\[CTA_MODULE\]
