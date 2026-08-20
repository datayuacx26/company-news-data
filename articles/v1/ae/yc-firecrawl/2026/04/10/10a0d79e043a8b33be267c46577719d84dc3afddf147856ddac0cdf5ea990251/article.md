---
schema_version: "1.0.0"
document_id: "10a0d79e043a8b33be267c46577719d84dc3afddf147856ddac0cdf5ea990251"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/best-cli-tools"
published_at: "2026-04-06T00:00:00+00:00"
first_seen_at: "2026-07-21T20:06:32.273956+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:aea0bd17ac10286d60d1acaccdaa7bc7650321008ee7f780b266861dd55695b0"
---

# Best CLI Tools for Your AI Agents in 2026

## TL;DR: Best CLI Tools for AI Agents


CLI What it does


Firecrawl CLI Web scraping, search, crawling, and browser automation


GitHub CLI Full GitHub workflow from issues to releases


Supabase CLI Run the full Supabase stack locally for development


Stripe CLI Test webhooks and manage Stripe resources locally


Google Workspace CLI One tool for Drive, Gmail, Calendar, Docs, Chat, and more


Vercel CLI Deploy, test locally, and manage Vercel projects


---


Most agents I've worked with can reason well but struggle to act in the real world. They write code they can't deploy, reference APIs they can't authenticate, and research topics they can't actually access. The fix isn't always an[MCP server](https://www.firecrawl.dev/blog/api-for-ai-agents) or a custom integration. Often, the right move is to hand the agent a CLI it can call like a shell command.


CLIs built by infrastructure teams are battle-tested, stable, and designed for automation. They authenticate once, produce clean structured output, and expose the full surface of a service through composable subcommands. That makes them ideal for agents running in agentic loops, where reliability matters more than polish.


These are the best CLI tools for AI agents I'd recommend to anyone building agentic workflows today. Six tools covering web data, version control, database management, payments, productivity, and deployments.


## What are agent tools?


