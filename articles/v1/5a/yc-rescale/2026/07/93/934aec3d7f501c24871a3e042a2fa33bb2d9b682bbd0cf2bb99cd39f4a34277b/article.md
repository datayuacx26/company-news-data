---
schema_version: "1.0.0"
document_id: "934aec3d7f501c24871a3e042a2fa33bb2d9b682bbd0cf2bb99cd39f4a34277b"
company_key: "yc-rescale"
company: "Rescale"
source_id: "yc-rescale-rss-a01d3810de6f"
canonical_url: "https://rescale.com/blog/data-connectors-harness-historical-engineering-datasets-simulation-breakthroughs/"
published_at: "2026-07-09T16:06:35+00:00"
first_seen_at: "2026-07-20T23:20:48.219568+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:cad9884f240b50d762ffd428c4e891dd6c5f7aecb9e873d87f4f1954fe01dafa"
---

# How Rescale Data Connectors Harness Historical Engineering Datasets to Unlock Simulation Breakthroughs

[Back to Blog](https://rescale.com/blog)


[Company Announcements](https://rescale.com/blog/category/company-announcements/)


## How Rescale Data Connectors Harness Historical Engineering Datasets to Unlock Simulation Breakthroughs


Garrett VanLee


July 9, 2026


###### Connect, find, and use external engineering data more easily on Rescale.


Engineering data rarely lives in one place. Simulation inputs, reference documents, program files, and supporting context often sit across cloud object stores, SharePoint libraries, and other enterprise systems. That fragmentation slows teams down, especially when engineers need to find the right file quickly, attach it to a job, or bring it into an AI-assisted workflow where context matters.


Rescale recently expanded its data connectors capabilities to make that work easier. The focus is practical: help engineering teams browse external data sources, search for the right files, and use that data more directly in Rescale jobs, workstations, and AI-assisted experiences without unnecessary manual steps.


Most importantly, none of this requires replacing existing systems or migrating data into a new repository. Rescale connects to the systems teams already use, preserves context, and makes data more actionable across the platform.


*Browse Rescale’s data connectors to build a data fabric of engineering context to power fast access to data and agentic workflows.*


### Connect External Storage Without Changing How You Work


Rescale’s data fabric now supports production connectors for AWS S3, Azure Blob Storage, SharePoint — the three systems most engineering organizations already depend on for reports, job inputs, reference files, and program documentation.


Rescale’s AWS S3 and Azure Blob connectors allow engineers to browse, attach, and import data directly from customer-managed storage into jobs and workstations without manual downloading and re-uploading. This streamlined process eliminates redundant transfer steps and saves time for teams moving large datasets, with both connectors evolving to support faster, more scalable workflows for complex scenarios.


Rescale’s SharePoint connector provides a strong example for the AI-assisted use cases. Engineering and enterprise documents of all kinds often live across large SharePoint drives, but only a subset of that content is relevant to any given team or project. New path filtering capability lets admins restrict what gets indexed and surfaced, scoping ingestion to approved sites, libraries, or folders rather than entire repositories. That improves relevance, reduces noise, and makes the feature practical in enterprise environments where access management and rollout constraints are real operational concerns.


**Browse and Import Files More Efficiently**


One of the most immediate workflow improvements is the ability to browse and attach files from connected storage directly when setting up jobs and workstations. Rather than hunting through folder hierarchies manually or downloading and re-uploading files, engineers can locate the right content inside Rescale and add it to a job in fewer steps.


Combined with improvements to connector management in the admin UI, including real-time connection testing, credential editing, and ingestion controls, the process of setting up and maintaining connectors is more self-service than it has been. Credential handling also matters here. Admins can update credentials without disrupting active connectors, reducing administrative interruptions for teams that depend on connected data daily..


### Search That Matches How Engineers Actually Look for Data


Search across connected sources means two different things in practice, and it is worth being precise about both.


Engineers with large S3 or Azure Blob repositories can search by name or path directly inside Rescale, eliminating the need to navigate nested folder structures just to locate a known file. In-platform search allows engineers to find files by name or path within a connected object store. For teams with large S3 or Azure Blob repositories, this eliminates the need to click through nested folder structures just to locate a known file or dataset.


Semantic search allows engineers to ask natural-language questions against connected sources and receive grounded answers with citations back to the original documents. That capability is what powers knowledge-base queries through Rescale Assistant today, most visibly for AWS S3, Azure Blob, and SharePoint content.


Together, these two modes of search turn external data sources into more useful engineering resources: faster file access for operational tasks and deeper knowledge retrieval for more complex, judgment-intensive work.


*Select connected data sources to easily find and analyze historical engineering context faster.*


### An AI-Ready Data Layer for Agentic and AI Physics Workflows


Better-connected data directly improves what agents can do, and how reliably they do it.


Rescale agentic digital engineering embeds agents into simulation workflows that benefit directly from a knowledge base grounded in real enterprise data. When an agent references a design standard, retrieves a prior test result, or validates a configuration against existing documentation, the ability to search connected sources with semantic accuracy and return cited answers is critical. Rather than working with generic context, engineers and agents can draw on the actual files, reports, and institutional knowledge that already exist in customer systems.


The same logic applies to AI Physics workflows. Building surrogate models that behave reliably depends on having access to well-organized, traceable data. Connecting simulation data, reference documents, and experimental results from S3, Azure Blob, or SharePoint into a structured, searchable layer gives engineering teams a cleaner foundation for curating the datasets those workflows need.


Rescale structures connected data for engineering search and agent retrieval, preserving source attribution so engineers and agents can trace every result back to its origin. Under the hood, Rescale’s data intelligence fabric handles ingestion, chunking, vectorization, and retrieval using a RAG pipeline. Files are deduplicated before re-ingestion, source metadata is preserved for attribution, and the system returns responses with citations that point back to specific source documents. That traceability matters for engineering workflows where the source of a result or recommendation needs to be verifiable.


*Get useful takeaways from past engineering findings and apply them within existing simulation workflows.*


### Engineering Data Should Work for You


Rescale’s data connectors eliminate the gaps between where engineering data lives and where engineers need it. These capabilities also strengthen the data foundation behind the rest of the Rescale platform, from everyday simulation jobs to agentic workflows and AI Physics applications that depend on grounded, trustworthy context.


See how new connector capabilities can help your team access external data more easily, bring more context into simulation and AI-assisted workflows, and build on the engineering knowledge your organization already has. Connected data is the foundation for everything that follows, from smarter simulations to agents that can act on real engineering context.


[Learn more about data connectors](https://rescale.com/platform/agentic-engineering/) , available in Rescale Agentic Engineering, and[contact us](https://rescale.com/contact-us/) to ask about bringing your data sources to Rescale’s unified digital engineering experience.
