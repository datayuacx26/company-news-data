---
schema_version: "1.0.0"
document_id: "11406d43e020ec6ae523709bdc57f566426c95544e9a5227f8d82a65acc60ed3"
company_key: "yc-mozart-data"
company: "Mozart Data"
source_id: "yc-mozart-data-rss-80d5f072b9ee"
canonical_url: "https://mozartdata.com/everyone-uses-data/"
published_at: "2024-01-24T23:15:50+00:00"
first_seen_at: "2026-07-24T11:43:21.667443+00:00"
fetched_at: "2026-08-20T02:02:30.556401+00:00"
content_hash: "sha256:a11907baccfcfc367f1439508e1427f1f7e7af531b94f1f9c51ca998c1c7ad97"
---

# Everyone Uses Data

*This post was written by Shai Weener on Mozart’s data analyst team.*


I was on a hike through the Marin Headlands with my uncle when he asked me what my company does. We had spent a lot of time in the past talking about what my day to day looks like, what it’s like working at a tech start up compared to the non-profit space, and what I think about WFH versus RTO (return to office). But then he asked me to explain, explicitly, what my company does. Maybe I shouldn’t admit this to my boss (sorry Fish), but my first reaction was “eh, I don’t really need to go into it. We basically just help people get access to their data and use it without having to hire a whole data team.” To which my uncle responded: “Ok, but what does that mean? Explain it to me.” Which I didn’t really see a point in doing.


My uncle is a partner at an architecture firm that builds libraries (and other buildings) around the country. His firm is small, focusing on a handful of big projects at a time, and from my guess, doesn’t spend a lot of time or energy thinking about advertising costs, SEO, platform usage, etc. etc. In my mind, I didn’t think it was worthwhile to go down into the rabbit hole of the modern data stack. Not every service is meant for every company, so why dive into the deep end for someone whose company doesn’t have a use case for it? Well, spoiler alert: it probably isn’t that simple.


See, a few months later, I’m working with a new customer at Mozart. Her use case is that, every quarter, she downloads the newest versions of Excel sheets directly from her source. She then undergoes the same cleaning process every time and replicates the same simple reports she’s used in prior quarters – including some descriptive counts, some demographic percents, etc. She approached Mozart not because she wanted the cheapest ETL solution and the ability to build out and schedule an extensive data pipeline. She wasn’t pursuing a data warehouse, an extraction tool, a transformation layer, or an orchestration tool. She came to us simply because she knew there had to be a better way than to manually replicate the same simple processes over and over again. Though she didn’t *need* a modern data stack, the solution to her problem *is* the modern data stack. The modern data stack doesn’t need to be utilizing an extraction tool to connect to APIs for many different data sources, putting them in a centralized warehouse, and reverse-ETL-ing data back. It can be as simple as a person with a bunch of Excel sheets saving themselves time by storing these sheets in one place and streamlining their cleaning process using our SQL data pipeline tool.


Using Mozart (with under the hood support from Fivetran and Snowflake), we were able to set her up with a straight forward solution to her problem:


Create a Google Drive that is connected to the data warehouse. Utilize some basic SQL functions (such as TRIM and LOWER) to automate the cleaning portion. Push the cleaned data out into whatever BI tool they choose.


tl;dr: After the initial set up, all she has to do every quarter is drag and drop the new Excel spreadsheets into a Google Drive and everything else is done automatically.


It was at this moment that I recognized how presumptive it was to think that the work I do isn’t relevant to my uncle. Ok, he may not need to utilize the full power of a data lake, but anyone who uses Excel sheets and manually replicates the same calculation process multiple times can probably benefit from the modern data stack. At some point, almost every company of any size hits a threshold where they start thinking about data. It may not be data engineering or data pipelines explicitly, maybe yet or maybe ever, but the modern data stack doesn’t have to just be for SaaS companies bringing in millions of data points a day. It can be for a non-profit trying to simplify their attendance reporting from Salesforce, or a boutique architecture firm hoping to have real time revenue predictions rather than manually calculating them at the end of every month. It could be for a transportation company who wants to be able to store all of their historical data in one place to make year-over-year tracking simpler. It could also be for a veterinary hospital who would rather spend their time furthering their business than spend valuable time each month transferring Excel formulas that they *could* do manually, but would prefer not to.


So if you’re thinking about how to start using data in your company, or you’re wondering how you can save yourself some time by streamlining some of the more manual and repetitive components of your workflow,[reach out to Mozart Data](https://try.mozartdata.com/contact-us) . Let us show you how the modern data stack doesn’t need to be as scary and complex as it appears.
