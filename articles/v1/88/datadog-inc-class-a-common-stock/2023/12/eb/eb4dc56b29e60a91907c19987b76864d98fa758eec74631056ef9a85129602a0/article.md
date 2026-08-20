---
schema_version: "1.0.0"
document_id: "eb4dc56b29e60a91907c19987b76864d98fa758eec74631056ef9a85129602a0"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/crunchconf-talk-self-serve-analytics/"
published_at: "2023-12-21T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:630aeb0305275a116ebd6f7de285d391ef6fa18888d325e0bcec692217111b51"
---

# Scaling self-serve analytics: The tools empowering 5,000 employees

Austin Lai


Jean-Mathieu Saponaro


In October at the Crunch Conference in Budapest,[Jean-Mathieu Saponaro](https://twitter.com/jmsaponaro) , Data & Analytics Senior Engineering Manager at Datadog, delivered a presentation about how he and his teams scaled self-serve analytics within Datadog up to the 5,000 employees we have today.


Self-serve analytics is the dream of any Data & Analytics team: being able to reach that point where everyone in your company is leveraging data to answer day-to-day questions and make decisions without requiring your team’s help. But how do you get and stay close to this ideal state as your company grows with an expanding variety of data expertise and data needs?


In the[video](https://www.youtube.com/watch?v=1r2NQQzDGn0) below, Jean-Mathieu Saponaro explains how the Data & Analytics department at Datadog built, on top of open-source technologies, self-serve analytics tools empowering all teams to make data-informed decisions, as we grew from 200 employees and 1 product to 5,000 global employees and dozens of products in just a few years. He dives into the entire suite of tools: data intake, transformation, data quality, data discovery, and reporting.


## Video


Jean-Mathieu is passionate about enabling people to make data-driven decisions. He joined Datadog as the first Data Analyst in 2015, became Data Engineer in 2016, started leading a team of Data Engineers & Analysts in 2018, to finally support today four teams and 25 Analytics/Data/Infrastructure/Frontend engineers and their managers. Not only has Jean-Mathieu been at Datadog for more than eight years, but also he has helped pioneer a data-driven culture within the company and build a suite of tools on top of open-source software that scaled self-serve analytics for all employees.


In his talk, Jean-Mathieu describes how his teams offer to all Datadog teams a single source of truth for all internal data, intuitive tools to leverage it, and data knowledge and literacy thanks to support and training. He also acknowledges limitations of self-serve analytics and how to mitigate them before explaining how the Data & Analytics department tracks the success of this self-serve analytics strategy.


## What is self-serve analytics?


Jean-Mathieu introduces the mission of self-serve analytics teams at Datadog: “Empower everyone at Datadog to make data-informed decisions on their own”. This means other employees can leverage data without any help, and most importantly, without the help of a centralized team.


As a result, Data & Analytics teams can **focus on high-value initiatives** rather than answering every single request or question, don’t necessarily have to **grow as fast** as the rest of the company, and play a key role in ensuring a true **data-driven culture** within Datadog.


Self-serve analytics is enabled by three pillars:


1.


Data


2.


Tools


3.


Knowledge


Before talking about what these pillars look like in practice, it’s important to understand who your audience is within your company as different profiles will have different self-serve analytics needs.


At Datadog, self-serve analytics teams have identified three main categories of internal users:


-


***Analytics Explorers*** who mostly need clear, intuitive, easy-to-discover data and pre-built reports.


-


***Analytics Builders*** who also need to build their own reports for their team or run advanced queries to answer more complex questions.


-


***Analytics Experts*** who also may want to expose new data, maintain its business logic, and control its quality.


## Data as a product: the “single source of truth”


It all starts with the data. It is the foundation of any analytical work. Self-serve analytics teams provide a single central source of truth for all internal data: product data, operations data, and business data.


Data & Analytics teams at Datadog also developed a tool called “ *Bring Your Own Data* ” (BYOD) which enables any team producing data to expose it for analytics.


Under the hood, here’s the architecture of the data intake behind this single source of truth:


A diagram of our data intake platform. A diagram of our data intake platform.


The word ***single*** in “single source of truth” means that all data consumers can look at the same state of the data world. The Data & Analytics org works with literally all other departments at Datadog: Engineering, Product, Marketing, Sales, Customer Success, Support, Finance, Recruiting, HR, and more. It also means that all data consumer applications point to the same data: BI tool, notebooks, Data Discovery, programmatic access, ML Models, and more.


Finally, the word ***truth*** in “single source of truth” means that the data must be reliable and trusted by its users. This is achieved with:


-


Strong **conventions** making the the data easy to understand


-


Comprehensive **documentation** answering common questions about the data and how to use it


-


**Data Quality Monitoring**


## Tools: The Self-Serve Analytics Stack


Here is an overview of the self-serve tools the Data & Analytics org offers to Datadog teams in order to enable them to interact with and get insights from data on their own:


### Self-serve Data Intake


We provide:


-


**Integrations** : connectors with all data sources (internal data stores and third party tools), *Bring Your Own Data*


-


**Scheduling**


-


**A user interface** to let teams easily expose or request new data


-


**Observability** on pipeline runs, data quality, and actionable alerting when something goes wrong


### Self-serve Transformation


Data Analysts across the company can:


-


**Control the business logic** of their department’s data with SQL on dbt with an intuitive development environment fully integrated with our ecosystem (workflow manager, metadata store, and pipeline runs platform).


-


**Work with strong, enforced conventions** to ensure that our data modeling layer, which is now open to all Data Analysts across several departments, remains clean, consistent, and intuitive.


-


**Have observability** at all levels: data lineage, pipeline runs, data quality monitoring, alerting, and more.


### Data Discovery


Any Datadog employee can:


-


**Browse** all the datasets and fields available in our single source of truth.


-


**Search** across all datasets, fields, and their metadata, and **find** which data will answer their question.


-


**Understand and trust** the data thanks to comprehensive metadata giving them all the context they need: where it comes from, who owns it, what does it mean, who uses it and where, how sensitive it is, how reliable it is, and more.


Our Data Discovery tool is of course connected with our entire ecosystem (metadata store, BI tool, Notebooks, ML Models, and more).


### Self-serve Reporting


Here is what we’re looking in a BI tool to fit our self-serve strategy:


-


**An intuitive interface** so any employee, data savvy or not, can easily explore data with point-and-click or SQL. It addresses very well 90% of data exploration and reporting use cases, while we find other ways to solve the most advanced ones.


-


**Customizability** : by choosing an open-source tool, we enabled ourselves to tweak its frontend and develop new features tailored to internal Datadog teams’ needs.


-


**Automation** : thanks to a powerful API (each feature has a corresponding API endpoint), we are able to automate manual processes such as user permissions or content management (e.g. automatically archiving unused content).


Process Open Source Homemade Technology


Data Intake Yes Yes Airbyte, Spark, *Bring Your Own Data*


Data Transformation Yes No dbt, Spark


Data Quality Yes Yes Deequ


Data Discovery No Yes Internal app


Analysis & Reporting Yes No Metabase, Jupyter Notebooks


## Knowledge: enabling users to use our data and tools


“ *Give a man a fish and you feed him for a day. Teach a man to fish and you feed him for a lifetime* .” You could say here “Give a man **data** and you **help** him for a day. Teach a man to **use data** and you **make him data-driven** for a lifetime.”


### Education / User Training


The Analytics Academy logo. The Analytics Academy logo.


Our Data & Analytics team offers a series of classes, called Analytics Academy, that any Datadog employee can take in order to have a bigger impact in their role and within their team thanks to data and analytics.


The Analytics Academy curriculum is tailored to different user personas and broken down by three levels:


1.


Analytics Explorer


2.


Analytics Builder


3.


Analytics Experts


As you’ve probably noticed, these actually correspond to our user categories which we talk about at the beginning of this post.


The Data & Analytics team tries to find the right mix of engaging live classes (usually focused on a specific data domain or department) and self-serve online courses that employees can take when they want and at their own pace.


### User support


There will always be a need to support people’s questions.


The Data & Analytics team offers:


-


Live support on Slack


-


A comprehensive internal knowledge base for data documentation and tool guides: Data Catalog, Confluence, StackOverflow For Teams


-


Of course we are looking into how we could leverage LLMs to better support our internal users and route them to the right resource or assist them in their data analysis without necessarily having our engineers involved


### Experts


As Datadog grew from a few dozens to more than 5,000 employees across the world, we went from a fully centralized team supporting all analytics needs (data intake, data modeling, reporting, analysis), to a semi-decentralized structure.


Over the past couple of years, we started collaborating with most Datadog departments on helping them hire their own ***Specialized Data Analysts*** who report to their own org. Our Data & Analytics teams support this “Data Analyst Community” around recruiting, onboarding, mentoring, assisting on projects, providing tooling, knowledge sharing, and giving regular feedback.


Such a data mesh organization isn’t easy to put in place and takes time. A simpler community to establish at any scale are “champions” within each department. At Datadog, ***Analytics Ambassadors*** are volunteers across our departments who help their teams become more data-driven thanks to analytics. They help their teammates leverage data to answer questions and make decisions. They work with the Data & Analytics department (or the *Specialized Data Analysts* in their department) to communicate their teams’ analytics needs and teach their teammates how to use analytics data and tooling. You would be surprised to see how many people in your company would be interested in being the *Analytics Ambassador* for their team! Not only does it make them more impactful in their role, but also it is a great skill set to add for their career.


## The limits of self-serve analytics


There are some limitations to the self-serve analytics model, primarily around what we call “Advanced Analytics” which, at our scale, still requires dedicated central teams partnering with other departments in the field to do advanced analysis and operationalize our data.


Here are the two areas that the Data & Analytics org focuses on around Advanced Analytics:


-


**Research** : We gather, aggregate, enrich, analyze, and share the data that is used in Datadog research publications about industry trends such as[10 Insights On Real-World Container Use](https://www.datadoghq.com/container-report/) and[The State of Serverless](https://www.datadoghq.com/resources/state-of-serverless-2023/) .


-


**Automated Insights** : We help teams around Datadog derive high-quality insights from data, take action upon it, make smarter decisions, and grow the business through prescriptive and predictive analytics powered by data science and automation.


| Department | Use case examples |


|---|---|


| Marketing | Customer segmentation, ads/email targeting, lead scoring |


| Growth | Automated alerts on customer cross-sell/upsell opportunities or risk of churn |


| Support | ML models for tickets auto-tagging and routing, sentiment analysis |


## Definition of Success


We wouldn’t be a good Data & Analytics team if we weren’t tracking our own KPIs. We realized that it was impossible to summarize all our work into one unique metric (e.g. “time-to-insight”) to measure the success of our self-serve analytics strategy. It’s more representative and actionable to look at each tool in our self-serve offering and think about what we’re trying to optimize with it for our users.


Process Examples of KPIs


Data Intake Time to ingest new data


Data Transformation Data Analysts’ engagement and development efficiency (e.g. time to expose a new data model, field, or business logic)


Data Quality Data quality metrics SLOs


Data Discovery Time or number of steps to find the right data


Analysis & Reporting User engagement, number of reports built


These metrics should be tracked by type of users such as *Explorers* , *Builders* , *Experts* , *Data Analysts* , or *Analytics Ambassadors* .


Most importantly, they should be completed with unmeasurable signals, the ones you get from user questions, requests, discussions, and user surveys. Usually, the most insightful qualitative feedback will come from self-serve analytics power users but also detractors, people who struggle or balk at using your tools.


## Conclusion


Any digital company–whether a small startup, a mid-size org, or a large enterprise–can take the learnings from the experience of Jean-Mathieu and his teams in implementing a robust self-serve analytics offering, helping Datadog be data-driven, and growing out over the years from the traditional model of a centralized team handling all requests.


Scaling self-serve analytics as your company grows can be done by understanding who are your internal users, implementing a clean single source of truth for data, user-friendly tools to interact with it, spreading data literacy with the help of data experts, and fostering a culture of collaboration.


Even with the best data model and the most intuitive tools, there will be times when self-serve is not enough, and you will have to find ways to address these advanced analytics use cases. Lastly, be data-driven yourself and make sure to track the right success metrics without forgetting about the unmeasurable signals.


We hope this talk and article were useful, and special thanks to CraftHub Events for putting on a great conference.


-
-
-
