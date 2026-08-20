---
schema_version: "1.0.0"
document_id: "ff3a1217c61cc95545ca146bc0de0654a7fe31979e5e749791c3552f1e2bd878"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/row-level-security-now-available/"
published_at: "2025-11-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T22:25:12.063826+00:00"
content_hash: "sha256:44cca4dc7df1de68a2f19210b2af3dcb0b1ad1d9188d447e802fc0343c4c0103"
---

# Row level security now available

## Row level security is now available in Basedash


Today we’re launching Row level security (RLS) for Postgres in Basedash. RLS lets you control exactly which rows each person can access, so every chart, chat, automation, and database table only shows data a user is authorized to see.


See our docs for full details on how it works and how to set it up:[https://www.basedash.com/docs/features/row-level-security](https://www.basedash.com/docs/features/row-level-security)


---


## Why this matters


- Protect tenant data in multi-tenant apps without duplicating databases
- Scope internal access by team or role to the rows they need
- Keep existing applications working as-is while adding policy-driven security for Basedash usage


---


## How it works


When someone in your Basedash workspace runs a query, we set a Postgres session variable called` basedash.groups` that contains all of that user’s group memberships. You can therefore setup Postgres RLS policies referencing this variable to filter rows automatically.


High level flow:


1. Assign users to groups in Basedash
2. Enable RLS on your Postgres tables
3. Create policies that check` basedash.groups`
4. Queries from Basedash return rows that match the user’s groups[\[1\]](https://www.basedash.com/docs/features/row-level-security)


---


## A simple policy example


Suppose your` orders` table includes a` department` column. You can enable RLS and create a SELECT policy like this:


```text
-- Enable RLS on the table
ALTER   TABLE   orders   ENABLE   ROW   LEVEL   SECURITY  ;


-- Create a policy for SELECT operations
CREATE   POLICY   orders_group_policy   ON   orders
FOR   SELECT
USING   (
current_setting(  'basedash.groups'  , true)   IS   NULL
OR   ','   ||   current_setting(  'basedash.groups'  , true)   ||   ','
LIKE   '%,'   ||   department   ||   ',%'
);
```


This allows normal access when` basedash.groups` isn’t set (existing apps continue to work), and filtered access for Basedash queries.


Multiple group memberships are supported automatically with the comma-separated approach.


---


### Works across Basedash


- Chat: Any SQL queries ran by the AI agent respects your RLS policies
- Automations: Scheduled runs use the automation creator’s groups
- Slack: The Slack app applies RLS when it can match a Slack user to a Basedash user


---


## Basedash Warehouse support


If you use Basedash Warehouse, you can create policies with your warehouse credentials. Just make sure that when you enable RLS on a table, you use` FORCE ROW LEVEL SECURITY` and add permissive policies to keep syncs working for INSERT, UPDATE, and DELETE.


---


## Getting started


1. In Basedash, open your data source settings and review available groups under “Row level security.”
2. Enable RLS on a table in Postgres (` ALTER TABLE "TableName" FORCE ROW LEVEL SECURITY` )
3. Add policies that reference` current_setting('basedash.groups', true)` .
4. Now your users can create chats, charts, and reports with access dictated by their Row level security policies/permissions.


---


### Try it today


RLS is available now for Postgres data sources in Basedash (including the Basedash warehouse). Secure access at the row level, keep teams productive, and ship multi-tenant experiences with confidence.


If you need RLS for another database, reach out to us at[\[email protected\]](https://www.basedash.com/cdn-cgi/l/email-protection#592a2c2929362b2d193b382a3c3d382a31773a3634) .
