---
schema_version: "1.0.0"
document_id: "59865b16642c6f16ead1c80c1ec29c813ac48f3f9d40a5f3c08060b004667a81"
company_key: "yc-retool"
company: "Retool"
source_id: "yc-retool-news-import-762698831de2"
canonical_url: "https://retool.com/blog/winter-2025-ai-build-week-insights"
published_at: "2025-11-21T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:05.217948+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:55f1dbf3cde36afc1153acf8cea8222fc79938292da4599335379470675fff2d"
---

# What we learned from Winter 2025 Build Week | Lessons in AI-assisted app development | Retool Blog

01 Getting started with AI-assisted app building in Retool02 Can you use the Assist feature on existing Retool apps?03 How much context does Assist have when building and editing apps?04 What if I don’t like what the AI builds?05 Working with data in AI-assisted development06 Can Assist connect to existing data sources in Retool?07 How does Retool handle messy or “dirty” data?08 Can I use Assist to write data back to my database?09 Can I control whether Assist can write to my database?10 Trust and control in AI-assisted development11 Can I see what queries the AI writes?12 Can I roll back to an earlier version of my app if something breaks?13 How do you fix issues when AI-assisted development makes mistakes?14 Are there limits on how much I can use Assist in Retool?15 What builders actually want from AppGen16 How can AI-assisted development help non-technical users build apps?17 How much control do builders want over AI-generated code?18 What’s the real value of natural language programming?19 What we learned


