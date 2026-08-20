---
schema_version: "1.0.0"
document_id: "4588a8af5bc1092a1828a82426d016973ac21358ea1de24cf7d685e43f5bb48e"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/verticalization-of-the-data-warehouse/"
published_at: "2026-01-29T00:00:00+00:00"
first_seen_at: "2026-07-22T10:08:50.167024+00:00"
fetched_at: "2026-07-28T22:22:56.687510+00:00"
content_hash: "sha256:55e0f979c53f83bbc3fbfd6bc23cf1b7d9dd4d645a35df0bb3910a5886a54d19"
---

# Verticalization of the Data Warehouse

2026 kicked off with Snowflake’s acquisition of Observe (subject to closing) – an observability product built on top of Snowflake. This marks the first vertical specific acquisition Snowflake has brought in house, and could hint at a new growth experiment for the leading data warehouse platforms.


## The obvious explanation: expansion


Snowflake and Databricks have achieved steady growth, in large part due to their consistently high customer expansion (NRR) at about 125% and 140% respectively. Year after year, both companies have expanded the share of data workloads you can perform within each platform, and have been rewarded by over a decade of incredible net retention metrics.


*Source:*[Databricks](https://www.databricks.com/company/newsroom/press-releases/databricks-surpasses-4-8b-revenue-run-rate-growing-55-year-over-year) *and*[Clouded Judgement](https://cloudedjudgement.substack.com/p/clouded-judgement-11626-platform)


But once all the DBT models are written, dashboards are created, and data science notebooks are running, what’s left?


## The vertical expansion play


Historically, the major cloud data platforms have been unopinionated about how or what the customer chooses to analyze. The product philosophies for Snowflake and Databricks have focused on providing the user with the tools to do whatever they want, without an opinion on what those things are or how the analysis should look.


Here’s an extreme example: if you want to analyze your compute usage and spend in Databricks, there is no “usage” page – instead, where you’d expect that page to be, there is simply a link to a template of a prebuilt Databricks dashboard that you can fork to get started in your custom spend analysis.


*Screenshot of Databricks billing page*


This historical focus on the infrastructure layer has left plenty of room for other businesses to capture the value at the application layer. But in order to sustain such high customer expansion, it could be a greenfield opportunity for the data platforms themselves.


## Anticipating the verticals


To get a sense of what these common use cases might be, look at Snowflake and Databricks marketing websites for some hints at the most popular vertical applications. Here are some of them:


- **Cybersecurity & Observability** : SIEM, Observability
- **Finance** : Billing, Payments, and Forecasting
- **Marketing** : CDP, CRM, Customer Engagement Platforms
- **IT** : ERP, HR


*Excerpts of Snowflake and Databricks marketing pages with highlighted vertical use cases*


Snowflake just acquired Observe, a Snowflake-powered Observability platform. A couple months before that, Snowflake launched a “Cortex Financial analyst” for finance-specific data analysis use cases. Meanwhile, Databricks has been building a team of highly experienced application builders, including Tasso Argyros who previously built ActionIQ – a warehouse-native CDP.


There are dozens of successful software businesses in each subvertical and these data platforms are one strategic acquisition or acquihire away from entering any of these domains.


## It’s the Oracle playbook


If this does play out, it wouldn’t be the first time a major database vendor decided to move up the stack. Perhaps the most famous database company, Oracle, has a long history of these types of moves: buying PeopleSoft (HR), Siebel Systems (CRM), and NetSuite (ERP), among others.


## But it’s also a defensive move


The growth angle is only one side – a few other dynamics are at play that could be forcing this hand.


### Switching costs are decreasing


Open table formats have made “data gravity” essentially nonexistent. The same exact tables can now be easily converted to Iceberg, moved to any object storage, and queried by almost any leading query engine. AI and SQL translation tools have made saved queries and SQL dialect differences almost meaningless. When switching costs for the core data platform approach zero, “lock-in” must be earned via ancillary offerings and an ecosystem that doesn’t exist on competing platforms.


### Core tech advantages are disappearing


The core tech advantages of Snowflake and Databricks are eroding. Most common benchmarks are saturated with closed and open source database technologies that all perform within an order of magnitude from each other, and seemingly cancel out across workload types.


At this point, some of the most popular new features are simply cost saving and efficiency features, further eroding the revenue expansion opportunities within the core platform.


### Vertical SaaS is expanding towards the data warehouse


Finally, many of the leading vertical SaaS players are pushing in the reverse direction, towards the Data Warehouse. Many vertical vendors already offer or have announced their “Data cloud” offerings, with at least some cloud data warehouse functionality overlap:[Everyone wants to be a Data Cloud](https://www.prequel.co/blog/everyone-wants-to-be-a-data-cloud) .


## How will it play out?


The leading data platforms require a lot of product surface area to continue the expansion march within their customers and this vertical play appears to be the newest of the many concurrent experiments.


Among the market leaders – Snowflake and Databricks – one open question is: will they pick separate verticals, or go head to head against each other?
