---
schema_version: "1.0.0"
document_id: "7f1d962c08877faccfadbe464b304db89dbbec10451efbf5cb418b55129ab1c7"
company_key: "yc-honeydew"
company: "Honeydew"
source_id: "yc-honeydew-rss-6ec5327dfb7a"
canonical_url: "https://honeydew.ai/blog/excel-is-the-best-bi-tool-and-its-not-going-anywhere-just-ask-your-cfo/"
published_at: "2024-10-10T08:47:30+00:00"
first_seen_at: "2026-07-20T23:20:19.939873+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:b69ecd7456cadcb3b607dac1f30ce2a6ce90ca2d8480bea240f931af30300dba"
---

# Excel is the Best BI Tool, and It’s Not Going Anywhere – Just Ask Your CFO

Over my career as a Director of Data and BI team lead, writing blog posts has never been in my comfort zone. I thought I’d give it a try because what I’m about to share is something everyone in the data space needs to hear.


Just like I’m not fully comfortable writing blog posts, many end-users or clients in data departments aren’t thrilled about adapting to modern BI tools like Looker or Power BI. For many, their old and trusted companion — Excel — remains their go-to tool.


I’ve witnessed firsthand the struggles analysts face when delivering insights to end-users who prefer Excel over more dynamic BI tools. This got me thinking — writing this blog post isn’t all that different from how data teams approach legacy tools.


### The Struggle is Real: End-Users vs. New Tools


For me, when I write, I go to ChatGPT. Likewise, I see data end-users gravitate toward their tried-and-true too - Excel. And just like me, they run into obstacles.


Imagine a CFO and his team using Microsoft Excel. As demand for data grows, its limitations become painfully clear:


- **Disconnected Data** : Excel data exists in the Excel file. It only changes when someone updates the file, for example by copying a data extract from an external system like a CRM.
- **Opaque Sources:** Where did that number come from? No one knows. Once data is copied or entered into an Excel sheet, its source is lost.
- **No Workflow** : Collaboration and data conflict resolution between Excel files is virtually impossible. Data decisions, like how to filter for a specific customer segment, can’t be deployed easily into Excel sheets, and no source of truth is possible.


The CFO doesn’t care about these limitations. The source data is in Snowflake but training the team on a “better data system” using it, whatever that may be, is not something he seeks. All he sees is an Excel report that isn’t right.


### Meeting Demands with Power BI?


When the CFO faces this issue, he quickly sends an urgent, disappointed email to the analyst, expressing frustration about not having immediate access to the data. Sound familiar? If you ask around my office, you’d hear the same about me and my complaints about writing this blog post!


The analyst, responding swiftly, loads his data from Snowflake to a report and sends the CFO a Power BI dashboard that was already built. Problem solved, right? Well, not quite.


The CFO, after a glance, still has follow-up questions. He tries to modify the Power BI dashboard but after 15 minutes, gives up and returns to the analyst, saying, “I need more flexibility! I don’t know how to use this tool!”


Now, the frustrated analyst has to sit with the CFO for half an hour, understand his specific requirements, then head back to his desk to create a pivot table in Excel. It’s as if I, after struggling to write this post, had to return to ChatGPT for rewrites.


### Just Connect it to the Cloud?


Throwing hands in air, the analyst builds up a new solution. Connect the team’s Excel to a cloud data warehouse. Let the CFO team build Pivot Tables in the tool they love, and refresh the data when they need it.


The next day, the CFO tries to refresh his Excel report to get updated data. In theory, it should work. But the CFO, being an early bird, tries to refresh it during a data extraction process. Naturally, the refresh fails.


A call to the analyst on his way to the office results in the dreaded response: “You’ll need to wait 45 minutes for the extraction to finish.”


Disappointed, the CFO makes his second cup of coffee and expresses his frustration to the head of the data team, who patiently listens to these all-too-familiar complaints. By now, the extraction is complete, and the Excel refresh works.


However, during a board meeting later that day, the CFO presented his Excel report, only to find that the numbers didn’t align with the CMO’s data from the Power BI dashboard. Frustration all around.


### The Real Issue: It’s Not the Tools, It’s the Workflow


When the numbers presented in Excel don’t match those in Power BI, the head of data realizes: it’s not the tools causing the problem—it’s how they’re being used.


But can we really teach the CFO to use Power BI? And is it worth the time and effort?


This is where the analogy comes full circle. Just as I struggled to write this blog and had to turn to ChatGPT for help — only to realize I needed my personal touch to write something I would also like to read — data teams face the same challenges with legacy tools like Excel. The tools we’re familiar with may not always fit our current needs, but instead of discarding them, we need to find ways to integrate them more effectively into modern workflows.


### The Solution: Honeydew


Enter[Honeydew](https://honeydew.ai/) . Honeydew bridges the gap between old and new by integrating Excel seamlessly with Snowflake. Honeydew’s API allows live queries in Excel, translating them into SQL based on a semantic model that ensures consistency across all reports. Here’s what it offers:


- **No More Downtime:** The CFO doesn’t have to wait for data extraction, as live queries pull real-time data directly from Snowflake.
- **Consistency Across Tools:** Whether it’s Excel or Power BI, the numbers are always aligned, thanks to the centralized semantic model.
- **Shorter Path to Insights:** Analysts can deliver flexible, reliable reports faster, eliminating unnecessary back-and-forths.


Just like how I had to sit down and put my own thoughts into this blog post, analysts need tools like Honeydew to make data flow smoothly in Excel — without reinventing the wheel.
