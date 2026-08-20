---
schema_version: "1.0.0"
document_id: "1814beb68624f160bf682d3c62e543220f5cf9a8ded72ae7ee23725b6df85f72"
company_key: "yc-taskade"
company: "Taskade"
source_id: "yc-taskade-rss-a662ed9a0141"
canonical_url: "https://www.taskade.com/blog/claude-code-skills"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:13.456142+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:787fe57d9309391936b0d9f614478b103afc39963cd3e689bb441851c10a1740"
---

# Best Claude Code Skills in 2026 (Tested + How to Build Your Own)

[Blog](https://www.taskade.com/blog)


[AI](https://www.taskade.com/blog/ai)


Best Claude Code Skills in…


On this page (25)


Claude Code skills went from a niche announcement to the main way people customize their coding agent in under a year. Anthropic launched Agent Skills as an open standard in October 2025, and by mid-2026 the ecosystem around[Claude Code](https://code.claude.com/docs/en/skills) includes official skill repositories with 157,000+ GitHub stars, community frameworks with 243,000+, and plugin marketplaces on every corner. The problem: the top guides are either pure reference docs, vendor listicles, or paywalled posts. This guide unifies all three intents in one free page — what skills are, which 12 are actually worth installing (star counts checked live on July 1, 2026), and how to build your own. If you want the conceptual, beginner-first explainer instead, read the sibling guide,[what Claude Skills are](https://www.taskade.com/blog/claude-skills-explained) — this page is the ranked list.


> **TL;DR:** The best Claude Code skills in 2026 are[Superpowers](https://github.com/obra/superpowers) (243,000+ GitHub stars), Anthropic's official[skills repo](https://github.com/anthropics/skills) (Skill Creator + document skills),[gstack](https://github.com/garrytan/gstack) , and[GSD](https://github.com/gsd-build/get-shit-done) . A skill is a folder with a SKILL.md that loads on demand — and non-developers get the same repeatable-workflow power with[Taskade custom agents](https://www.taskade.com/agents) .[Build a no-code agent free →](https://www.taskade.com/agents)


## What Are Claude Code Skills? (Quick Answer)


A Claude Code skill is **a folder containing a` SKILL.md` file — instructions, plus optional scripts and reference files — that the agent loads on demand when a task matches the skill's description** . At startup, Claude Code sees only each skill's name and description (tens of tokens per skill). The full instructions load only when relevant. That mechanism, called progressive disclosure, is why you can install dozens of skills without bloating your context window.


Quick answers to the questions people actually search:


Question Quick answer


What is a skill? A folder with a` SKILL.md` the agent loads when a task matches


Where do skills live?` ~/.claude/skills` (personal) or` .claude/skills` (project, git-shared)


What do they cost in context? Name + description only (tens of tokens) until triggered


Best ones to install first? Superpowers, official anthropics/skills, gstack, GSD


Can I build my own? Yes — one markdown file; the Skill Creator skill writes it for you


Skills matter because they solve the memory problem every[AI agent](https://www.taskade.com/blog/what-are-ai-agents) has: you explain a procedure perfectly once, and the next session starts from zero. A skill makes the explanation permanent, versionable, and shareable. Anthropic reports that around 80% of its own staff use Claude Code daily (September 2025), and the internal pattern that emerged — encode every repeated procedure as a file the agent can find — is exactly what the skills system productized. Skills are one layer of a larger stack; the[agent harness](https://www.taskade.com/blog/agent-harness-explained) and the full[AI agent stack](https://www.taskade.com/blog/ai-agent-stack) explain where they sit.


## Inside a Skill: The Annotated SKILL.md


Every skill reduces to **one required file with two required frontmatter fields —` name` and` description` — followed by plain-markdown instructions** . Here is a complete, working example with the parts annotated. Notice it is a business workflow, not code — skills are procedure files, and any procedure qualifies:


Markdown


```text
---
name: weekly-report
description: >
Compile the weekly status report. Use when the user asks for a
"weekly report", "status update", or "what shipped this week".
Reads completed tasks from the tracker export, groups them by
project, and writes the summary in report-format.md style.
--- Weekly Report
Steps


Locate this week's task export ( exports/tasks-*.csv  ); ask if missing.
Group completed items by project. Flag anything overdue in its own list.
Draft the report following the exact structure in  report-format.md  .
Keep it under 400 words. Bullets, no filler adjectives.


Rules


Never invent numbers. If a metric is missing, write "not tracked".
Dates in ISO format (2026-07-01).


Bundled files (load only if the skill runs)


report-format.md   <- the output template
scripts/rollup.py  <- deterministic math, run this instead of estimating
```


The highest-leverage line is the` description` , and the best practice almost no roundup teaches: **write the description as a routing rule, not a summary** . It is the only part of the skill the model sees at startup, so it must answer one question — *when should this fire?* Include trigger phrases users actually say ("weekly report", "status update"), name the inputs it expects, and state what it produces. A vague description ("Helps with reports") means the skill never fires at the right moment; a routing-rule description makes progressive disclosure work. Anthropic's guidance adds two more rules: keep the body under roughly 500 lines, and push anything deterministic (math, file renaming, API calls) into scripts so the model follows procedure instead of improvising.


Where the folders actually go:


```text
WHERE CLAUDE CODE SKILLS LIVE
─────────────────────────────────────────────────────────────
~/.claude/skills/                <- PERSONAL: follows you into
├── weekly-report/                  every project on this machine
│   └── SKILL.md
└── reflect/
└── SKILL.md your-project/  ├── .claude/skills/              <- PROJECT: committed to git,  │   ├── deploy-checklist/           the whole team gets it on pull  │   │   ├── SKILL.md  │   │   └── reference.md  │   └── release-notes/  │       └── SKILL.md  └── CLAUDE.md                    <- NOT a skill: always-loaded rules
/plugin marketplace add <repo>   <- PLUGINS: skills can also arrive                                      bundled from a marketplace
```


## The 12 Best Claude Code Skills in 2026


The strongest skill to install first in 2026 is **Superpowers** , followed by Anthropic's official skills repository — those two cover engineering discipline and document work, the two highest-frequency use cases. Every star count below was checked live against the GitHub API on July 1, 2026, because skill roundups are notorious for repeating stale or inflated numbers.


# Skill Best for Source (stars, Jul 2026)


1 Superpowers Engineering discipline end to end obra/superpowers (243K+)


2 Skill Creator Building your own skills anthropics/skills (157K+)


3 gstack A full team-in-a-box process garrytan/gstack (118K+)


4 GSD Spec-driven context engineering gsd-build/get-shit-done (64K+)


5 Document skills PDF, Word, Excel, PowerPoint anthropics/skills (157K+)


6 frontend-design Non-generic UI direction Official plugin marketplace


7 mcp-builder Scaffolding MCP servers anthropics/skills


8 webapp-testing Browser-driven app QA anthropics/skills


9 brand-guidelines On-brand output for teams anthropics/skills


10 A reflect skill Continual learning from corrections Build your own


11 /review Built-in code review Bundled with Claude Code


12 /deep-research Cited research (a workflow, not a skill) Bundled, v2.1.154+


### 1. Superpowers — best overall skill framework


[Superpowers](https://github.com/obra/superpowers) is a full software-development methodology packaged as composable skills: plan-first design, test-driven development (the RED-GREEN-REFACTOR loop), systematic debugging with root-cause analysis, git worktree management, subagent-driven development, and verification before completion. At 243,000+ GitHub stars (checked July 1, 2026), it is the most-adopted community skill framework by a wide margin. **When to use it:** you want Claude Code to stop jumping straight into code and start behaving like a disciplined senior engineer on every task. It also publishes install paths for[Cursor](https://cursor.com/) ,[Codex](https://openai.com/codex/) , and other agents, which makes it a safe investment if you switch harnesses.


Bash


```text
/plugin install superpowers@claude-plugins-official
# or: /plugin marketplace add obra/superpowers-marketplace


```


Honest note: it is opinionated. If you disagree with test-first development, you will be fighting your own skills.


### 2. Skill Creator — the official meta-skill


Skill Creator, part of Anthropic's official[skills repository](https://github.com/anthropics/skills) (157,000+ stars), is a skill that writes skills. Describe a procedure in plain language — or paste an existing SOP — and it generates the folder, the routing-rule description, and the structured body following Anthropic's own authoring best practices. **When to use it:** every time you catch yourself explaining the same procedure twice. It is the fastest way to internalize what good skill structure looks like, because it shows you rather than telling you. The same repo ships the Agent Skills specification and a bare template if you prefer writing by hand.


Bash


```text
/plugin marketplace add anthropics/skills
/plugin install example-skills@anthropic-agent-skills


```


Honest note: generated skills still need a human editing pass — tighten the description and delete boilerplate.


### 3. gstack — the YC CEO's software factory


[gstack](https://github.com/garrytan/gstack) (118,000+ stars) is Garry Tan's open-source Claude Code setup — the repo describes it as "23 opinionated tools that serve as CEO, Designer, Eng Manager, Release Manager, Doc Engineer, and QA." It organizes work into a Think → Plan → Build → Review → Test → Ship → Reflect loop, with role-playing skills like /office-hours (YC-style forcing questions), /plan-ceo-review (built on Brian Chesky's 10-star framework), and a security review that runs OWASP-style analysis. Tan claims part-time output of 10,000–20,000 lines of production code per day with it — his own numbers, no independent audit. **When to use it:** solo builders who want an entire org chart's worth of process pressure-testing their work.


Bash


```text
git clone https://github.com/garrytan/gstack
# then follow the README to link its commands into your setup


```


Honest note: it encodes one founder's taste. Adopt the loop, prune the roles you do not need.


### 4. GSD (Get Shit Done) — spec-driven context engineering


[GSD](https://github.com/gsd-build/get-shit-done) (64,000+ stars) is a lightweight meta-prompting and spec-driven development system by TÂCHES. The core idea, per its documentation and fans: break work into specs, then give each task a fresh subagent with a clean context so quality never degrades as a session ages — with checks that catch scope quietly getting dropped between plan and implementation. **When to use it:** long multi-task sessions where you have noticed the 15th task getting sloppier treatment than the 1st. It pairs the "uncorrelated context windows" insight from the Claude Code team itself with an enforceable process, and it is much lighter to adopt than Superpowers or gstack.


Bash


```text
# install per the README at github.com/gsd-build/get-shit-done


```


Honest note: the spec-first ceremony is overkill for quick one-file fixes — keep it for real features.


### 5. The official document skills (pdf, docx, xlsx, pptx)


The document skills in[anthropics/skills](https://github.com/anthropics/skills) are the source-available versions of what Claude uses in production for document work: filling and extracting PDFs, generating Word documents, building Excel spreadsheets with real formulas, and assembling PowerPoint decks. **When to use them:** any workflow that ends in a file a colleague opens in Office. This is the entry point for non-developer work in Claude Code — a recruiter generating offer letters or an analyst turning CSVs into formatted workbooks gets more from these four skills than from any coding framework on this list. They are also the best reference reading for skill authors: production-grade SKILL.md files you can study.


Bash


```text
/plugin marketplace add anthropics/skills
/plugin install document-skills@anthropic-agent-skills


```


Honest note: complex layouts (pixel-perfect slide design) still need human polish afterward.


### 6. frontend-design — Anthropic's design-taste skill


frontend-design is Anthropic's official skill for building distinctive interfaces instead of the same rounded-corner, purple-gradient page every AI produces. It pushes deliberate aesthetic direction — typography choices, layout intent, avoiding templated defaults — before any component gets written. **When to use it:** every greenfield UI task, especially if you have caught your agent producing what the community calls "AI slop design." Install it from Claude Code's official plugin marketplace (run` /plugin` and search the official listing). It is a good demonstration that skills can encode *taste* , not just procedure — the file is mostly design principles, and output quality visibly shifts with it enabled. Builders comparing agents on design quality consistently put Claude ahead on front-end work; this skill is part of why.


Honest note: taste skills constrain. For strict design-system work, your own brand skill should override it.


### 7. mcp-builder — scaffold MCP servers correctly


mcp-builder, also from the official anthropics/skills repo, scaffolds[Model Context Protocol](https://www.taskade.com/blog/mcp-servers) servers — the standard way to connect agents to external systems. It encodes the spec details people get wrong by hand: tool schema definitions, error shapes, transport setup. **When to use it:** you want Claude Code (or any agent) to reach an internal API, database, or SaaS tool that has no existing connector, and you would rather generate a correct server in minutes than study the protocol for a day. It pairs naturally with the skills-vs-MCP framework below: build the connection with mcp-builder, then write a small skill that teaches the agent when and how to use it. Our guide to[connecting Claude and Cursor via MCP](https://www.taskade.com/blog/connect-claude-cursor-mcp) covers the client side.


Honest note: generated servers need a security pass before touching production credentials.


### 8. webapp-testing — browser QA without a test suite


webapp-testing (official repo) drives a real browser against your running web app: click flows, form fills, screenshot checks, console-error sweeps. **When to use it:** verifying that a change actually works in the app, not just in the diff — the step most agent sessions skip. It converts "looks right to me" into observed behavior, which matters because agents are systematically overconfident about their own output; Anthropic notes its newest models were trained specifically to stop letting flaws in their own code "pass unremarked." For teams without a Playwright suite, this skill is the fastest path to any automated verification at all, and for teams with one, it is a cheap pre-commit smoke check.


Honest note: browser runs are slow and token-hungry compared to unit tests — use it at milestones, not on every edit.


### 9. brand-guidelines — the non-developer sleeper pick


brand-guidelines (official repo, enterprise/communications category) keeps generated output — docs, decks, emails, web copy — consistent with a defined brand voice and visual identity. You customize it once with your brand's rules, and every document skill downstream inherits them. **When to use it:** any team where more than one person generates customer-facing artifacts with AI. This is the category almost every "best skills" roundup ignores: marketing, ops, and docs teams are heavier repeat-procedure users than most engineers, and a brand skill plus the document skills is a complete non-developer workflow. It is also the clearest illustration that a skill is just an SOP in a folder — most companies already have this document; it is called a style guide.


Honest note: it enforces consistency, not quality — a weak style guide becomes a consistently weak output.


### 10. A reflect skill — continual learning from your corrections


The reflect pattern — endorsed in Anthropic's own large-codebase guidance and spread widely through the community — is a skill you build (Skill Creator can write it): after a session, it scans the transcript for the corrections you made, proposes updates to the relevant skill files with confidence levels, and commits approved changes to git. The result is the closest thing agents have to memory that compounds: **correct once, and then never again** . Anyscale CEO Robert Nishihara frames skills as exactly this — continual learning without touching model weights, in plain text humans can read and edit. **When to use it:** from week two onward, once you have real skills accumulating corrections. Our deep dive on[self-improving agents](https://www.taskade.com/blog/self-improving-ai-agents-reflection) covers the pattern in full.


Honest note: review every proposed update — an unsupervised reflect loop can happily encode your bad day into policy.


### 11. /review — the built-in you already have


/review is Claude Code's bundled code-review skill: point it at a diff, branch, or PR and it produces a structured review. **When to use it:** before every merge — it is free, local, and catches the mechanical mistakes (unused imports, missed null checks, inconsistent naming) that waste human reviewer attention. Community reports describe a heavyweight cloud sibling, /ultra review, that uploads a branch to a sandbox and runs a fleet of parallel reviewers that must reproduce a bug before reporting it, with a few free runs on paid plans and per-run pricing after — worth testing if your PRs are large, though details there are creator-reported rather than official. Start with plain /review; it is the highest-value zero-setup item on this list, and a good complement to the verification habits Superpowers builds in.


Honest note: agent review augments human review, it does not replace judgment about design and intent.


### 12. /deep-research — great, but it is not a skill


/deep-research earns its slot in every roundup with an asterisk: **it is officially a dynamic workflow, not a skill** . Dynamic workflows shipped in Claude Code v2.1.154 on May 28, 2026 — Claude writes a real orchestration script, and a background runtime executes it across many agents (up to 16 concurrent, 1,000 per run). /deep-research runs five stages: scope, parallel searches, source fetching with claim extraction, adversarial verification where independent agents try to refute each claim, and cited synthesis. One documented run used 105 agents, about 3.1 million tokens, and roughly 15 minutes. **When to use it:** questions where being wrong is expensive. The distinction matters for your wallet: a skill costs tens of tokens until triggered; a workflow can orchestrate a small army. More on the pattern family in our guide to[agentic workflows](https://www.taskade.com/blog/agentic-workflows-explained) .


Honest note: for casual questions, a single search is 100x cheaper — reserve workflows for claims you need verified.


**Honorable mentions, flagged honestly:** Context Mode (compresses noisy tool output; its 56 KB-snapshot-to-299-bytes numbers are self-published), ClaudeMem (cross-session memory claiming ~10x retrieval savings, also vendor-reported), and the[awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) directory (~14,000 stars) for discovery. All three come from sources with something to sell — try them, but trust your own measurements.


## Skills vs. MCP vs. Subagents vs. CLAUDE.md vs. Slash Commands


These five primitives answer **five different questions, and picking the wrong one is the most common Claude Code setup mistake** . Skills teach procedures. CLAUDE.md holds always-on conventions. MCP connects external systems. Subagents isolate big jobs in separate context windows. And slash commands, per Claude Code's own docs, merged into skills — a command is now just a skill you invoke explicitly instead of letting Claude discover it.


Primitive What it is When it loads Best for


Skill On-demand procedure folder When a task matches its description Repeatable workflows


Slash command A skill invoked by name When you type it Procedures you want on demand, not auto


CLAUDE.md Always-on rules file Every turn Conventions Claude must never forget


MCP server Connection to external systems When its tools are called Reaching databases, browsers, SaaS


Subagent Separate context window When delegated Big tasks that would pollute your context


Two rules of thumb from Anthropic's large-codebase guidance make the split easy in practice. First: *rules are conventions, skills are workflows* — "always use yarn" is CLAUDE.md, "how we cut a release" is a skill. Second: keep CLAUDE.md short and human-written. The nearest research agrees — an ETH Zurich study of agent context files (arXiv 2602.11988, February 2026) found developer-written files improved agent performance about +4% while auto-generated, README-duplicating ones slightly *hurt* (−3%). Bloat in the always-loaded layer is the expensive kind. For the deeper architecture of how these layers stack, see[what agentic engineering is](https://www.taskade.com/blog/what-is-agentic-engineering) and our[AI agent stack](https://www.taskade.com/blog/ai-agent-stack) flagship.


## How to Build Your Own Skill in 5 Steps


Your first custom skill should take **under ten minutes, and the procedure you already repeat most often is the right candidate** :


1. **Create the folder.**` ~/.claude/skills/my-skill/SKILL.md` for personal use,` .claude/skills/` in the repo if the team should get it. (Or ask Skill Creator to do all of this from a description.)
2. **Write the description as a routing rule.** Trigger phrases, expected inputs, produced outputs — this is the only line Claude sees before deciding to load the skill.
3. **Write the body as steps and rules, not prose.** Numbered steps, explicit constraints, exact file paths. Stay under ~500 lines; split long reference material into separate files the skill mentions.
4. **Push deterministic work into scripts.** Anything with one correct answer — math, renames, API calls — belongs in a bundled script the skill runs, not in instructions the model interprets.
5. **Test, then let it learn.** Ask for a matching task and watch whether the skill fires. If not, sharpen the description. Then commit the folder to git and add a reflect step so corrections keep improving it.


The git commit in step five is not bureaucracy — it is the point. A skills directory under version control is a readable, diffable history of everything your agent has learned, which is a form of[agent knowledge management](https://www.taskade.com/blog/ai-agent-knowledge-files) that outlives any single tool.


## Skills as SOPs: The Business Angle Nobody Covers


The most underrated framing of skills comes from the business side: **a skill is a standard operating procedure the AI can execute — "SOPs as code."** Nate, an AI-automation entrepreneur who says he sold a $100K/month agency, teaches exactly this move in his AI-operating-system framework: take an existing SOP document and tell Claude, "turn this into a skill." His AIOS is built on four C's, in strict order — Context (what the AI knows about you), Connections (what data it reaches), Capabilities (what it produces), and Cadence (when it acts on its own) — and he emphasizes the durable layer is the files, not the harness: he reports porting his whole skills directory from Claude Code to Codex in about two minutes. That is the deeper lesson of this entire list: the value you build is a folder of procedures, not a subscription.


But notice what his course assumes: a terminal, VS Code, git, and API keys for every tool. That is a real barrier — an entire audience is effectively being told to become sysadmins to get repeatable AI workflows. If that is you (or your ops, marketing, or docs team), the honest answer is that you do not need the terminal to get the outcome.[Taskade](https://www.taskade.com/agents) custom agents cover the same four C's without any of that stack: persistent workspace memory is the Context,[100+ bidirectional integrations](https://www.taskade.com/automate) are the Connections, agents with 34 built-in tools and custom slash commands are the Capabilities (a saved command is functionally a skill — a procedure defined once, reused forever), and scheduled[automations](https://www.taskade.com/automate) are the Cadence. Taskade Genesis goes a step further and turns procedures into[full working apps](https://www.taskade.com/ai/apps) your team or clients can use, and the[community gallery](https://www.taskade.com/community) has ready-made examples to clone. The fair split: for heavy code work in a real repository, Claude Code is the right tool and this list is your setup guide. For business workflows that need to run reliably without anyone opening a terminal, a hosted[AI workspace](https://www.taskade.com/blog/ai-sop-software) is the same idea with the ops removed — the conceptual walkthrough in[our Claude Skills explainer](https://www.taskade.com/blog/claude-skills-explained) maps the two side by side, and the[execution layer thesis](https://www.taskade.com/blog/execution-layer-thesis) explains why this convergence is happening everywhere.


## Do Skills Actually Save Tokens? An Honest Look at the Numbers


Here is the uncomfortable truth this SERP avoids: **almost no published skill-efficiency number is a measured, independent benchmark** . The design is genuinely economical — only name and description load until a skill fires, which is documented by Anthropic. But the headline percentages floating around skill roundups mostly belong to other features or to vendors. Sort the claims:


Claim Source Status


Skill metadata costs "tens of tokens" until triggered Anthropic skill docs Design fact — small and real


77,000 → 8,700 tokens (−85%) context cut Anthropic's Tool Search Tool data Real — but about tool search, **not** skills


56 KB browser snapshot → 299 bytes Context Mode (self-published) Vendor benchmark, unaudited


~10x retrieval token savings ClaudeMem (self-published) Vendor benchmark, unaudited


Agents ≈ 4x chat tokens; multi-agent ≈ 15x Anthropic engineering blog Anthropic's own engineering estimate


Developer-written context files ≈ +4% success ETH Zurich, arXiv 2602.11988 Peer research — on context files, not skills


The 85% figure deserves special mention because it is the most misquoted stat in this niche: it comes from Anthropic's Tool Search Tool (loading tool definitions on demand), and it routinely gets attributed to Agent Skills because both use the same load-on-demand philosophy. Same idea, different feature, different measurement. The practical takeaway: skills will not blow up your context, badly-written always-on files will, and the only numbers you should fully trust are your own — run` /context` before and after installing a skill set, and check` /cost` on a real week of work. If a skill's value does not show up in your output quality or your token bill, uninstall it. Curation beats collection.


## FAQ: Best Claude Code Skills


What are Claude Code skills?


Folders containing a` SKILL.md` — instructions plus optional scripts and reference files — that Claude Code loads on demand when a task matches the description. Launched by Anthropic as an open standard in October 2025. Only the name and description occupy context until the skill fires.


Where do Claude Code skills live?


` ~/.claude/skills/` for personal skills that follow you everywhere;` .claude/skills/` inside a repo for project skills the whole team shares via git; and plugins can bundle skills installed from marketplaces.


Which Claude Code skills should I install first?


Superpowers (243,000+ stars, checked July 2026) for engineering discipline, then the official anthropics/skills repo for Skill Creator and the document skills. Add gstack or GSD if you want more process, and build a reflect skill in week two.


Are skills the same as slash commands?


Effectively yes, now: custom slash commands were merged into the skills system. A slash command is a skill you invoke explicitly by name; a regular skill is one Claude triggers itself when the description matches the task.


What is the difference between skills and MCP?


Skills are procedural knowledge (how to do a task);[MCP](https://www.taskade.com/blog/mcp-servers) is the connection layer (access to external systems like databases and browsers). They compose: a skill can describe a procedure whose steps call MCP tools.


How is a skill different from CLAUDE.md?


CLAUDE.md loads on every turn and should hold only always-true conventions. A skill loads on demand and holds a workflow. Rules go in CLAUDE.md; procedures become skills.


Do skills use extra tokens or slow Claude Code down?


Barely, by design — tens of tokens per skill until one triggers. Most dramatic savings claims are either about other features (the famous −85% belongs to Anthropic's Tool Search Tool) or vendor self-reported. Measure with` /context` and` /cost` .


Do Claude Code skills work with Codex, Cursor, and other agents?


Mostly, yes — Agent Skills is an open standard, and major frameworks like Superpowers ship installs for multiple agents. One practitioner reports porting his full skills directory from Claude Code to Codex in about two minutes; Cursor supports the format too. See our[Claude Code alternatives](https://www.taskade.com/blog/claude-code-alternatives) guide for the landscape.


Is /deep-research a skill?


No — it is a dynamic workflow (shipped May 28, 2026, in v2.1.154): Claude writes an orchestration script that coordinates up to hundreds of agents. It behaves like a command but costs like a fleet; one documented run used 105 agents and ~3.1M tokens.


How do I share skills with my team?


Commit` .claude/skills/` to the repository, or package skills as a plugin on a marketplace. Since October 2025, organizations can also manage and distribute skills company-wide.


What if I want this power without a terminal?


Use a hosted agent platform.[Taskade](https://www.taskade.com/agents) custom agents store repeatable procedures as slash commands with 34 built-in tools and persistent memory, automations run them on triggers across 100+ integrations, and plans start free (Pro $10, Business $25, Max $100, Enterprise $250 per month, billed annually — see[pricing](https://www.taskade.com/pricing) ).[Try Taskade free →](https://www.taskade.com/create)


## Related Reading


- [What Are Claude Skills? Beginner Guide](https://www.taskade.com/blog/claude-skills-explained) — the conceptual sibling of this list
- [The History of Claude Code](https://www.taskade.com/blog/claude-code-history) — from weekend project to $2.5B run rate
- [Claude Code vs OpenClaw](https://www.taskade.com/blog/claude-code-vs-openclaw) and[Claude Code vs n8n](https://www.taskade.com/blog/claude-code-vs-n8n)
- [Best AI Agents in 2026](https://www.taskade.com/blog/best-ai-agents) and[Best Claude Cowork Alternatives](https://www.taskade.com/blog/claude-cowork-alternatives)
- [Anthropic's History](https://www.taskade.com/blog/anthropic-claude-history) and the[open-source LLM landscape](https://www.taskade.com/blog/open-source-llms)
- Comparisons:[Codex vs Claude Code](https://www.taskade.com/compare/codex-vs-claude-code) -[Free Claude Code alternative](https://www.taskade.com/compare/free-claude-code-alternative)


Skills are the clearest signal yet of where all agent tooling is heading: knowledge that lives in readable files, procedures that compound instead of evaporating, and execution that runs without you babysitting it. Install the two or three from this list that match your work, build one skill of your own this week, and put it in version control — that folder will outlast every tool on this page. And when you want the same compounding loop for the non-code half of your work — memory that persists, agents that act, automations that run —[create your first custom agent free](https://www.taskade.com/create) in Taskade. ▲ ■ ●
