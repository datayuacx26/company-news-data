---
schema_version: "1.0.0"
document_id: "7b844a41bd0ca9d250041f790f9a1ef3f93ce2df0380a7726905e3dff16ef0bf"
company_key: "yc-predictleads"
company: "PredictLeads"
source_id: "yc-predictleads-rss-ec716ebf37eb"
canonical_url: "https://predictleads.com/blog/discover-companies-by-location-and-size/"
published_at: "2026-07-27T07:41:53+00:00"
first_seen_at: "2026-07-27T08:47:01.608996+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:15adbba2bef870343fc89f47e6ccf02c17c2663ad2e759d04f772612e6579c71"
---

# How to Discover Companies by Location and Size

To define a market by firmographics, call the PredictLeads[Discover Companies endpoint](https://docs.predictleads.com/api_endpoints/companies_dataset/retrieve_companies) and filter by location and size. This is the base of a discovery engine: a query for US companies at 51-200 employees returns **239,699 companies** , and US companies at 1,001-5,000 employees return 21,007. Want to discover companies by location and size at scale? Once you have the base universe, you layer every other signal on top.


This is the capstone of the series. The earlier posts each start from a signal. This one starts from the market and replays the signals against it, which is how you size a total addressable market or plan territories.


Table of Contents


Toggle


##


How do you build a firmographic base list?


Filter by location and one or more size bands, and optionally by industry, NAICS code, or revenue range. Through the MCP server, you can reliably discover companies segmented both by size and location.


```text
Using the PredictLeads MCP server, list US software companies with 51-200
employees. Return company name, domain, and location.
```


The equivalent endpoint call:


```text
companies(
location: "United States",
sizes: ["51-200"],
naics_codes: ["541511"]
)
```


##


What filters can you combine?


The endpoint accepts several firmographic filters at once, so you can define a segment precisely. For example, you can discover companies by their size and location simultaneously with industry or revenue filters.


Filter What it does Example


location Country or state of the company United States


sizes Employee bands from 1 to 10,001+ 51-200, 1001-5000


industry Industry label Retail


naics_codes One or more NAICS codes 541511


revenue_range_low / high Revenue floor and ceiling 1,000,000 to 100,000,000


Size bands run from a single employee up to 10,001 and above, so you can match the exact segment your product is built for when you need to discover companies by both size and location filters.


##


How do you layer signals on top?


The base list becomes an engine when you enrich each company with the datasets from the earlier posts; start by discovering companies based on their location and size, then apply more signals.


- **Technology Detections** to keep companies on a target stack
- **Job Openings** to keep the ones hiring a role
- **News Events** to flag the ones with a recent trigger
- **Financing Events** to surface the ones with fresh budget


Each filter narrows the market to the accounts that match your full profile, not just one attribute. For example, starting with a list where you’ve already discovered companies by their size and location, you can now keep refining the list with further signals.


##


How do you expand the universe with lookalikes?


When you have a set of strong-fit companies, use the Similar Companies dataset to grow it. To scale up, try discovering companies with similar locations and sizes to your current best-fit accounts. Companies similar to Elastic, for instance, return Sumo Logic, SolarWinds, Cribl, Datadog, and New Relic, which extends an ideal customer profile without leaving it.


```text
company_similar_companies(company_id_or_domain: "elastic.co")
```


##


How do GTM teams and agencies use this?


- **TAM and territory planning.** Size a market by firmographics, then split it into territories. Many GTM teams will first discover companies by criteria like location and size to map their market.
- **A living ideal customer profile.** Re-run the query and the list updates itself as companies grow into or out of the profile.
- **Agency deliverables.** Produce a market map or ICP report for a client, backed by live data rather than a stale export.


##


How do you size a market with these filters?


Firmographic counts are the raw material for a total addressable market estimate. Start by adding the size bands that fit your product: 239,699 US companies at 51-200 employees plus 21,007 at 1,001-5,000 gives you a mid-market-and-up universe in one country. To size a market, first discover companies by their location and size, and then filter by other needs like revenue or industry.


That firmographic number is your total addressable market. Layering the signal datasets on top, technology, hiring, news, and funding, turns it into a serviceable list of accounts that match your full profile, which is the number your pipeline planning should actually use.


##


Frequently Asked Questions


**How do I find companies by location and size?**


Call the Discover Companies endpoint and filter by location and one or more employee-size bands. You can also filter by industry, NAICS code, and revenue range.


**What company sizes can I filter by?**


Size bands range from a single employee up to 10,001 and above, so you can match the segment your product targets.


**Can I layer other signals on a firmographic list?**


Yes. Enrich the base list with Technology Detections, Job Openings, News Events, and Financing Events to narrow it to accounts that match your full profile.


**How do I expand a target list of companies?**


Use the Similar Companies dataset on strong-fit accounts to find lookalikes, which grows an ideal customer profile while keeping it relevant.


##


Related Guides


- [Technographic Data for ABM and How to Build Target Account Lists](https://blog.predictleads.com/2026/06/26/technographic-data-for-abm-and-how-to-build-target-account-lists)
- [Company Signals: Hiring, News, Funding, and Technology](https://blog.predictleads.com/2026/06/02/company-signals-hiring-news-funding-technology)
- [Build a GTM Agent on the PredictLeads MCP Server](https://blog.predictleads.com/2026/07/09/gtm-agent-mcp-server)
- [Best MCP Servers for Company Enrichment in 2026](https://blog.predictleads.com/2026/07/15/best-mcp-servers-for-company-enrichment-in-2026)


###


Ready to see this in your own data?


Get 100 free API requests/month – no credit card, no sales call. It’s a great way to start discovering companies filtered by their location and size.


[Start Free →](https://predictleads.com/sign_up?utm_source=blog&utm_medium=cta&utm_campaign=discover-companies-by-location-and-size)
