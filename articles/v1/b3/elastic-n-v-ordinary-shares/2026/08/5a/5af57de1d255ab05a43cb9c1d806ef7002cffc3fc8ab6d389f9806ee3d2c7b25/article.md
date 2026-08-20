---
schema_version: "1.0.0"
document_id: "5af57de1d255ab05a43cb9c1d806ef7002cffc3fc8ab6d389f9806ee3d2c7b25"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/agentic-treasury"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T17:49:46.554777+00:00"
fetched_at: "2026-08-18T17:49:49.141207+00:00"
content_hash: "sha256:d59007d4d2659a278b71f990ecd9c22769a9f5f1a0a72781f5614004f6021cc2"
---

# Agentic treasury runs on searchable, observable context

# Agentic treasury runs on searchable, observable context


Why AI-powered treasury depends on real-time context, operational visibility, and trustworthy decision infrastructure.


By


[Karen Mcdermott](https://www.elastic.co/blog/author/karen-mcdermott)


August 18, 2026


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


It’s August, and with


[Sibos](http://sibos.com/) fast approaching, treasury is very much on my mind. Having worked at BNY earlier in my career, I still pay attention when the company publishes new thinking about the future of financial services.


BNY has long had its finger on the pulse of advancing technology, not by simply tracking what is changing but examining what those changes will mean for the infrastructure and operating models underpinning financial markets. Its recent article,


[Re-architecting Treasury Management: From Systems of Record to AI Powered Decision Intelligence](https://www.bny.com/corporate/global/en/insights/ai-decision-intelligence-treasury-management.html) is a good example.


The topic also connects two important parts of my own career. In addition to my time at BNY, I spent eight years at SAP, so the enterprise resource planning (ERP) landscape is very familiar to me. At SAP, treasury leaders alongside CFOs and their broader finance organizations were among the most important audiences we served. I saw firsthand both the extraordinary value of enterprise systems and the challenges financial leaders face when critical information is distributed across multiple applications, processes, and data environments.


## The real challenge: Turning data into decisions


BNY’s article makes an important distinction: Treasury does not necessarily suffer from a lack of data. It suffers from the time and effort required to turn that data into a decision.


Treasury teams already have access to balances, transactions, forecasts, payment activity, liquidity positions, counterparty information, and market signals. The problem is that this information often remains fragmented across enterprise resource planning systems, treasury management systems, banking platforms, dashboards, reports, and spreadsheets.


According to BNY, this creates


**decision latency**


. The information may exist, but people still have to find it, reconcile it, interpret it, and determine what to do next. More dashboards can improve visibility without necessarily accelerating that process.


That observation resonated with me because it extends well beyond treasury and because I have seen the issue from several sides: within a global financial institution, within one of the world’s largest enterprise software companies, and now through the lens of search, observability, and AI at Elastic.


## From systems of record to systems of intelligence


Across financial services, institutions have spent decades building highly reliable systems of record. Those systems are indispensable. They provide the controls, transaction processing, accounting integrity, and governance upon which the industry depends.


My years at SAP gave me a deep appreciation for the role ERP systems play in creating a trusted operational and financial foundation. CFOs and treasury leaders depend on them to manage core processes, maintain financial integrity, and establish a consistent view of the enterprise.


But even the most capable ERP or treasury platform operates within a much larger technology environment. Financial institutions also rely on banking platforms, payment systems, market-data services, risk applications, data pipelines, documents, communications, and an expanding range of cloud and AI services.


The challenge is therefore not that systems of record have failed. It is that they were not necessarily designed to interpret changing conditions across this entire environment in real time.


## Building the context layer for agentic treasury


BNY proposes an intelligence layer that sits above existing ERP, treasury management, and banking platforms. Rather than replacing those investments, this layer would connect information across them, apply context, recognize changing patterns, and help treasury teams move from reporting what happened to anticipating what may happen next.


That approach is important. Having spent years in the ERP space, I am skeptical of any technology narrative that begins by suggesting institutions should discard the foundational systems on which their businesses run. The more credible opportunity is to extend those systems by making their information


*and the information surrounding them*


more accessible, contextual, and actionable.


This is where I began thinking about the BNY article through an Elastic lens.


An intelligence layer can only be as effective as the context available to it. Before an AI application or agent can recommend an action, it needs to retrieve relevant information from across a highly distributed environment.


That context may include:


-


Account balances and transaction histories


-


Payment messages and settlement activity


-


Liquidity forecasts and positions


-


Accounts payable and receivable data


-


Market and foreign exchange signals


-


Counterparty documentation


-


Treasury policies and approval requirements


-


Application events, logs, metrics, and alerts


Some of this information is structured. Some is unstructured. Some arrives as a continuous stream of time-sensitive events. Some may be represented as vectors to support semantic retrieval.


Elasticsearch is built to store and search structured, unstructured, vector, geospatial, and time-series data in real time. It can provide hybrid retrieval across enterprise information, combining traditional lexical search, semantic search, filters, and analytical queries to surface relevant context for applications, employees, and AI agents.


That makes search more than a tool for finding documents. In this context,


**search becomes part of the decision architecture**


.


## From a financial signal to the full story


Consider a treasury team that sees an unexpected deterioration in its projected cash position. The immediate question is not simply, “What is the current balance?”


The team may also need to understand:


-


What changed since the previous forecast?


-


Which expected payments have not arrived?


-


Did customer behavior change?


-


Was a settlement delayed?


-


Did a source system fail to deliver current data?


-


Is the projected shortfall real, or is the underlying information incomplete?


-


What internal policy governs the proposed response?


-


Has the institution encountered a similar situation before?


Answering those questions requires retrieving context across ERP data, treasury records, financial transactions, operational systems, historical events, and enterprise knowledge.


That is where a search and analytics platform can help provide the foundation beneath the intelligence layer BNY describes.


## Why observability belongs in the treasury conversation


The article also prompted me to think about observability.


Observability is usually discussed as an IT discipline: monitoring infrastructure and applications, investigating failures, analyzing logs, tracing transactions, and maintaining system reliability.


But in an increasingly automated treasury environment, operational telemetry becomes part of the business context.


Imagine that a treasury intelligence application identifies a potential cash shortfall and recommends moving funds. Before acting, the treasurer needs confidence that the signal reflects an actual financial condition.


What if the apparent shortfall was caused by:


-


A delayed data pipeline


-


A failed banking API


-


An incomplete payment feed


-


An ERP synchronization issue


-


A stale market-data source


-


A problem with the application’s retrieval workflow


-


A failed call between an AI agent and one of its tools


The difference between a financial anomaly and a technology failure can be material.


This is where the boundaries between search and observability begin to converge. The institution must be able to examine both the financial signal and the health of the systems that produced it.


The question is no longer only whether the application is operating properly. It is also if I trust the business decision this application is asking me to make.


Elastic Observability brings together logs, metrics, traces, application performance data, infrastructure information, and other telemetry to help teams identify abnormal behavior and investigate its cause. In an AI-enabled treasury architecture, that visibility can help institutions understand whether a recommendation was affected by missing data, system degradation, application latency, or a failed dependency.


## Agentic treasury raises the stakes


BNY describes agentic treasury as the next stage of this evolution.


In this model, AI agents may continuously monitor liquidity and payments, identify anomalies, interpret changes, recommend responses, and in selected circumstances, execute predefined actions within established governance frameworks. BNY is careful to position this as an augmentation of treasury professionals rather than a removal of human accountability.


**That distinction is critical.**


The closer AI moves to consequential financial action, the more important it becomes to understand how a recommendation was produced.


Institutions will need evidence, such as:


-


Which data sources were consulted


-


Whether those sources were current and complete


-


What information the agent retrieved


-


Which models, tools, and workflows were involved


-


Whether any application or data-processing errors occurred


-


Which policies or thresholds were applied


-


When human approval was requested or granted


-


What action was ultimately taken


The governance framework for agentic treasury will extend across treasury policy, model risk management, identity and access controls, approval processes, regulatory requirements, and institution-specific business logic.


Search and observability can provide important elements of the supporting foundation: relevant context, operational evidence, searchable telemetry, anomaly detection, and a traceable record of how the underlying technology behaved.


## Automation must be earned


One of the strongest points in BNY’s article is that institutions do not have to move immediately from dashboards to autonomous execution.


The journey can be progressive.


A treasury team might begin by unifying search and analytics across fragmented information. It might then introduce AI-assisted investigation followed by recommendations supported by relevant evidence. Human-approved workflows could come next with selective automation introduced only for clearly defined actions operating within established controls.


That is a much more credible path than treating agentic AI as a switch that can simply be turned on.


In financial services, trust is built by proving that systems behave reliably, decisions can be explained, and people retain appropriate control. It is the familiar principle of “trust, but verify” made essential in an agentic world.


## The Elastic opportunity


My Elastic take on BNY’s vision is this:


**Agentic treasury will require a real-time context layer, but it will also require an observable decision architecture.**


ERP, treasury management, and banking platforms remain essential systems of record. Search can help AI applications and treasury professionals access and connect the relevant information within and around those systems to understand changing conditions.


Observability can help establish whether the applications, data pipelines, models, and services producing those insights are operating as intended.


Together, they can help institutions move not only from data to decisions, but also from decision latency to


**decision confidence**


. BNY is right that the future of treasury is not simply greater visibility. The real opportunity is continuous, contextual, and governed decision-making. But before institutions allow AI agents to act, they must be able to answer a foundational question:


**Can we see**


***and trust***


**how the decision was made?**


[Get in touch](https://www.elastic.co/contact?pg=global&plcmt=nav&cta=205352) to learn more about how Elastic can support your autonomous treasury journey.


###### Related blogs


- [Building the agentic SOC: A new model for financial services](https://www.elastic.co/blog/agentic-soc-for-financial-services)
- [Transform financial services with AI: Unlock growth, innovation, and insights](https://www.elastic.co/blog/how-banks-use-existing-data-ai-business-challenges)
- [AI-powered fraud detection: Protecting financial services with Elastic](https://www.elastic.co/blog/elastic-ai-fraud-detection-financial-services)
- [Agentic AI in financial services: The rise of autonomous intelligence](https://www.elastic.co/blog/agentic-ai-financial-services)
- [The rise of intelligent banking: Unifying fraud, security, and compliance in the era of AI](https://www.elastic.co/blog/intelligent-banking)


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


*In this blog post, we may have used or referred to third party generative AI tools, which are owned and operated by their respective owners. Elastic does not have any control over the third party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use.*


*Elastic, Elasticsearch, and associated marks are trademarks, logos or registered trademarks of elasticsearch B.V. in the United States and other countries. All other company and product names are trademarks, logos or registered trademarks of their respective owners.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
