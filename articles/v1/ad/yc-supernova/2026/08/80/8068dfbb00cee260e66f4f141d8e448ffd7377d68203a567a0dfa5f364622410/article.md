---
schema_version: "1.0.0"
document_id: "8068dfbb00cee260e66f4f141d8e448ffd7377d68203a567a0dfa5f364622410"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/what-to-put-in-claude-md-for-a-design-system"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-14T13:17:46.842723+00:00"
fetched_at: "2026-08-14T13:17:49.119979+00:00"
content_hash: "sha256:dd0d947907277977595e55dd2fe6c9b2ff9a01c8de4628d4d9dd211673355f30"
---

# What to Put in CLAUDE.md for a Design System (And What to Leave Out)

A practical answer, based on what Anthropic, GitHub and the AGENTS.md standard actually recommend.


If your agents keep producing UI that misses your design system, the obvious fix is to write the rules down in a` CLAUDE.md` or` AGENTS.md` file. The question is what belongs in there and what doesn't, because the answer is narrower than most teams assume.


The short version: **put your conventions in the file and keep your reference data out of it.** Tokens, component APIs and usage rules fail the tests all three vendors publish for what a context file should contain.


Here's what to include, what to exclude, and where the excluded parts should live instead.


## What should go in a CLAUDE.md file?


[Anthropic's best practices for Claude Code](https://code.claude.com/docs/en/best-practices) define the file by how it loads:


> "CLAUDE.md is loaded every session, so only include things that apply broadly."


For a design system, "applies broadly" is a small list. Something like:


```text
#   Design system
-   Use components from @acme/ui. Never write a raw button, input, or modal.
-   All colour, spacing and type values come from tokens. No hex values.
-   Import from the package root, not deep paths.
-   If a component doesn't exist, ask before building one.


```


Four lines, and they apply to every task an agent will pick up. This is the part that works, and most teams find it stops the obvious violations immediately.


GitHub's analysis of[more than 2,500 agents.md files](https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/) found the strongest files cover six areas: commands, testing, project structure, code style, git workflow, and boundaries. Design system rules fit under code style and boundaries. That's the slot they belong in.


## Can you put design tokens in CLAUDE.md?


Anthropic's guidance says no, and gives three separate reasons. Their exclusion list:


Exclude What that rules out


Detailed API documentation. Link to docs instead. Component props and variants


Information that changes frequently Design tokens, component versions


File-by-file descriptions of the codebase Your component inventory


A design system is detailed API documentation about a codebase that changes frequently. It fails all three tests at once.


There's a practical version of the same warning circulating among people who have tried it: avoid copying a token file into your context file, because a duplicated list goes out of date and nothing tells you when.


Neither design tokens nor component APIs appear anywhere in GitHub's six core areas either. The files that perform well are operational instructions. Reference data is a different kind of thing.


## What happens if you add it anyway?


Most teams add it anyway, because every gap in the output looks like a missing line in the file. Token names, then values, then component lists, then usage rules.


Anthropic names the result as one of five common failure patterns:


> "The over-specified CLAUDE.md. If your CLAUDE.md is too long, Claude ignores half of it because important rules get lost in the noise."


And in the writing guidance:


> "Keep it concise. For each line, ask: *Would removing this cause Claude to make mistakes?* If not, cut it. Bloated CLAUDE.md files cause Claude to ignore your actual instructions!"


The mechanism is stated plainly on the same page:


> "Most best practices are based on one constraint: Claude's context window fills up fast, and performance degrades as it fills."


The file loads on every turn. Every line competes with every other line, and with the work itself.


## Why the failure is hard to catch


An ignored instruction doesn't raise an error. No log entry, no warning, nothing showing which rule got dropped on which turn.


So the failure isn't an agent that stops. It's an agent that picks the nearest plausible token, ships a colour one step off the ramp, and carries on. The pull request looks right. The screenshot looks right. Nobody diffs the rendered output against the token documentation.


Design system teams are measured on consistency, and this produces small inconsistencies continuously with no signal attached to any of them. It surfaces later, in aggregate, when somebody asks why the product has four warning oranges.


## What about AGENTS.md and DESIGN.md?


Same answer, different file.


The[AGENTS.md standard](https://agents.md/) , stewarded by the Linux Foundation and used in over 60,000 repositories, describes itself as "a README for agents." Its suggested sections are project overview, build and test commands, code style, testing instructions and security. Orientation, not reference.


` DESIGN.md` is a newer convention, usually a companion file holding visual direction: palette, type scale, spacing, component look and feel. It's genuinely useful for giving an agent a consistent aesthetic, particularly when you're generating something new rather than working inside an existing product.


It runs into the same ceiling. A DESIGN.md that describes your visual language is fine. A DESIGN.md that tries to *be* your design system is a hand-maintained copy of something that changes weekly.


The test for any of these files is the same: does this apply to every task, and will it still be true next month?


## How the standard handles a system too big for one file


The AGENTS.md documentation addresses scale directly:


> "Large monorepo? Use nested AGENTS.md files for subprojects. Agents automatically read the nearest file in the directory tree, so the closest one takes precedence and every subproject can ship tailored instructions."


They mention that[OpenAI's own repository](https://github.com/openai/codex/blob/-/AGENTS.md) contains **88 AGENTS.md files** .


That's the right instinct, and worth reading carefully if you own a design system. The official answer to too much context is to break it into scoped pieces and load the relevant one. It also means a large organisation ends up with dozens of places design system rules live, each maintained by hand. Multiply by brands and platforms and the maintenance job becomes a distributed consistency problem, which is what the design system existed to prevent.


## Where the reference data should live instead


All three sources land on the same architecture.


Anthropic, on knowledge that isn't relevant to every task:


> "For domain knowledge or workflows that are only relevant sometimes, use skills instead. Claude loads them on demand without bloating every conversation."


The AGENTS.md standard, on scale: nearest-file scoping, so each agent gets the slice relevant to where it's working.


GitHub, on specifics: keep the file operational and let the details live where the agent can read them.


Short always-on instructions, plus everything else retrieved on demand and scoped to the task. Not a smaller file. A different mechanism.


For a design system that means the token values, component APIs, variant rules and usage guidance sit somewhere an agent can query at the moment it needs them, scoped to what that team is building, and current because they come from the system rather than a copy of it.


That's what Supernova's[AI context management](https://www.supernova.io/blog/we-just-shipped-ai-context-management) does. Contexts are generated from your design system data, scoped per team or workflow, and served over MCP, so the agent pulls the current value instead of reading a snapshot someone typed out in May.


## The rule of thumb


Put it in the file if it applies to every task and will still be true next month. Everything else belongs somewhere the agent can look it up.


Your conventions are a handful of lines. Your design system is reference material. The file should point at it, not contain it.


Supernova serves your design system to agents as scoped, current context instead of a file you maintain by hand —[see how it works](https://www.supernova.io/ask-ai-browse-your-design-system) .
