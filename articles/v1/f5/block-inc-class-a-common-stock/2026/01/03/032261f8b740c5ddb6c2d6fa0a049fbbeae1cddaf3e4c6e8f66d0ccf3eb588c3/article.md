---
schema_version: "1.0.0"
document_id: "032261f8b740c5ddb6c2d6fa0a049fbbeae1cddaf3e4c6e8f66d0ccf3eb588c3"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/ai-assisted-development-at-block"
published_at: "2026-01-18T12:00:00+00:00"
first_seen_at: "2026-07-20T03:32:06.876214+00:00"
fetched_at: "2026-07-28T20:54:46.350230+00:00"
content_hash: "sha256:c238ce92798518b9faddf1f8457775127b452e778babefab79ce8467f6747783"
---

# AI-Assisted Development at Block

I must admit, I got quite the kick out of reading Steve Yegge's[Gas Town article](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04) over the holiday break. Afterward, I couldn't help but stop and think about where our engineers are on this journey.


Loading image...


About 95% of our engineers are regularly using AI to assist with their development efforts. The largest population is at Stage 5, running a single agent mostly outside of an IDE. The second largest population is at Stage 6 and is running 3-5 agent instances in parallel. Then there's a small population that is actively building our internal agent orchestrator in preparation for the inevitable.


So how does an engineering organization move from Stage 1, where engineers are just starting their AI-assisted coding journey, to an advanced stage where they are managing so many parallel agents that they now need an orchestrator? Here's how we're doing it at Block.


## 1. Provide the Freedom to Explore


AI tooling and LLMs are changing so rapidly. One week, a new model drops that changes what's possible. You're gassed and ready to bet the farm on it. Then boom, next week, that model looks like child's play compared to a new competing model. The same is happening with AI-native IDEs and coding agents. This has been going on pretty much all of 2025. There is no clear winner yet. It would be foolish to lock in to any specific tool or model.


So, we buy them all. Well, not *all* , but a lot! We let our engineers freely explore the frontier models and tools to see what works well for them. Of course, we'd like to standardize on a couple of tools eventually but it would be a mistake to do so right now when the industry is still evolving at lightning speed.


In doing this, we've discovered that not all codebases are equal in the eyes of the AI. For example, our mobile and JVM backend devs have quite the challenge with the frontier models and tools. What works for web developers doesn't necessarily work as well for them.


## 2. Launch a Champions program


In August of 2025, I launched an Engineering AI Champions program with 50 developers from different teams who dedicated 30% of their time to investing in AI enablement efforts at the repo level. These were resilient engineers who, when the AI hallucinated or produced slop, leaned in to figure out why it failed and did the engineering work to make it succeed.


The program reached almost every corner of Block Engineering: Square, Cash App, Afterpay, Tidal, Proto, and Platform teams, covering everything from frontend to backend to mobile, hardware, data engineering, and infra. The mix of repo sizes was just as wide, from massive monorepos to tight, focused services and mobile apps. This diversity meant that instead of testing AI workflows in one environment, we pressure tested patterns across very different engineering realities, which gave us a clearer sense of what scales and what breaks.


### Repo Readiness


The AI Champions first focused on embedding AI into their repos so that agents are able to discover and consume their codebase, and be trusted to contribute and maintain it. We focused on repos because they are a central point of reference for every engineer contributing code, and while every team and repository is different, solving these steps for each repo enables the team to leverage AI in a way that makes sense for them.


This work is not exactly exciting, so we gamified it to add a bit of whimsy. Our internal Developer Relations team developed Repo Quest, an RPG-style game where developers complete quests to collect companions.


Loading image...


**Repo Quest Challenges**


Champions progressed through four tiers ( **Locked → Novice → Adept → Artisan** ) unlocking companions along the way as they completed quests. Here's the gist of what they worked through:


**Set up AI context files (Locked → Novice):**


