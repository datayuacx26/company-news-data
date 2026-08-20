---
schema_version: "1.0.0"
document_id: "7bc3238cfca338bd1e33d6c8d055e61e5aaccd5fa800d7b96580bf80920f0ee3"
company_key: "yc-datafold"
company: "Datafold"
source_id: "yc-datafold-news-import-e233b68116ce"
canonical_url: "https://www.datafold.com/blog/data-engineering-in-2026-predictions/"
published_at: "2026-03-05T00:00:00+00:00"
first_seen_at: "2026-07-21T15:52:35.106036+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:45b379a4a8709262a9f6a5c3aea3dbd29a31e1676fbdf771d532381d89cf95f1"
---

# Data Engineering in 2026: 12 Predictions

Tectonic shifts in technology are underway. Frontier lab leaders predict that artificial general intelligence is just a single-digit number of years away, and that software engineering will be automated soon. I believe it’s important to reflect on what this means for data engineering.


Here are my predictions for 2026:


1. \[Obviously\] Agentic data engineering will boom in 2026
2. The productivity gap between AI-native data engineering teams and everyone else will be huge
3. Enterprise adoption of AI in data will be slow, but it doesn’t have to be
4. The job market for data and analytics engineering will remain turbulent
5. Data engineering will become as important as ever
6. The cost of data platform migrations will plummet
7. Competition between data platforms will drastically increase
8. Legacy data platforms are cooked
9. Bring-your-own-agent is the winning pattern in AI
10. Agents will become the primary user persona for data platforms and tooling vendors
11. Data teams will stop chasing data quality because AI doesn’t care about data quality
12. Anyone can become a 50x data engineer


## \[Obviously\] Agentic data engineering will boom in 2026


This may be obvious to many, but I am stating it again because the actual adoption of agentic data engineering, even within our customer base, is very low!


While vibecoding has been a thing for at least a couple of years, the launch of Claude Opus v4.5 in late 2025 marked a turning point in AI-driven agentic development. All of a sudden, you could create really sophisticated software by prompting the agent in English. We at Datafold shipped two completely new products from zero to private preview in just two weeks, written entirely by Claude Code.


What is agentic data engineering?


Agentic data engineering is when you have AI agents complete end-to-end tasks such as analyzing data, writing code, and testing it all by themselves.


What is *not* agentic data engineering: Prompting LLMs to write SQL for you based on a textual prompt and then pasting and running that yourself, or using “tab completion”. Having the agent actually *run* the code it wrote and act based on the result is the loop that creates true magic.


This obviously has massive data privacy and security implications. Enterprise adoption will be slow because infosec teams are forbidding agents that leverage third-party AI providers from accessing the data sitting in your data lake.