Hundreds of builders from more than 30 countries joined[our second AI Build Week](https://community.retool.com/tag/ai-build-week-2) , a four-day live series showing what’s possible with Retool’s enterprise AppGen capabilities.


From Karachi to Kansas, Sydney to Vienna, the chat was filled with questions that revealed where builders are right now: curious, cautious, and ready to experiment. It felt like everyone was trying to solve the same problem—getting the most out of[AI-assisted development](https://retool.com/ai) without losing control of their apps and data.


Presenters[Keanan Koppenhaver](https://www.linkedin.com/in/keanankoppenhaver/) ,[Angelik Laboy Torres](https://www.linkedin.com/in/angelik-laboy/) , and[Alexis Ego](https://www.linkedin.com/in/alexis-ego/) prompted live and surfaced the kinds of questions that matter when you’re actually building in Retool’s AppGen platform. Let’s run through some of the most common questions covered.


## Getting started with AI-assisted app building in Retool


The format proved something important: the best way to understand natural language programming is to **build with it,** and it becomes even easier to build alongside someone who knows what they’re doing. Here were some questions we got about the basics of Retool’s enterprise AppGen:


## Can you use the Assist feature on existing Retool apps?


Yes, you *can* edit existing Retool apps, add filters, debug queries, and explain logic in plain English without rebuilding from scratch using Assist.


In the first session on building an app with government data, builders discovered they could add dropdowns to the app Keanan built by prompting Assist directly. As he explains during the first session: “because it’s built right into Retool, it knows where your components are, what queries exist, and what it has access to.”


This means Retool doesn’t start from zero each time you prompt. It understands what you’ve already built and works with it.


## How much context does Assist have when building and editing apps?


Because Assist is native to the Retool platform, it has full context on your resources, components, and data structure. It reads your schema, understands which tables to query, and knows what’s already on the canvas. Assist knows everything about how your Retool environment is configured, which components are available, and how your data is set up.


## What if I don’t like what the AI builds?


Every action is reviewable. Retool shows each query before it runs and auto-saves checkpoints so you can revert to earlier versions anytime.


Keanan explains:


“We’re saving a checkpoint at this stage, which allows us to revert back to this version if we ever want to. This gives me a little more comfort with asking Assist to do things for me,” he says.


Retool hooks into[Source Control](https://docs.retool.com/source-control/) , making it easier to make incremental changes as you go. You can also review your version history, like Angelik does in her session on building a customer churn risk dashboard:


## Working with data in AI-assisted development


Most of the live Q&A centered around data, including CSV limits, imports, and connecting real databases. Builders wanted to know how AI-assisted development handles the messiest, most critical part of any app. Some of the questions they asked included:


## Can Assist connect to existing data sources in Retool?


Yes. The Assist feature works with[Snowflake](https://retool.com/integrations/snowflake) ,[BigQuery](https://retool.com/integrations/bigquery) ,[Google Sheets](https://retool.com/integrations/google-sheets) , and[any other data source](https://retool.com/integrations?category=database) you’ve connected to Retool. It inspects your schema and builds directly on live data.


When prompted to access a resource, Assist’s first step is the same across Snowflake or BigQuery. It looks at the data and the schema, sees what’s going on, and uses that information to determine what the app should look like.


For example, during the “From data to dashboard” session on day three, Keanan gave Assist access to a resource—the “airports” table in RetoolDB. The next day in the “Lakehouse to live app” session, Alexis connected to a Snowflake data warehouse:


## How does Retool handle messy or “dirty” data?


Many builders[deal with messy data](https://retool.com/blog/ai-ready-data) —spreadsheets with blank cells, dates formatted five different ways, duplicate customer records, or columns that need to be split or combined before they’re useful. You can prompt Assist to clean data using natural language. Use prompts like “detect missing values and flag inconsistent types” and it generates the transformation logic you need.


## Can I use Assist to write data back to my database?


Yes, once your data is in a database, you can write back to it using Assist. CSV files are static, but databases support read and write operations—including writes generated by Assist.


## Can I control whether Assist can write to my database?


Yes—by default Assist requires approval before it does any sort of query that’s not a read. You can also change this behavior to one of the following values: “ask me about every query,” “auto-approve all the queries on this particular resource,” or “auto-approve all the queries everywhere.”


## Trust and control in AI-assisted development


Some of the most insightful questions were about guardrails. Builders wanted to know they could trust what AI was doing under the hood.


## Can I see what queries the AI writes?


Yes—when you prompt Assist in Retool, it’ll show you each query before it runs so you don’t have to worry about hidden operations.


Builders saw this in real-time during Build Week sessions. As Keanan explained during the session on turning spreadsheets into AI-generated apps: “By default, in Retool, every time Assist is writing a query for you. It’s going to stop and let you inspect the query so that you can be sure that—in addition to using the permissions that are built into your Retool account—it’s not doing anything you don’t want it to do.”


## Can I roll back to an earlier version of my app if something breaks?


Yes. Retool automatically creates checkpoints that you can revert to whenever you need to.


“As a builder, I have a lot more confidence in what I’m building, and it allows me to take some more risks,” Keanan says.


## How do you fix issues when AI-assisted development makes mistakes?


Use natural language to describe the fix. Retool understands the context and can correct its own work.


When the map didn’t center correctly on the selected airport during the session on building AI-generated apps from spreadsheets, Keanan asked Assist to fix it with this prompt: “Can you please center the map around the airport by setting the latitude and longitude fields to the coordinates for the airport.”


Retool fixed the map to automatically center on each airport’s location. Keanan then manually adjusted the zoom level from 15 to 8 for better context, noting that sometimes manual isn’t a bad thing.


“You wouldn’t ask Alexa to turn on your lights for you if you’re standing right next to the light switch,” Keanan says.


## Are there limits on how much I can use Assist in Retool?


Assist usage in Retool limits[vary by plan](https://retool.com/pricing) . Free plans have credit limits. Paid plans currently have unlimited prompting for a limited time. Retool’s engineering team is actively working on more visibility into usage and costs.


As for concerns about context window limits, there’s a practical workaround. If you ever feel Assist is losing context during a complex build, you can start a new thread to refresh it.


## What builders actually want from AppGen


After four days of learning and Q&A, one truth stood out: builders want AppGen that eliminates the undifferentiated heavy lifting while keeping them in control.


## How can AI-assisted development help non-technical users build apps?


Builders want AI to eliminate undifferentiated work, not eliminate their judgment, expertise, or control over critical decisions. Non-technical users can use AppGen platforms like Retool to build apps that solve their business problems.


Builder Zabrina Hale captured this in the chat on day three: “As a non-developer new to Retool within my org, Assist is really exciting and makes it feel a lot less daunting.”


## How much control do builders want over AI-generated code?


Builders still want a lot of control over AI-generated code. Builders asked about prompt visibility, naming conventions, code indentation, aesthetic control, and whether they could define their own best practices for Assist to follow.


## What’s the real value of natural language programming?


Natural language programming eliminates the gap between knowing what you want to build and having the technical skills to build it—but only when paired with proper guardrails and transparency.


Ben Miller, Head of Solutions Engineering, EMEA, summed it up on the last day of Build Week: “Our ambition is to make creating software as easy as writing a document—but with guardrails that keep your data safe.”


Every question builders asked revealed similar concerns: speed matters, but it shouldn’t come at the cost of security, control, or visibility into what the AI is actually doing.


## What we learned


AI-assisted development works best when builders are in control of the decisions that matter. Builders already knew AI could build apps. At Build Week, they learned how to build with AI without giving up control—through query previews, version rollbacks, and permission controls they could see and adjust in real time.


If you enjoyed AI Build Week, you’ll love the forums—it’s where builders share solutions and learn together every day. You can also find all past Build Week sessions[on YouTube](https://www.youtube.com/playlist?list=PLqWdQFDVLADn9Nsqb3Eoq5sxa2JLjJiEV) .


light Reader
