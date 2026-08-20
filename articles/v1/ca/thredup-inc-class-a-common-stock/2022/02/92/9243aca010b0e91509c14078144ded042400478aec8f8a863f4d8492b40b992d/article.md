---
schema_version: "1.0.0"
document_id: "9243aca010b0e91509c14078144ded042400478aec8f8a863f4d8492b40b992d"
company_key: "thredup-inc-class-a-common-stock"
company: "ThredUp Inc."
source_id: "thredup-inc-class-a-common-stock-rss-cddb21541ec6"
canonical_url: "https://medium.com/connect-the-dots/from-data-analyst-to-ml-engineer-how-to-choose-a-position-in-data-science-f2c92d91ef60"
published_at: "2022-02-12T23:23:18+00:00"
first_seen_at: "2026-07-20T23:18:30.437987+00:00"
fetched_at: "2026-08-20T00:32:43.569280+00:00"
content_hash: "sha256:0f17c8773d7586ae482a4465c3c1b07b5a0d682a4c08ef22ded6dc4b5359dc37"
---

# From Data Analyst to ML Engineer: How to Choose a Position in Data Science

*By*[Tetiana Torovets](https://www.linkedin.com/in/tetiana-torovets-2b0ab44/) *, Staff Data Scientist*


I have been working in data science for over 9 years, have experience as a data science team lead, and have conducted hundreds of data scientist interviews for various companies. Today I am a staff data scientist at thredUP, which employs people of many specializations in the world of data science: data analysts, data scientists, and machine learning engineers. I have previously practiced data science consulting as well.


This article could be useful for anyone interested in a data science/data analyst/machine learning engineer role, or those who want to understand better how thredUP organizes its team.


### Specializations in Data Science


Differences between data science specializations can be unclear for many people interested in starting a career in the data domain. One term I will use to help understand the differences is *hard skills* — technical skills, advanced knowledge that comes from education, experience, highly in-depth understanding of niche topics. Programming or data analysis skills are good examples of hard skills. According to my observations, hard skills are the primary differentiator between titles. Other differentiators are involvement in business context and active communication with business stakeholders and product managers. At thredUP, communication and an active understanding of the business is expected of all engineers: their career growth is generally impossible without it. However, success in an engineer’s primary role does not always hinge on their ability to understand business context.


*Changing expectations regarding hard skills and involvement in business context depending on specialization within data science*


There are three general specializations within data science at this time. Let’s review typical requirements and tasks for the different roles at thredUP.


### Data Analyst


- **Hard skills** . BI tools for data analysis (Looker,) are often used. Use R or Python to create scripts for data analysis. Most analysts know SQL well. The ability to work with big data and coding is always a plus, but it is usually not the main requirement. Knowledge of statistics.
- **Business context** . Strong communication skills, data visualization and presentations. Spreadsheets and presentations gurus. Perfectly understand the business context, have a lot of communication with business stakeholders (executive/finance/marketing/sales) and product managers. Not only find insights in the data, but also plan further actions based on data findings.
- **Typical tasks** . All types of EDA (exploratory data analysis), A/B test configuration and basic analysis, user segmentation.
- **Responsibilities at thredUP** . Our data analysts are the best friends of product managers. They focus on all the details of the product, help develop new features and analyse their effectiveness. All the analysts become Looker virtuosos very fast and use it to build reports and dashboards.


### Data Scientist


- **Hard skills** . Strong statistical and mathematical skills. Work with all basic types of models (classification, regression, unsupervised learning, time series, etc.), but don’t necessarily specialize in one single model type. Write code, usually in Python, to run analysis, create complex models and production pipelines . Work with big data (mostly using Spark). Actively engaged in feature mining. They are able to deploy their code at least at the basic level, and create machine learning pipelines.
- **Business context** . Strong presentation and communication skills. Often engaged in data visualisation. Compared to data analysts, they deal with data that is an order of magnitude more complex and requires a set of tools that can handle that complexity. Work a lot with business stakeholders and product managers to collect requirements for new projects and create predictive models, focusing on problems that require iteration and mastery to solve long-term projects.
- **Typical tasks** . Development of predictive models and decision automation algorithms. Exploratory data analysis with focus on big data, hypothesis formulation and planning experiments for their validation.
- **Responsibilities at thredUP** . Work with data engineers, create prototypes of data transformation, which data engineers scale and automate. Data scientists develop and maintain basic models that are used to optimize marketing costs (for example, user conversion prediction) or optimal pricing (product price optimisation algorithm). Create automated dashboards and data analysis in spark.


### Machine Learning Engineer


People with a machine learning engineer specialization at thredUP have slightly different skills and tasks depending on the domain. Here we focus on the general specialization.


- **Hard skills** . Strong software skills (Python, Scala, Java). Ability to work with streaming data and big data (in Spark). Know how to deploy machine learning models of any complexity and integrate them with other systems. Have good expertise in machine learning and know how to create advanced algorithms and develop predictive models.
- **Business context.** Work with stakeholders who tend to be less customer facing, or tend to work closely with other technical team members. Create automated dashboards to solve complex reporting cases or monitor model performance on production. Less focused than the other specialties on data visualization and presentations.
- **Typical tasks.** Optimize the performance of machine learning solutions. Can work with complex algorithms for search optimisation, responsible for both production ML pipelines and complex data transformation pipelines.
- **Responsibilities at thredUP** . Implementation of algorithms developed together with data scientists in the form of microservices (for example, fully integrated a new pricing algorithm in production). Work on technical solutions of complex problems such as search algorithms optimisation or product ranking.


thredUP has *cultural values* ¹ important for all employees in all roles. At thredUP we value everyone’s opinion and employees are encouraged to share their thoughts (speakUP). Everyone should be thinking about designing for the future (Think Big). People are encouraged to do their best and achieve superior performance at every project (Influence Outcomes).


The division of data science teams into specializations is a fairly natural phenomenon, analogous to the divisions into specializations on other technical teams like software engineering. In many cases, it’s convenient to focus data analyst purely on analysis, data scientist on creating algorithms and solutions, and giving machine learning engineers the solution to deployment and scaling problems.


However, for small companies (especially for startups) with limited resources it is more convenient to hire one person who can implement end-to-end solutions, starting from data analysis (in some cases to create ETL) and ending with the deployment of the model to production. In this case it is worth considering hiring a full stack data scientist. Full stack data scientist is another popular specialization in data science providing an interesting opportunity to own a project from end to end.


### Why a Data Engineer != Data Scientist


For people looking for the first position in the data world it is worth being careful with job descriptions. Definitions for various roles are not clear across companies and startups and small companies may have ambiguous expectations about roles. It sounds like a data scientist may solve data problems and a company may request to hire a data scientist. But data scientists can’t do anything if data is not collected yet or data quality is terrible. In that case the team needs a data engineer first. As a data scientist, it is worth clarifying what data already exists at the company and how data is managed before accepting an offer.


### The fundamental difference between expertise


There is no simple way to make a distinction between data science and data engineering. I will use one way to draw the line between the roles to build basic understanding. Typically data engineers create the data and data scientists use it. In reality, data scientists sometimes create the data and data engineers with analytical skills are downstream consumers. As the world of data professionals becomes more complex, it is likely that differences between data engineers and data scientists will become more notable.


If we imagine a typical data flow as a sequence of collecting, storing, transforming, analyzing data and building forecasts based on data, then the conditional boundary between data science and data engineering can be drawn after the stage of data transformation.


Again, in reality the distinction may not be as simple as on the schema above, but this is how it *frequently* works at thredUP.


### Data Engineer


People with a data engineer specialization at thredUP have different skills and tasks as compared to data scientists’ skills and tasks described in the previous section. We will see that none of the descriptors of data engineers are core skills for data scientists.


- **Hard skills** . Strong software engineering skills. Design performant robust pipelines. Knowledge of data storage configuration and management: work with SQL and NoSQL-databases, setup Lakehouse , storage optimization and data access.
- **Business context.** Work with a wide variety of stakeholders who are application engineers, data scientists, platform engineers or analysts.
- **Typical tasks.** Understand and profile the upstream data sources and model data so that it’s easy to understand and access. Data transformation: it could be a pipeline for event data on the site, where the result for the end user is a table in the database, or the transformation which requires the organisation of data access in the business intelligence platform (Looker). Data governance. All ETLs need monitoring, support and validation. Build Rest APIs to expose data to applications. Setup exchange of data between both internal and external partners.
- **Responsibilities at thredUP** . Organisation of data collection (especially big data and real-time data). Making data accessible for business users and other teams (marketing, data science, promo, operations etc.), Sharing data with applications and partners. Continuous improvement of data quality. Supporting data users as they work with data. Provision and onboard tools like Looker for Analytics , Monte Carlo for Data Observability , Snowplow for collecting rich customer behavioral data. Setup API integrations with marketing channels like Facebook, Google.


### Examples of cross-team collaboration


Having people of different specializations on a project strengthens each person and helps the team tackle the core problem from different angles. At thredUP we have great examples of collaboration within the data world that led to multiple powerful results. Let me share a few of them.


### Pricing system development


Pricing is at the heart of every e-commerce site and it is especially important in the resale business where every single item is unique. Items from the same brands and some SKU may have different quality and that changes how we price an item. In addition to that we vary prices depending on the “freshness” (recently released items will be priced higher soon after release and lower as time goes by), season, or user demand on certain types of items (office clothes had lower demand during the lockdown). Tackling pricing problems requires special focus from the data science team. The core team working on pricing consists of a data scientist and a machine learning engineer. These two specialists collaborate on defining and implementing thredUP’s pricing algorithm.


Team works in collaboration on:


- Designing pricing algorithms.
- Understanding gaps in the new system performance and proposing solutions.


Special focus of data scientists is:


- Identifying and quantifying opportunities for the business from introducing a new pricing system.
- Evaluating the new pricing system through an intensive cycle of A/B tests. This includes designing the test, evaluating results and planning subsequent tests based on results of the previous tests.
- Collaborating with business stakeholders to highlight the impact of pricing system changes on the P&L.


Special focus of a machine learning engineer is:


- Creating image similarity dataset to add visual components to pricing system.
- Implementing and maintaining pricing algorithms. This includes creating the architecture of a new system, scaling the system in production, and maintaining performance issues.


This project doesn’t require the involvement of a data analyst because data analysis was handled by the data scientist. The project also doesn’t require any massive involvement of the data engineering team because data was in place long before the project started, however the DE team supports reporting needs during the entire project.


### Migration to new data warehouse


Migration to the new data warehouse system is a critical data infrastructure project. ThredUP initiated this project to optimise infrastructure costs, improve performance and prepare for further data scaling. It requires the effort of many data engineers, data scientists and data analysts to implement the project and validate the correctness of the data after migration. It is critical to have specialists from both teams working on a project but they are solving different tasks.


Data engineering team:


- Creates architecture of the new warehouse and defines governance.
- Migrates the data and ensures completeness.
- Migrates BI tool to use new data source.
- Helps to migrate ETLs.
- Optimises queries and pipelines for cost and duration.


Data science team:


- Helps to validate the correctness of the data after migration.
- Migrates model pipelines.


Data analysts team:


- Validates that datasets and dashboards work correctly in the new warehouse.


The project doesn’t require a machine learning engineer because all changes to the modeling pipelines are focused on data source changes and no additional development is required.


### Churn prediction


Development of the churn model is inspired by the marketing team to boost efficiency of marketing spend on retargeting users and create effective campaigns to bring back users who are about to churn. A data analyst solidifies an idea for model development and a data scientist supports model development and validation.


Data analyst and data scientist work together on:


- Brainstorming what features to include in the model.
- Interpretation of model results.
- Brainstorming A/B tests design to validate model.


Data analyst has special focus on:


- Identifying business goal.
- Identifying business metrics for model validation and validation model on historical data.
- Collaborating with the product team to define an actionable roadmap for model testing.
- Designing target metric for model training.


Data scientist has a special focus on:


- Feature discovery.
- Tuning the model.
- Validation of model accuracy.
- Bringing in expertise from other similar predictive models.
- Model deployment.


A machine learning engineer is not involved in the project at the initial stage but he might be involved to help with model deployment and scaling. Data engineers likely will be involved to bring model results to Looker and make model predictions available for business users.


### Make your map in Data Science world


It can be difficult to find your place in the world of data science specializations. The best way to break into data science is to incorporate some data science elements into your current work.


People with a technical background in computer science, computer programming, software engineering, or robotics **** may want to enter the data world as machine learning engineers. If you are starting with an engineering training, then a path to joining a data team would be to leverage your engineering background and partner with a data scientist to implement and optimise their models. Then build understanding of ML techniques and theory through that work.


Another option is entering data science from a less technical domain (for example from a product management or business analyst role). In this case, it would be logical to start the career from the position of data analyst and gradually improve technical and modeling skills.


For beginners who are just starting out with a specialization, it is worth thinking about your desired balance between time spent on coding versus communicating with business partners and solving product problems.


If you are still not sure which data science specialization is the best for you — use the flow chart below (just don’t take it too seriously!). It is worth noting that to start working in data science one should have at least theoretical basic skills of modeling and /or data analysis. They are needed for all specializations. There are many online courses with great explanations of basics of data analysis, data science, and machine learning for people looking how to get basic knowledge and bootcamps to get advanced experience.


How to define your entry point to data science specializations


The world of data science is extremely interesting, exciting and open for people with different backgrounds. Opportunities to learn and cover any gaps in skills required for data specializations are growing continuously and it is easy to prepare yourself for your first position in data science. This is a[growing field](https://www.mckinsey.com/~/media/mckinsey/industries/public%20and%20social%20sector/our%20insights/the%20age%20of%20analytics%20competing%20in%20a%20data%20driven%20world/mgi-the-age-of-analytics-full-report.pdf) with the number of companies relying on data-driven decision making rapidly increasing over time. This is a great time to start an exciting journey in the data world.


— — — — —


¹ *About thredUP cultural values:*


- *Speak UP — We all learn from those around us, but it’s hard to learn if others are not willing to speakUP and share what they have to say. SpeakingUP can be doing so on behalf of customers; it can be doing so to make the employee experience better; it can be doing so to make the business operate better. When you have the choice to stay silent and go about your day or instead make your impact felt, you speakUP.*
- *Think Big — Thinking Big is not about coming up with wild and crazy ideas. It is not just about ideas at all. When we say Think Big at thredUP, what we’re saying is “Imagine the world as it could be, not as it is.” When given the chance to do the minimum and play it safe, you step back and say “How do I think bigger here? How do I turn this from a good idea/opportunity into a great one?” Thinking Big is the key for us to continue to grow and invent the future on behalf of our customers.*
- *Influence Outcomes — Much of our lives are focused on measuring inputs. How many hours did you put in? How many years of school did you complete? This is an input driven way of looking at the world, when what we really want to know is, how did you make an impact or a difference on the slice of the world you were in at the time? The best companies are built on a series of world-class teams influencing outcomes all along the way — big and small. It’s not what you’re going to do today, it’s what outcome you’re going to impact.*
- *Infinite Learning — This is a place for the infinitely curious and those who believe every day is an opportunity to learn, an opportunity to teach and, just as importantly, an opportunity to forget. The world is moving fast. To learn new things, you have to sometimes forget what you used to know (you know “the way it’s always been done”).*
- *Transparency — At thredUP, we don’t have offices. We talk in the open. We share the good news and the bad news. We give people information straight and we don’t obfuscate what’s going on. As a company we share it all, sometimes it hurts. But when we all know what’s up and what’s happening, it’s a whole lot easier to make the best decisions moving forward.*
- *Seek the Truth — As we’ve all seen over the course of our lifetime, the world will change, customer expectations will change, technology will change. First principle thinking encourages continuous innovation and creative problem-solving by reducing bias and faulty reasoning. It starts from the fundamental question of “why is this true?” Trust me, if you seek truth you will inspire others to do the same and we will all be better off.*


---


[From Data Analyst to ML Engineer: How to Choose a Position in Data Science](https://medium.com/connect-the-dots/from-data-analyst-to-ml-engineer-how-to-choose-a-position-in-data-science-f2c92d91ef60) was originally published in[Inside thredUP](https://medium.com/connect-the-dots) on Medium, where people are continuing the conversation by highlighting and responding to this story.
