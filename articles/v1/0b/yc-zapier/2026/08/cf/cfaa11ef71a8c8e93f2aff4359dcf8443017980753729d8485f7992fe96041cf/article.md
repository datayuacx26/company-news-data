---
schema_version: "1.0.0"
document_id: "cfaa11ef71a8c8e93f2aff4359dcf8443017980753729d8485f7992fe96041cf"
company_key: "yc-zapier"
company: "Zapier"
source_id: "yc-zapier-rss-44f77ec59be6"
canonical_url: "https://zapier.com/blog/zapier-tables-guide"
published_at: "2026-08-03T05:00:00+00:00"
first_seen_at: "2026-08-03T23:02:23.028490+00:00"
fetched_at: "2026-08-04T00:20:40.433623+00:00"
content_hash: "sha256:ba828a784e843b01b8b516fa6f545af9313907414c45e8906898690dd0d9766c"
---

# Zapier Tables: Store, move, and act on your data automatically

Plenty of tools can store data. But you probably don't need just a holding tank—you need your data to go somewhere and do something. And with most databases, moving data into other apps requires you to lean on integrations that may or may not work reliably.


[Zapier Tables](https://zapier.com/apps/zapier-tables/integrations) was designed specifically for AI-powered automation. That means you can use it to build data tables and trigger workflows straight from them. You can even add AI steps to enhance new and existing rows. You also get granular permissions, so you can decide exactly who can view, edit, or update records without handing over the keys to everyone involved in your work. Here, I'll show you the ropes.


You can build an unlimited number of Zapier tables on every Zapier plan. The number of rows you can store scales with each plan. And adding a Tables step to a Zap doesn't count toward your task usage. Compare Zapier plans on our[pricing page](https://zapier.com/pricing) .


## Table of contents


-


What is Zapier Tables?


-


What you can do with Zapier Tables


-


How to get started with Zapier Tables


## What is Zapier Tables?


Zapier Tables is a database tool that lets you store, edit, share, and automate your data—in one spot. If you've used Google Sheets or Excel before, you're familiar with the basics: every Zapier table contains "records" (what we call rows) as well as "fields" (what we call columns).


But unlike a spreadsheet, which you have to open and edit manually, Tables works hand in hand with automation. A change to a record—something added, updated, or a button clicked—can kick off a Zap, sending that data to more than 9,000


other apps in the Zapier directory. And *within* a Zap, you can create new table records, find existing ones, or update them, so your data and your automation remain in step.


Because Tables lives inside Zapier, your data never has to leave the platform, either. Admin-level app restrictions apply directly to Tables, so if IT blocks an app in the Admin Center, it can't slip back in through an import or a connected-app dropdown in your table. Permissions are granular too. You control who can view, edit, or update the data powering your workflows. And every Tables event streams to your SIEM alongside the rest of your Zapier activity, keeping your audit trail complete.


Key features of Zapier Tables include:


-


**Triggers:** Start a Zap when a record is added, updated, or deleted in a table—or when someone clicks a button on a record. You can also scope triggers to specific fields or views, so only the changes you care about kick off a workflow.


-


**Actions:** Create, find, update, or delete records from inside any Zap. You can also duplicate tables, increment values, change button states, and calculate summary formulas—all as steps in a workflow.


-


**Granular permissions and governance:** Table permissions are separate from Zap permissions, so you can grant someone access to edit records without exposing your workflows, and vice versa.


-


**AI fields:** Connect to OpenAI for free and automatically generate content for each record—to draft outreach emails, summarize notes, or classify leads—based on a prompt that references other fields in your table.


-


**AI enrichment:** Write a single prompt that populates multiple fields at once when a new record arrives. Use this to do things like fill in company details from a domain, categorize support tickets, or complete contact profiles without manual entry.


-


**Buttons:** Add clickable buttons to any records that trigger or continue a Zap. Use them for one-click approvals, rejections, or any action that needs a human decision before the workflow moves on.


-


**Linked records:** Create relationships between tables so data stays in sync. If you link an orders table to a customers table, for example, an address update in one will automatically reflect in the other.


-


**Record and summary formulas:** Build calculations directly in your table—multiply quantity by price, apply conditional logic with IF/ALL, or add summary formulas (SUM, AVG, COUNT) to the bottom of a column for instant rollups.


-


**Views:** Filter records, hide fields, and display specific slices of your data without changing the underlying table. Share views independently, so collaborators only see what's relevant to them.


## What you can do with Zapier Tables


Here are some ideas for putting Tables to work:


### Keep a running log of new issues


You want a traceable record of every issue your team opens, so nothing gets lost and you can track down what happened later.


*What this might look like:*


1.


A new issue is logged in **[Jira Software Cloud](https://zapier.com/apps/jira-software-cloud/integrations) .


2.


A row is added to a Zapier table, capturing the issue, its status, and a timestamp.


3.


[Gmail](https://zapier.com/apps/gmail/integrations) sends an email to your teammates, notifying them of the issue.


### Update the status of a job with a button click


You want one table to display the current status of every job, updated the moment someone clicks a button on the record.


*What this might look like:*


1.


A teammate clicks a button on a record in a table, kicking off the Zap.


2.


The status field is updated to the next stage.


3.


The dispatcher receives a[Slack](https://zapier.com/apps/slack/integrations) DM saying that the job advanced.


This visual diagram was created in[Zapier Canvas](https://zapier.com/blog/zapier-canvas-guide/) , our free built-in tool for mapping out your automated workflows.


### Turn a field photo into a catalogued asset


You want field teams to log hardware just by taking a picture, with no manual data entry.


*What this might look like:*


1.


A field technician uploads a photo through a form built with[Zapier Forms](https://zapier.com/apps/interfaces/integrations) .


2.


[AI by Zapier](https://zapier.com/apps/ai/integrations) reads the make, model, and serial number from the photo.


3.


[Formatter by Zapier](https://zapier.com/apps/formatter/integrations) standardizes the extracted values into clean fields.


4.


[Filter by Zapier](https://zapier.com/apps/filter/integrations) lets the Zap proceed only if a serial number is found.


5.


A record including the saved photo and other details is added to a Zapier table.


## How to get started with Zapier Tables


Let's walk through the basics of creating a Zapier table:


1.


Head to the[Tables dashboard](https://tables.zapier.com/) .


2.


If your data already exists in a CSV or an app, click **Import data** to begin the transfer. Otherwise, click **+Create** to begin from scratch.


3. From here, you can describe what you want in your table to[Zapier Copilot](https://zapier.com/blog/zapier-copilot-guide/) , our built-in AI assistant. Copilot can transform a plain-English request into a fully structured table. Or you can just build from a blank slate by clicking **+Start from scratch** .


Here's the table Copilot created for me based on my prompt.


[See a larger image.](https://cdn.zappy.app/0bfaae55f793c885ef8f219ecfc4c560.png)


4. To add a record manually, click the **plus sign** (+) under the last record. To add a field, click the **plus sign** (+) after the last field. A modal will open where you can ask Copilot to produce fields for you, add fields manually, edit the current field structure, or enrich fields with AI.


5. If you want data to automatically flow into your table, you'll need to connect it to one or more Zap workflows. Click **Automate your data** to open a sidebar and choose from multiple options for hooking up a Zap.


6. Alternatively, when you're building in the[Zap editor](https://zapier.com/editor) , you can add a Zapier Tables step to your workflow—be it a trigger or an action—and connect your table during the setup process.


That should be enough to build the bones of your first table and start feeding it data automatically. But Tables has a *lot* more depth, so for guidance on AI fields, linked records, formulas, governance controls, and more, visit our comprehensive[Tables help docs](https://help.zapier.com/hc/en-us/categories/13398036609933-Tables) .


## Set your data in motion with Zapier Tables


Your business keeps moving, and your data has to move with it. When you need that data to flow between systems and stay governed along the way, Zapier Tables gives you a reliable platform—and it gives your IT team one less system to secure.


Ready to get started? Visit the[Zapier Tables integration page](https://zapier.com/apps/zapier-tables/integrations) for inspiration, our[help docs](https://help.zapier.com/hc/en-us/categories/13398036609933-Tables) for more guidance, or jump right into the[Tables dashboard](https://tables.zapier.com/) to spin up your first table.


*This article was originally published in March 2023 with previous contributions by Krystina Martinez and Elena Alston. It was most recently updated in August 2026.*
