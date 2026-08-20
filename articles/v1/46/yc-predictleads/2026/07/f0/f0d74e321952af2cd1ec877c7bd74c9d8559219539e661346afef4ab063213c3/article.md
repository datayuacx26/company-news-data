---
schema_version: "1.0.0"
document_id: "f0d74e321952af2cd1ec877c7bd74c9d8559219539e661346afef4ab063213c3"
company_key: "yc-predictleads"
company: "PredictLeads"
source_id: "yc-predictleads-rss-ec716ebf37eb"
canonical_url: "https://predictleads.com/blog/find-companies-hiring-role-onet/"
published_at: "2026-07-24T13:06:00+00:00"
first_seen_at: "2026-07-25T19:43:43.337837+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:859158e21e967dc4b44e1df36626680e8b62fa78394990e0b5ab4cff1f5ab2b9"
---

# How to Find Companies Hiring for a Specific Role with O*NET Codes

To find companies hiring for a specific role, call the PredictLeads **[Discover Job Openings](https://docs.predictleads.com/api_endpoints/job_openings_dataset/retrieve_a_list_of_job_openings)** endpoint with one or more **O*NET codes** . O*NET is the standard occupational classification maintained at onetonline.org, and it lets you target a role precisely instead of guessing at job titles. A query for Data Scientists (code 15-2051.00) currently returns **263,776 open roles** , and 130,603 of those are in the United States.


Hiring is the clearest growth-intent signal there is. A company opening roles is spending, building, and changing, which is exactly when GTM teams and agencies want to reach it.


Table of Contents


Toggle


##


What are O*NET codes and why use them?


O*NET codes map job titles to standardized occupations, so “Data Scientist,” “Applied Scientist,” and “ML Scientist” all resolve to the same occupation. That removes the noise of free-text title search and makes counts comparable across companies. Some common codes and their current live volumes:


O*NET code Occupation Open roles now


15-2051.00 Data Scientists 263,776


15-1254.00 Web Developers 815,401


17-2071.00 Electrical Engineers 132,551


You can find any code in the full list at[onetonline.org](https://www.onetonline.org/) , then pass one or several at once.


##


How do you find companies hiring a role?


Pass the codes, and optionally narrow by company location and seniority. Through the MCP server:


```text
Using the PredictLeads MCP server, find US companies hiring Data Scientists
at director level or above. Return company name, domain, and role title.
```


The equivalent endpoint call:


```text
discover_job_openings(
onet_codes: "15-2051.00",
location: "United States",
seniority: "director"
)
```


The location filter applies to the company, and the seniority filter ranges from junior through C level, so you can separate a company staffing a team from one hiring its first leader.


##


What does a job opening record include?


Each opening comes back structured, not as raw text:


- a **normalized_title** alongside the original title
- **onet_data** with the occupation code, family, and name
- **seniority** and role categories
- **location_data** broken into city, state, and country
- **salary_data** where the posting includes it, normalized to USD
- the source URL and first-seen date


That structure is what lets you filter by role and seniority with confidence, rather than pattern-matching messy titles.


##


How do you combine hiring with technology?


Hiring alone is a strong signal. Hiring plus a known technology is a precise one. Take the companies from a technology query and keep only those hiring your target role, or start here and enrich each company with its detected stack:


```text
company_technology_detections(company_id_or_domain: "example.com")
```


Because technology detections carry the departments where a tool was found, you can line up “runs Snowflake” with “hiring Data Scientists” and reach companies that are actively building on the exact stack you serve. That is the double signal that separates a real opportunity from a cold name.


##


How do GTM teams and agencies use this?


- **GTM timing.** Reach out while a team is being built, before the budget and vendor choices are locked.
- **Territory focus.** Filter by location to feed the right roles to the right reps.
- **Seniority targeting.** Hiring a first head of a function is a different motion than scaling an existing team; the seniority filter separates them.
- **Agency campaigns.** Build role-based lists for HR-tech, recruiting, and developer-tool clients, where a hiring signal is the whole pitch.


##


How do you read intent from the role being hired?


The role a company hires for tells you what it is about to do, which shapes the message before you ever reach out. A few patterns hold up across industries:


- **Engineering and data roles** point to product build-out and new infrastructure, so a company hiring data scientists is likely investing in analytics or machine learning.
- **Sales and marketing roles** point to go-to-market expansion, which often means new tools for outreach, enablement, and reporting.
- **Operations roles** point to scaling or production demand, a sign the company is growing into new process needs.
- **First senior hires** , caught with the seniority filter, mean a function is being stood up from scratch, which is the moment to be the vendor they standardize on.


Reading the role this way turns a hiring list into a set of tailored conversations rather than one generic pitch.


##


Frequently Asked Questions


**How do I find companies hiring for a specific role?**


Call the Discover Job Openings endpoint with one or more O*NET codes for the role. It returns companies with matching open roles, optionally filtered by company location and seniority.


**What is an O*NET code?**


O*NET is a standardized occupational classification. Each code maps many job titles to one occupation, so you can target a role precisely instead of matching free-text titles. The full list is at onetonline.org.


**Can I filter job openings by seniority and location?**


Yes. You can narrow by company location and by seniority levels ranging from junior through C level.


**How do I combine hiring signals with technology use?**


Cross-reference companies hiring a role with the Technology Detections dataset, so you reach companies that are both building a team and running the stack your product serves.


##


Related Guides


- [Companies Hiring Data Engineers and Adopting Data Tools](https://blog.predictleads.com/2026/06/03/companies-hiring-data-engineers-adopting-data-tools)
- [Time Outreach by Funding Stage](https://blog.predictleads.com/2026/07/07/time-outreach-by-funding-stage)
- [Company Signals: Hiring, News, Funding, and Technology](https://blog.predictleads.com/2026/06/02/company-signals-hiring-news-funding-technology)
- [Build a GTM Agent on the PredictLeads MCP Server](https://blog.predictleads.com/2026/07/09/gtm-agent-mcp-server)


###


Ready to see this in your own data?


Get 100 free API requests/month – no credit card, no sales call.


[Start Free →](https://predictleads.com/sign_up?utm_source=blog&utm_medium=cta&utm_campaign=find-companies-hiring-role-onet)
