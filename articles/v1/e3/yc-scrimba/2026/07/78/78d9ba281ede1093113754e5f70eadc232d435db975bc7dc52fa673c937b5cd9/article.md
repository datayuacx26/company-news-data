---
schema_version: "1.0.0"
document_id: "78d9ba281ede1093113754e5f70eadc232d435db975bc7dc52fa673c937b5cd9"
company_key: "yc-scrimba"
company: "Scrimba"
source_id: "yc-scrimba-news-import-2d86273fd3f5"
canonical_url: "https://scrimba.com/articles/how-to-use-claude-code/"
published_at: "2026-07-23T08:14:23+00:00"
first_seen_at: "2026-07-23T12:55:07.390018+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:081dd1693b0ad880b1fc92d807d2f54ee9db7b464a521b3aa1d39e4c0e02e1de"
---

# How to Use Claude Code: A Developer's Guide [2026]

Claude Code is Anthropic's agentic coding tool. You install it, open a project, and describe a task in plain language. It reads your files, edits code, runs commands, and checks its own work in a loop until the task is finished or you stop it.


That last part is the whole story. Autocomplete guessed the end of your line. An agent takes the whole task, and your job shifts from typing to two harder things: describing work precisely enough for a machine to execute, and reading the result closely enough to trust it.


Plenty of developers have heard of Claude Code. Far fewer have finished a real task with it and understood *why* it went well or badly. This guide covers installation, the workflow loop, project memory, permissions, MCP, a worked bug fix, and when an agent is wrong for the job.


## What Is Claude Code?


Claude Code is an agentic coding tool that reads your codebase, edits files, runs commands, and integrates with your development tools from the terminal.


