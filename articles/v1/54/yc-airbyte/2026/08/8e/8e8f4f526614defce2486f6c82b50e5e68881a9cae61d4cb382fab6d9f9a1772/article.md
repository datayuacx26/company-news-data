---
schema_version: "1.0.0"
document_id: "8e8f4f526614defce2486f6c82b50e5e68881a9cae61d4cb382fab6d9f9a1772"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/hubspot-linkedin-ads-agent-connectors"
published_at: "2026-08-03T16:32:00+00:00"
first_seen_at: "2026-08-03T20:05:59.011998+00:00"
fetched_at: "2026-08-03T20:38:58.255627+00:00"
content_hash: "sha256:19e17119d1c294d2f98928ccc09e6cf8f30be49288a704f2a2b545106f23b474"
---

# New Agent Connectors: Hubspot and LinkedIn Ads

By popular request, Airbyte Agents now features connectors for HubSpot and LinkedIn Ads! These were among the most requested connectors, and we’re as excited about them as you are.


Both connectors are available via Airbyte Agents app, Agent MCP, SDK, CLI, and API. They also pair nicely: HubSpot has your pipeline, LinkedIn Ads has what you spent to build it, and an agent with access to both can connect the two without a single CSV export.


## **Your agents can execute actions, not just ingest context**


The cool thing about these connectors is they come with write and action capabilities, not just fetch.


With HubSpot, an agent that can identify a deal that has gone quiet and escalate it to the relevant sales rep. All of the engagement objects (notes, calls, emails, meetings, tasks) support creates, updates, and deletes. The four core CRM objects (contacts, companies, deals, and tickets) support creates and updates.


Agents can use the LinkedIn Ads connector to pause or adjust budgets on underperforming campaigns in near real-time. Accounts, campaigns, campaign groups, and creatives are all writable.


We still recommend a human in the loop, of course. But as always, our connectors are open-source and customizable, so you decide the level of autonomy you grant your agent.


However, LinkedIn only permits a true delete on records still in DRAFT. Anything live has to be soft-deleted by setting its status to` PENDING_DELETION` . It’s important to note that the connector does not do this for you: it exposes delete, and the caller must issue the update to PENDING_DELETION themselves.


I think this is a great guardrail, but I also think it’s fair to let users know about this in advance.


## **Questions that need both systems**


The value of Airbyte Agents has always been multi-source queries. With the Context Store under the hood, you get the full breadth of LinkedIn analytics filters (seniority, industry, company size, and more) as well as Hubspot’s contact and deal data. That enables you to ask questions neither system could answer alone. The possibilities are endless:


- Which campaigns reached the seniority levels we actually sell to?
- Which industries produced pipeline last quarter versus which ones just produced clicks?
- Are our best-converting accounts the ones we're spending the most on?


Lead Forms and Lead Form Responses sit behind the same interface, so lead-gen data and spend data come from the same place. If you've customized HubSpot heavily, the schemas and objects entities expose your custom object definitions. This is huge. It lets agents reason on your customized setup instead of generalizing.


## **The Context Store supercharges search**


I love the Context Store.


It’s the best way for agents to search and identify the relevant information to execute tasks. When you set up the Context Store for Hubspot and LinkedIn Ads, you basically give them pre-assembled context and capabilities that API plugins and vendor MCP servers can’t provide.


Both connectors support` context_store_search` , which does cool things like:


- Provide filters the native APIs don't offer (` fuzzy` ,` keyword` ,` like` ,` in` , and the usual comparisons)
- Help an agent narrow down what it wants locally before it spends an external call.


MAKEFILE


```text
# narrow the deal set locally


deals = await hubspot.deals.context_store_search(


query={ "filter"  : { "eq"  : { "archived"  : False}}}


)


# then pull live performance for the campaigns behind them


perf = await linkedin_ads.ad_campaign_analytics.list(


q= "analytics"  ,


pivot= "CAMPAIGN"  ,


time_granularity= "MONTHLY"  ,


date_range= "(start:(year:2026,month:1,day:1),end:(year:2026,month:7,day:31))"  ,


campaigns= "List(urn%3Ali%3AsponsoredCampaign%3A123)"  ,


)
```


Search first, then fetch. That ordering is what keeps an exploratory question from turning into a dozen paginated API calls.


What about authorization? It’s pretty straightforward.


HubSpot takes OAuth or a Private App token and LinkedIn Ads takes OAuth or an access token. Pick one at setup and Airbyte stores and refreshes it, so token rotation never lands in your agent code.


## **Get started**


Connect either connector at[app.airbyte.ai](https://app.airbyte.ai/) . The[connector docs](https://docs.airbyte.com/ai-agents/connectors) have the full entity and action reference if you want to see everything available before you wire anything up.


Really excited to see what you all build with these! We’re leveraging them for our own internal marketing efforts as well, and are enjoying the results so far.
