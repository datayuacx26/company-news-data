---
schema_version: "1.0.0"
document_id: "53eec1ac18531a24c102ce2f84061f880353f97549b358d441820209f2b40124"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-62ed1fb859d3"
canonical_url: "https://slite.com/blog/cerebras-knowledge-base"
published_at: null
first_seen_at: "2026-08-06T10:34:36.776839+00:00"
fetched_at: "2026-08-06T10:34:38.582285+00:00"
content_hash: "sha256:d81df48089bbde3333e6527496be59fe9778c698a7ada3c7d67a8f44803366cc"
---

# Inside the Cerebras knowledge base, and how to build one

In July 2026, Cerebras' internal knowledge base pulled in 2.4 million views on X.


Within three months of launching, employees, automations, and AI agents were asking it 15,000 questions a day.


Every company wants that: one permission-aware[internal knowledge base](https://slite.com/learn/internal-knowledge-base) that answers questions across Slack, GitHub, Google Docs, Jira, and every other tool, with citations instead of a pile of links.


The catch is that Cerebras needed a dedicated engineering team to build and run it, and most companies cannot spare one.


Here is what Cerebras got right, what it cost them, and how to get the same result without their headcount.


## Who is Cerebras?


Cerebras is an AI compute company with more than 700 employees. It builds wafer-scale processors, single chips the size of an entire silicon wafer, and runs one of the fastest AI inference services on the market.


A company whose entire business is making AI run fast had the talent, the infrastructure budget, and the engineering culture to build its internal knowledge solution in-house.


Cerebras built it to fix a problem most growing companies recognize.


[In their own telling](https://www.cerebras.ai/blog/how-we-built-our-knowledge-base) , as the company grew the same questions kept surfacing across teams:


- Where can I find X?
- Who is the expert in Y?
- What is Z?


while the answers stayed scattered across Slack, GitHub, docs, and Jira.


They were clear that this time around they wanted something to help them access their knowledge easily without building a single source of truth first, as they have had unsuccessful attempts at it in the past.


So, if they were not building a wiki for their knowledge base, what did they do?


## What kind of a knowledge base did Cerebras build?


Cerebras built an internal system that anyone at the company, human or AI agent, can ask questions in plain language. It searches across Slack, GitHub, Google Docs, Jira, code repositories, and internal databases, respects each person's access rights, and returns a cited answer rather than a list of documents to open.


Cerebras walks us through how they built it in[their announcement](https://x.com/cerebras/status/2077822555159945507) .


Knowledge stayed in the tools where it was created, and the search layer reached across all of them.


In industry terms, that is[enterprise search](https://slite.com/learn/enterprise-search) sitting on top of the existing stack.


## What the Cerebras team cracked: retrieval at runtime, not a stored graph


Ever since Andrej Karpathy started talking about a "[company brain](https://slite.com/learn/company-brain) " and Garry Tan showed off[GBrain](https://slite.com/learn/gbrain-review) , teams everywhere have been replicating their company knowledge into GitHub repos and building stored context graphs: a pre-computed model of everything the company knows, rebuilt on a schedule so an AI can read it.


Cerebras rejected that approach.


Cerebras built something simpler. Knowledge stays in the systems where it was created.


When someone asks a question, the system retrieves the relevant context from those sources at that moment. This is the same pattern behind[retrieval-augmented generation, or RAG](https://slite.com/learn/rag) , applied to a whole company.


There is no stored model of the company to keep in sync, and that matters because company knowledge changes constantly.


A context graph starts going stale the moment it is rebuilt. It is expensive to maintain. And most importantly, you do not need it: great search covers 99% of what a company brain actually gets used for.


For the deeper technical argument, see our piece on[MCP vs RAG](https://slite.com/learn/mcp-vs-rag) .


## Does every company need to build its own enterprise search?


Cerebras proves with a staggering 15 thousand questions a day that an internal enterprise search system works. It also reveals the bill. Making company knowledge query-ready requires:


- continuous ingestion, with refresh schedules tuned to each source
- connector design for every new tool you add
- ranking and reranking methods that keep answers relevant
- authentication and[access-control replicas](https://slite.com/learn/knowledge-base-security) , so search never shows anyone something they should not see
- auditing and analytics
- dedicated retrieval logic for each type of source


among many other things.


I can say this because I have spent the past two years building one of the best enterprise search systems out there with Slite.


Keeping every one of those pieces reliable is demanding work. Three of our engineers spend all of their time on it.


Cerebras can afford that. With 700+ employees, staffing three or four people on something as crucial as internal infrastructure is a rounding error.


For most teams the math does not work, and the internal build joins the long list of[knowledge bases that quietly fail](https://slite.com/learn/why-knowledge-bases-fail) .


You can build this. You should not have to.


## We productized what Cerebras built


For the past two years, we have built Slite Agent around the same insight Cerebras landed on: infer the right context when the question is asked, instead of freezing company knowledge into a graph.


That meant:


- building deep integrations with more than 20 tools (Slack, Google Drive, Linear, GitHub, Jira, Intercom, and the rest of a modern stack),
- doing the messy work behind permissions, syncing, retrieval, and reliability.


Answers come back with citations, drawn from a[single source of truth](https://slite.com/learn/single-source-of-truth) rather than five conflicting documents.


We also expose the same company brain through MCP, so your AI agents ([Claude Code included](https://slite.com/learn/claude-code-company-brain) ) can ask questions and receive clean, permission-aware context.


What you get:


- a company brain that works this week, without a multi-quarter internal build,
- one source of trusted context for your people and your AI agents,
- a team that handles permissions, infrastructure, security, and GDPR compliance for you.


The company brain Cerebras built, without Cerebras-sized resources to build and maintain it.


## And then we went one step further


Even perfect retrieval can only return what your sources say. If a document is[outdated or contradicted](https://slite.com/learn/dangers-of-stale-documentation) , search simply finds the wrong answer faster.


If you have an internal documentation system where you keep your SOPs, policies, runbooks, playbooks and other living documents, you want to use this same system to maintain it on autopilot.


So Slite Agent also monitors your essential knowledge. When a document drifts from other signals in your company (a Slack conversation, a merged pull request, a support ticket), it proposes:


- the change to be applied
- the reason, with supporting sources
- a visible diff


A human approves or rejects every change. Nothing silently rewrites the company record.


This is what we call a[self-maintaining knowledge base](https://slite.com/learn/self-maintaining-knowledge-base-guide) : retrieval answers today's question, and[maintenance](https://slite.com/learn/knowledge-base-maintenance) makes the next answer better.


## Final thoughts


Company brains have become essential infrastructure. Cerebras had to build theirs, and had the rare engineering bench to do it. You can buy yours, and have it answering questions this week.


If you want to see what that looks like for your team, start with the[self-maintaining AI knowledge base](https://slite.com/solutions/knowledge-base) .
