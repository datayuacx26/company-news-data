---
schema_version: "1.0.0"
document_id: "50f294cddcf5c9159efa7509396cbe52422d4069ddb9627fbf764cfbad060845"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/boost-dashboard-creation-with-insights-sql-assistant"
published_at: "2025-01-16T08:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:32627e4f4260802a9809891f069b92f8d285ed222626f259f4daaf3f88560494"
---

# Use AI to boost your dashboard creation with SQL Writer

If you’ve ever created your own Insights dashboards in Benchling using SQL queries, you likely know the feeling of trying to get your code just right — the right syntax, calling the right schema objects, producing the right data visualizations. Wouldn’t it be nice to have just a little help with this from time to time?


Today, we're excited to announce the newest addition to Benchling AI — the SQL Writer. This tool helps you create dashboards in Benchling Insights by generating SQL code that accesses your data in the Benchling data warehouse. Unlike other GenAI code generators, the Insights SQL assistant has direct understanding of your Benchling data model. It can make intelligent recommendations to your SQL code that help you generate and iterate Benchling dashboards faster. This feature is available to any Benchling customer that uses Insights.


This release follows the aims of Benchling AI — to unlock the potential of scientific data and improve the scientist experience. As examples,[Guided Search](https://help.benchling.com/hc/en-us/articles/25189301045773-Guided-search) employs Large Language Models (LLMs) to help scientists find data, and[Notebook Check](https://help.benchling.com/hc/en-us/articles/27659453707405-Notebook-check) helps improve entry quality and compliance.


## Why build SQL Writer?


Benchling Insights dashboards are a powerful way to visualize data. They pull structured data directly from your data warehouse and update automatically as new information is entered. Insights dashboards can be used to generate scientific insights such as dose response curves, as well as to track operational data, such as the number of plasmids created in a given month.


While many dashboards are pre-configured during implementation by our Professional Services team, you can also create your own dashboards by writing SQL queries. SQL is a common programming language for accessing information from relational databases like Benchling’s data warehouse. Using SQL allows for a high degree of precision in selecting data to access, but requires users to understand their Benchling data model, warehouse schemas, and SQL syntax.


In practice, even skilled data scientists seldom go from an idea to a completed SQL query purely in the editor. This task involves some spelunking of the Benchling data model and warehouse schemas, possibly some Googling of SQL syntax, and a dash of trial and error. With the advent and rapid improvement of LLMs, there is great opportunity to apply these technologies to support SQL development.


But using third-party tools like ChatGPT wouldn’t have native understanding of how your R&D data is structured in Benchling, and may require you to bring your data into another platform. Wouldn’t it be much easier to just have support for AI-enabled SQL editing sitting right within your Benchling environment, and have it already configured to understand your own unique data model? That's exactly what we have done with the SQL Writer.


## Putting the SQL Writer to work


The SQL Writer is embedded in the Insights SQL editor, and all you have to do to use it is write a natural language prompt using the “--!” syntax. In other words, just write a comment for what you are trying to accomplish and the SQL assistant will get to work.


We’ll walk through a few common use cases, and you can also watch a[demo video here](https://benchling-1.wistia.com/medias/flvu2rrh8j) .


## 1. Get started quickly with a new SQL query


If you're starting a new dashboard or visualization, why not let the SQL Writer help with a first draft? The SQL Writer can use its knowledge of your warehouse to help get you going, especially when you aren’t sure where to start. Just explain the details of what you're looking for using the --! syntax, and let the assistant come up with suggested SQL. In this example, we entered *--! Show the count of entries by the template they were created from* .


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## 2. Make modifications to existing SQL queries


Have a change you want to make to an existing SQL query? You can use one or more of the same comment lines (with --! syntax) to simply indicate the updates you’re looking for. In this example, we typed *--! can you update this to show these counts over time (by month)?* The SQL Writer will also highlight the differences, making it easier to review the proposed suggestions before accepting them.


## 3. Debug and fix errors


Fixing bugs and syntax errors is no one's favorite task. The SQL Writer can hone in on these and propose suggested fixes. In this example, we entered *--! can you fix the syntax error on this line* and the suggested SQL shows up on the right, highlighted in green to show the difference. The SQL Writer validates its own work, so if you get the green dot and the query looks like what you wanted, you are good to go.


## A little more on how SQL Writer works…


SQL Writer starts by analyzing your SQL and your comments and uses that as a starting point to select which of your warehouse tables will be relevant. It considers not only the schemas of your warehouse, but also understands how certain schema fields have potentially been renamed. It can map between the new schema names and old warehouse column names, helping to reduce time spent figuring out what the field you are looking for is called in the warehouse.


The SQL Writer can then sample data from relevant tables to help it author queries over complex data, notably JSON columns that can often contain arrays of data that need unnesting. After it generates a new SQL query, it verifies whether it’s valid or not. In cases where it’s not, it injects the error with the SQL to allow for quick and easy retries. In addition to providing the SQL suggestion, it also provides an explanation of the approach taken to generate the query. This can be helpful to understand and validate that it meets your expectations.


Customers can trust that their data remains safe and secure, and will not be used for any shared model training. Customers can learn more about our approach to security and privacy for LLM-powered features in this[help article](https://help.benchling.com/hc/en-us/articles/20901323667853-Security-and-Privacy-for-Benchling-Intelligence) .


## Ready to try it out?


The SQL Writer is now available to all customers as an open beta on any non-GxP tenant. All Benchling AI features are opt-in. Your Benchling admin can turn this feature on for select users or for the entire tenant following the instructions in this[help article](https://help.benchling.com/hc/en-us/articles/25260337033741-Benchling-Intelligence-overview) .


## More to come with Benchling AI


We're excited to deliver this new SQL Writer to our customers. There is much more to come for[Benchling AI](https://www.benchling.com/ai) , with new powerful ways to automate data entry and analysis on the roadmap. We’d love to hear from you on how these features are helping and any ideas for new ones in the[Benchling community](https://community.benchling.com/groups/benchling-ai-llm-betas-75) .
