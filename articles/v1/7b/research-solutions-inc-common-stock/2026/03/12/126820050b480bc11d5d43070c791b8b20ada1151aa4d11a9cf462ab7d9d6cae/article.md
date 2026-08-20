---
schema_version: "1.0.0"
document_id: "126820050b480bc11d5d43070c791b8b20ada1151aa4d11a9cf462ab7d9d6cae"
company_key: "research-solutions-inc-common-stock"
company: "Research Solutions Inc"
source_id: "research-solutions-inc-common-stock-rss-6de2ff1cfa96"
canonical_url: "https://www.researchsolutions.com/blog/your-research-infrastructure-has-a-blind-spot"
published_at: "2026-03-26T13:57:13+00:00"
first_seen_at: "2026-07-20T23:19:56.052731+00:00"
fetched_at: "2026-07-28T22:17:33.975741+00:00"
content_hash: "sha256:4519061566f1d48b118a8d0db296f6ad9628114d0fa0fa4ca7cdc0ab0491b26b"
---

# Your Research Infrastructure Has A Blind Spot

If you run a research institution, a corporate R&D library, or any team where literature review is part of the day job, you've already spent years building out the infrastructure to support it. Subscribed journals. Licensed databases. Link resolvers. Institutional repositories. That's not a small investment: we're talking anywhere from[$2M to $25M](https://www.researchsolutions.com/hubfs/2026/Product-Sheets/Scite_MCP_AI_Investment.pdf) annually for serious research organizations. You know what you're buying, and presumably, you know why.


What's less clear is whether your researchers are using any of it.


Researchers aren't waiting for institutional guidance to arrive. AI tool usage among researchers jumped from 57% to 84% between 2024 and 2025, according to[Wiley's ExplanAItions study](https://newsroom.wiley.com/press-releases/press-release-details/2025/AI-Adoption-Jumps-to-84-Among-Researchers-as-Expectations-Undergo-Significant-Reality-Check/default.aspx) .[A UNESCO survey](https://www.unesco.org/en/articles/unesco-survey-two-thirds-higher-education-institutions-have-or-are-developing-guidance-ai-use) of academic professionals that same year found the figure closer to nine in ten. The behavior has already shifted. The infrastructure hasn't caught up: only 19% of higher education institutions have a formal AI policy in place, and just 41% of researchers feel their institution offers adequate support for navigating these tools.


There's a more specific problem embedded in those numbers. Of the researchers using AI, 80% are reaching for mainstream tools like ChatGPT rather than anything purpose-built for science. Part of that is familiarity, and part is awareness. Only 11% of researchers surveyed had even heard of the specialized research AI tools available to them. The result is a patchwork of general-purpose solutions standing in for research infrastructure that institutions have spent years and significant budget building out.


That's the gap the Scite MCP Admin tool is designed to close.


### More Than A Dashboard


The[Scite MCP](https://www.researchsolutions.com/blog/connect-your-ai-tools-to-scientific-research-with-the-scite-mcp) (Model Context Protocol) server is the connection layer that plugs Scite's database (280 million articles, 1.6 billion Smart Citations) directly into AI tools like Claude, ChatGPT, and Microsoft Copilot. It lets those AI tools search verified scientific literature, retrieve citation context, surface Smart Citation signals, and resolve access through institutional holdings. Think of it as the plumbing that keeps AI-assisted research grounded in real sources instead of training data guesses.


The Admin tool is what lets institutional administrators manage and monitor all of this. It's a dashboard, yes, but the word "dashboard" undersells it. This is operational visibility at a level the research library world hasn't had before.


### The Data You Didn't Know You Were Missing


Start with the Overview tab: total AI reads, unique papers accessed, active users, and session counts.


AI reads, specifically, are worth explaining.


Each AI read represents a paper retrieved within an MCP call. So, when a researcher prompts their AI tool with a complex clinical question and the system pulls 10 papers to build a response, that's 10 AI reads. These numbers tell you, at a meaningful scale, how much of your team's research activity is flowing through AI-connected workflows.


The Overview tab also includes an Audit Log toggle. Enabled, it tracks individual user activity for institutional reporting; disabled, it gives privacy-sensitive environments a way to maintain researcher confidentiality within the system.


Within the Usage tab, research leaders can really start to drill down. Usage is visualized as a chart across a configurable date range, broken down month by month. More importantly, you can see per-user data: how many reads each individual researcher is running, how many unique articles they're accessing, session totals, and when they were last active. All of it is exportable as a CSV for further analysis or stakeholder reporting.


For enterprise R&D teams or academic institutions that need to justify budget allocations, this is the kind of concrete activity data that supports informed decisions.


### Connecting AI Activity To The Collections You Already Own


Most of the real value here sits in the LibKey and OpenURL tab. This is where the Admin tool stops being a reporting tool and becomes a collections management tool. Administrators can configure the institution's link resolver, proxy settings, and LibKey integration. Once those are in place, the Scite MCP starts resolving paper access against your institutional holdings automatically. Researchers in an AI session get routed to full text through your existing infrastructure, without needing to switch context or reauthenticate.


It also surfaces Collection Gaps: papers researchers attempted to access through the MCP but couldn't, because the entitlements weren't there. It’s based on real demand, captured in records of what your researchers tried to access but couldn’t. This changes the budget conversation.


For library directors who've spent time arguing for acquisition spending based on indirect evidence, the gap report is a meaningful shift. Budget season looks different when you have documented demand rather than projections.


### Compliance & COUNTER Reports, Without The Extra Work


The last tab in the dashboard handles COUNTER-style usage reporting. COUNTER Reports have been the library standard for usage statistics for years; they're what institutions use for compliance, renewal negotiations, and publisher conversations. The Scite MCP Admin tool generates these natively, with monthly summary and per-user item reports available across custom date ranges, exportable as CSV.


Having this built into the same dashboard as your usage analytics and collection gap data means fewer tools to manage, fewer exports to reconcile, and less manual effort to produce reports that are truly useful to internal stakeholders.


### A Note On How The MCP Handles Data


One thing worth stating clearly, especially for institutions with data governance requirements: the Scite MCP server is a read-only retrieval layer. It doesn't run a language model of its own. All language generation happens on the client side, the AI tool the researcher is already using. Scite receives only the specific tool call parameters (a search query, a DOI lookup) and returns structured data: search results, citation metadata, Smart Citation classifications, and access links. The full conversational prompt never reaches Scite's servers. Authentication is handled via OAuth 2.0 or API keys, and all content served through the MCP operates within Scite's publisher agreements with 40+ publishing partners.


That architecture matters for research IT teams and procurement reviewers who need to understand exactly what they're introducing into the workflow.


### What's Possible When True Visibility Is Enabled


Research institutions have spent years, and serious money, building the infrastructure to support good science. Subscriptions, link resolvers, licensed collections, document delivery all assembled with the assumption that researchers would move through it in predictable ways. AI has changed those patterns faster than most institutions have been able to track.


The Admin tool doesn't ask you to rebuild anything. It tells you what's happening inside what you've already built. Who's using it, how often, what they're finding, and where the gaps are. It turns activity that was previously invisible into something you can measure, act on, and report to stakeholders. The infrastructure you've invested in becomes legible, maybe for the first time.


And that's certainly worth something.


To see the Scite MCP Admin dashboard in action,[request a demo](https://www.researchsolutions.com/sales) or contact the Research Solutions sales team.
