---
schema_version: "1.0.0"
document_id: "27e1ffd9041b5722c9f15a1a021374b72044cf90b86e3f5b823d224126407244"
company_key: "yc-worklytics"
company: "Worklytics"
source_id: "yc-worklytics-news-import-9ab18239f248"
canonical_url: "https://www.worklytics.co/blog/how-to-extract-claude-spend-data-turn-into-per-cost-team-model"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-10T20:55:53.092918+00:00"
fetched_at: "2026-08-10T20:55:54.732797+00:00"
content_hash: "sha256:eb3151831fe35a874f590d6f771f143431c3c58439177c4d5335e4b11394bc97"
---

# How to Export Claude Spend Data & Turn It Into a Per-Team Cost Model

## **TL;DR**


- Anthropic exposes Claude spend through two separate APIs, split by product: the[Usage and Cost Admin API](https://platform.claude.com/docs/en/manage-claude/usage-cost-api) for Claude Console / Platform (Admin key), and the[Claude Enterprise Analytics API](https://platform.claude.com/docs/en/manage-claude/analytics-api) for claude.ai (Analytics key). The key types are not interchangeable; the wrong one returns an authentication error, not partial data.
- To export: pick the API for your organization, call the cost and usage endpoints with a date range and group_by, page through the results, and parse the amounts (decimal strings in cents). Full requests and a variable reference are below.
- Every dimension Anthropic returns describes infrastructure or individuals: workspace, API key, model, service tier, or user. No endpoint returns department, team, role, manager, or cost center.
- Spend is also scattered across surfaces: Priority Tier appears only in the usage endpoint, Code Execution only in the cost endpoint, and Claude Code run through Bedrock, Vertex, or Foundry is excluded entirely.
- Worklytics ingests both export paths, reconciles the surfaces into one figure, and joins Claude identity to your HRIS, so the same rows become spend per team, per model, and per function without a hand-maintained mapping table.


## **Export Claude spend data: choose your API and key**


The first decision determines everything downstream, because the two reporting APIs use different keys that are not interchangeable. Calling the wrong one returns an authentication error, not a partial answer.


*Which Claude cost API applies to Console versus Enterprise organizations: key types, endpoints, granularity, freshness, and rate limits.*


### **If you manage Claude through Claude Console (Claude Platform)**


*The Cost page in the Claude Console: spend grouped by model, with workspace, API-key, and month filters and an Export action. Source: Anthropic Claude Console.*


- Use the Usage and Cost Admin API with an Admin API key (sk-ant-admin01-…).
- Cost lands at /v1/organizations/cost_report at daily granularity; token usage at /v1/organizations/usage_report/messages at 1m, 1h, or 1d.
- Data typically appears within five minutes of request completion, and the API supports sustained polling once per minute.
- On Claude Platform on AWS these programmatic endpoints are not available; use the Console Usage and Cost pages instead.


**Where to find it:** sign in to the Claude Console and open Usage or Cost in the left sidebar. On the Cost page, the Export button (top right) downloads the current view as a CSV; the[Usage and Cost Admin API](https://platform.claude.com/docs/en/manage-claude/usage-cost-api) returns the same data programmatically. Anthropic's[Cost and usage reporting guide](https://support.claude.com/en/articles/9534590-cost-and-usage-reporting-in-the-claude-console) walks through both pages.


### **If you are a Claude Enterprise organization (claude.ai)**


- Admin keys do not exist for the parent organization. The primary owner creates an Analytics API key in organization settings; it carries the read:analytics scope.
- Endpoints live under /v1/organizations/analytics/. Rate limits are organization-level, defaulting to 60 requests per minute across all endpoints.
- Cost and usage data appears within four hours (up to 24) and stays revisable for 30 days. Cost endpoints apply to usage-based Enterprise plans; on seat-based plans they reflect usage credits only.


**Where to find it:** in claude.ai, click your initials in the lower-left corner and choose Analytics. Owners and admins see usage and adoption; spend is visible to owners only. The[Claude Enterprise Analytics API](https://platform.claude.com/docs/en/manage-claude/analytics-api) returns the same metrics programmatically, and Anthropic's[usage analytics guide](https://support.claude.com/en/articles/12883420-view-usage-analytics-for-team-and-enterprise-plans) covers the dashboard.


*The Analytics dashboard in claude.ai for Enterprise organizations: weekly active members, per-product usage, and total spend across Claude apps. Source: Anthropic (claude.ai).*


Most organizations run both products at once, which otherwise means two ingestion pipelines and two key types to maintain. **Worklytics reads both paths into a single dataset** , so the choice above becomes a configuration detail rather than two parallel exports you reconcile by hand.


### **Console spend or Claude Code spend?**


If you're on the Console / Platform side, decide which spend you actually need before you pull data: general API spend and Claude Code spend are two different exports, from two different endpoints.


Console (general API) spend Claude Code spend


**What it covers** All Claude API usage across your organization Claude Code developer activity only


**Endpoint**` cost_report` ,` usage_report/messages`` usage_report/claude_code`


**Granularity** Workspace, API key, model, service tier Per developer, per day


**Metrics** Tokens and cost (USD) Sessions, lines of code, commits, PRs, tool acceptance, estimated cost by model


**Key** Admin API key Admin API key


**Cost to use** Part of your API bill Free


**Best for** Total spend and chargeback by workspace Per-developer cost and productivity


Claude Code token cost also rolls into the general cost report, so don't add the two together; use the Claude Code Analytics API when you specifically want the per-developer view. On a claude.ai Enterprise plan, Claude Code activity comes through the Enterprise Analytics API instead, not this endpoint.


## **Run the export: requests and variables**


The export follows the same seven steps for every endpoint:


1. Create your key: an Admin API key for Console and Claude Code data, or an Analytics API key for Enterprise.
2. Pick the endpoint for what you're exporting: cost_report (Console cost), usage_report/messages (Console token usage), usage_report/claude_code (Claude Code, per developer), or the analytics/ endpoints (Enterprise).
3. Set the date range and grouping: starting_at, ending_at, and any group_by\[\] or filters.
4. Send the request with your key in the x-api-key header. The cURL examples below show a full request for each endpoint: the Console cost report, the Console usage report, the Enterprise Analytics API, and the Claude Code Analytics API.
5. Page through the results: follow next_page until has_more is false, keeping the query identical.
6. Parse the amounts: they're decimal strings in cents, so divide by 100.
7. Write it out: flatten to CSV or load it into your warehouse.


With the key created, an export is a paginated pull over a fixed date range. On the Console side, a monthly cost report grouped by workspace and description is a single call:


```text
curl   "https://api.anthropic.com/v1/organizations/cost_report?\
starting_at=2026-06-01T00:00:00Z&\
ending_at=2026-06-30T00:00:00Z&\
group_by[]=workspace_id&\
group_by[]=description"   \
-H   "anthropic-version: 2023-06-01"   \
-H   "x-api-key: $ANTHROPIC_ADMIN_KEY"


```


For token-level detail (and for anything billed under Priority Tier), use the usage endpoint, which supports finer buckets and more grouping dimensions:


```text
curl   "https://api.anthropic.com/v1/organizations/usage_report/messages?\
starting_at=2026-06-01T00:00:00Z&\
ending_at=2026-06-30T00:00:00Z&\
group_by[]=model&\
group_by[]=service_tier&\
bucket_width=1d"   \
-H   "anthropic-version: 2023-06-01"   \
-H   "x-api-key: $ANTHROPIC_ADMIN_KEY"


```


Claude Enterprise organizations call the Analytics API with the Analytics key. The resource paths for user activity, summaries, and cost and usage all live under the analytics base; see the[Analytics API reference](https://platform.claude.com/docs/en/manage-claude/analytics-api) for each:


```text
curl   "https://api.anthropic.com/v1/organizations/analytics/<resource>?\
starting_at=2026-06-01&\
ending_at=2026-06-30"   \
-H   "anthropic-version: 2023-06-01"   \
-H   "x-api-key: $ANTHROPIC_ANALYTICS_KEY"


```


For per-user Claude Code cost specifically, the[Claude Code Analytics API](https://platform.claude.com/docs/en/manage-claude/claude-code-analytics-api) returns sessions, lines of code, commits, pull requests, per-tool acceptance, and estimated cost by model, one UTC day per request, free to use:


### **The variables that matter**


A handful of parameters control every export. These are the ones that change your results:


Variable Applies to Values and notes


` starting_at` /` ending_at` all ISO-8601 UTC. If you omit` ending_at` , the response includes an incomplete tail past` data_refreshed_at` ; set it at or before a previously returned` data_refreshed_at` for stable results.


` bucket_width` usage` 1m` ,` 1h` , or` 1d` . The cost report is` 1d` only.


` group_by\[\]` cost / usage Cost:` workspace_id` ,` description` . Usage adds` api_key_id` ,` model` ,` service_tier` ,` context_window` ,` inference_geo` ,` speed` .


` models\[\]` ,` service_tiers\[\]` ,` workspace_ids\[\]` ,` api_key_ids\[\]` usage Filters.` service_tiers` includes` priority` , the only place Priority Tier spend appears.


` limit` ,` page` all Page size and the pagination cursor. The cursor is bound to the exact query that issued it.


### **Page it, parse it, write it out**


Two endpoints, one loop. Keep the same parameters on every page and follow the cursor until it runs out:


```text
page=  ""
while   : ;   do
resp=$(curl -s   "$BASE/cost_report?starting_at=$FROM&ending_at=$TO&group_by[]=workspace_id${page:+&page=$page}"   \
-H   "anthropic-version: 2023-06-01"   -H   "x-api-key: $ANTHROPIC_ADMIN_KEY"  )
echo   "$resp"   | jq -c   '.data[]'   >> claude_cost.jsonl
[   "$(echo "  $resp  " | jq -r .has_more)"   =   "true"   ] ||   break
page=$(echo   "$resp"   | jq -r .next_page)
done


```


Three details decide whether the exported rows survive downstream:


- Amounts are decimal strings in cents. "41280.000000" means $412.80; parse as a decimal and divide by 100, never as a binary float once totals reach the millions.
- Default-workspace spend returns a null workspace_id, and Workbench usage returns a null api_key_id. A join that assumes non-null keys silently drops both.
- Changing a filter or grouping mid-pagination returns a 400 rather than resuming; restart from the first page. Daily buckets also cap at 31 per request (168 hourly, 1,440 per minute), so any quarter is a loop, not a single call.


To land it somewhere useful, flatten the JSON to CSV on the way out:


```text
jq -r   '.data[] | [.date, .workspace_id, (.amount|tonumber/100), .description] | @csv'   \  claude_cost.jsonl > claude_cost.csv
```


*Claude data freshness across Console usage and cost, Enterprise cost and usage, and Enterprise engagement, with the 30-day revision window marked.*


This is the plumbing that quietly breaks internal reporting: the 30-day revision window, the null-key rows, the cursor rules, the cents parsing. **Worklytics handles it on ingestion** , so Claude cost arrives as a maintained feed with a stable as-of date rather than a script someone babysits each close. One operational note either way: Admin and Analytics keys carry organization-wide read scope, so store them in a secret manager and aggregate per-user spend to group level before sharing it.


## **What Anthropic's export can't tell you**


A clean export answers one question well: what did we spend on Claude infrastructure. It cannot answer the question finance actually asks (what did each team spend), and the reasons are structural, not a matter of finding the right parameter.


### **There is no organizational dimension**


Anthropic's billing model has no concept of your org chart. The cost report groups by workspace and description; usage adds API key, model, service tier, context window, data residency, and speed. Every one of those describes infrastructure. Two teams sharing a workspace are indistinguishable, and a contractor and a VP of Engineering on the same API key produce identical rows.


This is the gap **Worklytics** closes first: it maps each workspace and user to an HRIS record, so the same rows carry department, function, role, level, manager, and tenure.


### **Spend is scattered across surfaces**


"Claude spend" is really three billing realities (direct API usage, Claude Code, and Enterprise seats) that sit in different systems and refresh on different schedules. Priority Tier costs appear only in the usage endpoint; Code Execution only in the cost endpoint; Workbench activity carries no API key at all. Assembling one number means stitching these together yourself.


*Illustrative Worklytics view: estimated monthly Claude cost by surface, with Claude Code driving growth ahead of seat-based chat spend.*


**Worklytics reconciles the surfaces into a single figure per period** , so Claude Code growth and seat-based chat are read on one line instead of being added up by hand each month.


### **Cloud-marketplace usage is missing**


If you run Claude Code through Amazon Bedrock, the Enterprise Analytics API returns none of it. The Claude Code Analytics API is narrower still: it covers Claude Code on the Claude API only, and excludes Bedrock, Microsoft Foundry, Google Cloud (Vertex), and Claude Platform on AWS. Any team on a cloud marketplace is understated until that usage is reconciled against cloud cost data.


**Worklytics surfaces this shortfall in the same view** , so the missing marketplace spend is visible and can be reconciled against your cloud cost data rather than silently dropping out of a per-team total.


### **Even Enterprise is per-user, not per-team**


The Enterprise Analytics API is a real improvement: it returns per-user daily metrics and per-user cost across chat, Claude Code, Cowork, and Office Agent. But it still returns a user identifier, not a department. And the identifier itself varies: each Claude Code record carries an actor that is either a user_actor with an email_address (OAuth) or an api_actor with only an api_key_name (API key), so a join keyed on email silently drops the api_actor rows, which are usually CI pipelines and service accounts. You are one join away from a usable answer, and that join is where reporting stalls.


One scheduling caveat: engagement data for a day is aggregated at 10:00 UTC the next day and becomes queryable three days later, while cost data arrives within hours but stays revisable for 30 days. Join them naively and a dashboard shows populated cost against an empty active-user count for several days, which is expected behavior, not a broken pipeline.


## **Where Worklytics goes further: per team, per model, and beyond**


The export is the input. Worklytics turns it into the per-team, per-model view Anthropic's endpoints structurally can't produce.


### **One join, resolved as of the spend date**


Worklytics ingests Claude usage and cost (both the Console and Enterprise paths) alongside HRIS attributes, so every exported row inherits department, function, role, manager, and tenure. Because it resolves organizational identity as of the date of the spend rather than the date of the query, a March reorganization doesn't rewrite who owned February's cost; the answer is the same every time it is rebuilt.


*Worklytics view: estimated monthly Claude spend by department, split by surface (Claude Code, Cowork, Claude.ai).*


Anthropic's export already carries a model dimension; what it lacks is the team dimension. Once Worklytics adds that, the two read together: you can see spend per team and per model at once (Engineering's Opus versus Sonnet, say), and split each team by surface across Claude Code, chat, and Cowork. That cross-tab is exactly what a raw export can't produce, because it has no idea what a team is.


### **From reconciled spend to chargeback and ROI**


Worklytics[DataStream](https://www.worklytics.co/datastream) exports the joined dataset to your own warehouse or BI environment, where finance reconciles Claude spend against the general ledger using the same cost-center codes it uses for every other vendor. Pairing spend with adoption (weekly active users and seat utilization) turns the number into a decision: three measures per function (total spend, spend per weekly active user, and share of licensed seats with any weekly activity) separate intensive use from paid-for-but-idle capacity, which is the difference between a renewal you grow, one you reallocate, and one you renegotiate.


*Worklytics view: average tokens per request rising as the complexity of delegated work grows.*


*Worklytics view: average tokens per request rising as the complexity of delegated work grows* The same pipeline already handles[Claude Code usage tracking](https://www.worklytics.co/blog/tracking-claude-code-usage) and[Claude Enterprise usage tracking](https://www.worklytics.co/blog/track-if-employees-are-using-claude-enterprise) , which matters because most organizations run both surfaces at once and need them reconciled into one number per team. See[how Worklytics measures AI](https://www.worklytics.co/measureai) for the full model.


## **Frequently Asked Questions**


### **Can I export Claude spend data broken down by department directly from Anthropic?**


No. The cost endpoints group by workspace, API key, model, and service tier, and the Enterprise Analytics API groups by individual user. None of these carries department, function, or role. Department-level attribution requires joining the workspace ID or user email to an HRIS source, the step Worklytics automates.


### **Can I break Claude spend down per model?**


Yes. Both the usage report (group_by\[\]=model) and the Claude Code Analytics API (per-model cost breakdown) expose a model dimension, so a per-model export is straightforward. What Anthropic can't give you is per-model and per-team together; that needs the organizational join.


### **What is the difference between the Usage and Cost Admin API and the Claude Enterprise Analytics API?**


They serve different products with non-interchangeable keys. The Usage and Cost Admin API covers Claude Console and Platform organizations with an Admin key and reports usage and cost by workspace. The Claude Enterprise Analytics API covers Claude Enterprise organizations on claude.ai with an Analytics key created by the primary owner, and reports per-user engagement and cost across chat, Claude Code, Cowork, and Office Agent.


### **How far back can I export Claude Enterprise cost data?**


Claude Enterprise Analytics data is available for dates on or after January 1, 2026. Cost values remain revisable for up to 30 days after the usage date, so exports older than 30 days are the most stable.


### **Does Claude Code usage on Bedrock or Vertex appear in the exported spend data?**


No. The Enterprise Analytics API doesn't return Claude Code activity running through Amazon Bedrock, and the Claude Code Analytics API covers Claude Code on the Claude API only, excluding Bedrock, Microsoft Foundry, Google Cloud, and Claude Platform on AWS. Those deployments need cloud cost data to be reconciled into a complete per-team view.


### **Why do two people running the same Claude cost query get different totals?**


Three expected mechanisms: Enterprise cost values stay revisable for 30 days; omitting ending_at includes an incomplete tail past data_refreshed_at; and Priority Tier and code-execution costs sit in different endpoints. Fix it by stating an as-of date, setting ending_at at or before a returned data_refreshed_at, and documenting which cost categories the report includes.


‍
