---
schema_version: "1.0.0"
document_id: "4b05adf055df3c32aa7873d15d1b0f3b705e470b1ecef1f4939967989c940ad7"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/benchling-launches-ai-connectors-to-power-data-ecosystem"
published_at: "2026-04-16T08:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:a5c0f485bda84083d1b2ebfd3403aeaebcecc061b2a5e4b354e343ce60049d1b"
---

# Benchling Launches AI Connectors to Power the Data Ecosystem for R&D

**San Francisco, CA — April 16, 2026 —**[Benchling](https://www.benchling.com/) today launched[AI Connectors](https://www.benchling.com/ai/connectors) , a new set of capabilities built on MCP (Model Context Protocol) that connect scientific data to the growing ecosystem of AI tools used in R&D.


The number of AI tools in R&D is expanding quickly, but their usefulness depends on the ability to access the full context of scientific work. AI Connectors address this by establishing a common interface for data exchange, built on MCP. External data sources can be accessed within Benchling, and Benchling data can be queried by external AI tools. Scientific literature, experiment records, pipeline outputs, and institutional knowledge all stay connected, so every tool and every insight can build on the last.


“Today, scientists are still moving data between tools by hand: copying results, pasting back analyses, losing context along the way. AI Connectors eliminate that. It lets data move with the work, so every experiment and every insight can build on what came before,” said Ashu Singhal, co-founder and president of Benchling.


**What this looks like in practice**


In an antibody discovery program for example, teams often begin by reviewing prior internal work alongside published literature. Program leads want to query past project summaries that might be stored in[Notion](https://www.notion.com/) or[SharePoint](https://www.microsoft.com/en-us/microsoft-365/sharepoint/collaboration) to inform the program design. Before running experiments, a scientist uses[Elicit](https://elicit.com/) to search for published assay precedents and[GXL](https://gxl.ai/) to dig into figures, tables, and supplemental data across bioRxiv, medRxiv, and PubMed Central, pulling relevant findings directly into their Benchling notebook. As the program progresses,[Seqera](https://seqera.io/) pipeline outputs from sequencing runs are linked back to the experiment records that produced them. Large instrument outputs and generated reports stored in S3 are accessible through[Quilt](https://www.quilt.bio/benchling) without leaving Benchling or touching the AWS console.


Because the data stays connected throughout, Benchling's AI agents can draw on the full record when a scientist needs to pull together findings, compare results across runs, or answer a question about where a project stands. Nothing has to be reconstructed from memory or reassembled by hand.


**Available AI Connectors**


At launch, the[MCP Directory](https://www.benchling.com/ai/connectors) includes pre-built connectors, which Benchling Admins choose to enable and users can toggle on or off individually.


For teams that rely on enterprise tools for knowledge management and data infrastructure, the MCP Client connects to any tool with an existing MCP server, including[Notion](https://www.notion.com/) ,[SharePoint](https://www.microsoft.com/en-us/microsoft-365/sharepoint/collaboration) , and[Snowflake](https://www.snowflake.com/en/) , so institutional context is accessible alongside experimental work.


For scientific research and data, four purpose-built connectors are available at launch:


-


[Elicit](https://elicit.com/) **:** Search scientific literature and get relevant papers back, or generate a report on a research question based on the scientific literature, all without leaving Benchling. A scientist can quickly find published precedents for an assay or kick off a report to support findings before sharing with colleagues.


-


[GXL](https://gxl.ai/) **:** Search a deep index of 8M+ biomedical papers across BioRxiv, medRxiv, and PubMed Central, including figures, tables, and supplemental data.


-


[Quilt](https://www.quilt.bio/) **:** Link large datasets stored in Amazon S3 directly to Benchling notebook entries. Teams can access instrument outputs, pipeline results, and generated reports from within Benchling without touching the AWS console, and lineage is tracked from instrument to scientist to filing.


-


[Seqera](https://seqera.io/) **:** Seamlessly integrate bioinformatics into Benchling with the ability to launch and track Nextflow workflows, investigate results, and spin-up interactive environments.


**Making Benchling data available to AI tools**


Benchling's MCP Server lets external AI tools query Benchling data. Scientists working in Claude, ChatGPT, or a custom AI environment, can ask questions about their projects and get back structured answers drawn from their Benchling data, through Benchling AI agents like Deep Research and Ask. For example, a researcher drafting a summary presentation in Claude can ask a question about an antibody project and get results pulled from their Benchling records.


**Availability**


Benchling MCP Server is available now following a beta period with Benchling customers. General Availability of the full Benchling MCP offering, including the MCP Client and Directory, is planned for May 2026. Benchling customers interested in using these capabilities can[request access here](https://info.benchling.com/benchling-ai-connectors-ea.html?_gl=1*nrbeei*_gcl_au*MjI1OTEzNDAyLjE3NzI1NDUwMTk.*_ga*MTIwNjY2NTI0Mi4xNzcyNTQ1MDIw*_ga_BRQMQWV4FJ*czE3NzYyOTAxOTckbzYwJGcxJHQxNzc2MjkxMjE2JGo1MCRsMCRoMA..*_ga_ZM8Q1QRZY0*czE3NzYyOTAxOTckbzYyJGcxJHQxNzc2MjkxMjEyJGo1NCRsMCRoMA..*_ga_1KJG4ZGCKL*czE3NzYyOTAxOTckbzYyJGcxJHQxNzc2MjkxMjEyJGo1NCRsMCRoMA..) .


Partners and data providers interested in joining the MCP Directory can contact[Benchling Partnerships](https://www.benchling.com/partnerships) .


**About Benchling**


[Benchling](https://www.benchling.com/) is the AI platform for biotech R&D, unifying scientific data and automating workflows to accelerate discovery and development. Trusted by more than 1,300 companies worldwide, from pioneering startups to global leaders like Merck, Moderna, and Sanofi, Benchling gives scientists a single place to capture, connect, and act on data across the entire R&D lifecycle. With[Benchling AI](https://www.benchling.com/ai) , agents and models work directly inside scientific workflows, grounded in structured data. The result is faster teams, better molecules, and breakthroughs that reach the world sooner.
