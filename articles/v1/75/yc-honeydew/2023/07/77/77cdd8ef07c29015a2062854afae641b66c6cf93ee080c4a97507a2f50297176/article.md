---
schema_version: "1.0.0"
document_id: "77cdd8ef07c29015a2062854afae641b66c6cf93ee080c4a97507a2f50297176"
company_key: "yc-honeydew"
company: "Honeydew"
source_id: "yc-honeydew-rss-6ec5327dfb7a"
canonical_url: "https://honeydew.ai/blog/so-lets-talk-about-semantic-layers/"
published_at: "2023-07-04T10:09:03+00:00"
first_seen_at: "2026-07-26T14:25:40.247020+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:976e1008c7dfe8a82f2b3d20bdc91353d811dcbca0f6d5cc22d9341ff98a3660"
---

# Semantic layers are awesome

When is the last time you thought about water treatment plants?


Domestic wastewater treatment is an essential part of[modern city construction](https://www.ign.com/wikis/simcity/Sewage_Treatment_Plant) in a world of a rising population. But it can be so[boring](https://www.netsolwater.com/latest-innovations-in-waste-water-recycling-systems.php?blog=1324) .


*So let’s talk about semantic layers.*


## Your First Semantic Layer


When the company was small, you had this database. A few dashboards were attached on top, dutifully showing some KPIs: how many leads do we have?[are they good](https://www.forentrepreneurs.com/saas-metrics-2/) ?[do they like us](https://www.netpromoter.com/know/) ?


But the company grew, and with it, the databases, the dashboards, and the KPIs. Data is now so *messy.* Numbers misalign, answers are irrelevant, and the best source of truth is metrics_final3.xlsx . Everything is scattered, and everything is a Project.


So the company hires Eddie. Eddie has a beard.


Eddie’s task is to get the numbers to align. He hates disparate logic, so he builds a *script* . The script has many stages of SQL, and it generates Perfectly Fine Tables. The script knows how to join the data sources, what to filter out, and how to compute all the messy KPIs. There is no more chaos.


Your first semantic layer is not that script.


Your first semantic layer is Eddie.


### The Role of the Semantic Layer


A semantic layer bridges the gap between business and data. People who want to use data tend to ask questions like “why don’t we grow in Australia”, while data people` SELECT WHERE (country=61) AND (is_bot_score<0.1)` . If there is no way to jump across, the answer is 🤷.


A semantic layer allows everyone to use data. To do it, it must:


- Keep data consistent, so today’s 10 active users are still counted as 10 tomorrow.
- Keep data understandable, so when we say “active user”, we all know what it means.
- Keep data flexible, so when you want to count active users just in Australia, you can.


Eddie’s script is very consistent. But when you want to understand what it does, or when you want it to do something new, you ask Eddie and hope he writes in his script what you meant, and hope he likes you enough to do it soon.


The company’s ability to[use data](https://en.wikipedia.org/wiki/Data-driven) well to advance the business rests on Eddie’s shoulders.


Alas, as the stress mounts and the chaos keeps returning, Eddie had had enough. He leaves to pursue his childhood dream of growing apples in the[desert](https://www.gardeninginthedesert.com/how-to-grow-apple-trees-in-the-desert/) . Now, not only there is no semantic layer, there is this *script* full of dead code.


## Your Second Semantic Layer


Enter: Dana, a[Data Person](https://erikbern.com/2021/07/07/the-data-team-a-short-story.html) . She brings a Snowflake, or a BigQuery, or a Redshift to get all the data in one place. She builds a coherent team of people who know SQL. She puts all that scattered logic in one place. It takes Dana a year, but she disentangles Eddie’s code, and the Excels and ad-hoc dashboards built on top of it while waiting.


She structures a team to serve diverse data demands: data engineers to maintain the data pipelines, and data analysts with domain expertise serving business domains. She creates mentorship and collaboration between the many people who do data in Marketing, Product, Finance.


Dana does not bring order to the chaos. Instead, she creates a Process.


And thus, Dana isn’t your semantic layer, nor any of her analysts or engineers, or tools. The process is.


A combination of a[relay race](https://www.youtube.com/watch?v=d2RVs3aoXJA&ab_channel=OlympicsAquatics) and a game of[Telephone](https://en.wikipedia.org/wiki/Chinese_whispers) , it scatters the meaning and context of data across the team, pushing everyone to answer *fast* while penalizing any *error* .


- For consistency, it goes through a data engineer to implement an ETL pipeline.
- For understandability, it goes through to a Wiki, or a catalog, or a Slack.
- For flexibility, it goes through a data analyst, or a BI developer building a self-serve tool.


Maintaining the process - correcting the errors, aligning the players, removing duplication, becomes the main drag of[using data well](https://hbr.org/2020/02/10-steps-to-creating-a-data-driven-culture) for the business.


### The Expanded Role of the Semantic Layer


The semantic Layer allows many people to use data in a way that helps them achieve their goals.


Few have the time or inclination to understand every step of the data flow process and its complexities. Fewer will even try. One curse of the “[modern data stack](https://www.moderndatastack.xyz/) ” is that while it allows for a much richer data pipeline, its richness makes it harder for most data users to leverage data effectively.


Users either trust the process, can participate in it and wait for its roundtrip - or won’t trust and use the data.


When users are tired of waiting, they start hiring Eddies to their own domain, and build their own pockets of impenetrable mess, in an ever-growing[data mesh](https://www.datamesh-architecture.com/) of entropy.


As an organization expands, in addition to consistency, understandability, and flexibility, a fourth component becomes essential: *Curation* . Help the users work with a trustable subset where there is consistency and flexibility - while abstracting the hard parts.


## Your Last Semantic Layer


Like water treatment, semantic layers are a boring piece of technology. But even more so than the effect of water treatment on urban quality of life - semantic layers have the most profound effect on how well a business uses data.


A semantic layer is not “a single source of truth” (though it can be one!), or a “data mesh” (though it can facilitate one!) nor is it the solution for every data problem.


A semantic layer is the glue that holds the data together. When it is good, using data is a natural part of doing business.


When it is absent, dissipated in Telephone-game like process, or silo-ed in fortified pockets of non-reusable logic, data is hard and frustrating. Its anecdotal real successes glimpse a potential far from being fully realized.


At Honeydew, we change this. We make data integral to the daily operation of every business.


We create a semantic layer that frees data practitioners to focus on creative data work and not on the process. That unifies the meaning of data without creating another silo. That helps curate data for everyday business users. The helps domain expert data analysts to do a deep dive. That is based on a transparent collaboration of data analysts, data engineers, and data consumers.


*More details soon!*


## Update May 2024


Exactly 18 months passed since publishing this blog. Now this is not only a dream but a reality - Honeydew is the first Semantic Layer native to Snowflake. You can use it[today](https://honeydew.ai/get-started) .


Over the last months, we have learned that this short post captured a transition that thousands of companies go through: from *people* to *process* to a *platform* .


Almost every aspect of enterprise goes through that kind a transition. We are proud we can enable it in one of the most crucial aspects of business: the data foundation.


Now that analysts and data scientists are joined by LLMs such as ChatGPT leveraging that same data foundation, this is more important than ever.
