---
schema_version: "1.0.0"
document_id: "d885df5a17248c7a22a9df05e40b85e5f5525943a087c58186e3396aafdf6453"
company_key: "yc-alloy"
company: "Alloy"
source_id: "yc-alloy-news-import-baf7e6c723e9"
canonical_url: "https://alloy.app/library/migrate-website-to-claude-code"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-08-04T13:43:06.031930+00:00"
fetched_at: "2026-08-05T03:48:37.411401+00:00"
content_hash: "sha256:25d5ed482ddb9411e702984a1448e5f81eb7298108d5a4ab744bd60a4c386b8d"
---

# How to Migrate an Existing Website to Claude Code: Tools and Workflow (2026)

To migrate an existing website to Claude Code, you prepare the project so an AI agent can work in it safely: get the code into Git, write a` CLAUDE.md` that explains how the project runs, make the dev server and tests one-command operations, and start with small, reviewable changes. If you cannot access the code — or you want to validate changes visually before touching the repository — capture the live site from the browser and prototype against the capture first.


This guide covers both paths: the direct repository migration, and the capture-first workflow teams use when the goal is to iterate on an existing product without starting from a blank terminal.


**Key takeaways:**


- Claude Code works on existing codebases in any stack — migration is about preparing the repo, not rewriting the site.
- A good` CLAUDE.md` , runnable commands, and passing tests matter more than any prompt technique.
- Start with small scoped changes to calibrate how the agent behaves in your codebase before attempting refactors.
- When code access is missing or premature, a browser capture of the live site gives an agent real UI context to work from.
- The end state of a prototype iteration should be a pull request your team reviews like any other change.


## Why Teams Are Migrating Existing Websites to Claude Code


[Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview) is Anthropic's agentic coding tool. It reads your codebase, plans changes, edits files, runs commands and tests, and opens pull requests. Unlike prompt-to-app generators, it is built for existing software: the whole workflow assumes there is already a repository with history, conventions, and constraints.


