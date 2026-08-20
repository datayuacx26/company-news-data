---
schema_version: "1.0.0"
document_id: "6e7a48b3408948c12db13e3cde38e9db3e399f1a29efae0e4a95c844fcaef5cb"
company_key: "yc-rippling"
company: "Rippling"
source_id: "yc-rippling-news-import-ac8763083bf2"
canonical_url: "https://www.rippling.com/blog/introducing-rippling-data-cloud"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-22T11:59:40.213718+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:672c8304b327af64e43fe9c88796f45ca1425f5db62d2e3e2f3af512d4e39ec0"
---

# Introducing Rippling Data Cloud: AI-Powered BI for Your Workforce | Rippling

Every business question starts with “what,” but the next one almost always starts with “who.”


Who booked the most deals last quarter? Which support managers have the fastest resolution times? Which engineering teams have the greatest code velocity? Which stores are drifting into overtime? Which employees were on this team when customer complaints spiked?


At first, these sound like questions about bookings, tickets, pull requests, store sales, time tracking, or CSAT. But the answer depends on knowing who the data is about: the employee, their manager, their team, their location, their role, their permissions, and what that all looked like at the time something took place.


This is where traditional BI infrastructure breaks down. Data warehouses are good at storing and querying data, and BI tools are good at visualizing it. But none of that solves the harder problem: connecting business data to the worker identities, org structures, permissions, and historical context needed to produce an accurate answer to a question about your company.


This is nothing new: people have always struggled to work with data that lacks a sturdy wrapper of context. But this issue is newly important because AI is essentially helpless without it.


For years, we've watched our customers struggle to marry the identity data from Rippling with their operational data using systems like Fivetran and Tableau in an attempt to enable useful analysis. But they often failed. First, it's inherently hard to export data while retaining its referential integrity; once flattened, it's just rows in a spreadsheet. As a result, it's hard to produce an analysis that matches the nuance of a given question, so in the end, customers settle for answers to simpler questions than the ones they began with.


Nearly three years ago, we set out to solve this problem for our customers, and today's launch is the result.


## Introducing Rippling Data Cloud


Rippling Data Cloud is a new suite of products that aggregates data from across your company into Rippling, connects it to worker identity, and makes it available for analysis, visualization and action. It preserves and enriches data context to enable precise and accurate answers to your most important and nuanced business questions.


It's a complete data stack including data connectors, transformations, visualizations, AI-powered analytics, and even inbound Zero-Copy. It understands how all of that data relates to employees, managers, departments, locations, cost centers, permissions, and historical changes in your ever-changing business. That makes it possible to ask questions that traditional BI systems struggle to answer correctly.


## Other options fall short


There are many ways to put AI on top of business data. Most fail because they do not understand the business context behind the data.


To illustrate the concept, consider this business question: how long has it taken new sales reps to close their first deal in each segment over the past four quarters? To answer it correctly, a system needs more than sales data. It needs to know when each rep joined, when they entered a quota-carrying role, which segment they belonged to at the time, who managed them, and which opportunities should count.


Short of asking a data scientist to do the heavy lifting, business users have a few options to answer this question.


**Approach**


**Where it falls short**


General-purpose AI tools, like Claude or ChatGPT


MCPs are slow and usually restricted, and CSVs are always forked from the system of record. Because they lack governed definitions and field history, there's simply no way to compute an answer to the sales rep question. (They might confidently provide a wrong one, however.)


AI inside a single vendor system, like Salesforce


These tools better understand their own data, but lack interfaces to third-party systems that expose the full picture around the organization. In the sales rep question, the AI likely treats the current org chart as static in time; this yields a misleading answer, because orgs always change.


AI inside a data warehouse, like Snowflake AI


Configured correctly, a warehouse AI can query tables from across the business, but it has no privileged view of any particular class of data (sales, HR, etc). Those have to be modeled manually before the AI can answer people-related business questions correctly, including in our sales rep example. It's possible, but the juice isn't usually worth the data science squeeze.


Beyond the challenges of joining and interpreting data, these systems struggle with permissions and governance both in their ability to access data, and their ability to share their output.


## Everything starts from worker identity


Rippling started as an HCM, which makes it uniquely capable of understanding identity data: who works at the company, whom they report to, what they can access, what team they belong to, where they are located, what they do, and how all of that changes over time.


But this data is useful far beyond HR. A GitHub pull request has an author. A Salesforce opportunity has an owner. A helpdesk ticket has an assignee. A point-of-sale transaction has a cashier. A device has an employee. A payroll run has workers, departments, locations, and managers attached to it. Once those records are connected to worker identity, business data becomes easier to analyze, easier to govern, and easier to act on.


Rippling Data Cloud uses that identity layer across the entire stack: data ingestion, cataloging, transformations, history, dashboards, AI, and custom applications.


## What we're launching


Rippling Data Cloud includes every component needed to run a complete AI-powered BI stack from managed connectors up to visualization and collaboration. It's a just-add-water approach that simplifies data analysis for every user in your company.


### Dashboards


