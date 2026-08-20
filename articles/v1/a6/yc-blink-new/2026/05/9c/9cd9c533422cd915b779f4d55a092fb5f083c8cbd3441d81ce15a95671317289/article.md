---
schema_version: "1.0.0"
document_id: "9cd9c533422cd915b779f4d55a092fb5f083c8cbd3441d81ce15a95671317289"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/agents-md-vs-claude-md"
published_at: "2026-05-08T12:42:31+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:12.047673+00:00"
content_hash: "sha256:c36c7319164a2650588482e925d04ff0baa270880dff1165f2a94ef2df9629c4"
---

# AGENTS.md vs CLAUDE.md: The Definitive Guide (2026)

## Key Differences


CLAUDE.md AGENTS.md


**Read by** Claude Code (Anthropic) OpenAI Codex


**Discovery** Walks UP the directory tree Walks DOWN from repo root


**Global location**` ~/.claude/CLAUDE.md`` ~/.codex/AGENTS.md`


**Override file**` CLAUDE.local.md`` AGENTS.override.md`


**Max size** ~200 lines (soft limit) 32 KiB (configurable)


**Nested dirs** Load on demand when files accessed Active during discovery walk-down


**Cross-tool** Reads AGENTS.md via` @import` Does not read CLAUDE.md


**License** Anthropic OpenAI


The discovery direction is the most consequential difference. Claude walks **up** to find parent context. Codex walks **down** to build a layered instruction chain from root to current dir.


## When to Use Each


**Use only` CLAUDE.md` when:**


- Your team exclusively uses Claude Code
- You don't use OpenAI Codex on this project


**Use only` AGENTS.md` when:**


- Your team exclusively uses OpenAI Codex
- You want a simpler single-file setup


**Use both when:**


- Your team uses both Claude Code and Codex on the same project
- You have CI/CD pipelines using Codex and local development using Claude Code
- You want to maintain one canonical instruction file and let each tool read it


Directory structure for AGENTS.md and CLAUDE.md project setup


Blink


## How to Maintain Both Without Duplication


The best approach: **write one canonical file, import it into the other.**


Claude Code supports` @import` syntax in` CLAUDE.md` . You can import your` AGENTS.md` so Claude reads the same rules without duplicating:


```text
# CLAUDE.md
@AGENTS.md


## Claude-Specific Rules
Use plan mode for changes under   `src/billing/`  .
```


Claude loads the imported file at session start, then appends anything after it. This way:


- ` AGENTS.md` is your primary instruction file (maintained by the team)
- ` CLAUDE.md` imports` AGENTS.md` and adds Claude-specific additions
- One source of truth, no drift between files


The Anthropic docs confirm this pattern. You can also use a symlink if you don't need Claude-specific content:


```text
# Creates a symlink: CLAUDE.md → AGENTS.md
ln   -s   AGENTS.md   CLAUDE.md
```


On Windows, use the` @AGENTS.md` import instead (symlinks require Admin/Developer Mode).


## Real-World Project Structure


Here's what a well-organized multi-agent project looks like:


```text
my-project/
├── AGENTS.md              # Primary: team working agreements
├── CLAUDE.md              # Imports AGENTS.md + Claude-specific rules
├── CLAUDE.local.md        # Your personal preferences (gitignored)
├── .codex/
│   └── config.toml        # Codex config (fallback filenames, size limit)
├── services/
│   └── payments/
│       └── AGENTS.md      # Overrides for the payments service


```


The nested` services/payments/AGENTS.md` file lets the payments team set different rules (different test commands, different API key handling) without affecting the root instructions.


## Frequently Asked Questions


No.` CLAUDE.md` is read only by Claude Code (Anthropic's CLI). Cursor uses` .cursorrules` for project context. GitHub Copilot uses` .github/copilot-instructions.md` . Each tool has its own convention.


Some tools are adding AGENTS.md support, but OpenAI Codex is the primary consumer. The file has no official cross-tool standard. Always check each tool's docs before relying on AGENTS.md being read automatically.


` CLAUDE.md` at the project root is committed to git and visible to your team. Use` CLAUDE.local.md` (gitignored by default) for personal or environment-specific instructions. Never put API keys, credentials, or secrets in any of these files.


Long files consume context tokens and reduce adherence. Claude may ignore instructions buried deep in a large file. Split instructions into topic files under` .claude/rules/` , and use path-scoped rules (YAML frontmatter with a` paths` field) to load rules only when relevant files are accessed.


In projects with mixed agent usage, make` AGENTS.md` the primary and have` CLAUDE.md` import it. This is the pattern Anthropic's own documentation recommends, and it avoids maintaining two separate instruction sets.


Yes. Running` /init` in Claude Code analyzes your codebase and generates a starter` CLAUDE.md` with build commands, test instructions, and conventions it discovers. If an` AGENTS.md` exists, it reads it and incorporates relevant parts automatically.


## Build Context-Aware Apps With Your AI Agent


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up both CLAUDE.md and AGENTS.md for this project and deploy it on Blink Cloud."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


---


**Sources:**


- [Anthropic Claude Code CLAUDE.md docs](https://docs.anthropic.com/en/docs/claude-code/claude-md)
- [OpenAI Codex AGENTS.md guide](https://developers.openai.com/codex/guides/agents-md)
- [Addy Osmani: Lesson 16 — AGENTS.md](https://addyosmani.com/agents/15-agents-md/)
