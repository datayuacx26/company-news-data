---
schema_version: "1.0.0"
document_id: "b056fe9e81b5071a3bfabd9fdfc44af1f4bc69baa5c34122864090809440f397"
company_key: "research-solutions-inc-common-stock"
company: "Research Solutions Inc"
source_id: "research-solutions-inc-common-stock-rss-6de2ff1cfa96"
canonical_url: "https://www.researchsolutions.com/blog/what-an-mcp-actually-does-with-research-content-what-it-doesnt"
published_at: "2026-07-29T13:38:48+00:00"
first_seen_at: "2026-07-29T14:06:45.507212+00:00"
fetched_at: "2026-07-29T14:06:46.675361+00:00"
content_hash: "sha256:02fa2bb595e6bcf2e9fc68a0869c23ba2c3f992588ed3d0c07980f1b3b1edc0c"
---

# What An MCP Actually Does With Research Content (& What It Doesn't)

Researchers are already asking AI assistants for help with literature reviews, citation checks, and article access. That shift is underway whether any of us plan for it or not. The real question is what those AI tools are connected to. Left on their own, they invent references. Connected to a governed source of record, they return real papers with accurate citations and legitimate routes to the full text.


MCPs were designed to solve this exact issue. Since most people in research and publishing are new to the term "MCP," it helps to explain what an MCP is, how it handles scholarly content, and just as importantly, what it does not do.


### First, The Plain-Language Version


MCP stands for Model Context Protocol. It's an open standard that lets AI assistants such as Claude, ChatGPT, and Copilot retrieve information from trusted external services. When you connect an AI tool to an MCP server, you're not uploading a database into the AI. You're giving the AI a supervised way to ask questions of that service, one request at a time, following the same rules as a human user.


Basically, an MCP is a library catalog the AI can consult, not a copy of the library it can carry away.


Research Solutions offers two MCP servers,[one for Scite](https://www.researchsolutions.com/hubfs/2026/Product-Sheets/Scite_MCP_Explainer.pdf) and[one for Article Galaxy](https://www.researchsolutions.com/hubfs/2026/Product-Sheets/AG_MCP_Explainer.pdf) . They do different jobs, but both are built on the same principle: the AI gets structured information, and the content itself stays exactly where its license says it belongs.


### The Scite MCP: Discovery Without Disclosure


[Scite's MCP](https://scite.ai/mcp) connects AI tools to Scite's citation database. When a researcher asks their assistant a question, the MCP returns real, verifiable results: bibliographic metadata, the abstract where the publisher has made one available, citation counts, Smart Citation classifications showing whether citing papers support, contrast, or mention the work, and a DOI with a resolved link to the article.


This is especially important for publishers: unless a publisher has an explicit Scite MCP amendment in place, no full text beyond openly licensed material (CC-BY and equivalent permissive licenses) is ever shown, not to the user and not to the AI. Subscription and paywalled text stays behind the paywall. Even open access articles under restrictive licenses, such as non-commercial or no-derivatives, are excluded from AI-readable text.


People often wonder: *Scite indexes partner full text under indexing agreements, so doesn't that mean the AI can read it?* No. Indexing allows the system to search inside articles to find the right paper. Finding an article through full-text search is not the same as displaying its text. A paywalled paper can be located through the MCP, but its body is never passed along.


Instead, each result routes the researcher to the version of record on the publisher's platform, through an institutional subscription or a legitimate per-article purchase.


This makes the MCP a channel for traffic and demand.


Fully indexed articles consistently surface higher in AI-mediated results, which means more link-outs to the publisher's site. Scite also provides publishers and institutional customers with the same organization-level and journal-title-level reporting on usage and denials, so missed demand becomes visible evidence for new subscriptions rather than lost signal.


### The Article Galaxy MCP: The Order Desk, Not The Archive


[Article Galaxy's MCP](https://www.researchsolutions.com/api-and-mcp) handles a different part of the process: it checks if articles are available, verifies reuse rights, and allows purchases from within the AI conversation, using the same permissions a human would need. It's like making the Article Galaxy order desk accessible through an AI tool. The rules don’t change.


The AI never receives the article itself, not for paywalled content and not even after a purchase. The licensed PDF is delivered directly to the researcher through their Article Galaxy account, under the same terms as any other order. What the AI sees is structured data: availability, pricing, rights status, and metadata.


If a researcher does want to analyze an article with AI tools, they need a separate AI/TDM Rights grant for each article, and only if the publisher is part of the program. Those grants come with strict contractual limits: no model training, no permanent retention, capped verbatim snippets with DOI attribution, and internal use only. If a publisher hasn't joined, there is no AI-use option. If they have, each grant is a per-article fee, turning previously unlicensed and unmonitored uses into new, reportable revenue, itemized at the title level.


### Why This Architecture, & Why Now


Neither MCP runs an AI model on publisher content. The answers a researcher reads are written by their own assistant, whether that's Claude, ChatGPT, or Copilot.[Scite](https://www.researchsolutions.com/blog/your-research-infrastructure-has-a-blind-spot) and[Article Galaxy](https://www.researchsolutions.com/blog/article-galaxy-mcp-your-ai-now-talks-to-your-literature-library) simply supply the verified facts it draws on. Neither MCP trains models on scholarly content nor permits anyone else to do so through them. Access in both cases is limited to authenticated subscribers, each with their own credentials, usage limits, and full logging. There is no bulk access, crawling, or corpus exporting. An AI tool connected through either MCP can only do what its human user could do directly. Nothing more.


That last point sums up the whole design philosophy. AI-assisted research is already how a growing share of researchers works. The scholarly ecosystem now faces a choice about how AI tools will interact with the literature: through ungoverned channels that fabricate citations and ignore licenses, or through auditable infrastructure that respects rights, routes readers to the version of record, and creates new licensed revenue along the way.


Grounded, auditable infrastructure based on provenance and attribution is what we chose to build. Researchers get grounded, citable answers. Institutions get visibility. Publishers get protection, traffic, and a new rights channel with clear reporting.


If you are a publisher and want to see how your content is managed, reach out to our Vice President of Publisher Relations & Content Strategy,Sharon Mattern Büttiker . If you're a research organization looking to bring AI into your workflow with integrity built in,[talk with our team](https://www.researchsolutions.com/sales) about where to start.