That definition comes from[Anthropic's own documentation](https://code.claude.com/docs/en/overview?ref=scrimba.com) , and every word is load-bearing. Claude Code is not a chat window that hands back a snippet. It has hands.


Underneath, it runs what Anthropic calls the *agentic loop* .


> When you give Claude a task, it works through three phases: gather context, take action, and verify results. ([Claude Code documentation](https://code.claude.com/docs/en/how-claude-code-works?ref=scrimba.com) )


The tools inside that loop fall into five groups: **file operations** , **search** , **execution** (shell commands, tests, git), **web** access, and code intelligence. Ask it to fix failing tests and it runs the suite, reads the output, finds the source files, edits them, and reruns. If the category is new to you, our guide to[agentic coding](https://scrimba.com/articles/what-is-agentic-coding/) covers the concept.


## How Do You Install Claude Code?


Install Claude Code with a one-line native installer, a package manager, or WinGet, then verify with` claude --version` and log in on first run.


1. **Native install (recommended).** On macOS, Linux, or WSL:` curl -fsSL https://claude.ai/install.sh | bash` . On Windows PowerShell:` irm https://claude.ai/install.ps1 | iex` .
2. **Homebrew.**` brew install --cask claude-code` .
3. **WinGet.**` winget install Anthropic.ClaudeCode` .
4. **Verify.** Run` claude --version` . It prints a version number and` (Claude Code)` .
5. **Start and sign in.**` cd` into a project, run` claude` , and follow the login prompt.


One detail matters before you pick: native installs update themselves in the background, while Homebrew and WinGet installs do not ([Anthropic](https://code.claude.com/docs/en/quickstart?ref=scrimba.com) ).


You also need an account. Claude Code ships with the Pro, Max, Team, and Enterprise plans, or a Claude Console account with prepaid credits. Pro is **$17 per month billed annually, $20 monthly** , and the free plan does not include it ([Anthropic pricing](https://claude.com/pricing?ref=scrimba.com) ).


Four shell commands cover most days:` claude` opens a session,` claude -p "query"` answers once and exits,` claude -c` continues the last conversation,` claude -r` picks an older one.


## The Core Workflow Loop: Explore, Plan, Code, Commit


Anthropic's recommended workflow has four phases, and jumping straight to phase three is how new users get bad results.


1. *Explore.* Enter plan mode and let Claude read. Something like "read` src/auth` and explain how sessions and login work." No edits happen yet.
2. *Plan.* Ask for a written implementation plan: "I want to add Google OAuth. What files change? What is the session flow?" Press` Ctrl+G` to open that plan in your editor and correct it before anything runs.
3. *Implement.* Leave plan mode and let it build against the plan you approved.
4. *Commit.* "Commit with a descriptive message and open a PR."


Plan mode is reached with` Shift+Tab` or a` /plan` prefix. It is useful, and it is also overhead. Anthropic's guidance on skipping it is blunt: *if you could describe the diff in one sentence, skip the plan* ([best practices](https://code.claude.com/docs/en/best-practices?ref=scrimba.com) ).


The phase people underinvest in is the back half of implementation. An agent stops when the work *looks* done, and without a check it can run, that is the only signal available.


> Give Claude a check it can run: tests, a build, a screenshot to compare. It's the difference between a session you watch and one you walk away from. ([Anthropic](https://code.claude.com/docs/en/best-practices?ref=scrimba.com) )


That check can be a test suite, a build exit code, a linter, or a screenshot compared against a design. Ask for evidence, not just a verdict. "Run the tests and show me the output" is a different instruction from "make sure the tests pass."


## How Does CLAUDE.md Project Memory Work?


CLAUDE.md is a markdown file Claude Code reads at the start of every session, holding the build commands, conventions, and rules it cannot infer from your code.


Every session starts with an empty context window, so CLAUDE.md is how knowledge survives. Files load broadest scope first:


Location Scope Shared with


` ~/.claude/CLAUDE.md` Your preferences, every project Just you


` ./CLAUDE.md` or` ./.claude/CLAUDE.md` This project Your team, via version control


` ./CLAUDE.local.md` This project, personal Just you (add to` .gitignore` )


Run` /init` to generate a starter file,` /context` to confirm it loaded, and` /memory` to edit it later ([Anthropic](https://code.claude.com/docs/en/memory?ref=scrimba.com) ).


The rule that catches everyone is size. Anthropic recommends **staying under 200 lines** , because the file loads into context every session and a bloated one makes Claude ignore half of it. A test for each line: would removing this cause a mistake? If not, cut it. Write down what a new teammate would have to be told, like the build command, the test runner, and conventions that differ from language defaults. Leave out what Claude can learn by reading the code.


Two extras are worth knowing. You can pull in other files with` @path/to/file` syntax, and larger projects can split instructions into` .claude/rules/` , where a` paths` field scopes a rule to matching files. Claude Code also reads` CLAUDE.md` , not` AGENTS.md` . If your repo has an` AGENTS.md` , make` @AGENTS.md` the first line of a` CLAUDE.md` so both tools read the same instructions.


## Slash Commands, Permission Modes, and MCP


### The slash commands worth memorizing


You do not need all of them. These earn their keep:


- ` /clear` starts a fresh conversation with empty context.
- ` /compact` summarizes the conversation to free space, and takes a focus argument.
- ` /context` shows what is consuming your context window.
- ` /rewind` (or a double` Esc` ) rolls code and conversation back to a checkpoint.
- ` /code-review` checks the current diff for bugs in a separate context.
- ` /model` ,` /permissions` , and` /doctor` cover models, approval rules, and a broken setup.


### Permission modes decide how often Claude stops to ask


Press` Shift+Tab` to cycle modes mid-session, or start with` claude --permission-mode plan` .


Mode Runs without asking Best for


Manual (` default` ) Reads only Getting started, sensitive work


` acceptEdits` Reads, file edits, common filesystem commands Iterating on code you are reviewing


` plan` Reads only, no source edits Exploring before you change anything


` auto` Everything, with background safety checks Long tasks, fewer interruptions


` bypassPermissions` Everything Isolated containers and VMs only


` auto` mode runs a classifier over each action before it executes, blocking things like` curl | bash` , force pushes, and production deploys ([Anthropic](https://code.claude.com/docs/en/permission-modes?ref=scrimba.com) ).` bypassPermissions` does not, and Anthropic's warning is explicit: use it only in containers or VMs. It is a lab tool, not a productivity setting. In every other mode, writes to protected paths like` .git` and` .claude` are never auto-approved.


### Connecting your tools with MCP


The Model Context Protocol is an open standard for wiring AI tools to external systems. Connect a server when you catch yourself pasting data into the terminal from a ticket tracker or a dashboard.


Adding one takes a single command:` claude mcp add --transport http notion https://mcp.notion.com/mcp` . Servers install at three scopes: *local* (default, private to you), *project* (written to` .mcp.json` , shared through git), and *user* (all your projects). Run` /mcp` to check status and handle OAuth, or start with our roundup of[MCP tutorials and courses](https://scrimba.com/articles/best-mcp-tutorials-and-courses/) .


## A Worked Example: Fixing a Real Bug


Here is a scenario most web developers have lived through. A signup form on a React app accepts empty submissions, and a junk record lands in the database every time.


The instinct is to type "fix the signup bug." Resist it. Compare it with what works:


```text
Users can submit the signup form with empty fields and it creates a
record. The form lives in src/components/SignupForm.tsx and the handler
is in src/api/signup.ts. Write a failing test that reproduces it first,
then fix it. Address the root cause, do not just disable the button.


```


Four things changed. The **symptom** is described, the **likely location** is named, the definition of *fixed* is stated, and a verification step is built into the prompt.


A realistic sequence from there:


1. Start in plan mode with` Shift+Tab` and ask Claude to read both files and explain the current validation path.
2. Read the plan it produces. This is the cheapest place to catch a wrong assumption.
3. Leave plan mode and send the prompt above. Claude writes the failing test, runs it, edits the validation, and reruns.
4. Ask for the test output rather than accepting "tests pass" as a claim.
5. Finish with "commit with a descriptive message and open a PR."


If step three goes sideways, press` Esc` to stop it mid-action, or` Esc` twice to rewind. Checkpoints cover only changes made through Claude's file-editing tools, so they are a convenience, not a replacement for git.


## Patterns That Work, and Mistakes That Cost You


Four habits separate good sessions from frustrating ones:


- **Point at existing code.** "Look at how` HotDogWidget` is implemented and follow that pattern" beats abstract description.
- **Use` @` to reference files** instead of describing where code lives.
- **Delegate research to subagents.** They explore in their own context window and report back a summary, so a big investigation never floods yours.
- **Ask for evidence.** Test output and command results, not assertions.


The failure patterns Anthropic names come down to context hygiene:


1. **The kitchen sink session.** Unrelated tasks in one conversation. Fix:` /clear` between them.
2. **Correcting over and over.** After two failed corrections,` /clear` and rewrite the prompt.
3. **The over-specified CLAUDE.md.** Too long, so the important rules get lost.
4. **The trust-then-verify gap.** A plausible implementation that misses edge cases.
5. **The infinite exploration.** An unscoped "investigate this" that reads hundreds of files.


The fourth is not hypothetical. In the[2025 Stack Overflow Developer Survey](https://survey.stackoverflow.co/2025/ai/?ref=scrimba.com) , the biggest frustration developers reported with AI tools was **"AI solutions that are almost right, but not quite,"** cited by 66% of respondents, with 45.2% adding that debugging AI-generated code costs them more time. Almost-right output is the default failure mode of every coding agent, and verification is the only defense.


## When Claude Code Is the Wrong Tool


Claude Code is one of three tools most developers choose between in 2026, and they are not interchangeable.


Dimension Claude Code OpenAI Codex Cursor


**Primary interface** Terminal CLI, plus IDE, desktop, and web Terminal CLI, IDE, cloud agent, GitHub AI-native code editor


**Models** Anthropic's Claude models OpenAI models Composer plus Claude, GPT, and Gemini


**Project instructions**` CLAUDE.md`` AGENTS.md` Rules files


**Entry price** Included with Claude Pro, $17/mo annual Included with paid ChatGPT plans Free tier, paid plans from $20/mo


**Best for** Terminal-first multi-file work with tight permission control Handing a task to a cloud agent that returns a pull request Staying inside a full editor


For the full head-to-head, see[Claude Code vs Codex vs Cursor](https://scrimba.com/articles/claude-code-vs-codex-vs-cursor/) . Our[guide to using OpenAI Codex](https://scrimba.com/articles/how-to-use-openai-codex/) is the direct counterpart to this one, and its cloud agent suits a different shape of work: fire and forget, review the pull request later.


Reach for something else when the task is a one-line change you could make faster yourself, when you cannot review the output competently, or when the work touches production systems no checkpoint can roll back. Agents are also a poor substitute for learning. Watching one write a React component teaches less than writing three.


## What You Need to Know to Direct an Agent Well


Every skill in this guide sits on top of one it cannot replace: reading a diff and knowing whether it is correct. An agent will hand you a confident fix for a bug it misdiagnosed, and the only thing between that and production is you.


If you are still building those foundations, Scrimba's[Learn to Code with AI](https://scrimba.com/learn-to-code-with-ai-c02m?ref=scrimba.com) is a free 4.5-hour course with Guil Hernandez. It is *not* a Claude Code course and does not cover agentic tools. It teaches HTML, CSS, and JavaScript from zero while using AI assistance, which is the layer you need before an agent's output means anything.


For developers who already write JavaScript and want to build AI-powered applications rather than just use them, Scrimba's[AI Engineer Path](https://scrimba.com/the-ai-engineer-path-c02v?ref=scrimba.com) covers embeddings, vector databases, agents, context engineering, and MCP across 11.4 hours on the Pro plan at $24.50 per month billed annually. Location-based, student, and promotional discounts are available, and free courses include certificates.


## Frequently Asked Questions


### Is Claude Code free?


No. Claude Code requires a paid Claude plan (Pro, Max, Team, or Enterprise), a Claude Console account with prepaid credits, or access through a supported cloud provider. Pro starts at $17 per month billed annually. The free Claude plan does not include Claude Code.


### Do I need to know how to code to use Claude Code?


You can run it without coding knowledge, but you should not ship its output without it. Claude Code produces plausible code that is sometimes subtly wrong. Reviewing a diff and judging whether a fix addresses the root cause both require real programming ability.


### What is the difference between Claude Code and the Claude app?


The Claude app is a chat interface that returns text. Claude Code is an agent with tools: it reads your files, edits them, runs shell commands, uses git, and verifies its own work inside your project directory. Both run on the same Claude models.


### How do I stop Claude Code from making a change I do not want?


Press Esc to stop it mid-action while preserving context. Press Esc twice, or run the rewind command, to restore conversation and code to an earlier checkpoint. You can also stay in Manual or plan mode so nothing is edited without your approval.


### Where should I put project instructions for Claude Code?


Put team-shared instructions in CLAUDE.md at the project root and commit them. Put personal preferences that apply everywhere in the CLAUDE.md inside your home .claude directory, and project-specific notes in CLAUDE.local.md, which should be gitignored.


## Key Takeaways


- Claude Code is an agentic coding tool that reads, edits, runs, and verifies inside your project, not a chat window that returns snippets.
- Install it with the native installer, Homebrew, or WinGet, then run` claude` in a project directory.
- The four-phase loop is explore, plan, implement, commit. Skip planning when you could describe the diff in one sentence.
- CLAUDE.md loads every session, so keep it under 200 lines or Claude starts ignoring it.
- ` Shift+Tab` cycles permission modes. Use` bypassPermissions` only inside isolated containers or VMs.
- Give the agent a check it can run. Without verification, you become the test suite.
- 66% of developers name "almost right, but not quite" as their top AI frustration, exactly the error class verification catches.


## Sources


Anthropic, Claude Code documentation, accessed July 2026:


- Overview:[https://code.claude.com/docs/en/overview](https://code.claude.com/docs/en/overview?ref=scrimba.com)
- Quickstart:[https://code.claude.com/docs/en/quickstart](https://code.claude.com/docs/en/quickstart?ref=scrimba.com)
- How Claude Code works:[https://code.claude.com/docs/en/how-claude-code-works](https://code.claude.com/docs/en/how-claude-code-works?ref=scrimba.com)
- Memory and CLAUDE.md:[https://code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory?ref=scrimba.com)
- Permission modes:[https://code.claude.com/docs/en/permission-modes](https://code.claude.com/docs/en/permission-modes?ref=scrimba.com)
- Best practices:[https://code.claude.com/docs/en/best-practices](https://code.claude.com/docs/en/best-practices?ref=scrimba.com)
- MCP:[https://code.claude.com/docs/en/mcp](https://code.claude.com/docs/en/mcp?ref=scrimba.com)
- Anthropic. "Pricing." Accessed July 2026.[https://claude.com/pricing](https://claude.com/pricing?ref=scrimba.com)
- Stack Overflow. "2025 Developer Survey: AI." 2025.[https://survey.stackoverflow.co/2025/ai/](https://survey.stackoverflow.co/2025/ai/?ref=scrimba.com)
- Cursor. "Pricing." Accessed July 2026.[https://cursor.com/pricing](https://cursor.com/pricing?ref=scrimba.com)
- OpenAI. "Codex." Accessed July 2026.[https://developers.openai.com/codex/](https://developers.openai.com/codex/?ref=scrimba.com)
