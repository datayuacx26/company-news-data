---
schema_version: "1.0.0"
document_id: "7996db318eac18cdc54f302ee2e4c4ae9cf1deb5582873adf2cebcc63e1ce4a6"
company_key: "yc-stably-ai"
company: "Stably AI"
source_id: "yc-stably-ai-news-import-4d9fcc9f146f"
canonical_url: "https://www.stably.ai/blog/install-kung-fu-for-your-ai-agent"
published_at: "2026-01-15T00:00:00+00:00"
first_seen_at: "2026-07-24T02:44:25.851186+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:307b87927b7972698ed72d43b6fa4de1d3098798bc5de6632e081c2c006d924e"
---

# Install Kung-Fu for Your AI Agent (with Vercel's Agent Skills)

Remember the Matrix moment where Neo wakes up and says, "I know kung-fu"? This is the practical version of that for AI coding agents. Vercel's[vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) repo is a set of focused, reusable instruction packs that give your agent a real playbook instead of improvising every time.


## What agent-skills actually contains


Today the repo ships three skills, each with a specific job:


- **react-best-practices** : 40+ rules across 8 categories, prioritized by impact. It is a solid checklist for React and Next.js performance reviews.
- **web-design-guidelines** : 100+ rules across accessibility, UX, and UI performance. Think of it as a UI audit kit.
- **vercel-deploy-claimable** : A deployment skill designed for chat-based flows that returns a preview URL and a claim URL.


These are not vague "tips." Each skill is a packaged workflow that your agent can apply consistently across repos.


## What` npx add-skill` does under the hood


The` add-skill` CLI installs skills into the agent-specific folders your tools already read. It supports OpenCode, Claude Code, Codex, Cursor, and Antigravity. You can install per-project (default) or globally across all projects.


Project-level locations look like this:


- Codex:` .codex/skills/<name>/`
- Claude Code:` .claude/skills/<name>/`
- Cursor:` .cursor/skills/<name>/`


Global installs land in your home directory, for example` ~/.codex/skills/<name>/` .


It also auto-detects which agents you have installed by checking their config directories, and prompts you if none are found.


The CLI also knows how to discover skills inside repos. It looks for` SKILL.md` files in standard locations like` skills/` ,` .codex/skills/` , or` .claude/skills/` , and falls back to a recursive search if nothing is found.


## Skill format and portability


Each skill is a directory with a` SKILL.md` file that includes YAML frontmatter (` name` and` description` ). Optional` scripts/` and` references/` folders can add automation or context. The skills follow the Agent Skills spec, so the same skill can work across different agents, with a few feature differences (for example, Claude Code supports hooks and` context: fork` ).


## The exact flow from Vercel (and why it works)


1. Install the skill:


```text
npx add-skill vercel-labs/agent-skills


```


1. Paste this prompt:


```text
Assess this repo against React best practices. Make a prioritized list of quick wins and top fixes.


```


1. Review and prompt to "make the fixes"


This works because the skill gives the agent a stable rubric, so the output is consistent and ranked, not just a brainstorm.


## Practical options you will actually use


You can target specific agents, list skills before installing, or pick a single skill by name:


```text
# List available skills in the repo
npx add-skill vercel-labs/agent-skills --list


# Install just one skill
npx add-skill vercel-labs/agent-skills --skill react-best-practices


# Install for specific agents
npx add-skill vercel-labs/agent-skills -a codex -a claude-code


# Global install, non-interactive
npx add-skill vercel-labs/agent-skills -g -y


```


If you want to pull from a specific folder in a repo,` add-skill` accepts full Git URLs and subpaths as well.


## Why this is more than a clever prompt


Most teams already have standards; they are just scattered across docs and tribal knowledge. Skills turn those standards into executable instructions. The result is:


- **Prioritized fixes** instead of a flat list
- **Consistent reviews** across repos and contributors
- **Faster follow-through** because the fixes are actionable


It is not magic, but it is repeatable, which is the part that actually saves time.


## Use the Matrix moment responsibly


Install the skill, run the review, and keep the "make the fixes" step as a separate decision. You still want a human to sign off on anything that changes behavior, but the agent can get you 80% of the way there in minutes.


If you want to branch out, try variations like:


- "Audit this repo for accessibility gaps and propose fixes."
- "Find the top performance wins in this Next.js app."
- "Review component structure for readability and reuse."


That is the real benefit: you are not just asking a model to be smart, you are giving it a playbook to follow.
