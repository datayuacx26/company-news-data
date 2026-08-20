---
schema_version: "1.0.0"
document_id: "b5822fd5c2d95d28726e128ecb26f592661b889048e04c22b51314a2ea3f0e99"
company_key: "yc-predictleads"
company: "PredictLeads"
source_id: "yc-predictleads-rss-ec716ebf37eb"
canonical_url: "https://predictleads.com/blog/find-companies-by-funding-round/"
published_at: "2026-07-27T07:41:33+00:00"
first_seen_at: "2026-07-27T08:47:01.608996+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:dc666558d0a295611e817108393aabcc0d4362fa7f354e1442f95b7cabdad509"
---

# How to Find Companies by Funding Round Type

To find companies that just raised, call the PredictLeads **[Discover Financing Events](https://docs.predictleads.com/api_endpoints/financing_events_dataset/retrieve_financing_events)** endpoint and filter by normalized financing type. The types cover the full range from pre-seed through late-stage series and their bridges. Live counts right now: **18,129 Seed rounds** , 15,254 Series A rounds, and 8,101 Series B rounds.


Fresh capital means budget and urgency. A newly funded company is hiring, buying tools, and building process, which makes funding one of the highest-intent signals a GTM team or agency can act on.


Table of Contents


Toggle


##


What funding types can you filter by?


Financing types are normalized, so messy source labels resolve to clean values you can query. A snapshot of current volumes by stage:


Normalized type Stage Events now


seed Seed 18,129


series_a Series A 15,254


series_b Series B 8,101


Beyond these, the normalized set includes pre-seed, later series through series_j, and plus-rounds, extensions, and bridges such as series_a_plus and series_b1. A query for a type returns those rounds no matter how each source phrased it.


##


How do you find recently funded companies?


Pass one or more financing types, and optionally a company location. Through the MCP server:


```text
Using the PredictLeads MCP server, find companies that raised a Series B.
Return company name, domain, round amount, and the investors.
```


The equivalent endpoint call:


```text
financing_events(financing_types_normalized: ["series_b"], company_location: "United States")
```


##


What does a financing event include?


Each event is detailed enough to prioritize and personalize:


- the **amount** as reported and an **amount_normalized** value
- the **financing_type** and its normalized form
- **effective_date** and **found_at**
- the **investors** involved
- the source articles behind the round


##


How do you add investor context and expand the list?


The investors on each round connect to the Portfolio Companies dataset, so you can move from one funded company to the rest of an investor’s portfolio. You can also take a funded company and pull lookalikes to find similar companies that may raise next.


The strongest play is the double signal. Combine a funding filter with a hiring or technology filter to find companies that just raised and are already spending, for example funded and hiring:


```text
company_job_openings(company_id_or_domain: "example.com", onet_codes: "15-2051.00")
```


##


How do GTM teams and agencies use this?


- **Time the outreach.** Reach out while the budget is fresh and vendor decisions are still open.
- **Prioritize by round.** Sort by stage and amount to focus on companies with the means to buy.
- **Agency lists.** Build “just raised” lists for clients that sell into funded startups, refreshed as new rounds land.


PredictLeads collects this funding data as a first-party dataset, so you are working from the source rather than a resold database.


##


How do you prioritize funded companies?


A raise is a signal, but not all raises are equal for your pipeline. Three attributes help you sort:


- **Amount.** The normalized amount tells you how much budget just landed, which correlates with how much a company can spend.
- **Recency.** The effective date shows how fresh the round is; the first few months after a raise are when new tools get bought.
- **Stage fit.** A seed company and a Series D company buy differently, so match the stage to the price point and motion your product fits.


##


What about bridge and extension rounds?


The normalized types include bridges and extensions such as series_a_plus and series_b1. These often signal a company raising to reach the next milestone, which can mean tighter budgets but clearer priorities. Filtering them in or out lets you separate momentum raises from stopgap ones.


##


Frequently Asked Questions


**How do I find companies that recently raised funding?**


Call the Discover Financing Events endpoint and filter by normalized financing type, such as seed or series_b. It returns companies with matching rounds, ordered by update date.


**What funding round types can I filter by?**


Normalized types range from pre-seed and seed through late-stage series and their bridges and extensions, so source labels resolve to clean, queryable values.


**Does the data include investors and amounts?**


Yes. Each financing event includes the round amount, the normalized type, source articles, and the investors involved.


**How do I combine funding with other signals?**


Add a hiring or technology filter to the funded list. Companies that just raised and are already hiring or running your target stack are the highest-intent accounts.


##


Related Guides


- [Time Outreach by Funding Stage](https://blog.predictleads.com/2026/07/07/time-outreach-by-funding-stage)
- [VC Portfolio Companies for Account-Based Targeting](https://blog.predictleads.com/2026/07/08/vc-portfolio-companies-for-account-based-targeting)
- [Company Signals: Hiring, News, Funding, and Technology](https://blog.predictleads.com/2026/06/02/company-signals-hiring-news-funding-technology)
- [The 37 News Event Categories That Signal a GTM Opportunity](https://blog.predictleads.com/2026/07/06/the-37-news-event-categories-that-signal-a-gtm-opportunity)


###


Ready to see this in your own data?


Get 100 free API requests/month – no credit card, no sales call.


[Start Free →](https://predictleads.com/sign_up?utm_source=blog&utm_medium=cta&utm_campaign=find-companies-by-funding-round)
