---
schema_version: "1.0.0"
document_id: "9b36c5981f8df4695c6708b97ee65bb8dba6320614e13f9977d890ca332420c3"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/cursor"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:a734247162f426039a387593283592562ea0aabf75003f124d8638f145f1dc12"
---

# Elastic and Cursor partner to accelerate context engineering with coding agents

# Elastic and Cursor partner to accelerate context engineering with coding agents


By


[Hemant Malik](https://www.elastic.co/blog/author/hemant-malik)[Laurent Saint-Félix](https://www.elastic.co/blog/author/laurent-saint-felix)[Rithika Kancharla](https://www.elastic.co/blog/author/rithika-kancharla)


April 13, 2026


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


Elastic is thrilled to announce a deeper partnership with Cursor, the leading AI coding platform for building software with agents. Elastic is the context backbone that those agents call into, providing real‑time observability, security, and search data drawn from production systems at scale. Together, Cursor and Elastic are partnering to bring context engineering directly into the agentic development platform, so coding agents can go beyond repository awareness and ground every suggestion, refactor, and fix in live operational data. With the new[Elastic plugin on the Cursor Marketplace](https://cursor.com/marketplace/elastic) , developers no longer need to leave their editor to query logs, triage alerts, or look up Elasticsearch syntax — Cursor handles it natively, as part of the coding workflow.


The Cursor Marketplace equips agents with the tools and data to do real work \[...\] With Elastic as a native plugin, every agent in Cursor gets access to live production logs, security alerts, and Elasticsearch data, giving teams context engineering where it belongs: inside the development workflow, not a separate AI console.


###### Brian McCarthy, President, Cursor


Reliable, trustworthy AI starts with context engineering, the practice of building systems that give AI agents the precise, grounded information and tools they need to complete complex tasks accurately. Elasticsearch connects data, retrieval, tools, and memory — the essential components that enable models to learn, reason, and act from the right information.


But context engineering doesn't end at the data layer. Inside Cursor, it shows up as a continuous loop: An agent reads code, runs tests, observes logs, surfaces alerts, and proposes fixes all without leaving the editor. Rather than a single retrieval augmented generation (RAG) query that returns a static answer, Cursor's harness efficiently orchestrates which tools agents invoke and when, including Elastic skills, linters, test runners, repo search, based on what the task actually demands.


Elastic's new plugin on the Cursor Marketplace brings these two halves together. It packages Elastic's platform expertise as first-class Agent Skills and a built-in Elastic Docs MCP server, removing the architectural friction of separate, disconnected platforms. Cursor can now discover and invoke Elastic's data-aware tools on demand:


- **Search:** Semantic hybrid search over private knowledge bases directly from the editor.
- **Query:** Execute ES|QL queries against live production data mid-workflow.
- **Observe:** Manage and surface Kibana dashboards when a build fails or a test breaks.
- **Secure:** Triage security alerts and propose fixes grounded in the alert data that triggered them.


**The result:** Engineering teams get an agent that sees code and production reality in a single workspace, accelerating product innovation, reducing operational toil, and enabling more robust AI solutions at scale.


Context engineering connects data, retrieval, tools, and memory, so agents can reason and act from the right information, and Elasticsearch is built precisely for that \[...\] By bringing these capabilities directly into Cursor, we're giving engineering teams a way to build agents that know their systems and data — the foundation needed for team shipping reliable AI applications.


###### Greg Tademoto, GVP, Business Development & Strategic AI Partnerships, Elastic


## Elastic plugin on Cursor Marketplace


The[plugin](https://cursor.com/marketplace/elastic) bundles two complementary capabilities to deliver a fully context-engineered development experience.


1.


**Open source Elastic Agent Skills:** Covers Elasticsearch APIs, Kibana, Elastic Observability, Elastic Security, and Elastic Cloud. Give the Cursor agent the same architectural standards and best practices used by Elastic's own engineering teams. These skills allow Cursor's agent to securely invoke custom, private data tools, such as semantic search, analysis of observability logs, and querying of security alerts — all governed by Elasticsearch's granular role-based access controls and field-level security.


Built on the open[agentskills.io](http://agentskills.io/) specification and released under the Apache 2.0 license, these skills are composable and modular by design: The ES|QL skill activates when writing queries, the dashboards skill picks up when visualizing results, and the triage skill chains into case management workflows when investigating a security alert.


2.


[The Elastic Docs MCP server](https://docs-v3-preview.elastic.dev/elastic/docs-builder/tree/main/mcp) **:** Allows Cursor's agent to interact with Elastic documentation directly


## Get building


With Elastic's plugin now available on the Cursor Marketplace, developers and engineering leaders evaluating how to build production-grade AI applications can go from a natural language request to a precise, verified answer drawn from their own private data, all without ever leaving the code editor. Bring Elastic expertise into your agent runtime with the Elastic plugin directly from the[Cursor Marketplace](https://cursor.com/marketplace/elastic) or run npx skills add elastic/agent-skills


in your terminal.


###### Learn more


- [Get started with Elasticsearch on Cursor](https://cursor.com/marketplace/elastic)
- [Learn more about Elastic Agent Skills](https://www.elastic.co/search-labs/blog/agent-skills-elastic)


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
