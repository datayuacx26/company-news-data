---
schema_version: "1.0.0"
document_id: "f7eb3bfee59afd091728ce16c7736c69c6fe13763c24240fbc58d0c612bcb5fc"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/agent-experience"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-22T22:11:34.419543+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:265e4b9aa3992e9451fdb6113f638663a4dd9308bc79d88f6344702dcc0725db"
---

# Your agents can operate GrowthBook now

The[Fyxer team ran 541 experiments](https://www.growthbook.io/customers/fyxer) last year with a small team. They didn’t just get faster at using their tools. They put agents in the loop to run the workflow itself.


This is the workflow they shared with us: someone fills out a form with a hypothesis and target metrics. This triggers an agent to create a flag and experiment in GrowthBook and publish to a company-wide experiment index. Someone starts the experiment with a “launch” message in Slack. Only 25% of their experiments won, but that fast iteration and learning loop helped drive ARR growth from $1M to $35M.


Fyxer wired that together themselves. **GrowthBook 5.0** makes this[agentic experience](https://www.growthbook.io/platform/ai-native-development) first-class and accessible for more users and teams natively. Now, agents can operate GrowthBook: create flags, set targeting rules, brainstorm and create and launch experiments, with a consistent experience across interfaces.


## How agents operate GrowthBook


Agents can now run GrowthBook from the places you work. Picture one experiment over the course of a week.


You start a morning in Claude Desktop brainstorming your next experiment. The GrowthBook skills ground your ideas in your own history: what you’ve run, what succeeded, and how metrics moved.


You pick a direction, switch to Cursor and build it. Your agent creates the feature flag, sets up the variations, and configures the experiment alongside the code changes. Our skills carry guardrails, so the experiment lands as a draft for a teammate to review rather than a live test. Review, launch, and ship it.


Create an experiment with GrowthBook Skills in Claude


Later in the week, you’ve got results. You open the GrowthBook app and ask the new AI Assistant how the test did and whether you should ship. You want to dig deeper into how feature usage differs across segments, so you work with the AI analyst to build out a dashboard to share with the team.


You change tools, but the platform and the experience stay the same.


We built this consistency by building reusable foundation elements:


- [25 open-source skills](https://github.com/growthbook/skills) that teach an agent how to use GrowthBook effectively across flags, experiments, and product analytics.
- **A**[CLI](https://github.com/growthbook/cli) **rebuilt with Speakeasy** that covers 100% of our REST API and regenerates automatically from our OpenAPI spec. It won’t drift, and it returns typed output with errors an agent can use effectively.
- **A comprehensive**[REST API](https://docs.growthbook.io/app/api) **underneath** that fuels the skills and the CLI. If you’d rather see it than read about it,[watch here](https://youtu.be/pchg4xOoTSk?si=DQiseh5zJPfkYVGb) .


This foundation powers new ways you and your agents can run GrowthBook: the in-app AI Assistant, a reworked MCP server that integrates the skills, and a native Slack app (coming soon!).


## How to trust your agents


When agents are part of your team, they should follow your team’s standards and processes, just like anyone else shipping flags or experiments. Agents can make mistakes, but we’ve built our skills and Assistant so that agent-driven changes land as drafts and a person reviews before anything publishes. The same approval policies, audit trails, and guardrails to catch bad flags before they ship apply to both humans and agents. Your team maintains judgment and governance.


We’ll dive more into the improvements we’ve made to governance, especially for features flags, on Thursday.


## One platform for humans and agents


Whether you and your agents are in the terminal, the editor, or the app, you’re working on the same GrowthBook platform, with your flags, experiments, and warehouse data. We’ll keep evolving interfaces; you should never feel like you’ve outgrown the platform.


Connect your agent and[run your first skill](https://docs.growthbook.io/integrations/ai-agents/agent-skills/) .


‍


[Sign up](https://www.growthbook.io/events/growthbook-5-office-hours?utm_source=gb-blog&utm_medium=blog&utm_campaign=release-5-0) and join us for a live Office Hours Thursday, July 30, at 9am PT and tell us what to build next.


‍ **Tomorrow** : the new AI Visual Editor, rebuilt from scratch to make experimentation easier in the browser.