Databricks and Snowflake recognize this and have gone further than just offering LLM inference endpoints. Both now ship their own coding agents: Databricks has[Genie Code](https://www.databricks.com/company/newsroom/press-releases/databricks-launches-genie-code-bringing-agentic-engineering-data) and Snowflake has[Cortex Code](https://www.snowflake.com/en/news/press-releases/snowflake-unveils-cortex-code-an-ai-coding-agent-that-drastically-increases-productivity-by-understanding-your-enterprise-data-context/) (which also has a CLI you can use from your terminal, outside of Snowsight). You can also point third-party agents like Claude Code at Databricks or Snowflake LLM endpoints. Either way, the inference stays within the same security perimeter as your data.


With that said, there is 0 reason for any data team not to use agentic AI for development, but realistically, it will take years to fully propagate across the enterprise.


## The productivity gap between AI-native data engineering teams (using agentic AI for development) and everyone else will be huge


A major part of the data engineering workflow is understanding the data for a given business task, writing code, running and debugging it by looking at the result. Turns out that the current frontier LLMs and coding agents are very good at that with proper supervision.


Agentic AI gives its users tremendous leverage.


In the pre-AI world, a typical data engineer spent, say, 30% of their time planning, thinking, talking to stakeholders, and 70% writing and debugging their code.


Being a “10x Data Engineer” definitely meant having a substantial edge in writing the code, but just like with software engineering, the best data engineers didn’t simply win by churning out the code faster and with higher precision: they had a strategic edge — got involved in high-impact projects, picked the right implementation strategy, navigated stakeholders well, etc.


Now, if the 70% of writing and debugging code is fully automated by agents, this means the best data engineers have over 3x more time for things that truly differentiate them, so the impact multiplier grows from 10x to more like 30-50x.


## Enterprise adoption of AI in data will be slow, but it doesn’t have to be


Setting up a 50x agentic workflow for yourself at a startup is one thing, but enabling hundreds of people in a regulated enterprise environment to use AI effectively and securely with highly sensitive, complex analytical data, as the entire field evolves so quickly, is a significant challenge.


Data practitioners and teams at large companies who are willing to adopt agentic AI in their workflows are facing significant friction from IT and Security.


This is understandable given the “lethal trifecta” of AI agents in security: access to private data, exposure to untrusted content (e.g., when going on the web to fetch documentation), and the ability to communicate externally pose a previously unknown security threat.


However, I believe it is possible to deploy agentic AI to data teams safely and effectively by thinking from first principles and leveraging appropriate offerings from data platforms and vendors. For example, state-of-the-art agents like Claude Code have[enterprise features](https://www.anthropic.com/news/claude-code-on-team-and-enterprise) that allow centralized configuration, including whitelisting domains the agent can visit. The data platforms themselves now offer coding agents that run entirely within their security perimeter, like Databricks’[Genie Code](https://www.databricks.com/company/newsroom/press-releases/databricks-launches-genie-code-bringing-agentic-engineering-data) and Snowflake’s[Cortex Code](https://www.snowflake.com/en/news/press-releases/snowflake-unveils-cortex-code-an-ai-coding-agent-that-drastically-increases-productivity-by-understanding-your-enterprise-data-context/) , and both also offer LLM endpoints you can use with agents like Claude Code.


## The job market for data and analytics engineering will remain turbulent


Per[Indeed Hiring Lab’s 2026 report](https://www.hiringlab.org/) , data and analytics postings declined 15.2% year-over-year through October 2025, and overall tech postings dropped 8.5%. Taking this at face value, it suggests that data jobs are taking twice the hit from AI than an average tech job.


Zach Wilson conducted a deep dive into specific data engineering tasks and skills and how they are affected by AI. And Joe Reis rightfully[says](https://www.yourresource.com/) , “Ignore AI at your Peril” — the use of AI is a baseline expectation.


But *why* are data roles hit so hard by AI? I don’t think that’s because data engineering and analytics are less intellectually demanding than software engineering, or because they’re easier to do well.


At the same time, the bulk of the job over the past few decades has been writing and maintaining data pipelines to meet business requirements, and many of the tasks seem well-suited to AI.


My hypothesis is that this has to do with the problem space size: unlike software engineering, data engineering is a domain with ***more constraints*** : e.g., data is flowing left to right (from sources to end consumers) and is mostly in the form of tabular datasets, with SQL being the dominant language, which is simple and structured. And AI agents are known to tackle more pointed, structured tasks better than open-ended ones.


The predictions are that software engineering will be automated in 3-5 years, and that data engineering will be automated sooner.


As with software, one argument is that data engineers will simply move to higher levels of abstraction and their jobs will be more about interacting with human stakeholders, setting vision, and managing teams of agents. However, because of the more constrained domain, properties like personal taste, which have emerged as moats for software engineers in a fully agentic world, will be less important. Tables simply have fewer degrees of freedom than software apps, making them, once again, something AI can master sooner.


## Data engineering will become as important as ever


Data engineering, the process of delivering high-quality data in the right form at the right time to the right consumer, will become increasingly important as more and more consumers become AI agents that are not only incredibly data-hungry but, more importantly, make the data more valuable.


The value of enterprise analytical data depends on the economic value of the improved decision-making by humans and machines based on that data. For example, as a ride-hailing company, I may run an A/B test of a rider discount program, which, if successful, could lead to 1.5% revenue growth through increased conversions. At a billion-dollar revenue scale, the data used for the A/B tests can be worth millions.


In the pre-AI era, operationalizing data for decision-making was hard: it required collecting data in the first place, then integrating it into a data lake, cleaning and transforming it, and delivering it in a fresh state to a consuming application. Dashboards needed to be built, stakeholders educated, and machine learning models trained, deployed, and monitored.


AI makes each of those tasks either cheaper or irrelevant: e.g., if an AI agent is making the actual decision or deriving an insight, maybe you don’t need to build a highly curated dashboard and worry about delivering data to it?


This makes leveraging data in business cheaper, and I believe that the[Jevons Paradox](https://en.wikipedia.org/wiki/Jevons_paradox) applies here too: as building data pipelines, analytical, and ML applications becomes cheaper, the demand for data will increase.


I am therefore bullish on platforms like Databricks and Snowflake, given their aggressive horizontal expansion covering both data engineering, warehousing, AI, and transactional data.


## The cost of data platform migrations will plummet


Despite the word “data” here, every data practitioner knows that the real friction in a data platform migration, e.g., Informatica > Snowflake or Oracle > Databricks, is not moving the data — it’s rewriting all the legacy code that is platform-specific.


Previously, enterprise-scale data platform migrations lasted years, cost millions, and pretty much always went over budget and timeline. Rewriting millions of lines of code has been an incredibly labor-intensive problem with very limited tooling support.


AI is becoming really good at writing code, and data platform migrations are particularly well-suited for AI automation because it has a very concrete success criterion: every data point across legacy and new platforms should match.


The problem is still extremely hard, though: the SOTA AI Agents are magical when pointed at a particular task under tight developer supervision and planning review. A typical enterprise data platform migration spans tens of thousands of such tasks, requiring highly specific architectures to scale AI automation.


I am extremely familiar with this problem because we at Datafold automate migrations as an AI-enabled service. Even last year, using the Sonnet 3.7/4 generation of models, we were[6x cheaper and faster](https://www.datafold.com/blog/the-opportunity-hidden-in-legacy-etl-migrations) than traditional services-based consultancies. This year, leveraging the latest models, we will be compressing projects from years to single-digit weeks.


Even though I believe that Datafold provides the best AI technology for migrations at the moment, the market is still dominated by traditional consulting firms, which are forced to adopt AI tools to compete with each other. While I am bearish on that market’s business model and future prospects due to its billable-hours model and the lack of elite talent required for true AI breakthroughs, increased competition and automation will result in lower migration prices.


Low migration costs have the following implications:


## Competition between data platforms will drastically increase


Previously, data platforms locked in their customers in two ways:


1. Data lock-in: proprietary data formats and egress costs made data movement costly.
2. Logic lock-in (main one): moving platforms required rewriting millions of lines of code of platform-specific SQL and Python, as well as refactoring paradigms: want to move from stored procedures to dbt? Good luck — may need to redesign the entire pipeline and data model.


With migration friction drastically reduced, both lock-ins matter far less. As a result, data platforms will be forced to compete more aggressively on price/performance and to invest in new product offerings to differentiate substantially. This is good news for data teams as we will likely see better products at better prices.


## Legacy data platforms are cooked


Teradata, Informatica, Talend, Ab Initio, etc., are once-innovative platforms that as of today, are inferior to modern data stacks across every dimension and retain users only because of migration friction. As migrations become easy, they will lose customers quickly, and their shareholders will see most of their investment value evaporate.


Some legacy platforms, e.g., SQL Server, belong to hyperscalers, who are actively investing in modern replacements themselves (e.g., Microsoft Fabric) and working to migrate their customers to their new offerings before competitors do.


Others, like Teradata and Talend, are in a much more perilous position as they lack the resources and alternative technology to move customers to. These legacy platforms can’t do much about their fate because they lost the ability to innovate long ago. The only way to save themselves could be to acquire technologies and teams with significant market relevance. E.g., Teradata acquiring ClickHouse or MotherDuck, Salesforce/Informatica acquiring Fivetran/dbt, etc. But M&A Deals are risky and often value-destroying. So my confidence in their ability to turn it around is low.


## Bring-your-own-agent is the winning pattern in AI data engineering


Over the past few years, every vendor, including Datafold, has claimed to launch its own “AI Agent”. Most have been relatively thin wrappers around the vendor’s APIs and database, and with the advance of frontier coding agents like Claude Code (which are, ironically, also relatively thin wrappers around frontier LLMs), the power user move has been to let your own agent interact with the vendor’s API or MCP (which vendors have been rolling out quite rapidly).


The BYOA pattern makes a lot of sense for a few reasons:


In the rapidly (exponentially?) evolving AI space, the gap between the frontier model/agent and the second-best can be enormous, and for most vendors, keeping up with the frontier is impossible — their agents will always be worse.


Furthermore, BYOA allows users to benefit from connections to all systems simultaneously, e.g., my Claude Code is always connected to Notion, Linear, Unblocked, Datadog & Datafold MCPs. It’s much more powerful to use a single agent across multiple systems than to switch between agents with limited access and context.


I think this is good, and vendors should not feel insecure about letting someone else’s agents drive development on their platforms. After all, there are plenty of ways in which vendor systems are highly complementary if not essential to agent-driven development, and exposing your full functionality as MCP rather than trying to lock users in your own “agent” is most likely better for everyone.


## Agents will become the primary user persona for data platforms and tooling vendors


In a world where the majority of data engineering workflow parts and soon end-to-end tasks are performed by agents, it is logical that agents will be the primary users of platforms and tools: running SQL queries, querying metadata, etc.


This means that human interfaces and UIs will become less important, while agent interfaces such as APIs and MCPs will become ubiquitous: data platform and tool vendors will shift to cater increasingly more to AI agents as the core user persona.


Data has always been notoriously complex, increasingly so. To tame that complexity, the enterprise invested in data catalogs that attempted to structure the vastness of data for human consumers. But even the most advanced data catalogs only scratched the surface of unifying metadata available for an enterprise data platform, and operationalizing this metadata has remained extremely limited. Indeed, hot in 2020-2021, data cataloging as a category largely hasn’t proven commercially successful, with most players acquired/acquihired over the past few years. It’s not because the products were bad, but because human ability to navigate and act upon the complexity of modern enterprise analytical data has been quite limited, and consequently, the value provided by the data catalogs has been limited too.


Agents are much better than humans at efficiently navigating complexity, given the right context. They can handle arbitrary complexity and take action themselves, provided the right context is present. Context becomes the real differentiator in a world where access to frontier AI capabilities is cheap and available.


At Datafold, we invested in building an unprecedented, comprehensive context engine — Data Knowledge Graph that combines metadata about data lineage, business logic, usage, and scattered business context across all data platforms with incredible detail. Such a graph would be overwhelming for humans to use, but it is the perfect complement to AI agents.


## Data teams will stop chasing data quality because AI doesn’t care about data quality


“Data Quality” was a major topic of discussion in the data space five years ago, and numerous startups received non-trivial amounts of VC funding to tackle this problem. At Datafold, we spent several years focused almost exclusively on automating data quality as part of our broader mission to automate data engineering.


However, the data quality trend didn’t pan out as many anticipated: hundreds of millions of dollars invested in and years of R&D, data teams are still largely in the same spot with data quality, and “data quality startups” haven’t achieved nearly the same outcomes as the software darlings like Datadog.


It appears that **data teams largely gave up on data quality** — not that they don’t pay attention to it, but it turned from something that gets onto annual OKRs to something that’s best effort.


I have several hypotheses for why that happened:


Most data quality vendors approached solving data quality by applying software patterns (unit tests > data expectations, software monitoring > data monitoring, software CI/CD > data regression testing and diffing, etc.). This perfectly logical idea turned out to be useful to a certain degree, but not as much as many (myself included) had predicted.


In most software applications, ground truth is easy to define and relatively easy to test: the user either logs in successfully or not, and the order amount in the checkout flow is either correct or incorrect. Given fixed inputs, you know what outputs you want.


In data applications, **ground truth is notoriously difficult to define** :


1. Data is complex. You have multiple data streams flowing into the warehouse from various sources (events, third-party systems, and OLTP replicas). Some data may arrive early, while some may arrive late. This results in *a lot* of data, some of which is duplicated or even conflicting.
2. Definitions are loose and fluid: how an “active user” is defined may be different across marketing, product, and finance teams’ use cases.


**Data is inherently noisy** , and more alerting and test coverage proven to have diminishing returns — coverage is always a moving target, and maximizing it just leads to more noise.


Often, most impactful **data work happens on the frontier** — the mature use cases where data is fully studied, tested, documented, widely understood, and consumed passively via dashboards or self-serve analytics. Data people spend most of their time on questions or data apps that are net new and, by definition, haven’t been as thoroughly tested, so in a way, they are destined to always be dealing with poor data quality.


In data applications, **accuracy matters less than in software applications.**


Data applications help humans or (increasingly) machines make decisions, and for many use cases, including A/B testing, high-level reporting, and ML, data coverage matters more than the accuracy of each data point.


The entire field of data science, including various ML techniques, has evolved as a response to imperfect and noisy data, and by 2026, it appears that the field has given up on managing data quality on the micro level (e.g., pursuing column-specific data tests) and **learned how to work with imperfect data** .


### AI doesn’t care about data quality — it cares about context


All of the above arguments apply to why pursuing data quality for data teams has yielded diminishing returns.


Previous attempts to improve data quality were based on how humans work with data. Data quality has been important to humans because we have limited attention span and processing capacity. The sheer volume and complexity of enterprise data are overwhelming — that’s why large companies have structured their data teams as a matrix, with data engineers, analysts, and data scientists embedded in particular business units, where they spend months becoming familiar with the data specific to the subdomain.


In that world, it has been important to ensure there is a single, curated source of truth to focus limited human attention. Data models have been meticulously curated, tested, and documented — at least we aspired for that.


Current frontier AI models simply don’t have those limitations: they can learn any analytical domain in seconds and spawn dozens of queries to profile or validate the data.


The success of AI in analytics depends primarily on three things:


1. Being agentic, i.e., having a closed loop of having a task, writing queries, running queries, exploring outcomes, and iterating (vs. just being asked to write code without the true data feedback loop)
2. Having access to a lot of data for analysis
3. Having as much useful context about the data as possible.


The #3 is really important. What is that useful context?


The classic data quality patterns of “this is the gold table for describing users” and having dozens of assertions for each column like “order amount is always > 0” are helpful *somewhat* , but as we all know, the answer to which data to use for analysis is more complex and nuanced — finding the right way to answer analytical questions often comes down to things like *“you gotta use dataset A because we use it for regulatory reporting, and then join dataset B to look up the context, and also join dataset C which is undocumented but person Z said on Slack it can be used as a fallback when B has null values”* .


In the pre-AI era, this nuanced context lived mostly in the heads of data people.


In the AI era, to be effective and reliable in analytics, AI needs to access the same context at runtime.


The best way to represent that context is to construct a rich metadata graph. At Datafold, we call it Data Knowledge Graph. It is a multidimensional graph that contains:


1. Data layer with column-level lineage across all systems from sources to BI applications
2. Source code layer — for any column or dataset, being able to instantly look up the business logic
3. Context layer — Slack messages, documentation, git log, etc., that are relevant to datasets and columns
4. Ontology layer — the map of core business entities such as User, Order, Store, Product, etc. with relationships between them defined as well as links to the Data and other layers.


Having such a knowledge graph at their disposal via MCP, AI agents do not need to rely on human-curated, but still inadequate, definitions of data quality — they can determine the most appropriate data to use and how to use it correctly at the level of a staff data engineer/analyst or better.


## Anyone can become a 50x data engineer


All the disruption from agentic AI doesn’t mean data engineers don’t have a future — it’s just a different future, where a distinguishing skill of a 10x data engineer is the ability to write very complex SQL correctly.


With agentic AI, data engineers can become not 10x but 50x versions of themselves in terms of impact, as long as they actively embrace the new technology and invest heavily in learning, tinkering, and staying up to date with the field.


Good luck building and learning this year!
