---
schema_version: "1.0.0"
document_id: "6750d11bc2c91504192c592d19031967e3f4abdfb504b1eff3f7d29c88ed1fb6"
company_key: "yc-afterquery"
company: "AfterQuery"
source_id: "yc-afterquery-news-import-ad1daf482411"
canonical_url: "https://www.afterquery.com/blog/solving-the-last-mile-problem-in-partnership-with-the-raine-group"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-21T05:04:33.106205+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:e00dc3558f3dcd0ddaaf4255e55fb9ad300ecea97712cbdd6cf2c5b823db6e2f"
---

# Solving the Last Mile Problem in Partnership with The Raine Group

Our mission is to[encode domain-specific excellence into forms that machines can learn](https://www.afterquery.com/blog/the-afterquery-thesis) so that agents are able to think and execute like real-world experts. However, for most enterprises, encoding domain-specific excellence is not a one-size-fits-all endeavor, and theory doesn’t always map to practice.


Memorizing a corporate finance textbook and[acing an Excel modeling test](https://claude.com/claude-for-excel) doesn't make someone a capable investment banking analyst. The most important knowledge lives inside the organization — in past deliverables, internal templates, reviewer comments, Slack threads, and the way a team has always “done things.” It's messy, scattered across systems, and mostly lives inside the heads of long-tenured employees. This type of knowledge is only learned by working side by side with people who are intimately familiar with the alphabet soup of firm-specific data, battle-tested SOPs, and tribal knowledge.


Many AI copilots start with what AI agents can do today and work inward — they bolt agent capabilities onto an organization without addressing the underlying messy data problem. The result is impressive demos that fall apart in production because agents have no reliable source of truth and no first principle understanding of firm-specific workflows. In finance, ground truth is fragmented across decks, models, memos, markups, CRMs, email, and Teams; without a data foundation, even the best agents end up generating plausible-sounding outputs no analyst trusts enough to send to their VP, or 'almost there' work products that still require significant analyst cleanup and ultimately don’t justify spend.


Instead of starting with agents and working inwards, AfterQuery has taken the opposite approach: start with the data, establish a source of truth and understanding of each firm’s atomic workflows, and work outward. Agents come last. Working with all the frontier labs has given us a front row seat to the critical role data plays in improving the models of tomorrow. But as these models become more capable by the hour, the bottleneck for needle-moving enterprise agent deployments is no longer raw intelligence and agent capabilities — it's the infrastructure and tooling for making sense of the firm-specific data, workflows, precedents, and eclectic context that enterprises actually run on. We view data as the last mile problem in enterprise AI, and we're honored to partner with The Raine Group along this journey.


> “As both investors and advisors to some of the largest companies in the world, one of our most valuable assets is the earned knowledge and expertise held by our team across a global platform,” said Jeff Sine, Co-Founder and Partner at The Raine Group. “Partnering with AfterQuery reflects Raine’s commitment to equipping our team with thoughtfully designed tools that enhance judgment, not replace it. By taking a knowledge-first approach to building tailored AI solutions, we will enable faster, more informed decision-making, reinforced by the aggregation of our renowned institutional expertise.”


## Identifying Firmwide Bottlenecks


Investment banking is an industry of precedents. Deal flow velocity and a seemingly infinite client coverage universe makes utilizing precedent “off the shelf” materials a critical part of junior banker workflows (~50%+): there simply isn’t enough time to make every presentation or analysis from scratch.


AfterQuery spent several weeks learning how Raine bankers identify and utilize precedent materials through conversations with bankers across the firm. Our findings matched what our forward-deployed team had seen firsthand during their banking days at Goldman Sachs and Morgan Stanley: precedents exist in scattered folders across the firm that are often siloed by deal and industry teams. As a result, there is no shared knowledge index across the firm, so precedents are underutilized, and work is often unnecessarily duplicated. In isolation, this seems like a minor workflow inefficiency, but small bottlenecks that touch every deal are exactly where compounding advantages hide. And this impact doesn't stay at the analyst level—duplicated work delays execution, slower execution weakens competitive positioning, and weaker positioning puts firm mandates at risk. What starts as a search problem metastasizes into a P&L problem.


> “In enterprise AI, the challenge is often less about model capability and more about connecting models to a firm’s unique data and workflows,” said Alex Urbahn, AI Strategy Lead at The Raine Group. “Our work with AfterQuery is helping turn fragmented knowledge into more structured, searchable intelligence across our deal teams. That improves speed, strengthens institutional memory, and lets our professionals focus on differentiated insight for clients. It also helps lay the foundation for AI systems that better understand our business. AfterQuery’s frontier model expertise and hands-on collaboration have made it a valuable partner in these efforts.”


## Increasing Banker Leverage with Raine Search


Our forward deployed team spent three days in Raine's Midtown New York offices working side by side with bankers to build Raine Search: a semantic search tool that lets bankers at the firm query the Raine precedent library using natural language prompts that capture underlying banker intent. The system runs on AfterQuery's Atlas annotation platform and uses our data labeling agent harness, which together ingest, parse, and index Raine's historical deal materials into searchable vector embeddings. But building a search index was only half the problem. We spent significant time with deal team captains across Raine co-developing a firm-specific taxonomy—deal type, sector, asset class, transaction stage, etc. —that turns institutional Raine knowledge into semantically indexed, agent-ready data. This taxonomy is continuously refined through AfterQuery Atlas, so the system compounds in value with every new project at the firm.


Critically, we designed the taxonomy to serve two goals. For bankers today, it powers faster, more precise precedent retrieval via a custom semantic search platform that lives within Raine’s PowerPoint and internal cloud environments. For the firm tomorrow, this Raine Search tool becomes part of the harness scaffolding for Raine internal agents to execute long horizon, end-to-end workflows with full precedent firm context.


In a world where each[new Claude for Financial Services](https://claude.com/solutions/financial-services) release calls into question whether enterprises should be allocating their AI budgets towards vertical app layer agent solutions, we believe that regardless of whether the winning solution ultimately lives in the application or model provider layer, what matters in either world is laying the data infra groundwork that agents will need to operate with firm-specific context. At AfterQuery, we work with the frontier labs to improve their models of tomorrow, and that work gives us a direct line of sight into where the data gaps lie with enterprise deployments of these models. We built Raine Search to close that gap with a structured, continuously enriching index of Raine's precedents, deal taxonomy, and workflow context that increases in usefulness as agents become more capable of doing end-to-end work with each new model release.


– Carlos Georgescu, Sam Jacob
