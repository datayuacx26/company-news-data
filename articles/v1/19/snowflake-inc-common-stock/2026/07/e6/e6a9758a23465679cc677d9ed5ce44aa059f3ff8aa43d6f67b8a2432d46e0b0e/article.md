---
schema_version: "1.0.0"
document_id: "e6a9758a23465679cc677d9ed5ce44aa059f3ff8aa43d6f67b8a2432d46e0b0e"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/apache-ossie-open-semantic-interchange-incubator"
published_at: "2026-07-09T04:12:00+00:00"
first_seen_at: "2026-07-24T01:53:21.113388+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:dedab49aabc7eff8cb776f67b2c04e84c095ba96b8447ced2c6a9392da592acd"
---

# Apache Ossie (Incubating): The New Name for Open Semantic Interchange

*Apache Ossie is currently undergoing incubation at The Apache Software Foundation (ASF).*


If you've been following the Open Semantic Interchange project, the open specification for semantic layer and ontology, there's an important update. The project has been accepted into the Apache Incubator under a new name: Apache Ossie (Incubating).


The spec, the community and the mission haven't changed, but the name, governance home and long-term trajectory have.


## Why the new name?


You may know this project as Open Semantic Interchange, or OSI. As the community prepared for incubation, the next step in the project's open source journey, they decided to rename the project "Ossie" to avoid confusion with other projects in the open source ecosystem that share the OSI acronym.


The mascot is a kangaroo carrying semantic metadata in its pouch as it hops between systems in the data stack.


Going forward:


- The project is Apache Ossie (Incubating)
- References to "OSI" as a project name are historical
- The specification and the YAML-based format for defining metrics, dimensions and relationships are unchanged


If you've been building on Open Semantic Interchange, nothing breaks. The name changed, but the spec didn't.


## What is Ossie?


Ossie is an open specification for both semantic layer and ontology. It defines a vendor-neutral format for expressing business metrics, dimensions and their relationships as well as broader business concepts and rules. It allows any tool — whether BI platforms, query engines or AI agents — to consume and produce semantic definitions without loss of meaning.


The problem it solves is straightforward: The same business concept (say, "Monthly Active Users") is often defined inconsistently across an organization's CRM, data warehouse and BI tools. When a human analyst or an AI agent runs a query, they shouldn't have to guess which definition is correct. Ossie provides a shared, machine-readable format that encodes not just the data but the intent and business meaning behind it.


Figure 1: Ossie-compliant semantic models are interoperable across data sources, AI agents, BI platforms and query engines.


With Ossie, organizations can go beyond defining metrics to include broader business knowledge across data sources and tools. AI agents that calculate and make decisions based on these definitions get the full context a human analyst would have, not just raw numbers.


## Why the Apache Software Foundation (ASF)?


Incubating Ossie with the Apache Software Foundation ensures that it remains an open standard with no single controlling entity. Given that the goal of Ossie is to provide industry-wide standardization of semantic data, ensuring that it has a vendor-neutral ground to operate in is crucial.


Under incubation, Ossie operates with public mailing lists, GitHub-based development, a formal discussion-and-vote process for spec changes, and committership earned through contribution rather than employer affiliation. Note that as part of this transition, all mailing lists referring to Open Semantic Interchange will be retired; community members should use the ASF-provided project resources that are linked below instead.


## The community behind Ossie


Ossie didn't start as a single-company project, and it already isn't governed as one. Since the repository opened in November 2025:


- More than 100 commits and 35 merged pull requests have landed from contributors at Snowflake, Salesforce, Databricks, dbt Labs, RelationalAI, GoodData and Honeydew
- The participating coalition has grown from 17 launch partners to[more than 50 organizations](https://www.snowflake.com/en/blog/open-semantic-interchanges-specs-finalized/)
- Three working groups (Metric Language, Catalog and Ontology) operate with dedicated leads, meetings and public channels
- Reference implementations have been created, including the Ossie-to-dbt MetricFlow converter, Apache Polaris™ catalog metadata converter, and a Snowflake Semantic Model converter


## What's next


Snowflake has been contributing to this project since its inception, and we'll continue as an active contributor to Ossie as it grows under ASF governance.


The roadmap is not predetermined; all items will go through the same open discussion-and-vote process as everything else in the project. That said, there are a few areas we're excited about and hope to work with the community to contribute proposals for:


- Deepening the spec's expressiveness to accommodate what real enterprise models demand, including advanced metric logic, windowing functions and complex relationships
- Building converters for additional platforms and frameworks so that adopting Ossie doesn't require ripping out what you already have
- A standardized semantic query specification that any engine can support
- Integration with Apache Polaris so that semantic models are discoverable directly from the catalog


## Get involved


Ossie is transitioning to ASF infrastructure as part of incubation. Watch for updates on the new[project website](http://ossie.apache.org/) , join the development mailing list, collaborate on GitHub and join the[Ossie Slack workspace](https://join.slack.com/t/apache-ossie/shared_invite/zt-42i1xkgy8-7YQtKEDq7v~mceFmdiLhkA) .


Whether you're building an AI agent, BI tool or a query engine that needs to understand business context, Ossie is the community working to make sure you don't have to tackle semantic interoperability alone.
