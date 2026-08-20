---
schema_version: "1.0.0"
document_id: "a88b69cde2b395b7cfa77b471dacc7600cd380d546f08226f362e559135e8688"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/best-opencode-skills"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-24T08:10:35.258497+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:72f4df49efe9d93defe2021823906cdeeb7ed25e61b79c91431de2073840c8f1"
---

# 10 OpenCode Skills Worth Installing in 2026

## TL;DR: Best OpenCode Skills


Skill What it does


Firecrawl Gives OpenCode live web context: search, scrape, crawl, and browser automation


stop-slop Strips AI writing tells from any prose the agent generates: em dashes, jargon, throat-clearing, rhetorical setups


Handoff Compresses a session into a markdown doc so you can continue in a fresh session or hand off to a different agent


Grill Me Interviews you relentlessly about a plan until shared understanding is reached, before any code is written


Obra Superpowers The most complete multi-agent development framework: brainstorming, worktrees, TDD, subagents, systematic debugging


Understand-Anything Turns any codebase into an interactive knowledge graph with fuzzy search, semantic search, and guided tours


Caveman Cuts output tokens by 65% on average by stripping narration while keeping every technical fact intact


skill-optimizer A three-skill toolkit that mines your session history for skill-worthy workflows and personalizes installed skills


Vault Daydream Samples random pairs of notes from an Obsidian vault, dispatches parallel subagents to find non-obvious connections


Composio Teaches the agent how to use Composio's 1000+ SaaS integrations (GitHub, Linear, Slack, Stripe) via MCP, native tools, or CLI


---