[Agent tools](https://www.firecrawl.dev/blog/agent-tools) are defined capabilities that allow a language model to execute operations and retrieve information from external systems. A model without tools is like an analyst locked in a room with no internet, no phone, and no way to verify anything: it can reason about what it knows, but it can't check live facts, run code, or take any action beyond generating text.


CLIs are one of the most practical forms of agent tools because they wrap a full service API behind a stable terminal interface, handling auth and response serialization so the agent just calls a command and reads structured output.


## What are CLI tools and why do they matter for agents?


A CLI tool is a command-line interface built by a SaaS company or infrastructure team that exposes their core product functionality through terminal commands. Think` gh pr create` ,` stripe listen` , or` vercel deploy` . These are tools originally built for human developers, but they work just as well for agents.


What makes them different from raw API calls is that the CLI handles auth, serializes responses, and exposes a stable command surface that doesn't change between API versions. An agent doesn't need to know how OAuth works to run` gh repo clone` . It just calls the command and parses the output. That's a big part of why[AI coding agents](https://www.firecrawl.dev/blog/why-clis-are-better-for-agents) have gravitated toward CLIs over IDE integrations for autonomous workflows.


There are two kinds of CLI tools worth giving your agents access to:


- **Service CLI tools** : Tools like GitHub CLI, Stripe CLI, and Supabase CLI that let your agent interact with a specific service directly from the terminal.
- **Data access CLI tools** : Tools like Firecrawl CLI that give your agent access to live web data, which it otherwise has no way to retrieve.


Both types are worth installing. The combination of service CLI tools and data access CLI tools covers most of what agents need to operate independently.


## Best CLI tools your agents should try in 2026


## 1. Firecrawl CLI


**Firecrawl CLI gives AI agents reliable access to the live web: scraping, search, site mapping, browser automation, and autonomous research, all from a single terminal tool.**


Most agents run with a knowledge cutoff and no live internet access. Firecrawl CLI solves both. It covers the web search and extraction category of agent tools that every agent stack needs: scrape any URL to clean markdown, search the web and scrape the results, map an entire site's URL structure, interact with pages by clicking and filling forms, launch cloud browser sessions for JavaScript-heavy pages, and run autonomous research agents using natural language prompts.


What makes it different from other scraping tools is the skill installer. Running` npx -y firecrawl-cli@latest init --all --browser` registers Firecrawl as a skill across every AI coding agent detected on your machine, so Claude Code, Codex CLI, and others pick it up automatically. The agent doesn't need to be told how to use it. For Codex specifically, Firecrawl can also be added[via MCP](https://docs.firecrawl.dev/quickstarts/codex-cli) instead — covering the gap where[Codex agents can't browse the web](https://www.firecrawl.dev/glossary/web-search-apis/codex-cli-agent-browse-web) and the[built-in web search](https://www.firecrawl.dev/glossary/web-search-apis/codex-web-search) returns snippets only.


For a complete walkthrough of switching Codex from its stale cached default to live results and wiring in Firecrawl MCP, see[Codex web search with Firecrawl](https://www.firecrawl.dev/blog/codex-web-search-using-firecrawl) . For a curated list of the[best Codex skills](https://www.firecrawl.dev/blog/best-codex-skills) worth adding to your Codex CLI workflow, see the best Codex skills guide.


- [scrape](https://docs.firecrawl.dev/features/scrape) : Extracts clean markdown, HTML, links, screenshots, or structured JSON from any URL, with JS rendering support via` --wait-for`
- [search](https://docs.firecrawl.dev/features/search) : Searches the web and optionally scrapes each result, with time filters, category filters, and geo-targeting
- [map](https://docs.firecrawl.dev/features/map) : Discovers all URLs on a site quickly, with sitemap and subdomain support
- [interact](https://docs.firecrawl.dev/features/interact) : Scrapes a page then lets the agent take over to click, type, scroll, and extract data without a full browser session
- [browser](https://docs.firecrawl.dev/features/interact) : Launches a cloud Chromium session with Playwright support, no local browser install required. Useful for[AI browser automation](https://www.firecrawl.dev/blog/best-browser-agents) workflows that previously needed a full headless setup
- [crawl](https://docs.firecrawl.dev/features/crawl) : Crawls an entire site with depth and path controls, with progress tracking
- [agent](https://docs.firecrawl.dev/features/agent) : Runs an autonomous research agent that searches and gathers structured data using natural language


**Install:**


```text
# Install as an AI agent skill (recommended for Claude Code and other agents)
npx   -y   firecrawl-cli@latest   init   --all   --browser


# Or install globally
npm   install   -g   firecrawl-cli
```


**Example:**


```text
# Scrape a page to clean markdown
firecrawl   https://docs.example.com   --only-main-content


# Search the web and scrape results
firecrawl   search   "supabase edge functions guide 2026"   --scrape   --scrape-formats   markdown


# Run an autonomous agent for research
firecrawl   agent   "Find pricing and feature comparison across the top 5 vector databases"   --wait
```


**Honest take:** Firecrawl CLI is the only tool in this list that gives agents access to live web data, which makes it essential for any research or enrichment workflow, including[deep research for AI agents](https://www.firecrawl.dev/blog/deep-research-for-ai-agents) that need to synthesize information across many sources. The skill installer is a genuine time saver. The one thing to be aware of is credits: each scrape and search consumes from your API quota, so agents running in tight loops should use` --max-credits` on agent jobs to cap spend.


**Cons:** Requires a Firecrawl account and API key. Heavy crawl jobs and browser sessions consume credits faster than basic scrapes. Self-hosting is available but adds infrastructure overhead.


Here is a quick look at the Firecrawl CLI in action:


Full reference at[docs.firecrawl.dev/sdks/cli](https://docs.firecrawl.dev/sdks/cli) . Repo:[github.com/firecrawl/cli](https://github.com/firecrawl/cli) .


---


## 2. GitHub CLI


**GitHub CLI brings every GitHub operation to the terminal, so your agent can manage pull requests, issues, releases, and repo settings without touching a browser.**


` gh` is GitHub's official CLI, free and open source. GitHub has always been improving to meet the needs of the developer ecosystem, and the agentic AI world is no different. The CLI gives your agents everything they need to participate fully in a GitHub-based workflow: create and review pull requests, open and close issues, trigger GitHub Actions workflows, search repos, and clone with one command. In 2026, GitHub added` gh copilot` for inline AI assistance without leaving the shell, making it one of the few CLIs that brings AI assistance into the CLI itself.


One of the most useful features for agents is` gh api` , which lets you make authenticated requests to any GitHub API endpoint directly. Combined with aliases, agents can define shorthand for common multi-step operations.` gh alias set bugs 'issue list --label="bugs"'` creates a reusable command the agent can call by name.


GitHub's continued investment in developer tooling for the AI era is worth noting:


- ` gh pr create` : Opens a pull request with a title, body, and reviewer assignment from the terminal
- ` gh pr checks` : Shows CI check status for a pull request, including links to each run
- ` gh issue list --label <label>` : Filters issues by label, assignee, state, or keyword
- ` gh workflow run <name>` : Triggers a GitHub Actions workflow by name from the terminal
- ` gh repo clone <owner/repo>` : Clones a repository in a single command
- ` gh api <endpoint>` : Makes authenticated HTTP requests to any GitHub REST API endpoint
- ` gh copilot -p "<task>"` : Gets inline AI suggestions for shell commands and GitHub operations without leaving the terminal
- ` gh alias set <name> <command>` : Creates reusable command shortcuts for complex queries


**Install:**


```text
# macOS
brew   install   gh


# Windows
winget   install   --id   GitHub.cli


# Linux (see full instructions)
# https://github.com/cli/cli/blob/trunk/docs/install_linux.md
```


**Example:**


```text
# Create a pull request
gh   pr   create   --title   "feat: add retry logic"   --body   "Adds exponential backoff to API calls"


# Check CI status on current branch
gh   pr   checks


# Trigger a GitHub Actions workflow
gh   workflow   run   deploy.yml   --ref   main


# Clone a repo in one command
gh   repo   clone   owner/repo


# List all open bugs
gh   issue   list   --label   "bug"   --state   open


# Get AI suggestions for a shell task inline
gh   copilot   -p   "how do I squash my last 3 commits"


# Make a raw API call
gh   api   /repos/owner/repo/actions/runs   --jq   '.workflow_runs[0].status'
```


**Honest take:**` gh` is the most complete CLI for GitHub operations and the one I trust most for agent use. The` gh api` command in particular is powerful: it handles auth and lets an agent reach any endpoint without building a custom request. The addition of` gh copilot` in 2026 is genuinely useful for agents that need guidance on shell tasks mid-session. The one limitation is that it doesn't cover GitHub Actions authoring. For creating or editing workflow files, the agent still needs to work with YAML directly.


**Cons:**` gh` only covers GitHub, not GitLab or Bitbucket. Some advanced admin operations require organization owner permissions. Enterprise Server support exists but requires separate configuration.


Full reference at[cli.github.com](https://cli.github.com/) . Repo:[github.com/cli/cli](https://github.com/cli/cli) .


---


## 3. Supabase CLI


**Supabase CLI lets your agent spin up and manage a full local Supabase stack, including Postgres, Auth, Storage, Edge Functions, and Studio, with two commands.**


Supabase CLI is built for local development workflows. Two commands get you running:` supabase init` to scaffold a new project, then` supabase start` to launch the entire Supabase stack in Docker. The agent gets back a local API URL, database URL, Studio dashboard, and API keys it can use immediately, no cloud account required for local dev.


For agents handling database migrations or seeding, the` supabase db` subcommands are the core workflow.` supabase db diff` generates a migration file from schema changes.` supabase db dump` exports the current database state. This gives agents a safe, local sandbox to test database changes before pushing to production.


- ` supabase init` : Scaffolds a new local Supabase project configuration
- ` supabase start` : Launches the full Supabase stack locally via Docker, printing connection URLs and API keys
- ` supabase stop` : Stops the local stack without resetting data
- ` supabase db diff -f <name>` : Detects schema changes and generates a migration file
- ` supabase db dump --local --data-only` : Exports local database data for backup or seeding
- ` supabase status` : Shows connection details for the running local stack


**Install:**


```text
# Run without installing (requires Node.js 20+)
npx   supabase   --help


# Or install as a dev dependency
npm   install   supabase   --save-dev
```


**Example:**


```text
# Start a new local project
supabase   init
supabase   start


# Check the running stack's URLs and keys
supabase   status


# Generate a migration from schema changes
supabase   db   diff   -f   add_user_profiles_table


# Export data before stopping
supabase   db   dump   --local   --data-only   >   supabase/seed.sql
supabase   stop
```


**Honest take:** The local stack is genuinely useful for agents working on backend tasks. The fact that it starts a full Supabase environment, including Studio, with one command is impressive. The main friction is Docker: if Docker Desktop isn't running, the agent can't start the stack, and error messages aren't always clear about why. Make sure Docker is configured before pointing an agent at this.


**Cons:** Requires Docker (Desktop or compatible alternative like OrbStack). First run is slow due to image downloads. Global` npm install -g` is not supported, so it always runs via` npx` or as a local dev dependency. Upgrading requires stopping containers and deleting volumes first.


Full reference at[supabase.com/docs/guides/local-development/cli/getting-started](https://supabase.com/docs/guides/local-development/cli/getting-started) .


---


## 4. Stripe CLI


**Stripe CLI lets your agent test webhooks locally, trigger payment events in a sandbox, stream real-time API logs, and create Stripe resources directly from the terminal.**


The Stripe CLI is the standard tool for building and testing Stripe integrations without touching the Dashboard. For agents working on payment features, it removes the two biggest development bottlenecks: setting up local webhook forwarding and triggering specific payment events to test against.


` stripe listen --forward-to` opens a local tunnel and routes Stripe events to any localhost endpoint.` stripe trigger` fires real Stripe events like` checkout.session.completed` or` payment_intent.succeeded` in your sandbox, generating all the associated API objects. This means an agent can test end-to-end payment flows without any manual Dashboard interaction.


- ` stripe listen --forward-to <url>` : Forwards Stripe webhook events to a local endpoint during development
- ` stripe trigger <event>` : Fires a real sandbox event with all associated API objects created
- ` stripe logs tail` : Streams real-time API request logs to the terminal for debugging
- ` stripe products create --name <name>` : Creates Stripe resources directly from the CLI
- ` stripe customers create` : Creates a customer object with inline parameters
- ` stripe login --interactive` : Authenticates non-interactively for CI/CD environments using an existing API key


**Install:**


```text
# macOS
brew   install   stripe/stripe-cli/stripe


# Windows
scoop   install   stripe


# Docker
docker   pull   stripe/stripe-cli
```


**Example:**


```text
# Authenticate
stripe   login


# Forward webhooks to local server
stripe   listen   --forward-to   localhost:4242/webhooks


# Trigger a checkout completed event to test your handler
stripe   trigger   checkout.session.completed


# Tail API request logs for debugging
stripe   logs   tail


# Create a product and price
stripe   products   create   --name=  "Pro Plan"
stripe   prices   create   --unit-amount=2900   --currency=usd   --product=prod_ABC123
```


**Honest take:**` stripe listen` is one of the best developer experiences I've seen in any CLI. The webhook signing secret it prints is ready to use, no Dashboard setup required.` stripe trigger` generates realistic test data, not just empty event shells. The one thing to know is that the CLI operates on sandbox mode by default. Agents need to pass` --live` explicitly for live mode operations, which is the right default but can cause confusion when test data doesn't appear where expected.


**Cons:** Requires a Stripe account. The hCaptcha on Stripe docs pages can block automated scraping if you try to research CLI commands programmatically. Live mode operations need careful handling to avoid unintended charges.


Full reference at[docs.stripe.com/stripe-cli](https://docs.stripe.com/stripe-cli) .


---


## 5. Google Workspace CLI


**Google Workspace CLI is a single command-line tool for Drive, Gmail, Calendar, Sheets, Docs, Chat, Admin, and more, dynamically generated from the Google Discovery API and purpose-built for AI agents.**


The Google Workspace CLI (installed as` gws` ) has 23,900 GitHub stars and is written in Rust. What sets it apart from other Workspace integrations is how it's built: the entire command surface is generated dynamically from Google's Discovery Service, which means it stays current as Google adds new API capabilities without requiring manual updates to the CLI itself.


For agents specifically, the CLI ships with over 100 pre-built agent skills for common Workspace workflows. Structured JSON output is the default for all responses, so agents can parse results without additional processing. An agent can list unread Gmail threads, create a Calendar event, upload a file to Drive, or send a Chat message, all through consistent terminal commands.


- ` gws drive files list` : Lists files in Google Drive with filtering and pagination, using` --params '{"pageSize": N}'` for results
- ` gws gmail users messages list` : Lists Gmail messages matching a query, e.g.` --params '{"userId": "me", "q": "is:unread"}'`
- ` gws calendar events insert` : Creates a new Calendar event with attendees and metadata via` --params` and` --json`
- ` gws sheets spreadsheets values get` : Reads cell ranges from Google Sheets using` --params '{"spreadsheetId": "...", "range": "..."}'`
- ` gws chat spaces messages create` : Sends a message to a Google Chat space using` --params '{"parent": "spaces/..."}'`
- ` gws admin directory_v1 users list` : Lists users in a Google Workspace organization (admin only)


**Install:**


```text
# Via npm (downloads the appropriate binary automatically)
npm   install   -g   @googleworkspace/cli


# macOS / Linux via Homebrew
brew   install   googleworkspace-cli


# Build from source (Rust required)
cargo   install   --git   https://github.com/googleworkspace/cli   --locked
```


**Example:**


```text
# List unread emails from the last 7 days
gws   gmail   users   messages   list   --params   '{"userId": "me", "q": "is:unread newer_than:7d"}'


# Create a calendar event
gws   calendar   events   insert   --params   '{"calendarId": "primary"}'   --json   '{"summary": "Agent sync", "start": {"dateTime": "2026-04-10T10:00:00Z"}, "end": {"dateTime": "2026-04-10T11:00:00Z"}}'


# Read a spreadsheet range
gws   sheets   spreadsheets   values   get   --params   '{"spreadsheetId": "<id>", "range": "Sheet1!A1:D10"}'


# Upload a file to Drive
gws   drive   files   create   --json   '{"name": "report.pdf"}'   --upload   ./report.pdf
```


**Honest take:** This is the most impressive new CLI in the list. The fact that it's generated from Google's Discovery API means the command surface is always accurate and complete. The 100+ included agent skills are a genuine shortcut for common Workspace automation. The setup is the hardest part: Google OAuth configuration requires creating credentials in the Google Cloud Console, which takes time and isn't something an agent can do for itself.


**Cons:** OAuth setup requires manual Google Cloud Console configuration before the CLI can authenticate. Some Admin SDK commands require elevated Workspace admin permissions. The CLI is relatively new (under active development), so edge cases in less-used API surfaces may surface issues.


Repo:[github.com/googleworkspace/cli](https://github.com/googleworkspace/cli) .


---


## 6. Vercel CLI


**Vercel CLI lets your agent deploy projects, replicate the Vercel environment locally, manage environment variables, inspect logs, and control every aspect of a Vercel project from the terminal.**


Vercel CLI covers the full deployment lifecycle.` vercel dev` replicates the production Vercel environment locally, including serverless function behavior and environment variables, so agents can test before deploying.` vercel deploy` ships a preview deployment and returns the URL.` vercel rollback` reverts to a previous deployment in seconds. For agents working on frontend or full-stack projects, this is the deployment layer.


What's useful beyond basic deploys is the depth of the command surface. Agents can pull environment variables with` vercel env pull` , manage Blob storage with` vercel blob` , set up MCP client configuration with` vercel mcp` , inspect deployment logs with` vercel logs --follow` , and even run binary search across deployments with` vercel bisect` to surface regressions.


- ` vercel dev` : Runs the project locally with production-accurate Vercel behavior, including edge functions and env vars
- ` vercel deploy --prod` : Deploys to production and returns the live URL
- ` vercel env pull <file>` : Pulls remote environment variables into a local` .env` file
- ` vercel logs <deployment-url> --follow` : Streams runtime logs from a specific deployment
- ` vercel rollback` : Promotes a previous deployment to production in one command
- ` vercel blob put <file>` : Uploads a file to Vercel Blob storage and returns the URL


**Install:**


```text
# npm (recommended)
npm   i   -g   vercel


# pnpm
pnpm   i   -g   vercel


# Yarn
yarn   global   add   vercel
```


**Example:**


```text
# Link current directory to a Vercel project
vercel   link


# Pull environment variables locally
vercel   env   pull   .env.local


# Start local dev server with Vercel environment
vercel   dev


# Deploy to production
vercel   deploy   --prod


# Roll back if something breaks
vercel   rollback


# Check recent deployment logs
vercel   logs   <  deployment-ur  l  >   --follow
```


**Honest take:** Vercel CLI is one of the most complete deployment tools available. The` vercel dev` local environment is accurate enough that most issues surface before deploy, not after. The` vercel bisect` command is genuinely clever for debugging regressions across deployments. The only friction I've hit is authentication in CI: generating a token and passing it via` --token` works, but token management adds a setup step.


**Cons:** Tied to the Vercel platform, so it's not useful for agents deploying to AWS, GCP, or other providers. Some commands like` vercel buy` and` vercel rolling-release` are Vercel plan-dependent. The CLI assumes a linked project context, so agents need to run` vercel link` first in new directories.


Full reference at[vercel.com/docs/cli](https://vercel.com/docs/cli) .


---


## Building the top CLI tools for AI agents into your workflow


The combination that tends to work best is Firecrawl CLI plus one service CLI matched to your stack. Firecrawl gives any agent access to live web data and research capabilities it otherwise lacks entirely. The service CLI tools (GitHub, Supabase, Stripe, Vercel) give it the ability to act on what it learns. Together, they form the[agent harness](https://www.firecrawl.dev/blog/what-is-an-agent-harness) that connects reasoning to real-world outcomes.


For agents working on full-stack applications, the natural stack is Firecrawl for research, GitHub CLI for version control, Supabase CLI for the local database environment, and Vercel CLI for deployments. These four cover the complete loop from planning to shipping without any custom integration work.


The Google Workspace CLI stands apart as the strongest option for agents handling productivity and business operations: managing email inboxes, updating spreadsheets, scheduling meetings, or broadcasting announcements through Chat. Once OAuth is configured, it's one of the most capable agent tools available for non-engineering workflows.


For agents that need to explore what else is available, the[Firecrawl docs](https://www.firecrawl.dev/blog/claude-code-skill) cover how to combine the Firecrawl skill with Claude Code for end-to-end research and data workflows. The[best MCP servers for developers](https://www.firecrawl.dev/blog/best-mcp-servers-for-developers) post covers the MCP layer that sits alongside these CLI tools. For a full token cost comparison and decision framework, see[MCP vs CLI for AI agents](https://www.firecrawl.dev/blog/mcp-vs-cli) . For the deeper case on why terminal agents outperform IDE-based ones on token efficiency and composability, see[why AI agents prefer the CLI](https://www.firecrawl.dev/blog/why-ai-agents-prefer-cli) . If you haven't settled on a primary coding agent yet, the[best AI coding agents comparison for 2026](https://www.firecrawl.dev/blog/best-ai-coding-agents) covers all eight major tools — Claude Code, Codex, Cursor, Copilot, OpenCode, and more — ranked on harness depth, cost, and benchmark accuracy. The[Claude Code vs OpenCode comparison](https://www.firecrawl.dev/blog/claude-code-vs-opencode) and the[Claude Code vs Codex comparison](https://www.firecrawl.dev/blog/claude-code-vs-codex) cover the key differences in harness depth, sandboxing, and model-agnosticism. And if you are evaluating which CLI tools are most useful as a starting point for AI agents, these six cover the surface area that comes up most often in real agentic workflows.