- Create an[AGENTS.md](https://agents.md/) file giving AI agents direct instructions about the codebase including build/test commands, code style conventions, and architecture patterns.
- Add a` HOWTOAI.md` as a human-facing guide for your team on using AI tools effectively in the repo. Include setup instructions, tips, and example workflows.[\[Example\]](https://github.com/block/goose/blob/main/HOWTOAI.md)


**Level up workflows (Novice → Adept):**


- Create repeatable[agent skills](https://agentskills.io/home) as packaged, reusable workflows for common tasks like generating API tests, refactoring legacy code, or creating documentation. These can be shared across teams.
- Enable automated AI code review on PRs with customizable focus areas.
- Set up headless AI assistance that can autonomously iterate on tasks, fix CI failures, and push changes.


**Make it measurable and share artifacts (Adept → Artisan):**


- Add PR labels or indicators to track when AI significantly contributed to the work. Transparency helps measure what's actually working.
- Wire AI into the CI/CD pipeline for automated analysis during builds, and use AI tools to make incident response and postmortems smoother.
- Contribute proven recipes and workflows back to shared libraries so other teams can build on what works.


As an example, here's the AI-readiness architecture of one of our largest monorepos with 40,000+ source files, 30,000+ test files, 650+ deployable services, 5,800+ endpoints, and thousands of contributors:


```text
1  ┌─────────────────────────────────────────────────────────────────────────────────────┐
2  │                              AI CODING ASSISTANTS                                   │
3  │         Claude Code • Goose • Cursor • Copilot • Cline • AMP • Firebender           │
4  └─────────────────────────────────────────────────────────────────────────────────────┘
5                                           │
6                                           ▼
7                                     reads context
8                                           │
9  ┌─────────────────────────────────────────────────────────────────────────────────────┐
10  │  ROOT LEVEL                                                        monorepo-wide    │
11  ├─────────────────────────────────────────────────────────────────────────────────────┤
12  │                                                                                     │
13  │   CLAUDE.md ──────────────┐                                                         │
14  │   AGENTS.md ──────────────┼──▶ @ai-rules/.generated-ai-rules/*                      │
15  │   .cursor/rules/*.mdc ────┘                                                         │
16  │                                                                                     │
17  │   ai-rules/                                                                         │
18  │   ├── index.md                 # Core guidelines, build commands, citations         │
19  │   ├── framework-overview.md    # Framework documentation                            │
20  │   └── ai-usage-tracking.md     # Co-authored-by attribution for commits             │
21  │                                                                                     │
22  └─────────────────────────────────────────────────────────────────────────────────────┘
23                                           │
24                          ┌────────────────┼────────────────┐
25                          ▼                ▼                ▼
26                           650+ services inherit root context
27                          │                │                │
28  ┌───────────────────────┴──┐ ┌───────────┴───────────┐ ┌──┴───────────────────────────┐
29  │  SERVICE LEVEL           │ │  SERVICE LEVEL        │ │  SERVICE LEVEL               │
30  │  service-1/              │ │  service-2/           │ │  service-N/                  │
31  ├──────────────────────────┤ ├───────────────────────┤ ├──────────────────────────────┤
32  │                          │ │                       │ │                              │
33  │  CLAUDE.md               │ │  CLAUDE.md            │ │  CLAUDE.md                   │
34  │  ai-rules/               │ │  .goosehints          │ │  .goosehints                 │
35  │  ├── index.md            │ │                       │ │  .cursor/rules/              │
36  │  ├── testing.md          │ │  - Service purpose    │ │  ├── kafka-patterns.mdc      │
37  │  ├── migrations.md       │ │  - Architecture       │ │  ├── dynamodb-patterns.mdc   │
38  │  ├── api-design.md       │ │  - Key components     │ │  └── testing-patterns.mdc    │
39  │  └── data-access.md      │ │  - Code conventions   │ │                              │
40  │                          │ │                       │ │                              │
41  │  Step-by-step workflows  │ │  Domain-specific      │ │  Technology-specific         │
42  │  with checklists         │ │  context              │ │  patterns                    │
43  │                          │ │                       │ │                              │
44  └──────────────────────────┘ └───────────────────────┘ └──────────────────────────────┘
45                                                                        │
46                                                                        ▼
47                                                         ┌──────────────────────────────┐
48                                                         │  SUBDIRECTORY LEVEL          │
49                                                         │  service-N/src/.../          │
50                                                         ├──────────────────────────────┤
51                                                         │                              │
52                                                         │  store/.goosehints           │
53                                                         │    └── Database patterns     │
54                                                         │                              │
55                                                         │  feeds/.goosehints           │
56                                                         │    └── Event stream rules    │
57                                                         │                              │
58                                                         │  action/.goosehints          │
59                                                         │    └── API handler patterns  │
60                                                         │                              │
61                                                         └──────────────────────────────┘
62
63  ┌─────────────────────────────────────────────────────────────────────────────────────┐
64  │  AUTOMATION (uses https://github.com/block/ai-rules)                                │
65  ├─────────────────────────────────────────────────────────────────────────────────────┤
66  │                                                                                     │
67  │   $ ai-rules init                                                                   │
68  │                                                                                     │
69  │   Analyzes codebase → Generates ai-rules/index.md → Creates agent-specific configs  │
70  │                                                                                     │
71  │   Outputs: CLAUDE.md, AGENTS.md, .cursor/rules/, .clinerules/, firebender.json      │
72  │                                                                                     │
73  └─────────────────────────────────────────────────────────────────────────────────────┘
```


This shows how we deliberately structure repos so AI agents can understand, navigate, and contribute at scale. This kind of setup makes AI a predictable collaborator by giving it clear, scoped instructions, automation to keep those rules consistent, and just enough local context to do real work without constant correction.


This allowed most engineers to comfortably use AI-assisted tools without much training, because the Champions had already laid the foundation at the repo level.


### Context Engineering


Getting agents to produce quality work at scale required more than just AI-ready repos and good prompts. It required structured approaches to how we fed them context, especially in our large codebases.


We adopted[RPI](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md) as our context engineering approach. RPI (Research → Plan → Implement) is a technique introduced by HumanLayer that's designed for work that's too complex for standard prompting. It has the agent first research the relevant parts of the codebase and document its findings, then starts a new session with fresh context where the agent reads that research and documents a detailed plan for execution, then a final session where the agent implements the plan.


AI Champions used RPI for everything from UI revamps to major upgrades, language migrations, and large localization projects. It gave teams a consistent way to get[high quality PRs](https://block.github.io/goose/docs/tutorials/rpi/#final-result) out of agents and prevented the usual AI drift.


### Automated PRs


Champions became so comfortable with using AI that they began embedding agents into their sprints and assigning them tickets that were well understood and low complexity... directly from Linear and Jira. The agent reads the ticket, makes a plan, creates the branch, writes the code, opens the pull request, watches CI fail, and fixes its own mistakes. Humans only step in at review time. Teams even reported pulling in more tickets mid-sprint because their agent had already completed 15 days of engineering work. A good problem to have.


### Impact of AI Champions Program


Within three months of launching the Champions program, AI-authored code jumped by 69%, reported time savings increased 37%, and automated PRs increased 21×.


## 3. Training


Training was another major unlock. AI Champions, along with Developer Relations, didn't just learn these tools, they taught them to all of Engineering. They ran brownbags, demos, agent battles, and office hours. They built onboarding kits, repo templates, agent rules and skills, prompt libraries,[MCP servers](https://engineering.block.xyz/blog/blocks-playbook-for-designing-mcp-servers) , and more.


But the real magic was in the knowledge transfer between peers. When an engineer on your team shows you how they used an agent to knock out a tedious migration in an afternoon, it hits different than a generic tutorial. Champions became the local experts on their teams, the goto person that engineers could turn to when they got stuck, when the AI produced garbage, or when they weren't sure if a task was even worth trying with an agent. They also shielded their team from AI fatigue by being the guinea pig for new tools and agentic patterns, but only bringing forth the ones that actually work.


## What's Next


Even with broad training, there's now a noticeable gap between the AI Champions and the rest of Engineering. The training exposed people to what was possible, but it wasn't enough to get them to the next level. Watching a demo is one thing, rewiring how you work is another.


This year, we want to close that gap. By the end of the year, we'd like most of the Engineering org comfortably using orchestrators to manage multiple agents in parallel to increase developer velocity.


Broad teachings won't get us there. We're now focused on team-based workshops that go deep on preparing repos for AI, context engineering, and multi-agent workflows. Instead of showing people what's possible, DevRel is pairing up with teams to apply these techniques to their actual codebase and their actual work.


We're also[investing in the next generation](https://block.xyz/builder-fellowship) : early career builders who are AI-native and comfortable working with agents. We're bringing them in not just to learn from us, but for us to learn from them. They don't carry the muscle memory of pre-AI development, and that fresh perspective is invaluable as we figure out what modern software engineering actually looks like.