[OpenCode](https://opencode.ai/) is model-agnostic by design. It runs any LLM through any provider, which is why the community around it has grown fast since[Anthropic blocked third-party tools from using Claude subscriptions](https://www.firecrawl.dev/blog/claude-code-vs-opencode) in early 2026. But the model is only half of what makes an agent useful. The other half is context and workflow, and that is what skills provide.


OpenCode implements the Agent Skills open standard natively. It exposes a` skill` tool to the model that lists every installed skill by name and description at startup, then loads the full instructions only when the model decides a skill applies. That progressive loading is what makes it possible to have dozens of skills installed without eating context on unrelated work. It is also what makes a skill written for Claude Code or Codex work in OpenCode without any modification, because OpenCode reads Claude-compatible and agent-standard directories alongside its own.


These are the best OpenCode skills I would install first. Ten skills that cover the real gaps in what a coding agent can do out of the box: live web access, session handoff, codebase understanding, plan pressure-testing, multi-agent orchestration, output compression, and SaaS integrations.


## What are OpenCode skills?


OpenCode skills are directories containing a` SKILL.md` file and optional supporting scripts or reference files. The` SKILL.md` file opens with YAML frontmatter (required fields:` name` and` description` ) followed by markdown instructions the agent reads when the skill activates.


What makes OpenCode's implementation distinct is the native` skill` tool. At session start, OpenCode reads every installed skill and exposes its name and description to the model through a tool definition. When you give the agent a task, it inspects the list, calls` skill({ name: "..." })` on any that match, and only then loads the full instruction body. Skills that never get called never enter the context.


OpenCode looks for skills in six directory locations:


Scope Path Convention


Project` .opencode/skills/` OpenCode-native


Project` .claude/skills/` Claude Code-compatible


Project` .agents/skills/` Agent Skills open standard


Global` ~/.config/opencode/skills/` OpenCode-native user directory


Global` ~/.claude/skills/` Claude Code-compatible


Global` ~/.agents/skills/` Agent Skills open standard


Note that the global OpenCode path is` ~/.config/opencode/skills/` , not` ~/.opencode/skills/` . Several third-party guides get this wrong. The official docs are the source of truth.


### Skills work with any model


Because skills are markdown, they work with whatever provider OpenCode is pointed at: Anthropic Claude, OpenAI, Google Gemini, xAI, Moonshot's Kimi, GLM, DeepSeek, Qwen, or local models via Ollama. A few skills that originated in the Claude Code ecosystem lean on Claude-specific features (particularly the Task tool for subagents) and may need adaptation on other providers. I flag those below.


### Skills, plugins, and permissions


OpenCode also has a separate plugin system: JavaScript or TypeScript modules that hook into runtime events (session lifecycle, tool execution, chat messages) rather than instructing the agent. Some tools, like Obra Superpowers, ship both a skill collection and a plugin. Skills teach the agent what to do; plugins change how OpenCode behaves.


Permissions are OpenCode-specific and worth knowing about. In` opencode.json` , you can allow, deny, or gate individual skills:


```text
{
"permission"  :   {
"skill"  :   {
"*"  :   "allow"  ,
"internal-*"  :   "deny"  ,
"experimental-*"  :   "ask"
}
}
}
```


Skills marked` deny` are hidden from the agent entirely. Per-agent overrides let different agents in the same project see different skill sets. If you want a specific agent to skip skills altogether, set` tools: { skill: false }` on that agent.


For a deeper walkthrough of how the Agent Skills spec came together and grew to over 40,000 published skills, read Firecrawl's[Agent Skills Explained](https://www.firecrawl.dev/blog/agent-skills) .


## What separates a good OpenCode skill from a bad one


After installing and testing a lot of them, the skills that actually earn their spot share a pattern.


**Signs a skill is worth installing:**


- **A specific trigger description.** Bad: "helps with web tasks." Good: "Use when the user asks to scrape a URL, search the web, crawl a documentation site, or extract structured data from a webpage." OpenCode's native` skill` tool pattern-matches against the description at every turn. Vague descriptions produce vague activation.
- **Deterministic work runs in scripts.** Parsing, validation, and sorting belong in bundled scripts, not in the model. Skills that offload the mechanical parts to code are cheaper and more reliable.
- **Lean SKILL.md, deep reference files.** The main body should be short. Push edge cases into companion files that only load when needed.
- **One skill, one job.** Skills that try to cover five workflows trigger at the wrong times and confuse the routing.


**Signs a skill will cause problems:**


- **A bloated SKILL.md** that loads on every adjacent task and burns context regardless of relevance.
- **Undocumented network calls** inside bundled scripts. Read the SKILL.md and every script before installing from unfamiliar authors.
- **No examples.** If you cannot infer the use case from the description and body, neither can the model.


---


## What are the best OpenCode skills to try?


Here are the ten I actually keep installed.


## 1. Firecrawl


**An agent is only as smart as the context it has. The Firecrawl skill gives OpenCode live web context: search, scrape, crawl, map, and browser interaction.**


OpenCode ships with no live web access. Whatever the model was trained on is what it knows, which means recent docs, library changelogs, and live pages are invisible by default. The Firecrawl skill fixes that by teaching OpenCode how to install and use the[Firecrawl CLI](https://www.firecrawl.dev/blog/introducing-firecrawl-skill-and-cli) on its own. After one install command, OpenCode has a complete web data toolkit it can reach for autonomously.


The Firecrawl CLI is built specifically for AI agents. Results write to files rather than dumping into the context window, JavaScript rendering is handled automatically, and the commands map cleanly to how an agent thinks about web tasks. Search to find sources, scrape to read them, crawl to index a whole site, map to discover URLs, interact to control a live browser session.


**Install:**


```text
npx   -y   firecrawl-cli@latest   init   --all   --browser
```


The` --all` flag detects OpenCode alongside every other AI coding agent on your machine and installs the skill to each. The` --browser` flag opens browser authentication so you can connect your API key without copying it manually. Get a free API key at[firecrawl.dev/app/api-keys](https://firecrawl.dev/app/api-keys) . For a broader tour of the eight major[AI coding agents in 2026](https://www.firecrawl.dev/blog/best-ai-coding-agents) , including which support Firecrawl out of the box, see the sourced comparison.


**Commands OpenCode gets access to:**


- ` firecrawl search` : Live web search with full-page content, not just snippets or a cached index (see the[best web search MCP servers](https://www.firecrawl.dev/blog/best-web-search-mcp) comparison for how Firecrawl compares to Tavily and Exa)
- ` firecrawl scrape` : Clean markdown from any page, including JavaScript-heavy sites
- ` firecrawl crawl` : Recursively follow links across an entire site
- ` firecrawl map` : Discover all URLs on a domain
- ` firecrawl interact` : Scrape a page then interact with it via natural language prompts or Playwright code, with a[live view stream](https://docs.firecrawl.dev/features/interact) you can watch or control


**Example:**


```text
"Scrape the Prisma docs changelog and summarize what changed in the last 30 days"
"Search for the top three competitors to our product and extract their pricing pages into a comparison table"
"Crawl the OpenCode docs and find every page that mentions the plugin system"
```


**Honest take:** Firecrawl is the primitive I install first on any coding agent, OpenCode included. The reason is architectural: unlike Composio (which handles SaaS APIs) or Cloudflare's skill (which teaches product knowledge), Firecrawl gives OpenCode access to the open web. That is the one thing no LLM provider ships natively. Every other skill that depends on live information (documentation lookups, competitor research, changelog watching) sits on top of it.


**Cons:** You can try the endpoints without an API key, but ongoing use needs one and consumes credits on heavy usage. The free tier of 1,000 credits per month covers substantial testing, but production research workflows may need a paid plan.


[Firecrawl](https://www.firecrawl.dev/) is used by 1.25M+ developers across 150,000+ companies and has served 5B+ requests to date. Builders keep coming back because the whole workflow ([search](https://www.firecrawl.dev/search) ,[scrape](https://www.firecrawl.dev/scrape) , crawl,[interact](https://www.firecrawl.dev/interact) ) lives in a single install on the real web.


Full documentation and CLI reference at[docs.firecrawl.dev/cli](https://docs.firecrawl.dev/cli) . Also worth reading: the deeper look at[why default agent web search falls short](https://www.firecrawl.dev/blog/codex-web-search-using-firecrawl) and the[Claude Agent SDK with Firecrawl](https://www.firecrawl.dev/blog/claude-agent-sdk-firecrawl) guide.


## 2. stop-slop


**Hardik Pandya's stop-slop skill teaches the agent to detect and remove common AI writing tells from any prose it generates.**


Anyone who has asked a coding agent to draft a README, a commit message, or documentation knows the pattern: throat-clearing openers, business jargon, em dashes everywhere, and the "Not X. But Y." rhetorical rhythm that shows up in every AI-generated draft. The stop-slop skill, currently at 13.4k stars, is a` SKILL.md` plus three companion reference files (` phrases.md` ,` structures.md` ,` examples.md` ) that give the agent a specific taxonomy of what to strip and a 1-10 scoring rubric across five dimensions (Directness, Rhythm, Trust, Authenticity, Density). The skill revises anything scoring below 35 out of 50.


This is not a code skill. It is an Encoded Preference skill for writing tasks: docs, blog posts, changelogs, marketing copy, anything where the agent generates prose you will actually ship.


**Install:**


```text
mkdir   -p   ~/.agents/skills
git   clone   https://github.com/hardikpandya/stop-slop.git   ~/.agents/skills/stop-slop
```


For an OpenCode-native install, drop into` ~/.config/opencode/skills/stop-slop` instead. Both paths work because OpenCode reads six recognized skill directories.


**What the skill actually catches:**


- Throat-clearing openers like "Here's what I find interesting..."
- Emphasis crutches, all adverbs, vague declaratives
- Structural cliches: binary contrasts ("Not X. But Y."), dramatic fragmentation, rhetorical setups, narrator-from-a-distance voice, passive voice
- Em dashes, Wh-sentence starters, staccato fragmentation
- Business jargon and lazy extremes


**Example:**


```text
"Draft the README for this repo and run stop-slop on it before showing me"
"Rewrite this changelog entry, then apply stop-slop"
"Score this landing page copy with stop-slop and revise anything under 35"
```


**Honest take:** This is the skill I reach for most on documentation tasks. The score-then-revise pattern is smarter than a simple "make it less AI-sounding" prompt because it gives the agent a concrete rubric to hit. Reading the flagged reference files is also educational: you start noticing the same patterns in your own writing.


**Cons:** Not designed for code output, only prose. And the rubric is opinionated. If your team writing style already leans heavy on em dashes or dramatic contrasts, the skill will fight you. Fork it and tune the reference files before turning it loose on marketing copy.


Repo:[github.com/hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop) . Author writeup:[hvpandya.com/stop-slop](https://hvpandya.com/stop-slop) .


## 3. Handoff


**[Matt Pocock's](https://x.com/mattpocockuk) handoff skill compresses your current OpenCode session into a structured markdown document so you can continue the work in a fresh session, or pass it to a different agent entirely.**


The problem it solves is context drift. As[discussed on HN](https://news.ycombinator.com/item?id=47581897) , sessions nearing compaction limits do not just slow down, they get worse at their job. After roughly 120k tokens, attention relationships strain and response quality degrades.` /handoff` gives you a clean exit before you hit that wall: a document containing the purpose of the next session, relevant context from the current one, suggested skills to invoke, and pointers to existing artifacts, without duplicating file content.


The pattern that makes this genuinely powerful is cross-agent: plan in Claude Code (or use` /grill-me` there), generate a handoff doc, then hand it to OpenCode running any model of your choice for the actual implementation. Or split a large task across parallel OpenCode worktrees with the same handoff seeding each one.


**Install:**


```text
npx   skills@latest   add   mattpocock/skills
```


The installer prompts you to pick which skills to install and which agents to install them to; select` handoff` and OpenCode. Matt's collection sits at 158k GitHub stars and is one of the largest skills repositories in the ecosystem.


**Example:**


```text
"Create a handoff for this session before I run out of context"
"Handoff, I want to continue this in a new OpenCode session"
"Generate a handoff doc so I can delegate the implementation to a worktree running Qwen"
```


**Honest take:** The handoff document is more purposeful than a` /compact` summary because you control what the next session needs to know. It works especially well in OpenCode because you can hand off to a session running a completely different model, matching model cost and capability to the specific task at hand.


**Cons:** The generated document goes to your OS temp directory by default, so commit it if you want a permanent record. Most useful when you are deliberately splitting work across sessions or agents; less necessary for single-threaded sessions.


Full reference at[aihero.dev/skills-handoff](https://www.aihero.dev/skills-handoff) .


## 4. Grill Me


**Matt Pocock's grill-me skill interviews you relentlessly about every aspect of a plan until you reach shared understanding, before OpenCode writes any code.**


Grill-me went viral on X earlier this year because it fixes the most common failure mode in agentic coding: the agent charging ahead with wrong assumptions before you had a chance to correct them. The instruction in the SKILL.md is direct: "Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer." If a question can be answered by reading the codebase, OpenCode reads it and moves on rather than asking you.


This is a design review tool. Use it before you write code, not after. The questions surface implicit assumptions, dependency chains between decisions, and gaps in your plan that feel obvious until you try to articulate them.


**Install:**


```text
npx   skills   add   https://github.com/mattpocock/skills   --skill   grill-me
```


**Example:**


```text
"Grill me on this feature spec before I start building"
"Run grill-me on my plan for the auth refactor"
"Walk me through every decision in this architecture before I commit to it"
```


**Honest take:** Grill-me is where I would start if I were new to OpenCode. It forces the model to think about the plan before generating code, which changes the ratio of code you have to throw away later. The recommended-answer pattern also keeps things moving. You are not stuck at each question with no direction, you are reacting to a proposed answer.


**Cons:** Requires an actual plan or design to stress-test. "I want to build X" is too vague to grill effectively. Prepare a written spec or description before invoking the skill. The session is open-ended by design, not a checklist with a fixed number of questions.


Repo:[github.com/mattpocock/skills](https://github.com/mattpocock/skills) .


## 5. Obra Superpowers


**Superpowers is the most complete agentic development framework available as an OpenCode-compatible skill set.**


Obra's Superpowers is currently the highest-starred skills repository on GitHub (247k stars, 21.9k forks). It ships two things: a plugin that injects a bootstrap into every session (the "1% rule": if there is even a 1% chance a skill applies, use it), and a set of composable skills that structure the full software development lifecycle. Brainstorming, git worktree setup, implementation planning, subagent-driven execution, test-driven development, systematic debugging, verification before completion, and finishing a development branch.


OpenCode is a first-class target for Superpowers. There is a dedicated` .opencode/` directory in the repository with harness-specific install instructions and documentation at` docs/README.opencode.md` .


**Install (OpenCode-specific):**


Paste this as a prompt to OpenCode:


```text
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md


```


OpenCode will fetch the install doc and run through the setup itself. Alternatively, add the plugin declaratively in` opencode.json` :


```text
{
"plugin"  :   [  "superpowers@git+https://github.com/obra/superpowers.git"  ]
}
```


**Key skills in the collection:**


- ` brainstorming` : Socratic design refinement before any code
- ` using-git-worktrees` : isolated workspace per feature, with a clean baseline check
- ` writing-plans` : breaks work into 2-5 minute tasks with exact file paths and verification steps
- ` subagent-driven-development` : dispatches fresh subagents per task with two-stage review (spec compliance, then code quality)
- ` test-driven-development` : enforces red-green-refactor discipline
- ` systematic-debugging` : a four-phase root-cause process
- ` verification-before-completion` : ensures the fix actually works before marking a task done


**Example:**


```text
"Brainstorm the approach for adding real-time collaboration to this note app"
"Write a plan for the auth refactor, then execute it"
"Debug this failing test using systematic-debugging"
```


**Honest take:** Superpowers is heavy. The bootstrap runs in every session, and the opinionated methodology (TDD, YAGNI, DRY) will not fit every workflow. But if your project has clear requirements and you want systematic execution instead of exploratory prototyping, it is the closest thing available to a full agentic engineering harness dropped into your existing tool. The subagent-driven approach also matches OpenCode's model flexibility well: you can route subagents to cheaper models and keep frontier models for design review.


**Cons:** Ambient token cost from the bootstrap in every session. Requires the plugin system (separate install path from most other skills). Telemetry ships on by default; disable with` SUPERPOWERS_DISABLE_TELEMETRY=1` .


Repo:[github.com/obra/superpowers](https://github.com/obra/superpowers) . Original release writeup:[blog.fsck.com/2025/10/09/superpowers](https://blog.fsck.com/2025/10/09/superpowers/) .


## 6. Understand-Anything


**Understand-Anything turns any codebase into an interactive knowledge graph that OpenCode can query, tour, and diff against your changes.**


Understand-Anything is a multi-agent skill collection (71k stars) that runs a pipeline of specialized agents (project-scanner, file-analyzer, architecture-analyzer, tour-builder, graph-reviewer, domain-analyzer) to produce a full knowledge graph of your project. Every file, function, class, and dependency becomes a node with plain-English summaries. Then a dashboard exposes fuzzy and semantic search, color-coded architectural layers, and guided tours.


The skill combines Tree-sitter (deterministic structure extraction) with LLM analysis (semantic summaries) so the graph is grounded in the actual code, not just what the model remembers about your repo.


**Install (OpenCode-specific):**


```text
curl   -fsSL   https://raw.githubusercontent.com/Egonex-AI/Understand-Anything/main/install.sh   |   bash   -s   opencode
```


Restart OpenCode after install. Windows PowerShell:` iwr -useb https://raw.githubusercontent.com/Egonex-AI/Understand-Anything/main/install.ps1 | iex` . Update with` ./install.sh --update` , uninstall with` ./install.sh --uninstall opencode` .


**Commands OpenCode gets:**


- ` /understand` : scans the project and builds` .understand-anything/knowledge-graph.json` , incremental on subsequent runs
- ` /understand-dashboard` : opens an interactive web dashboard with color-coded architectural layers and search
- ` /understand-chat` : ask natural-language questions about the codebase
- ` /understand-diff` : see which parts of the system your changes affect before committing
- ` /understand-domain` : extract business domains, flows, and process steps
- ` /understand-knowledge` : analyze wiki-style knowledge bases (the Karpathy LLM-wiki pattern)


**Example:**


```text
"Run /understand on this monorepo and open the dashboard"
"Which modules depend on the payment service?"
"Show me the domain flow for user signup"
```


**Honest take:** The first` /understand` run on a large codebase eats a substantial number of tokens because it has to analyze every file. Subsequent runs are incremental and cheap. This makes Understand-Anything a great fit for OpenCode specifically because you can point the first-time analysis at a local model via Ollama or a cheap open model, and switch to a frontier model for the interactive queries later.


**Cons:** First-run cost is real. If your project is over 100k lines and you are on a paid API, budget accordingly. Requires restart after install to register the commands.


Repo:[github.com/Egonex-AI/Understand-Anything](https://github.com/Egonex-AI/Understand-Anything) .


## 7. Caveman


**Julius Brussee's Caveman skill cuts OpenCode output tokens by an average of 65% by stripping narration, filler, and pleasantries while keeping every technical fact and code block byte-for-byte intact.**


The concept: when OpenCode explains a React re-render bug normally, it says something like "The reason your component is re-rendering is likely because you're creating a new object reference on each render cycle. I'd recommend using useMemo to memoize the object." (69 tokens). In caveman mode: "New object ref each render. Inline object prop = new ref = re-render. Wrap in` useMemo` ." (19 tokens). Same fix. 75% fewer words.


A March 2026 paper found that constraining large models to brief responses actually improved accuracy by 26 points on certain benchmarks. Caveman does not make the model dumber; it makes its output smaller while keeping its reasoning the same size.


**Install:**


```text
curl   -fsSL   https://raw.githubusercontent.com/JuliusBrussee/caveman/main/install.sh   |   bash
```


The one-liner detects OpenCode alongside 30+ other agents and installs to each. Windows:` irm https://raw.githubusercontent.com/JuliusBrussee/caveman/main/install.ps1 | iex` . Node 18+ required. Takes about 30 seconds.


**What you get:**


- ` /caveman \[lite|full|ultra|wenyan\]` : compress every reply at your chosen intensity; modes persist until you say "normal mode"
- ` /caveman-commit` : Conventional Commit messages, 50-char subject line max, why over what
- ` /caveman-review` : one-line PR review comments like` L42: bug: user null. Add guard.`
- ` /caveman-compress <file>` : rewrites your AGENTS.md or CLAUDE.md into caveman-speak, cutting ~46% of input tokens per session
- ` /caveman-stats` : real session token usage, lifetime savings, and USD cost
- ` caveman-shrink` : MCP middleware that wraps any MCP server and compresses tool descriptions


**Example:**


```text
"/caveman full"
"Explain what changed in this diff"
```


**Honest take:** The 65% output reduction is real, and the` /caveman-compress` companion cuts your AGENTS.md down by roughly half so every future session runs on fewer input tokens too. On OpenCode this pairs particularly well with cheaper model routing: use Caveman on a mid-tier model and you can push through workloads that would strain a frontier plan. For a full picture of systematic[Claude Code token efficiency techniques](https://www.firecrawl.dev/blog/claude-code-token-efficiency) , Firecrawl's guide covers 12 approaches including path-scoped rules and model routing that transfer directly to OpenCode.


**Cons:** Caveman's own HONEST-NUMBERS.md is refreshingly candid about the tradeoffs. The skill compresses output tokens only, not input or reasoning tokens. The skill itself adds ~1-1.5k input tokens per turn, so on very terse workloads it can go net-negative. The OpenCode integration was fixed in a recent commit but the maintainer noted it was "not smoke-tested against a real opencode runtime" at the time, so verify it behaves as expected on your setup.


Repo:[github.com/JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) (85k stars, growing).


## 8. skill-optimizer


**hqhq1025's skill-optimizer is a three-skill toolkit that manages your other skills: mining your session history for skill-worthy workflows, personalizing installed skills to your habits, and generalizing local skills for publication.**


Most skills teach the agent to do a task. Skill-optimizer teaches the agent to manage its own skill collection. The v2.0 release (May 2026) covers the full lifecycle from discovery through publication:


- ` skill-miner` : scans your coding-agent history, archives, memories, and repeated work to surface workflows that would benefit from being encoded as a skill. Ships with a deterministic scan script (` scripts/scan_sessions.py` ) that covers Codex, Claude Code, Gemini, Antigravity task files, and exported transcripts.
- ` skill-personalizer` : audits installed skills. Diagnoses undertriggering, overtriggering, and question-friction. Adapts skills to your local phrasing, preferred tools, and verification habits.
- ` skill-generalizer` : turns local or private skills into publishable form. Strips private paths, credentials, and transcript quotes. Makes install commands portable across harnesses.


**Install:**


Paste this as a prompt to OpenCode:


```text
Install the skills from https://github.com/hqhq1025/skill-optimizer into ~/.agents/skills/


```


Or manually:


```text
git   clone   https://github.com/hqhq1025/skill-optimizer.git   /tmp/skill-optimizer
mkdir   -p   ~/.agents/skills
cp   -r   /tmp/skill-optimizer/skills/skill-miner   ~/.agents/skills/
cp   -r   /tmp/skill-optimizer/skills/skill-personalizer   ~/.agents/skills/
cp   -r   /tmp/skill-optimizer/skills/skill-generalizer   ~/.agents/skills/
```


**Example:**


```text
"Run skill-miner on my last month of Codex sessions"
"Personalize the caveman skill to match how I actually work"
"Generalize my internal debug-loop skill so I can publish it"
```


**Honest take:** Skill-optimizer is meta, and it only makes sense if you have session history to mine. Brand new users will not get much value. But once you have been using OpenCode for a few weeks with a stable set of skills,` skill-miner` reliably finds three or four repeatable workflows worth encoding, and` skill-personalizer` catches the ones that quietly fail to activate. The README explicitly lists OpenCode's three skill paths as recommended install locations.


**Cons:** Small user base (125 stars). Not battle-tested at scale yet, but the code is clean and the mental model is sharp. Documentation is thinner than the bigger skills, so read the SKILL.md carefully before running against a production session archive.


Repo:[github.com/hqhq1025/skill-optimizer](https://github.com/hqhq1025/skill-optimizer) .


## 9. Vault Daydream


**Vault Daydream samples random pairs of notes from an Obsidian vault, dispatches parallel subagents to synthesize non-obvious connections, and writes accepted insights back as new notes.**


Vault Daydream comes from Gleb's[claude-skills](https://github.com/glebis/claude-skills) collection and is based on Gwern's[LLM Daydreaming essay](https://gwern.net/ai-daydreaming) . The concept mimics the brain's default mode network. When you are not actively thinking about a problem, novel connections form between unrelated memories. The skill does the same thing for your notes: 50 random pairs of recently-modified notes, generator subagents that hunt for non-obvious links, critic subagents that score novelty and coherence, and a filter that keeps only insights scoring above 7.0/10.


**Install:**


```text
git   clone   https://github.com/glebis/claude-skills.git
cp   -r   claude-skills/daydream   ~/.agents/skills/
# Edit ~/.agents/skills/daydream/instructions.md to point VAULT_ROOT at your vault
```


For OpenCode-native install use` ~/.config/opencode/skills/daydream/` instead.


**Example:**


```text
"Run daydream on my Obsidian vault"
"Daydream over notes I've modified in the last 60 days"
```


**Honest take:** Vault Daydream is my favorite non-code skill on this list. It runs during a walk or a coffee break and comes back with 5-10 genuinely useful cross-note observations you would not have made on your own. On OpenCode you can route the generator subagents to Sonnet-tier models and the critic subagents to Haiku-tier for cost efficiency, though this requires configuration.


**Cons:** The SKILL.md was written for Claude Code and uses Anthropic-specific` Task(model: sonnet|haiku)` invocations. On OpenCode with non-Anthropic providers, you may need to adapt the dispatch pattern to work with your configured models. Also: cost is real, about $0.40-$0.50 per run against Claude, per the author's measurements. Cheaper on open models, but budget it.


Repo:[github.com/glebis/claude-skills](https://github.com/glebis/claude-skills) .


## 10. Composio


**The Composio skill teaches OpenCode how to use Composio's 1000+ SaaS integrations (GitHub, Linear, Slack, Stripe, Salesforce, Jira, Notion) via MCP, native tools, or CLI.**


Firecrawl handles the open web. Composio handles the authenticated one. Together, they cover the two ways a coding agent actually needs to reach the outside world: reading arbitrary pages and calling SaaS APIs your team already uses. The Composio skill is a set of 14+ rules covering Tool Router best practices, session management, authentication flows, and Triggers (real-time event webhooks). It includes framework integration guides for Vercel AI SDK, OpenAI Agents, LangChain, Claude, and CrewAI, all with ❌/✅ examples in TypeScript and Python.


**Install:**


```text
npx   skills   add   composiohq/skills
```


Then install the Composio CLI separately:


```text
curl   -fsSL   https://composio.dev/install   |   bash
composio   login
```


For a specific project:


```text
composio   init
```


**What the skill covers:**


- Tool Router: session lifecycle, user isolation, native tools vs MCP comparison
- Framework integration guides across five major agent frameworks
- Authentication: auto-auth in chat and manual authorization flows
- Triggers: creating real-time event subscriptions, webhook verification, production deployment
- Best-practice patterns with concrete counterexamples


**Example:**


```text
"Create a Linear ticket for the bug I just fixed and link it to this PR"
"Send a Slack message to #eng-releases summarizing the changelog"
"List the pending Stripe subscriptions expiring this month"
```


**Honest take:** The Composio skill itself is small (97 stars). What matters is what it unlocks: authenticated access to the SaaS surface area your team already lives in, without you writing an OAuth flow for each service. For OpenCode users this pairs naturally with model-agnostic routing. You can drive Composio actions with whatever model the specific task calls for.


**Cons:** Requires a Composio account and API key. The skill repo has not shipped a commit since March 2026, so the surface it covers may lag Composio's product releases. If you rely on a specific integration, verify the skill has patterns for it before installing.


Repo:[github.com/ComposioHQ/skills](https://github.com/ComposioHQ/skills) . Product:[composio.dev](https://composio.dev/) .


## Building the top OpenCode skills into your workflow


Skills fix the two things that limit any coding agent out of the box: what it knows about your world (context) and how it works through problems (workflow). OpenCode's model-agnostic design makes both of those problems more interesting, not less. Different models have different strengths, and skills let you pin the right behavior across all of them.


The combination I keep going back to: Firecrawl for live web access, stop-slop on any writing task, grill-me before any non-trivial feature, Superpowers when the task calls for a systematic approach, and Caveman running in the background to keep session cost down. Handoff and Understand-Anything come out on longer projects or when I need to pass work between sessions.


One thing worth calling out: skills are an open standard. The same` SKILL.md` file works in OpenCode, Claude Code, Codex, Cursor, Gemini CLI, and Copilot without modification. That is why OpenCode reads six recognized directories, including` ~/.claude/skills/` and` ~/.agents/skills/` alongside its own path. Install a skill once and it follows you across agents. If you are still weighing which agent to make primary, the[Claude Code vs Codex comparison](https://www.firecrawl.dev/blog/claude-code-vs-codex) covers a related decision on pricing, harness depth, and community verdict.


You do not have to stick to these templates. The skills above are a starting point, but everyone's workflow and use cases are different, and the skills that will actually save you time are the ones you write for the specific work you do every day. A skill can be as small as a checklist for a task you repeat, or as involved as a full multi-step workflow. If it saves you from re-explaining context to the agent, it is worth building.


For discovering what else is out there,[skills.sh](https://skills.sh/) (maintained by Vercel) is the searchable directory across every agent skills ecosystem. The[awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) repository is another curated list of top OpenCode skills and Claude Code skills worth browsing, and our list of[best GitHub repos 2026](https://www.firecrawl.dev/blog/best-github-repos) covers other skill packs and infrastructure repos worth a star. If you want to build your own from scratch, the[Claude Code skill tutorial](https://www.firecrawl.dev/blog/claude-code-skill) walks through building a Firecrawl-powered skill end to end, and the[best Codex skills](https://www.firecrawl.dev/blog/best-codex-skills) and[best Claude Code skills](https://www.firecrawl.dev/blog/best-claude-code-skills) roundups cover what is worth installing on those harnesses. Every skill on those lists that uses the standard` SKILL.md` format also runs in OpenCode.
