---
schema_version: "1.0.0"
document_id: "cec09b776d93f2f7e4dcf92af85ffa47c817711fd141b6399c4d212072bc8dfc"
company_key: "yc-nectar"
company: "Nectar"
source_id: "yc-nectar-news-import-9e0f80bd6d18"
canonical_url: "https://nectarclimate.com/blog/post/comprehensive-and-accurate-data"
published_at: "2025-01-16T00:00:00+00:00"
first_seen_at: "2026-07-23T17:43:23.891177+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:c69843098746a7de414a4e6e3296414273576d62915ea80a5ddc6e445a83617d"
---

# Essential Utility Data API Requirements (Part 1)

Energy brokers, sustainability managers, and consultants all share a common need: access to detailed, accurate utility data that goes far beyond a monthly dollar amount. Whether negotiating procurement contracts, calculating Scope 2 emissions, or advising clients on efficiency investments, these professionals depend on granular billing and consumption data to do their work effectively. The quality of the utility data API they use directly determines the quality of the decisions they make.


This is the first installment in a three-part series on the essential requirements for a utility data API. We start with the most fundamental requirement: comprehensive and accurate data.


## Why comprehensive energy data matters


Surface-level utility data — a total bill amount and a billing period — is sufficient for paying invoices but insufficient for everything else. Energy professionals need to see *inside* the bill:


- Brokers need kWh consumption, demand readings, and rate structures to build accurate load profiles for procurement RFPs. Missing or inaccurate data leads to poorly priced contracts and lost margin.
- Sustainability managers need consumption volumes by fuel type — electricity, natural gas, water — to calculate emissions using standardized factors. Gaps in data coverage mean gaps in carbon inventories.
- Consultants need historical trends, line-item cost breakdowns, and supply-versus-delivery splits to identify savings opportunities and validate the impact of efficiency measures.


In each case, the difference between useful and useless data is granularity.


## Key data requirements


### Granular usage data


An API should deliver consumption data at the most detailed level available: kWh for electricity, therms or CCF for natural gas, gallons for water, and tons for waste. Where interval data is available, the API should provide it. Monthly totals are a starting point, but time-of-use breakdowns and demand readings unlock significantly more analytical value.


### Detailed line items


Utility bills are not monolithic charges. They contain dozens of individual line items — generation charges, transmission fees, distribution charges, demand charges, renewable energy surcharges, taxes, and regulatory assessments. An API that only delivers the total amount leaves the most valuable data on the table. Line-item granularity is what enables rate validation, billing error detection, and cost optimization.


### Supply and delivery breakdowns


In deregulated markets, supply (commodity) and delivery (distribution) charges are billed separately, often by different entities. An API must clearly distinguish between these components. Brokers need supply costs isolated to evaluate commodity pricing. Sustainability teams need delivery data to understand grid-related charges. Blending these together makes downstream analysis unreliable.


### Historical data


A single billing period provides a snapshot. Twelve months provides a trend. Three to five years provides the foundation for meaningful benchmarking, weather normalization, and long-term cost modeling. An API should provide access to as much historical data as the utility makes available, not just the most recent bill.


## How Nectar delivers


Nectar's API is built to meet these requirements at scale. The platform collects data from over 7,000 utility providers across electricity, natural gas, water, and waste, parsing each bill down to individual line items with 99%+ accuracy. Data is standardized into a consistent schema regardless of the source utility, so downstream consumers receive the same structured output whether the bill comes from a large investor-owned utility or a small municipal provider.


## Beyond the basics


Comprehensive and accurate data is the foundation, but it is only the first requirement. In[Part 2](https://nectarclimate.com/blog/post/reliability-and-integration) of this series, we will examine the reliability and integration standards that ensure this data is delivered continuously, securely, and in a format that fits into existing workflows.
