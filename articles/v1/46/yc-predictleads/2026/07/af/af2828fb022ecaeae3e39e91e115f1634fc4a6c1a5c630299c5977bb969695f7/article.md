---
schema_version: "1.0.0"
document_id: "af2828fb022ecaeae3e39e91e115f1634fc4a6c1a5c630299c5977bb969695f7"
company_key: "yc-predictleads"
company: "PredictLeads"
source_id: "yc-predictleads-rss-ec716ebf37eb"
canonical_url: "https://predictleads.com/blog/find-companies-by-news-event-category/"
published_at: "2026-07-27T07:40:55+00:00"
first_seen_at: "2026-07-27T08:47:01.608996+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:35742fe8e691f6750888d7daa2aa13899269f939f2aa8c69d6c2d9354d8784de"
---

# How to Find Companies by News Event Category

To find companies by what just happened to them, call the PredictLeads **[Discover News Events](https://docs.predictleads.com/api_endpoints/news_events_dataset)** endpoint and filter by category. Categories are grouped into themes such as expansion, investment, leadership, partnership, and new offerings. The dataset is large and live: partnership events alone number **1,238,951** , leadership hires 1,272,025, and product launches 3,432,419.


News events are the timing layer of a discovery engine. A company that just expanded, hired a new executive, or launched a product is in a moment of change, and change is when outreach lands.


Table of Contents


Toggle


##


What news event categories can you track?


Each category is a specific, machine-readable event type, grouped into themes. A sample of the most useful for GTM, with current volumes:


Theme Example category Events now


Partnership partners_with 1,238,951


Leadership hires 1,272,025


New offering launches 3,432,419


Expansion expands_offices_to 42,148


Investment receives_financing 236,940


For a full tour of the categories and what each one signals, see the related guide on news event categories.


##


How do you find companies by news category?


Pass one or more categories, and optionally a company location. Through the MCP server:


```text
Using the PredictLeads MCP server, find companies that announced a partnership
in the last month. Return company name, domain, and a one-line summary.
```


The equivalent endpoint call:


```text
news_events(categories: ["partners_with"], company_location: "United States")
```


Each event comes back with a plain-language summary, the source article, an effective date, and, for many categories, the second company involved, so a partnership event names both partners.


##


What does a news event include?


Events are structured so you can act on them without reading the article:


- a **summary** and the **category**
- **found_at** and **effective_date** , plus a confidence score
- **company1** and, for relational events, **company2**
- category-specific attributes such as amount, headcount, job_title, and location
- the most relevant source article


##


How do you turn a one-time pull into monitoring?


A single query is a snapshot. To make it a live trigger, follow the companies you care about and let new events come to you through webhooks:


```text
follow_company(company_id_or_domain: "example.com")
```


For partnership and leadership events, pair the news with the Connections dataset to see who a company just partnered with or who joined, which turns a headline into a map of relationships you can act on. A new executive often brings new vendor preferences, and a new partnership often opens an integration need.


##


How do GTM teams and agencies use this?


- **Trigger outreach.** Reach out on an expansion or a leadership change, when priorities and budgets are being set.
- **Account monitoring.** Follow a target list and get alerted the moment something happens.
- **Agency services.** Run trigger-based, monitored campaigns for clients, where the news is the reason for the touch.


##


Which events signal a planned move versus a done deal?


Timing depends on whether an event has already happened or is about to. Events carry an **effective_date** alongside the date they were found, and a **planning** flag that marks announced-but-not-yet-completed moves. An expansion that is planned for next quarter is an early opening; one that just took effect is a reason to reach out now. Reading both fields keeps you from acting too early or too late.


##


Which categories matter most for GTM?


Not every category is a buying signal for every seller. Expansion and new-offering events tend to create fresh needs. Leadership changes reset vendor preferences, since a new executive often brings their own stack. Partnership and integration events open a door for anyone who fits the new ecosystem. Choose the two or three categories that map to your product, and treat the rest as background rather than triggers.


##


Frequently Asked Questions


**How do I find companies by a news event?**


Call the Discover News Events endpoint and filter by one or more categories, such as partners_with or hires. It returns companies with matching recent events, ordered by when each was found.


**What news event categories are available?**


Categories are grouped into themes including expansion, investment, leadership, partnership, contract, and new offerings. Each theme contains specific event types you can filter on.


**Can I get alerted when a company has a new event?**


Yes. Follow the companies you care about and receive new events through webhooks, which turns a one-time query into ongoing monitoring.


**Does a news event tell me which companies are involved?**


Yes. Events carry a summary, source, and effective date, and relational events such as partnerships name both companies involved.


##


Related Guides


- [The 37 News Event Categories That Signal a GTM Opportunity](https://blog.predictleads.com/2026/07/06/the-37-news-event-categories-that-signal-a-gtm-opportunity)
- [Companies Mentioning SpaceX](https://blog.predictleads.com/2026/06/19/companies-mentioning-spacex)
- [Company Signals: Hiring, News, Funding, and Technology](https://blog.predictleads.com/2026/06/02/company-signals-hiring-news-funding-technology)
- [Time Outreach by Funding Stage](https://blog.predictleads.com/2026/07/07/time-outreach-by-funding-stage)


###


Ready to see this in your own data?


Get 100 free API requests/month – no credit card, no sales call.


[Start Free →](https://predictleads.com/sign_up?utm_source=blog&utm_medium=cta&utm_campaign=find-companies-by-news-event-category)
