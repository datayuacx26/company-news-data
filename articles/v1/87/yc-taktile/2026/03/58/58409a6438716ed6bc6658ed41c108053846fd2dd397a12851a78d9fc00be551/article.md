---
schema_version: "1.0.0"
document_id: "58409a6438716ed6bc6658ed41c108053846fd2dd397a12851a78d9fc00be551"
company_key: "yc-taktile"
company: "Taktile"
source_id: "yc-taktile-news-import-01317932feb4"
canonical_url: "https://engineering.taktile.com/blog/five-layers-from-writing-code-to-writing-companies/"
published_at: "2026-03-11T00:00:00+00:00"
first_seen_at: "2026-07-24T03:38:54.472601+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:e45c91bcb28fcf3c4c142d7bd659b18b6c47d82a5be5ef57bab183639a879fef"
---

# From writing code to writing companies

Mar 11, 2026 ·


10 min read


# From writing code to writing companies


---


*A story about how the entire industry is going through a major upheaval right now. How we build new abstraction layers, and how each new layer collapses faster than the last.*


Before 2025, building software meant writing code. You opened your editor, typed functions, fixed bugs, committed changes. The feedback loop was tight and the work was deeply manual. If you wanted something built, you either built it yourself or explained it to another engineer who then wrote the code for you.


That world is over. Not in the way people predicted, where AI would slowly nibble at the edges of productivity. It is over in the sense that the entire definition of what it means to “build” is being rebuilt from the ground up, one abstraction layer at a time, at a pace that is difficult to grasp.


Thinking in layers of abstraction, I count five layers:


1. **Writing Code:** Human writes every line
2. **Describing Changes:** Human tells the agent what to change, reviews the result
3. **Harness Programming:** Agents write 100% of code
4. **The Organisation:** Goals, roles, intent. Code is invisible. Humans are the board
5. **Meta-Organisations:** Build many companies, companies that build companies


Each layer does the same thing: it moves humans further from the implementation and closer to the intent. Each one feels radical when you first encounter it and obvious in hindsight.


Let me walk through them.


# **Layer 1: Writing Code**


This is where most of us started. You think about what needs to happen, you translate that into code, you run it, you debug it, you ship it. The entire creative and mechanical burden sits on your shoulders.


You are both the architect and the bricklayer.


Most of the history of software engineering lives here. Decades of tooling, languages, frameworks, and practices, all designed to make this loop faster. Version control, linters, type systems, CI/CD. All of it built to help humans write code better.


We thought this was fun.


# **Layer 2: Describing Changes**


Then Claude Code, Codex, and similar tools arrived. Suddenly you could describe what you wanted changed, and an agent would write the code for you. Not perfectly, but well enough that your job shifted from writing to reviewing.


This is where most engineers are right now. You open a ticket in Linear or a Slack thread, describe what you want, and the agent produces a diff. You review it. You push back. You steer it. You might say “no, use this pattern instead” or “the error handling should work differently here.” The agent iterates, and eventually you approve.


### **Before**


> “Let me write the auth middleware, handle edge cases, add tests, and submit a PR.”


### **Now**


> “Add auth middleware that validates JWT tokens. Handle expired tokens gracefully. Cover the edge cases in tests.”


You are writing code faster than ever. But you are still deeply involved at the code level. You still review diffs, you still care about architectural patterns, you still insert judgment and taste.


You feel, rightly, that these things matter for your product to succeed.


The key shift here is subtle: you have moved from writing code to *defining work* .


But you are still looking at the code.


# **Layer 3: Harness Programming**


