---
schema_version: "1.0.0"
document_id: "c0dea6eff9e501abbe1029106aaa0944d5ad099dc93c01b0aca5f71e560f24f3"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-af8c2eac0472"
canonical_url: "https://www.atlassian.com/blog/company-news/ai-sdlc"
published_at: "2026-07-15T15:50:00+00:00"
first_seen_at: "2026-07-24T17:38:21.036095+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:964a6758178c2048f456b0099d09b3c09b5fd4b78891575b03eacac92d7f17d8"
---

# How we’re evolving Jira for AI-native software development

### *New Jira and Teamwork Graph capabilities help engineering teams plan, assign, govern, and measure work across humans and AI agents.*


Whether I speak to customers or Atlassian’s own engineering teams, the message is consistent: the unprecedented adoption of powerful coding agents has transformed software development, but the hard parts of *delivering software* are…gasp…still pretty hard.


Teams still have to decide what to build, and why it should exist. They need to understand the system they’re changing and which constraints matter. They have to know what “done” means, and whether the output is safe to ship.


That is the reality behind the AI productivity gap. In a[longitudinal study we ran with DX](https://getdx.com/blog/ai-impact-report-q1-2026/) across professional engineering teams, AI usage has increased by 65%, but overall developer velocity did not. It topped out at a 15% increase, with many organizations seeing gains averaging 10%


The gap is not because models are bad at writing code. It’s because software development has never been only about writing code. It is about turning business goals, strategies, and context into working software inside a real organization.


For more than two decades, Jira has evolved to meet software teams of every shape and methodology: we’re the source of truth for what to build, who is doing it, how it’s going, and what shipped.


Today, the shape of software teams is changing rapidly.


AI-native software development teams require a new system where context for agents is a first-class citizen and tasks are delegated to agents while humans steer, and review. Engineers, product managers, designers, and security teams bring judgment and context to the work.


It’s a new way for humans and agents to work together with clear plans, shared context, and validation that what comes back is something the team can stand behind.


Today, we’re announcing new agentic product development capabilities in Jira built for that shift. Teams can plan work with AI, turn intent into agent-ready specs, assign work to coding agents, monitor sessions, automate engineering loops, and measure AI cost against output.


Jira began as a bug tracker and now serves as the system of record for millions of teams’ work. And we will continue to evolve to serve the AI-native teams of the future.


### What AI-native software development means


The practical version is this: the SDLC needs to become legible to agents without becoming less accountable to humans.


That means three things.


- **Intent has to be structured before work starts.** An agent needs more than a prompt or a Jira summary. It needs the requirements, the relevant architecture, the decision history, and the constraints the team already knows.
- **Choosing the right agent should not create diverging processes.** A team may use the Cursor IDE for web development, Claude Code for complex backend tasks, a custom agent running in a cloud sandbox for unique codebases, and Jira Coding Agent to automate routine fixes at low cost. The workflow should not fork every time the runtime changes.
- **Autonomy has to stay observable** . Agent sessions cannot disappear into terminals, tabs, or disconnected logs, with critical context trapped on local devices. Teams need to see what happened, who reviewed it, and which work item started it.


Do those three things together and the system changes. Agents stop operating like isolated copilots and start participating in the same SDLC as the rest of the team.


That is where the Teamwork Graph matters. It is Atlassian’s context layer: a living map of work, code, people, decisions, and dependencies that helps agents understand not just the task, but the system around it.


### Why Jira is the right place for this shift


As the bulk of coding work shifts to agents, agents require well-defined tasks with rich, explicit context to deliver high-quality code while efficiently managing token costs. And context is almost never in one place, which is why Atlassian built the Teamwork Graph: to bring together that atomic task in Jira, the requirements in Confluence, the conversation in Slack, code context from GitHub, and customer insights from Jira Product Discovery.


Jira uses Teamwork Graph context to break big ideas into atomic tasks agents can handle and packages context for them to use when they work.


Without context, agents produce code that creates a productivity bottleneck down the road. They solve the ticket too literally. They miss the architectural constraint. They generate a PR that looks plausible until a senior engineer spends an hour unwinding it.


That is why this launch is not just about putting more agents in more places. It is about giving agents access to the organizational memory your team already relies on.


The Teamwork Graph supplies the context. Jira is where that context becomes workflow: intent starts there, agent work gets assigned there, session history is recorded there, and output comes back there for review.


### What we’re announcing today


Agentic work breaks in predictable places: vague plans, lossy handoffs, and output teams do not know how to trust. We built for those failure points.


#### 1. Plan with better context


- **Jira Planner** brings spec-driven development to Jira. For complex projects, Jira Planner pulls from your codebase, Jira and Confluence history, and team context to define requirements and generate a structured technical spec in Confluence. The output is readable by a human and useful to an agent. One artifact, two audiences.


- **Jira for Slack** turns the conversations where work is born into context-rich Jira work items. Teams can ask @Jira to create work, capture the nuance of a thread, sync conversation updates as comments, and assign work to coding agents without losing the discussion that shaped the decision. We’re also launching expanded Microsoft Teams capabilities coming soon.
- **Loom video prompts** turn what you show and say into structured instructions that agents can use to execute tasks. Record your screen and talk through the task. Loom captures your screens, clicks, links, and voice instructions and generates an action plan you can turn into agent-ready Jira work items in a few clicks.


### Delegate work to the right agent


- **Agents in Jira** let teams assign work items directly to Claude Code, Cursor, or GitHub Copilot, with Codex coming soon. Work stays grounded in Jira as the source of truth while context flows to the agent doing the work.


- **Jira Coding Agent** is built into every paid Jira plan. It can take a well-scoped work item, use enterprise context and code intelligence via the Teamwork Graph, make the change, and return a ready-to-review pull request without requiring a developer to switch into a local environment for routine fixes.
- **Agent sessions in Jira** : See which AI coding agents are stuck, what’s waiting for review, and what’s complete. Every engineer working in Jira gets visibility into agent sessions running across their spaces and repos in a single view, grouped by what needs attention first.


#### Scale agentic engineering with governance


- **Coding agent automations in Jira** let teams route routine work like bug fixes, vulnerability remediation, test generation, and documentation updates to agents using Jira’s enterprise-grade automation rule builder. Engineers are notified when a PR is ready, and every step remains tied to the original request.


- **The Agentic Engineering project template** helps teams stand up agent-ready Jira projects in minutes, with workflows, statuses, tracking, and integrations pre-configured.
- **DX AI cost management** gives engineering leaders a way to understand the economics of AI development. It unifies spend and token data across tools like Claude, Cursor, GitHub Copilot, and Jira, maps that investment to teams and projects, and estimates cost per PR in DX.


### A system for humans and agents


We have been running these patterns inside Atlassian with our own engineering teams. What we’ve found is that Atlassian’s Teamwork Graph provides the enterprise context behind many of these capabilities, connecting work, teams, goals, code, and knowledge across the SDLC so agents can act with greater relevance and accuracy. In internal benchmarking, agents enriched by Teamwork Graph showed 44% more accurate results while using 48% fewer tokens than agents operating without that context. Not only that, we’ve seen a reduction in PR cycle time, and less time spent on routine tasks.


“The bottleneck in AI-native development isn’t agent capability, it’s coordination at scale to keep our engineers in the flow. We’re partnering with Atlassian to solve that: one place where every agent action is visible, governed, and tied to a business outcome.”


**Sean Joerg** **Deputy CISO & Head of Corporate Engineering** ,


**Reddit**


Agents will change how software gets built. They will not remove the need for judgment, context, or accountability.


That is why Jira has to evolve. The next chapter of Jira is not just tracking software work. It is helping humans and agents do the work together.


Go to[jira.dev](https://jira.dev/) to try it today.