That is exactly why "how do I migrate my existing site to Claude Code" has become such a common question. Most teams do not want a new app. They want the product they already ship — the marketing site, the dashboard, the ten-year-old admin panel — to become something an agent can safely change. The payoff is compounding: once the project is agent-ready, every future change gets cheaper, whether it comes from an engineer in a terminal or a PM[prototyping a feature in the browser](https://alloy.app/library/how-to-prototype-changes-existing-web-app) .


"Migration" here means one of two things, and it is worth being precise about which one you need:


1. **Workflow migration:** your code stays where it is; you prepare the repository so Claude Code can work in it. This is the common case, and it covers legacy code too — migrating a legacy web system to Claude Code is the same preparation with more emphasis on tests and guardrails.
2. **Platform migration:** the site lives somewhere without usable code access — Webflow, a legacy CMS, an agency-built site you never got the source for — and you want to rebuild it as a codebase that Claude Code can maintain.


The prerequisites and workflow below cover the first case; the capture-and-rebuild section covers the second.


## Prerequisites: What Claude Code Needs From Your Repository


Claude Code is only as effective as the environment you give it. Before running your first session, check four things:


- **Git, with a sane branch model.** The agent should work on branches, never directly on main. If your site is deployed from a folder on a server, getting it into Git is step zero.
- **One-command install and run.**` npm install && npm run dev` (or your stack's equivalent) should bring the site up locally. If setup requires tribal knowledge, the agent will trip on the same steps a new hire would.
- **Some form of automated verification.** Tests, a type checker, a linter — anything that lets the agent confirm its own changes. A site with zero checks forces you to manually review everything it touches.
- **A` CLAUDE.md` file.** This is the project-level instruction file Claude Code reads at the start of every session. It is the single highest-leverage artifact in the migration.


A useful` CLAUDE.md` for a migrated website covers:


```text
# CLAUDE.md


## Commands
- Install: npm install
- Dev server: npm run dev (localhost:3000)
- Tests: npm test
- Lint: npm run lint


## Structure
- /pages — route components
- /components — shared UI, uses our design tokens in /styles/tokens.css
- /content — markdown, do not edit generated HTML in /public


## Conventions
- CSS Modules, no inline styles
- Never edit files in /generated
- All changes go through a PR against main


```


You can generate a first draft by running` /init` inside Claude Code, then correcting it. Treat it like onboarding documentation: every time the agent does something wrong that a sentence would have prevented, add the sentence. This is context engineering in its most practical form — most agent failures on migrated codebases are context failures, not model failures, and the repository itself is where the context lives.


## The Migration Workflow, Step by Step


### Step 1: Audit What You Are Migrating


List what the site actually consists of: framework and version, styling approach, build pipeline, hosting, third-party scripts, and any parts with no test coverage. The goal is not to fix everything up front — it is to know where the agent can work freely and where it needs guardrails.


### Step 2: Make the Project Runnable and Checkable


Fix the install-and-run path first. Then add the cheapest verification you can: a type check, a lint pass, a handful of smoke tests on critical pages. Agents thrive on fast feedback loops; a project where` npm test` means something is dramatically easier to delegate work in.


### Step 3: Write the CLAUDE.md and Set Permissions


Document commands, structure, and conventions as above. Claude Code also supports permission settings that control which commands the agent may run without asking — start conservative (read and edit freely, ask before running anything destructive) and loosen as trust builds.


### Step 4: Start With Small, Scoped Changes


Your first sessions should be changes you can fully review in minutes: copy updates, a component tweak, a dependency bump. This calibrates two things at once — how well the agent understands your codebase, and how well your` CLAUDE.md` steers it. Widen the scope only when small changes are consistently landing clean.


### Step 5: Scale Up to Features and Refactors


With the loop proven, larger work becomes practical: extracting components, migrating a styling system, adding whole features. Keep each unit of work sized to a reviewable pull request. The migration is "done" when agent-authored PRs move through your normal review process without special handling.


## Browser-Based Prototyping Tools That Migrate an Existing Site to Claude Code


There are two situations where the repository is the wrong starting point — and where a browser-based prototyping tool becomes the migration path instead:


- **You don't have the code.** The site lives in Webflow or a legacy CMS, or was built by an agency that never handed over the source.
- **You have the code, but the change isn't validated yet.** A PM or designer wants to explore a feature on the live product, and spinning up the full local workflow for an idea that may not survive review is overkill.


This is where browser capture comes in.[Alloy captures the live page](https://alloy.app/guide/how-to-capture) from an authorized browser session — real markup, real styles, real images, including pages behind a login — and turns it into an editable starting point. In[our August 2026 test of seven prototyping tools](https://alloy.app/library/best-ai-prototyping-tools) , that capture-based approach was the only one that preserved the product's exact fonts and imagery instead of generating approximations.


From the capture,[cloud agents](https://alloy.app/cloud-agents) iterate on the UI the same way Claude Code iterates on files — you describe the change, the agent makes it against the real interface, and the result is a shareable prototype rather than a diff you have to run locally to see.


The two paths then converge:


- **For workflow migrations,** connect the repository via[Alloy's codebase connectivity](https://alloy.app/guide/github-codebase-connectivity) and the validated prototype becomes a pull request into your actual codebase — capture to iterate to PR.
- **For platform migrations,** the capture becomes the faithful reference for the rebuild. This is[Alloy's website cloner](https://alloy.app/website-cloner) workflow: instead of asking an agent to reproduce your site from screenshots and guesswork, it starts from the real DOM and styles, and the output is a codebase you own — which is then, finally, a project Claude Code can maintain.


## Tools for Migrating an Existing Website to Claude Code, Compared


Tool What it does Works from Best for Limitation


**Claude Code** Terminal/IDE agent that edits, runs, and tests code Your repository Implementing and refactoring once code is agent-ready Needs code access and a runnable project


**Alloy** Browser capture + cloud agents + PRs into your repo The live product, or a[connected codebase](https://alloy.app/guide/github-codebase-connectivity) Validating changes on the real UI before or alongside code work Complex data and integrations still need the codebase path


**Cursor / Windsurf** AI-native IDEs with repo-wide context Your repository Engineers who want agent assistance inside an editor Same code-access prerequisite as Claude Code; local-first


**HTTrack / wget** Static mirroring of a public site Any public URL Archiving or studying a site's front-end Output is a frozen snapshot, not a maintainable codebase — see[website cloner approaches compared](https://alloy.app/website-cloner)


**v0 / Lovable / Bolt** Prompt-to-app generation, optionally from screenshots A prompt or image Greenfield rebuilds where fidelity to the original matters less Reconstructions approximate your fonts, assets, and design system


The honest summary: Claude Code and the AI IDEs are where implementation happens; capture-based tools like Alloy are how you bring a live product into that workflow when the code is inaccessible, or when you want to see and share a change before committing engineering time. Static cloners preserve pixels but produce nothing an agent can meaningfully maintain, and prompt-to-app generators rebuild an approximation rather than migrating the product you have. For a deeper comparison of which platforms genuinely work with existing code, see our guide to[AI prototyping platforms that integrate with your existing codebase](https://alloy.app/library/ai-prototyping-existing-codebase) .


## Common Mistakes When Migrating to Claude Code


- **Starting with a refactor.** The first sessions should test the loop, not the limits. Small changes reveal missing documentation cheaply.
- **Skipping CLAUDE.md.** Every convention the agent has to rediscover per session is time and tokens spent repeatedly.
- **No verification commands.** An agent that cannot run tests cannot check its own work, which shifts the entire burden to your review.
- **Rebuilding when you could capture.** If the goal is to iterate on an existing product's UI, reconstructing it from prompts introduces differences you never intended to test. Start from the real thing.
- **Letting prototypes skip review.** Whether a change comes from a terminal session or a captured prototype, it should land as a pull request that passes normal review and CI.


## FAQs


### Can I use Claude Code on a website without access to its codebase?


Not directly — Claude Code operates on files in a repository. But you can capture the live site from the browser with a tool like Alloy, iterate on the captured UI with cloud agents, and involve the codebase only when a change is worth implementing. For sites on closed platforms like Webflow or a legacy CMS, a capture or clone becomes the starting point for a rebuild.


### Do I need to rewrite my website to use Claude Code?


No. Claude Code works with existing codebases in any language or framework. Migration means preparing the repository — documentation, runnable commands, tests — so an agent can work in it safely, not rewriting the site. A rewrite is only on the table when the current platform gives you no code access at all.


### What should go in a CLAUDE.md file when migrating an existing site?


The commands to install, run, lint, and test the project; the directory structure and where key features live; conventions the codebase follows (styling approach, state management, naming); and anything an agent should never do, such as editing generated files or committing directly to main.


### How is Alloy different from using Claude Code directly?


Alloy runs coding agents in cloud sandboxes against either a browser capture of your live product or your connected codebase. Instead of a terminal session on your machine, you get a visual, shareable prototype to iterate on, and a pull request when the change is validated. Teams often use both: Alloy to explore and validate, Claude Code locally to implement and review.


## Migrate the Workflow, Not Just the Code


The most successful Claude Code migrations treat the agent like a new team member: give it documentation, a runnable environment, fast feedback, and progressively larger responsibilities. And when the starting point is a live product rather than a friendly repository, capture the product first —[Alloy](https://alloy.app/) turns the site your users already see into something agents can iterate on today and your codebase can absorb tomorrow, one reviewed pull request at a time.