Rippling AI generates charts and dashboards with trusted, reusable components and inspectable SQL from natural-language prompts. Users can also build classic dashboards with charts, filters, pivots, calculated fields, and saved views. BI is different inside Rippling because dashboards inherit the context of the platform. For example, a manager can see the same dashboard as another manager, but automatically scoped to their own team. A user can drill from a chart into the employees, devices, opportunities, tickets, or other records behind the number. Unlike dashboards in standalone BI tools, which are disconnected reporting artifacts, Rippling Dashboards become a live navigation layer over the business. Read the full article on[BI and Dashboards](https://www.rippling.com/blog/rippling-data-cloud-bi-and-dashboards) .


### Data Connectors


Data Connectors bring third-party business data into Rippling, preserving and enriching the context that makes it useful. Traditional ETL tools move data from one system to another, but leave teams to rebuild joins, permissions, metadata, object relationships, and worker identity mappings by hand. Rippling Data Connectors do that work automatically: they import data from systems like CRMs, support tools, finance systems, and other warehouses, then map that data into Rippling Custom Objects. That means a GitHub pull request, support ticket, sales opportunity, or point-of-sale transaction lands already connected to the right employee, manager, team, permissions model, and business context. The result is data that is immediately easier to analyze with AI, govern through Data Catalog, reuse in Transformations, and put to work in dashboards, workflows, and Custom Apps. Read the full article on[Data Connectors](https://www.rippling.com/blog/rippling-data-cloud-data-connectors) .


### Transformations


Transformations turns raw business data into governed, reusable datasets. Instead of letting every dashboard, SQL query, spreadsheet, or AI prompt define metrics slightly differently, Transformations gives companies a central place to encode the logic behind the metrics you use, like revenue, margin, store performance, customer risk, or whatever else matters to a given operation. Analysts can write SQL directly, business users can use Rippling AI to help define and refine logic, and the resulting datasets can be reused across Dashboards, AI answers, workflows, and Custom Apps. Read the full article on[Transformations](https://www.rippling.com/blog/rippling-data-cloud-transformations) .


### Data Catalog and Lineage


Data Catalog gives Rippling Data Cloud and Rippling AI a map of your business data. It is the central inventory for every data object in Rippling, including native Rippling data, data from Data Connectors, Transformations, and external warehouse data. For analysts, it makes data easier to find, understand, trust, and govern, with searchable documentation, lineage, usage metadata, and field-level permissions. For AI, it is even more important: the Catalog gives Rippling the context it needs to choose the right objects, fields, joins, filters, and business definitions when answering questions. Read the full article on[Data Catalog and Lineage](https://www.rippling.com/blog/rippling-data-cloud-data-catalog) .


### History


Object History lets Rippling Data Cloud answer historical business questions without projecting today's org chart backward. Most business analysis is really asking what was true at a specific point in time: whom someone reported to, what team they were on, when their role changed, what workflow ran, who approved a change, or which org structure applied when a metric moved. Object History makes that context queryable across Rippling, so reports, dashboards, Transformations, workflows, Custom Apps, and Rippling AI can reason from the actual historical state of the business. Although many business questions look like they're about revenue, payroll, support volume, or headcount, they're really questions about people in time: who did what, when did they do it, and what was true about the business around them at that moment. Read the full article on[History](https://www.rippling.com/blog/rippling-data-cloud-object-history) .


### Custom Apps


Custom Apps let teams build company-specific software on top of the data inside Rippling. Dashboards show you what's happening, but most business problems still require a process: an approval, an exception review, a payroll adjustment, a remediation workflow, or a record that someone needs to update. Custom Apps use the same data, permissions, workflows, and object model that power the rest of Rippling. That means a Salesforce opportunity, Brivo badge-in, Mindbody class record, or Litmos certification can become part of an application inside Rippling, not just a row in a report. Data can trigger workflows, route for approval, update records, stage payroll changes, and give teams a structured interface for the process itself. Read the full article on[Custom Apps](https://www.rippling.com/blog/custom-apps-and-rippling-data-cloud) .


### Snowflake Zero Copy


Zero Copy for Snowflake lets companies use warehouse data inside Rippling Data Cloud without building custom pipelines. Data from Snowflake can appear in Rippling as external objects, where it can be joined to worker identity, governed by Rippling permissions, surfaced in the Data Catalog, and used by Rippling AI, Dashboards, and Transformations. Your warehouse remains the source of truth, but Rippling adds the worker identity, org context, permissions, and history needed to answer business questions that depend on who did what, when, and where they sat in the business. Read the full article on[Snowflake Zero Copy](https://www.rippling.com/blog/rippling-data-cloud-zero-copy-for-snowflake) .


## A new era for business intelligence


Our customers are already deriving value from our AI-powered business intelligence. They're answering business questions that felt impossible to answer before.


1/3


Rippling Data Cloud, together with Rippling AI, unlocks a new frontier of analytical capabilities. Answer questions about the who behind every what. Better understand your company's performance dynamics across sales, engineering, and operations using only a conversational interface. Rippling Data Cloud will instantly become a mainstay of every data-driven leader.


If you'd like to try it out — even if you're not a Rippling customer today —[please contact us](https://www.rippling.com/request-demo) .


Disclaimer


Rippling and its affiliates do not provide tax, accounting, or legal advice. This material has been prepared for informational purposes only, and is not intended to provide or be relied on for tax, accounting, or legal advice. You should consult your own tax, accounting, and legal advisors before engaging in any related activities or transactions.
