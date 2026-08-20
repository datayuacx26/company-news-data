---
schema_version: "1.0.0"
document_id: "8e8015f1003e6d7aff12fa268bbad00d3fd5639e2b543424d41234a6338a04c4"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-af8c2eac0472"
canonical_url: "https://www.atlassian.com/blog/jira/writing-tickets-for-ai-agents"
published_at: "2026-08-10T03:59:36+00:00"
first_seen_at: "2026-08-13T01:30:56.386294+00:00"
fetched_at: "2026-08-13T01:30:58.023305+00:00"
content_hash: "sha256:c95dc076b082ebb85ad7aa3756322ca1302feaf7e9ee5bef1b03c1cf15a8376d"
---

# Why better tickets help agents write better code

*A reflection on building an enterprise product using AI agents, and what the data says about how we worked **.***


The observation


For the past few months we have been rapidly building an enterprise-wide, production-grade application that helps with employee compensation planning, management and communication. I compared what we built against a traditionally-built product to draw out insights about our new ways of working.


We set out with one goal, to push ourselves to see how fast a small team could ship a production-grade product. So we skipped the usual sprint ceremonies and traditional frameworks and just built.
Going in, we had a loose hypothesis that by working with agents, we would deliver faster, and we would probably write a few more tickets than usual to keep them on track. That much we expected.


What we didn’t expect was the scale. When we measured the result against a comparable product and timeframe delivered the traditional way, our per-engineer output was roughly **5x higher** across net lines of code, cyclomatic complexity, database schema size, and external integrations, with the two builds similar in size. And a team of five wrote nearly **5x more Jira tickets** per person than that team did.


The bigger surprise was the quality. On a source-blinded score, our tickets averaged **4.47 out of 5** for writing quality against 2.72 for traditionally authored ones, and **83%** were “agent-ready” versus 6%.


Better ticket in, better result out.


## Why we wrote more tickets, not fewer


Counter-intuitively, moving faster with agents made[well-scoped tickets](https://atlassianblog.wpengine.com/development/scale-agent-impact-with-jira-automation) *more* valuable, not less. Three reasons:


**Reason** **Explanation**


Tight scope keeps agents on the rails. We were using AI and agent harnesses, but this is not a vibe-coded app, it’s production-grade. Keeping each unit of work tightly scoped is what stops an agent from going rogue or inventing requirements we never asked for.


Coordination still needs a plan. With around 5 engineers each driving agents in parallel, planning is what keeps us from stepping on each other’s work and creating conflicts. The agent executes; the humans still coordinate.


A good ticket is the agent’s[execution loop](https://atlassianblog.wpengine.com/development/spec-driven-development-with-rovo-dev) . We want to hand the agent a well-defined unit of work with clear exit criteria, so it can run to completion without constant back-and-forth prompting. A well-written Jira ticket *is* that loop.


## How we actually create and delegate work items


Since the “how” is the part people ask me about most, here is what it looked like once we found our rhythm. The pattern that worked was to push the human effort upstream, deep understanding first, so that by the time an agent runs,[the ticket is a spec it can execute](https://atlassianblog.wpengine.com/company-news/ai-sdlc) .


**Step** **What we did**


Walkthrough Our TPM walks the team through a prototype of the feature. The engineers dig in, ask questions, and document the edge cases as they surface, before any tickets exist. The Loom notetaker records the meeting transcript, which later becomes another input for the planner agent.


Design together We discuss the high-level design as a team and collaborate on it in Confluence, so the shared understanding lives somewhere durable rather than in one person’s head.


Plan with an agent We hand a planner agent the context it needs, the PRD, the design document, the source code, and the meeting transcript from the walkthrough, and do interactive planning with it: investigating feasibility, pressure-testing the approach, ordering the execution and proposing the acceptance criteria. We go back and forth with the agent, refining scope and criteria until we are happy, and finally break the epic down into manageable, well-scoped tickets that the agent creates directly through the Atlassian MCP with the agreed acceptance criteria. The human effort concentrates on judgment (is this the right breakdown, are these the right criteria) rather than on typing tickets out, which is why the up-front scoping mattered so much and why the tickets scored so consistently.


Execute with an agent harness At the ticket level, we run an agent harness that follows the rules and guidelines defined in the repo, writes the code, and reviews it. Because the context lives on the ticket, multiple engineers run this loop in parallel.


Review and ship We use the Atlassian MCP to open pull requests and review each other’s work. At the pace PRs land, human review alone is hard to keep up with, so[agent assistance in review](https://atlassianblog.wpengine.com/ai-at-work/your-jira-board-just-got-a-new-kind-of-teammate) is essential. The Atlassian MCP connects planning, epic creation, Jira ticket creation, PR opening and review, and pipeline monitoring. That is a major reason Jira sits at the center of our workflow.


## Can we validate this with data?


Yes. I built an LLM judge that scored a source-blinded sample of tickets on five dimensions of writing quality (clarity, scope, acceptance criteria, context, and actionability), and compared our AI-native tickets against the traditionally-authored ones.


Here’s how the numbers broke down.


**Metric** **AI-native** **Traditional**


Tickets per engineer (in window) ~180 ~43


Mean ticket-quality score (1 to 5) **4.47** 2.72


Tickets that are “agent-ready” (score >= 4) **83%** 6%


Acceptance-criteria clarity (1 to 5) **4.32** 2.20


*Sample: 300 tickets (150 per product), seeded-random from the set actually referenced by shipped commits. The gap is statistically overwhelming (Welch t of about 17.5, p < 0.001), meaning with 150 tickets per side there is less than a 1-in-1,000 chance the difference is a sampling fluke.*


The widest gap is exactly where it matters most for agents: **acceptance criteria and context** , the two things an agent needs to execute autonomously. Our tickets read like executable specs (explicit acceptance criteria, areas in scope, out-of-scope notes, dependency links); traditional tickets were typically a one-line summary and a link.


## Where it fell short (and the good news)


One thing was genuinely lacking. Agents frequently surface real problems, latent bugs, shortcuts, tech debt, while working on something else. But with no frictionless way to capture them in the moment, those findings evaporate when the session ends.


The result is a kind of amnesia: the same issues get rediscovered over and over, and we pay the cost of finding them every time without ever paying it down.


The good news is we’re already building toward the fix: making agent activity a first-class citizen on every ticket, so an agent can capture a tech-debt finding the moment it spots one, instead of losing it when the session ends.


## The takeaway


What strikes me most is that none of this was mandated. We didn’t set out to prove a point about Jira. We reached for it organically, in the middle of moving as fast as we possibly could, and it kept earning its place. Every time we needed to scope an agent, coordinate across the team, or hand off work cleanly, a good ticket was the tool that made it work. It quietly became the backbone of how we operate in an AI-native workflow.


There’s a trust angle too. Enterprises already trust Atlassian with their most sensitive planning and knowledge data. Keeping the artifacts agents produce, the context they use, the decisions they make, the corrections, and the token cost, in that same trusted place matters, because you want to own that record rather than scatter it across outside AI tools.


*In a world where AI keeps improving and can seemingly grant any wish, what matters is how the wish is made. The specifics we choose to make explicit are how we shape what gets built, and every assumption we leave for the agent to fill in is a cost we pay later. And the place we found to make those wishes well, to capture intent clearly and hand it to an agent, was the Jira ticket.*
