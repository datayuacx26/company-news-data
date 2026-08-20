---
schema_version: "1.0.0"
document_id: "3746fc3a1b7154c51292bb903d4f1e4b58a5a9ddf21ac0ae69ebd3ffbcf73fe4"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-df2ad9c38740"
canonical_url: "https://www.atlassian.com/blog/company-news/inside-rovo-mcp-usage"
published_at: "2026-07-01T15:56:00+00:00"
first_seen_at: "2026-07-24T17:38:22.240177+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:563881a7b18d2cdbd7f486b21ddf0a3c752d12994bac59c077780793a0689890"
---

# What 5M+ daily MCP tool calls taught us about the future of AI at work

### *AKA how agents are actually being used and who’s getting value from them*


Less than six months ago, the[Atlassian Rovo Model Context Protocol (MCP) server](https://www.atlassian.com/platform/remote-mcp-server) went[GA](https://www.atlassian.com/blog/announcements/atlassian-rovo-mcp-ga) , giving Claude, Cursor, and every major AI agent direct access to Atlassian for our customers. Today, over one million users trust it every month to do real work through agents.


But that number isn’t the story. The story is what’s happening inside those interactions: how AI agents are actually being used at enterprise scale, and who’s getting the most value. The answer traces back to the[Teamwork Graph](http://teamworkgraph.com/) , a living context layer that connects work, people, knowledge, and code across Atlassian and all your favorite SaaS apps.


### Agents are doing the work


We’re seeing more than five million tool calls every working day through our MCP server, and that number is climbing month over month. This means agents are creating Jira work items, updating statuses, logging decisions, and linking work to the conversations that explain why it all matters.


These agents aren’t just observing the work system, they’re fully participating in it. They’re teammates with commit access.


### The most intense users aren’t always who you’d expect


Here’s something that might surprise you: 44% of all MCP users aren’t on software teams, and that number climbs every month. And C-suite executives and product managers use MCP just as intensely as engineers do.


Why? Because the people whose jobs are about synthesis across tools, teams, and decisions are the ones who deal with the highest workflow fragmentation. And they can’t put it down. What started as a tool for power-user engineers has quickly become a way for any knowledge worker to connect their context into agents so they can operate at a fundamentally different level.


This is where the value of the Teamwork Graph shines. It goes well beyond surfacing a single work item or page. Teamwork Graph is your organization’s shared brain and the collective intelligence of your entire ecosystem of SaaS apps, accessible to any agent or person, all in one interaction.


### Every agent action makes the whole system smarter


We couldn’t stop nerding out over the fact that nearly a third of all our MCP tool calls are writes, which means agents creating structured data vs. just consuming it. That’s a really big deal and a different pattern than any integration we’ve ever seen.


Every write adds structured context back into Atlassian, with more relationships and more organizational memory for the next agent interaction.


It delivers compounding value: usage generates richer context, richer context generates better answers, and better answers drive more usage. This means agents are both using and contributing to your team’s system of record in real time and as part of the flow of work.


### Enterprise is leading, not lagging


Over 50% of our monthly active users are enterprise customers. If you’ve been in tech long enough, you know that’s unusual. Enterprises often need more runway.


But in this case, it makes sense: the larger the organization, the more fragmented the tooling, the higher the coordination tax, the more valuable a connected context layer becomes. That’s exactly what the Teamwork Graph provides, not just for Jira or Confluence, but across Google Drive, Slack, and your entire tool stack. This is why the biggest organizations are moving fast on adopting MCP.


It also helps that the Atlassian Rovo MCP Server is enterprise-ready by design: Atlassian-hosted, OAuth-secured, respecting existing[permissions](https://support.atlassian.com/security-and-access-policies/docs/Configure-Atlassian-Rovo-MCP-server-permission/) , with admin controls over which AI clients can connect and full audit visibility into what they’re doing.


### Richer context means better and more cost effective


A common misconception about MCP is that it’s a fancy way to grab your next Jira ticket. That couldn’t be further from the truth. When an agent pulls a work item, it surfaces the related context that often isn’t even linked, like the Google Doc with the original brief, the Confluence page with the background research, and the Slack thread where the scope changed.


For better or for worse, we humans tend to be lazy linkers. We know things connect, but we don’t always make that explicit. The Teamwork Graph always does. That’s why agents grounded in Teamwork Graph context deliver 44% more accurate answers using 48% fewer tokens.


*“The more context an AI agent has about how a company actually works, the more it can get done. Atlassian’s Teamwork Graph captures something most systems miss: the connections between people, projects, and work. Through MCP, Claude can draw on that structure to understand how any piece of information fits into everything else going on, and act on it.”*


Lee Blackwell Applied AI leader,


Anthropic


In a world where every AI interaction has a cost, that matters. Pre-mapped organizational context on the relationships between people, projects, decisions, and tools is dramatically cheaper and more accurate than agents discovering those relationships on the fly, one API call at a time.


As AI usage scales across an organization, the cost advantage of structured context widens.


### People come back, and they keep finding new ways in


Retention is up in every single cohort. That’s the kind of metric that separates a trend from a fad. Customers are fully building MCP into how they work.


“ *MCP connectors in Figma have become a core part of product development workflows for entire teams. Whether it’s a product manager pulling in a Confluence doc to spin up a prototype in Figma Make, or a designer referencing Jira tickets to explore ideas with the Figma design agent, teams can build with shared content on the multiplayer canvas while keeping updates in sync with their Atlassian tools.* “


Anna Kohnen vp of partnerships,


figma


What’s more, they’re connecting through whatever AI agent they want to use, whether that’s Figma or ChatGPT, right from where they already work. When organizational context makes it to every agent across your toolset better, adoption follows.


### Building for how agents actually work


The data told us exactly where to invest, and today we’ve shipped a whole set of new MCP capabilities based on these patterns. With writes as one third of all tool calls, we’ve made multi-step work native. An agent can create a work item, attach the relevant file, find the right assignee, edit an earlier comment, and link the new ticket back to the incident that caused it, all in one flow. We’ve also added deeper connections between code and work, and precision-scoped retrieval that keeps token costs low as usage scales. Learn more about the enhancements[here](https://www.atlassian.com/blog/jira/mcp-enhancements) .


### What’s next


MCP adoption continues to accelerate, and the patterns we’re seeing aren’t slowing down either. Users outside of software teams are growing every month. Enterprise customers are leading adoption, not lagging. And agents are becoming the layer that links decisions to execution, context to action, and people to the work that matters.


Seeing those five million daily tool calls from our users tells us that connected organizational context is delivering real value, and it’s shaping how we build. You’ll see deeper context across Atlassian and your entire tool stack, agents that don’t just execute multi-step work but initiate and orchestrate it proactively, and enterprise governance controls that scale with you.


Check out[Rovo MCP Server](https://www.atlassian.com/platform/remote-mcp-server) and hop into our[documentation](https://support.atlassian.com/atlassian-rovo-mcp-server/docs/use-atlassian-rovo-mcp-server/) to get started. Learn more about the Teamwork Graph at[teamworkgraph.com](http://teamworkgraph.com/) .