Some teams have pushed further. The goal is to let agents write 100% of the code. Not 80%, not “most of it.” All of it. This approach has been called Harness Programming or Harness Engineering, a term crystallised by OpenAI in their detailed account of[building a million-line codebase with zero manually typed code](https://openai.com/index/harness-engineering/) .


In a five-month internal experiment, a small team of engineers guided Codex agents through pull requests and CI workflows. The agents wrote everything: application logic, tests, CI configuration, documentation, observability, and tooling. The humans wrote zero code. Their primary job became enabling the agents to do useful work: designing the constraints, encoding “golden principles” into the repository, and building the feedback loops that kept the agents on track.


In this model, you are no longer looking at individual files and diffs. You are looking at Claude Code or Codex as your primary interface. You steer the agent through higher-level instructions. You are in the loop and you can intervene, but your relationship with the actual code is becoming indirect.


The natural next question: if the agents are doing all the work, why are you still manually dispatching it? Why are you sitting in Claude Code, feeding tasks one by one?


OpenAI’s[Symphony](https://github.com/openai/symphony) is one answer to this.


It builds on the same ideas as the[Ralph Wiggum loop](https://awesomeclaude.ai/ralph-wiggum) , and is basically a long-running service that reads tickets from Linear, creates an isolated workspace for each issue, and runs a coding agent inside it. You point it at a project, give it a few agents, and it starts churning through the backlog. The agents open PRs, review each other’s work, and merge when CI passes.


Symphony is still Harness Engineering at its core: it is repo-first, ticket-scoped, and implementation-focused. But it automates the dispatching. Instead of you babysitting each agent session, the system picks up work and runs with it. What is remarkable about the Symphony repository is that it is barely a code implementation. It is more a specification, with a strong encouragement to implement your own version.


They do not care about the code that runs it. They care about the ideas.


The critical insight here: **guardrails are everything** . The more rigid you are about what agents are allowed to do, what patterns they follow, what linting and testing rules they obey, the better this works. Without strict boundaries, agents drift. They do their own thing. They lose touch with reality. And then you have a refactoring project on your hands. Strict linting, formatting, and testing rules, easily accessible for every agent, are what keep the whole system grounded.


This also means its surprisingly hard to apply to a brown fields project, because agents need those strict guardrails, and we humans have a tendency to leave those out.


# **Layer 4: The Organisation**


Layer 4 is a big jump.


In Layers 1 through 3, humans are still thinking about code, even if they are no longer writing or reading it. Tickets are scoped to implementation. Work is defined in terms of repositories, endpoints, and modules. The abstraction has changed, but the mental model has not.


At this layer, you stop thinking about code entirely. Not just stop reading it, but stop scoping work around it. Instead of describing what the code should do, you describe what you want to be true. Not “implement a rate limiter on the API,” but “users should never be able to overwhelm the system.” Not a ticket, but an intent.


Thomas Dohmke, who served as GitHub’s CEO until August 2025, left to build a new company called[Entire](https://entire.dev/) with $60M in seed funding. His thesis: GitHub was built for human-to-human collaboration around code, but in a world where machines are the primary producers of code, the code itself is no longer the most important artifact. What matters is the reasoning, specifications, and intent that produced it. As he put it, we are moving toward “a much higher abstraction, which is specifications, reasoning, session logs, intent, outcomes”.


We never specified the code. We specify what we would like the world to look like. If something is off, we see it at the application level and create new work. We never touch the code.


But capturing intent is not just a tooling change. It is an organisational one. When you stop thinking about code, you start thinking about roles, responsibilities, goals, and accountability. Who defines what the product should be? Who decides priorities? Who reviews outcomes? These are organisational questions, not engineering ones.


The natural structure that emerges looks like a company. You set a high-level objective: “build the best possible open-source Python AWS serverless boilerplate.” You appoint an agent CEO, give them the mission and the vision, and let them execute. The CEO hires a founding engineer. The engineer hires juniors. The CEO brings on a product manager and a marketeer. They start building in public, writing blog posts, shipping code, iterating on feedback.


Tools like[Paperclip](https://paperclip.ing/) make this concrete: you define a company with an org chart, assign agent roles, set goals and budgets, and let the organism run.


You operate as the board, approving hires, reviewing strategy, and overriding decisions when needed.


Role Key Responsibilities


**Humans (Board)** - Set the mission
- Steer overall direction
- Approve major decisions


**Agent CEO** - Hire team members
- Delegate responsibilities
- Decide priorities


**Engineering** - Founding engineer and juniors
- Write code and manage infrastructure


**Product & Marketing** - Product manager, marketing team
- Manage blogs, growth, and outreach


These organisations are not static.


They learn. They figure out what worked and what did not. They adapt. Maybe at some point the CEO decides the team needs more people, or that someone is underperforming and should be replaced. This is how real organizations work. The structure is alive.


As humans, we operate as a board. We can still steer. There are things that require our approval, and we can instruct the CEO at any time.


> “I think you should focus on this opportunity.”


> “Fire this person.”


> “The strategy is wrong; pivot.”


The CEO takes the instruction and delegates to the rest of the organisation.


We never manage individual work items again.


# **Layer 5: Meta-Organisations**


And here is where it goes vertical.


If you can build one organisation and supervise it as a board, you can build ten. You can have companies that build companies. Companies that build templates for other companies. A portfolio of autonomous organisations, each pursuing its own mission, each with its own agent CEO, each growing and adapting.


A board can supervise many companies. That is, after all, what boards do.


This is the layer we understand the least. We have the primitives and tools such as Paperclip already support multiple companies in a single deployment with complete data isolation. The concept of downloadable company templates, pre-built org structures you can import and run, is on the roadmap.


But what happens when an organisation can spin up new organisations to solve subproblems? When a company can create a subsidiary, staff it with agents, give it a budget and a mission, and let it run? When the meta-organisation itself starts making decisions about which companies to fund, which to shut down, and which to merge?


We do not know yet. This is the frontier.


# **The Pace**


Between 2010 and 2016, the JavaScript community went through a period sometimes called the framework wars. React, Angular, Ember, Backbone, Vue, each rising in popularity, each fighting for dominance. It was volatile. You had to learn a new framework every year, sometimes every few months. Meta, Google, and smaller teams all competed to define the future of frontend development.


That war lasted about six years.


### **The pace of change**


AI layering timeline:


- Jan 2026: OpenClaw released. Becomes the standard for running dedicated agents. Fastest-rising project in GitHub history.
- Feb 2026: OpenAI acquires OpenClaw.
- Feb 2026: OpenAI publishes[Harness Engineering](https://openai.com/index/harness-engineering/) . A million lines of code, zero manually typed.
- Feb 2026:[Entire](https://entire.dev/) launches. Former GitHub CEO bets $60M that intent, not code, is the new primitive.
- Mar 2026:[Symphony](https://github.com/openai/symphony) released. Automated ticket-to-agent dispatching via Linear.
- Mar 2026:[Paperclip](https://paperclip.ing/) launches. Agent CEOs, org charts, budgets, governance. Companies as organisms.


Comparison


1. JS Wars: ~6 years
2. AI Layers: ~weeks


Everything I have described, from OpenClaw to[Harness Engineering](https://openai.com/index/harness-engineering/) to[Symphony](https://github.com/openai/symphony) to[Paperclip](https://paperclip.ing/) , was released within the span of a few weeks.


And here is the thing about abstraction layers: as you move up, the iteration speed increases. If agents can now build companies that themselves move up the abstraction layer, the progress compounds. Each layer accelerates the discovery of the next one.


Whether you believe in the singularity, AGI, or any of the grand narratives, it is important to recognise that this is not playing out on a timescale of years.


It is playing out in weeks and months.


# **What it Means for Us**


There will be a small group of people who benefit first. That is true of every technological shift. But I do not believe in the[permanent underclass](https://www.newyorker.com/culture/infinite-scroll/will-ai-trap-you-in-the-permanent-underclass) . I believe that everyone will have room to thrive in this new world, regardless of when or where they chose to participate, or even if they choose not to.


My hope is that humans get to focus on what makes us human. The creative work. Time with friends and family. The relationships and experiences that actually matter. If agents are doing the bulk of the implementation, then work and money might stop being the status markers they have been.


We get to be the board, not the bricklayers.


Let’s see how that turns out.
