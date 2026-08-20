---
schema_version: "1.0.0"
document_id: "d01e901a51f6e6edb1e1db27a6de7bc911bc45960f3d57ce704d64128c65ff9e"
company_key: "yc-mindsdb"
company: "MindsDB"
source_id: "yc-mindsdb-news-import-209cdb193474"
canonical_url: "https://mindshub.ai/blog/introducing-mindsdb-v2600-with-improved-agents-and-knowledge-bases"
published_at: "2026-02-26T00:00:00+00:00"
first_seen_at: "2026-07-22T04:26:30.664731+00:00"
fetched_at: "2026-07-28T21:26:27.452652+00:00"
content_hash: "sha256:b92a8707a0f2ef4a9f01627179f79f61531b2d7bb50a1c3db2b5238dee13f6c4"
---

# Introducing MindsDB v26.0.0 With Improved Agents and Knowledge Bases

## **A Federated Data & Context Engine for AI Apps and Agents**


Building AI applications and agents is no longer just about choosing the right model. The hard part is connecting that model to real, messy, distributed data safely, observably, and in a way that actually fits into production systems.


With **MindsDB**[v26.0.0](https://github.com/mindsdb/mindsdb/releases/tag/v26.0.0) , we’re doubling down on a clear direction:


**MindsDB is the federated query and context engine for AI applications and agents.**


This release sharpens MindsDB around the core primitives that matter most to AI app developers:


-


unified access to data across systems


-


reliable agents that work directly on that data


-


and structured, actionable outputs you can analyze or write back


v26.0.0 is a foundational step toward MindsDB acting as a universal Model Context Protocol (MCP) server, a single point where AI systems read from and write to enterprise data with confidence.


## **Why This Matters If You’re Building AI Apps or Agents**


Most AI stacks today still look like this:


-


pull data from multiple systems with custom pipelines


-


ship rows into an LLM or agent framework


-


post-process unstructured output


-


push results back through another integration


It’s slow, brittle, and hard to trust.


MindsDB[v26.0.0](https://github.com/mindsdb/mindsdb/releases/tag/v26.0.0) moves this workflow *into the data plane* :


-


Query data where it lives


-


Pass it to agents for semantic reasoning


-


Analyze the results using SQL


-


Optionally write results back


That’s the workflow MCP is trying to standardize. MindsDB is making it practical.


## **What’s New in v26.0.0**


This release focuses on three themes that directly improve how developers build AI-powered systems.


#### **1. More Officially Supported Data Integrations**


We’ve expanded the set of **verified and supported integrations** , so you can connect data across your stack without building bespoke connectors.


**New database integrations include:**


-


Oracle


-


Databricks


-


Amazon Redshift


-


TimescaleDB


-


MariaDB


**New application integrations include:**


-


**HubSpot** (CRM, tickets, activities, timeline events)


-


**Shopify** (products, orders, customers, inventory, marketing events)


-


**NetSuite** (SQL access via SuiteQL)


You can query all of these sources using simple SQL, then feed the results directly into AI agents.


####


#### **2. Faster, More Reliable Agents (Without Changing How You Use Them)**


Agents in v26.0.0 are now built on a[Pydantic AI](https://pydantic.dev/case-studies/mindsdb) agentic framework, replacing LangChain.


What this means for you:


-


Better performance and reliability


-


More predictable behavior


-


Fewer hidden abstractions


What doesn’t change:


-


Agents are still created with CREATE AGENT


-


Existing agent workflows continue to work


-


No rewrite required to benefit from the new foundation


This is a purely structural upgrade that makes agents dependable enough to sit in the critical path of AI applications.


#### **3. Structured, Actionable Agent Outputs - via SQL**


One of the most important shifts in v26.0.0 is how agents return results. Instead of treating agent responses as unstructured text, you can now shape outputs using SQL itself.


**Example: semantic enrichment + analytics**


```text
SELECT   product_category, average_sentiment
FROM   amazon_reviews_agent
WHERE   question   =   'What is the sentiment for each product category?'  ;
```


In this pattern:


1.


You query structured data (e.g. reviews, transactions, tickets)


2.


Each row is passed to an agent for semantic reasoning (e.g. sentiment, classification, extraction)


3.


The agent is guided to return structured columns


4.


You can immediately:


-


aggregate results


-


join them with other tables


-


or write them back to a database


This turns LLM output into database-ready data, not just text blobs.


With this structured output, users can also immediately build dashboards and charts, leveraging a new feature in the MindsDB GUI for simple chart creation.


We think of this as semantic decoration: enriching rows with AI-generated meaning, then continuing to work with them using SQL.


##


## **Backward-Incompatible Changes (Read Before Upgrading)**


For those relying on the features deprecated below (e.g. LangChain, ChromaDB, built-in ML handlers), MindsDB v25.14.x remains the recommended option.


#### **Agents**


-


LangChain has been fully deprecated


-


Agents now run on a Pydantic-based framework


-


Agent creation and usage syntax remains the same


#### **Knowledge Bases (KBs)**


-


ChromaDB has been deprecated


-


PGVector is now the default vector store for Docker Desktop


- MindsDB via PyPI or Docker image do not have any default vector store


These changes significantly improve KB performance for large-scale ingestion and retrieval.


#### **ML Handlers**


-


Built-in ML integrations (e.g. Lightwood) have been removed


-


MindsDB now focuses on:


-


federated data access


-


knowledge bases


-


AI agents


If you need custom ML logic, you can still bring your own model (BYOM) and integrate it into agent workflows.
