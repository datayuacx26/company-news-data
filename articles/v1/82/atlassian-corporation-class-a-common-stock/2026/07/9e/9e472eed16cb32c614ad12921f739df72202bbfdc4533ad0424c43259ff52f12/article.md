---
schema_version: "1.0.0"
document_id: "9e472eed16cb32c614ad12921f739df72202bbfdc4533ad0424c43259ff52f12"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-df2ad9c38740"
canonical_url: "https://www.atlassian.com/blog/company-news/closing-the-ai-context-gap"
published_at: "2026-07-07T17:03:34+00:00"
first_seen_at: "2026-07-24T17:38:22.240177+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:8186ed2fa245fdedeb9b9c96363e87d869257dffbd30cc845a9266c0b153deb9"
---

# AI that knows your business

### *How Atlassian connects your organizational memory across tools, teams, and decisions*


AI can write a launch plan in seconds, even if it doesn’t know what you’re launching. It can triage incidents without knowing your architecture, onboard new hires without knowing your team, and prioritize a backlog without knowing your customer. It has intelligence in spades and can produce like crazy, yet still miss what counts. AI on its own is unaware of your business.


Context is the difference between plugging into a few APIs and a genuinely deep understanding of how an organization works. It’s the kind of understanding that comes from 20 years of teams planning, deciding, shipping, and iterating inside your products every day. That’s the[Teamwork Graph](http://teamworkgraph.com/) , the collective intelligence of your entire organization and your full ecosystem of SaaS apps. It’s a living, permission-aware map of how your business operates, accessible to your agents and teams in one interaction.


It’s simple to get started. MCP works[so well](https://www.atlassian.com/blog/company-news/inside-rovo-mcp-usage) because anyone can connect it. Connectors are a switch any admin can flip. What’s underneath is what makes Teamwork Graph different: the deepest organizational memory in enterprise software, compounding every day. That’s why your AI + Atlassian context is so much more than a data connection, it’s knowing exactly how work gets done.


## **Your context wherever you work**


AI should not live in one product, one model, or one vendor’s ecosystem. It should live wherever your teams work. Which means the context layer underneath can’t be locked to a single provider, either. You need the freedom to use whatever AI works best for your teams, the confidence that your organizational context will follow, and the same permission boundaries your admins already set.


That’s why we designed it as a bi-directional system. Your Atlassian context flows out to whatever AI your teams choose, while context from your entire toolchain flows back in. And underneath, everything stays connected through the Teamwork Graph, compounding every time a partner connects, your team works, and an agent acts.


### **Your context, any AI**


The[Atlassian Rovo MCP Server](https://www.atlassian.com/platform/rovo-mcp) gives any AI client your teams already use, like ChatGPT, Claude, Copilot, Cursor, and Gemini, secure, admin-controlled access to your organizational context.


And it’s not just retrieval. Nearly one-third of the[5M+ daily tool calls](https://www.atlassian.com/blog/company-news/inside-rovo-mcp-usage) across our MCP server are writes, 50% of usage comes from enterprises, and 44% of users aren’t on software teams. This signals teams of all stripes are using AI to take action in real production workflows, updating work items, logging decisions, and assigning next steps. Your agent gets to see more than just the code, document, or update. Now, it sees the full intent behind it, from the decision that created it to the moment that shaped it to the team accountable for what comes next.


Today, we’ve made it easy for your agents to find context. With[Teamwork Graph tools in Rovo MCP Server and Teamwork Graph CLI](https://www.atlassian.com/blog/company-news/teamwork-graph-team-26) , they can begin to preserve it, writing discovered relationships back to the graph so your organizational intelligence compounds over time instead of disappearing when the session ends. An agent that remembers what it found is fundamentally different from one that starts fresh every time.


**Teamwork Graph CLI: Now generally available**


Teamwork Graph CLI, a purpose-built command-line interface that gives AI coding agents like Claude Code, Cursor, and Codex connected work context across your entire organization, is now[generally available](https://community.atlassian.com/forums/Atlassian-AI-Rovo-articles/Teamwork-Graph-CLI-GA-connected-context-at-enterprise-scale/ba-p/3254146?utm_source=atlcomm&utm_medium=email&utm_campaign=kudos_article&utm_content=topic) . Your agents can reason across Jira, Confluence, JSM, Bitbucket, goals, projects, teams, and 100+ connected third-party tools, all from a terminal or agentic workflow.


“It’s incredibly powerful. I have Forge apps building on my machine autonomously, and that’s all driven from Jira, and sewn into a console app on my machine via the CLI.”


Richard Sworder Head of Product Ownership,


Atlassian Williams F1 Team


What’s new since beta: enterprise-grade security and governance, built in from the start.


- **OAuth 2.1 authentication** — short-lived, auto-rotating credentials that replace static tokens.
- **Granular scope controls** — give agents read and write access without handing over delete permissions. Admins set org-wide policies; users can customize their own scopes.
- **Full audit logs** — every CLI request flows through Atlassian’s audit infrastructure. See which commands were invoked, by whom, and when. Export to Splunk as JSON.
- **567 commands** across Jira, Confluence, Jira Service Management, Bitbucket, Assets, and third-party surfaces.


“Teamwork Graph CLI is legitimately good at breaking down silos. We used it to look across Jira and team conversations for similar AI QA harness work happening elsewhere, and it did a great job surfacing duplicative efforts that would otherwise have stayed hidden.”


Ryan Boyd VP of IT,


SpotOn


Because Teamwork Graph CLI delivers pre-assembled connected context rather than forcing LLMs to fetch raw data one tool at a time, your agents get 44% better answers using 48% fewer tokens based on our internal benchmarking. That’s the difference between an agent that burns through your token budget building context on the fly and one that shows up already understanding the relationships between your work.


### **Bring your outside world in**


Your teams work across Atlassian, but also across dozens more tools. Our Teamwork Graph[connectors](https://www.atlassian.com/software/rovo/connectors) ensure AI gets context from everywhere you work, so AI can reason across your actual toolchain, not just one slice of it.


We built a system that understands what the data means, so when external context lands in Atlassian, it gets mapped to the Teamwork Graph. The applications your teams use the most, like GitHub, Google Drive, Salesforce, Sharepoint, and Teams, can now seamlessly be a part of your knowledge base. This means a Slack thread becomes a Slack thread where three engineers agreed to change the API contract, connected to the PR that implemented it, the customer who requested it, and the Jira work item that records it. Your external signals stop being scattered noise and start becoming organizational memory you can actually use, so when you Search, you find what you’re looking for, when you Chat, you get complete answers, and when you tee up Agents, they have the full context to act.


We’ve also invested deeply in making connectors reliable, fast, and trustworthy enough for enterprise teams to depend on daily. New improvements include:


- **Your critical tools, deeply integrated.** 50+ connectors live across GitHub, Google Drive, SharePoint, Teams, Salesforce, Figma, and more, with natural-language search across all of them that’s gotten dramatically faster. Your teams get better answers from their own tools because Teamwork Graph understands the context behind the results, not just the keywords.
- **Less friction, more control.** Connector setup time has dropped by more than 40%, with guided onboarding and real-time health dashboards so admins always know what’s working. For key sources like Google Drive and GitHub, an admin enables once and all users get access, no per-user OAuth gates. Multiple connector types let you choose the right approach for each source: deep indexing when you need full search, live retrieval via MCP when you don’t want data at rest, and lightweight Smart Links for zero-setup answers.
- **Expanding fast, and open.** New MCP connectors for Zendesk, ServiceNow, and more are one click away in Chat and Agents. Rovo can answer questions about any Smart Link content with zero connector setup. And our integration pipeline means new apps come online quickly.


When you need connectors for your line-of-business apps or other tools in your stack, you can build your own with[Teamwork Graph custom connectors](https://developer.atlassian.com/platform/teamwork-graph/build-a-teamwork-graph-connector/) , bringing legacy systems, proprietary databases, and internal tools into the same context layer that powers everything else.


## A system that compounds


Most AI tools are static. They’re as smart on day 300 as they were on day one. This architecture is different. It gives you something that gets better without you thinking about it:


- **Your work makes it smarter.** Every time your team ships a project, closes an incident, or makes a decision, the Teamwork Graph gets richer. You don’t have to configure anything. It’s always running in the background.
- **Your toolchain makes it broader.** Every connector you turn on brings more of your actual working environment into the map. AI gets the full picture of how your organization operates.
- **The ecosystem makes it self-reinforcing.** As more partners connect, the graph gets deeper for everyone. More context per session. More value per interaction. More signal, less noise.


Every team that joins, every tool you connect, and every decision you document makes the whole system more valuable. It’s a governed, auditable asset you’re building every day.


*Your agents are only as useful as the context they can access. Start by adding more data with*[Connectors](https://www.atlassian.com/software/rovo/connectors) *and bring it all together with the*[Teamwork Graph CLI](https://community.atlassian.com/forums/Atlassian-AI-Rovo-articles/Teamwork-Graph-CLI-GA-connected-context-at-enterprise-scale/ba-p/3254146?utm_source=atlcomm&utm_medium=email&utm_campaign=kudos_article&utm_content=topic) *. See what agents can do when they actually understand your organization.*
