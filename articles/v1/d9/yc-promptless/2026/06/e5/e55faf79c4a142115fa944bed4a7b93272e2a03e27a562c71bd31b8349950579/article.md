---
schema_version: "1.0.0"
document_id: "e55faf79c4a142115fa944bed4a7b93272e2a03e27a562c71bd31b8349950579"
company_key: "yc-promptless"
company: "Promptless"
source_id: "yc-promptless-news-import-48c94b307195"
canonical_url: "https://promptless.ai/blog/product-updates/doc-agents-existing-skills/"
published_at: "2026-06-26T00:00:00+00:00"
first_seen_at: "2026-08-07T08:03:50.270603+00:00"
fetched_at: "2026-08-07T08:03:52.181710+00:00"
content_hash: "sha256:782730a89ce73654344110c4df258b909fbf8b1d52f0f6553289e6cbce35e113"
---

# Doc Agents Now Run Your Existing Agent Skills

# Doc Agents Now Run Your Existing Agent Skills


[← Back to Blog](https://promptless.ai/blog)


Your documentation repository may already have Agent Skills for Claude Code, Cursor, or another AI coding tool. Promptless now discovers and runs them automatically.


## The problem


Section titled “The problem”


Documentation workflows vary across teams. One team has a naming convention for API reference pages that must stay consistent. Another team runs a Vale linting pass, then a custom completeness check, before any suggestion ships. A third team follows a two-step process. The team first expands the existing content, then rewrites the introduction to match a specific voice.


Promptless generates documentation suggestions by running an agent against each trigger. That agent read the diff, pulled context, and wrote the suggestion the same way every time. This approach produced good output for the common case, but it could not encode a team’s specific workflow. Teams sometimes developed a better documentation process through iteration, tooling, or institutional knowledge. Promptless could not use that process.


The same teams that built these workflows were often the same teams building skill libraries for Claude Code and Cursor. They had the workflow encoded, but Promptless could not see it.


## What changed


Section titled “What changed”


Before running each task, Promptless now scans your documentation repository for Agent Skills. Skills under` .claude/skills/` ,` .agents/skills/` , or` .cursor/skills/` are picked up automatically. When the description of a task matches a skill’s description, the agent runs that skill instead of deriving the approach from scratch.


The scan order is` .claude` , then` .agents` , then` .cursor` . If the same skill name exists in more than one directory, the first one wins. Skill filenames are case-sensitive.


You can guide when and how Promptless uses specific skills. You can prefer a certain skill for API reference tasks, or exclude a skill that only applies to interactive sessions. Add these instructions to a` PROMPTLESS.md` file in your Agent Knowledge Base.


*We regularly share actionable insights grounded in research, experiments, and real-world product learnings. Subscribe to get future posts in your inbox.*


## Who benefits most


Section titled “Who benefits most”


Teams that have already built agent skills for their documentation workflows benefit most. If your docs team uses Claude Code, you may have skills for checking documentation style, enforcing a glossary, validating API reference structure, or following a review checklist. Promptless can now run those same workflows when it generates suggestions. The skills you built for interactive use now also run on every automated documentation trigger.


This closes a gap for AI-forward teams. These teams built skill libraries, but the documentation agent could not share that context. The same skill library now powers both interactive and automated use.


Teams without existing skills can still benefit. Any skill you create now works in both contexts automatically, so building it costs less.


## How to use it


Section titled “How to use it”


You need no setup if you already have skills in your documentation repository. Promptless picks them up on the next trigger.


To create your first skill, add a Markdown file to` .claude/skills/` (or` .agents/skills/` or` .cursor/skills/` ) in your docs repository. The file should describe what the skill does and the steps it follows. When a documentation task matches the skill’s description, the agent runs it.


To add guidance about when Promptless should use which skills, create a` PROMPTLESS.md` in your Agent Knowledge Base. See[How Promptless Learns Your Docs](https://promptless.ai/docs/configuring-promptless/doc-collections/how-promptless-learns-your-docs#use-your-existing-skills) for reference.


## More from the blog


- [Fix skill slop before it makes your AI workforce worse](https://promptless.ai/blog/product-updates/introducing-promptless-for-agent-instructions) Product Updates


- [Grant Promptless Access to Files in Private and Shared Teams Channels](https://promptless.ai/blog/product-updates/teams-private-channel-file-access) Product Updates


- [Promptless Now Alerts You When an Integration Has a Problem](https://promptless.ai/blog/product-updates/integration-health-alerts) Product Updates


[← Back to Blog](https://promptless.ai/blog)
